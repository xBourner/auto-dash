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

The Header Card comes automatically with the default template. This is the easiest way to setup.
<br> <br> 
You can delete the header card if you want to. Just delete the code further down.

### Available Variables:

| Variable | Option | Requirement | Default | Description |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| weather | entity | optional | weather.openweathermap | Define your Weather Entity |
| greeting| string | optional | Hello | Define your default Greeting |
| greeting_morning | string | optional | Good Morning | Define your Greeting for the morning |
| greeting_afternoon | string | optional | Good Afternoon | Define your Greeting for the afternoon |
| greeting_evening | string | optional | Good Evening | Define your Greeting for the evening |
| greeting_night | string | optional | Good Night | Define your Greeting for the night |
| monday | string | optional | Monday | Define the wording for "Monday" in your language |
| tuesday | string | optional | Tuesday | Define the wording for "Tuesday" in your language |
| wedenesday | string | optional | Wednesday | Define the wording for "Wednesday" in your language |
| thursday | string | optional | Thursday | Define the wording for "Thursday" in your language |
| friday | string | optional | Friday | Define the wording for "Friday" in your language |
| saturday | string | optional | Saturday | Define the wording for "Saturday" in your language |
| sunday | string | optional | Sunday | Define the wording for "Sunday" in your language |


Add a default card with the code:

```yaml
type: custom:decluttering-card
view_layout:
  grid-area: header
template: header_card
```

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
