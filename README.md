# LAB-2-Rooting-Android
# FICHE ENVIRONNEMENT – TRAÇABILITÉ TEST SÉCURITÉ ANDROID
## 1️ Informations générales

| Champ | Valeur |
|-------|--------|
| Date | 14.02.2026 |
| Auteur | Mohamed Douassi |
| Application testée | My_Application |
| Version application | v1.0 |
| Support utilisé | AVD Pixel 6 |
| Type d’environnement | Émulateur (AOSP rootée) |
| Version Android | Android 16 |
| API Level | API 36 |
| Objectif du test | Comprendre le processus de rooting et ses impacts sur la sécurité |
| Données utilisées | Données fictives (aucune donnée réelle) |
| Configuration réseau | Réseau isolé (Host-Only) |

## 2 Scénarios testés
### Scénario 1 :les app bien installer
<img width="395" height="853" alt="image" src="https://github.com/user-attachments/assets/1ec4ff3a-9257-4d92-9b0c-f147bbb4eee2" />

### Scénario 2:recherche d'une app
<img width="392" height="851" alt="image" src="https://github.com/user-attachments/assets/663ddad0-3b23-4f2f-a340-02ff56a96bf2" />

### Scénario 3 :l'application bien executer et lancer
<img width="394" height="854" alt="image" src="https://github.com/user-attachments/assets/3e083264-962e-4eed-8430-18edec10cfb9" />

## 3️ Observations factuelles
- Bootloader : Déverrouillé
- État Verified Boot : Orange (système modifié)
- Environnement : Émulateur AOSP rooté
- Aucune donnée réelle utilisée
- Aucun trafic externe détecté (réseau isolé Host-Only)

## 4️ Limites du test
- Test effectué uniquement sur émulateur (pas sur appareil physique)
- Pas d’analyse réseau approfondie (TLS / MITM non testé)
- Application simple (v1.0, fonctionnalités limitées)
- Aucun test d’exploitation avancé réalisé
- 
## app lancer 
<img width="564" height="227" alt="image" src="https://github.com/user-attachments/assets/e86df989-fa0f-4605-a739-e1f7920fe53a" />

## adb root
<img width="564" height="227" alt="image" src="https://github.com/user-attachments/assets/7c47ae74-e226-4b45-bd5d-db5b57fa69b0" />

- **Objectif :** Confirmer l'accès super-utilisateur.
- **Résultat observé :**
- Accès root activé avec succès.
- Shell exécuté en mode root.
  
## resultat :getprop ro.boot.verifiedbootstate
<img width="543" height="52" alt="image" src="https://github.com/user-attachments/assets/ebda1c86-9370-4ee3-adb3-656171f47d91" />

- **Objectif :** Identifier l'état de sécurité du démarrage.
- **Résultat observé :**
- Valeur retournée : `orange`
- Interprétation : Bootloader déverrouillé / système modifié (rooté).

## 5️ Reset / Nettoyage environnement

| Élément | Statut |
|----------|--------|
| Snapshot restauré | Oui |
| Wipe effectué | Oui |
| Appareil réinitialisé | Oui |

## wipe data
<img width="374" height="399" alt="image" src="https://github.com/user-attachments/assets/b72fd534-7cd6-4fd2-b098-e7c188f8a2c8" />

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

##  Boot ROM (Hardware)
- **Clé publique gravée dans le matériel**
- **IMMUTABLE – Fixé en usine**
-  Vérifie la signature du Bootloader

---

##  Bootloader
- Signé par le constructeur
- État : `Locked` / `Unlocked`
- Vérifie la signature du Boot Image

---

##  Boot Image (`boot.img`)
- Contient :
  - Kernel
  - Ramdisk
- Vérifie l’intégrité via **dm-verity**

---

## System Partition (`system.img`)
- Contient le système Android
- Vérifiée par la chaîne de confiance

---

# Résultat du Démarrage

| Couleur | Signification | État |
|----------|---------------|------|
| 🟢 GREEN  | Chaîne vérifiée | Mode sécurisé (Locked) |
| 🟠 ORANGE | Modification détectée | Mode rooté (Unlocked) |
| 🔴 RED    | Échec de vérification | Démarrage bloqué |


##  Détail des niveaux de sécurité

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

## 6.  Remise à zéro AVD
<img width="374" height="399" alt="image" src="https://github.com/user-attachments/assets/24196ddd-528c-4960-a120-cbff07b0e15f" />
