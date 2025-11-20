# FCH7 - Flight Controller pour Drone Quadcopter

##  Vue d'ensemble

FCH7 est un contrôleur de vol (flight controller) haute performance pour drones quadcopters, conçu avec KiCad. Ce projet vise à créer une solution complète et intégrée pour le contrôle de drones FPV avec des composants modernes et performants.

> ** Statut du projet:** Ce projet est actuellement en cours de développement. Les schématiques sont en phase de conception et le PCB n'est pas encore finalisé.

##  Caractéristiques principales

### Microcontrôleur
- **STM32H743VITx** - Processeur ARM Cortex-M7 cadencé à 480 MHz
  - 2048 KB de mémoire Flash
  - 1024 KB de RAM
  - 82 GPIO disponibles
  - Package LQFP100

### Capteurs embarqués
- **ICM42688P** - IMU haute performance (gyroscope + accéléromètre)
- **MPU6500** - IMU secondaire pour redondance
- **QMC5883L** - Magnétomètre 3 axes pour la boussole
- **SPL06-001** - Baromètre pour l'altitude

### Connectivité
- **ExpressLRS (ELRS)** - Récepteur RF 2.4 GHz intégré
  - Module RF_2.4 dédié
  - Support pour liaison longue portée et faible latence
- **USB-C** - Connecteur USB 2.0 16 broches pour configuration et mise à jour
- **Connecteurs HDGC1002WR** - Multiples connecteurs pour périphériques
  - GPS (4 broches)
  - Caméra FPV
  - VTX (émetteur vidéo)
  - Ports UART additionnels

### Alimentation
- **Gestion de puissance intelligente**
  - Régulateur **MP9943GQ-z** - Buck converter principal
  - Régulateur **ME6217C33M5G** - LDO 3.3V
  - Support pour batteries LiPo (VBAT)
  - Rails multiples: VDD_MCU, VDD_RECV, 4V5
  - Protection et filtrage

### OSD (On-Screen Display)
- **AT7456E** - Puce OSD pour affichage de télémétrie sur le flux vidéo FPV
  - Overlay d'informations en temps réel
  - Compatible avec les standards vidéo analogiques

### Stockage et LEDs
- **Blackbox** - Interface SDIO pour carte microSD
  - Enregistrement des données de vol pour analyse
- **LEDs WS2812** (adressables) - Indicateurs visuels programmables
- **LEDs de statut** - Multiples LEDs pour diagnostic

##  Architecture du projet

Le projet est organisé en modules hiérarchiques distincts:

```
fch7/
├── fch7.kicad_sch          # Schématique principal (root)
├── MCU.kicad_sch           # Module microcontrôleur (STM32H7)
├── Power.kicad_sch         # Module gestion d'alimentation
├── sensors.kicad_sch       # Module capteurs (IMU, magnéto, baro)
├── rf_rx.kicad_sch         # Module récepteur RF (ExpressLRS)
├── osd.kicad_sch           # Module OSD vidéo
├── blackbox_led.kicad_sch  # Module enregistrement et LEDs
├── connectors.kicad_sch    # Module connecteurs externes
├── fclib.kicad_sym         # Bibliothèque de symboles personnalisés
└── fch7.kicad_pcb          # Layout PCB (à venir)
```

##  Composants personnalisés

Le projet utilise une bibliothèque de symboles personnalisés (`fclib.kicad_sym`) contenant:
- AT7456E (OSD)
- ICM42688P (IMU)
- MPU6500 (IMU)
- QMC5883L (Magnétomètre)
- SPL06-001 (Baromètre)
- RF_2.4 et elrs_rx (Modules RF)
- MP9943GQ-z (Buck converter)
- ME6217C33M5G (LDO)
- Connecteurs HDGC1002WR (4P, 6P, 8P)
- CSNP1GCR01-BOW
- WS2812_ (LED adressable)

##  Applications

Ce flight controller est conçu pour:
- Drones de course FPV
- Drones de freestyle
- Drones longue portée
- Applications de vol stabilisé et acrobatique
- Compatibilité avec les firmwares open-source (Betaflight, INAV, etc.)

##  Spécifications techniques

### Interfaces disponibles
- **SPI** - Communication haute vitesse avec capteurs et OSD
- **I2C** - Communication avec magnétomètre et baromètre
- **UART** - Multiples ports pour GPS, télémétrie, périphériques
- **SDIO** - Interface carte SD pour blackbox
- **PWM/DShot** - Contrôle moteurs (via GPIO MCU)
- **USB** - Configuration et mise à jour firmware

### Alimentation
- Entrée: Batteries LiPo (2-6S typique)
- Rails régulés: 3.3V, 4.5V, VDD_MCU, VDD_RECV
- Protection contre les inversions de polarité
- Filtrage capacitif avancé

##  Outils requis

### Logiciels
- **KiCad 9.0** ou supérieur - Conception schématique et PCB
- **Git** - Gestion de version

### Pour le développement firmware (futur)
- STM32CubeIDE ou PlatformIO
- Firmware compatible STM32H7 (Betaflight, INAV, ArduPilot, etc.)
- Programmateur ST-Link V2/V3

##  Statut du développement

- [x] Schématique du MCU
- [x] Schématique de l'alimentation
- [x] Schématique des capteurs
- [x] Schématique du récepteur RF
- [x] Schématique OSD
- [x] Schématique Blackbox/LEDs
- [x] Schématique des connecteurs
- [x] Bibliothèque de symboles personnalisés
- [ ] Layout PCB
- [ ] Routage des pistes
- [ ] Vérification DRC/ERC
- [ ] Génération des fichiers Gerber
- [ ] Prototype et tests
- [ ] Firmware de base
- [ ] Documentation utilisateur

##  Notes de conception

### Considérations importantes
- Le STM32H743 offre des performances exceptionnelles pour les algorithmes de vol
- Le double IMU (ICM42688P + MPU6500) permet une redondance et un filtrage avancé
- L'intégration du récepteur ExpressLRS réduit le poids et améliore la fiabilité
- Le baromètre SPL06-001 est particulièrement adapté aux drones
- L'OSD AT7456E est compatible avec les systèmes vidéo analogiques standard

### Améliorations futures possibles
- Support pour capteurs additionnels (distance, optique)
- Interface CAN pour périphériques avancés
- Connecteurs JST-GH standardisés
- Régulateur BEC pour caméra FPV
- Support pour moteurs high-KV

##  Contribution

Ce projet est en développement actif. Les contributions, suggestions et retours sont les bienvenus !

##  Licence

*(À définir - suggéré: Open Source Hardware sous licence CERN-OHL ou similaire)*

##  Auteur

Maxim - [maks7d](https://github.com/maks7d)

##  Ressources

### Datasheets
- [STM32H743VI](https://www.st.com/resource/en/datasheet/stm32h743vi.pdf)
- [ICM42688P](https://invensense.tdk.com/products/motion-tracking/6-axis/icm-42688-p/) - TDK InvenSense
- [SPL06-001](https://www.infineon.com/cms/en/product/sensor/pressure-sensors/pressure-sensors-for-iot/spl06-001/) - Infineon
- [AT7456E](http://www.artosyn.com/) - Artosyn OSD

### Firmwares compatibles
- [Betaflight](https://github.com/betaflight/betaflight) - Le firmware FPV le plus populaire
- [INAV](https://github.com/iNavFlight/inav) - Navigation et GPS
- [ArduPilot](https://ardupilot.org/) - Plateforme complète

### Communauté
- [ExpressLRS](https://www.expresslrs.org/) - Documentation du système RC
- [KiCad Forum](https://forum.kicad.info/) - Support KiCad

---

**Version:** 0.1-alpha  
**Dernière mise à jour:** Novembre 2025
