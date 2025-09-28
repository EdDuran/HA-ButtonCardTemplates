Templates created with the [custom:button-card](https://github.com/custom-cards/button-card) for [Home Assistant](https://www.home-assistant.io/) 

# Prerequisites
- Use the HACS tool to install the custom:button-card in your Home Assistant system.
  - Reboot your Home Assistant system to complete the button-card installation

<details>
<summary>hml_light_template</summary>

# hml_light_template
The **hml_light_template** provides the user interface to control a Light for settings: off, high, medium, low. It is built with the custom:button-card (provides templating) and contains a [custom:bubble-card](https://github.com/Clooos/Bubble-Card) for the user experience. The bubble card sets the input_select Helper Entity state, and the python script **hml_lights.py* script adjusts the brightness of the assocate Light Entity.

## (1) Install hml_lights.py
- Go to [hml_lights](https://github.com/EdDuran/HA-pyscript-hml-lights) and follow the instructions.

## (2) Create an HML Entity
- Create an input_select Helper with the values: off, low, medium, high
  - For example: ***input_select.master_bedroom_hml***
- Update the ***hml_lights.py*** script's configuration file
- Reload with: Developer Tools -> Action: pyscript:load_hml_light_config  

Example config file:
```
hml_data:
  input_select.master_bedroom_hml
    - light.master_nightstand_left

light_data:
  light.master_nightstand_left:
    'off': 0
    low: 5
    medium: 40
    high: 100
```

## (3) Edit Raw Dashboard
- Open your Home Assistant Dashboard, click the edit icon, and open the ***Raw configuration editor***.
- Add the **button_card_templates:** section (if it doesn't already exist)
- Add the **hml_light_template** under button_card_templates
```
button_card_templates:
  hml_light_template:
    variables:
      var_name: ''
      var_hml_entity: ''
    triggers_update:
      - '[[[ return variables.var_hml_entity; ]]]'
  :
  :
```
## (4) Add a Card to your Dashboard
- Open your dashboard, click the edit icon, and click ***Add Card*** button
- Click on the ***Button Card***
- Edit the card's yaml to use the hml_light_template
```
type: custom:button-card
template: hml_light_template
variables:
  var_name: Master
  var_hml_entity: input_select.master_bedroom_hml
```

## Flow
- Click Low, Medium, High ... or bubble background to turn off
  - The bubble card modifies the HML Entity state value
  - The bubble card updates visually
- The PyScript ***hml_lights.py*** is triggered by HML Entity the State Change
  - Validates the trigger is for an HML Entity and it is in the ***hml_data:***
  - Then, sets the Light Entity's brightness according to the ***light_data:*** values


![example](https://github.com/EdDuran/HA-ButtonCardTemplates/blob/main/HML%20Bubbles.jpg?raw=true)
</details>
