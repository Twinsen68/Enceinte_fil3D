binary_sensor:
  - platform: gpio
    id: bouton_mode
    pin:
      number: GPIO27
      mode: INPUT_PULLUP
      inverted: true
    on_press:
      then:
        - lambda: |-
            if (id(mode_fonctionnement).state == "Off") {
              id(mode_fonctionnement).publish_state("Test");
            } else if (id(mode_fonctionnement).state == "Test") {
              id(mode_fonctionnement).publish_state("Maintien");
            } else if (id(mode_fonctionnement).state == "Maintien") {
              id(mode_fonctionnement).publish_state("Séchage approfondi");
            } else {
              id(mode_fonctionnement).publish_state("Off");
            }
        - script.execute: gestion_chauffage
