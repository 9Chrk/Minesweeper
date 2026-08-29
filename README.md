# Demineur

![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue)

Une implémentation en ligne de commande du jeu du Démineur, écrite en Python. Le joueur révèle progressivement les cases d'une grille tout en évitant les mines, placées aléatoirement après le premier coup.

**Auteur :** Jawad Cherkaoui — matricule 576517

## Fonctionnalités

- Création d'une grille dont le nombre de lignes, de colonnes et de mines est fourni au lancement.
- Placement aléatoire des mines après le premier dévoilement ; la case initiale et ses voisines ne reçoivent pas de mine.
- Affichage en terminal d'une grille colorée avec coordonnées.
- Dévoilement des cases et propagation automatique autour des zones sans mine adjacente.
- Pose et retrait de drapeaux sur les cases non explorées.
- Détection de la victoire ou de la défaite.

## Prérequis

- Python 3.10 ou version ultérieure.

Le jeu utilise uniquement la bibliothèque standard de Python.

## Installation

```bash
git clone https://github.com/9Chrk/Demineur.git
cd Demineur
```

## Lancement

Lancez une partie en indiquant, dans l'ordre, le nombre de lignes, le nombre de colonnes et le nombre de mines :

```bash
python demineur.py 10 10 20
```

Le premier coup doit être un dévoilement. À chaque tour, saisissez une action suivie des coordonnées de la case :

```text
c x y
f x y
```

- `c x y` dévoile la case située à la ligne `x` et à la colonne `y`.
- `f x y` pose un drapeau sur une case non explorée, ou le retire si un drapeau y est déjà présent.

Les coordonnées commencent à `0`. Le joueur gagne en dévoilant toutes les cases sans mine ou en plaçant des drapeaux sur toutes les mines. Il perd en dévoilant une mine.

> Le programme utilise des séquences ANSI pour les couleurs du terminal. Leur rendu peut varier selon le système et le terminal employés.

## Tests

Le fichier `demineur_test.py` contient des tests automatisés avec `pytest`. Si `pytest` est disponible dans votre environnement, exécutez :

```bash
pytest -v demineur_test.py
```

## Structure du projet

```text
Demineur/
├── demineur.py         # Logique et interface en ligne de commande du jeu
├── demineur_test.py    # Tests automatisés avec pytest
├── Projet Demineur.pdf # Document PDF associé au projet
├── LICENSE             # Licence MIT
└── README.md
```

## Document associé

- [Projet Demineur.pdf](Projet%20Demineur.pdf)

## Licence

Ce projet est distribué sous licence [MIT](LICENSE).
