# XML

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

