# Demineur

![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue)
![Interface](https://img.shields.io/badge/Interface-terminal-lightgrey)
![Licence MIT](https://img.shields.io/badge/Licence-MIT-green)

Demineur est une implémentation en ligne de commande du célèbre jeu de Démineur, écrite en Python. Une partie se joue sur une grille dont les dimensions et le nombre de mines sont fournis au lancement : il faut dévoiler les cases sûres sans tomber sur une mine.

Les mines ne sont placées qu’après le premier dévoilement. La case de départ et ses huit voisines éventuelles sont donc protégées, puis le terminal affiche la grille, ses coordonnées et les changements d’état au fil des coups. Le projet n’utilise que la bibliothèque standard de Python ; les tests automatisés reposent sur `pytest`.

**Auteur :** Jawad Cherkaoui — matricule 576517

## 📖 Sommaire

- [Fonctionnalités](#-fonctionnalités)
- [Prérequis](#-prérequis)
- [Installation](#-installation)
- [Lancement](#️-lancement)
- [Utilisation](#-utilisation)
- [Architecture](#-architecture)
- [Flux général](#-flux-général)
- [Tests](#-tests)
- [Structure du projet](#-structure-du-projet)
- [Documentation associée](#-documentation-associée)
- [Problèmes fréquents](#-problèmes-fréquents)
- [Licence](#-licence)

## ✨ Fonctionnalités

- **Grille paramétrable** : le nombre de lignes, de colonnes et de mines est choisi au lancement de la partie.
- **Premier coup sécurisé** : les mines sont réparties aléatoirement après le premier dévoilement ; la case choisie et ses voisines ne peuvent pas contenir de mine.
- **Affichage terminal coloré** : la grille est imprimée avec ses coordonnées, des couleurs ANSI pour les nombres, les mines et les drapeaux, ainsi qu’un compteur de drapeaux.
- **Dévoilement en cascade** : lorsqu’une case sans mine adjacente est révélée, les zones vides voisines et leurs bordures numérotées sont dévoilées automatiquement.
- **Gestion des drapeaux** : la commande de marquage pose un drapeau sur une case inexplorée ou le retire s’il est déjà présent, dans la limite du nombre de mines.
- **Fin de partie** : une partie est gagnée lorsque toutes les mines sont marquées exactement, ou lorsque toutes les cases non minées sont dévoilées sans drapeau restant ; elle est perdue en révélant une mine.

## 🧰 Prérequis

- Python **3.10** ou version ultérieure. Le code emploie notamment la syntaxe d’annotations `list[...]` et l’union de types avec `|`.
- Un terminal prenant en charge les séquences ANSI pour bénéficier de l’affichage coloré.

Le jeu lui-même n’a aucune dépendance externe. `pytest` est nécessaire uniquement pour lancer les tests.

## 📦 Installation

Clonez le dépôt puis placez-vous à sa racine :

```bash
git clone https://github.com/9Chrk/Demineur.git
cd Demineur
```

## ▶️ Lancement

Exécutez `demineur.py` en passant, dans cet ordre, le nombre de lignes, le nombre de colonnes et le nombre de mines :

```bash
python demineur.py 10 10 20
```

Le programme attend ensuite obligatoirement un premier dévoilement avant de générer les mines.

## 🎮 Utilisation

À chaque tour, saisissez une action suivie de la ligne et de la colonne ciblées :

```text
c x y
f x y
```

- `c x y` dévoile la case située à la ligne `x` et à la colonne `y`.
- `f x y` pose un drapeau sur une case non explorée, ou le retire si elle est déjà marquée.

Les coordonnées commencent à `0`. Par exemple, `c 2 3` dévoile la case de la troisième ligne et de la quatrième colonne. Le premier coup doit employer `c`.

Les caractères affichés sur la grille correspondent à l’état de chaque case : `.` pour une case encore masquée, `F` pour un drapeau, `X` pour une mine dévoilée et `0` à `8` pour le nombre de mines adjacentes. Une mine révélée met fin à la partie.

## 🧱 Architecture

Le projet est volontairement regroupé dans `demineur.py`. Ce fichier assure à la fois l’interface en terminal, l’état de la partie et les règles du jeu ; `main()` est le point d’entrée exécuté lorsque le fichier est lancé directement.

Deux matrices de chaînes sont maintenues en mémoire. `game_board` représente ce que le joueur voit, tandis que `reference_board` conserve la position des mines et les valeurs numériques réelles. Au premier coup, `init_game()` crée les deux grilles, attend une commande `c`, puis appelle `place_mines()` et `fill_in_board()` avant d’appliquer la propagation depuis la zone de départ.

La logique s’appuie sur `get_neighbors()` pour limiter les huit voisins possibles aux dimensions de la grille. `place_mines()` tire des coordonnées aléatoires distinctes en excluant la zone initiale. `fill_in_board()` parcourt ensuite les mines et incrémente leurs voisins. Enfin, `propagate_click()` utilise une récursion pour étendre le dévoilement depuis les cases ayant la valeur `0`, tout en révélant leurs bordures non minées.

Pendant la boucle de `main()`, `parse_input()` lit et valide les commandes, puis le programme met à jour `game_board`. `check_win()` compare les positions des drapeaux et des mines, ou celles des cases encore masquées et des mines, pour déterminer l’issue de la partie. `print_board()` et `print_game()` assurent le rendu et les messages d’état.

## 🧬 Flux général

```text
arguments de ligne de commande
        ↓
main()
        ↓
init_game() → premier « c x y » → place_mines() → fill_in_board()
        ↓
boucle de jeu : parse_input() → mise à jour de game_board
        ├── c : dévoilement + propagate_click()
        └── f : pose ou retrait d’un drapeau
        ↓
check_win() → victoire, défaite ou tour suivant
```

## 🧪 Tests

Les scénarios de test sont regroupés dans `demineur_test.py`. Ils vérifient notamment le placement déterministe des mines avec une graine aléatoire, l’initialisation de la partie, les deux conditions de victoire, la défaite sur une mine et la reprise après des entrées hors limites ou des commandes invalides.

Si `pytest` est disponible dans l’environnement, lancez :

```bash
pytest -v demineur_test.py
```

Les tests simulent les entrées du joueur et vérifient que `main()` renvoie `1` pour une victoire et `0` pour une défaite.

## 📂 Structure du projet

```text
Demineur/
├── demineur.py         # Point d’entrée, affichage terminal et règles du jeu
├── demineur_test.py    # Tests pytest des fonctions et scénarios de partie
├── Projet Demineur.pdf # Document PDF associé au projet
├── LICENSE             # Texte de la licence MIT
└── README.md           # Documentation du dépôt
```

## 📄 Documentation associée

- [Projet Demineur.pdf](Projet%20Demineur.pdf)

## ❗ Problèmes fréquents

### `IndexError` ou argument manquant au lancement

La commande doit recevoir exactement trois valeurs convertibles en entiers : lignes, colonnes, puis nombre de mines. Utilisez par exemple `python demineur.py 10 10 20`.

### Grille non affichée

`print_board()` n’imprime la grille que lorsque son nombre de lignes et de colonnes est compris entre `4` et `100`. Choisissez des dimensions dans cette plage.

### Lancement bloqué lors du placement des mines

Les mines doivent pouvoir être placées hors de la case du premier coup et de ses voisines. Un nombre de mines trop élevé par rapport aux dimensions de la grille ne laisse pas assez de positions admissibles.

### Couleurs absentes ou caractères de contrôle visibles

L’affichage s’appuie sur des séquences ANSI. Utilisez un terminal compatible ANSI si les couleurs ne sont pas correctement rendues.

### `pytest: command not found`

Les tests nécessitent `pytest`, qui n’est pas une dépendance du jeu lui-même. Installez-le dans votre environnement Python avant d’exécuter la commande de test.

## 📜 Licence

Ce projet est distribué sous licence [MIT](LICENSE).
