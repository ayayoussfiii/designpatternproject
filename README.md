<div align="center">

# 🌌 Tetris Galaxy Dream

**Une réimagination moderne et galactique du Tetris classique**  
*Projet académique — Module Design Patterns*

![Java](https://img.shields.io/badge/Java-17+-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![JavaFX](https://img.shields.io/badge/JavaFX-17+-blue?style=for-the-badge&logo=java&logoColor=white)
![Design Patterns](https://img.shields.io/badge/Design%20Patterns-6%20patterns-purple?style=for-the-badge)
![SOLID](https://img.shields.io/badge/Architecture-SOLID-green?style=for-the-badge)

</div>

---

##  Aperçu

**Tetris Galaxy Dream** est une implémentation moderne du jeu classique Tetris, développée dans le cadre du module **Design Patterns**. Le projet démontre comment des patterns architecturaux bien appliqués permettent de construire un code **maintenable**, **extensible** et conforme aux **principes SOLID**.

L'interface graphique, réalisée avec **JavaFX**, propose une expérience immersive avec un thème *Galaxy Dream* : effets visuels, blocs spéciaux, ghost piece et bien plus.

---

##  Fonctionnalités

| Fonctionnalité | Description |
|---|---|
| 👻 **Ghost Piece** | Aperçu de la position de chute de la pièce |
| 🔮 **Blocs Spéciaux** | Pièces avec comportements uniques |
| 🌌 **Thème Galaxy Dream** | Interface visuelle immersive avec effets modernes |
| 🎯 **Score & Niveaux** | Système de progression avec difficulté croissante |
| ⏸️ **Gestion des états** | Pause, Game Over, Menu — transitions fluides |
| 🧱 **Architecture modulaire** | 6 packages bien séparés et extensibles |

---

##  Architecture & Design Patterns

Le projet est organisé en **6 packages principaux**, chacun encapsulant un pattern spécifique :

```
src/
├── 📦 core/          → Logique centrale du jeu (Board, Tetromino, GameLoop)
├── 📦 composite/     → Pattern Composite — composition des pièces
├── 📦 decorators/    → Pattern Decorator — blocs spéciaux & effets visuels
├── 📦 states/        → Pattern State — gestion des états (Menu, Play, Pause, GameOver)
├── 📦 factory/       → Pattern Factory — génération des pièces
└── 📦 utils/         → Utilitaires partagés (constantes, helpers)
```

###  Patterns implémentés

#### 🏭 Factory Pattern — `factory/`
> Délègue la création des pièces Tetromino à une factory dédiée, sans exposer la logique d'instanciation.

```java
Tetromino piece = TetrominoFactory.create(TetrominoType.T_SHAPE);
```

#### 🎭 Decorator Pattern — `decorators/`
> Ajoute dynamiquement des comportements aux blocs (effets visuels, comportements spéciaux) sans modifier les classes de base.

```java
Tetromino special = new ExplosiveDecorator(new GlowDecorator(basePiece));
```

####  Composite Pattern — `composite/`
> Traite les pièces simples et composées de manière uniforme — une pièce est une composition de blocs individuels.

####  State Pattern — `states/`
> Chaque état du jeu (Menu, Playing, Paused, GameOver) est une classe indépendante avec ses propres transitions et comportements.

```
MenuState ──► PlayingState ──► PausedState
                    │
                    └──► GameOverState
```

####  Observer Pattern — `core/`
> Le tableau de jeu notifie automatiquement l'interface graphique à chaque changement d'état.

#### 🔁 Singleton Pattern — `utils/`
> Le gestionnaire de configuration et le score manager sont des instances uniques partagées.

---

## 🧩 Principes SOLID respectés

| Principe | Application dans le projet |
|---|---|
| **S** — Single Responsibility | Chaque classe a un rôle unique (Board, Renderer, ScoreManager...) |
| **O** — Open/Closed | Les blocs sont extensibles via Decorator sans modifier le code existant |
| **L** — Liskov Substitution | Tout Tetromino peut être remplacé par un décorateur sans casser le jeu |
| **I** — Interface Segregation | Interfaces fines et ciblées (Renderable, Movable, Scorable...) |
| **D** — Dependency Inversion | Le core dépend d'abstractions, pas d'implémentations concrètes |

---

## 🚀 Lancement du projet

### Prérequis
- Java **17+**
- JavaFX **17+**
- Maven ou Gradle

### Installation

```bash
# Cloner le dépôt
git clone https://github.com/ayayoussfiii/designpatternproject.git
cd designpatternproject

# Compiler et lancer avec Maven
mvn clean javafx:run

# Ou avec Gradle
gradle run
```

### Contrôles

| Touche | Action |
|---|---|
| `←` `→` | Déplacer la pièce |
| `↓` | Descente rapide (soft drop) |
| `↑` ou `Z` | Rotation |
| `Espace` | Chute instantanée (hard drop) |
| `P` | Pause |
| `Échap` | Menu principal |

---

## 📁 Structure détaillée

```
designpatternproject/
├── src/main/java/
│   ├── core/
│   │   ├── GameBoard.java
│   │   ├── GameLoop.java
│   │   ├── Tetromino.java
│   │   └── GhostPiece.java
│   ├── composite/
│   │   ├── BlockComponent.java
│   │   ├── SingleBlock.java
│   │   └── CompositeBlock.java
│   ├── decorators/
│   │   ├── TetrominoDecorator.java
│   │   ├── GlowDecorator.java
│   │   └── ExplosiveDecorator.java
│   ├── states/
│   │   ├── GameState.java (interface)
│   │   ├── MenuState.java
│   │   ├── PlayingState.java
│   │   ├── PausedState.java
│   │   └── GameOverState.java
│   ├── factory/
│   │   ├── TetrominoFactory.java
│   │   └── TetrominoType.java
│   └── utils/
│       ├── Constants.java
│       └── ScoreManager.java
└── src/main/resources/
    ├── styles/galaxy-theme.css
    └── assets/
```

---

## 🎓 Contexte académique

> Projet réalisé dans le cadre du module **Design Patterns** — Cycle Ingénieur, spécialité IA & Confiance Numérique, **ENSA Fès**.

**Objectifs pédagogiques :**
- Maîtriser les patterns GoF (Gang of Four)
- Concevoir une architecture logicielle robuste
- Appliquer les principes SOLID en pratique
- Développer une application GUI complète avec JavaFX

---

## Auteure

<div align="center">

**Aya YOUSSFI**  
Étudiante Ingénieure — IA & Confiance Numérique  
ENSA Fès · 2024–Present

[![GitHub](https://img.shields.io/badge/GitHub-ayayoussfiii-black?style=flat-square&logo=github)](https://github.com/ayayoussfiii)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-aya--youssfi-blue?style=flat-square&logo=linkedin)](https://linkedin.com/in/aya-youssfi)

</div>

---

<div align="center">
<sub>✨ Built with passion · Galaxy Dream Edition · ENSA Fès 2025 ✨</sub>
</div>
