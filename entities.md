# Entities

This file specifies what entities will be there in the money log app.

## Spending

Single (atomic) spending of money done by the user. The "building block"
for statistics.

### Fields:

- name - simple string that describes spending
- amount - the amount of money spent
- date - a day when spending has happened.
- duration - time interval duration paid by this spending. Empty
if not applicable. This will change how statistics are calculated:
for example, payment for year subscription will be divided by 12 for
single month stats.
entry.
- category - list of categories this spending belongs to.
- structured info - some additional JSON-like data with format
specified by the template.

## Category

A label that can be added to spending to allow aggregated stats.
Note that because single spending always belongs to one category.

### Fields:

- name - Name for the category

## Template

A template that helps a user to create new spending by filling up
some fields. Also allows creating structured fields to describe
spendings more precisely.

### Fields:

- name - Name of the template
- defaults - Obj with default values for spending. All
             of following items is optional
  - name - default spending name
  - amount - default spending amount
  - duration - default spending duration
  - category - default category
- fields - list of fields that will be presented in structured
           info of the spending.
  - name - name of the field
  - type - a type of the field (either string or int or enum)
  - default - field default (optional)
  - options - list of possible options for enum type. Missing
              if not an enum

## Statistics query

Represent a plan on how to group spendings to generate the total amount for each group. 
The simplest example is "per category query" - it selects spendings by category and 
presents total amount for each one.

Query defines two steps: how to filter spending that a relevant for this query and what field to use for grouping. Filtering is specified by a list of filters, where each filter is triple "field", "operator", "constant value".

### Fields

- name - Name of the query
- grouping field - Either name of top-level field of spending or 'custom.<field_name>' where <field_name> represents name of custom field
- filters - list of filters of the following format:
  -  field - Either name of top level field of spending or 'custom.<field_name>' where <field_name> represents name of custom field
  -  operator - one of '==', '!=', '<=', "<", ">=", ">". Ordering operators are not support for string fields
  -  contant value - either literal (string or number), relative date (like '30d') that is evaluted as ('now' - relative) while filtering, or some special values like 'this week', 'this month', 'this year'.


