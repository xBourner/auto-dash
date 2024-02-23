---
title: Header
layout: home
nav_order: 5
parent: Configuration
---

# Header Configuration

The Header card shows:
- weather based on entity
- greetings
- clock

![image](https://github.com/xBourner/auto-dash/assets/64064679/38c8af10-f367-44ec-a832-6816f76ee9b6)


## Header Configuration

Default Variables:

      - weather: weather.openweathermap
      - greeting: Hello
      - greeting_morning: Good Morning
      - greeting_afternoon: Good Afternoon
      - greeting_evening: Good Evening
      - greeting_night: Good Night
      - monday: Monday
      - tuesday: Tuesday
      - wednesday: Wednesday
      - thursday: Thursday
      - friday: Friday
      - saturday: Saturday
      - sunday: Sunday

You can change the variables to the one you want. 

For example:

```yaml
type: custom:decluttering-card
view_layout:
  grid-area: header
template: header_card
variables:
  - greeting: Hallo
  - greeting_morning: Guten Morgen
  - greeting_afternoon: Schönen Nachmittag
  - greeting_evening: Guten Abend
  - greeting_night: Gute Nacht
  - monday: Montag
  - tuesday: Dienstag
  - wednesday: Mittwoch
  - thursday: Donnerstag
  - friday: Freitag
  - saturday: Samstag
  - sunday: Sonntag
```
