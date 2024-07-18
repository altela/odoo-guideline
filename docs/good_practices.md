## Always include string parameter when declaring fields
When you declare field in python such as

```python
company_partner_id = fields.Many2one('res.partner')
```

it is better to put `string` parameter as well
```python
company_partner_id = fields.Many2one('res.partner', string='Client')
```
Why?
- It will enhance user experience when import-export .xlsx
- Possibility to declaring variable and defining different name (string) for it

Also, avoid naming field via .xml because in this case it will become useless and the field names will make user confused. In this image below, string in XML is `Client` and the fields is `Company Partner`

![Screen Shot 2024-06-08 at 15 41 02](https://github.com/altela/odoo-doc/assets/68892527/68f88c94-d9ab-449a-848a-673425cd5df1)

## Never Use `store=True` For Computed Fields
Never declare a computed field with parameter `store=True`.

```python
money_count = fields.Float(compute=compute_money_count, store=True)
```

That's because it will lead to a consistency issue. `money_count` above is supposed to keep calculation when certain condition are met. If the @api.depends() decorator are triggered, the first calculation will be saved into the database. However, the second or third may not and it's really dangerous. Make sure to never store any calculated field into database.

## Always Define `company_id` field to support multicompany
The title says it all. Just do it.
```python
company_id = fields.Many2one('res.company', 'Company', default=lambda self: self.env.company)
```

## Always Broke Your Code To Parts
Split your code into different method. This will ensure the code is reusable.
- Using single parameter like `self` is fine.
- Return. Use return so you can assign the result into variable
  
Example
```python
def multiply(self):
    return self.field_one * self.field_two

self.field_three = self.multiply()
```
