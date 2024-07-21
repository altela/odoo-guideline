---
layout: default
title: Python Notes
nav_order: 3
description: "Python Notes."
---

# Python Notes

## Compare / Convert The Database Time (UTC0) With Logged in User Timezone
By default, time and date inside Odoo postgresql is saved in UTC0 format. If you are working with times in Odoo, it is always recommended to convert the date and times into user's timezone first.
After you made some manipulation on those times in computer memory, simply save it back into fields.Datetime(). it will also converted back automatically into the UTC0 as well.

The recommended flow to work on the date in a nutshell is :
fetch a date from postgresql (use ORM is also fine) -> convert the date into current user timezone -> make some manipulation -> store it back to fields.Datetime() -> it will saved back into postgresql

This is the block example : 

```python
from odoo import fields

# Initializing the records and user timezones
api_time = record['updatedAt'] # Assuming this is the date and time, fetch from external API or casual text format
api_time_in_datetime_format = fields.Datetime.from_string(api_time) # Convert api_time into Datetime format using odoo fields import
user_timezone = self.env.user.tz # This will get current user timezone, accessible from .env

# convert from UTC0 (server timezone) to user timezone
api_time_in_user_timezone = fields.Datetime.context_timestamp(self.with_context(tz=user_timezone), timestamp=api_time_in_datetime_format)
api_time_converted_as_strftime= api_time_in_user_timezone.strftime("%Y-%m-%d %H:%M:%S")
api_time_converted_as_strptime = datetime.strptime(api_time_converted_as_string, "%Y-%m-%d %H:%M:%S")
```

## Forcefully Write Debit-Credit On `account.move.line`

Using `.with_context()` will help to forcefully write debit-credit of journal entry without cancelling it.

```python
journal_entry_line = self.env['account.move.line'].search([('ref', '=', str(self.name + " - " + " Vendor Exchange Out" + " - " + line.main_product.name))])
for record in journal_entry_line:
    if record.debit > 0:
        record.with_context(check_move_validity=False).write({'debit': line.main_product_cost * line.main_product_qty})
    if record.credit > 0:
        record.with_context(check_move_validity=False).write({'credit': line.main_product_cost * line.main_product_qty})
```

---

## Set Current Date Using fields
```python
define_a_date = fields.Date.today()

# or

define_a_date = fields.Date.context_today(self)
```

---

## Link Register Payment With Invoice
We can use `js_assign_outstanding_line()` to link invoice with payment

```python
def register_payment(self):
    payment_data = {
        'payment_type': 'inbound',
        'partner_id': self.partner_id.id,
        'amount': order_pending['grandTotal'],
        'ref': 'Payment for Invoice %s' % invoice.name,
        'date': datetime.strptime(order_pending['createdAt'], '%Y-%m-%d %H:%M:%S').date(),
        'journal_id': rule.auto_workflow.payment_journal.id
    }

    payment = self.env['account.payment'].create(payment_data)

    # Post the payment
    payment.action_post()

    # Link payment with the invoice
    receivable_line = payment.line_ids.filtered('credit')
    invoice.js_assign_outstanding_line(receivable_line.id)
```

---

## Passing Context From `other_method()` Into `create()` Method
This also can be applied to another method but to simplify stuff we will use `create()` method example instead
first we inherit a `create()` method into the module
```python
@api.model_create_multi
def create(self, vals):
    if vals.get('payroll_number', 'New') == 'New':
        vals['payroll_number'] = self.env['ir.sequence'].next_by_code('payroll.number.sequence') or 'New'
    result = super(CentralPayroll, self).create(vals)

    return result
```

now, in another part of the block, we have `cron_auto_create_monthly_transfer_payroll()`. We need to create context as dictionary to be passed on when `create()` is called
```python
def cron_auto_create_monthly_transfer_payroll(self):
    context = {
        'is_via_cron': True
    }

    payroll = self.env['central.payroll'].with_context(context).create({ # Put the context during creation with .with_context(args)
        'employee_id': employee.id,
        'periode_bulan': str(current_month),
        'periode_tahun': self.env['central.configuration.year'].search([('year','=',current_year)]).id,
        'attendance_start': start_date,
        'attendance_finish': finish_date,
        'account_debit': self.env['account.journal'].search([('name','=','BCA 1910928887 (IDR)')]).id
    })
```

if we go back to the create(), we can fetch the context using self.env.context
```python
@api.model_create_multi
def create(self, vals):
    if vals.get('payroll_number', 'New') == 'New':
        vals['payroll_number'] = self.env['ir.sequence'].next_by_code('payroll.number.sequence') or 'New'
    result = super(CentralPayroll, self).create(vals)

    context = self.env.context
    print("Context:", context) # This shall print out all context
    return result
```

---

## Track Invoice Payment
```python
@api.depends('amount_residual', 'move_type', 'state', 'company_id')
def _compute_payment_state(self):
    res = super(InvoiceInherit, self)._compute_payment_state()

    for rec in self:
        if rec.type_name == 'Invoice':
            print(rec.id)
            print(rec.payment_state)
    return res
```

---

## Auto Create Sequence
Generate sequence when record is created
```python
class RekapOrder(models.Model):
    _name = 'rekap.order'
    _description = 'Rekap Order Pengiriman'
    _rec_name = 'name'

    name = fields.Char(readonly=True, required=True, copy=False, default='New')
    @api.model_create_multi
    def create(self, vals):
        # Auto Assign record name
        if vals.get('name', 'New') == 'New':
            vals['name'] = self.env['ir.sequence'].with_company(self.company_id.id).next_by_code('rekap.order.sequence') or 'New'
        result = super(OrderSetoran, self).create(vals)

        return result
```

---

## Set Domain Based on Company
This will fetch current logged in user by getting from `env`
```python
account_stock_journal = fields.Many2one('account.journal', ondelete='restrict', domain=lambda self: self._get_company_domain())

@api.model
def _get_company_domain(self):
    return [('company_id', '=', self.env.company.id)]
```

---

## Compute Tax (Most Recommended Way)
```python
@api.onchange('ticket_number')
def compute_tax(self):
    taxes = self.line_id.tax_id
    for rec in self:
        ticket_amount = rec.ticket_number.total_amount

        if taxes:
            tax_computation = taxes.compute_all(ticket_amount, currency=self.ticket_number.currency_id, quantity=1)
            tax_total = tax_computation['total_included'] - tax_computation['total_excluded']
        else:
            tax_total = 0

        rec.ticket_tax = tax_total
```

---

## Download Record as PDF And Save it To `ir.attachment`
```python

report_pdf = self.env["ir.actions.report"].sudo()._render_qweb_pdf(self.env.ref('crm_project_task.report_well_information_project'), res_ids=self.id)

filename = 'attachment' + '.pdf'
attachment = self.env['ir.attachment'].create({
    'name': filename,
    'type': 'binary',
    'datas': base64.b64encode(report_pdf[0]),
    'res_model': 'project.task',
    'res_id': self.id,
    'mimetype': 'application/x-pdf'
})

```

---

## Launch Record To a New View
```python
def launch_other_view(self):
    action = {
        'type': 'ir.actions.act_window',
        'name': 'Email Record',
        'res_model': 'email.record',
        'view_mode': 'form',
        'res_id': record.id,
        'target': 'current',
    }
    return action
```

---

## Apply Filter Domain on Many2one
```python
tunjangan_type = fields.Selection([
    ('bonus', 'Bonus/Tunjangan Karyawan'),
    ('potongan', 'Potongan'),
    
], string='Tipe Tunjangan',default='bonus', states={
    'draft': [('readonly', False)],
    'paid': [('readonly', True)],
    'cancel': [('readonly', True)],
})

@api.onchange('tunjangan_type')
def _compute_tunjangan_domain(self):
    for record in self:
        if record.tunjangan_type == 'bonus':
            return {'domain': {'account_tunjangan': ['&', '|', '|', '|', 
                                                        ('name', 'ilike', 'bonus'), 
                                                        ('name', 'ilike', 'pesangon'), 
                                                        ('name', 'ilike', 'lembur'), 
                                                        ('name', 'ilike', 'seragam'), 
                                                        ('user_type_id', '=', 'Expenses')]}}
```

---

## One2many Guideline
Here's the basic One2many guidelines
```python
class MainModels(models.Model):
    _name = 'main.models'

    the_field = fields.One2many("comodels.of.main", "model_id")

class ComodelsOfMain(models.Model):
    _name = 'comodels.of.main'

    model_id = fields.Many2one('main.models')
```

---

## Create Mail Activity (To-Do)
```python
self.env['mail.activity'].create({
    'res_model_id': self.env['ir.model']._get('field.ticket.generator').id,
    'res_id': field_ticket.id,
    'summary': str(self.name) + str(" - Field Ticket Needs To Be Created"),
    'user_id': bookkeeper.id,
    'date_deadline': fields.Datetime.context_timestamp(self.with_context(tz=user_timezone), timestamp=today_in_string),
    'activity_type_id': 4,
})
```

---

## Assign Existing Record into Many2many
```python
previous_well_info = fields.Many2many('well.information', 'previous_well_info', 'uwi', 'uwi_id', string='Legumes', readonly=True)

def fetch_previous_uwi(self):
     previous_uwi = self.env['well.information'].search([('uwi', '=', self.uwi), ('id', '!=', self.id)])
     self.previous_well_info = [(5, 0, 0)]

     if bool(previous_uwi):
         self.previous_well_info = previous_uwi.ids
```

---

## post_init_hook
For version 17.0, it doesnt need cr and registry as parameter anymore but rather using env as code below
```python
# -*- coding: utf-8 -*-
from . import models
from odoo import api, SUPERUSER_ID


def post_init_hook(env):
    # env = api.Environment(env, SUPERUSER_ID, {})
    companies = env['res.company'].search([])

    for company in companies:
        env['default.service'].create({
            'company_id': company.id,
            'name': 'Default Service',
        })
```

---

## Define Single Digits
```python
psn_depth = fields.Float(string="PSN Depth", digits=(6, 1))
```

---

## Create Pop Up Notification
```python
from odoo import _

message = _("Connection Test Successful!")
return {
    'type': 'ir.actions.client',
    'tag': 'display_notification',
    'params': {
        'message': message,
        'type': 'success',
        'sticky': False,
    }
}
```
'type' can be changed to 'success', 'warning', 'danger', 'info'

## Changing Form String Name Title (In Python Ways)
You can Add 'name' keys to pass wizard
```python
    def open_project_task_wizard(self):
        return {
            'name': ("Create Project Task"),
            'type': 'ir.actions.act_window',
            'res_model': 'crm.project.task.wizard',
            'view_mode': 'form',
            'target': 'new',
        }
```

---

## Find Record in Database Based on XML ID
```python
record_id = self.env.ref('module_name.xml_id_of_record').id 
```

---

## Add New Selection Options
You can add new option into Odoo's fields.Selection(selection_add=[]) like :

```python
print_format = fields.Selection(selection_add=[
    ('zpl', 'ZPL Labels'),
    ('zplxprice', 'ZPL Labels with price')]
, ondelete={'zpl': 'set default', 'zplxprice': 'set default'})
```
If the print_format is defined somewhere in other module, you can just _inherit that module and make extension of the selection with the block above. it works.

---

## Create Access Token For A Record
Access token will mostly be used to access certain record, basically to let external user to see a record through certain page (like invoice).
In order to let your record has access token, you should make inheritance to your module like this example :

```python
_inherit = ['portal.mixin', 'mail.thread', 'mail.activity.mixin', 'sequence.mixin']
    
access_token = fields.Char()
```
Everything should have been exactly like example above, so it will allows you to make an access token.

---

## Define And Create Attachment
```python
attachment_id = fields.Many2many('ir.attachment')
```

```xml
<field name="attachment_id" widget="many2many_binary"/>
```

---