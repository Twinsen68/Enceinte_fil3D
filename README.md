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
``Mode: Maintien / Séchage  
Temp: XX.X°C  
Humidité: XX.X%  
Cible: XX.X%  ```