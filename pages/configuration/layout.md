---
title: Layout
layout: home
nav_order: 4
parent: Configuration
---

# Layout Configuration

This dashboard is based on the layout-card. <br>
You can easilly put cards into columns which will behave different on mobile, tablet or desktop.
<br> <br>

You dont'have to explicitly add some code here. This site is more like an explanation.

Here is the grid layout:
![image](https://github.com/xBourner/auto-dash/assets/64064679/02174034-e860-49e9-abf7-5c33b973f766)

which comes from the code:

```
  grid-template-areas: |
    "header . ."
    "status . ."
    "favorit . ."
    "floor1 floor2 floor3"
    "room1 room2 room3"
    "room4 room5 room6"  
    "footer footer footer"
```

A dot (.) means the grid column is empty. Repeat the name of the grid to fill the columns.
You can add/remove the colums. Change the code to this:

```
  grid-template-columns: repeat(2, minmax(0px, 1fr))
  grid-template-areas: |
    "header . "
    "status . "
    "favorit . "
    "floor1 floor1"
    "floor2 floor2"
    "floor3 floor3"
    "room1 room2"
    "room3 room4"  
    "footer footer"
```
## Configuration

If you want to add some floors because you have to many rooms you can add a simple card with navigation action.
Add a subview for the desired floor. Add a navigation action like shown in code:


```yaml
  - type: custom:mushroom-template-card
    view_layout:
      grid-area: floor1
    primary: Floor 1
    icon: mdi:numeric-1
    tap_action:
      action: navigate
      navigation_path: /auto-dash/floor1
```

Or by adding via gui editor:
![image](https://github.com/xBourner/auto-dash/assets/64064679/470aee36-c16b-4800-846c-516128a77c30)

Name it like you want. The only required thing is to use the correct grid area naming. 
Add floor1, floor2 or floor3 to the area to show the card at the correct place.

```yaml
    view_layout:
      grid-area: floor1
```

You can add your own cards or more decluttering cards with templates.
