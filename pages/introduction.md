---
title: Introduction
layout: home
nav_order: 2
---

# Introduction

You need to have HACS already installed on your Home Assistant instance and you will need to know your way around .yaml file for integrate this.

There is no HACS integration as this is my own home dashboard. 

Prerequisites for custom integration can be found on the [Custom Integration Requirement page](https://avenger11.github.io/HomeOS-doc/pages/Custom%20integration%20required.html)


## A Typical files and folder directory layout for each card

> this is an example for the weather card integration

    .
    ├── ...
    ├── homeos.yaml                        # This is where the card is called !include HomeOS_Module/weather/weather_card.yaml 
    ├── homeos_Module
    │   ├── weather
    │       ├── weather_card.yaml          # Full definition of the card
    ├── packages
    │   ├── weather_package.yaml           # Entity to change to integrate the card
    ├── themes
    │   ├── homeos
    |       ├── general_theme.yaml
    │       ├── light
    │           ├── weather_theme_light.yaml
    │       ├── dark
    │           ├── weather_theme_dark.yaml
    ├── www 
    |   ├── homeos
    │       ├── weather_media           # Add the complete folder, it contains image, svg used by the card
    │           ├── clearsky_night.png
    │           ├── Icons=cloud.bolt.fill.png
    │           ├── ...
    

In conclusion, each card will potentially need the following file and folder in order to work properly:
* One folder in HomeOS_Module (i.e: weather folder) this is the definition of the card
* One folder in www for all the picture, icon used by the card (i.e weather_media)
* One file in package for the edition of the entity and value template (i.e weather_package.yaml)
* One file in themes/HomeOS/light for the light theme version of the card
* One file in themes/HomeOS/dark for the dark theme version of the card



# Naming convention

By default, everything will be in lower case.

### File convention

`name_type_sub-type.extension`

example: weather_card.yaml


### theme variable convention

`homeos-type-definition:`

example: homeos-topbar-height:
