# FCH7 - Flight Controller pour Drone Quadcopter

##  Vue d'ensemble

FCH7 est un projet visant à développant un contrôleur de vol autour du MCU STM32H743VITx (flight controller) pour drones de 4 à 8 moteurs, conçu avec KiCad.

> ** Statut du projet:** Ce projet est actuellement en cours de développement. Les schématiques sont en phase de conception et le PCB n'est pas encore finalisé.

##  Caractéristiques principales

### Microcontrôleur
- **STM32H743VITx** - Processeur ARM Cortex-M7 cadencé à 480 MHz
  - 2048 KB de mémoire Flash
  - 1024 KB de RAM
  - 82 GPIO disponibles
  - Package LQFP100

### Capteurs embarqués
- **ICM42688P** - IMU moderne (gyroscope + accéléromètre)
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
  - Rails multiples: VDD_MCU, VDD_RECV, 4V5, VDD_GYRO

### Stockage et LEDs
- **Blackbox** - Interface SDIO pour carte microSD
  - Enregistrement des données de vol pour analyse
- **LEDs WS2812** (adressables) - Indicateurs visuels programmables
- **LEDs de statut** - Multiples LEDs pour diagnostic

##  Architecture du projet

Le projet est organisé en modules hiérarchiques distincts:

```
Structure du dossier
Le numéro de série du volume est D4CE-14CE
C:.
│   .gitignore
│   fch7.kicad_pcb
│   fch7.kicad_prl
│   fch7.kicad_pro
│   fclib.bak
│   fclib.kicad_sym
│   fp-info-cache
│   fp-lib-table
│   README.md
│   sym-lib-table     
├───Hardware
│   ├───3D
│   │       CSTNE8M00G55Z000R0.stp
│   │       ICM-42688-P.stp
│   │       ME6217C33M5G.step
│   │       MP9943AGQ-Z.stp
│   │       MPU-6500.stp
│   │       STM32H743VIT6.stp
│   │
│   ├───Footprints
│   │   └───fch7_fp.pretty
│   │           AP2112K-1.8TRG1.kicad_mod
│   │           CONN-SMD_HDGC1002WR-S-6P.kicad_mod
│   │           CONN-SMD_HDGC1002WR-S-8P.kicad_mod
│   │           CRYSTAL-SMD_4P-L2.0-W1.6-BL.kicad_mod
│   │           CSNP1GCR01-BOW.kicad_mod
│   │           CSTNE8M00G55Z000R0.kicad_mod
│   │           IIM42352.kicad_mod
│   │           LED0603-R-RD_RED.kicad_mod
│   │           LGA-16_L3.0-W3.0-P0.50-BL_SQ.kicad_mod
│   │           LQFP-100_14x14mm_P0.5mm_STM32H743.kicad_mod
│   │           QFN40P300X300X95-25M-D.kicad_mod
│   │           RES-ARRAY-SMD_0603-8P-L3.2-W1.6-BL.kicad_mod
│   │           SENSOR-SMD_SPL06-001.kicad_mod
│   │           SM03B-SRSS-TB(LF)(SN).kicad_mod
│   │           SolderPad_1x01_SMD_2.2x1.6mm.kicad_mod
│   │           SON65P300X300X100-8N.kicad_mod
│   │           SOT23-5.kicad_mod
│   │           STM32H743VIT6.mod
│   │           SW-SMD_2P-TS24CA.kicad_mod
│   │           TSSOP-28_L9.7-W4.4-P0.65-LS6.4-BL-EP-2.kicad_mod
│   │           USB-C-SMD_TYPE-C-6PIN-2MD-073.kicad_mod
│   │           WIFIM-SMD_ESP-01F-2M.kicad_mod
│   │           WIRELM-SMD_E28-2G4M27SX.kicad_mod
│   └───Symbols
│       └───fch7_sym
│               CSTNE8M00G55Z000R0.bak
│               CSTNE8M00G55Z000R0.kicad_sym
│               fch7_sym.bak
│               fch7_sym.kicad_sym
│               ICM-42688-P.kicad_sym
│               ME6217C33M5G.bak
│               ME6217C33M5G.kicad_sym
│               MP9943AGQ-Z.bak
│               MP9943AGQ-Z.kicad_sym
│               MPU-6500.kicad_sym
│               STM32H743VIT6.kicad_sym
└───Schematic
        blackbox_led.kicad_sch
        connectors.kicad_sch
        ERC.rpt
        fch7.csv
        fch7.kicad_sch
        fp-info-cache
        MCU.kicad_sch
        Power.kicad_sch
        rf_rx.kicad_sch
        sensors.kicad_sch
        ~fch7.kicad_sch.lck
```

##  Composants personnalisés

Le projet utilise une bibliothèque de symboles personnalisés (`fclib.kicad_sym`) contenant:
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
- Rails régulés: 3.3V, 4.5V, VDD_MCU, VDD_RECV, VDD_GYRO
- Protection contre les inversions de polarité

##  Outils requis

### Logiciels
- **KiCad 9.0** ou supérieur - Conception schématique et PCB
- **Git** - Gestion de version

##  Statut du développement

- [x] Schématique du MCU
- [x] Schématique de l'alimentation
- [x] Schématique des capteurs
- [x] Schématique du récepteur RF
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
