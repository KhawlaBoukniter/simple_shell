# Simple Shell

![Shell Banner](https://github.com/iAmG-r00t/simple_shell/assets/125459327/bb784c5f-9521-4265-8ac8-9bf72a8396eb)

## 📧 Description

Ce projet est une implémentation d'un interpréteur de commandes UNIX (shell) développé en langage C. Il reproduit les fonctionnalités essentielles d'un shell comme `sh`, permettant l'exécution de commandes, la gestion des processus, et l'interaction avec le système d'exploitation via des appels système. Ce shell a été créé dans le cadre d'un projet pédagogique pour comprendre les concepts avancés de programmation système en C.

## ✨ Fonctionnalités

Le Simple Shell supporte les fonctionnalités suivantes :

### Exécution de commandes
- **Commandes externes** : Exécution de programmes via `fork()` et `execve()`
- **Gestion du PATH** : Recherche automatique des commandes dans les répertoires du PATH
- **Mode interactif** : Invite de commande `$ ` pour une utilisation en temps réel
- **Mode non-interactif** : Lecture depuis stdin pour l'exécution de scripts

### Commandes intégrées (Builtins)
- `exit [status]` : Quitter le shell avec un code de retour optionnel
- `env` : Afficher les variables d'environnement
- `setenv VARIABLE VALUE` : Définir une variable d'environnement
- `unsetenv VARIABLE` : Supprimer une variable d'environnement
- `cd [DIRECTORY]` : Changer de répertoire (supporte `cd -` pour revenir au répertoire précédent)
- `help` : Afficher l'aide
- `history` : Afficher l'historique des commandes
- `alias [name[='value'] ...]` : Gérer les alias de commandes

### Opérateurs et chaînage de commandes
- **Point-virgule (`;`)** : Exécution séquentielle de commandes
- **ET logique (`&&`)** : Exécution conditionnelle (si commande précédente réussit)
- **OU logique (`||`)** : Exécution conditionnelle (si commande précédente échoue)

### Remplacement de variables
- **`$?`** : Code de retour de la dernière commande exécutée
- **`$$`** : PID du shell actuel
- **`$VARIABLE`** : Expansion des variables d'environnement

### Gestion des erreurs
- Messages d'erreur détaillés (commande non trouvée, permission refusée, etc.)
- Codes de retour appropriés (127 pour commande introuvable, 126 pour permission refusée)
- Gestion des erreurs d'allocation mémoire

### Autres fonctionnalités
- **Gestion de la mémoire** : Allocation dynamique et libération appropriée
- **Historique** : Sauvegarde des commandes dans `.simple_shell_history`
- **Gestion des commentaires** : Support des commentaires avec `#`
- **Parsing avancé** : Tokenisation et analyse des arguments

## 🔧 Compilation & Exécution

### Prérequis
- Compilateur GCC
- Système d'exploitation Unix/Linux
- Bibliothèques standard C

### Compilation
Pour compiler le shell, utilisez la commande suivante :

```bash
gcc -Wall -Werror -Wextra -pedantic -std=gnu89 *.c -o hsh
```

### Exécution

**Mode interactif :**
```bash
./hsh
```

Le shell affichera l'invite `$ ` et attendra vos commandes.

**Mode non-interactif :**
```bash
echo "ls -l" | ./hsh
```

ou

```bash
cat commands.txt | ./hsh
```

## 💻 Exemples d'utilisation

### Exécution de commandes simples
```bash
$ ls -la
$ pwd
$ echo "Hello, World!"
```

### Utilisation des builtins
```bash
$ env
$ cd /tmp
$ cd -
$ setenv MY_VAR "Hello"
$ unsetenv MY_VAR
$ exit 98
```

### Chaînage de commandes
```bash
$ ls ; pwd ; echo "Done"
$ mkdir test && cd test && touch file.txt
$ cd /nonexistent || echo "Directory not found"
```

### Remplacement de variables
```bash
$ echo "Exit status: $?"
$ echo "Shell PID: $$"
$ setenv USER "student"
$ echo "User is $USER"
```

### Utilisation d'alias
```bash
$ alias ls="ls -l"
$ alias ll="ls -la"
$ ls
```

### Historique
```bash
$ history
```

## 📂 Structure du projet

```
simple_shell/
├── shell.h              # Fichier d'en-tête principal avec structures et prototypes
├── hsh.c                # Boucle principale du shell et recherche de commandes
├── shell.c              # Point d'entrée (fonction main)
├── builtemul.c          # Implémentation des builtins (exit, cd, help)
├── builtemul_.c         # Builtins additionnels (history, alias)
├── chain.c              # Gestion des opérateurs logiques (&&, ||, ;)
├── env1.c / env2.c      # Gestion des variables d'environnement
├── getline.c            # Lecture et parsing des entrées utilisateur
├── path.c               # Recherche de commandes dans le PATH
├── func1.c / func2.c / func3.c  # Fonctions utilitaires (strings)
├── list.c / liststr.c   # Gestion des listes chaînées
├── mem1.c / mem2.c      # Gestion de la mémoire
├── moore.c / more.c     # Fonctions additionnelles
├── info.c               # Gestion de la structure d'information
├── iofunc.c             # Fonctions d'entrée/sortie
├── errstr.c             # Gestion des erreurs
├── AUTHORS              # Liste des contributeurs
├── man_1_simple_shell   # Page de manuel
└── README.md            # Ce fichier
```

### Fichiers clés

- **`shell.h`** : Définit les structures de données (`info`, `list`, `built_table`) et tous les prototypes de fonctions
- **`hsh.c`** : Contient la boucle principale `h()`, la recherche de builtins `_builtin()`, et l'exécution de commandes `forkcmd()`
- **`chain.c`** : Implémente la détection et le traitement des opérateurs `&&`, `||`, et `;`
- **`getline.c`** : Gère la lecture de l'entrée utilisateur et le buffering
- **`path.c`** : Recherche les exécutables dans les répertoires du PATH

## 🛠️ Notes techniques

### Architecture
Le shell utilise une architecture modulaire avec :
- Structure `info` centrale contenant tous les contextes d'exécution
- Listes chaînées pour l'environnement, l'historique et les alias
- Séparation claire entre parsing, recherche et exécution

### Gestion des processus
- **fork()** : Création d'un processus enfant pour chaque commande
- **execve()** : Remplacement du processus enfant par le programme cible
- **wait()** : Attente de la fin du processus enfant et récupération du code de retour

### Gestion de la mémoire
- Allocation dynamique avec `malloc()` et libération avec `free()`
- Fonction `__realloc()` personnalisée pour redimensionner les buffers
- Fonction `freeinf()` pour libérer toutes les ressources de la structure `info`
- Prévention des fuites mémoire via une gestion rigoureuse

### Parsing et tokenisation
- Fonction `str_tow()` pour découper les chaînes en tableaux de tokens
- Support des délimiteurs multiples (espaces, tabulations, retours)
- Gestion des chaînes vides et des arguments multiples

### Gestion du PATH
- Parcours séquentiel des répertoires du PATH
- Test d'exécutabilité avec `stat()` et vérification des permissions
- Support des chemins absolus et relatifs

### Historique et persistance
- Sauvegarde automatique dans `.simple_shell_history`
- Limite de 5000 entrées (MAX_HIS)
- Lecture au démarrage et écriture à la sortie

## 🚀 Améliorations futures

- Implémenter la redirection d'entrée/sortie (`<`, `>`, `>>`)
- Ajouter le support des pipes (`|`)
- Compléter l'implémentation de la commande `help`
- Ajouter la complétion automatique avec la touche Tab
- Support des variables locales et des fonctions shell
- Améliorer la gestion des guillemets et échappements

## 📖 Documentation

Consultez la page de manuel pour plus d'informations :
```bash
man ./man_1_simple_shell
```

## 👥 Auteurs

- **Khawla Boukniter** - [boukniter.khawla@gmail.com](mailto:boukniter.khawla@gmail.com)
- **Mohamed Hossam Baiz** - [baizmohamedhossam@gmail.com](mailto:baizmohamedhossam@gmail.com)

## 📚 Ressources

- [Unix shell](https://en.wikipedia.org/wiki/Unix_shell)
- [Thompson shell](https://en.wikipedia.org/wiki/Thompson_shell)
- [Ken Thompson](https://en.wikipedia.org/wiki/Ken_Thompson)

---

**Projet réalisé dans le cadre de la formation ALX Africa Software Engineering**
