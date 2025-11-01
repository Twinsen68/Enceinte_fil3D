# 🔥 Enceinte de stockage de filament avec chauffage intelligent

## 📌 Introduction
Cette enceinte de stockage pour **filaments 3D** permet de **maintenir une faible humidité** grâce à un **chauffage intelligent** contrôlé par **ESPHome**.

### **Fonctionnalités principales**
- **Mode maintien** : Maintient l'humidité sous un seuil défini.
- **Mode séchage approfondi** : Assèche intensément les filaments et le dessicant.
- **Régulation intelligente de la chauffe** (PWM progressif selon l'humidité).
- **Affichage OLED avec veille automatique** après 10 minutes d’inactivité.
- **Réglage de la durée du séchage approfondi** (1 à 8 heures).
- **Contrôle via boutons physiques et Home Assistant**.
- **Sélection du type de filament directement via les boutons physiques en mode Off ou Maintien/Séchage**
- **Régulation intelligente selon le type de filament choisi (température cible adaptée)**
- **Mode Test manuel pour contrôler la puissance PWM de chauffe directement**

**⚙ Compatible avec Home Assistant** via ESPHome pour une gestion domotique complète.

---

## 🎛️ Modes de fonctionnement

### **1️⃣ Mode "Maintien de l'humidité"**
➡ **Objectif** : Garder l’humidité sous un seuil défini en activant le chauffage modérément.  
✅ Idéal pour **stocker les filaments sans les abîmer**.

- **Si l'humidité est supérieure au seuil +10%** → Chauffage à **100%**
- **Si l'humidité est supérieure au seuil +5%** → Chauffage à **50%**
- **Si l'humidité dépasse légèrement le seuil** → Chauffage à **30%**
- **Si l'humidité est sous le seuil** → Chauffage **éteint**

⚡ En fonction du filament choisi (PLA, PETG, ABS, Nylon), la température cible de maintien est automatiquement ajustée pour optimiser la conservation.
⚡ La puissance de chauffe est régulée de manière progressive via un contrôle PID selon l'écart avec la température cible.
### **4️⃣ Mode Test**
➡ **Objectif** : Permettre de tester manuellement le chauffage PWM.

- **Réglez manuellement la puissance PWM entre 0% et 100% via les boutons + et -.**
- **Idéal pour tester le MOSFET ou le chauffage sans attendre une condition d'humidité/température.**
- **L'écran OLED affichera 'Chauffage: ON XX%' selon le réglage manuel.**

---

### **2️⃣ Mode "Séchage approfondi"**
➡ **Objectif** : Séchage intensif des filaments et du dessicant sur une durée personnalisée.  
✅ Idéal après **une ouverture de l’enceinte** ou lorsque l’humidité est trop élevée.

**Température recommandée par type de filament** :
- **PLA** : 40-45°C (4 heures)
- **ABS** : 65-75°C (4 heures)
- **Nylon** : 70-80°C (4 heures)

💡 **La durée est réglable de 1h à 8h via Home Assistant**.  
⌛ **Une fois terminé, l’enceinte repasse automatiquement en mode Maintien**.

---

## 📺 Affichage OLED
L’écran affiche en permanence :

Mode: Maintien / Séchage
Temp: XX.X°C
Humidité: XX.X%
T Cible: XX°C / H Cible: XX%

💡 Affichage dynamique du type de filament sélectionné dans tous les modes.
💡 En mode Off, les boutons + et - permettent de sélectionner le type de filament à l'écran directement.
💡 En mode Test, affichage de la puissance de chauffe manuelle réglée.

✅ **Extinction automatique après 10 minutes d’inactivité**.
✅ **Rallumage dès qu’un bouton est pressé**.

### Réglages visibles à l'écran selon le mode

- **Mode Off**
  - Affiche le matériau choisi (PLA, PETG, ABS, ...).
  - Les boutons **+** et **-** font défiler les matériaux et mettent à jour la température cible par défaut.
- **Mode Maintien**
  - Affiche l'humidité actuelle et la température cible associée au matériau.
  - Les boutons **+** et **-** modifient l'humidité de consigne affichée.
- **Mode Séchage approfondi**
  - Affiche le temps restant et la puissance de chauffe appliquée.
  - Les boutons **+** et **-** ajustent l'humidité cible pour le séchage.
- **Mode Test**
  - Affiche "Puissance" suivie du pourcentage de PWM appliqué au chauffage.
  - Les boutons **+** et **-** font varier cette puissance manuellement.

---

## 🎛️ Boutons de contrôle

L’enceinte dispose de **trois boutons physiques** pour ajuster les paramètres et changer de mode.

- **En mode Off** : les boutons + et - changent le filament affiché et sélectionné.
- **En mode Test** : les boutons + et - modifient directement la puissance PWM de chauffe.
- **En mode Maintien ou Séchage** : les boutons + et - ajustent l'humidité cible.
- **Bouton Mode** : cycle entre Off, Test, Maintien, Séchage approfondi.

---

# 🛠️ Installation et Déploiement ESPHome

## 1️⃣ Matériel requis
- **ESP32 DevKit V1**
- **Capteur d’humidité et température DHT20**
- **Écran OLED SSD1306**
- **MOSFET pour le chauffage**
- **Chauffage 12V ou 24V (selon ton installation)**
- **Boutons poussoirs pour le contrôle physique**
- **Alimentation 5V pour l’ESP32**

### 🔌 Schéma de câblage (GPIO)

- **DHT20** → `GPIO4`
- **MOSFET (PWM)** → `GPIO14`
- **OLED SDA/SCL** → `GPIO21`/`GPIO22`
- **Bouton +** → `GPIO26`
- **Bouton -** → `GPIO13`
- **Bouton Mode** → `GPIO27`
- **Alimentation** : 5V pour l’ESP32 et 12/24V pour le chauffage

---

## 2️⃣ Installation avec ESPHome

### 📥 **Méthode simple : copier `install.yaml`**

ESPHome permet d’inclure la configuration directement depuis **GitHub**.  
➡ **Copiez ce fichier dans ESPHome** :

```yaml
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
  ```

## 3️⃣ Déploiement dans ESPHome

1. **Ouvrez ESPHome dans Home Assistant**.
2. **Ajoutez un nouvel appareil** et copiez-collez `install.yaml`.
3. **Flashez l’ESP32 via USB** pour la première installation.
4. **Le module se connectera au Wi-Fi et sera visible dans Home Assistant**.
5. **Accédez aux contrôles directement depuis Home Assistant**.

---

### 🔐 Configuration du Wi-Fi via `secrets.yaml`

Pour que l'ESP32 se connecte correctement à votre réseau, les identifiants Wi-Fi doivent être définis dans un fichier de secrets :

1. **Depuis Home Assistant / ESPHome** :
   - Ouvrez l'interface ESPHome et cliquez sur le menu "Secrets" (icône 🔑 en bas à gauche).
   - Ajoutez les entrées suivantes en remplaçant par vos informations :
     ```yaml
     wifi_ssid: "NomDeVotreReseau"
     wifi_password: "MotDePasseSuperSecret"
     ```
2. **En compilation locale** : créez (ou complétez) le fichier `secrets.yaml` dans le même dossier que `install.yaml` avec les mêmes clés `wifi_ssid` et `wifi_password`.

⚠️ Sans ces secrets, l'appareil ne pourra pas se connecter au Wi-Fi et la compilation ESPHome échouera. Assurez-vous également que `install.yaml` référence bien ces clés (voir fichier pour les commentaires détaillés).

---

### 💻 Compilation locale depuis un terminal macOS

Si Home Assistant n'arrive pas à compiler le projet (fichier trop volumineux ou mémoire insuffisante), il est possible de **générer le firmware directement depuis un Mac** en utilisant le terminal.

1. **Installer/Mettre à jour Python** : macOS dispose déjà de Python 3, mais vous pouvez installer la dernière version depuis [python.org](https://www.python.org/downloads/).
2. **Créer un environnement virtuel** (permet d'isoler les dépendances ESPHome) :
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```
3. **Installer ESPHome** dans l'environnement :
   ```bash
   pip install --upgrade pip esphome
   ```
4. **(Optionnel) Réinitialiser le cache ESPHome** si vous rencontrez des erreurs de compilation liées à d'anciennes versions :
   ```bash
   rm -rf ~/.esphome
   ```
5. **Lancer la compilation** en pointant vers le fichier `install.yaml` du dépôt cloné :
   ```bash
   esphome run /chemin/vers/Enceinte_fil3D/install.yaml
   ```

   > 📌 **Syntaxe rappel** : la commande s'utilise sous la forme `esphome run <chemin_du_yaml> [--device <port>]`.
   > Assure-toi de laisser un **espace entre `run` et le chemin** (par exemple `esphome run /Users/.../install.yaml`).
   > Si le chemin contient des espaces, place-le entre guillemets (`"..."`).

   > ℹ️ **Si plusieurs périphériques série sont présents**, précise explicitement le port USB de l'ESP32 avec l'option `--device`.
   > Par exemple sur macOS&nbsp;:
   > ```bash
   > esphome run install.yaml --device /dev/cu.usbserial-1101
   > ```
   > et sur Linux&nbsp;:
   > ```bash
   > esphome run install.yaml --device /dev/ttyUSB0
   > ```

   > ℹ️ **Astuce :** si vous préférez utiliser un chemin relatif, placez-vous d'abord dans le dossier du projet :
   > ```bash
   > cd /chemin/vers/Enceinte_fil3D
   > esphome run install.yaml
   > ```
   > Sinon, assurez-vous de fournir le **chemin complet** vers `install.yaml` lorsque vous lancez la commande depuis un autre emplacement.

> 💡 Lors de la première exécution, ESPHome vous proposera de flasher l'ESP32 connecté en USB. Les compilations suivantes pourront être envoyées via le réseau.

---

## 🧰 Structure du dépôt

- `enceinte_fil3D.yaml` : configuration principale de l'enceinte avec tout le code ESPHome.
- `install.yaml` : fichier minimal à copier dans ESPHome pour importer la configuration.
- `instructions compli terminal mac` : aide rapide pour installer ESPHome en local sur macOS.

---

## 📝 Personnalisation de la configuration

- Modifie **`install.yaml`** pour renseigner ton `wifi_ssid` et `wifi_password`.
- Dans **`enceinte_fil3D.yaml`**, ajuste si besoin :
  - les valeurs d'**humidité cible** (`humidite_cible_maintien` et `humidite_cible_sechage`)
  - la **durée de séchage par défaut** (`duree_sechage`)
  - la **liste des filaments** disponibles dans le sélecteur `filament`
  - la **température cible** selon ton propre matériel

---

## 🔄 Mise à jour du cache ESPHome

Si vous modifiez le fichier `enceinte_fil3D.yaml` et que les changements ne sont pas pris en compte, il est nécessaire de forcer la mise à jour du cache d'ESPHome.  
Pour cela, il suffit de modifier la version du fichier dans `install.yaml` :

**Dans `install.yaml`, remplace :**
```yaml
ref: v1.0.0
```
**Par une nouvelle version :**
```yaml
ref: v1.0.1
```

Ensuite, **redémarrez ESPHome et rechargez la configuration** pour que les mises à jour soient bien prises en compte.

---

# ❓ Dépannage

- **Le type de filament affiché ne change pas** : assurez-vous d'être en mode Off pour le sélectionner avec les boutons + et -.
- **Le chauffage ne s'allume pas en mode Test** : vérifiez que la puissance PWM réglée est supérieure à 0%.
- **Le chauffage ne s'allume pas** : Vérifiez si l’humidité actuelle est inférieure au seuil défini.
- **L'écran OLED n'affiche rien** : Vérifiez qu’il est bien alimenté et connecté à SDA/SCL.
- **Le module ne se connecte pas au WiFi** : Vérifiez le SSID et le mot de passe dans ESPHome.
- **L'upload OTA échoue avec `Connecting to ... failed: timed out`** : assurez-vous que l'ESP32 est bien connecté au même réseau, que l'adresse IP est correcte (vérifiez dans Home Assistant) et que le port 3232 n'est pas bloqué par un pare-feu. N'hésitez pas à lancer `ping 192.168.1.100` pour confirmer l'accessibilité ou à reflasher une première fois via USB si l'appareil ne répond plus.

---

# 🎯 Conclusion

Tu as maintenant une **enceinte totalement autonome et intelligente** pour stocker tes **filaments 3D** dans **les meilleures conditions**.  
🔥 **Profite d’une meilleure qualité d’impression et d’un stockage sans humidité !**  

✨ Désormais, ton enceinte de filament est capable de gérer automatiquement la température de maintien en fonction du filament choisi et permet un contrôle manuel complet en mode Test.

Besoin d’améliorations ? **Ouvre une issue sur GitHub** ou demande des conseils ! 🚀😊

---

## 📎 Liens utiles

- [ESPHome Documentation](https://esphome.io/)
- [Home Assistant](https://www.home-assistant.io/)
- [GitHub du projet](https://github.com/Twinsen68/Enceinte_fil3D)

---

**🔧 Contributeurs** : @Twinsen68  
**💡 Licence** : MIT  

---

# 🚀 Prêt à l’utiliser ?

Télécharge **ESPHome**, flashe ton **ESP32**, et **commence à sécher tes filaments !** 😃🔥