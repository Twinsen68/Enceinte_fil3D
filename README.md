🔥 Enceinte de stockage de filament avec chauffage intelligent

📌 Introduction

Cette enceinte de stockage pour filaments 3D permet de maintenir une faible humidité grâce à un chauffage intelligent contrôlé par ESPHome.

Fonctionnalités principales :
	•	Mode maintien : Maintient l’humidité sous un seuil défini.
	•	Mode séchage approfondi : Assèche intensément les filaments et le dessicant.
	•	Régulation intelligente de la chauffe (PWM progressif selon l’humidité).
	•	Affichage OLED avec veille automatique après 10 minutes d’inactivité.
	•	Réglage de la durée du séchage approfondi (1 à 8 heures).
	•	Contrôle via boutons physiques et Home Assistant.

Compatible avec Home Assistant via ESPHome pour une gestion domotique complète.

⸻

🎛️ Modes de fonctionnement

1️⃣ Mode “Maintien de l’humidité”

➡ Objectif : Garder l’humidité sous un seuil défini en activant le chauffage modérément.
✅ Idéal pour stocker les filaments sans les abîmer.
	•	Si l’humidité est supérieure au seuil +10% → Chauffage à 100%
	•	Si l’humidité est supérieure au seuil +5% → Chauffage à 50%
	•	Si l’humidité dépasse légèrement le seuil → Chauffage à 30%
	•	Si l’humidité est sous le seuil → Chauffage éteint

⸻

2️⃣ Mode “Séchage approfondi”

➡ Objectif : Séchage intensif des filaments et du dessicant sur une durée personnalisée.
✅ Idéal après une ouverture de l’enceinte ou lorsque l’humidité est trop élevée.

Température recommandée par type de filament :
	•	PLA : 40-45°C (4 heures)
	•	ABS : 65-75°C (4 heures)
	•	Nylon : 70-80°C (4 heures)

Une fois terminé, l’enceinte repasse automatiquement en mode Maintien.
La durée est réglable de 1h à 8h via Home Assistant.

⸻

📺 Affichage OLED

L’écran affiche en permanence :
`Mode: Maintien / Séchage  
Temp: XX.X°C  
Humidité: XX.X%  
Cible: XX.X%  `

	•	Extinction automatique après 10 minutes d’inactivité
	•	Rallumage dès qu’un bouton est pressé

⸻

🎛️ Boutons de contrôle

L’enceinte dispose de trois boutons physiques pour ajuster les paramètres et changer de mode.
	•	Bouton + (GPIO26) → Augmente le seuil d’humidité cible.
	•	Bouton - (GPIO13) → Diminue le seuil d’humidité cible.
	•	Bouton Mode (GPIO27) → Alterne entre “Maintien” et “Séchage approfondi”.

⸻

🛠️ Installation et Déploiement ESPHome

1️⃣ Matériel requis
	•	ESP32 DevKit V1
	•	Capteur d’humidité et température DHT20
	•	Écran OLED SSD1306
	•	MOSFET pour le chauffage
	•	Chauffage 12V ou 24V (selon ton installation)
	•	Boutons poussoirs pour le contrôle physique
	•	Alimentation 5V pour l’ESP32

⸻

2️⃣ Installation avec ESPHome

📥 Méthode simple : copier install.yaml

ESPHome permet d’inclure la configuration directement depuis GitHub.
➡ Copiez ce fichier dans ESPHome :

esp32:  
  board: esp32dev  # Spécifie le modèle de la carte ESP32 (ESP32 DevKit V1 ici).
  framework:
    type: arduino  # Utilisation du framework Arduino, largement compatible avec ESPHome.

packages:  # Inclusion d'une configuration externe pour modularité et réutilisation.
  twinsen68.enceinte_fil3d:  # Nom du package inclus.
    url: https://github.com/Twinsen68/Enceinte_fil3D  # Lien vers le dépôt GitHub contenant la configuration de l’enceinte.
    file: enceinte_fil3D.yaml  # Fichier YAML spécifique inclus depuis le dépôt GitHub.
    ref: v1.0.0  # Version spécifique du fichier à utiliser.

esphome:  
  name: enceinte_fil3d
  name_add_mac_suffix: false  # Empêche l'ajout d'un suffixe MAC au nom pour éviter les doublons sur le réseau.
  friendly_name: "Enceinte filament 3D contrôlée"

api:  # Active l’API ESPHome pour la communication avec Home Assistant.
  encryption:
    key: tsHqLo48mLGhWo/qQQrZUFdsEwuZcC1/BlUOXWExy14=  # Clé de chiffrement pour sécuriser les échanges.

wifi:
  ssid: !secret wifi_ssid  # Nom du réseau Wi-Fi
  password: !secret wifi_password  # Mot de passe du Wi-Fi

3️⃣ Déploiement dans ESPHome
	1.	Ouvrez ESPHome dans Home Assistant.
	2.	Ajoutez un nouvel appareil et copiez-collez install.yaml.
	3.	Flashez l’ESP32 via USB pour la première installation.
	4.	Le module se connectera au Wi-Fi et sera visible dans Home Assistant.
	5.	Accédez aux contrôles directement depuis Home Assistant.

⸻

❓ Dépannage
	•	Le chauffage ne s’allume pas : Vérifiez si l’humidité actuelle est inférieure au seuil défini.
	•	L’écran OLED n’affiche rien : Vérifiez qu’il est bien alimenté et connecté à SDA/SCL.
	•	Le module ne se connecte pas au WiFi : Vérifiez le SSID et le mot de passe dans ESPHome.

⸻

🎯 Conclusion

Tu as maintenant une enceinte totalement autonome et intelligente pour stocker tes filaments 3D dans les meilleures conditions.
🔥 Profite d’une meilleure qualité d’impression et d’un stockage sans humidité !

Besoin d’améliorations ? Ouvre une issue sur GitHub ou demande des conseils ! 🚀😊

⸻

📎 Liens utiles
	•	ESPHome Documentation
	•	Home Assistant
	•	GitHub du projet

⸻

🔧 Contributeurs : @Twinsen68
💡 Licence : MIT

⸻

🚀 Prêt à l’utiliser ?

Télécharge ESPHome, flashe ton ESP32, et commence à sécher tes filaments ! 😃🔥

⸻

👉 Cette version est optimisée pour un copier-coller direct dans VSCode et GitHub, sans tableau. 