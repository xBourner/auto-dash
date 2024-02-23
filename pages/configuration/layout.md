---
title: Layout
layout: home
nav_order: 4
parent: Configuration
---

# Layout Configuration

Add the following config to a card

```yaml
title: Grid layout
type: custom:layout-card
layout_type: custom:grid-layout
layout:
  grid-template-columns: repeat(3, minmax(0px, 1fr))
  grid-template-rows: auto
  grid-template-areas: |
    "header . ."
    "status . ."
    "favorit . ."
    "room1 room2 room3"
    "room4 room5 room6"  
    "footer footer footer" 
  mediaquery:
    '(max-width: 600px)':
      grid-template-columns: repeat(2, minmax(0px, 1fr))
      grid-template-areas: |
        "header header"
        "status status"
        "favorit favorit"
        "room1 room2" 
        "room3 room4" 
        "room5 room5" 
    '(max-width: 1000px)':
      grid-template-columns: repeat(2, minmax(0px, 1fr))
      grid-template-areas: |
        "header header" 
        "status status"
        "favorit favorit"
        "room1 room2" 
        "room3 room4" 
        "room5 room6"
    '(max-width: 1200px)':
      grid-template-columns: repeat(3, minmax(0px, 1fr))
      grid-template-areas: |
        "header header ." 
        "status status ."
        "favorit favorit ."
        "room1 room2 room3" 
        "room4 room5 room6"
cards:
  - type: custom:decluttering-card
    view_layout:
      grid-area: header
    template: header_card

```

You can add your own cards or more decluttering cards with templates.
