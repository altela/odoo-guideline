---
layout: default
title: Good Practices
nav_order: 2
description: "Good practices when developing Odoo."
---

# Good Practices
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Always Include String Parameter When Declaring Fields
When you declare field in Odoo such as

```python
company_partner_id = fields.Many2one('res.partner')
```

it is better to put `string` parameter as well
```python
company_partner_id = fields.Many2one('res.partner', string='Client')
```
Why?
- It will enhance user experience when they wanna do import-export .xlsx
- You (as developer) will be able to declare it (fields) with different name (string) to it

Also, avoid naming field via .xml because it will make user confused. Declaring in .xml will only appear on XML level so when user exporting records to .xlsx (Excel File) they fill have a hard time.

## Never Use `store=True` For Computed Fields
Never declare a computed field with parameter `store=True`.

```python
money_count = fields.Float(compute=compute_money_count, store=True)

@api.depends('money_count')
def compute_money_count(self):
  self.money_count = self.another_field_1 * self.another_field_2
```

That's because it will lead to a consistency issue. `money_count` above is supposed to keep calculation when certain condition are met. If the `@api.depends()` decorator are triggered, the first calculation will be saved into the database. However, the second or third may not (at it will still use the first calculation result) and it's really dangerous. Make sure to never store any calculated field into database.

## Always Define `company_id` field to support multicompany
The title says it all. Just do it.
```python
company_id = fields.Many2one('res.company', 'Company', default=lambda self: self.env.company)
```

## Always Broke Your Code Into Parts
Split your code into different method. This will ensure the code is reusable.
- Using single parameter like `self` is fine.
- Use `return` so you can assign the result into other object.
  
Example
```python
def multiply(self):
    return self.field_one * self.field_two

self.field_three = self.multiply()
```
