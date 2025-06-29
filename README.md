substitutions:
  - old: |-
      - platform: template
        name: "Filament"
        id: filament
        icon: "mdi:printer-3d"
        lambda: |-
          if (id(mode) == 0) return "PLA";
          else if (id(mode) == 1) return "PETG";
          else if (id(mode) == 2) return "ABS";
          else if (id(mode) == 3) return "Nylon";
          else return "N/A";
  - new: |-
      # Define filament index (0=PLA,1=PETG,2=ABS,3=Nylon)
      globals:
        - id: filament_index
          type: int
          restore_value: true
          initial_value: '0'

      # Define manual power for test mode (0-100%)
        - id: puissance_test
          type: int
          restore_value: true
          initial_value: '0'

      # Filament name template sensor for display
      text_sensor:
        - platform: template
          name: "Filament"
          id: filament
          icon: "mdi:printer-3d"
          lambda: |-
            switch (id(filament_index)) {
              case 0: return {"PLA"};
              case 1: return {"PETG"};
              case 2: return {"ABS"};
              case 3: return {"Nylon"};
              default: return {"N/A"};
            }
binary_sensor:
  # Buttons already defined here; add actions below

script:
  - id: gestion_chauffage
    then:
      - lambda: |-
          // Determine target temperature based on filament_index
          float target_temp = 0.0;
          switch (id(filament_index)) {
            case 0: target_temp = 42.5; break; // PLA 40-45°C average
            case 1: target_temp = 55.0; break; // PETG approx 55°C (assumed)
            case 2: target_temp = 70.0; break; // ABS 65-75°C average
            case 3: target_temp = 75.0; break; // Nylon 70-80°C average
            default: target_temp = 40.0;
          }
          id(target_temperature) = target_temp;

          // Now apply heating logic depending on mode
          if (id(mode) == 2) { // Test mode: use puissance_test
            if (id(puissance_test) > 0) {
              id(chauffage_pwm).set_level((float)id(puissance_test) / 100.0);
              id(chauffage_on) = true;
            } else {
              id(chauffage_pwm).set_level(0);
              id(chauffage_on) = false;
            }
          } else if (id(mode) == 0) { // Maintien mode
            if (id(humidity) > id(humidity_threshold) + 10) {
              id(chauffage_pwm).set_level(1.0);
              id(chauffage_on) = true;
            } else if (id(humidity) > id(humidity_threshold) + 5) {
              id(chauffage_pwm).set_level(0.5);
              id(chauffage_on) = true;
            } else if (id(humidity) > id(humidity_threshold)) {
              id(chauffage_pwm).set_level(0.3);
              id(chauffage_on) = true;
            } else {
              id(chauffage_pwm).set_level(0);
              id(chauffage_on) = false;
            }
          } else if (id(mode) == 1) { // Séchage approfondi
            id(chauffage_pwm).set_level(1.0);
            id(chauffage_on) = true;
          } else {
            id(chauffage_pwm).set_level(0);
            id(chauffage_on) = false;
          }

binary_sensor:
  - platform: gpio
    id: bouton_plus
    pin:
      number: GPIO26
      mode: INPUT_PULLUP
      inverted: true
    on_press:
      then:
        - lambda: |-
            if (id(mode) == 2) { // Test mode
              // Increase puissance_test by 10%, max 100%
              if (id(puissance_test) < 100) {
                id(puissance_test) += 10;
              }
            } else {
              // In other modes, increase humidity threshold by 1
              id(humidity_threshold) += 1;
            }
        - script.execute: gestion_chauffage

  - platform: gpio
    id: bouton_moins
    pin:
      number: GPIO13
      mode: INPUT_PULLUP
      inverted: true
    on_press:
      then:
        - lambda: |-
            if (id(mode) == 2) { // Test mode
              // Decrease puissance_test by 10%, min 0%
              if (id(puissance_test) > 0) {
                id(puissance_test) -= 10;
              }
            } else {
              // In other modes, decrease humidity threshold by 1
              id(humidity_threshold) -= 1;
            }
        - script.execute: gestion_chauffage

  - platform: gpio
    id: bouton_mode
    pin:
      number: GPIO27
      mode: INPUT_PULLUP
      inverted: true
    on_press:
      then:
        - lambda: |-
            id(mode) = (id(mode) + 1) % 3; // Cycle modes 0,1,2
        - script.execute: gestion_chauffage

  # Additional buttons to cycle filament type in Test mode (simulate long press or double press)
  - platform: gpio
    id: bouton_plus_long
    pin:
      number: GPIO14
      mode: INPUT_PULLUP
      inverted: true
    on_press:
      then:
        - lambda: |-
            if (id(mode) == 2) { // Test mode
              // Cycle filament index +1 modulo 4
              id(filament_index) = (id(filament_index) + 1) % 4;
              // Update target_temperature immediately
              float target_temp = 0.0;
              switch (id(filament_index)) {
                case 0: target_temp = 42.5; break;
                case 1: target_temp = 55.0; break;
                case 2: target_temp = 70.0; break;
                case 3: target_temp = 75.0; break;
                default: target_temp = 40.0;
              }
              id(target_temperature) = target_temp;
            }
        - script.execute: gestion_chauffage

  - platform: gpio
    id: bouton_moins_long
    pin:
      number: GPIO15
      mode: INPUT_PULLUP
      inverted: true
    on_press:
      then:
        - lambda: |-
            if (id(mode) == 2) { // Test mode
              // Cycle filament index -1 modulo 4
              id(filament_index) = (id(filament_index) + 3) % 4;
              // Update target_temperature immediately
              float target_temp = 0.0;
              switch (id(filament_index)) {
                case 0: target_temp = 42.5; break;
                case 1: target_temp = 55.0; break;
                case 2: target_temp = 70.0; break;
                case 3: target_temp = 75.0; break;
                default: target_temp = 40.0;
              }
              id(target_temperature) = target_temp;
            }
        - script.execute: gestion_chauffage

display:
  - platform: ssd1306_i2c
    id: oled_display
    address: 0x3C
    lambda: |-
      it.printf(0, 0, id(font), "Mode: %s", id(mode) == 0 ? "Maintien" : id(mode) == 1 ? "Séchage" : "Test");
      it.printf(0, 10, id(font), "Temp: %.1fC", id(temperature));
      it.printf(0, 20, id(font), "Hum: %.1f%%", id(humidity));
      it.printf(0, 30, id(font), "T Cible: %.1fC", id(target_temperature));

      if (id(mode) == 2) { // Test mode
        it.printf(0, 40, id(font), "Puissance: %d%%", id(puissance_test));
      } else {
        // Show heating line with ON/OFF and PWM level
        if (id(chauffage_on)) {
          it.printf(0, 40, id(font), "Chauffage: ON %d%%", (int)(id(chauffage_pwm).get_level() * 100));
        } else {
          it.printf(0, 40, id(font), "Chauffage: OFF 0%%");
        }
      }

# End of substitutions
