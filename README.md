# XML
## Use print_report_name To Define the Report PDF Name
```xml
    <record id="report_action_field_ticket" model="ir.actions.report">
        <field name="name">Field Ticket</field>
        <field name="model">field.ticket.generator</field>
        <field name="report_type">qweb-pdf</field>
        <field name="print_report_name">'Echo Ticket %s %s' % (object.display_name, object.creation_date.strftime('%d-%m-%Y'))</field>
        <field name="report_name">field_ticket_generator.report_field_ticket</field>
        <field name="report_file">field_ticket_generator.report_field_ticket</field>
        <field name="binding_model_id" ref="model_field_ticket_generator" />
        <field name="binding_type">report</field>
    </record>
```
Result :

![Screen Shot 2024-06-19 at 10 40 22](https://github.com/altela/odoo-doc/assets/68892527/47e24f92-3e08-47da-8fcf-26236e0bcbb6)

## Split XML Group Into Multiple Column
```xml
    <group col="1">
        <group col="3">
            <group >
                <field name="unit_make" />
            </group>
            <group>
                <field name="unit_size"/>
            </group>
            <group >
                <field name="run_cycle"/>
            </group>
            <group>
                <field name="unit_of_pins"/>
            </group>
            <group>
                <field name="stroke_length"/>
            </group>
            <group >
                <field name="psn_depth"/>
            </group>
            <group>
                <field name="tubing_depth"/>
            </group>
        </group>
    </group>
```
Result

<img width="1170" alt="Screen Shot 2024-06-14 at 00 07 43" src="https://github.com/altela/odoo-doc/assets/68892527/0a8d01a7-bf1c-4ff5-b8da-60f212dc094b">


## Align Right Using Div
```xml
    <div style="text-align:right; margin-top:20px; ">
        <div style="text-align: left">
            <div style="float:right; color:#000; font-size:17px; width:30%; margin-bottom:20px">
                <b>Ticket Number :</b> <span t-esc="o.ticket_number"/><br/>
                <b>Ticket Date :</b> <span t-esc="context_timestamp(o.create_date).strftime('%d-%b-%Y')"/><br/>
                <b>Major-Minor :</b> <span t-esc="o.minor_major"/><br/>
            </div>
        </div>
    </div>
```
![image](https://github.com/altela/odoo-doc/assets/68892527/aff620b9-90a6-414e-947c-608e9089280a)

## Changing Form String Name Title
```xml
    <form string="Change Effective Date">
```

## Transparent Line Table
```xml
<tr style="color: #000; border: 0; border-top: 1px solid transparent; border-left: 1px solid transparent; border-right: 1px solid transparent; border-bottom: 1px solid transparent;">
```
## Transparent Background Table
```xml
<td width="45%" style="border: none; background-color: transparent; color: #000; font-size: 15px;">
```

## Set Multicompany Rules For Record of Different Company
```xml
    <record model="ir.rule" id="default_stock_journal_multicompany_rule">
        <field name="name">Multi-Company Default Stock Journal</field>
        <field name="model_id" search="[('model','=','journal.setup.effective')]" model="ir.model"/>
        <field name="domain_force">[('company_id', 'in', company_ids)]</field>
    </record>
```

## Make Total Box
```xml
    <div class="oe_right" style="display: flex; justify-content: flex-end; align-items: center; margin-bottom: 10px;">
        <table width="10%" style="border: 0px !important; border-collapse: collapse;">
            <tr style="border: none; background-color:#fff; color: #000;">
                <td class="border-0" style="border: none; background-color:#fff; color: #000; padding-bottom: 5px;">
                    <div>
                        <b>Subtotal :</b>
                    </div>
                </td>
                <td style="border: none; background-color:#fff; color: #000; text-align: right;">
                    <div>
                        <field name="subtotal" style="text-align:right;" force_save="1" readonly="1"/>
                    </div>
                </td>
            </tr>
        </table>
    </div>
```
![Screen Shot 2024-05-31 at 11 01 43](https://github.com/altela/odoo-doc/assets/68892527/a870800d-928d-4007-a704-0bcfb7f04d14)

## QWEB Report Two Table Left and Right
```xml
<table width="100%" style="border: 0px !important; border-collapse: collapse; ">
    <tr style="border-top: 0px solid #fff; border-left: 0px solid #fff; border-right: 0px solid #fff; border-bottom: 0px solid #fff; border-collapse: collapse; background-color:#fff; color: #000" >
        <td class="border-0" width="45%" style="border: none; background-color:#fff; color: #000; padding-bottom: 5px; font-size:15px">
            <div style="font-size:20px">
                <b>Company :</b> <t t-esc="o.company_id.name"/>
            </div>
        </td>

        <td width="25%" style="border: none; background-color:#fff; color: #000; font-size:15px">
        </td>

        <td width="45%" style="border: none; background-color:#fff; color: #000; font-size:15px">
            <div style="font-size:20px">
                <b>Field :</b> Field A
            </div>
        </td>
    </tr>
</table>
```

This will result as follow

<img width="687" alt="image" src="https://github.com/altela/odoo-doc/assets/68892527/925802a8-6e5f-4bf7-ac17-d52067160ee6">

## Make XML State With Badge and Color
```xml
<field name="state" string="State" optional="show" widget="badge" decoration-info="state in ('draft', 'ticket_sent')" decoration-success="state in ('ticket_approved', 'invoiced')"/>
```

Will resulting like this

<img width="1122" alt="Screen Shot 2024-06-01 at 10 17 35" src="https://github.com/altela/odoo-doc/assets/68892527/fd75fc60-ffb9-4b19-ae8c-7faaedc216fb">

## Defining Password Field
```xml
<field name"sensitive_field" password="True" >
```

## Make Field 100% Of Form Sheet View
```xml
    <group class="oe_structure">
        <field name="uwi" placeholder="100/00-00-000-00W5/00" string="UWI"/>
    </group>
```
## Display Currency in the QWEB
```xml
<span t-field="line.amount" t-options="{'widget': 'monetary', 'display_currency': o.currency_id}"/>
```

## Display Floating Header on The Right
```xml
    <div style="width:100%; display:flex; flex-direction:column;">
        <div style="display:flex; justify-content:flex-end; padding-bottom: 2px;">
            <div style="width:55%; font-size:15px; text-align:left; margin-left:550px;">
                <div style="font-size:20px; text-align:left; color:#000">
                    <b>Ticket Number :</b> <span t-esc="o.ticket_number" /><br/>
                    <b>Ticket Date :</b> <span t-esc="context_timestamp(o.create_date).strftime('%d-%b-%Y')" /><br/>
                    <b>Major-Minor :</b> <span t-esc="o.minor_major" /><br/>
                </div>
            </div>
        </div>
    </div>
```
The result will be

![image](https://github.com/altela/odoo-doc/assets/68892527/db102ba4-3ee9-48bb-a81e-ab41ae244c21)


## Create a Search
```xml
<record id="well_information_search" model="ir.ui.view">
    <field name="name">well.information.search</field>
    <field name="model">well.information</field>
    <field name="arch" type="xml">
        <search>
            <field name="uwi" string="UWI"/>
            <field name="company_id" string="Company"/>
            <field name="employee_id" string="Employee"/>
            <field name="well_license" string="Well License"/>
        </search>
    </field>
</record>
```

## Make Field On the Far Left and The Far Right
```xml
<!-- UWI And Date -->
<div style="display: flex; justify-content: space-between; gap: 100px;">
    <div style="flex: 1;">
        <group>
            <field name="uwi" placeholder="100/00-00-000-00W5/00" string="UWI"/>
        </group>
    </div>
    
    <div style="flex: 1; display: flex; justify-content: flex-end;">
        <group>
            <field name="create_date" />
        </group>
    </div>
</div>
```
The result

![Screen Shot 2024-05-17 at 14 22 44](https://github.com/altela/odoo-doc/assets/68892527/e9d21c9f-88f2-4e48-a09a-dc650d4bedfe)

## One2Many Attribute No Create, No Create Edit
```xml
<notebook>
    <page string="Product Exchange">
        <field name="product_line" options="{'no_create': True, 'no_create_edit':True}">
            .....
        </field>
    </page>
</notebook>
```

## Print Date in Report
```xml
<t t-esc="context_timestamp(o.attendance_start).strftime('%d/%m/%Y')"/>
```

## Overlay Text

![Screen Shot 2024-05-07 at 09 31 00](https://github.com/altela/odoo-doc/assets/68892527/b87c7b38-a17f-422f-87ef-54ab33b1b7e2)

```xml
    <div class="alert alert-info">
        <t>Semua record yang berhasil muncul adalah record yang berstatus 'Paid'.</t>
    </div>
```

## Readable or Clickable State
Make state readable or clickable
```xml
<field name="state" widget="statusbar" options="{'clickable':1}"/>
```

## Many2many Checkbox
Add checkbox on many2many
```xml
<field name="order_pengiriman_recap" widget="many2many_checkboxes"/>
```
It will resulting like this

![Screen Shot 2024-04-16 at 00 50 13](https://github.com/altela/odoo-doc/assets/68892527/5103f79d-a2b1-44e0-83c3-a8639148bd6a)

## Override XML Template
You can always override XML record by following this example :

```xml
<template id="product.report_productlabel" />
```

Inside your module, declare a line like the snippet above. it will monkeypatch `product.report_productlabel` inside the Odoo.

## Define a Button using XML Action instead of Python def()
You can define XML action as a button through this block example :
```xml

<!-- Template Example -->
<xpath expr="//button[@name='button_validate']" position="after">
    <button name="%(module_name.action_name)d" type="action" data-hotkey="z" string="Dispatch"/>
</xpath>

<!-- Real Case Example -->
<xpath expr="//button[@name='button_validate']" position="after">
    <button name="%(dispatch_selective.action_dispatch_selective)d" type="action" data-hotkey="z" string="Dispatch"/>
</xpath>
```

## Differentiate the view for HTML or Non HTML (WHKTMLTOPDF)
In case if you are developing a view that you wish to render to html or WKHTMLTOPDF you can use this
```xml
<div t-attf-class="#{'col-md-6 ml-auto' if report_type != 'html' else 'col-sm-7 col-md-6'}" style="width: 50%;">
    <!-- ... rest of your code ... -->
</div>
```

## QWEB Remove Trailing Digits And Add Thousand Separator
You can remove the trailing digits and add thousand separator using this format :
```xml
Rp<t t-esc="'{:,}'.format(int(monthly_penerimaan.nominal)).replace(',', '.')"/>
```

## QWEB Make Empty Single Line in the Table
This will make empty single lin in the table
```xml
    <tr>
        <td>&#160;</td>
        <td>&#160;</td>
        <td>&#160;</td>
    </tr>
```

## QWEB Signature
This will help you to make single signature on the bottom right
```xml
<div style="float: left; color:#000; text-align:left; font-size:15px; margin-bottom:20px; margin-top:25px">
    <div class="row" style="page-break-inside:avoid;">
        <div class="text-center" style="margin-left:600px">
            <div class="col-sm-2" style="color:#000">
                Penerima,
                    <br/>
                    <t t-esc="o.employee_id.name" />
                <br/>
                <br/>
                <br/>
                <br/>
            </div>

            <div class="col-sm-2" style="color:#000">
                <p>(____________________)</p>
            </div>
        </div>
    </div>
</div>
```

# Python

## Forcefully write debit-credit

using .with_context() will help to forcefully write debit-credit of journal entry without cancelling it.

```python
journal_entry_line = self.env['account.move.line'].search([('ref', '=', str(self.name + " - " + " Vendor Exchange Out" + " - " + line.main_product.name))])
for record in journal_entry_line:
    if record.debit > 0:
        record.with_context(check_move_validity=False).write({'debit': line.main_product_cost * line.main_product_qty})
    if record.credit > 0:
        record.with_context(check_move_validity=False).write({'credit': line.main_product_cost * line.main_product_qty})
```

## Set Current Date Using fields
```python
define_a_date = fields.Date.today()

# or

define_a_date = fields.Date.context_today(self)
```

## Link register payment with invoice
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

## Passing Context from other_method() into create() method
This also can be applied to another method but to simplify stuff we will use create() method instead
first we inherit a create() method into the module
```python
    @api.model
    def create(self, vals):
        if vals.get('payroll_number', 'New') == 'New':
            vals['payroll_number'] = self.env['ir.sequence'].next_by_code('payroll.number.sequence') or 'New'
        result = super(CentralPayroll, self).create(vals)

        return result
```

now, in another part of the block, we have cron_auto_create_monthly_transfer_payroll(). We need to create context as dictionary to be passed on when create() is called
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
    @api.model
    def create(self, vals):
        if vals.get('payroll_number', 'New') == 'New':
            vals['payroll_number'] = self.env['ir.sequence'].next_by_code('payroll.number.sequence') or 'New'
        result = super(CentralPayroll, self).create(vals)

        context = self.env.context
        print("Context:", context) # This shall print out all context
        return result
```

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

## Auto Create Sequence
Generate sequence when record is created
```python
class RekapOrder(models.Model):
    _name = 'rekap.order'
    _description = 'Rekap Order Pengiriman'
    _rec_name = 'name'

    name = fields.Char(readonly=True, required=True, copy=False, default='New')
    @api.model
    def create(self, vals):
        # Auto Assign record name
        if vals.get('name', 'New') == 'New':
            vals['name'] = self.env['ir.sequence'].with_company(self.company_id.id).next_by_code('rekap.order.sequence') or 'New'
        result = super(OrderSetoran, self).create(vals)

        return result
```

## Set Domain Based on Company
```python
    account_stock_journal = fields.Many2one('account.journal', ondelete='restrict', domain=lambda self: self._get_company_domain())

    @api.model
    def _get_company_domain(self):
        return [('company_id', '=', self.env.company.id)]
```

## Compute Tax In Odoo in Recommended Way
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

## Download Attachment Through Button, Make ir.attachment, And Create Email
It works like casual Odoo email sending, but not using window pop up
```python
    def send_email(self):
        # Step 1: Generate the report
        report_pdf = self.env["ir.actions.report"].sudo()._render_qweb_pdf(self.env.ref('crm_project_task.report_well_information_project'), res_ids=self.id)

        # Step 2 : Create ir.attachment
        filename = 'altela' + '.pdf'
        attachment = self.env['ir.attachment'].create({
            'name': filename,
            'type': 'binary',
            'datas': base64.b64encode(report_pdf[0]),
            'res_model': 'project.task',
            'res_id': self.id,
            'mimetype': 'application/x-pdf'
        })

        # Step 4: Create a new record in 'email.record' with the attachment
        email_record = self.env['email.record'].create({
            'attachments': [(4, attachment.id, False)],
        })

        # Step 5: Return an action to open the newly created email_record
        action = {
            'type': 'ir.actions.act_window',
            'name': 'Email Record',
            'res_model': 'email.record',
            'view_mode': 'form',
            'res_id': email_record.id,
            'target': 'current',
        }
        return action
```

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

## Assign Existing Record into Many2many
```python
previous_well_info = fields.Many2many('well.information', 'previous_well_info', 'uwi', 'uwi_id', string='Legumes', readonly=True)

def fetch_previous_uwi(self):
     previous_uwi = self.env['well.information'].search([('uwi', '=', self.uwi), ('id', '!=', self.id)])
     self.previous_well_info = [(5, 0, 0)]

     if bool(previous_uwi):
         self.previous_well_info = previous_uwi.ids
```

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

## Define Single Digits
```python
psn_depth = fields.Float(string="PSN Depth", digits=(6, 1))
```

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

## Find Record in Database Based on XML ID
```python
record_id = self.env.ref('module_name.xml_id_of_record').id 
```

## Add New Selection Options
You can add new option into Odoo's fields.Selection(selection_add=[]) like :

```python
print_format = fields.Selection(selection_add=[
    ('zpl', 'ZPL Labels'),
    ('zplxprice', 'ZPL Labels with price')]
, ondelete={'zpl': 'set default', 'zplxprice': 'set default'})
```
If the print_format is defined somewhere in other module, you can just _inherit that module and make extension of the selection with the block above. it works.

## Create Access Token For A Record
Access token will mostly be used to access certain record, basically to let external user to see a record through certain page (like invoice).
In order to let your record has access token, you should make inheritance to your module like this example :

```python
_inherit = ['portal.mixin', 'mail.thread', 'mail.activity.mixin', 'sequence.mixin']
    
access_token = fields.Char()
```
Everything should have been exactly like example above, so it will allows you to make an access token.

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

## Define And Create Attachment
```python
attachment_id = fields.Many2many('ir.attachment')
```

```xml
<field name="attachment_id" widget="many2many_binary"/>
```

# QOL Programming And Tips

## Error at row 0: "Unknown error during import: <class 'TypeError'>: list indices must be integers or slices, not str"
This error mostly come during import-export product
Make sure that indices field is not refering to existing record such as this example
```python
    # The previous_well_info is an indiced (many2many) field
    def create(self, vals):
        res = super(WellInformation, self).create(vals)

        previous_uwi = self.env['well.information'].search([('uwi', '=', vals['uwi']), ('id', '!=', res.id)])

        if bool(previous_uwi):
            res.previous_well_info = previous_uwi.ids

        return res

```
It should be done this way
```python
    def create(self, vals):
        res = super(WellInformation, self).create(vals)

        try:
            # For regular create() process by casual user
            previous_uwi = self.env['well.information'].search([('uwi', '=', vals['uwi']), ('id', '!=', res.id)])

            if bool(previous_uwi):
                res.previous_well_info = previous_uwi.ids
        except:
            # For upload (import) process
            previous_uwi = self.env['well.information'].search([('uwi', '=', vals[0]['uwi']), ('id', '!=', res.id)])

            if bool(previous_uwi):
                res.previous_well_info = previous_uwi.ids

        return res
```

## Why You Should include string parameter into fields declaration
When you declare field in python such as

```python
company_partner_id = fields.Many2one('res.partner')
```

it is better to put string parameter as well
```python
company_partner_id = fields.Many2one('res.partner', string='Client')
```
Why?
Because it is better for you when you already create a field which has data into it but customer require that field under the other name. Thus, instead of renaming the field it is better to just declare string to it.
Both you and user will be happy. In addition, it will increase user's QoL during import-export data.
When they are importing and exporting, both object (variable) and UI string will be appeared at the same moment

![Screen Shot 2024-06-08 at 15 35 59](https://github.com/altela/odoo-doc/assets/68892527/58eeaaec-fb01-4de0-b4f5-d86ed7431921)

when you are not declaring it, it will show as 'Company Partner ID' instead

![Screen Shot 2024-06-08 at 15 37 58](https://github.com/altela/odoo-doc/assets/68892527/7163697d-e2c9-4b3b-8d34-acdf8ada3323)

Also, avoid naming field via .xml because in this case it will become useless and the field names will make user confused

![Screen Shot 2024-06-08 at 15 41 02](https://github.com/altela/odoo-doc/assets/68892527/68f88c94-d9ab-449a-848a-673425cd5df1)

The good thing is, during import, when you specifiy either string name or field name inside .xls or .csv file, it will support both name
