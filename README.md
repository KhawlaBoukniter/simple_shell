# Simple Shell

![Shell Banner](https://github.com/iAmG-r00t/simple_shell/assets/125459327/bb784c5f-9521-4265-8ac8-9bf72a8396eb)

## 📧 Description

Ce projet est une implémentation logicielle d'un interpréteur de commandes UNIX (shell) développé intégralement en langage C. Il reproduit les fonctionnalités fondamentales d'un shell tel que `sh`, assurant l'exécution de binaires, la gestion des processus et l'interaction directe avec le noyau via des appels système. Réalisé dans un cadre pédagogique, ce projet démontre une compréhension approfondie de la programmation système, de la gestion mémoire et de l'architecture logicielle modulaire.

## ✨ Fonctionnalités

Le Simple Shell intègre les capacités suivantes :

### Exécution de commandes
- **Commandes externes** : Recherche et exécution de programmes via `fork()` et `execve()`.
- **Gestion du PATH** : Localisation automatique des exécutables dans les répertoires définis par la variable d'environnement `PATH`.
- **Mode interactif** : Interface en ligne de commande avec invite `$ ` pour une interaction directe.
- **Mode non-interactif** : Capacité à lire et exécuter des commandes depuis l'entrée standard (stdin) ou des fichiers.

### Commandes intégrées (Builtins)
- `exit [status]` : Quitter le shell proprement avec un code de retour spécifié.
- `env` : Affichage exhaustif des variables d'environnement actuelles.
- `setenv VARIABLE VALUE` : Définition ou modification d'une variable d'environnement.
- `unsetenv VARIABLE` : Suppression d'une variable d'environnement existante.
- `cd [DIRECTORY]` : Navigation dans l'arborescence (supporte `cd -` pour le retour au répertoire précédent).
- `help` : Accès à la documentation intégrée (en cours de développement).
- `history` : Visualisation de l'historique des commandes saisies.
- `alias [name[='value'] ...]` : Création et gestion de raccourcis personnalisés.

### Opérateurs et chaînage
- **Point-virgule (`;`)** : Exécution séquentielle simple.
- **ET logique (`&&`)** : Exécution conditionnée par le succès de la commande précédente.
- **OU logique (`||`)** : Exécution conditionnée par l'échec de la commande précédente.

### Remplacement de variables & Parsing
- **Variables spéciales** : Support de `$?` (code de retour) et `$$` (PID du shell).
- **Expansion** : Remplacement dynamique des variables d'environnement (ex: `$USER`).
- **Gestion des commentaires** : Ignorance des lignes ou segments commençant par `#`.
- **Tokenisation robuste** : Analyse syntaxique précise des arguments et délimiteurs.

## 🔧 Compilation & Exécution

### Prérequis
- Compilateur GCC
- Environnement Unix-like (Linux, macOS, WSL)

### Compilation
Utilisez les flags de rigueur pour garantir la conformité et la sécurité du code :
```bash
gcc -Wall -Werror -Wextra -pedantic -std=gnu89 *.c -o hsh
```

### Exécution
**Mode interactif :**
```bash
./hsh
```

**Mode non-interactif :**
```bash
echo "ls -l" | ./hsh
```

## 📂 Structure du Projet

```
simple_shell/
├── shell.h              # Définitions des structures (info, list, built_table) et prototypes
├── hsh.c                # Cœur du shell : boucle principale et aiguillage
├── shell.c              # Point d'entrée principal (main)
├── builtemul*.c         # Implémentations des commandes internes (builtins)
├── chain.c              # Logique des opérateurs de contrôle (&&, ||, ;)
├── env*.c               # API de gestion de l'environnement système
├── getline.c            # Gestion optimisée de la lecture et du buffering d'entrée
├── path.c               # Algorithmes de recherche dans le PATH
├── func*.c              # Bibliothèques utilitaires pour la manipulation de chaînes
├── list*.c              # Implémentation de structures de données (listes chaînées)
├── mem*.c               # Gestionnaire de mémoire (allocations, libérations, realloc)
├── info.c               # Initialisation et nettoyage du contexte d'exécution
├── AUTHORS              # Liste des contributeurs officiels
└── man_1_simple_shell   # Documentation technique complète (page de manuel)
```

## ⚠️ Limitations

Bien que robuste, cette version présente certaines limitations :
- **Pipes** : Le chaînage par tubes (`|`) n'est pas encore supporté.
- **Redirections** : Les redirections de flux (`>`, `<`, `>>`) sont absentes.
- **Échappement** : Support partiel des guillemets complexes et des caractères d'échappement.
- **Jobs Control** : Pas de gestion avancée des processus en arrière-plan (fg/bg).

## 👥 Auteurs

- **Khawla Boukniter** - [boukniter.khawla@gmail.com](mailto:boukniter.khawla@gmail.com)
- **Mohamed Hossam Baiz** - [baizmohamedhossam@gmail.com](mailto:baizmohamedhossam@gmail.com)

---
**Projet réalisé dans le cadre de la formation ALX Africa Software Engineering**
