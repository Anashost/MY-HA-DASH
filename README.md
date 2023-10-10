[![GitHub Latest Release][releases_shield]][latest_release]
[![GitHub All Releases][downloads_total_shield]][releases]
[![Community Forum][community_forum_shield]][community_forum]<!-- anashost_support_badges_start -->
[![PayPal.Me][paypal_me_shield]][paypal_me]
[![Revolut.Me][revolut_me_shield]][revolut_me]
[![ko_fi][ko_fi_shield]][ko_fi_me]
<!-- anashost_support_badges_end -->

Here's a [link](https://github.com/Anashost/MY-HA-DASH-V1) to the old version of this dashboard.

# In this repo i will share with you the code of my dashboard, mainly those cards:
- **Header card**
- **Persons cards**
- **Rooms cards**
<img src="https://github.com/Anashost/MY-HA-DASH/assets/41167157/fb7c4c4a-df88-41c9-8b4e-3d79cdbf88ea" width=50% height=50%>

### Notes:
- if you dont have motion sensors, you can remove the motion sensors sections from the card.
- for fans, i'm just using smart plugs exammple: `switch.fan_plug`.
- try to understand what the code does in the card to be able to customize the cards the way you like.
- if you have any qustions, feel free to open an issue and i will reply ASAP.
- more coming soon, follow me on [YouTube](https://www.youtube.com/@anasbox)

# preparation:
### 1. you need to install the folowing integrations from HACS:
>In this [YouTube video](https://youtu.be/96TTQJ7quMY) i went through the cards we need which are the folowing:
>
>For Home page:
>* [lovelace-mushroom](https://github.com/piitaya/lovelace-mushroom)
>* [lovelace-mushroom-themes](https://github.com/piitaya/lovelace-mushroom-themes)
>* [button-card](https://github.com/custom-cards/button-card)
>* [stack-in-card](https://github.com/custom-cards/stack-in-card)
>* [swipe-card](https://github.com/bramkragten/swipe-card)
>* [card-mod](https://github.com/thomasloven/lovelace-card-mod)
>
>For other views:
>* [mini-graph-card](https://github.com/kalkih/mini-graph-card)
>* [vacuum-card](https://github.com/denysdovhan/vacuum-card)
>* [xiomi-vacuum-card](https://github.com/PiotrMachowski/lovelace-xiaomi-vacuum-map-card)
>* [frigate-card](https://github.com/dermotduffy/frigate-hass-card)

### 2. you need to enable these sensors in the companion app:
>* Detectd Activity
>* Battery sensors
>* Geocoded Location
>* Last Reboot
>* Location sensors
>* Network sensors

### 3. you need to create Area's for your rooms in:

>Settings > Areas & Zones > + CREATE AREA
>
>also create an Area called: Media (we need it later for media count)

### 4. you need to create this virtual/fake button:
>this button (input_boolean) is needed to give some mushroom cards (that we need for navigation only) some colors to create it:
>
>Settings > Devices & Services > Helpers > + CREATE HELPER > Toggle
>
> name it: on state
>
> so this button entity id would be: `input_boolean.on_state`

### 5. you need to have/create **groups** and lights count **sensors**:
> - Note: make more groups & sensors for every user & room you want.
> - Restart is required after you create the sensors.
> - use your own entities!
>
>Groups examples: (Click to expand)
>
><details>
>  <summary>Livingroom group</summary>
>  
>* paste this code to your **groups.yaml**
>  
>  ```
>  livingroom_lights:
>    name: Livingroom lights
>    entities:
>    - light.livingroom_lamp
>    - light.livingroom_led
>    - light.livingroom_neon
>    - light.livingroom_ambient
>  ```
>  
></details>
>
><details>
>  <summary>Office group</summary>
>  
>* paste this code to your **groups.yaml**
>  
>  ```
>  office_lights:
>    name: office lights
>    entities:
>    - light.office_lamp
>    - light.office_led
>    - light.office_desk
>  ```
>  
></details>
>
><details>
>  <summary>All lights group</summary>
>  
>* paste this code to your **groups.yaml**
>  
>  ```
>  all_lights:
>    name: All lights
>    entities:
>    - light.livingroom_lamp
>    - light.livingroom_led
>    - light.livingroom_neon
>    - light.livingroom_ambient
>    - light.office_lamp
>    - light.office_led
>    - light.office_desk
>  ```
>  
></details>
>
><details>
>  <summary>All media group</summary>
>  
>* paste this code to your **groups.yaml**
>  
>  ```
>  all_media:
>    name: All media
>    entities:
>    - media_player.livingroom_speaker
>    - media_player.bedroom_speaker
>    - media_player.samsung_tv
>  ```
>  
></details>
>
><details>
>  <summary>All devices group</summary>
>  
>* paste this code to your **groups.yaml**
>  
>  ```
>  all_devices:
>    name: All devices 
>    entities:
>    - light.livingroom_lamp
>    - light.livingroom_led
>    - light.livingroom_neon
>    - light.livingroom_ambient
>    - light.office_lamp
>    - light.office_led
>    - light.office_desk
>    - media_player.livingroom_speaker
>    - media_player.bedroom_speaker
>    - media_player.samsung_tv
>  ```
>  
></details>
>
><details>
>  <summary>All plugs group</summary>
>  
>* paste this code to your **groups.yaml**
>  
>  ```
>  all_plugs:
>    name: All plugs
>    entities:
>    - switch.plug_1
>    - switch.plug_2
>    - switch.plug_3
>  ```
>
></details>
>
><br>
>
>Lighs/Media/Devices counters examples: (Click to expand)
>
><details>
>  <summary>Livingroom Lights count</summary>
>  
>* paste this code to your **sensors.yaml**
>* replace livingroom in line 7 with your own Area name, (upper/lower case sensitive).
>  
>  ```
>    - platform: template
>      sensors:
>        livingroom_lights_of_count:
>          friendly_name: 'livingroom lights of count'
>          unique_id: livingroom_lights_of_count
>          value_template: >
>            {{ expand(area_entities('Livingroom'))
>              | selectattr('domain', 'in', ['light'])
>              | selectattr('state', 'eq', 'on')
>              | rejectattr('object_id', 'search', 'segment')
>              | list | count
>            }}
>  ```
>  
></details>
>
><details>
>  <summary>Office Lights count</summary>
>  
>* paste this code to your **sensors.yaml**
>* replace livingroom in line 7 with your own Area name, (upper/lower case sensitive).
>  
>  ```
>    - platform: template
>      sensors:
>        office_lights_of_count:
>          friendly_name: 'Office lights of count'
>          unique_id: office_lights_of_count
>          value_template: >
>            {{ expand(area_entities('Office'))
>              | selectattr('domain', 'in', ['light'])
>              | selectattr('state', 'eq', 'on')
>              | rejectattr('object_id', 'search', 'segment')
>              | list | count
>            }}
>  ```
>  
></details>
>
><details>
>  <summary>Media count</summary>
>  
>* paste this code to your **sensors.yaml**
>  
>  ```
>    - platform: template
>      sensors:
>        media_of_count:
>          friendly_name: 'Media of count'
>          unique_id: media_of_count
>          value_template: >
>            {{ expand(area_entities('Media'))
>              | selectattr('domain', 'in', ['media_player'])
>              | selectattr('state', 'in', ['on', 'idle'])
>              | rejectattr('object_id', 'search', 'segment')
>              | list | count
>            }}
>  ```
>  
></details>
>
><details>
>  <summary>All Lights count</summary>
>  
>* paste this code to your **sensors.yaml**
>* replace Area's names in line 8 with your own, (upper/lower case sensitive).
>
>  ```
>    - platform: template
>      sensors:
>        lights_of_count:
>          friendly_name: 'Lights of count'
>          unique_id: lights_of_count
>          value_template: > 
>            {%- set search_state = 'on' %}
>            {%- set search_areas = ['Bedroom','Livingroom','Kitchen','Hall','Bath','Inner Hall','WC','Office'] %}
>            {%- set ns = namespace(lights=[]) %}
>            {%- for light in states.light 
>            | selectattr('state','eq', search_state)
>            | rejectattr('object_id', 'search', 'segment')%}
>              {%- for area in search_areas %}
>                {% if area_name(light.entity_id) == area %}
>                  {%- set ns.lights = ns.lights + [ light.entity_id ] %}
>                {% endif%}
>                {%- endfor %}
>            {%- endfor %}
>            {{ ns.lights| list | length }}
>  ```
></details>
>
> **_(Note: make more groups & sensors for every user & room you want)_**

### 6. you need to create these additional **sensors**:
(Restart is required after you create the sensors)
>
><details>
>  <summary>Battery icon & color based of phone battery level</summary>
>  
>* paste this code to your **sensors.yaml** (replace the word *user* with your own name or family members names)
>  
>  ```
>    - platform: template
>      sensors:
>        user_battery_icon:
>          friendly_name: 'user battery icon'
>          value_template: >
>              {% set state = states('sensor.user_battery_level')|float %}
>              {% if state >= 0 and state < 10 %} mdi:battery-10
>              {% elif state >= 10 and state < 20 %} mdi:battery-20
>              {% elif state >= 20 and state < 30 %} mdi:battery-30
>              {% elif state >= 30 and state < 40 %} mdi:battery-40
>              {% elif state >= 40 and state < 50 %} mdi:battery-50
>              {% elif state >= 50 and state < 60 %} mdi:battery-60
>              {% elif state >= 60 and state < 70 %} mdi:battery-70
>              {% elif state >= 70 and state < 80 %} mdi:battery-80
>              {% elif state >= 80 and state < 95 %} mdi:battery-90
>              {% else %} mdi:battery
>              {% endif %}
>    - platform: template
>      sensors:
>        user_battery_color:
>          friendly_name: 'user battery color'
>          value_template: >
>              {% set state = states('sensor.user_battery_level')|float %}
>              {% if state >= 0 and state < 20 %} red
>              {% elif state >= 20 and state < 50 %} orange
>              {% elif state >= 50 and state < 80 %} yellow
>              {% else %} green
>              {% endif %}
>  ```
>  
></details>
>
><details>
>  <summary> (for header) Temperature icon color based of temperature</summary>
>  
>* paste this code to your **sensors.yaml** 
>
>  ```
>    - platform: template
>      sensors:
>        livingroom_temp_color_no_rgb:
>          friendly_name: 'livingroom temp color no rgb'
>          value_template: >
>              {% set state = states('sensor.livingroom_temperature')|float %}
>                {% if state >= 0 and state < 18 %} blue
>                {% elif state >= 18 and state < 20 %} yellow
>                {% elif state >= 20 and state < 23 %} orange
>                {% elif state >= 23 and state < 50 %} red
>                {% else %} grey
>                {% endif %}
>  ```
>  
></details>
>
><details>
>  <summary> (for rooms cards) Temperature icon color based of temperature</summary>
>  
>* paste this code to your **sensors.yaml**
>  
>  ```       
>    - platform: template
>      sensors:
>        livingroom_temp_color:
>          friendly_name: 'livingroom temp color'
>          value_template: >
>              {% set state = states('sensor.livingroom_temperature')|float %}
>                {% if state >= 0 and state < 18 %} rgb(26, 209, 255)
>                {% elif state >= 18 and state < 20 %} rgb(255, 214, 51)
>                {% elif state >= 20 and state < 23 %} rgb(255, 163, 26)
>                {% elif state >= 23 and state < 50 %} rgb(255, 51, 51)
>                {% else %} grey
>                {% endif %}
>  ```
>  
></details>
>
><details>
>  <summary>(for person cards) This shows the user address nicley in the card</summary>
>  
>* paste this code to your **sensors.yaml** (replace the word *user* with your own name or family members names)
>  
>  ```
>    - platform: template
>      sensors:
>        user_geo_1:
>          friendly_name: 'user geo 1'
>          value_template: >
>              {{ (states('sensor.user_geocoded_location')).split(",")[0] }}
>    - platform: template
>      sensors:
>        user_geo_2:
>          friendly_name: 'user geo 2'
>          value_template: >
>              {{ (states('sensor.user_geocoded_location')).split(",")[1] }}
>  ```
>  
></details>
>
><details>
>  <summary>(for person cards) needed for Proximity (converting to KM and math stuff)</summary>
>  
>* paste this code to your **sensors.yaml** (replace the word *user* with your own name or family members names)
>  
>  ```
>    - platform: template
>      sensors:
>        user_proximity:
>          friendly_name: 'user proximity'
>          value_template: >
>              {% set state = states('proximity.user')|float %}
>              {% if state >= 0 and state < 50 %} {{ (states('proximity.user')| int * 0)}} km
>              {% elif state >= 50 and state < 999 %} {{ (states('proximity.user')| int * 0.001) | round(1)}} km
>              {% elif state > 999 and state <= 99999 %} {{ (states('proximity.user')| int * 0.001) | round(1)}} km
>              {% elif state > 99999 %} {{ (states('proximity.user')| int * 0.001) | round(0)}} km
>              {% endif %}
>  ```
>  
></details>
>
><details>
>  <summary>(for person cards) Proximity (how far from home is a family member)</summary>
>
>* add this to your configuration.yaml:
>  `proximity: !include proximity.yaml`
>* create a proximity.yaml in your config/ and paste this code in it (replace the word *user* with your own name or family members names)
> this calculates how far from home is a family member in KM
>  
>  ```
>user:
>    zone: home
>    devices:
>      - device_tracker.user
>    tolerance: 50
>    unit_of_measurement: m
>  ```
>  
></details>

### 7. laptop/PC states/actions:
>To get the state of the laptop/PC like in the office room,
>i've used [HASS.Agent](https://github.com/LAB02-Research/HASS.Agent).

# Setup Dashboard :

### Header card:
<img src="https://github.com/Anashost/MY-HA-DASH/assets/41167157/bd2b3456-237c-4628-8b70-2a2b76ff5a97" width=60% height=60%>

- Go to your dashboard add card then choose **Manual**.
- copy/paste the code below.
- in this card use the (for header) sensor from step .6 to avoid errors.
- replace entities like: lights count, groups, temperature sensors, nav bath, etc.. with your own entities.

<details>
  <summary>Yaml Code (Click to expand)</summary>

```
type: custom:stack-in-card
mode: vertical
keep:
  outer_padding: false
  margin: true
cards:
  - type: custom:mushroom-chips-card
    chips:
      - type: template
        entity: input_boolean.on_state
        double_tap_action:
          action: none
        icon: mdi:pine-tree
        content: ''
        icon_color: green
        hold_action:
          action: none
        tap_action:
          action: navigate
          navigation_path: weather
      - type: weather
        entity: weather.forecast_home
        double_tap_action:
          action: none
        show_conditions: true
        show_temperature: true
        hold_action:
          action: none
      - type: entity
        entity: sensor.openweathermap_wind_speed
        icon_color: blue-grey
        icon: mdi:weather-windy
        double_tap_action:
          action: none
        hold_action:
          action: none
    alignment: center
  - type: custom:mushroom-chips-card
    chips:
      - type: template
        double_tap_action:
          action: none
        icon: mdi:home
        icon_color: |-
          {% if is_state('group.all_devices','on')%}
           amber
          {%endif%}
          {% if is_state('group.all_devices','off')%}
           grey
          {%endif%}
        hold_action:
          action: more-info
        tap_action:
          action: none
        entity: group.all_devices
      - type: template
        entity: sensor.livingroom_temperature
        icon_color: '{{states(''sensor.livingroom_temp_color_no_rgb'')}}'
        icon: mdi:thermometer
        use_entity_picture: true
        double_tap_action:
          action: none
        hold_action:
          action: none
        tap_action:
          action: more-info
        content: |-
          {% if is_state('sensor.livingroom_temperature','unavailable') %}
            -
          {% elif is_state('sensor.livingroom_temperature','unknown') %}
            -
          {% else %}
            {{states('sensor.livingroom_temperature')}}°
          {% endif %}
      - type: template
        entity: sensor.livingroom_humidity
        icon_color: light-blue
        icon: mdi:water-percent
        name: ''
        use_entity_picture: true
        hold_action:
          action: none
        double_tap_action:
          action: none
        tap_action:
          action: more-info
        content: |-
          {% if is_state('sensor.livingroom_humidity','unavailable') %}
            -
          {% else %}
            {{states('sensor.livingroom_humidity')}}%
          {% endif %}
      - type: template
        double_tap_action:
          action: none
        icon: mdi:lightbulb-group
        content: |-
          {% if is_state('sensor.lights_of_count','0') %}
            
          {% else %}
            {{states('sensor.lights_of_count')}}
          {% endif %}
        icon_color: |-
          {% set state = states('sensor.lights_of_count')|float %}
          {% if state <= 0 %} grey
          {% else %} amber
          {% endif %}
        hold_action:
          action: none
        tap_action:
          action: none
        entity: sensor.lights_of_count
      - type: template
        double_tap_action:
          action: none
        icon: mdi:play
        content: |-
          {% if is_state('sensor.media_of_count','0') %}
            
          {% else %}
            {{states('sensor.media_of_count')}}
          {% endif %}
        icon_color: |-
          {% set state = states('sensor.media_of_count')|float %}
          {% if state <= 0 %} grey
          {% else %} red
          {% endif %}
        hold_action:
          action: none
        tap_action:
          action: none
        entity: sensor.media_of_count
    alignment: center
  - type: custom:swipe-card
    mode: vertical
    keep:
      outer_padding: false
      margin: true
      box_shadow: false
      background: false
    cards:
      - type: horizontal-stack
        cards:
          - type: custom:mushroom-entity-card
            card_mod:
              style: |
                ha-card{
                 box-shadow: none;
                 }
            entity: input_boolean.on_state
            double_tap_action:
              action: none
            icon: mdi:remote
            icon_color: teal
            layout: vertical
            hold_action:
              action: none
            tap_action:
              action: navigate
              navigation_path: remote
            primary_info: none
            secondary_info: name
            name: TV
          - type: custom:mushroom-entity-card
            card_mod:
              style: |
                ha-card{
                 box-shadow: none;
                 }
            entity: group.all_lights
            icon_color: yellow
            secondary_info: name
            layout: vertical
            name: Lights
            double_tap_action:
              action: none
            hold_action:
              action: none
            tap_action:
              action: more-info
            primary_info: none
            icon: mdi:lamps
          - type: custom:mushroom-entity-card
            card_mod:
              style: |
                ha-card{
                 box-shadow: none;
                 }
            entity: input_boolean.on_state
            icon: mdi:floor-plan
            icon_color: white
            layout: vertical
            primary_info: none
            secondary_info: name
            tap_action:
              action: navigate
              navigation_path: floor
            hold_action:
              action: none
            double_tap_action:
              action: none
            name: Floor
          - type: custom:mushroom-entity-card
            card_mod:
              style: |
                ha-card{
                 box-shadow: none;
                 }
            entity: input_boolean.on_state
            name: Media
            icon: mdi:television-play
            icon_color: red
            layout: vertical
            double_tap_action:
              action: none
            hold_action:
              action: none
            tap_action:
              action: navigate
              navigation_path: media
            secondary_info: name
            primary_info: none
          - type: custom:mushroom-entity-card
            card_mod:
              style: |
                ha-card{
                 box-shadow: none;
                 }
            entity: input_boolean.on_state
            name: Clean
            icon: mdi:vacuum
            icon_color: cyan
            layout: vertical
            double_tap_action:
              action: none
            hold_action:
              action: none
            tap_action:
              action: navigate
              navigation_path: vacuum
            secondary_info: name
            primary_info: none
          - type: custom:mushroom-entity-card
            card_mod:
              style: |
                ha-card{
                 box-shadow: none;
                 }
            entity: group.all_plugs
            name: Plugs
            icon: mdi:power-socket-eu
            icon_color: green
            layout: vertical
            double_tap_action:
              action: none
            hold_action:
              action: none
            tap_action:
              action: navigate
              navigation_path: plugs
            secondary_info: name
            primary_info: none
      - type: horizontal-stack
        cards:
          - type: custom:mushroom-entity-card
            card_mod:
              style: |
                ha-card{
                 box-shadow: none;
                 }
            entity: input_boolean.on_state
            double_tap_action:
              action: none
            icon: mdi:shield-home
            icon_color: teal
            layout: vertical
            hold_action:
              action: none
            tap_action:
              action: navigate
              navigation_path: arm
            primary_info: none
            secondary_info: name
            name: Alarm
          - type: custom:mushroom-entity-card
            card_mod:
              style: |
                ha-card{
                 box-shadow: none;
                 }
            entity: input_boolean.on_state
            double_tap_action:
              action: none
            icon: mdi:flask-outline
            icon_color: cyan
            layout: vertical
            hold_action:
              action: none
            tap_action:
              action: navigate
              navigation_path: lab
            primary_info: none
            secondary_info: name
            name: Lab
          - type: custom:mushroom-entity-card
            card_mod:
              style: |
                ha-card{
                 box-shadow: none;
                 }
            entity: input_boolean.on_state
            double_tap_action:
              action: none
            icon: mdi:gas-station
            icon_color: orange
            layout: vertical
            hold_action:
              action: none
            tap_action:
              action: navigate
              navigation_path: brandstof
            primary_info: none
            secondary_info: name
            name: Gas
          - type: custom:mushroom-entity-card
            card_mod:
              style: |
                ha-card{
                 box-shadow: none;
                 }
            entity: input_boolean.on_state
            double_tap_action:
              action: none
            icon: mdi:shopping
            icon_color: white
            layout: vertical
            hold_action:
              action: none
            tap_action:
              action: navigate
              navigation_path: shopping
            primary_info: none
            secondary_info: name
            name: List
          - type: custom:mushroom-entity-card
            card_mod:
              style: |
                ha-card{
                 box-shadow: none;
                 }
            entity: input_boolean.on_state
            double_tap_action:
              action: none
            icon: none
            icon_color: none
            layout: vertical
            hold_action:
              action: none
            tap_action:
              action: navigate
              navigation_path: Home
            primary_info: none
            secondary_info: none
            name: TV
          - type: custom:mushroom-entity-card
            card_mod:
              style: |
                ha-card{
                 box-shadow: none;
                 }
            entity: input_boolean.on_state
            double_tap_action:
              action: none
            icon: none
            icon_color: none
            layout: vertical
            hold_action:
              action: none
            tap_action:
              action: navigate
              navigation_path: Home
            primary_info: none
            secondary_info: none
            name: TV

```
</details>

*****

### person cards:
![person cards](https://github.com/Anashost/MY-HA-DASH/assets/41167157/a43b2841-f16d-4ceb-a700-660cfd00561f)

- Go to your dashboard add card then choose **horizontal-stack**.
- within the **horizontal-stack** add card with + and choose **Manual**
- copy/paste the code below.
- you can add a second user by clicking +
- Replace "user" with your own users names.

<details>
  <summary>Yaml Code (Click to expand)</summary>

```
type: custom:swipe-card
cards:
  - type: custom:stack-in-card
    cards:
      - type: custom:mushroom-person-card
        entity: person.user
        icon_type: entity-picture
        tap_action:
          action: more-info
        double_tap_action:
          action: none
        hold_action:
          action: none
        secondary_info: state
        layout: horizontal
        primary_info: none
        fill_container: true
      - type: custom:mushroom-chips-card
        card_mod:
          style: |
            ha-card {
              --chip-font-size: 0.3em;
              --chip-icon-size: 0.5em;
              --chip-border-width: 0;
              --chip-box-shadow: none;
              --chip-background: none;
              --chip-border: none;
              --chip-spacing: none;
              --chip-font-weight: bold;
            }
        chips:
          - type: template
            entity: sensor.user_battery_level
            icon: '{{states(''sensor.user_battery_icon'')}}'
            icon_color: '{{states(''sensor.user_battery_color'')}}'
            content: '{{states(''sensor.user_battery_level'')}}%'
            tap_action:
              action: more-info
          - type: template
            entity: sensor.user_battery_level
            icon: >-
              {% if
              is_state('binary_sensor.user_is_charging','on')%}mdi:power-plug {%
              else %} mdi:power-plug-off {% endif %}                  
            icon_color: |-
              {% if is_state('binary_sensor.user_is_charging','on')%}blue
              {% else %} grey
              {% endif %}
            content: ''
            tap_action:
              action: none
          - type: template
            entity: sensor.user_network_type
            icon: |-
              {% if is_state('sensor.user_network_type','wifi')%}
               mdi:wifi 
              {% elif is_state('sensor.user_network_type','cellular')%}
               mdi:signal-4g
              {% else %} mdi:network-strength-off
              {% endif %}  
            icon_color: |-
              {% if is_state('sensor.user_network_type','wifi')%}
               green
              {% elif is_state('sensor.user_network_type','cellular')%}
               red
              {% else %} grey
              {% endif %}   
            content: ''
            hold_action:
              action: none
            double_tap_action:
              action: none
            tap_action:
              action: none
        alignment: center
      - type: custom:mushroom-chips-card
        card_mod:
          style: |
            ha-card {
              --chip-font-size: 0.28em;
              --chip-icon-size: 0.4em;
              --chip-border-width: 0;
              --chip-box-shadow: none;
              --chip-background: none;
              --chip-border: none;
              --chip-font-weight: normal;
            }
        alignment: justify
        chips:
          - type: entity
            entity: device_tracker.user
            content_info: last-updated
            icon: mdi:clock
            icon_color: grey
            tap_action:
              action: more-info
          - type: entity
            entity: sensor.user_proximity
            content_info: state
            icon: mdi:map-marker-right-outline
            icon_color: grey
            tap_action:
              action: more-info
  - type: custom:stack-in-card
    mode: vertical
    keep:
      margin: true
      outer-padding: false
    cards:
      - type: custom:mushroom-template-card
        card_mod:
          style: |
            ha-card {
              --font-size: 0.32em;
              --icon-size: 0.52em;
              --border-width: 0;
              --box-shadow: none;
              --background: none;
              --border: none;
              --spacing: 0.5em;
              --font-weight: normal;
            }
        primary: ''
        secondary: 'WiFi: {{states(''sensor.user_wifi_connection'')}}'
        icon: ''
        entity: sensor.user_wifi_connection
        layout: vertical
        double_tap_action:
          action: none
        hold_action:
          action: none
        tap_action:
          action: more-info
      - type: custom:mushroom-chips-card
        card_mod:
          style: |
            ha-card {
              --chip-font-size: 0.32em;
              --chip-icon-size: 0.52em;
              --chip-border-width: 0;
              --chip-box-shadow: none;
              --chip-background: none;
              --chip-border: none;
              --chip-spacing: none;
              --chip-font-weight: normal;
            }
        chips:
          - type: entity
            entity: sensor.user_detected_activity
            icon_color: teal
            double_tap_action:
              action: none
            hold_action:
              action: none
            tap_action:
              action: more-info
          - type: entity
            entity: automation.a_on_state
            icon_color: red
            icon: mdi:cellphone-wireless
            use_entity_picture: false
            content_info: none
            tap_action:
              action: call-service
              service: script.user_find_phone
              data: {}
              target: {}
            hold_action:
              action: none
            double_tap_action:
              action: none
        alignment: center
      - type: custom:mushroom-entity-card
        card_mod:
          style: |
            ha-card {
              --font-size: 0.2em;
              --icon-size: 0.5em;
              --border-width: 0;
              --box-shadow: none;
              --background: none;
              --border: none;
              --spacing: none;
              --font-weight: normal;
            }
        entity: sensor.user_geo_1
        icon_type: none
        primary_info: ''
        name: 'Possible Location:'
        layout: vertical
        fill_container: true
        double_tap_action:
          action: none
        hold_action:
          action: none
        tap_action:
          action: more-info
      - type: custom:mushroom-entity-card
        card_mod:
          style: |
            ha-card {
              --font-size: 0.2em;
              --icon-size: 0.5em;
              --border-width: 0;
              --box-shadow: none;
              --background: none;
              --border: none;
              --spacing: none;
              --font-weight: normal;
            }
        entity: sensor.user_geo_2
        icon_type: none
        primary_info: ''
        name: 'Possible Location:'
        layout: vertical
        fill_container: true
        double_tap_action:
          action: none
        hold_action:
          action: none
        tap_action:
          action: more-info
  - type: custom:stack-in-card
    mode: vertical
    keep:
      margin: true
      outer-padding: true
    cards:
      - type: custom:mushroom-template-card
        card_mod:
          style: |
            ha-card {
              --font-size: 0.3em;
              --icon-size: 0.5em;
              --border-width: 0;
              --box-shadow: none;
              --background: none;
              --border: none;
              --spacing: 0.5em;
              --font-weight: normal;
            }
        primary: ''
        secondary: 'other info:'
        icon: ''
        entity: sensor.karam_wifi_connection
        layout: vertical
        double_tap_action:
          action: none
        hold_action:
          action: none
        tap_action:
          action: more-info
      - type: custom:mushroom-chips-card
        card_mod:
          style: |
            ha-card {
              --chip-font-size: 0.32em;
              --chip-icon-size: 0.52em;
              --chip-border-width: 0;
              --chip-box-shadow: none;
              --chip-background: none;
              --chip-border: none;
              --chip-spacing: none;
              --chip-font-weight: normal;
            }
        chips:
          - type: entity
            entity: sensor.user_public_ip_address
            icon_color: teal
            double_tap_action:
              action: none
            hold_action:
              action: none
            tap_action:
              action: more-info
        alignment: center
      - type: custom:mushroom-chips-card
        card_mod:
          style: |
            ha-card {
              --chip-font-size: 0.32em;
              --chip-icon-size: 0.52em;
              --chip-border-width: 0;
              --chip-box-shadow: none;
              --chip-background: none;
              --chip-border: none;
              --chip-spacing: none;
              --chip-font-weight: normal;
            }
        chips:
          - type: entity
            entity: sensor.user_last_reboot
            icon_color: teal
            double_tap_action:
              action: none
            hold_action:
              action: none
            tap_action:
              action: more-info
        alignment: center

```
</details>

*****

### Rooms cards:
- Go to your dashboard add card then choose **horizontal-stack**.
- Within the **horizontal-stack** add card with + and choose **Manual**
- copy/paste the code below.
- in this card use the (for rooms) sensor from step .6 to avoid errors.
- replace entities like: Lights, lights counters, groups, temperature sensors, etc.. with your own entities.
- most room cards have **light.empty** entity, leave it as is, as this let you have an empty row (in the room card quick toggle view).

#### Livingroom - Office
![living-office](https://github.com/Anashost/MY-HA-DASH/assets/41167157/4b70c121-1807-4fc7-9cbb-2c912748c00f)

<details>
  <summary>Livingroom card</summary>
  
```
type: custom:swipe-card
cards:
  - type: custom:stack-in-card
    card_mod:
      style: |
        ha-card {
          {% if is_state('group.livingroom_lights', 'on') %}
          background: rgba(255, 152, 0, 0.1);
          {% endif %}
         }
    mode: vertical
    keep:
      outer_padding: false
    cards:
      - type: custom:mushroom-template-card
        card_mod:
          style:
            mushroom-shape-icon$: |
              @keyframes ping {
                0% { box-shadow: 0 0 1px 1px rgba(var(--rgb-{{ config.icon_color }}), 0.7); }
                100% { box-shadow: 0 0 5px 15px transparent; }
              }
            .: |
              mushroom-shape-icon {
                --icon-size: 76px;
                display: flex;
                margin: -21px 0px 0px -21px !important;
              }
              ha-card {
                clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
              }
        primary: Living room
        secondary: ''
        icon: mdi:sofa
        layout: horizontal
        entity: group.livingroom_lights
        icon_color: |-
          {% if is_state('group.livingroom_lights','on')%}
           amber
          {%endif%}
        tap_action:
          action: navigate
          navigation_path: livingroom
        hold_action:
          action: toggle
        double_tap_action:
          action: none
        badge_color: ''
        badge_icon: ''
        fill_container: true
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            tap_action:
              action: none
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
            icon: mdi:lightbulb-group
            entity: group.livingroom_lights
            styles:
              icon:
                - width: 19px
              grid:
                - position: relative
              custom_fields:
                notification:
                  - border-radius: 50%
                  - position: absolute
                  - left: 50%
                  - top: 3%
                  - height: 20px
                  - width: 20px
                  - font-size: 10px
                  - line-height: 20px
                  - font-weight: bold
              card:
                - border-radius: 50%
                - width: 40px
                - height: 40px
            custom_fields:
              notification: |
                [[[
                   if (states['sensor.livingroom_lights_of_count'].state == '0')
                    return ' '
                   return `${states['sensor.livingroom_lights_of_count'].state}`
                ]]]
            name: ' '
          - type: custom:button-card
            state:
              - value: 'on'
            icon: mdi:thermometer
            entity: sensor.livingroom_temperature
            styles:
              icon:
                - color: |
                    [[[
                        return `${states['sensor.livingroom_temp_color'].state}`
                    ]]]
                - width: 15px
                - position: relative
                - top: 4px
                - right: 12px
              card:
                - border-radius: 80%
                - width: 40px
                - height: 40px
              name:
                - color: white
                - font-size: 10px
                - position: relative
                - bottom: 10px
                - left: 5px
                - font-weight: bold
            name: |
              [[[
                if (states['sensor.livingroom_temperature'].state == 'unknown')
                 return '-'
                return `${states['sensor.livingroom_temperature'].state}°`
              ]]]
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'off'
                color: grey
              - value: 'on'
                color: rgb(4,212,240)
                styles:
                  icon:
                    - animation: rotating 0.8s linear infinite
                  card:
                    - border-radius: 50%
            icon: mdi:fan
            tap_action:
              action: none
            entity: switch.plug_4_local
            styles:
              icon:
                - width: 19px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: rgb(255, 51, 51)
              - value: idle
                color: rgb(255, 51, 51)
                styles:
                  card:
                    - border-radius: 50%
            tap_action:
              action: none
            icon: mdi:youtube-tv
            entity: media_player.samsung_tv
            styles:
              icon:
                - width: 19px
              card:
                - width: 40px
                - height: 40px
            name: ' '
  - type: custom:stack-in-card
    mode: vertical
    cards:
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:chandelier
            tap_action:
              action: toggle
            entity: light.livingroom_lamp_local
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:chandelier
            tap_action:
              action: toggle
            entity: light.livingroom_socket_local
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:desk
            tap_action:
              action: toggle
            entity: light.desk_led
            styles:
              icon:
                - width: 24px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:television-ambient-light
            tap_action:
              action: toggle
            entity: light.behind_tv_led
            styles:
              icon:
                - width: 21px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:monitor
            tap_action:
              action: toggle
            entity: light.monitor_led
            styles:
              icon:
                - width: 21px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:led-strip
            tap_action:
              action: toggle
            entity: light.left_led
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:rectangle
            tap_action:
              action: toggle
            entity: light.under_tv_led
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:led-strip
            tap_action:
              action: toggle
            entity: light.right_led
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card

```
</details>

<details>
  <summary>Office card</summary>
  
```
type: custom:swipe-card
cards:
  - type: custom:stack-in-card
    card_mod:
      style: |
        ha-card {   

           {% if is_state('group.office_lights', 'on') %}
           background: rgba(255, 152, 0, 0.1);
           {% endif %}
          }
    mode: vertical
    keep:
      outer_padding: false
    cards:
      - type: custom:mushroom-template-card
        card_mod:
          style:
            mushroom-shape-icon$: |
              @keyframes ping {
                0% { box-shadow: 0 0 1px 1px rgba(var(--rgb-{{ config.icon_color }}), 0.7); }
                100% { box-shadow: 0 0 5px 15px transparent; }
              }
            .: |
              mushroom-shape-icon {
                --icon-size: 76px;
                display: flex;
                margin: -21px 0px 0px -21px !important;
              }
              ha-card {
                clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
              }
        primary: Office
        secondary: ''
        icon: mdi:desk
        layout: horizontal
        entity: group.office_lights
        icon_color: |-
          {% if is_state('group.office_lights','on')%}
           amber
          {%endif%}
        tap_action:
          action: navigate
          navigation_path: office
        hold_action:
          action: toggle
        double_tap_action:
          action: none
        badge_color: ''
        badge_icon: ''
        fill_container: true
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            tap_action:
              action: none
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
            icon: mdi:lightbulb-group
            entity: group.office_lights
            styles:
              icon:
                - width: 19px
              grid:
                - position: relative
              custom_fields:
                notification:
                  - border-radius: 50%
                  - position: absolute
                  - left: 50%
                  - top: 3%
                  - height: 20px
                  - width: 20px
                  - font-size: 10px
                  - line-height: 20px
                  - font-weight: bold
              card:
                - border-radius: 50%
                - width: 40px
                - height: 40px
            custom_fields:
              notification: |
                [[[
                   if (states['sensor.office_lights_of_count'].state == '0')
                    return ' '
                   return `${states['sensor.office_lights_of_count'].state}`
                ]]]
            name: ' '
          - type: custom:button-card
            state:
              - value: 'on'
            icon: mdi:thermometer
            entity: sensor.livingroom_temperature
            styles:
              icon:
                - color: |
                    [[[
                        return `${states['sensor.livingroom_temp_color'].state}`
                    ]]]
                - width: 15px
                - position: relative
                - top: 4px
                - right: 12px
              card:
                - border-radius: 80%
                - width: 40px
                - height: 40px
              name:
                - color: white
                - font-size: 10px
                - position: relative
                - bottom: 10px
                - left: 5px
                - font-weight: bold
            name: |
              [[[
                if (states['sensor.livingroom_temperature'].state == 'unknown')
                 return '-'
                return `${states['sensor.livingroom_temperature'].state}°`
              ]]]
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: Unlocked
                color: rgb(34,139,34)
              - value: Locked
                color: rgb(255, 71, 26)
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:laptop
            tap_action:
              action: more-info
            entity: sensor.laptop_sessionstate
            styles:
              icon:
                - width: 19px
              card:
                - width: 40px
                - height: 40px
            name: ' '
  - type: custom:stack-in-card
    mode: vertical
    cards:
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: none
            tap_action:
              action: none
            entity: light.empty
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:desk
            tap_action:
              action: toggle
            entity: light.desk_led
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:chandelier
            tap_action:
              action: toggle
            entity: light.livingroom_socket_local
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:monitor
            tap_action:
              action: toggle
            entity: light.monitor_led
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: none
            tap_action:
              action: none
            entity: light.empty
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '

```
</details>

*****

![bedroom-kitchen](https://github.com/Anashost/MY-HA-DASH/assets/41167157/3ad550e4-ac3f-428c-a54b-9fce6ce182e9)

<details>
  <summary>Bedroom card</summary>
  
```
type: custom:swipe-card
cards:
  - type: custom:stack-in-card
    card_mod:
      style: |
        ha-card {
           
           {% if is_state('group.bedroom_lights', 'on') %}
           background: rgba(255, 152, 0, 0.1);
           {% endif %}
          }
    mode: vertical
    keep:
      outer_padding: false
    cards:
      - type: custom:mushroom-template-card
        card_mod:
          style:
            mushroom-shape-icon$: |
              @keyframes ping {
                0% { box-shadow: 0 0 1px 1px rgba(var(--rgb-{{ config.icon_color }}), 0.7); }
                100% { box-shadow: 0 0 5px 15px transparent; }
              }
            .: |
              mushroom-shape-icon {
                --icon-size: 76px;
                display: flex;
                margin: -21px 0px 0px -21px !important;
              }
              ha-card {
                clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
              }
        primary: Bedroom
        secondary: ''
        icon: mdi:bunk-bed
        layout: horizontal
        entity: group.bedroom_lights
        icon_color: |-
          {% if is_state('group.bedroom_lights','on')%}
           amber
          {%endif%}
        tap_action:
          action: navigate
          navigation_path: bedroom
        hold_action:
          action: toggle
        double_tap_action:
          action: none
        badge_color: ''
        badge_icon: ''
        fill_container: true
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            tap_action:
              action: none
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
            icon: mdi:lightbulb-group
            entity: group.bedroom_lights
            styles:
              icon:
                - width: 19px
              grid:
                - position: relative
              custom_fields:
                notification:
                  - border-radius: 50%
                  - position: absolute
                  - left: 50%
                  - top: 3%
                  - height: 20px
                  - width: 20px
                  - font-size: 10px
                  - line-height: 20px
                  - font-weight: bold
              card:
                - border-radius: 50%
                - width: 40px
                - height: 40px
            custom_fields:
              notification: |
                [[[
                   if (states['sensor.bedroom_lights_of_count'].state == '0')
                    return ' '
                   return `${states['sensor.bedroom_lights_of_count'].state}`
                ]]]
            name: ' '
          - type: custom:button-card
            state:
              - value: 'on'
            icon: mdi:thermometer
            entity: sensor.mi_bedroom_temperature
            styles:
              icon:
                - color: |
                    [[[
                        return `${states['sensor.bedroom_temp_color'].state}`
                    ]]]
                - width: 15px
                - position: relative
                - top: 4px
                - right: 12px
              card:
                - border-radius: 80%
                - width: 40px
                - height: 40px
              name:
                - color: white
                - font-size: 10px
                - position: relative
                - bottom: 10px
                - left: 5px
                - font-weight: bold
            name: |
              [[[
                if (states['sensor.mi_bedroom_temperature'].state == 'unavailable')
                 return '-'
                return `${states['sensor.mi_bedroom_temperature'].state}°`
              ]]]
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'off'
                color: grey
              - value: 'on'
                color: rgb(4,212,240)
                styles:
                  icon:
                    - animation: rotating 0.8s linear infinite
                  card:
                    - border-radius: 50%
            icon: mdi:fan
            tap_action:
              action: none
            entity: switch.plug_3_local
            styles:
              icon:
                - width: 19px
              card:
                - width: 40px
                - height: 40px
            name: ' '
  - type: custom:stack-in-card
    mode: vertical
    cards:
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: none
            tap_action:
              action: none
            entity: light.wled_window
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:light-recessed
            tap_action:
              action: toggle
            entity: light.bedroom_lamp_local
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:table-furniture
            tap_action:
              action: toggle
            entity: light.table_led
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: none
            tap_action:
              action: none
            entity: light.wled_window
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '

```
</details>

<details>
  <summary>Kitchen card</summary>
  
```
type: custom:swipe-card
cards:
  - type: custom:stack-in-card
    card_mod:
      style: |
        ha-card {
            
            {% if is_state('group.kitchen_lights', 'on') %}
            background: rgba(255, 152, 0, 0.1);
            {% endif %}
          }
    mode: vertical
    keep:
      outer_padding: false
    cards:
      - type: custom:mushroom-template-card
        card_mod:
          style:
            mushroom-shape-icon$: |
              @keyframes ping {
                0% { box-shadow: 0 0 1px 1px rgba(var(--rgb-{{ config.icon_color }}), 0.7); }
                100% { box-shadow: 0 0 5px 15px transparent; }
              }
            .: |
              mushroom-shape-icon {
                --icon-size: 76px;
                display: flex;
                margin: -21px 0px 0px -21px !important;
              }
              ha-card {
                clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
              }
        primary: Kitchen
        secondary: ''
        icon: mdi:silverware-fork-knife
        layout: horizontal
        entity: group.kitchen_lights
        icon_color: |-
          {% if is_state('group.kitchen_lights','on')%}
          amber 
          {%endif%}
        tap_action:
          action: navigate
          navigation_path: kitchen
        hold_action:
          action: toggle
        double_tap_action:
          action: none
        badge_color: ''
        badge_icon: ''
        fill_container: true
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            tap_action:
              action: none
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
            icon: mdi:lightbulb-group
            entity: group.kitchen_lights
            styles:
              icon:
                - width: 19px
              grid:
                - position: relative
              custom_fields:
                notification:
                  - border-radius: 50%
                  - position: absolute
                  - left: 50%
                  - top: 3%
                  - height: 20px
                  - width: 20px
                  - font-size: 10px
                  - line-height: 20px
                  - font-weight: bold
              card:
                - border-radius: 50%
                - width: 40px
                - height: 40px
            custom_fields:
              notification: |
                [[[
                   if (states['sensor.kitchen_lights_of_count'].state == '0')
                    return ' '
                   return `${states['sensor.kitchen_lights_of_count'].state}`
                ]]]
            name: ' '
          - type: custom:button-card
            state:
              - value: 'on'
            icon: mdi:thermometer
            entity: sensor.livingroom_temperature
            styles:
              icon:
                - color: |
                    [[[
                        return `${states['sensor.livingroom_temp_color'].state}`
                    ]]]
                - width: 15px
                - position: relative
                - top: 4px
                - right: 12px
              card:
                - border-radius: 80%
                - width: 40px
                - height: 40px
              name:
                - color: white
                - font-size: 10px
                - position: relative
                - bottom: 10px
                - left: 5px
                - font-weight: bold
            name: |
              [[[
                if (states['sensor.livingroom_temperature'].state == 'unknown')
                 return '-'
                return `${states['sensor.livingroom_temperature'].state}°`
              ]]]
          - type: custom:button-card
            state:
              - operator: default
                color: yellow
              - value: 'off'
                color: grey
              - value: 'on'
                color: rgb(255, 71, 26)
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:motion-sensor
            tap_action:
              action: more-info
            entity: binary_sensor.pir_kitchen_occupancy
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '
  - type: custom:stack-in-card
    mode: vertical
    cards:
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: none
            tap_action:
              action: none
            entity: input_boolean.on_state
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:light-recessed
            tap_action:
              action: toggle
            entity: light.kitchen_lamp_local
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: none
            tap_action:
              action: none
            entity: input_boolean.on_state
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '

```
</details>

*****

![bath-wc](https://github.com/Anashost/MY-HA-DASH/assets/41167157/ddb09146-df43-4f8c-91f6-d77d3440154b)

<details>
  <summary>Bath card</summary>
  
```
type: custom:swipe-card
cards:
  - type: custom:stack-in-card
    card_mod:
      style: |
        ha-card {
            {% if is_state('group.bath_lights', 'on') %}
            background: rgba(255, 152, 0, 0.1);
            {% endif %}
          }
    mode: vertical
    keep:
      outer_padding: false
    cards:
      - type: custom:mushroom-template-card
        card_mod:
          style:
            mushroom-shape-icon$: |
              @keyframes ping {
                0% { box-shadow: 0 0 1px 1px rgba(var(--rgb-{{ config.icon_color }}), 0.7); }
                100% { box-shadow: 0 0 5px 15px transparent; }
              }
            .: |
              mushroom-shape-icon {
                --icon-size: 76px;
                display: flex;
                margin: -21px 0px 0px -21px !important;
              }
              ha-card {
                clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
              }
        primary: Bath
        icon: mdi:shower-head
        layout: horizontal
        entity: group.bath_lights
        icon_color: |-
          {% if is_state('group.bath_lights','on')%}
          amber 
          {%endif%}
        tap_action:
          action: navigate
          navigation_path: bath
        hold_action:
          action: toggle
        double_tap_action:
          action: none
        badge_color: ''
        badge_icon: ''
        fill_container: true
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            tap_action:
              action: none
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
            icon: mdi:lightbulb-group
            entity: group.bath_lights
            styles:
              icon:
                - width: 19px
              grid:
                - position: relative
              custom_fields:
                notification:
                  - border-radius: 50%
                  - position: absolute
                  - left: 50%
                  - top: 3%
                  - height: 20px
                  - width: 20px
                  - font-size: 10px
                  - line-height: 20px
                  - font-weight: bold
              card:
                - border-radius: 50%
                - width: 40px
                - height: 40px
            custom_fields:
              notification: |
                [[[
                   if (states['sensor.bath_lights_of_count'].state == '0')
                    return ' '
                   return `${states['sensor.bath_lights_of_count'].state}`
                ]]]
            name: ' '
          - type: custom:button-card
            state:
              - operator: default
                color: yellow
              - value: 'off'
                color: grey
              - value: 'on'
                color: rgb(255, 71, 26)
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:motion-sensor
            tap_action:
              action: more-info
            entity: binary_sensor.pir_bath_occupancy
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '
  - type: custom:stack-in-card
    mode: vertical
    cards:
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: none
            tap_action:
              action: none
            entity: light.wled_window
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:dome-light
            tap_action:
              action: toggle
            entity: light.bath_lamp_local
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: none
            tap_action:
              action: none
            entity: light.wled_window
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '

```
</details>

<details>
  <summary>WC card</summary>
  
```
type: custom:swipe-card
cards:
  - type: custom:stack-in-card
    card_mod:
      style: |
        ha-card {
            {% if is_state('group.wc_lights', 'on') %}
            background: rgba(255, 152, 0, 0.1);
            {% endif %}
          }
    mode: vertical
    keep:
      outer_padding: false
    cards:
      - type: custom:mushroom-template-card
        card_mod:
          style:
            mushroom-shape-icon$: |
              @keyframes ping {
                0% { box-shadow: 0 0 1px 1px rgba(var(--rgb-{{ config.icon_color }}), 0.7); }
                100% { box-shadow: 0 0 5px 15px transparent; }
              }
            .: |
              mushroom-shape-icon {
                --icon-size: 76px;
                display: flex;
                margin: -21px 0px 0px -21px !important;
              }
              ha-card {
                clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
              }
        primary: WC
        icon: mdi:toilet
        layout: horizontal
        entity: group.wc_lights
        icon_color: |-
          {% if is_state('group.wc_lights','on')%}
          amber 
          {%endif%}
        tap_action:
          action: navigate
          navigation_path: toilet
        hold_action:
          action: toggle
        double_tap_action:
          action: none
        badge_color: ''
        badge_icon: ''
        fill_container: true
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            tap_action:
              action: none
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
            icon: mdi:lightbulb-group
            entity: group.wc_lights
            styles:
              icon:
                - width: 19px
              grid:
                - position: relative
              custom_fields:
                notification:
                  - border-radius: 50%
                  - position: absolute
                  - left: 50%
                  - top: 3%
                  - height: 20px
                  - width: 20px
                  - font-size: 10px
                  - line-height: 20px
                  - font-weight: bold
              card:
                - border-radius: 50%
                - width: 40px
                - height: 40px
            custom_fields:
              notification: |
                [[[
                   if (states['sensor.wc_lights_of_count'].state == '0')
                    return ' '
                   return `${states['sensor.wc_lights_of_count'].state}`
                ]]]
            name: ' '
          - type: custom:button-card
            state:
              - operator: default
                color: yellow
              - value: 'off'
                color: grey
              - value: 'on'
                color: rgb(255, 71, 26)
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:motion-sensor
            tap_action:
              action: more-info
            entity: binary_sensor.pir_wc_occupancy
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '
  - type: custom:stack-in-card
    mode: vertical
    cards:
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: none
            tap_action:
              action: none
            entity: light.empty
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:light-recessed
            tap_action:
              action: toggle
            entity: light.wc_lamp
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: none
            tap_action:
              action: none
            entity: light.empty
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '

```
</details>

*****

![security-hall](https://github.com/Anashost/MY-HA-DASH/assets/41167157/f766ca03-efda-44d4-94dc-04c4df1b49b0)

<details>
  <summary>Security card</summary>
  
```
type: custom:stack-in-card
card_mod:
  style: |
    ha-card {
        {% if is_state('camera.tapo_cam_sd_stream', 'idle') %}
         background: rgba(91, 215, 91,0.1);
        {% endif %}
      }
mode: vertical
cards:
  - type: custom:mushroom-template-card
    card_mod:
      style:
        mushroom-shape-icon$: ''
        .: |
          mushroom-shape-icon {
            --icon-size: 76px;
            display: flex;
            margin: -21px 0px 0px -21px !important;
          }
          ha-card {
            clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
          }
    primary: Security
    secondary: ''
    icon: mdi:cctv
    layout: horizontal
    entity: switch.tapo_cam_privacy
    icon_color: green
    badge_color: ''
    badge_icon: ''
    multiline_secondary: false
    fill_container: false
    double_tap_action:
      action: none
    tap_action:
      action: navigate
      navigation_path: security
    hold_action:
      action: none
  - type: horizontal-stack
    cards:
      - type: custom:button-card
        state:
          - operator: default
            color: grey
          - value: 'on'
            color: orange
          - value: 'off'
            color: grey
            styles:
              card:
                - border-radius: 50%
        icon: none
        tap_action:
          action: none
        entity: light.empty
        styles:
          icon:
            - width: 18px
          card:
            - width: 40px
            - height: 40px
        name: ' '


```
</details>

<details>
  <summary>Hall card</summary>
  
```
type: custom:swipe-card
cards:
  - type: custom:stack-in-card
    card_mod:
      style: |
        ha-card {
            {% if is_state('group.hall_lights', 'on') %}
            background: rgba(255, 152, 0, 0.1);
            {% endif %}
          }
    mode: vertical
    keep:
      outer_padding: false
    cards:
      - type: custom:mushroom-template-card
        card_mod:
          style:
            mushroom-shape-icon$: |
              @keyframes ping {
                0% { box-shadow: 0 0 1px 1px rgba(var(--rgb-{{ config.icon_color }}), 0.7); }
                100% { box-shadow: 0 0 5px 15px transparent; }
              }
            .: |
              mushroom-shape-icon {
                --icon-size: 76px;
                display: flex;
                margin: -20px 0px 0px -20px !important;
              }
              ha-card {
                clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
              }
        primary: Hall
        icon: mdi:door
        layout: horizontal
        entity: group.hall_lights
        icon_color: |-
          {% if is_state('group.hall_lights','on')%}
          amber 
          {%endif%}
        tap_action:
          action: navigate
          navigation_path: hall
        hold_action:
          action: toggle
        double_tap_action:
          action: none
        badge_color: ''
        badge_icon: ''
        fill_container: true
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            tap_action:
              action: none
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
            icon: mdi:lightbulb-group
            entity: group.hall_lights
            styles:
              icon:
                - width: 19px
              grid:
                - position: relative
              custom_fields:
                notification:
                  - border-radius: 50%
                  - position: absolute
                  - left: 50%
                  - top: 3%
                  - height: 20px
                  - width: 20px
                  - font-size: 10px
                  - line-height: 20px
                  - font-weight: bold
              card:
                - border-radius: 50%
                - width: 40px
                - height: 40px
            custom_fields:
              notification: |
                [[[
                   if (states['sensor.hall_lights_of_count'].state == '0')
                    return ' '
                   return `${states['sensor.hall_lights_of_count'].state}`
                ]]]
            name: ' '
          - type: custom:button-card
            state:
              - operator: default
                color: yellow
              - value: 'off'
                color: grey
              - value: 'on'
                color: rgb(255, 71, 26)
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:motion-sensor
            tap_action:
              action: more-info
            entity: binary_sensor.pir_hall_occupancy
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '
  - type: custom:stack-in-card
    mode: vertical
    cards:
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: none
            tap_action:
              action: none
            entity: light.empty
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            color_type: blank-card
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: mdi:light-flood-down
            tap_action:
              action: toggle
            entity: light.hall_lamp_local
            styles:
              icon:
                - width: 23px
              card:
                - width: 40px
                - height: 40px
            name: ' '
          - type: custom:button-card
            color_type: blank-card
      - type: horizontal-stack
        cards:
          - type: custom:button-card
            state:
              - operator: default
                color: grey
              - value: 'on'
                color: orange
              - value: 'off'
                color: grey
                styles:
                  card:
                    - border-radius: 50%
            icon: none
            tap_action:
              action: none
            entity: light.empty
            styles:
              icon:
                - width: 18px
              card:
                - width: 40px
                - height: 40px
            name: ' '

```
</details>

# Enjoy

[latest_release]: https://github.com/Anashost/MY-HA-DASH/releases/latest

[releases_shield]: https://img.shields.io/github/release/Anashost/MY-HA-DASH.svg?style=popout

[releases]: https://github.com/Anashost/MY-HA-DASH/releases

[downloads_total_shield]: https://img.shields.io/github/downloads/Anashost/MY-HA-DASH/total

[community_forum_shield]: https://img.shields.io/static/v1.svg?label=%20&message=Forum&style=popout&color=41bdf5&logo=HomeAssistant&logoColor=white

[community_forum]: https://github.com/Anashost/MY-HA-DASH/issues

[paypal_me_shield]: https://img.shields.io/static/v1.svg?label=%20&message=PayPal&logo=paypal

[paypal_me]: https://www.paypal.me/anashost

[revolut_me_shield]: https://img.shields.io/static/v1.svg?label=%20&message=Revolut&logo=revolut

[revolut_me]: https://revolut.me/anas4e

[ko_fi_shield]: https://img.shields.io/static/v1.svg?label=%20&message=ByMeCoffee&logo=ko-fi

[ko_fi_me]: https://ko-fi.com/anasbox
