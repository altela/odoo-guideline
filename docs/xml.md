---
layout: default
title: XML Notes
nav_order: 4
description: "XML Notes."
---

# XML Notes

## Unpack List in XML Qweb in Single Line
```xml
<t t-set="technician" t-value="['Aal', 'Altela']"/>
Technician : <span t-esc="', '.join(technician)"/>
```

---

## Sort XML QWEB Based on Date
```xml
<t t-set="sorted_uwi_main" t-value="sorted(docs.work_table.uwi, key=lambda r: r.creation_date, reverse=True)"/>
<t t-foreach="sorted_uwi_main" t-as="o">
    <!-- Start Lopping -->
</t>
```

---

## Transparent Line Table
```xml
<tr style="color: #000; border: 0; border-top: 1px solid transparent; border-left: 1px solid transparent; border-right: 1px solid transparent; border-bottom: 1px solid transparent;">
```

---

## Transparent Background Table
```xml
<td width="45%" style="border: none; background-color: transparent; color: #000; font-size: 15px;">
```

---

## Use `print_report_name` To Define the Report PDF Name
```xml
<record id="report_action_field_ticket" model="ir.actions.report">
    <field name="name">Field Ticket</field>
    <field name="model">field.ticket.generator</field>
    <field name="report_type">qweb-pdf</field>
    <field name="print_report_name">'The File Name %s %s' % (object.display_name, object.creation_date.strftime('%d-%m-%Y'))</field>
    <field name="report_name">field_ticket_generator.report_field_ticket</field>
    <field name="report_file">field_ticket_generator.report_field_ticket</field>
    <field name="binding_model_id" ref="model_field_ticket_generator" />
    <field name="binding_type">report</field>
</record>
```

---

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

Result

![Screen Shot 2024-07-21 at 23 19 51](https://github.com/user-attachments/assets/b1ef91cc-e814-4996-b971-99ebe6092af7)

---

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

---

## Changing Form String Name Title
```xml
<form string="Change Effective Date">
```

---

## Set Multicompany Rules For Record of Different Company
```xml
<record model="ir.rule" id="default_stock_journal_multicompany_rule">
    <field name="name">Multi-Company Default Stock Journal</field>
    <field name="model_id" search="[('model','=','journal.setup.effective')]" model="ir.model"/>
    <field name="domain_force">[('company_id', 'in', company_ids)]</field>
</record>
```

---

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

---

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

---

## Make XML State With Badge and Color
```xml
<field name="state" string="State" optional="show" widget="badge" decoration-info="state in ('draft', 'ticket_sent')" decoration-success="state in ('ticket_approved', 'invoiced')"/>
```

Will resulting like this

<img width="1122" alt="Screen Shot 2024-06-01 at 10 17 35" src="https://github.com/altela/odoo-doc/assets/68892527/fd75fc60-ffb9-4b19-ae8c-7faaedc216fb">

---

## Defining Password Field
```xml
<field name"sensitive_field" password="True" >
```

---

## Make Field 100% Of Form Sheet View
```xml
<group class="oe_structure">
    <field name="uwi" placeholder="100/00-00-000-00W5/00" string="UWI"/>
</group>
```

---

## Display Currency in the QWEB
```xml
<span t-field="line.amount" t-options="{'widget': 'monetary', 'display_currency': o.currency_id}"/>
```

---

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

---

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

---

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

---

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

---

## Print Date in Report
```xml
<t t-esc="context_timestamp(o.attendance_start).strftime('%d/%m/%Y')"/>
```

---

## Overlay Text

```xml
<div class="alert alert-info">
    <t>Semua record yang berhasil muncul adalah record yang berstatus 'Paid'.</t>
</div>
```

Result :

![Screen Shot 2024-05-07 at 09 31 00](https://github.com/altela/odoo-doc/assets/68892527/b87c7b38-a17f-422f-87ef-54ab33b1b7e2)

---

## Readable or Clickable State
Make state readable or clickable
```xml
<field name="state" widget="statusbar" options="{'clickable':1}"/>
```

---

## Many2many Checkbox
Add checkbox on many2many
```xml
<field name="order_pengiriman_recap" widget="many2many_checkboxes"/>
```
It will resulting like this

![Screen Shot 2024-04-16 at 00 50 13](https://github.com/altela/odoo-doc/assets/68892527/5103f79d-a2b1-44e0-83c3-a8639148bd6a)

---

## Override XML Template
You can always override XML record by following this example :

```xml
<template id="product.report_productlabel" />
```

Inside your module, declare a line like the snippet above. it will monkeypatch `product.report_productlabel` inside the Odoo.

---

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

---

## Differentiate the view for HTML or Non HTML (WHKTMLTOPDF)
In case if you are developing a view that you wish to render to html or WKHTMLTOPDF you can use this
```xml
<div t-attf-class="#{'col-md-6 ml-auto' if report_type != 'html' else 'col-sm-7 col-md-6'}" style="width: 50%;">
    <!-- ... rest of your code ... -->
</div>
```

---

## QWEB Remove Trailing Digits And Add Thousand Separator
You can remove the trailing digits and add thousand separator using this format :
```xml
Rp<t t-esc="'{:,}'.format(int(monthly_penerimaan.nominal)).replace(',', '.')"/>
```

---

## QWEB Make Empty Single Line in the Table
This will make empty single lin in the table
```xml
<tr>
    <td>&#160;</td>
    <td>&#160;</td>
    <td>&#160;</td>
</tr>
```

---

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

---

This will make two column
```xml
<div style="float: left; color:#000; text-align:left; font-size:15px; margin-bottom:20px; margin-top:25px">
    <div class="row" style="page-break-inside:avoid;">
        <div class="text-center" style="margin-left:50px">
            <div class="col-sm-2" style="color:#000">
                Tertanda,
                <br/>
                <br/>
                <br/>
                <br/>
            </div>

            <div class="col-sm-2" style="color:#000">
                <p>(____________________)</p>
            </div>
        </div>

        <div class="text-center" style="margin-left:450px">
            <div class="col-sm-2" style="color:#000">
                Mengetahui,
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
