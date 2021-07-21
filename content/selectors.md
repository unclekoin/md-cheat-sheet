[&#8678; get back to the start...](../README.md)
# CSS Selecors

## Basic selectors
* `*` - universal selector
* `div` - type selectot
* `.class` - class selector
* `#id` - id selector
* `[attr=value]` - attribute selector

## Grouping selectors
* `div, span`

## Combinators
* `div span` - descendant combinator
* `ul > li` - child combinator
* `p ~ span` - seneral sibling combinator
* `h2 + p` - adjacent sibling combinator
* `col || td` - column combinator 🧪

## Filters
* `:first-child`
* `:last-child`
* `:only-child`
* `:nth-child(a)`
* `:nth-last-child(a)`
---
* `:first-of-type`
* `:last-of-type`
* `:only-of-type`
* `:nth-of-type`
* `:nth-last-of-type`

## Attribute selectors
* `[attr]`
* `[attr="val"]`
* `[attr^="val"]`
* `[attr|="val"]`
* `[attr*="val"]`
* `[attr~="val"]`
* `[attr$="val"]`

## Pseudo classes
* `:link`
* `:visited`
* `:focus`
* `:hover`
* `:active`
* `:empty`
* `:checked`
* `:disabled`
* `:enabled`
* `:target`

## Pseudo elements
* `::before`
* `::after`