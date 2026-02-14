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

### Android Verified Boot (AVB)
- Version 2.0 de Verified Boot, plus moderne et flexible.
- Comme passer d'une serrure mécanique à un système de sécurité électronique programmable.

## 3. Mesures défensives
- Réseau isolé pour éviter toute communication non contrôlée
- Données fictives uniquement pour éliminer tout risque de fuite réelle
- Device/AVD dédié exclusivement aux tests de sécurité
- Snapshots ou wipe en fin de séance pour ne laisser aucune trace
- Journal de configuration détaillé pour assurer la reproductibilité
- Aucun compte personnel pour éviter tout mélange de données
- Contrôle strict des APK installées pour limiter les risques
- Horodatage + captures des étapes pour une traçabilité complète

> Ces mesures défensives fonctionnent comme les protocoles de sécurité dans un laboratoire manipulant des substances dangereuses : isolement, équipement dédié, procédures de décontamination, et documentation rigoureuse.

## 4. MASVS (2 exigences)
- **STORAGE-1 :** Les données sensibles comme les API keys, mots de passe ou tokens doivent être stockées de manière sécurisée en utilisant des fonctions de chiffrement appropriées.
- **NETWORK-1 :** Les communications réseau doivent utiliser TLS avec une configuration correcte et vérifier les certificats.

## 5. MASTG (2 idées de tests)
- Vérifier si les fichiers de préférences partagées contiennent des informations sensibles en clair (`/data/data/[package_name]/shared_prefs/`)
- Analyser les logs de l'application avec `adb logcat` pour détecter des fuites d'informations sensibles pendant l'exécution
## 6.Remise à zéro AVD
<img width="374" height="399" alt="image" src="https://github.com/user-attachments/assets/db3d1d8f-77d2-44b0-aebb-dff983c1aa2d" />

