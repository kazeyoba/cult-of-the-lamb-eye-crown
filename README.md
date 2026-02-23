# cult-of-the-lamb-eye-crown

**Regard de prédateur intelligent à suivi de mouvement**

Ce projet transforme un **ESP32-S3** et un **écran TFT** en un œil de monstre hyper-réaliste. Grâce à l'intelligence artificielle embarquée, l'œil détecte les visages et les suit du regard avec une fluidité organique.

## 🛠️ LISTE DES COMPOSANTS

| Composant | Rôle | Spécification |
| --- | --- | --- |
| **ESP32-S3 Terminal Board** | Cerveau & IA | Version avec PSRAM et borniers à vis. |
| **Caméra OV2640** | Vision | Objectif **160° Grand Angle** (Fisheye). |
| **Écran TFT 2.8" SPI** | Le Regard | Résolution 320x240, contrôleur **ILI9341**. |
| **Batterie LiPo 3.7V** | Énergie | Plat (pouch), min. 1000mAh. |
| **Fils Jumper M-F** | Connectique | Côté femelle sur l'écran, dénudé dans les vis. |

## 🔌 SCHÉMA DE CÂBLAGE (SANS SOUDURE)

Dénude l'extrémité des fils Jumper et insère-les dans les borniers correspondant aux numéros GPIO suivants :

| PIN ÉCRAN (TFT) | BORNES ESP32-S3 | NOTE |
| --- | --- | --- |
| **VCC** | **3V3** | Alimentation |
| **GND** | **GND** | Masse |
| **CS** | **GPIO 10** | Chip Select |
| **RESET** | **GPIO 11** | Reset écran |
| **DC** | **GPIO 12** | Data/Command |
| **SDI (MOSI)** | **GPIO 13** | Données SPI |
| **SCK (CLK)** | **GPIO 14** | Horloge |
| **LED** | **3V3** | Rétroéclairage |

> **IMPORTANT :** La caméra se branche directement dans le connecteur à clapet (nappe FPC) situé sur la carte ESP32-S3.


## 🧠 LOGIQUE D'ANIMATION

Le code intègre trois couches de mouvements superposées pour un aspect vivant :

1. **Suivi (Tracking) :** Mapping des coordonnées  de la caméra vers l'écran avec interpolation fluide.
2. **Pulsation :** La pupille verticale "respire" (élargissement sinusoïdal de  pixels).
3. **Clignement :** Fermeture aléatoire de la paupière rouge toutes les 3 à 9 secondes.


## 💻 INSTALLATION DU CODE

### 1. Préparation de l'IDE Arduino

* Installez le support des cartes **ESP32** (Outils > Type de carte > Gestionnaire de carte).
* Installez la bibliothèque **TFT_eSPI** de Bodmer.
* **Configuration cruciale :** Allez dans le dossier `libraries/TFT_eSPI/User_Setup.h` et assurez-vous que les numéros de pins correspondent à ceux du tableau ci-dessus.

### 2. Téléchargement du code

* Copiez le code complet fourni précédemment.
* Sélectionnez le modèle de carte **ESP32S3 Dev Module**.
* Activez l'option **PSRAM: "OPI PSRAM"** dans les menus de l'IDE (essentiel pour l'IA).
* Téléversez via USB-C.

## 🎨 DESIGN VISUEL

* **Sclère :** Fond rouge intense (`TFT_RED`).
* **Iris :** Dégradé rouge sombre (`0x8000`).
* **Pupille :** Ovale verticale noire type reptile.
* **Reflet :** Point blanc fixe décalé pour simuler la réflexion de la lumière sur une cornée humide.

## 📦 INTÉGRATION DANS LE PROPS

1. **Caméra :** Doit être centrée juste au-dessus de l'écran.
2. **Isolation :** Placez la batterie entre l'écran et la carte ESP32, isolée par du ruban adhésif pour éviter tout court-circuit.
3. **Masquage :** Utilisez une façade noire (carton plume ou impression 3D) pour ne laisser voir que l'écran et la lentille de la caméra.

## ⚠️ PRÉCAUTIONS

* **Polarité Batterie :** Ne jamais inverser le rouge (+) et le noir (-) dans les borniers.
* **Chauffe :** Le processeur S3 peut chauffer lors de l'analyse IA. Assurez-vous d'avoir quelques trous d'aération dans votre accessoire.