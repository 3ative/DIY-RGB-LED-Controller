# DIY RGB LED Controller
ESPHome Code for LED Controller

* Watch the full tutorial here: https://youtu.be/wSL9Q0Xhbj8

ESPHome Code:
```yaml
esphome:
  name: feature_light

esp8266:
  board: esp01_1m

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

logger:
api:
ota:
  platform: esphome

binary_sensor:
  - platform: gpio
    pin:
      number: GPIO0
      mode: INPUT_PULLUP
      inverted: True
    name: "Feature Light Button"
    internal: true
    on_press:
      - switch.toggle: feature_light_relay
  - platform: template
    id: turn_switch_if_light
    lambda: !lambda |-
      return id(feature_light_rgb).current_values.is_on();
    on_press:
      then:
        - switch.turn_on: feature_light_relay
    on_release:
      then:
        - switch.turn_off: feature_light_relay

switch:
  - platform: gpio
    pin: GPIO12
    name: "Feature Light Relay"
    id: feature_light_relay
    internal: true

status_led:
  pin:
    number: GPIO13
    inverted: yes

light:
  - platform: rgb
    name: "Feature Light"
    id: feature_light_rgb
    red: red
    green: green
    blue: blue
    effects:
      - lambda:
          name: Randon Colours
          update_interval: .5s
          lambda: |-
            auto call = id(feature_light_rgb).turn_on();
            call.set_transition_length(500);
            call.set_rgb(random(10)*.1, random(10)*.1, random(10)*.1);
            call.perform();
      - lambda:
          name: Randon Colours Slow
          update_interval: 1.5s
          lambda: |-
            auto call = id(feature_light_rgb).turn_on();
            call.set_transition_length(1000);
            call.set_rgb(random(10)*.1, random(10)*.1, random(10)*.1);
            call.perform();

output:
  - platform: esp8266_pwm
    id: red
    pin: GPIO1
  - platform: esp8266_pwm
    id: green
    pin: GPIO3
  - platform: esp8266_pwm
    id: blue
    pin: GPIO2
```

---
### 🤝 Found this useful, want to say 'Thanks' and support my efforts. CHEERS🍺
| Buy me a Coffee | PATREON |
|-----------------|---------|
| [![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-donate-yellow.svg?style=flat-square&logo=buy-me-a-coffee)](https://www.buymeacoffee.com/3ative) | [![Patreon](https://img.shields.io/badge/Patreon-support-red.svg?style=flat-square&logo=patreon)](https://www.patreon.com/3ative) |
---
