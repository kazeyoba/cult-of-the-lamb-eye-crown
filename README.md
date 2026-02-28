# 👁️ Cult of the Lamb : Eye Crown Project

**Regard de prédateur intelligent à suivi de mouvement**

Ce projet transforme un **ESP32-S3** et un **écran TFT** en un œil de monstre hyper-réaliste inspiré de l'univers de *Cult of the Lamb*. Grâce à l'IA embarquée, l'œil détecte les visages et les suit du regard avec une pupille slit (verticale) et un fond rouge sang.

## 🛠️ LISTE DES COMPOSANTS

| Composant | Rôle | Spécification |
| --- | --- | --- |
| **ESP32-S3 Freenove** | Cerveau & IA | Version avec port caméra FPC. |
| **Caméra OV2640** | Vision | Objectif **160° Grand Angle** (Fisheye). |
| **Écran TFT 2.8" SPI** | Le Regard | 320x240, contrôleur **ILI9341**. |
| **Batterie LiPo 3.7V** | Énergie | EEMB 2000mAh 103454. |
| **Module TP4056** | Charge & Sécurité | Protection de décharge incluse. |
| **Interrupteur Slide** | Contrôle | Marche/Arrêt physique. |

## 🔌 SCHÉMA D'ALIMENTATION SÉCURISÉ

1. **Batterie** [B+/B-] -> **TP4056** [B+/B-]
2. **TP4056 [OUT-]** -> **ESP32-S3 [GND]**
3. **TP4056 [OUT+]** -> **Interrupteur [Patte milieu]**
4. **Interrupteur [Patte latérale]** -> **ESP32-S3 [5V/VIN]**

## 🧠 LOGIQUE D'ANIMATION

1. **Suivi (Tracking) :** Mapping des coordonnées de la caméra vers l'écran.
2. **Pupille Slit :** Forme ovale verticale noire (type reptile/démon).
3. **États de conscience :** - *Repos :* Balayage lent de gauche à droite.
   - *Concentration :* La pupille se rétrécit et fixe la cible détectée.

### 💻 CODE ESP32 (Arduino IDE)

Ce code utilise la bibliothèque **TFT_eSPI**. Assure-toi que ton fichier `User_Setup.h` correspond aux pins : **CS:10, RST:18, DC:17, MOSI:11, SCK:12**.

```cpp
#include <SPI.h>
#include <TFT_eSPI.h> // Bibliothèque de Bodmer

TFT_eSPI tft = TFT_eSPI();

// Configuration des couleurs
#define C_BLOOD 0xB800    // Rouge sombre
#define C_PUPIL TFT_BLACK
#define C_SCLERA TFT_RED

// Variables d'animation
int eyeX = 160, eyeY = 120;
int targetX = 160, targetY = 120;
int pupilWidth = 30;
bool isTracking = false;

void setup() {
  tft.init();
  tft.setRotation(1); // Mode paysage
  tft.fillScreen(C_SCLERA);
  
  Serial.begin(115200);
}

void drawEye(int x, int y, int pW) {
  tft.fillScreen(C_SCLERA); 
  
  tft.fillSmoothCircle(x, y, 70, C_BLOOD);
  
  // FillEllipse(x, y, rayon_horizontal, rayon_vertical, couleur)
  tft.fillEllipse(x, y, pW, 65, C_PUPIL);
  
  tft.fillSmoothCircle(x - 15, y - 25, 8, TFT_WHITE);
}

void loop() {  
  if (!isTracking) {
    // MODE REPOS : Balayage horizontal lent
    targetX = 160 + sin(millis() / 1000.0) * 60;
    targetY = 120;
    pupilWidth = 35; // Pupille un peu plus large
    
    // Aléatoirement, on simule une "concentration"
    if (random(200) == 1) isTracking = true;
  } else {
    // MODE CONCENTRATION : Pupille fine et fixe
    pupilWidth = 15; 
    if (random(100) == 1) isTracking = false;
  }

  // Interpolation fluide (Ease-in-out)
  eyeX += (targetX - eyeX) * 0.1;
  eyeY += (targetY - eyeY) * 0.1;

  drawEye(eyeX, eyeY, pupilWidth);
  
  delay(20); // ~50 FPS
}
```
