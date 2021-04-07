# Implemenation plan

## Just something level

- Create a simple screen to enter new spending (just name/amount)
- Implement simple in-memory storage for spendings
- Create a screen with a list of spending, display spendings
- Add stats screens, that allows getting the total for a given period
- Move storage to DB solution

## Adding categories

- create blank settings screen (show version there)
- add categories list to settings
- add category select widget to create spending screen
- calculate stats by all categories

## Smarter spending list
- support search in spending list
- support filtering of spending by category
- support sorting by the amount
- support filtering by time
- auto redirect to spending screen from stats on click
on category with same time filter as for stats, sorted by
amount and filtered by category.

## Templates

- add create template screen (simple version without custom fields) 
and link it to settings
- add a "list of templates" screen and replace the settings menu item with a link to it.
- add "select template" screen: it shows when a user tries
to create a new spending. Support search there.
- Implement templated creation of spending

## Custom fields

- support defining of custom fields in the template editor screen
- add custom fields to the spending creation screen

## Duration of spending

- add the duration of spending both in template and spending screens
- adjust stats algorithm for duration spendings
- adjust the list of spending when time filter is enabled for duration spendings

## Query editor
TBD

## Server side
TBD
