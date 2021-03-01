# Implemenation plan

## Just something level

- Create simple screen to enter new spending (just name/ammount)
- Implement simple in memory storage for spendings
- Create screen with list of spending, display spendings
- Add stats screens, that allows to get total for given period
- Move storage to DB solution

## Adding categories

- create blank settings screen (show version there)
- add categories list to settings
- add categories select widget to create spending screen
- calculate stats by all categories
- allow to select categories for stats

## Smarter spending list
- support search in spending list
- support filtering of spending by category
- support sorting by ammount
- support filtering by time
- auto redirect to spending screen from stats on click
on category with same time filter as for stats, sorted by
ammount and filtered by category.

## Templates

- add create template screen (simple version with out custom fields) 
and link it to settings
- add list of templates screen and replace settings entry
with it.
- add "select template" screen: it shows when user tries
to create new speding. Support custom option and search there.
- Implement templated creation of spending

## Custom fields

- support defining of custom fields in template editor screen
- add custom fields to spending creation screen

## Duration of spending

- add duration of spending both in template and spending screens
- adjust stats algorithm for duration spendings
- adjust list of spending when time filter is enabled for duration spendings

## Server side
TBD
