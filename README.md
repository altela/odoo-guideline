# XML
### Override XML Template
You can always override XML record by following this example :

```xml
  <template id="product.report_productlabel" />
```

Inside your module, declare a line like the snippet above. it will monkeypatch `product.report_productlabel` inside the Odoo.

### Differentiate the view for HTML or Non HTML (WHKTMLTOPDF)
In case if you are developing a view that you wish to render to html or WKHTMLTOPDF you can use this
```xml
<div t-attf-class="#{'col-md-6 ml-auto' if report_type != 'html' else 'col-sm-7 col-md-6'}" style="width: 50%;">
    <!-- ... rest of your code ... -->
</div>
```

# Python
### Add New Selection Options
You can add new option into Odoo's fields.Selection(selection_add=[]) like :

```python
print_format = fields.Selection(selection_add=[
    ('zpl', 'ZPL Labels'),
    ('zplxprice', 'ZPL Labels with price')]
, ondelete={'zpl': 'set default', 'zplxprice': 'set default'})
```
If the print_format is defined somewhere in other module, you can just _inherit that module and make extension of the selection with the block above. it works.

### Create Access Token For A Record
Access token will mostly be used to access certain record, basically to let external user to see a record through certain page (like invoice).
In order to let your record has access token, you should make inheritance to your module like this example :

```python
_inherit = ['portal.mixin', 'mail.thread', 'mail.activity.mixin', 'sequence.mixin']
    
access_token = fields.Char()
```
Everything should have been exactly like example above, so it will allows you to make an access token.
