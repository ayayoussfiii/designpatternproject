<div align="center">

# 🌌 Tetris Galaxy Dream

**Une réimagination moderne et galactique du Tetris classique**

*Projet académique — Module Design Patterns · ENSA Fès · 2025*

<br>

![Java](https://img.shields.io/badge/Java-17+-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![JavaFX](https://img.shields.io/badge/JavaFX-17+-4A90D9?style=for-the-badge&logo=java&logoColor=white)
![Patterns](https://img.shields.io/badge/Design%20Patterns-6%20GoF-8A2BE2?style=for-the-badge)
![SOLID](https://img.shields.io/badge/Architecture-SOLID-27AE60?style=for-the-badge)
![License](https://img.shields.io/badge/License-Academic-lightgrey?style=for-the-badge)

<br>

> *Un Tetris qui n'est pas qu'un jeu — c'est une démonstration vivante de l'ingénierie logicielle.*

</div>

---

##  Présentation

**Tetris Galaxy Dream** est une implémentation complète du jeu classique Tetris, repensée avec un thème visuel immersif *Galaxy Dream* et une architecture logicielle rigoureuse.

Développé dans le cadre du module **Design Patterns** (Cycle Ingénieur, IA & Confiance Numérique — ENSA Fès), ce projet illustre comment les **patterns du Gang of Four (GoF)** et les **principes SOLID** permettent de construire un code modulaire, extensible et maintenable — en partant d'un problème concret et ludique.

**Ce que vous trouverez ici :**
- 6 design patterns GoF appliqués avec intention, pas mécaniquement
- Une interface graphique JavaFX avec ghost piece, blocs spéciaux et effets visuels
- Une architecture en packages bien séparés, testable et évolutive
- Une application complète des principes SOLID, justifiée cas par cas

---

##  Fonctionnalités

| Fonctionnalité | Description |
|---|---|
|  **Ghost Piece** | Projection semi-transparente de la position de chute pour aider le joueur |
|  **Blocs Spéciaux** | Pièces décorées avec des comportements uniques (explosion, glow...) |
|  **Thème Galaxy Dream** | Effets visuels CSS custom, palette spatiale, animations fluides |
|  **Score & Niveaux** | Progression dynamique avec accélération de la difficulté |
|  **Gestion d'états** | Transitions fluides entre Menu, Playing, Paused et Game Over |
|  **Architecture modulaire** | 6 packages indépendants, chacun encapsulant un pattern distinct |

---

##  Architecture & Design Patterns

### Structure des packages

```
src/main/java/
├── core/           → Logique centrale : GameBoard, GameLoop, Tetromino, GhostPiece
├── composite/      → Pattern Composite — composition hiérarchique des blocs
├── decorators/     → Pattern Decorator — comportements spéciaux à la volée
├── states/         → Pattern State — machine à états du jeu
├── factory/        → Pattern Factory — génération découplée des pièces
└── utils/          → Utilitaires partagés : constantes, ScoreManager (Singleton)
```

---

###  Factory Pattern — `factory/`

Découple la création des pièces Tetromino du reste de la logique. Le client ne connaît pas les classes concrètes — il demande un type, la factory s'occupe du reste.

```java
// Avant : couplage fort, création manuelle partout
Tetromino piece = new TShape(x, y, color);

// Après : création centralisée et découplée
Tetromino piece = TetrominoFactory.create(TetrominoType.T_SHAPE);
```

**Bénéfice :** Ajouter une nouvelle pièce ne nécessite qu'une entrée dans l'enum `TetrominoType` et sa factory — aucune modification du code client.

---

###  Decorator Pattern — `decorators/`

Ajoute des comportements aux blocs (effets visuels, explosions) sans modifier les classes de base ni utiliser l'héritage. Les décorateurs s'empilent librement.

```java
// Empilement de comportements à l'exécution
Tetromino special = new ExplosiveDecorator(
                        new GlowDecorator(
                            TetrominoFactory.create(TetrominoType.L_SHAPE)
                        )
                    );
```

**Bénéfice :** Chaque comportement est isolé, testable séparément, et combinable sans explosion combinatoire de sous-classes.

---

###  Composite Pattern — `composite/`

Traite les blocs simples (`SingleBlock`) et les pièces composées (`CompositeBlock`) de façon uniforme via l'interface `BlockComponent`. La logique de rendu et de collision n'a pas besoin de distinguer les deux.

```
BlockComponent (interface)
├── SingleBlock      → un carré unitaire
└── CompositeBlock   → une pièce = collection de SingleBlocks
```

**Bénéfice :** La récursivité structurelle permet d'introduire des pièces complexes (multi-blocs, formes non standard) sans toucher au moteur de jeu.

---

###  State Pattern — `states/`

Chaque état du jeu est une classe autonome implémentant l'interface `GameState`. Le contexte (`GameLoop`) délègue le comportement à l'état courant — aucun `if/switch` géant.

```
MenuState
    │
    └──► PlayingState ◄──► PausedState
              │
              └──► GameOverState ──► MenuState
```

```java
// Le contexte ne sait pas dans quel état il est — il délègue
gameContext.setState(new PlayingState(gameContext));
gameContext.handleInput(KeyCode.P); // → transition vers PausedState
```

**Bénéfice :** Ajouter un état (ex. `HighScoreState`) = créer une classe, pas modifier les existantes.

---

###  Observer Pattern — `core/`

Le `GameBoard` (sujet observable) notifie automatiquement les composants d'interface graphique (`GameRenderer`, `ScoreDisplay`) à chaque changement d'état. Aucun couplage direct entre la logique et l'UI.

```java
// Le Board publie — il ne sait pas qui écoute
board.addObserver(renderer);
board.addObserver(scoreDisplay);
board.notifyObservers(); // appelé après chaque tick
```

**Bénéfice :** La logique de jeu reste testable sans JavaFX. L'UI peut être remplacée entièrement sans toucher au core.

---

###  Singleton Pattern — `utils/`

`ScoreManager` et `GameConfig` sont des instances uniques partagées. L'accès au score depuis n'importe quel composant est garanti cohérent.

```java
ScoreManager.getInstance().addPoints(400); // lignes × niveau
int best = ScoreManager.getInstance().getHighScore();
```

**Bénéfice :** Évite les désynchronisations entre plusieurs instances — le score est une vérité unique.

---

##  Principes SOLID

| Principe | Application concrète dans ce projet |
|---|---|
| **S** — Single Responsibility | `GameBoard` gère la grille ; `ScoreManager` gère les points ; `GameRenderer` gère l'affichage. Chaque classe a une seule raison de changer. |
| **O** — Open/Closed | Les blocs sont étendus par Decorator sans modifier `Tetromino`. Nouveaux effets = nouvelles classes uniquement. |
| **L** — Liskov Substitution | Tout `BlockComponent` (simple ou composite) est interchangeable. Tout `Tetromino` décoré se comporte comme l'original. |
| **I** — Interface Segregation | `Renderable`, `Movable`, `Scorable` — interfaces fines. Les classes implémentent ce dont elles ont besoin, pas plus. |
| **D** — Dependency Inversion | `GameLoop` dépend de `GameState` (abstraction), pas de `PlayingState`. Le core dépend d'interfaces, jamais d'implémentations. |

---

##  Installation & Lancement

### Prérequis

- Java **17+**
- JavaFX **17+**
- Maven **3.8+** ou Gradle **7+**

### Cloner et lancer

```bash
# 1. Cloner le dépôt
git clone https://github.com/ayayoussfiii/designpatternproject.git
cd designpatternproject

# 2a. Avec Maven
mvn clean javafx:run

# 2b. Avec Gradle
gradle run
```

### Contrôles

| Touche | Action |
|---|---|
| `←` `→` | Déplacer la pièce horizontalement |
| `↓` | Descente accélérée (soft drop) |
| `↑` ou `Z` | Rotation |
| `Espace` | Chute instantanée (hard drop) |
| `P` | Pause / Reprendre |
| `Échap` | Retour au menu principal |

---

## 📁 Structure complète du projet

```
designpatternproject/
├── src/
│   └── main/
│       ├── java/
│       │   ├── core/
│       │   │   ├── GameBoard.java          → Grille, détection collisions, suppression lignes
│       │   │   ├── GameLoop.java           → Boucle principale, gestion du temps
│       │   │   ├── Tetromino.java          → Pièce abstraite de base
│       │   │   └── GhostPiece.java         → Calcul et rendu de la projection
│       │   ├── composite/
│       │   │   ├── BlockComponent.java     → Interface commune (Composite Pattern)
│       │   │   ├── SingleBlock.java        → Bloc unitaire
│       │   │   └── CompositeBlock.java     → Assemblage de blocs
│       │   ├── decorators/
│       │   │   ├── TetrominoDecorator.java → Décorateur abstrait de base
│       │   │   ├── GlowDecorator.java      → Effet lumineux
│       │   │   └── ExplosiveDecorator.java → Comportement explosif
│       │   ├── states/
│       │   │   ├── GameState.java          → Interface État
│       │   │   ├── MenuState.java
│       │   │   ├── PlayingState.java
│       │   │   ├── PausedState.java
│       │   │   └── GameOverState.java
│       │   ├── factory/
│       │   │   ├── TetrominoFactory.java   → Création centralisée des pièces
│       │   │   └── TetrominoType.java      → Enum des types disponibles
│       │   └── utils/
│       │       ├── Constants.java          → Dimensions grille, vitesses, couleurs
│       │       └── ScoreManager.java       → Singleton — gestion du score et high score
│       └── resources/
│           ├── styles/
│           │   └── galaxy-theme.css        → Thème visuel Galaxy Dream
│           └── assets/                     → Images, sons, sprites
├── pom.xml
└── README.md
```

---

##  Contexte académique

Ce projet a été réalisé dans le cadre du module **Design Patterns** du Cycle Ingénieur, spécialité **IA & Confiance Numérique**, à l'**ENSA Fès** (2024–2025).

**Objectifs pédagogiques :**
- Maîtriser les patterns GoF (Gang of Four) et comprendre *pourquoi* les utiliser, pas seulement *comment*
- Concevoir une architecture logicielle robuste face au changement
- Appliquer les principes SOLID sur un projet concret et non trivial
- Développer une application GUI complète avec JavaFX

---

## Auteure

<div align="center">

**Aya YOUSSFI**
Étudiante Ingénieure — IA & Confiance Numérique
ENSA Fès · 2024–2026

[![GitHub](https://img.shields.io/badge/GitHub-ayayoussfiii-181717?style=flat-square&logo=github)](https://github.com/ayayoussfiii)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-aya--youssfi-0A66C2?style=flat-square&logo=linkedin)](https://linkedin.com/in/aya-youssfi)

</div>

---

<div align="center">
<sub>✨ Galaxy Dream Edition · ENSA Fès 2025 ✨</sub>
</div>
