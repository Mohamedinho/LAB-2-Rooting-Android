# LAB-2-Rooting-Android
# Rapport de Sécurité Android

## 1. Rooting Android
- Le root donne les privilèges super-utilisateur sur l’appareil.
- Il modifie les protections et la confiance du système, permettant d’accéder à tout.
- Très utile en laboratoire pour observer certains comportements, mais risqué si mal utilisé.
- Le rooting, c’est comme avoir un passe-partout pour toutes les portes d’un bâtiment : pratique pour le personnel autorisé, dangereux si entre de mauvaises mains.

## 2. Verified Boot / AVB

### Verified Boot
- **Objectif principal :** Garantir que le système qui démarre est celui prévu par le fabricant, sans modifications malveillantes.
- **Chain of trust :** Chaque composant vérifie l’authenticité du suivant avant de lui faire confiance.
- **Importance :** Si le démarrage est compromis, toutes les protections ultérieures peuvent être contournées.

#### Schéma simplifié Verified Boot / AVB
# ANDROID VERIFIED BOOT (AVB)
## Chain of Trust

---

## 🔐 1️⃣ Boot ROM (Hardware)
- **Clé publique gravée dans le matériel**
- **IMMUTABLE – Fixé en usine**
- ✅ Vérifie la signature du Bootloader

---

## 🔓 2️⃣ Bootloader
- Signé par le constructeur
- État : `Locked` / `Unlocked`
- ✅ Vérifie la signature du Boot Image

---

## 🧩 3️⃣ Boot Image (`boot.img`)
- Contient :
  - Kernel
  - Ramdisk
- ✅ Vérifie l’intégrité via **dm-verity**

---

## 📦 4️⃣ System Partition (`system.img`)
- Contient le système Android
- Vérifiée par la chaîne de confiance

---

# 🚀 Résultat du Démarrage

| Couleur | Signification | État |
|----------|---------------|------|
| 🟢 GREEN  | Chaîne vérifiée | Mode sécurisé (Locked) |
| 🟠 ORANGE | Modification détectée | Mode rooté (Unlocked) |
| 🔴 RED    | Échec de vérification | Démarrage bloqué |

---

## 🔗 Résumé de la Chaîne de Confiance





## 🔍 Détail des niveaux de sécurité

| Niveau | Composant | Protection | État |
|--------|-----------|------------|------|
| **1** | Boot ROM | Gravée hardware | 🔒 Immuable |
| **2** | Bootloader | Signature OEM | 🔒 Locked / 🔓 Unlocked |
| **3** | Boot Image | dm-verity | ✅ Vérifié |
| **4** | System | AVB + dm-verity | ✅ Vérifié |

## 🚦 Interprétation des codes couleur

- 🟢 **GREEN** : Démarrage normal, chaîne de confiance intacte
- 🟠 **ORANGE** : Bootloader déverrouillé, modifications possibles
- 🔴 **RED** : Alerte de sécurité, démarrage bloqué
> Chaque étape vérifie la signature de l’étape suivante pour garantir l’intégrité du système.

### Android Verified Boot (AVB)
- Version 2.0 de Verified Boot, plus moderne et flexible.
- Comme passer d'une serrure mécanique à un système de sécurité électronique programmable.

## 3. Mesures défensives
| Risque / Contexte | Mesure défensive |
|------------------|----------------|
| Communication non contrôlée | Réseau isolé |
| Fuite de données réelles | Utiliser uniquement des données fictives |
| Appareil partagé | Device/AVD dédié aux tests de sécurité |
| Traces après tests | Snapshots ou wipe en fin de séance |
| Reproductibilité | Journal de configuration détaillé |
| Mélange de données | Aucun compte personnel utilisé |
| APK non vérifiées | Contrôle strict des APK installées |
| Suivi et traçabilité | Horodatage + captures des étapes |

> Ces mesures défensives fonctionnent comme les protocoles de sécurité dans un laboratoire manipulant des substances dangereuses : isolement, équipement dédié, procédures de décontamination, et documentation rigoureuse.

## 4. MASVS (2 exigences)
- **STORAGE-1 :** Les données sensibles comme les API keys, mots de passe ou tokens doivent être stockées de manière sécurisée en utilisant des fonctions de chiffrement appropriées.
- **NETWORK-1 :** Les communications réseau doivent utiliser TLS avec une configuration correcte et vérifier les certificats.

## 5. MASTG (2 idées de tests)
- Vérifier si les fichiers de préférences partagées contiennent des informations sensibles en clair (`/data/data/[package_name]/shared_prefs/`).
- Analyser les logs de l'application avec `adb logcat` pour détecter des fuites d'informations sensibles pendant l'exécution.


