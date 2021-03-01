# Entities

This files specifies what entites will be there in money log app.

## Spending

Single (atomic) spending of money done by user. The "building block"
for statistics.

### Fields:

- name - simple string that describes spending
- amount - amount of money spent
- datetime - moment when spending have happened.
- duration - time interval duration payed by this spending. Empty
if not applicable. This will change how statistics is calculated:
for example, payment for year suscription will by divided by 12 for
single month stats.
- template - `Template` used to create spending. Empty if user customized
entry.
- categories - list of categories this spending belongs.
- description - any additional info added by user.
- structured info - some additional json-like data with format
specified by template.

## Category

Label that can be added to spending to allow aggregated stats.
Note that because single spending can belong to multiple categories.
This allows to divide spending in groups by different dimensions,
for example by purporse and by person who did it (for family logs).

### Fields:

- name - Name for the category

## Template

Template that helps user to create new spending by filling up
some fields. Also allows to create structed fields to describe
spendings more precisly.

### Fields:

- name - Name of the template
- defaults - Obj with default values for spending. All
             of following items is optional
  - name - default spending name
  - amount - default spending amount
  - duration - default spending duration
  - categories - list of default categories
  - description - default spending description
- fields - list of fields that will be presented in structured
           info of the spending.
  - name - name of the field
  - type - type of the field (either string or int or enum)
  - default - field default (optional)
  - options - list of possible options for enum type. Missing
              if not a enum
