# [Tansoftware](https://www.tansoftware.com) — Mathématiques pour la programmation de jeux vidéo

[![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png)](https://fr.wikipedia.org/wiki/Fran%C3%A7ais) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE) [![Made with Markdown](https://img.shields.io/badge/Made%20with-Markdown-1f425f.svg)](https://www.markdownguide.org/) [![Topic](https://img.shields.io/badge/Topic-Game%20Math-brightgreen.svg)](https://en.wikipedia.org/wiki/Video_game_graphics) [^1]

> Un cours complet, en français, qui rassemble les fondations mathématiques nécessaires pour comprendre, concevoir et programmer un jeu vidéo moderne — du vecteur 2D jusqu'au pipeline de rendu d'un GPU.

---

## Table des matières

1. [Introduction](#introduction)
2. [Bases des mathématiques](#bases-des-mathématiques)
   - [Coordonnées cartésiennes](#coordonnées-cartésiennes)
     - [Conventions de repère : main droite vs main gauche, Y-up vs Z-up](#conventions-de-repère--main-droite-vs-main-gauche-y-up-vs-z-up)
   - [Précision flottante : ce que tout dev de jeu doit savoir](#précision-flottante--ce-que-tout-dev-de-jeu-doit-savoir)
   - [Aléa et déterminisme](#aléa-et-déterminisme)
   - [Trigonométrie](#trigonométrie)
   - [Vecteurs](#vecteurs)
     - [Magnitude](#magnitude)
     - [Addition et soustraction de vecteurs](#addition-et-soustraction-de-vecteurs)
     - [Multiplication par un scalaire](#multiplication-par-un-scalaire)
     - [Produit scalaire](#produit-scalaire)
     - [Produit vectoriel](#produit-vectoriel)
   - [Interpolation](#interpolation)
   - [Matrices](#matrices)
     - [Addition et soustraction de matrices](#addition-et-soustraction-de-matrices)
     - [Multiplication d'une matrice par un scalaire](#multiplication-dune-matrice-par-un-scalaire)
     - [Multiplication de matrices](#multiplication-de-matrices)
   - [Transformations](#transformations)
     - [Translation](#translation)
     - [Rotation](#rotation)
     - [Quaternions](#quaternions)
     - [Mise à l'échelle (et son cas particulier, l'homothétie)](#mise-à-léchelle-et-son-cas-particulier-lhomothétie)
     - [Cisaillement](#cisaillement)
   - [Géométrie linéaire](#géométrie-linéaire)
     - [Projection](#projection)
     - [Perspective](#perspective)
     - [Transformation de vue](#transformation-de-vue)
     - [Espaces de coordonnées](#espaces-de-coordonnées)
3. [Graphiques informatiques](#graphiques-informatiques)
   - [Graphiques vectoriels et bitmap](#graphiques-vectoriels-et-bitmap)
   - [Résolution et profondeur de couleur](#résolution-et-profondeur-de-couleur)
   - [Espaces de couleur](#espaces-de-couleur)
     - [Linéaire vs sRGB : le piège que tout le monde rencontre](#linéaire-vs-srgb--le-piège-que-tout-le-monde-rencontre)
   - [Formats de fichier d'image](#formats-de-fichier-dimage)
4. [Éclairage et ombres](#éclairage-et-ombres)
   - [Sources de lumière](#sources-de-lumière)
   - [Modèles d'éclairage](#modèles-déclairage)
   - [Ombres](#ombres)
5. [Texture et mappage UV](#texture-et-mappage-uv)
   - [Texture et coordonnées de texture](#texture-et-coordonnées-de-texture)
   - [Mappage UV](#mappage-uv)
6. [Animation](#animation)
   - [Animation par squelette](#animation-par-squelette)
   - [Animation de forme](#animation-de-forme)
   - [Cinématique inverse](#cinématique-inverse)
7. [Physique des jeux](#physique-des-jeux)
   - [Simulation physique](#simulation-physique)
     - [Pas de simulation fixe ≠ pas de frame](#pas-de-simulation-fixe--pas-de-frame)
   - [Détection de collision](#détection-de-collision)
   - [Résolution de collision](#résolution-de-collision)
8. [Intelligence artificielle](#intelligence-artificielle)
   - [Comportement de base](#comportement-de-base)
   - [Navigation](#navigation)
   - [Apprentissage automatique](#apprentissage-automatique)
9. [Réseau et multijoueur](#réseau-et-multijoueur)
   - [Modèles de réseau](#modèles-de-réseau)
   - [Protocoles de communication](#protocoles-de-communication)
   - [Programmation de jeu multijoueur](#programmation-de-jeu-multijoueur)
10. [Techniques avancées](#techniques-avancées)
    - [Génération procédurale et bruit](#génération-procédurale-et-bruit)
    - [Physique des fluides](#physique-des-fluides)
    - [Écrans multiples et fenêtrage](#écrans-multiples-et-fenêtrage)
    - [Intelligence artificielle avancée](#intelligence-artificielle-avancée)
    - [Rendu avancé](#rendu-avancé)
11. [Pipeline de rendu](#pipeline-de-rendu)
    - [Étapes du pipeline](#étapes-du-pipeline)
    - [Culling et occlusion](#culling-et-occlusion)
    - [Shaders](#shaders)
      - [Vertex shaders](#vertex-shaders)
      - [Geometry shaders](#geometry-shaders)
      - [Fragment shaders](#fragment-shaders)
12. [Pour aller plus loin](#pour-aller-plus-loin)

---

## Introduction

> **Avant toute chose**, si vous n'êtes pas familiarisé avec les formules ou notions mathématiques présentées ici, nous vous conseillons de prendre le temps nécessaire pour monter en compétences dans le domaine des mathématiques et de la physique, par exemple sur le site [Khan Academy](https://fr.khanacademy.org/).

Ce dépôt est conçu pour vous fournir une **compréhension approfondie** des concepts mathématiques et des techniques utilisés dans la programmation de jeux 3D et les graphiques informatiques.

Nous allons couvrir une variété de sujets allant des **bases des mathématiques** (vecteurs, matrices, transformations) aux **transformations géométriques**, en passant par l'**éclairage**, la **couleur**, les **projections**, le **rendu 3D**, les **techniques d'optimisation**, la **physique** et la **simulation**.

Vous apprendrez à appliquer ces concepts pour résoudre les problèmes liés à :

- la **géométrie** et aux **transformations** (déplacer, faire tourner, redimensionner) ;
- l'**éclairage** et au **rendu** (calculer la couleur d'un pixel) ;
- la **performance** (n'afficher que ce qui est visible) ;
- la **physique du jeu** (faire bouger les objets, gérer les collisions).

Que vous soyez un développeur expérimenté ou que vous débutiez tout juste, nous espérons que ce dépôt vous aidera à **renforcer vos connaissances** en mathématiques et à **améliorer vos compétences** en programmation de jeux 3D.

[ Retour en haut de page](#table-des-matières)

---

## Bases des mathématiques

Dans cette section, nous explorerons les concepts fondamentaux des mathématiques nécessaires pour la programmation de jeux 3D et les graphiques informatiques. Nous aborderons les **coordonnées cartésiennes**, les **vecteurs**, les **matrices** et les **transformations**.

### Coordonnées cartésiennes

Les coordonnées cartésiennes sont un système de coordonnées permettant de représenter les points dans l'espace à l'aide de nombres réels.

> Les nombres **réels** sont une extension des nombres rationnels qui permettent de représenter toutes les grandeurs physiques, y compris les nombres irrationnels tels que $\pi$ et $\sqrt{2}$.
>
> <img align="center" src="https://2.bp.blogspot.com/-E6UXjmd-37Q/WlEur3M7wtI/AAAAAAAALyQ/KDwmVBLf7CE_VQbHJ3gx-LHjf6aymu6OwCLcBGAs/s640/ob_83e7ec_ensembles.png" alt="Ensembles de nombres" width="420">

En **2D**, l'espace cartésien est un plan composé d'un axe horizontal et d'un axe vertical. Les coordonnées d'un point dans ce plan sont généralement notées $(x, y)$, où $x$ est l'**abscisse** (horizontal) et $y$ est l'**ordonnée** (vertical).

En **3D**, l'espace cartésien est un espace à trois dimensions composé d'un axe horizontal (l'axe des $x$), d'un axe vertical (l'axe des $y$) et d'un axe perpendiculaire à ces deux axes (l'axe des $z$). Les coordonnées d'un point dans cet espace sont généralement notées $(x, y, z)$.

```mermaid
graph LR
A((origine)) --> B((x))
A --> C((y))
A --> D((z))
```

L'espace cartésien est défini par un système de coordonnées cartésiennes, qui utilise des **axes orthogonaux** (des droites perpendiculaires les unes aux autres) et des nombres réels pour définir la position de points dans l'espace.

Fondamentales dans le domaine des jeux vidéo, en particulier pour les jeux en 3 dimensions, elles permettent de représenter et de manipuler les **positions**, les **mouvements** et les **orientations** des objets dans l'espace virtuel.

#### Conventions de repère : main droite vs main gauche, Y-up vs Z-up

Trois choses qu'on néglige souvent à ses dépens :

1. **L'orientation du repère** : *main droite* (right-handed, RH) ou *main gauche* (left-handed, LH). Pliez votre main droite : pouce $= x$, index $= y$, majeur $= z$. Pour un repère gauche, faites pareil avec votre main gauche — l'axe $z$ pointe dans l'autre sens.
2. **L'axe vertical** : *Y-up* (l'axe $y$ pointe vers le haut) ou *Z-up* (l'axe $z$ pointe vers le haut).
3. **Le sens de rotation positif** : antihoraire (mathématiques classiques) ou horaire selon la convention.

Les moteurs et outils ne sont pas d'accord :

| Système                 | Orientation | Vertical         | Notes                                |
| ----------------------- | ----------- | ---------------- | ------------------------------------ |
| OpenGL, Maya, Houdini   | main droite | $Y$ vers le haut | Convention "graphique" historique    |
| DirectX (legacy), Unity | main gauche | $Y$ vers le haut | $z$ pointe vers l'écran              |
| Unreal Engine           | main gauche | $Z$ vers le haut | Hérité du moteur Quake               |
| Blender, 3ds Max, CAO   | main droite | $Z$ vers le haut | Hérité de la convention CAO          |
| glTF, Vulkan, WebGPU    | main droite | $Y$ vers le haut | Standard d'échange moderne           |

**Pourquoi ça compte ?** Quand on importe un modèle Blender (RH, Z-up) dans Unity (LH, Y-up), un asset orienté correctement à l'export apparaît tourné de 90° et reflété par rapport à un axe. Les outils d'import font automatiquement la conversion, mais c'est la source #1 de bugs visuels en pipeline 3D ("ma voiture est sur le toit").

**Conversion Z-up → Y-up** : on permute $y$ et $z$ et on change un signe :

```math
\begin{pmatrix} x' \\ y' \\ z' \end{pmatrix}_\text{Y-up} = \begin{pmatrix} x \\ z \\ -y \end{pmatrix}_\text{Z-up}
```

**Conversion main-droite → main-gauche** : on inverse un seul axe (souvent $z$) — *toutes les rotations doivent alors être inversées* (sinon les rotations apparaissent à l'envers) :

```math
\begin{pmatrix} x' \\ y' \\ z' \end{pmatrix}_\text{LH} = \begin{pmatrix} x \\ y \\ -z \end{pmatrix}_\text{RH}
```

> **Règle de survie.** Quand vous écrivez du code mathématique dans un projet, **annoncez la convention en commentaire** au début du fichier ("Convention : right-handed, Y-up, rotation positive antihoraire vue depuis l'axe positif"). Tous les bugs de rotation inverse, de skybox à l'envers et de normales mal-orientées viennent d'une convention non documentée.

### Précision flottante : ce que tout dev de jeu doit savoir

Les nombres réels n'existent pas en machine. Ce que votre CPU/GPU manipule, ce sont des **flottants IEEE 754**, et leurs limites se révèlent dès qu'on programme un jeu sérieux.

#### Représentation : le format `float` 32 bits

Un `float` (binary32) découpe ses 32 bits en :

- **1 bit** de signe ;
- **8 bits** d'exposant (biaisé de 127) ;
- **23 bits** de mantisse (24 bits effectifs avec le `1` implicite).

La valeur représentée est : $(-1)^s \times 1{.}m \times 2^{e-127}$.

Conséquences pratiques pour le game-dev :

- **Précision relative**, pas absolue. À l'origine, la précision est d'environ $1.2 \times 10^{-7}$ ; à $x = 10\,000$, elle tombe à environ $7.6 \times 10^{-4}$ (~ 1 mm). À $x = 1\,000\,000$ (genre une carte open-world très large), la précision tombe à $6 \cdot 10^{-2}$ (~ 6 cm) : les objets s'animent avec des saccades visibles.
- **L'addition n'est pas associative** : `(a + b) + c ≠ a + (b + c)` en général. Si vous accumulez du `Δt` à chaque frame depuis l'origine, vous accumulez aussi de l'erreur — d'où l'usage du compteur de temps en `double`.
- **`0.1 + 0.2 == 0.3` est faux** : la base 2 ne représente pas exactement les fractions de base 10.

#### Le piège de la comparaison directe

```csharp
// FAUX en général
if (transform.position.y == targetHeight) { ... }

// Correct : comparer à un epsilon
if (Mathf.Abs(transform.position.y - targetHeight) < 1e-4f) { ... }

// Encore mieux : epsilon relatif (utile pour des grandes valeurs)
bool ApproxEqual(float a, float b, float relTol = 1e-5f, float absTol = 1e-7f)
 => MathF.Abs(a - b) <= MathF.Max(absTol, relTol * MathF.Max(MathF.Abs(a), MathF.Abs(b)));
```

> **`Mathf.Approximately` (Unity) utilise un epsilon de l'ordre de $10^{-6}$** : adapté aux objets proches de l'origine, **inutilisable** pour comparer des positions à plusieurs kilomètres. Roulez votre propre comparateur dans ce cas.

#### Catastrophic cancellation

Soustraire deux nombres flottants proches **détruit la précision relative** :

```text
a = 1.234567f          // 7 chiffres significatifs
b = 1.234566f          // 7 chiffres significatifs
a - b = 0.000001f      // 1 seul chiffre significatif !
```

C'est pourquoi le calcul d'une normale de triangle par `cross(b - a, c - a)` est sensible quand les trois sommets sont presque colinéaires : on soustrait des positions très proches.

#### Open-world : la solution du repère flottant

Au-delà de quelques kilomètres en `float`, la solution canonique est de **recentrer le repère sur le joueur** : périodiquement (toutes les `1024` unités, par exemple), on recalcule toutes les positions du monde par rapport au joueur, qui retourne à $(0, 0, 0)$. *Star Citizen*, *Outerra* et *Kerbal Space Program* utilisent cette technique. Alternative : passer en `double` côté CPU et reconvertir en `float` juste avant l'envoi GPU.

### Aléa et déterminisme

Un jeu vidéo a besoin d'aléa **partout** — placement d'arbres dans une forêt, dégâts critiques, mélange du paquet de cartes, génération de niveau infini, bruit pour les textures, comportement d'IA. Mais cet "aléa" doit souvent être **reproductible** :

- **Replay** : rejouer une partie enregistrée doit reproduire les mêmes événements.
- **Multijoueur lockstep** : *Age of Empires*, *StarCraft*, *Factorio* synchronisent les joueurs en n'envoyant que les inputs ; tout le reste (collisions, IA, RNG) doit donner le même résultat sur chaque machine.
- **Génération procédurale** : *Minecraft*, *Terraria*, *No Man's Sky* doivent recréer le même monde à partir d'une **seed** (graine) donnée.

D'où l'usage de **PRNG** (pseudo-random number generators) : des générateurs déterministes qui, à partir d'un état initial, produisent une séquence "qui ressemble à de l'aléatoire".

#### Anatomie d'un PRNG

Un PRNG entretient un **état** $S_n$ et applique une fonction de transition $f$ à chaque appel :

```math
S_{n+1} = f(S_n) \qquad x_n = g(S_n)
```

où $x_n$ est le nombre observable. La **période** est la longueur du cycle $S_0 \to S_1 \to \cdots \to S_0$ avant répétition.

#### Choisir un PRNG

| Algorithme             | État (bits) | Période             | Vitesse | Qualité                | Cas d'usage                          |
| ---------------------- | ----------- | ------------------- | ------- | ---------------------- | ------------------------------------ |
| **LCG**                | 32-64       | $\le 2^{64}$        |    | Médiocre (motifs 2D)   | À éviter, présent par héritage       |
| **Mersenne Twister**   | 19 968      | $2^{19937}-1$       |      | Bonne                  | `rand()` de C++/Python, surpoids RAM |
| **xorshift / xoshiro** | 64-256      | $\ge 2^{128}-1$     |     | Bonne                  | Rust `SmallRng`, GPU-friendly        |
| **PCG**                | 64-128      | $\ge 2^{64}$        |     | Excellente (BigCrush)  | Le défaut moderne (M. O'Neill, 2014) |
| **SplitMix64**         | 64          | $2^{64}$            |    | Bonne                  | Seeder, hachages spatiaux            |

 **Lexique du tableau :**

- **État (bits)** : la mémoire interne du générateur. Plus c'est grand, plus la séquence avant répétition peut être longue.
- **Période** : nombre d'appels avant que la séquence se répète exactement à l'identique.
- **BigCrush** : suite de tests statistiques de référence (TestU01, Pierre L'Ecuyer) utilisée pour disqualifier les PRNG biaisés.
- **LCG** (*Linear Congruential Generator*) : la formule $S_{n+1} = (a \cdot S_n + c) \bmod m$.

> **Aucun PRNG n'est cryptographiquement sûr** par défaut — pour générer un token de session ou simuler un draw "fair" avec mise réelle, utilisez `crypto.randomBytes`/`SecureRandom`. Pour un jeu, c'est inutile et coûteux.

#### Seeder proprement

Une seed mal choisie biaise la séquence. Deux règles :

1. **Ne jamais utiliser `time()` directement comme seed** : sur un cluster multijoueur, deux instances démarrées à la même seconde ont la même seed. Préférez une combinaison `time()`-XOR-`pid()`-XOR-`hash(machineId)`.
2. **Toujours passer la seed à travers SplitMix64** avant de l'utiliser : SplitMix64 distribue uniformément les bits, ce qui évite les corrélations sur des seeds proches (`seed=1` et `seed=2` donneraient sinon des séquences similaires sur certains PRNG).

```python
def splitmix64(x: int) -> int:
 x = (x + 0x9E3779B97F4A7C15) & 0xFFFFFFFFFFFFFFFF
 x = ((x ^ (x >> 30)) * 0xBF58476D1CE4E5B9) & 0xFFFFFFFFFFFFFFFF
 x = ((x ^ (x >> 27)) * 0x94D049BB133111EB) & 0xFFFFFFFFFFFFFFFF
 return x ^ (x >> 31)
```

#### Hachage spatial (PRNG sans état)

Pour un terrain procédural infini, on a besoin d'aléa par cellule **sans entretenir d'état** : `noise(x, y)` doit toujours donner le même résultat sans dépendre de l'ordre d'appel. La technique : un **hachage** `(x, y) → uint32` qu'on découpe en bits :

```csharp
// Hachage spatial déterministe (style PCG)
uint Hash2D(int x, int y, uint seed)
{
 uint h = seed;
 h ^= (uint)x * 0x85EBCA6B;
 h ^= ((uint)y * 0xC2B2AE35) ^ (h >> 16);
 h *= 0x27D4EB2F;
 return h ^ (h >> 16);
}
float Random01(uint h) => (h >> 8) * (1f / (1u << 24));
```

C'est exactement comme ça que *Minecraft* place des arbres : `Hash2D(blockX, blockZ, worldSeed)` détermine le contenu de chaque chunk, sans avoir à mémoriser quoi que ce soit.

#### Distributions au-delà de l'uniforme

Un PRNG donne des nombres uniformes dans $[0, 1[$. Pour autre chose :

- **Loi normale** (cloches autour d'une moyenne) : Box-Muller. Si $u_1, u_2$ sont uniformes, $z = \sqrt{-2 \ln u_1}\,\cos(2\pi u_2)$ suit la loi normale standard. Utile pour bruit gaussien, dispersion réaliste de tirs.
- **Choix pondéré** (loot tables) : on calcule la somme cumulative des poids, on tire $u$ uniforme dans $[0, \text{somme}]$ et on renvoie l'item correspondant. Pour beaucoup d'items, l'**alias method** de Walker fait ça en $O(1)$ après un précalcul $O(n)$.
- **Échantillonnage de Poisson** (placement d'objets sans agglomérat) : *Poisson disk sampling*, l'algorithme de Bridson en $O(n)$. Utilisé dans le placement réaliste d'arbres, étoiles, nuages.

### Trigonométrie

La trigonométrie est l'étude des relations entre les **angles** et les **longueurs** dans un triangle. C'est un outil omniprésent en programmation de jeux : rotation d'un sprite, déplacement d'un projectile, oscillation, calcul d'un angle de tir, etc.

#### Le cercle trigonométrique

Sur le cercle unité (rayon $1$ centré à l'origine), un point repéré par l'angle $\theta$ a pour coordonnées :

```math
(x, y) = (\cos\theta,\ \sin\theta)
```

> ℹ En programmation, les angles sont **presque toujours exprimés en radians** ($2\pi$ rad $= 360°$). La conversion se fait avec : $\theta_\text{rad} = \theta_\text{deg} \times \dfrac{\pi}{180}$.

#### Fonctions trigonométriques

Pour un angle $\theta$ dans un triangle rectangle d'hypoténuse $h$, de côté opposé $o$ et de côté adjacent $a$ :

```math
\sin\theta = \frac{o}{h}, \quad \cos\theta = \frac{a}{h}, \quad \tan\theta = \frac{o}{a} = \frac{\sin\theta}{\cos\theta}
```

#### Identités utiles

```math
\sin^2\theta + \cos^2\theta = 1
```

```math
\sin(\alpha + \beta) = \sin\alpha\cos\beta + \cos\alpha\sin\beta
```

```math
\cos(\alpha + \beta) = \cos\alpha\cos\beta - \sin\alpha\sin\beta
```

#### Exemple : déplacement d'un projectile

Pour tirer un projectile à la vitesse $v$ et à l'angle $\theta$ :

```math
v_x = v \cos\theta, \quad v_y = v \sin\theta
```

```csharp
float angleRad = angleDeg * MathF.PI / 180f;
Vector2 velocity = new Vector2(
 speed * MathF.Cos(angleRad),
 speed * MathF.Sin(angleRad)
);
```

#### atan2 — l'incontournable

Pour retrouver l'angle correspondant à un vecteur $(x, y)$, on utilise `atan2(y, x)` plutôt que `atan(y/x)` — car `atan2` gère correctement les quatre quadrants et le cas $x = 0$ :

```math
\theta = \mathrm{atan2}(y, x) \in (-\pi,\ \pi]
```

C'est l'outil idéal pour faire pivoter un personnage ou une arme vers un point cible.

### Vecteurs

Les vecteurs sont des entités mathématiques représentant à la fois une **magnitude** (longueur) et une **direction**. Ils sont généralement utilisés pour décrire la position, la vitesse, l'accélération et d'autres propriétés dans l'espace 2D ou 3D, dans un espace cartésien.

> Dans un jeu vidéo, on privilégiera un type particulier de repère cartésien : le **repère orthonormé**.

![image](https://user-images.githubusercontent.com/22911157/233814039-82e7aa63-d3dc-498f-ab2c-e19d3385eadf.png)

Son format d'écriture usuel s'exprime par :

```math
V =
\begin{pmatrix}
v_1 \\
v_2 \\
\vdots \\
v_n
\end{pmatrix}
```

où chaque $v_i$ est une **composante** du vecteur $V$ (telle que $v_1$ est la première composante, $v_2$ la deuxième, et ainsi de suite). La matrice représente un vecteur à $n$ composantes, disposées verticalement en une seule colonne.

> Dans le contexte des vecteurs, une composante est un élément constitutif du vecteur qui indique sa valeur le long d'un axe particulier. Un vecteur est défini par un ensemble de composantes, qui ensemble déterminent sa direction et sa magnitude.
>
> Par exemple, un vecteur à deux dimensions a deux composantes ($v_1$ et $v_2$) qui représentent respectivement sa valeur le long des axes $x$ et $y$. De même, un vecteur à trois dimensions a trois composantes ($v_1$, $v_2$ et $v_3$), qui correspondent à sa valeur le long des axes $x$, $y$ et $z$.
>
> Les composantes d'un vecteur permettent de décrire sa position ou sa direction dans un espace à $n$ dimensions, où $n$ est le nombre de composantes du vecteur.

![image](https://user-images.githubusercontent.com/22911157/233814114-59fac9fb-8ac5-421d-8a8a-256f591eef26.png)

#### Magnitude

La magnitude d'un vecteur en dimension $n$ est donnée par la formule suivante :

```math
\left\Vert\mathbf{v}\right\Vert = \sqrt{\sum_{i=1}^{n} v_i^2}
```

> Le symbole $\sum$ est appelé « somme » en mathématiques. Il indique que l'on doit additionner les termes indiqués. Ici, on additionne les carrés de chaque composante du vecteur $\mathbf{v}$.
>
> Le symbole $\left\Vert\mathbf{v}\right\Vert$ représente la magnitude (ou norme) du vecteur $\mathbf{v}$, c'est-à-dire sa longueur ou sa taille.
>
> Les indices $i$ de la somme indiquent qu'on somme les carrés des composantes de $\mathbf{v}$ de $i=1$ jusqu'à $i=n$, où $n$ est la dimension du vecteur. Cela signifie qu'on calcule le carré de la première composante, puis le carré de la deuxième, et ainsi de suite jusqu'à la $n$-ème composante.
>
> Le symbole $v_i$ représente la $i$-ème composante du vecteur $\mathbf{v}$. On élève cette composante au carré en utilisant le symbole $^2$.
>
> Enfin, la racine carrée $\sqrt{\ }$ est appliquée à la somme des carrés pour obtenir la magnitude. L'opération de carré supprime les signes négatifs des composantes (toutes les valeurs deviennent positives), tout en gardant leur contribution proportionnelle à leur magnitude.

Pour un vecteur 2D représenté par les coordonnées $(x, y)$, la magnitude est donnée par :

```math
\left\Vert\mathbf{v}\right\Vert = \sqrt{\sum_{i=1}^{2} v_i^2} = \sqrt{v_1^2 + v_2^2}
```

En effet, dans un espace 2D, $n = 2$ ; dans un espace 3D, $n = 3$, etc.

La magnitude d'un vecteur 2D est donc égale à la racine carrée de la somme des carrés de ses deux composantes — autrement dit, à la longueur de l'**hypoténuse** d'un triangle rectangle dont les côtés adjacents sont les composantes $x$ et $y$ du vecteur.

##### Une représentation possible en C\#

```csharp
using TansoftwareEngine;

public class Game : GameEngine
{
 void Start()
 {
 Vector3 position = new Vector3(1.0f, 2.0f, 3.0f);
 Player myPlayer = new Player();

 myPlayer.setPosition(position);
 }
}
```

> Ici, la classe `Game` instancie une position qui est un vecteur de dimension 3, affectée au joueur, avec une position en $x$ (`1.0f`) correspondant à l'abscisse (horizontal), $y$ (`2.0f`) à l'ordonnée (vertical) et $z$ (`3.0f`) à la profondeur de champ (distance par rapport à une caméra).

La magnitude serait donc :

```math
\|\mathbf{v}\| = \sqrt{\sum_{i=1}^{3} v_i^2} = \sqrt{1.0^2 + 2.0^2 + 3.0^2} \approx 3{,}74
```

L'avantage d'utiliser des vecteurs, plutôt que des nombres concrets, tient aux propriétés mathématiques associées, qui permettent une représentation plus flexible et une manipulation aisée des quantités géométriques dans les jeux vidéo et d'autres applications.

En outre, les opérations vectorielles standard — addition, soustraction et multiplication par un scalaire — simplifient les calculs et les transformations géométriques requises dans de nombreux scénarios.

#### Addition et soustraction de vecteurs

Pour additionner ou soustraire deux vecteurs, on additionne ou soustrait les composantes correspondantes de chaque vecteur :

- **Addition** : $\mathbf{u} + \mathbf{v} = (u_x + v_x,\ u_y + v_y,\ u_z + v_z)$
- **Soustraction** : $\mathbf{u} - \mathbf{v} = (u_x - v_x,\ u_y - v_y,\ u_z - v_z)$

#### Multiplication par un scalaire

> Un **scalaire** est la représentation d'une quantité, sans direction.

Pour multiplier un vecteur par un scalaire, on multiplie chaque composante du vecteur par le scalaire :

- **Multiplication par un scalaire** : $a \cdot \mathbf{v} = (a \cdot v_x,\ a \cdot v_y,\ a \cdot v_z)$

#### Produit scalaire

Le produit scalaire (ou produit intérieur) prend deux vecteurs et renvoie un nombre réel. Il est défini comme suit :

```math
\mathbf{u} \cdot \mathbf{v} = u_x v_x + u_y v_y + u_z v_z = \|\mathbf{u}\|\,\|\mathbf{v}\|\,\cos\theta
```

où $\theta$ est l'angle entre les deux vecteurs. Le produit scalaire est utilisé entre autres pour calculer un **angle** entre deux directions ou pour tester si deux vecteurs sont **orthogonaux** (produit scalaire nul).

#### Produit vectoriel

Le produit vectoriel (ou produit extérieur) prend deux vecteurs et renvoie un nouveau vecteur **perpendiculaire** à ces deux vecteurs. Il est défini comme suit :

```math
\mathbf{u} \times \mathbf{v} = (u_y v_z - u_z v_y,\ u_z v_x - u_x v_z,\ u_x v_y - u_y v_x)
```

Le produit vectoriel est très utilisé pour calculer la **normale** d'un triangle (utile pour l'éclairage), pour déterminer un **sens de rotation**, ou pour construire un repère orthonormé local à partir de deux vecteurs.

### Interpolation

L'**interpolation** consiste à calculer une valeur intermédiaire entre deux (ou plusieurs) valeurs connues. C'est l'une des opérations les plus utilisées dans un jeu : caméra qui suit le joueur en douceur, animation de fondu, transition de couleur, lissage d'un déplacement réseau, etc.

#### Interpolation linéaire (LERP)

L'interpolation linéaire entre deux valeurs $A$ et $B$ avec un paramètre $t \in [0, 1]$ s'écrit :

```math
\mathrm{lerp}(A, B, t) = (1 - t) \cdot A + t \cdot B = A + t \cdot (B - A)
```

- $t = 0$ donne $A$ ;
- $t = 1$ donne $B$ ;
- $t = 0{,}5$ donne le milieu entre $A$ et $B$.

```csharp
float Lerp(float a, float b, float t) => a + (b - a) * t;
Vector3 LerpVec(Vector3 a, Vector3 b, float t) => a + (b - a) * t;
```

#### Inverse-LERP

L'opération inverse — retrouver $t$ connaissant $A$, $B$ et la valeur courante $V$ :

```math
\mathrm{invLerp}(A, B, V) = \frac{V - A}{B - A}
```

#### Interpolation sphérique (SLERP)

Pour interpoler entre deux **directions** ou deux **rotations** sur une sphère unité, l'interpolation linéaire ne suffit pas (la vitesse angulaire varie). On utilise le **SLERP** (*Spherical Linear Interpolation*). La formule complète est donnée plus bas dans la [section Quaternions](#interpolation--slerp), où le contexte est plus naturel.

#### Easing — interpolation non-linéaire

> **Qu'est-ce que l'easing ?**
> *Easing* signifie littéralement "adoucir". C'est l'idée que dans la vraie vie, rien ne démarre à pleine vitesse et ne s'arrête pile : une voiture accélère puis freine, un objet rebondit, une porte de placard se referme avec un léger ralenti final. En interpolation, **un LERP "tout droit" donne un mouvement mécanique**, robotique. Les fonctions d'easing remplacent le paramètre $t$ par une version courbée de lui-même pour simuler ces accélérations / décélérations naturelles. Les UI modernes (transitions CSS, animations Apple/Material), les caméras de jeu, les démineurs de Démineur — tout passe par de l'easing.

Le principe : on garde $t \in [0, 1]$ mais on l'envoie à travers une fonction $f$ avant l'interpolation : `lerp(A, B, f(t))`. Quelques classiques :

```math
\text{easeInQuad}(t) = t^2
```

départ lent, arrivée brutale.

```math
\text{easeOutQuad}(t) = 1 - (1 - t)^2
```

départ rapide, arrivée en douceur.

```math
\text{easeInOutQuad}(t) = \begin{cases} 2t^2 & \text{si } t < 0{,}5 \\ 1 - 2(1-t)^2 & \text{sinon} \end{cases}
```

départ et arrivée en douceur, vitesse maximale au milieu.

```math
\text{smoothstep}(t) = 3t^2 - 2t^3
```

> **Pourquoi `smoothstep` est partout en graphisme ?** Sa **dérivée** vaut 0 aux deux bords ($t=0$ et $t=1$) et $1{,}5$ au centre. Concrètement : la vitesse de l'animation démarre exactement à zéro, monte, puis revient à zéro. Pas de cassure à l'œil. Sa cousine `smootherstep(t) = 6t^5 - 15t^4 + 10t^3` pousse encore plus loin (dérivée première ET seconde nulles aux bords), au prix de quelques multiplications de plus. C'est la fonction d'easing préférée des shaders et de la génération procédurale.
>
> **Pour aller plus loin** — visualisez tous les easings classiques (sine, cubic, expo, elastic, bounce…) et copiez le code sur [easings.net](https://easings.net/). C'est la référence universelle des animateurs UI.

#### Courbes de Bézier

Pour un mouvement plus libre (trajectoire d'une caméra, animation d'UI…), on utilise des **courbes de Bézier**. La courbe cubique entre $P_0$ et $P_3$ avec deux points de contrôle $P_1$ et $P_2$ s'écrit :

```math
B(t) = (1-t)^3 P_0 + 3(1-t)^2 t P_1 + 3(1-t)t^2 P_2 + t^3 P_3, \quad t \in [0, 1]
```

### Matrices

Les matrices sont des **tableaux rectangulaires** de nombres, utilisés pour effectuer des transformations linéaires sur des vecteurs.

Elles sont couramment utilisées pour représenter des transformations géométriques telles que la translation, la rotation, la mise à l'échelle, etc., que nous verrons par la suite.

Une matrice est généralement représentée sous la forme d'un tableau avec $M$ lignes et $N$ colonnes. Les matrices sont généralement notées en lettres majuscules, telles que $A$, $B$, $C$, etc.

Les opérations courantes sur les matrices incluent l'addition, la soustraction, la multiplication par un scalaire et la multiplication de matrices.

> Il convient de différencier les matrices selon leur représentation **mathématique** et **informatique**.
>
> En mathématiques, elles sont utilisées pour représenter des transformations linéaires, résoudre des systèmes d'équations linéaires et effectuer des opérations sur des vecteurs.
>
> En informatique, elles sont utilisées pour stocker et manipuler des données sous forme de tableaux à deux dimensions, pour des applications telles que les graphiques, l'apprentissage automatique, la modélisation de données et la simulation.

#### Addition et soustraction de matrices

Pour additionner ou soustraire deux matrices, on additionne ou soustrait les éléments correspondants de chaque matrice :

- **Addition** : $A + B = [\,a_{ij} + b_{ij}\,]$
- **Soustraction** : $A - B = [\,a_{ij} - b_{ij}\,]$

#### Multiplication d'une matrice par un scalaire

Pour multiplier une matrice par un scalaire, il suffit de multiplier chaque élément de la matrice par le scalaire :

- **Multiplication par un scalaire** : $a \cdot A = [\,a \cdot a_{ij}\,]$

#### Multiplication de matrices

La multiplication de matrices est une opération qui prend deux matrices et renvoie une nouvelle matrice. Elle est définie de telle manière que si $A$ est une matrice de taille $m \times n$ et $B$ une matrice de taille $n \times p$, alors le produit $AB$ est une matrice de taille $m \times p$.

La multiplication est effectuée en multipliant les éléments de chaque ligne de la première matrice par les éléments correspondants de chaque colonne de la deuxième matrice, puis en additionnant les résultats :

```math
AB = [\,c_{ij}\,] \quad \text{où} \quad c_{ij} = \sum_{k=1}^{n} a_{ik} \cdot b_{kj}
```

> La multiplication de matrices **n'est pas commutative** : en général, $AB \neq BA$.

### Transformations

Une transformation en mathématiques est une fonction qui associe à chaque élément d'un ensemble un autre élément du même ensemble.

En graphisme, on utilise principalement les transformations suivantes — chacune représentée par une matrice qu'on peut combiner par multiplication :

#### Translation

La **translation** est une transformation qui déplace un objet d'une position à une autre sans changer sa forme ou son orientation. En 3D, elle peut être représentée par une matrice de transformation homogène 4×4 :

```math
\begin{pmatrix} 1 & 0 & 0 & t_x \\ 0 & 1 & 0 & t_y \\ 0 & 0 & 1 & t_z \\ 0 & 0 & 0 & 1 \end{pmatrix}
```

où $t_x$, $t_y$ et $t_z$ sont les quantités de mouvement dans chaque direction.

Cette matrice peut être utilisée pour déplacer un vecteur de position homogène

```math
\mathbf{v}_h = \begin{pmatrix} x \\ y \\ z \\ 1 \end{pmatrix}
```

d'une quantité de mouvement spécifique dans chaque direction.

La multiplication de la matrice de translation homogène par le vecteur de position homogène produit un nouveau vecteur de position homogène :

```math
\mathbf{v}'_h = \begin{pmatrix} 1 & 0 & 0 & t_x \\ 0 & 1 & 0 & t_y \\ 0 & 0 & 1 & t_z \\ 0 & 0 & 0 & 1 \end{pmatrix} \begin{pmatrix} x \\ y \\ z \\ 1 \end{pmatrix} = \begin{pmatrix} x + t_x \\ y + t_y \\ z + t_z \\ 1 \end{pmatrix}
```

#### Rotation

La **rotation** en 3D est une transformation qui fait tourner un objet autour d'un point ou d'un axe donné, sans changer sa position ou sa taille. En 3D, elle peut être représentée par une matrice de transformation homogène 4×4 :

```math
\begin{pmatrix} r_{11} & r_{12} & r_{13} & 0 \\ r_{21} & r_{22} & r_{23} & 0 \\ r_{31} & r_{32} & r_{33} & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}
```

où $r_{11}, r_{12}, \ldots, r_{33}$ sont les coefficients de la matrice de rotation.

Ces coefficients peuvent être calculés à partir des angles de rotation autour de chacun des axes $X$, $Y$ et $Z$, ou à partir d'un vecteur d'axe de rotation et d'un angle de rotation.

Par exemple, la rotation autour de l'axe $Z$ d'un angle $\theta$ s'écrit :

```math
R_z(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta & 0 & 0 \\ \sin\theta & \cos\theta & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}
```

La multiplication de la matrice de rotation homogène par le vecteur de position homogène produit un nouveau vecteur de position homogène :

```math
\mathbf{v}'_h = \begin{pmatrix} r_{11} & r_{12} & r_{13} & 0 \\ r_{21} & r_{22} & r_{23} & 0 \\ r_{31} & r_{32} & r_{33} & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix} \begin{pmatrix} x \\ y \\ z \\ 1 \end{pmatrix} = \begin{pmatrix} r_{11} x + r_{12} y + r_{13} z \\ r_{21} x + r_{22} y + r_{23} z \\ r_{31} x + r_{32} y + r_{33} z \\ 1 \end{pmatrix}
```

#### Quaternions

Les **quaternions** sont des nombres hypercomplexes utilisés pour représenter les **rotations en 3D**. Ils sont devenus le standard dans les moteurs de jeu modernes (Unity, Unreal, Godot…) parce qu'ils résolvent plusieurs problèmes des matrices de rotation et des angles d'Euler.

##### Pourquoi pas les angles d'Euler ?

Les angles d'Euler (yaw / pitch / roll) souffrent du **gimbal lock** (perte d'un degré de liberté quand deux axes s'alignent), interpolent mal et accumulent des erreurs numériques. Les quaternions évitent ces problèmes.

##### Définition

Un quaternion s'écrit :

```math
\mathbf{q} = w + x\,\mathbf{i} + y\,\mathbf{j} + z\,\mathbf{k} = (w,\ x,\ y,\ z)
```

avec les règles : $\mathbf{i}^2 = \mathbf{j}^2 = \mathbf{k}^2 = \mathbf{i}\mathbf{j}\mathbf{k} = -1$.

Pour représenter une rotation d'angle $\theta$ autour d'un axe unitaire $\mathbf{u} = (u_x, u_y, u_z)$, on utilise un **quaternion unitaire** :

```math
\mathbf{q} = \left(\cos\frac{\theta}{2},\ u_x \sin\frac{\theta}{2},\ u_y \sin\frac{\theta}{2},\ u_z \sin\frac{\theta}{2}\right)
```

##### Propriétés

- **Norme** : $\|\mathbf{q}\| = \sqrt{w^2 + x^2 + y^2 + z^2}$. Un quaternion unitaire a une norme de $1$.
- **Conjugué** : $\mathbf{q}^* = (w, -x, -y, -z)$.
- **Inverse** : pour un quaternion unitaire, $\mathbf{q}^{-1} = \mathbf{q}^*$.

##### Multiplication (produit de Hamilton)

Le produit de deux quaternions $\mathbf{q}_a = (w_a, x_a, y_a, z_a)$ et $\mathbf{q}_b = (w_b, x_b, y_b, z_b)$ s'écrit :

```math
\mathbf{q}_a \, \mathbf{q}_b = \begin{pmatrix}
w_a w_b - x_a x_b - y_a y_b - z_a z_b \\
w_a x_b + x_a w_b + y_a z_b - z_a y_b \\
w_a y_b - x_a z_b + y_a w_b + z_a x_b \\
w_a z_b + x_a y_b - y_a x_b + z_a w_b
\end{pmatrix}
```

> La multiplication de quaternions **n'est pas commutative** : $\mathbf{q}_a \mathbf{q}_b \neq \mathbf{q}_b \mathbf{q}_a$.

**Composition de rotations** — convention. Avec la formule de rotation $\mathbf{v}' = \mathbf{q}\,\mathbf{p}\,\mathbf{q}^{-1}$ (présentée juste après), pour appliquer **d'abord** $\mathbf{q}_1$ **puis** $\mathbf{q}_2$ on calcule :

```math
\mathbf{q}_\text{total} = \mathbf{q}_2 \, \mathbf{q}_1
```

C'est la même convention que les matrices : la transformation appliquée en premier se trouve **à droite** du produit. L'ordre est important — c'est exactement la raison pour laquelle on préfère les quaternions : ils composent proprement, sans gimbal-lock.

##### Rotation d'un vecteur

Pour faire tourner un vecteur $\mathbf{v}$ par un quaternion $\mathbf{q}$, on construit le quaternion pur $\mathbf{p} = (0, v_x, v_y, v_z)$ puis on calcule :

```math
\mathbf{v}' = \mathbf{q}\,\mathbf{p}\,\mathbf{q}^{-1}
```

##### Interpolation : SLERP

Pour interpoler entre deux orientations $\mathbf{q}_0$ et $\mathbf{q}_1$ de manière fluide, on utilise le **SLERP** (*Spherical Linear Interpolation*) :

```math
\mathrm{slerp}(\mathbf{q}_0, \mathbf{q}_1, t) = \frac{\sin\!\big((1-t)\,\Omega\big)}{\sin\Omega}\,\mathbf{q}_0 + \frac{\sin(t\,\Omega)}{\sin\Omega}\,\mathbf{q}_1
```

où $\Omega$ est l'angle entre les deux quaternions, donné par leur **produit scalaire 4D** :

```math
\cos\Omega = \mathbf{q}_0 \cdot \mathbf{q}_1 = w_0 w_1 + x_0 x_1 + y_0 y_1 + z_0 z_1
```

> **Astuce d'implémentation.** Si $\cos\Omega < 0$, on renverse le signe de l'un des deux quaternions ($-\mathbf{q}$ représente la même rotation) pour passer par le chemin court sur la sphère. Et quand $\Omega$ est très petit, on retombe sur un LERP suivi d'une normalisation pour éviter la division par $\sin\Omega \to 0$ — c'est ce que font Unity et Unreal en interne.

```csharp
// Unity / .NET
Quaternion targetRot = Quaternion.AngleAxis(45f, Vector3.up);
transform.rotation = Quaternion.Slerp(transform.rotation, targetRot, t);
```

#### Mise à l'échelle (et son cas particulier, l'homothétie)

La **mise à l'échelle** agrandit ou rétrécit un objet en multipliant ses coordonnées par un facteur indépendant par axe, sans changer sa position. En matrices homogènes :

**En 2D :**

```math
S(s_x, s_y) = \begin{pmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \\ 0 & 0 & 1 \end{pmatrix}
\quad\Rightarrow\quad
S \begin{pmatrix} x \\ y \\ 1 \end{pmatrix} = \begin{pmatrix} s_x x \\ s_y y \\ 1 \end{pmatrix}
```

**En 3D :**

```math
S(s_x, s_y, s_z) = \begin{pmatrix} s_x & 0 & 0 & 0 \\ 0 & s_y & 0 & 0 \\ 0 & 0 & s_z & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}
```

- $s > 1$ agrandit ; $0 < s < 1$ réduit ; $s < 0$ effectue une réflexion par rapport à l'axe.
- L'**homothétie** est le cas particulier $s_x = s_y = s_z = s$ : un facteur unique appliqué uniformément. Toutes les distances entre les points de l'objet sont multipliées par $s$, les angles sont préservés.

> **Mettre à l'échelle non uniformément avant de faire pivoter** déforme l'objet (oblique). En infographie on suit en général l'ordre **scale → rotate → translate** : multiplier $T \cdot R \cdot S$ et appliquer la matrice à un sommet.

#### Cisaillement

Le **cisaillement** est une transformation géométrique qui déforme un objet en le poussant le long d'un axe parallèle à un autre axe. Il peut être considéré comme une combinaison de translations et d'étirements.

En 2D :

```math
\begin{pmatrix} 1 & a & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{pmatrix}
```

où $a$ est le coefficient de cisaillement. Le coefficient $a$ détermine la quantité de déplacement du vecteur dans la direction de l'axe des $x$, par rapport à sa position d'origine, en fonction de sa coordonnée sur l'axe des $y$.

**Cisaillement 3D — un axe à la fois.** Un cisaillement *pur* dans l'espace déforme un seul axe en fonction d'un autre. Les six combinaisons possibles ($x$ par $y$, $x$ par $z$, $y$ par $x$, $y$ par $z$, $z$ par $x$, $z$ par $y$) sont représentées par une seule entrée hors-diagonale dans la matrice. Par exemple, un cisaillement de $x$ par $y$ d'intensité $a$ s'écrit :

```math
S_{xy}(a) = \begin{pmatrix} 1 & a & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}
```

Combiner plusieurs cisaillements revient à multiplier ces matrices (l'ordre compte). Mettre simultanément les six coefficients hors-diagonale dans une même matrice produit une **transformation affine générale**, pas un cisaillement pur — à utiliser avec prudence.

La multiplication produit :

**En 2D :**

```math
\mathbf{v}'_h = \begin{pmatrix} 1 & a & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{pmatrix} \begin{pmatrix} x \\ y \\ 1 \end{pmatrix} = \begin{pmatrix} x + a y \\ y \\ 1 \end{pmatrix}
```

**En 3D, cisaillement de $x$ par $y$ :**

```math
\mathbf{v}'_h = S_{xy}(a) \begin{pmatrix} x \\ y \\ z \\ 1 \end{pmatrix} = \begin{pmatrix} x + a y \\ y \\ z \\ 1 \end{pmatrix}
```

### Géométrie linéaire

La géométrie linéaire est la branche des mathématiques qui étudie les transformations géométriques dans l'espace en utilisant des outils algébriques tels que les **matrices** et les **vecteurs**.

En informatique graphique, la géométrie linéaire est utilisée pour créer des images en 2D et en 3D. Les transformations géométriques sont appliquées aux objets pour les déplacer, les faire tourner et les étirer dans l'espace. Les images sont ensuite **projetées** sur un écran pour les afficher.

#### Projection

La **projection** est une transformation utilisée pour projeter un objet en 3D sur un plan en 2D pour son affichage à l'écran. Elle peut être réalisée en multipliant un vecteur de position homogène

```math
\begin{pmatrix} x \\ y \\ z \\ 1 \end{pmatrix}
```

par une matrice de projection appropriée.

Il existe deux types de projection couramment utilisés : la **projection orthographique** et la **projection perspective**. La projection orthographique projette l'objet en parallèle sur le plan en 2D ; la projection perspective utilise une distance de vue pour simuler les effets de perspective dans l'affichage de l'objet.

En 3D, la projection **orthographique** peut être représentée par :

```math
\begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}
```

où la troisième colonne est remplacée par des zéros pour indiquer que la projection se fait sur un plan en 2D.

La projection **perspective** peut être représentée en 3D par une matrice 4×4 :

```math
\begin{pmatrix} \dfrac{1}{\tan\left(\dfrac{\theta}{2}\right)} & 0 & 0 & 0 \\ 0 & \dfrac{h}{w\cdot\tan\left(\dfrac{\theta}{2}\right)} & 0 & 0 \\ 0 & 0 & \dfrac{-(f+n)}{f-n} & \dfrac{-2fn}{f-n} \\ 0 & 0 & -1 & 0 \end{pmatrix}
```

où $\theta$ est l'angle de vue (FOV), $w$ et $h$ sont les largeur et hauteur de l'écran, $n$ et $f$ sont les distances du plan de coupe avant (*near*) et arrière (*far*).

#### Perspective

> ℹ Nous vous invitons à ne pas confondre la **perspective**, qui fait référence à la façon dont les objets apparaissent différents en taille et en forme en fonction de leur position et de leur distance par rapport à un point de vue, et la **projection perspective**, vue juste avant, qui est une méthode utilisée pour projeter des objets en 3D sur un plan en 2D en utilisant une caméra virtuelle.

La perspective est une transformation utilisée en informatique graphique pour donner une **impression de profondeur** et de distance aux objets en 3D. Elle peut être représentée en 3D par la même matrice 4×4 que la projection perspective :

```math
\begin{pmatrix} \dfrac{1}{\tan\left(\dfrac{\theta}{2}\right)} & 0 & 0 & 0 \\ 0 & \dfrac{h}{w\cdot\tan\left(\dfrac{\theta}{2}\right)} & 0 & 0 \\ 0 & 0 & \dfrac{-(f+n)}{f-n} & \dfrac{-2fn}{f-n} \\ 0 & 0 & -1 & 0 \end{pmatrix}
```

Cette matrice peut être utilisée pour transformer un vecteur de position homogène

```math
\begin{pmatrix} x \\ y \\ z \\ 1 \end{pmatrix}
```

en un nouveau vecteur de position homogène qui représente la position de l'objet vue depuis le point de vue de l'observateur, en appliquant une perspective qui diminue la taille des objets à mesure qu'ils s'éloignent.

#### Transformation de vue

La **transformation de vue** est utilisée pour modifier la perspective de l'observateur sur un objet en 3D — autrement dit, exprimer le monde dans le repère de la caméra.

Elle peut être représentée en 3D par une matrice de transformation homogène 4×4 :

```math
\begin{pmatrix} R_{11} & R_{12} & R_{13} & -d_x \\ R_{21} & R_{22} & R_{23} & -d_y \\ R_{31} & R_{32} & R_{33} & -d_z \\ 0 & 0 & 0 & 1 \end{pmatrix}
```

où $d_x$, $d_y$ et $d_z$ sont les composantes de la position de la caméra et $R$ est la matrice de rotation qui représente l'orientation de la caméra.

Cette matrice transforme un vecteur de position homogène

```math
\begin{pmatrix} x \\ y \\ z \\ 1 \end{pmatrix}
```

en un nouveau vecteur de position homogène qui représente la position de l'objet vue depuis le point de vue de l'observateur.

#### Espaces de coordonnées

Dans un moteur 3D, un sommet passe par **plusieurs espaces de coordonnées** avant d'arriver à l'écran. Comprendre cette chaîne est essentiel pour déboguer un problème de rendu, écrire un shader ou positionner correctement un objet.

```mermaid
graph LR
A[Espace local<br/>Object Space] -->|Matrice modèle M| B[Espace monde<br/>World Space]
B -->|Matrice vue V| C[Espace caméra<br/>View Space]
C -->|Matrice projection P| D[Espace clip<br/>Clip Space]
D -->|Division perspective| E[Espace NDC<br/>-1..1]
E -->|Viewport| F[Espace écran<br/>pixels]
```

| Espace | Repère | Usage |
| --- | --- | --- |
| **Local / Objet** | repère de l'objet | sommets tels qu'exportés du logiciel 3D |
| **Monde** | repère global de la scène | position absolue de l'objet |
| **Vue / Caméra** | caméra à l'origine, regardant $-Z$ | éclairage, calculs liés à la caméra |
| **Clip** | espace de projection | culling avant projection |
| **NDC** | cube $[-1, 1]^3$ | espace normalisé après division par $w$ |
| **Écran** | pixels | affichage final |

La transformation complète d'un sommet $\mathbf{v}_{\text{local}}$ s'écrit comme un produit de matrices, appliqué dans l'ordre $\text{modèle} \to \text{vue} \to \text{projection}$ — c'est la fameuse matrice **MVP** :

```math
\mathbf{v}_{\text{clip}} = P \cdot V \cdot M \cdot \mathbf{v}_{\text{local}}
```

[ Retour en haut de page](#table-des-matières)

---

## Graphiques informatiques

En informatique graphique, les graphiques informatiques constituent une partie non négligeable, souvent traduite par la notion d'**image numérique**.

### Graphiques vectoriels et bitmap

Bien que les images puissent être représentées de différentes manières (matricielles, HDR — *High Dynamic Range*, panoramiques, etc.), les deux méthodes les plus courantes dans le domaine des jeux vidéo sont les **graphiques vectoriels** et les **graphiques bitmap**.

#### Graphiques vectoriels

Contrairement aux images bitmap, qui sont composées de pixels individuels, les images vectorielles sont constituées de **courbes**, de **lignes** et de **formes géométriques**.

Ce sont donc des images créées à partir de **formules mathématiques** décrivant les différentes formes et couleurs de l'image.

Lorsque l'image est agrandie ou réduite, ces formes sont simplement recalculées en fonction de la nouvelle taille de l'image, garantissant ainsi que l'image conserve une qualité élevée — à l'instar des bitmaps qui étirent les pixels affichés et les « dégradent ».

##### Fonctionnement (vectoriel)

Les images vectorielles sont constituées de formes géométriques décrites mathématiquement par des équations.

Chaque forme est représentée par un ensemble de **points**, de **lignes** et de **courbes** qui sont reliés les uns aux autres pour créer la forme souhaitée.

Les formes géométriques peuvent être de différentes sortes : des lignes droites, des **courbes de Bézier**, des cercles, des ellipses, des polygones, etc.

##### Exemple

Prenons un cercle de rayon $r$ centré en $(x_c, y_c)$ sur un plan cartésien. Sa représentation mathématique est donnée par l'équation suivante :

```math
(x - x_c)^2 + (y - y_c)^2 = r^2
```

Pour représenter ce cercle dans une image vectorielle, on utilise une équation paramétrique qui décrit chaque point $(x, y)$ de la forme comme une fonction de son angle $\theta$ :

```math
x = x_c + r \cos \theta, \quad y = y_c + r \sin \theta
```

On peut ensuite relier ces points par des segments de ligne pour créer le cercle dans l'image vectorielle.

Ainsi, pour un cercle de rayon $3$ centré en $(2, 2)$, l'équation mathématique est $(x - 2)^2 + (y - 2)^2 = 9$ et son équation paramétrique :

```math
x = 2 + 3 \cos \theta
```

```math
y = 2 + 3 \sin \theta
```

> $\cos$ et $\sin$ de $\theta$ sont utilisés ici, dans le cas du cercle, pour obtenir les valeurs $x$ et $y$ correspondant à chaque angle $\theta$ donné.

où $\theta$ est l'angle par rapport à l'origine du cercle.

En prenant des valeurs différentes de $\theta$ (par exemple $\theta = 0, \pi/4, \pi/2, 3\pi/4, \pi, \ldots$), on calcule les coordonnées correspondantes $(x, y)$ et on relie ces points par des segments de ligne pour créer le cercle dans l'image vectorielle.

#### Bitmap

Les **graphiques bitmap**, également appelés images matricielles, sont créés en utilisant une grille de pixels de différentes couleurs.

##### Fonctionnement (bitmap)

Les images bitmap sont stockées sous forme de **matrice de pixels**, où chaque pixel est représenté par une valeur de couleur. Pour comprendre comment cela fonctionne, considérons un exemple simple : une image bitmap en noir et blanc de taille 4×4.

Nous pouvons stocker cette image sous forme de matrice de pixels 4×4 où chaque pixel est représenté par un nombre binaire indiquant s'il est blanc ($0$) ou noir ($1$) :

```math
\begin{pmatrix} 0 & 1 & 0 & 1 \\ 1 & 0 & 1 & 0 \\ 0 & 1 & 0 & 1 \\ 1 & 0 & 1 & 0 \end{pmatrix}
```

Plus la résolution de l'image est élevée, plus la taille de la matrice de pixels est grande et plus l'image est détaillée.

Pour stocker des images en couleur, nous pouvons utiliser une matrice de pixels **tridimensionnelle** où chaque pixel est représenté par une valeur de couleur RVB (rouge, vert, bleu) ou CMJN (cyan, magenta, jaune, noir).

La valeur de chaque canal de couleur est généralement stockée dans un octet (8 bits), ce qui signifie qu'il y a 256 niveaux de chaque couleur (de 0 à 255).

### Résolution et profondeur de couleur

La résolution et la profondeur de couleur sont deux concepts étroitement liés qui déterminent la **qualité visuelle** et la **taille des données** d'une image numérique.

#### Résolution

La résolution d'une image est définie par le nombre de pixels qu'elle contient horizontalement et verticalement, généralement noté $W \times H$ (par exemple, 800×600, signifiant 800 pixels de large pour 600 pixels de haut).

La résolution a des implications importantes sur la quantité de données requises pour stocker une image. Pour une image fixe avec une profondeur de couleur constante $b$, le nombre total de bits requis est donné par :

```math
N_\text{bits} = W \times H \times b
```

où $W$ est la largeur, $H$ la hauteur et $b$ la profondeur de couleur en bits.

La résolution a également un impact sur la **bande passante** requise pour transmettre des images en temps réel, comme c'est le cas dans les jeux vidéo : une résolution plus élevée nécessite plus de bande passante.

#### Profondeur de couleur

La profondeur de couleur, également appelée *bit depth*, représente le nombre de bits utilisés pour décrire la couleur d'un pixel, généralement noté $b$.

Une profondeur de couleur plus élevée permet de représenter un plus grand nombre de couleurs $C = 2^b$, rendant les transitions entre les couleurs plus douces et permettant des images plus réalistes.

Supposons que nous utilisions un espace de couleur RVB. La profondeur de couleur est divisée également entre les composantes rouge, verte et bleue, chacune ayant $b_\text{RGB} = b/3$ bits. Alors, le nombre de valeurs possibles pour chaque composante est $2^{b_\text{RGB}}$. Par conséquent, le nombre total de couleurs différentes pouvant être représentées est :

```math
C = (2^{b_\text{RGB}})^3 = 2^b
```

### Espaces de couleur

> **Qu'est-ce qu'un espace de couleur ?** Une **convention** qui dit comment encoder une couleur en chiffres : combien de composantes (rouge/vert/bleu, ou cyan/magenta/jaune…), sur quelle plage (0-255, 0.0-1.0…) et selon quelle transformation. Deux images peuvent contenir les "mêmes" pixels physiques mais avec des chiffres totalement différents si elles utilisent des espaces différents.

Les images bitmap peuvent être stockées en utilisant différents espaces de couleur. Les plus courants :

- **RVB** (Rouge, Vert, Bleu) : chaque pixel est représenté par trois valeurs pour les composantes rouge, verte et bleue. Format dominant en jeu vidéo et en infographie. Mathématiquement : un triplet $(R, G, B)$.
- **RVBA** : RVB + un canal **alpha** (transparence). Alpha = 0 totalement transparent, alpha = 1 totalement opaque.
- **CMJN** (Cyan, Magenta, Jaune, Noir) : utilisé en impression. Soustractif au lieu d'additif.
- **HSL / HSV** (Teinte, Saturation, Luminosité / Valeur) : reprise des coordonnées RVB sous forme circulaire, pratique pour la manipulation artistique des couleurs (un curseur de "teinte" plutôt que trois sliders R/G/B).
- **YUV / YCbCr** : utilisé en compression vidéo (JPEG, MPEG, H.264). Sépare la luminance (Y) des composantes de chrominance (U/V), ce qui permet de compresser plus agressivement la chrominance, à laquelle l'œil est moins sensible.

#### Linéaire vs sRGB : *le* piège que tout le monde rencontre

Quand vous voyez une couleur `(0.5, 0.5, 0.5)` stockée dans une texture, à quoi correspond-elle physiquement ? À 50 % de la lumière émise par un pixel blanc ? Ou à 50 % de "l'éclat perçu" par l'œil ? Les deux sont **complètement différents** parce que :

1. L'œil humain est **non-linéaire** : il distingue mieux les nuances dans les sombres que dans les clairs. Un pixel à 50 % de l'éclat perçu correspond en fait à environ 22 % de l'éclat physique réel.
2. Les écrans LCD/OLED appliquent traditionnellement une **courbe gamma** (~2.2) à leur entrée pour compenser ce biais perceptuel — c'est ce qui définit l'espace **sRGB**.

```math
C_\text{linéaire} \approx C_\text{sRGB}^{2.2}
\qquad
C_\text{sRGB} \approx C_\text{linéaire}^{1/2.2}
```

(la formule sRGB exacte est par morceaux, mais $2.2$ est une excellente approximation utilisée par les shaders.)

**Pourquoi ça compte ?** Les calculs d'éclairage (Phong, Lambert, PBR — voir plus bas) **doivent** se faire en **espace linéaire**, où l'addition de deux faisceaux de lumière correspond à `C_a + C_b`. Si vous ajoutez deux couleurs sRGB sans conversion préalable, vous obtenez un résultat **délavé**, gris-jaunâtre, qui ne ressemble à rien de réaliste.

**Workflow correct dans un jeu moderne :**

1. **Texture.png** est stockée en sRGB → marquer la sampler `SRGB` à la création (Unity : "sRGB (Color Texture)" coché ; Vulkan : `VK_FORMAT_R8G8B8A8_SRGB`).
2. Lors du sample dans le shader, le GPU **convertit automatiquement** sRGB → linéaire avant calcul.
3. Tous les calculs d'éclairage, alpha-blending, post-process, se font en linéaire.
4. Le pipeline final convertit linéaire → sRGB juste avant l'écriture dans le framebuffer.

> **Le bug classique** : un junior met une texture albedo en `RGBA8_UNORM` au lieu de `RGBA8_SRGB`. Le shader croit lire du linéaire, fait ses calculs sur des chiffres déjà gamma-corrigés, le résultat est trop sombre dans les ombres et trop clair dans les highlights. C'est exactement ce qui produisait l'aspect "PS3" de certains jeux mal calibrés des années 2008-2012.
>
> **Qu'est-ce que le HDR (*High Dynamic Range*) ?** Plage dynamique étendue : on stocke des composantes au-delà de `[0, 1]` (un soleil peut faire `(50, 50, 50)`). Cela ouvre la porte au *bloom*, à l'*exposition*, et au *tonemapping* (Reinhard, ACES, etc.) qui ramène la scène HDR dans la plage `[0, 1]` affichable par l'écran. Indispensable en PBR.

### Formats de fichier d'image

Les formats de fichier d'image déterminent la manière dont les données d'image sont organisées et stockées. Plusieurs formats sont couramment utilisés dans les jeux vidéo et les applications graphiques :

- **BMP** (Bitmap) : format non compressé développé par Microsoft. Il stocke les données pixel par pixel, sans compression, ce qui peut entraîner des fichiers volumineux.
- **JPEG** (*Joint Photographic Experts Group*) : format compressé avec **perte** qui utilise la compression DCT (*Discrete Cosine Transform*) pour réduire la taille des fichiers. Bien adapté aux images photographiques avec de nombreux détails et variations de couleur.
- **PNG** (*Portable Network Graphics*) : format compressé **sans perte** qui utilise la compression DEFLATE. Bien adapté aux images avec des zones de couleur uniforme et des bords nets, comme des graphiques ou des logos.
- **GIF** (*Graphics Interchange Format*) : format compressé sans perte développé par CompuServe. Limité à une palette de 256 couleurs, principalement utilisé pour les images animées simples et les graphiques avec des zones de couleur uniforme.
- **TGA** (Targa) : format développé par Truevision qui prend en charge les images en couleur 8, 16, 24 et 32 bits. Souvent utilisé dans les jeux vidéo et les applications de rendu 3D pour stocker des **textures**.

#### Schéma comparatif

```mermaid
graph TB
 A[Formats de fichier d'image]
 A --> B1[BMP]
 A --> B2[JPEG]
 A --> B3[PNG]
 A --> B4[GIF]
 A --> B5[TGA]

 B1 --> C1[Non compressé]
 B1 --> C2[Simple]
 B2 --> C3[Compressé avec perte]
 B2 --> C4[Adapté aux photographies]
 B3 --> C5[Compressé sans perte]
 B3 --> C6[Adapté aux graphiques et logos]
 B4 --> C7[Compressé sans perte]
 B4 --> C8[Limité à 256 couleurs]
 B4 --> C9[Adapté aux images animées simples]
 B5 --> C10[Supporte 8, 16, 24 et 32 bits]
 B5 --> C11[Utilisé pour les textures 3D]
```

[ Retour en haut de page](#table-des-matières)

---

## Éclairage et ombres

Les techniques d'éclairage et de gestion des ombres sont basées sur des concepts mathématiques et physiques qui permettent de **simuler la manière dont la lumière interagit** avec les objets et l'environnement.

### Sources de lumière

Les sources de lumière sont des entités qui émettent de la lumière dans une scène. Les principales sources utilisées dans les jeux vidéo et les graphiques 3D sont :

1. **Lumière directionnelle** : représente une source située à une distance infinie, comme le soleil. Tous les rayons lumineux sont parallèles et ont la même intensité. Souvent utilisée pour simuler la lumière du jour.
2. **Lumière ponctuelle** : émet de la lumière dans toutes les directions à partir d'un point dans l'espace. L'intensité diminue avec la distance à la source, généralement proportionnelle à l'**inverse du carré de la distance**.
3. **Lumière spot** : émet de la lumière dans une direction conique à partir d'un point dans l'espace. Souvent utilisée pour simuler les projecteurs ou les lampes torches.

### Modèles d'éclairage

Les modèles d'éclairage décrivent comment la lumière interagit avec les objets et les surfaces. Voici quelques-uns des modèles les plus couramment utilisés :

1. **Modèle d'éclairage de Phong** : basé sur trois composantes — l'éclairage **ambiant**, l'éclairage **diffus** et l'éclairage **spéculaire**. L'éclairage ambiant est une constante qui simule la lumière indirecte réfléchie par l'environnement. L'éclairage diffus est proportionnel à l'angle entre la normale de la surface et la direction de la lumière. L'éclairage spéculaire dépend de l'angle entre la direction de la lumière réfléchie et la direction de la caméra.
2. **Modèle d'éclairage de Lambert** : simplification du modèle de Phong, qui ne prend en compte que l'éclairage ambiant et l'éclairage diffus. Moins réaliste mais plus rapide à calculer — un choix approprié pour les jeux vidéo sur des systèmes à faible puissance de calcul.
3. **Modèle d'éclairage de Blinn-Phong** : amélioration du modèle de Phong qui utilise une approximation de la direction de la lumière réfléchie pour calculer l'éclairage spéculaire. Plus réaliste que le modèle de Phong et souvent utilisé dans les jeux vidéo modernes.

### Ombres

Les **ombres** sont des zones où la lumière est bloquée par un objet. Elles ajoutent de la profondeur et du réalisme à une scène. Voici quelques techniques couramment utilisées :

1. **Ombres portées** (*Shadow mapping*) : on crée une carte des profondeurs (*depth map*) à partir de la perspective de la source de lumière. Cette carte stocke la distance entre la source de lumière et le point le plus proche qui la bloque. Lors du rendu, on compare la distance entre la source de lumière et le point courant avec la distance stockée dans la carte. Si la distance courante est supérieure, le point est dans l'ombre.
2. **Ombres volumétriques** (*Volumetric shadows*) : simulent les ombres en calculant l'atténuation de la lumière lorsqu'elle traverse des objets semi-transparents, comme la fumée ou la brume. Donnent un aspect réaliste aux scènes où la lumière interagit avec des particules en suspension dans l'air.
3. **Ombres douces** (*Soft shadows*) : ombres qui présentent un flou progressif en s'éloignant de l'objet qui les projette. On simule plusieurs sources de lumière proches les unes des autres, ou on utilise des techniques de filtrage pour adoucir les bords des ombres portées.
4. **Ray tracing** : technique de rendu avancée qui simule le comportement de la lumière en traçant des rayons depuis la caméra jusqu'à la source de lumière, en prenant en compte les **réflexions** et les **réfractions**. Permet de générer des ombres, des reflets et des effets de lumière globale très réalistes — mais coûteux en temps de calcul. De plus en plus utilisé dans les jeux vidéo grâce à l'évolution des cartes graphiques et des algorithmes de rendu.

[ Retour en haut de page](#table-des-matières)

---

## Texture et mappage UV

### Texture et coordonnées de texture

Les **textures** sont des images 2D appliquées sur des objets 3D pour donner l'illusion de détails tels que les couleurs, les motifs ou les reliefs.

Elles peuvent être utilisées pour représenter la **couleur** de base d'un objet, sa **brillance**, sa **rugosité**, sa **transparence**, etc.

Les **coordonnées de texture**, également appelées **coordonnées UV**, déterminent la manière dont une texture est mappée sur un objet 3D.

Pour appliquer une texture à un objet 3D, on attribue à chaque sommet de l'objet un ensemble de coordonnées UV, qui correspondent aux coordonnées $(u, v)$ dans l'image de texture. Les coordonnées UV varient généralement de $0$ à $1$, où $(0, 0)$ correspond au coin inférieur gauche de l'image de texture et $(1, 1)$ au coin supérieur droit.

### Mappage UV

Le **mappage UV** est le processus qui consiste à déterminer les coordonnées UV pour chaque sommet d'un objet 3D. Ce processus est souvent réalisé manuellement par des artistes 3D à l'aide de logiciels spécialisés, mais il existe également des algorithmes de mappage UV automatiques.

Il existe plusieurs techniques de mappage UV :

1. **Mappage planaire** : projette la texture sur l'objet 3D à partir d'un plan. Fonctionne bien pour les objets ayant une forme relativement plane, mais peut provoquer des distorsions et des étirements sur les objets plus complexes.
2. **Mappage cylindrique** : enroule la texture autour de l'objet 3D comme si elle était imprimée sur un cylindre. Fonctionne bien pour les objets cylindriques.
3. **Mappage sphérique** : projette la texture sur l'objet 3D à partir d'une sphère. Fonctionne bien pour les objets sphériques, mais peut provoquer des distorsions aux pôles.
4. **Mappage par morceaux** (*UV unwrapping*) : on découpe l'objet 3D en morceaux, puis on les déplie en 2D pour créer une représentation plane de l'objet. Cette technique permet de minimiser les distorsions, mais nécessite généralement un travail manuel minutieux pour obtenir de bons résultats.

[ Retour en haut de page](#table-des-matières)

---

## Animation

L'animation en infographie consiste à créer l'illusion de **mouvement** ou de **changement** d'un objet ou d'une scène 3D au fil du temps.

Les techniques les plus courantes sont l'**animation par squelette**, l'**animation de forme** et la **cinématique inverse**.

### Animation par squelette

L'**animation par squelette** (*Rigging*), également appelée animation par armature, consiste à définir une structure osseuse (ou armature) pour un objet 3D et à manipuler cette structure pour créer des mouvements.

Chaque os de l'armature est associé à une partie de l'objet 3D et déforme cette partie lorsqu'il est déplacé ou orienté. L'animation par squelette est largement utilisée pour animer des **personnages** et des **créatures** dans les jeux vidéo et les films d'animation.

Une armature est un ensemble de **nœuds** (ou articulations) reliés par des **os**. Les nœuds ont des positions 3D et des orientations, généralement représentées par des matrices de transformation 4×4. Pour déterminer la position et l'orientation d'un nœud, on utilise la relation suivante :

```math
T_\text{parent} \times T_\text{local} = T_\text{global}
```

où $T_\text{parent}$ est la matrice de transformation globale du nœud parent, $T_\text{local}$ est la matrice de transformation locale du nœud actuel, et $T_\text{global}$ est la matrice de transformation globale du nœud actuel.

L'animation d'une armature consiste à modifier les matrices de transformation locale des nœuds au fil du temps, créant ainsi des mouvements.

### Animation de forme

L'**animation de forme**, également appelée *morphing* ou interpolation de formes, consiste à interpoler entre différentes formes d'un objet 3D pour créer des animations. Cette technique est souvent utilisée pour animer des objets dont la géométrie change de manière complexe, comme les **visages** ou les **vêtements**.

L'animation de forme implique généralement l'**interpolation linéaire** entre les positions des sommets des différentes formes. Pour interpoler entre deux formes $A$ et $B$ à un facteur d'interpolation $t$, où $0 \leq t \leq 1$, on utilise la formule suivante :

```math
P_\text{interpolated} = (1 - t) \times P_A + t \times P_B
```

où $P_\text{interpolated}$ est la position interpolée du sommet, et $P_A$ et $P_B$ sont les positions du sommet dans les formes $A$ et $B$, respectivement.

### Cinématique inverse

La **cinématique inverse** (*Inverse Kinematics*, IK) est une technique d'animation utilisée pour calculer les angles des articulations d'une armature en fonction de la **position désirée** d'un effecteur (généralement la main ou le pied d'un personnage).

Cette technique est particulièrement utile pour les animations interactives — par exemple, lorsqu'un personnage saisit un objet ou marche sur un terrain irrégulier.

La cinématique inverse implique généralement la résolution d'un **système d'équations non linéaires** décrivant les positions et les orientations des nœuds de l'armature. Les algorithmes les plus connus sont **CCD** (*Cyclic Coordinate Descent*) et **FABRIK** (*Forward And Backward Reaching Inverse Kinematics*).

[ Retour en haut de page](#table-des-matières)

---

## Physique des jeux

La **physique des jeux** est un élément clé pour créer des environnements interactifs et réalistes dans les jeux vidéo. Elle comprend la **simulation de mouvements**, de **forces** et de **collisions** entre objets dans un monde virtuel.

Les principales composantes de la physique des jeux incluent la simulation physique, la détection de collision et la résolution de collision.

### Simulation physique

La simulation physique calcule les **mouvements** et les **forces** qui agissent sur les objets. Le point de départ reste la **deuxième loi de Newton** :

```math
\mathbf{F} = m\,\mathbf{a}
```

où $\mathbf{F}$ est la résultante des forces (vecteur), $m$ la masse de l'objet (scalaire) et $\mathbf{a}$ son accélération (vecteur). Les forces peuvent inclure la gravité, les forces de contact, le frottement, et d'autres forces externes ; l'accélération s'obtient en sommant l'ensemble :

```math
\mathbf{a} = \frac{1}{m}\sum_i \mathbf{F}_i
```

Position et vitesse sont ensuite intégrées dans le temps. La méthode la plus simple, l'**Euler explicite**, s'écrit :

```math
\mathbf{v}_{t+1} = \mathbf{v}_t + \mathbf{a}\,\Delta t
```

```math
\mathbf{p}_{t+1} = \mathbf{p}_t + \mathbf{v}_{t+1}\,\Delta t
```

où $\Delta t$ est le pas de temps. Euler explicite gagne en simplicité ce qu'il perd en stabilité : sur de longues simulations, il introduit une dérive énergétique (les ressorts gagnent de l'énergie, les orbites s'écartent). On lui préfère :

- **Euler semi-implicite** (*symplectic Euler*) : on met à jour la vitesse **avant** la position (`v += a*dt; p += v*dt`). Conserve l'énergie sur les systèmes oscillants. **Le défaut par défaut** dans la plupart des moteurs de jeu.
- **Verlet** (notamment *velocity Verlet*) : exact à l'ordre 2, conserve très bien l'énergie. Utilisé par les moteurs de cloth et de soft-body.
- **Runge-Kutta 4 (RK4)** : très précis mais coûteux ; utilisé en simulation aérospatiale ou physique éducative, rarement dans les jeux temps réel.

#### Pas de simulation fixe ≠ pas de frame

C'est *le* piège du game-dev débutant : faire l'intégration physique avec le `Δt` de la frame courante (variable selon la machine). Conséquences :

1. **Comportement non déterministe** : le même replay donne des résultats différents sur deux machines parce que les `dt` ne sont pas identiques.
2. **Tunneling** : un objet rapide traverse un mur si la frame est lente, parce que `position += velocity * dt` saute par-dessus la collision.
3. **Instabilité numérique** : les ressorts et contraintes de constantes physiques calculées pour un `dt = 16ms` explosent quand la frame chute à `60ms`.

La solution canonique (Glenn Fiedler, [*"Fix Your Timestep"*](https://gafferongames.com/post/fix_your_timestep/)) :

```text
fixedDt   = 1/60               // pas de simulation, constant
accumulator += frameDelta      // temps réel écoulé depuis la dernière frame
while accumulator >= fixedDt:
 physicsStep(fixedDt)       // itération à pas constant, déterministe
 accumulator -= fixedDt
alpha = accumulator / fixedDt
render(state, alpha)           // interpolation visuelle pour la fluidité
```

C'est exactement ce que fait Unity (`FixedUpdate`), Unreal (`PhysicsTickRate`), Godot (`_physics_process`). Le rendu peut tourner à 144 Hz, la physique reste à 60 Hz fixe.

### Détection de collision

La **détection de collision** est le processus par lequel on détermine si deux objets se touchent ou se croisent. Il existe de nombreuses techniques pour détecter les collisions :

- les tests de **boîtes englobantes** (AABB — *Axis-Aligned Bounding Box*) ;
- les tests de **sphères englobantes** ;
- les tests de **séparation d'axes** (SAT — *Separating Axis Theorem*) ;
- l'algorithme **GJK** (*Gilbert-Johnson-Keerthi*) pour les formes convexes.

Chaque technique a ses avantages et ses inconvénients en termes de **précision** et de **performances**.

```mermaid
graph LR
A[AABB] -->|Rapide, mais moins précis| B(Détection de collision)
C[Sphères englobantes] -->|Précis pour les objets sphériques, moins pour les autres| B
D[SAT] -->|Précis, mais plus lent| B
E[GJK] -->|Précis pour formes convexes| B
```

### Résolution de collision

La **résolution de collision** est le processus par lequel on modifie les positions, les vitesses et les forces des objets en collision pour éviter qu'ils ne se chevauchent ou ne traversent les autres objets.

La résolution de collision peut être basée sur des principes de mécanique classique, comme la conservation de l'**énergie cinétique** et de la **quantité de mouvement**, ou sur des techniques heuristiques pour simplifier les calculs et améliorer les performances.

La résolution de collision implique généralement l'application d'une **force d'impulsion** aux objets en collision pour les séparer :

```math
J = \frac{-(1 + e) \times (v_{A_t} - v_{B_t}) \cdot n}{\dfrac{1}{m_A} + \dfrac{1}{m_B}}
```

où $J$ est l'impulsion, $e$ est le coefficient de **restitution** (élasticité, $0$ = parfaitement inélastique, $1$ = parfaitement élastique), $v_{A_t}$ et $v_{B_t}$ sont les vitesses des objets $A$ et $B$ avant la collision, $m_A$ et $m_B$ sont les masses des objets, et $n$ est le vecteur normal à la surface de contact.

Ensuite, les vitesses des objets après la collision sont mises à jour en fonction de l'impulsion appliquée :

```math
v_{A_{t+1}} = v_{A_t} + \frac{J}{m_A} \times n
```

```math
v_{B_{t+1}} = v_{B_t} - \frac{J}{m_B} \times n
```

La position des objets peut également être corrigée pour éviter les chevauchements en déplaçant les objets en fonction de la profondeur de pénétration $P$ et d'un facteur de correction :

```math
p_{A_{t+1}} = p_{A_t} - \frac{1}{m_A} \times \frac{m_A + m_B}{m_A \times m_B} \times P \times n
```

```math
p_{B_{t+1}} = p_{B_t} + \frac{1}{m_B} \times \frac{m_A + m_B}{m_A \times m_B} \times P \times n
```

où $P$ est la profondeur de pénétration et $n$ est le vecteur normal à la surface de contact.

```mermaid
graph LR
A(Impulsion) -->|Modifie les vitesses| B(Résolution de collision)
C(Correction de position) -->|Évite les chevauchements| B
```

[ Retour en haut de page](#table-des-matières)

---

## Intelligence artificielle

Les jeux vidéo utilisent souvent l'**intelligence artificielle** (IA) pour contrôler les **personnages non joueurs** (PNJ).

### Comportement de base

> **PNJ** = *Personnage Non Joueur*. Tout ce qui bouge à l'écran sans que vous teniez la manette : ennemis, alliés, marchands, civils. Le code qui décide ce qu'ils font à chaque instant, c'est l'IA du jeu.

Les comportements de base des PNJ peuvent être modélisés via plusieurs paradigmes, du plus simple au plus expressif :

#### Machines à états finis (FSM)

> **FSM** = *Finite State Machine* — machine à états finis. Le PNJ a un état courant (ex. *patrouille*, *poursuite*, *attaque*) et des **transitions** déclenchées par des événements (*"j'ai vu le joueur"*, *"j'ai perdu de vue le joueur"*). À chaque tick, on exécute le code de l'état courant, et on évalue les transitions.

```mermaid
stateDiagram-v2
 [*] --> Patrouille
 Patrouille --> Poursuite: joueur visible
 Poursuite --> Attaque: joueur à portée
 Poursuite --> Patrouille: joueur perdu
 Attaque --> Poursuite: joueur hors portée
 Attaque --> Mort: PV ≤ 0
 Mort --> [*]
```

**Avantages** : trivial à implémenter, déterministe, débogable.

**Limites** : explosion combinatoire dès qu'on a plusieurs orthogonalités (état d'arme × état de mouvement × état d'humeur = $n_1 \times n_2 \times n_3$ états explicites). Au-delà de ~10 états, les FSM deviennent illisibles.

#### Arbres de comportement (Behavior Trees)

Inventés pour *Halo 2* (2004) puis popularisés par *Spore*, les **Behavior Trees** (BT) remplacent les transitions explicites par une **structure hiérarchique** que l'on parcourt à chaque tick. Trois types de nœuds :

- **Sequence** ($\rightarrow$) : exécute les enfants en ordre, **réussit** si tous réussissent, échoue dès qu'un échoue. Équivalent du *AND* logique.
- **Selector** ($?$) : exécute les enfants en ordre, **réussit** dès qu'un réussit, échoue si tous échouent. Équivalent du *OR*.
- **Leaf** : action concrète (`MoveTo`, `Attack`, `Wait(2s)`) ou condition (`PlayerVisible?`).

Exemple — un garde "patrouille, et attaque s'il voit quelqu'un" :

```text
Selector (?)
├── Sequence (→)        # priorité 1 : combat
│   ├── PlayerVisible?
│   ├── MoveTo(player)
│   └── Attack(player)
└── Sequence (→)        # priorité 2 : patrouille
 ├── PickRandomWaypoint
 └── MoveTo(waypoint)
```

L'avantage est la **composabilité** : on remplace `Attack` par `[Selector: ShootIfRanged, MeleeIfClose]` sans rien changer ailleurs. C'est ce qui motive son adoption dans Unreal (`BehaviorTree`), Unity (`Behavior Designer`), et tous les middleware AAA.

#### Utility AI

Plutôt que de hard-coder des transitions, l'**Utility AI** assigne une **fonction de score** à chaque action possible et choisit l'action de score maximal :

```math
\mathrm{score}(action) = \prod_{i} f_i(\text{contexte})
```

où chaque $f_i \in [0, 1]$ est une **considération** (distance au joueur, niveau de PV, présence de couvert…). C'est l'approche utilisée dans *The Sims*, *XCOM*, *Hitman*. Plus organique que les BT pour les comportements émergents.

#### GOAP — Goal-Oriented Action Planning

Pour les comportements vraiment intelligents (*F.E.A.R.* en 2005, encore référence), l'IA fait littéralement de la **planification** : étant donné un état initial, un état but (« joueur mort »), et un catalogue d'actions avec **préconditions** et **effets**, GOAP cherche la séquence d'actions optimale via… **A\*** sur l'espace des états (voir section suivante).

### Navigation

La **navigation** des PNJ nécessite de **planifier un chemin** entre deux points en évitant les obstacles. L'algorithme de référence est **A\*** (« A étoile »), inventé en 1968 et toujours utilisé tel quel dans les moteurs modernes.

#### A\* — la fonction d'évaluation

A\* explore un graphe (cases d'une grille, sommets d'un mesh de navigation, voire ECS d'actions GOAP) en évaluant à chaque nœud :

```math
f(n) = g(n) + h(n)
```

où :

- $g(n)$ est le **coût réel** déjà parcouru pour aller du départ à $n$ (somme des poids d'arêtes).
- $h(n)$ est une **heuristique** : une *estimation* du coût restant entre $n$ et le but.

À chaque itération, A\* sort de la file de priorité le nœud de plus petit $f$, l'expand, et enregistre ses voisins.

#### Pourquoi l'admissibilité de l'heuristique compte

> **Heuristique admissible** : qui ne **surestime jamais** le coût réel restant. Formellement : $h(n) \le h^\ast(n)$ pour tout $n$, où $h^\ast(n)$ est le coût optimal réel entre $n$ et le but.

**Théorème** : si $h$ est admissible, A\* trouve **toujours** le chemin optimal.

*Idée de preuve* : supposons par l'absurde qu'A\* renvoie un chemin sous-optimal. Alors il existe au moins un nœud $n$ sur le chemin optimal qui est dans la frontière non explorée au moment où A\* termine. Pour ce $n$, on a $f(n) = g(n) + h(n) \le g(n) + h^\ast(n) = \text{coût optimal}$, donc $f(n) \le f(\text{but trouvé})$, ce qui contredit le fait qu'A\* a choisi le but trouvé en premier. ∎

**Heuristiques classiques sur grille** :

| Distance        | Formule                                                 | Admissible si déplacement                       |
| --------------- | ------------------------------------------------------- | ----------------------------------------------- |
| **Manhattan**   | $\lvert\Delta x\rvert + \lvert\Delta y\rvert$           | 4-connexe (haut/bas/gauche/droite uniquement)   |
| **Chebyshev**   | $\max(\lvert\Delta x\rvert, \lvert\Delta y\rvert)$      | 8-connexe avec coût uniforme                    |
| **Octile**      | $\Delta_\text{max} + (\sqrt{2} - 1)\,\Delta_\text{min}$ | 8-connexe avec coût $\sqrt{2}$ en diagonal      |
| **Euclidienne** | $\sqrt{\Delta x^2 + \Delta y^2}$                        | toujours (sous-estime sur graphe discret)       |

> **Heuristique inadmissible** : A\* **trouve toujours** un chemin (s'il existe), mais pas forcément l'optimal. Volontairement, on utilise parfois une heuristique légèrement sur-estimante pour **accélérer** A\* au prix d'un peu d'optimalité — c'est *Weighted A\**.

#### Optimisations courantes

- **Hierarchical Pathfinding (HPA\*)** : on précalcule un graphe coarse (clusters de cases). On planifie d'abord à gros grain, puis on raffine à grain fin sur le segment courant. Indispensable au-delà de 1000×1000 cases.
- **Jump Point Search (JPS)** : sur une grille uniforme 8-connexe, on saute directement aux points "intéressants", divisant le nombre de nœuds explorés par 10-100×.
- **Theta\*** : variante qui autorise à "couper les coins" (any-angle), résultats visuellement plus naturels que les chemins en escalier.

#### MCTS — Monte Carlo Tree Search

Pour les jeux à grande arborescence (Go, certains RTS), A\* sur l'espace des états est intractable. **MCTS** (*Monte Carlo Tree Search*, 2006) explore l'arbre en quatre phases répétées :

- **Selection.** On descend dans l'arbre depuis la racine en choisissant à chaque pas l'enfant qui maximise la formule **UCB1** (*Upper Confidence Bound*) :

```math
UCB1(n) = \frac{w_n}{v_n} + c\sqrt{\frac{\ln V_p}{v_n}}
```

 où $w_n$ est le nombre de victoires depuis le nœud $n$, $v_n$ le nombre de visites, $V_p$ les visites du parent, et $c \approx \sqrt{2}$ règle l'arbitrage exploration ↔ exploitation.

- **Expansion.** Si le nœud sélectionné n'est pas terminal, on ajoute un nouvel enfant non encore exploré.
- **Simulation** (*rollout*). On joue ensuite la partie aléatoirement (ou avec une politique légère) jusqu'à la fin pour obtenir un résultat.
- **Backpropagation.** On remonte le résultat dans l'arbre, mettant à jour $w$ et $v$ sur tous les nœuds visités.

C'est l'algorithme qui a fait gagner **AlphaGo** contre Lee Sedol (2016, en combinaison avec un réseau de neurones d'évaluation de position).

### Apprentissage automatique

> **Réseau de neurones (NN)** : un graphe de calcul composé de **couches** d'unités élémentaires (neurones) qui transforment leurs entrées via une combinaison linéaire pondérée suivie d'une **fonction d'activation** non-linéaire (ReLU, sigmoid…). Les **poids** des connexions sont appris à partir de données par **rétropropagation du gradient**.

Trois grands paradigmes en jeu vidéo :

- **Apprentissage supervisé** : on entraîne le réseau à imiter une cible. Utilisé pour la reconnaissance de gestes via Kinect, la transcription speech-to-text dans le chat in-game.
- **Apprentissage par renforcement (RL)** : l'agent apprend une **politique** $\pi(s) \to a$ qui maximise une **récompense cumulée** $\sum_t \gamma^t r_t$. Algorithmes connus : Q-learning, **DQN** (DeepMind sur Atari, 2015), **PPO** (OpenAI Five sur Dota 2, 2018), **AlphaZero** (DeepMind, échecs/Go/shogi, 2017). $\gamma \in [0, 1[$ est le **facteur d'actualisation** : il pénalise les récompenses lointaines pour les rendre comparables.
- **Modèles génératifs** (LLM, diffusion) : génération de dialogues, de quêtes ou de textures à la volée. Encore expérimental côté production, prometteur pour le contenu procédural narratif (*AI Dungeon*, *Inworld AI* dans *Mecha BREAK*).

```math
Q(s, a) \leftarrow Q(s, a) + \alpha\,\Big[r + \gamma \max_{a'} Q(s', a') - Q(s, a)\Big]
\quad\text{(mise à jour de Bellman, Q-learning)}
```

> **Le RL fonctionne brillamment dans des environnements simulables**. Pour les jeux multijoueurs en ligne, on n'utilise quasi jamais du RL temps réel : le coût d'inférence + le risque de comportements aberrants sont prohibitifs. Le RL sert plutôt à **entraîner** des politiques offline, qu'on **distille** ensuite en arbres de décision ou tables de lookup pour le runtime (technique utilisée par *Forza Motorsport* pour le drivatar).

[ Retour en haut de page](#table-des-matières)

---

## Réseau et multijoueur

Le **réseau** et le **multijoueur** introduisent une dimension supplémentaire dans les jeux vidéo : la **synchronisation** d'un état partagé entre plusieurs machines, malgré la latence et la perte de paquets. Cette section présente les modèles d'architecture, les protocoles utilisés et les principaux défis de la programmation multijoueur.

### Modèles de réseau

Les jeux multijoueurs en réseau reposent sur différentes architectures pour synchroniser les données entre les joueurs. Les deux modèles principaux sont le modèle **client-serveur** et le modèle **peer-to-peer** (P2P).

- **Client-serveur** : un serveur central gère l'état du jeu et communique avec les clients (les joueurs). Les clients envoient des informations sur leurs actions au serveur, qui met à jour l'état du jeu et envoie des mises à jour aux clients. Le serveur est responsable de la synchronisation des données et de la gestion des conflits entre les clients. C'est le modèle dominant pour les jeux compétitifs (il facilite l'**autorité serveur** et la lutte anti-triche).

```mermaid
graph TB
A1(Client) --> B(Serveur)
A2(Client) --> B
A3(Client) --> B
B --> A1
B --> A2
B --> A3
```

- **Peer-to-peer** : les joueurs se connectent directement les uns aux autres sans passer par un serveur central. Chaque joueur est responsable de la synchronisation de son propre état de jeu avec les autres joueurs. Ce modèle peut être plus efficace en termes de bande passante et de latence, mais il peut également être plus complexe à mettre en œuvre, en particulier pour les jeux avec un grand nombre de joueurs.

```mermaid
graph TB
A1(Client) --> A2(Client)
A1 --> A3(Client)
A2 --> A1
A2 --> A3
A3 --> A1
A3 --> A2
```

### Protocoles de communication

Les jeux en réseau utilisent différents protocoles de communication pour échanger des données entre les joueurs. Les deux protocoles les plus courants sont :

- **UDP** (*User Datagram Protocol*) : protocole de communication **sans connexion** et **sans garantie** de livraison. Généralement utilisé dans les jeux en temps réel en raison de sa faible **latence**. Cependant, les paquets de données peuvent être perdus ou arriver dans le désordre, ce qui nécessite une gestion supplémentaire (numéros de séquence, retransmissions sélectives) de la part du programme de jeu.
- **TCP** (*Transmission Control Protocol*) : protocole de communication **orienté connexion** avec **garantie** de livraison. Il garantit que les paquets de données sont livrés dans l'ordre et sans erreurs. Généralement utilisé pour les communications non critiques pour le temps, telles que le chat en jeu, la mise à jour des classements ou le téléchargement d'assets.

> De plus en plus de jeux modernes utilisent **WebRTC** ou **QUIC** (basé sur UDP), qui combinent fiabilité, faible latence et chiffrement.

#### Synchronisation et latence

 **Vocabulaire de base :**

- **RTT** (*Round-Trip Time*) : temps aller-retour d'un paquet entre client et serveur (typiquement 30-150 ms en jeu compétitif).
- **Tick rate** : fréquence à laquelle le serveur calcule un nouvel état (Counter-Strike 2 = 64 Hz, Valorant = 128 Hz).
- **Snapshot** : un état complet du monde envoyé du serveur aux clients à chaque tick (positions, animations, vies…).
- **Input** : action du joueur (déplacement, tir) envoyée du client au serveur, à chaque frame.

Un jeu multijoueur typique tourne avec un client à 144 Hz qui parle à un serveur à 64 Hz, sur un réseau qui ajoute 50 ms de latence et perd 1 % des paquets. Naïvement, ça produit un jeu injouable. Quatre techniques essentielles le rendent fluide :

##### 1. Snapshot interpolation (côté client, pour les autres joueurs)

Le client n'affiche **pas** la dernière position reçue : il **retarde volontairement** son rendu de quelques snapshots et **interpole** entre deux snapshots passés. Cela donne un mouvement fluide même quand un paquet manque.

```math
S(t) = (1 - \alpha)\,S_0 + \alpha\,S_1
\qquad
\alpha = \frac{t - t_0}{t_1 - t_0}
```

où $S_0, S_1$ sont les deux snapshots qui encadrent le temps $t$. La règle pratique est de retarder de **2× la période du tick rate** : sur un serveur 64 Hz, on rend les autres joueurs avec 30 ms de retard. *Counter-Strike*, *Valorant*, *Overwatch* font tous ça.

##### 2. Client-side prediction (côté client, pour soi-même)

On ne peut pas attendre l'aller-retour serveur pour bouger son propre personnage : à 100 ms RTT, ça donnerait l'impression d'un input lag insupportable. La technique :

1. Le client **simule immédiatement** l'effet de son input.
2. Il **mémorise l'input avec son numéro de séquence** $n$.
3. Quand le serveur renvoie son state autoritaire avec le numéro $n$, le client **compare** :
 - Si l'état serveur correspond, RAS.
 - Sinon (ping-pong, collision contestée), le client **corrige sa position** et **rejoue tous les inputs postérieurs** $n+1, n+2, \dots$ — c'est la **reconciliation**.

```python
# Pseudocode côté client
inputs = []   # historique des inputs envoyés
for input_n at tick t:
 state.apply(input_n)         # prédiction immédiate
 inputs.append((t, input_n))
 send_to_server(t, input_n)

on receive (server_state, server_acked_tick):
 state = server_state          # rebase sur l'état autoritaire
 for (t, i) in inputs if t > server_acked_tick:
 state.apply(i)             # rejouer la queue
 inputs = inputs[server_acked_tick+1:]
```

C'est la technique fondatrice de Quake World (1996, John Carmack). Toujours utilisée presque telle quelle.

##### 3. Lag compensation (côté serveur, pour les hits)

Quand un joueur tire, le serveur reçoit l'input avec un délai $\Delta = \text{RTT}/2 + t_\text{interp}$. Si le serveur valide le tir contre les positions actuelles, le joueur a déjà raté la cible (la cible a bougé pendant $\Delta$). Solution : le serveur **rembobine** le monde de $\Delta$ et vérifie le hit dans le passé.

```math
\Delta_\text{rewind} = \frac{\text{RTT}_\text{client}}{2} + t_\text{interpolation}
```

Le serveur garde un buffer circulaire des dernières snapshots (~1 s d'historique). C'est ce qui produit l'effet "j'ai été tué après m'être caché derrière le mur" : du point de vue du tireur, il vous voyait encore exposé.

##### 4. Lockstep déterministe (alternative aux snapshots)

Pour les RTS (*Age of Empires*, *StarCraft*, *Factorio*), envoyer 200 unités de positions à chaque tick est trop cher. À la place :

- Tous les clients démarrent avec la **même seed** et le **même état initial**.
- À chaque tick, tous les clients reçoivent **uniquement les inputs de tous les joueurs** (volume négligeable).
- Chaque client simule la totalité de la partie de manière **bit-exact**.

Pour que ça marche, **toute** la simulation doit être déterministe : pas de `Random` non-seedé, pas de comparaison d'adresses mémoire, pas d'arithmétique flottante non-déterministe (selon l'ordre d'exécution). Les RTS modernes (*Factorio*, *Age of Empires II Definitive*) utilisent même de l'**arithmétique en virgule fixe** pour éviter les divergences entre CPUs.

> **Le piège du flottant en lockstep.** L'instruction `x87` du x86 vs SSE vs ARM ne donnent pas les mêmes derniers bits sur `sin()`, `sqrt()`. Sur un FPS classique on s'en fiche. En lockstep, deux clients divergent en quelques minutes et la partie devient injouable. Solution : compiler avec `--ffast-math` désactivé, utiliser une lib math soft-float croisée comme `softfloat`, ou rester en virgule fixe.

### Programmation de jeu multijoueur

La programmation de jeu multijoueur implique la gestion de la **synchronisation** des données entre les joueurs, la **détection** et la **résolution des conflits**, et la **gestion des erreurs** de réseau. Les développeurs de jeux doivent également prendre en compte des problèmes tels que la **latence**, la **bande passante** et la **sécurité**.

Pour gérer ces problèmes, les développeurs peuvent utiliser des bibliothèques de réseau spécifiques au jeu (comme **ENet**, **yojimbo** ou **GameNetworkingSockets**) ou des moteurs de jeu intégrant des fonctionnalités réseau (Unity Netcode, Unreal Replication, Godot High-Level Multiplayer, Photon, Mirror, etc.).

Les développeurs doivent également implémenter des mécanismes pour gérer les **déconnexions** de joueurs, les **tricheurs** et les **attaques par déni de service**.

> **Lecture obligatoire** : ["What Every Programmer Needs To Know About Game Networking"](https://gafferongames.com/post/what_every_programmer_needs_to_know_about_game_networking/) de Glenn Fiedler. Court, dense, applicable.

[ Retour en haut de page](#table-des-matières)

---

## Techniques avancées

Cette section regroupe quelques **techniques avancées** que l'on retrouve dans les jeux modernes. Elles s'appuient sur tout ce qui a été vu précédemment — vecteurs, matrices, équations différentielles, IA — mais en poussent les limites pour atteindre un niveau de réalisme ou de complexité supérieur.

### Génération procédurale et bruit

La **génération procédurale** consiste à produire du contenu (terrain, textures, niveaux, biomes…) à partir d'algorithmes plutôt que manuellement. Elle s'appuie largement sur des **fonctions de bruit** déterministes, c'est-à-dire des fonctions qui retournent toujours la même valeur pour une même entrée mais qui semblent aléatoires.

> **Qu'est-ce qu'un "bruit" en programmation graphique ?** Pas un son. C'est juste une **fonction mathématique** $f(x, y)$ qui, pour chaque coordonnée du plan (ou de l'espace), retourne une valeur "désordonnée mais reproductible". On les dessine en niveau de gris (`f(x, y)` → noir si bas, blanc si haut). On dit que le bruit est **déterministe** parce que `f(3, 5)` retourne toujours la même chose, et **continu** quand on peut zoomer dessus sans voir d'arêtes franches. On les utilise pour générer des terrains, des textures, des dispositions d'objets, qui ont l'air "naturels" parce que la nature elle-même est faite de variations continues.

#### Bruit blanc

> **Définition.** Le **bruit blanc** est le bruit le plus simple : à chaque case entière, une valeur indépendante des voisines. Le nom vient de l'analogie avec la *lumière blanche* (qui contient toutes les fréquences avec la même intensité) et le **son blanc** (le sifflement statique d'une radio mal réglée). Visuellement, c'est ce qu'on voit sur la "neige" d'une vieille télé : chaque pixel est tiré au sort sans aucun lien avec ses voisins.

Formellement :

```math
N_\text{blanc}(x, y) = \mathrm{hash}(x, y) \in [0, 1]
```

> **Pourquoi c'est inutilisable seul.** Comme chaque pixel est indépendant, deux pixels voisins peuvent avoir des valeurs totalement différentes. Si on l'utilisait pour un terrain, le sol monterait à 100 m puis redescendrait à 0 d'un mètre à l'autre — injouable. Le bruit blanc sert d'**ingrédient de base** dans des constructions plus sophistiquées (bruit de valeur, bruit fractal) qui le **lissent**.

#### Bruit de Perlin

> **Définition.** Le **bruit de Perlin** est une fonction continue et lisse, inventée par **Ken Perlin** en 1983 pour générer les textures du film *Tron*. Elle ressemble à un terrain vu de dessus : des vallées et des collines qui se succèdent sans cassure. Perlin a reçu un Oscar technique en 1997 pour son invention. Aujourd'hui, *Minecraft*, *No Man's Sky*, *Terraria* et la quasi-totalité des jeux à monde ouvert reposent sur Perlin (ou son successeur Simplex).

L'idée intuitive : on découpe l'espace en une grille de cellules entières. À chaque sommet de la grille, on stocke un **vecteur gradient** pseudo-aléatoire (une direction). Quand on demande la valeur en un point $(x, y)$ quelconque, on combine les contributions des 4 sommets de la cellule qui contient le point, en les pondérant selon la position relative dans la cellule.

Concrètement, pour un point $(x, y)$ :

1. Trouver la cellule entière qui contient $(x, y)$ ;
2. Pour chaque coin de la cellule, lire (ou calculer par hachage) un **gradient pseudo-aléatoire** ;
3. Calculer le produit scalaire entre le gradient et le vecteur du coin vers $(x, y)$ ;
4. **Interpoler** ces produits scalaires avec une fonction d'easing (typiquement `smootherstep` — voir le chapitre Easing pour le pourquoi).

```math
\mathrm{smootherstep}(t) = 6t^5 - 15t^4 + 10t^3
```

> **Bruit de Simplex** (Perlin, 2001) : amélioration directe, qui utilise une grille triangulaire/tétraédrique au lieu d'une grille carrée/cubique. Avantages : moins de calculs en haute dimension (3D, 4D), moins d'artefacts directionnels (pas de "rails" alignés sur les axes). Inconvénient : un brevet historique de Perlin a longtemps freiné son adoption ; aujourd'hui le brevet est expiré et OpenSimplex / OpenSimplex2 sont les implémentations libres de référence.

#### Bruit fractal (FBM — *Fractional Brownian Motion*)

> **Définition.** **FBM** signifie *Fractional Brownian Motion* — mouvement brownien fractionnaire. C'est l'analogue mathématique du mouvement aléatoire d'une particule en suspension (Robert Brown, 1827), mais corrélé sur plusieurs échelles. En pratique : un terrain a des montagnes, des collines sur ces montagnes, des rochers sur ces collines, des cailloux sur ces rochers — *des détails à toutes les échelles*. La FBM reproduit ça en superposant plusieurs couches du même bruit, chacune deux fois plus fine et deux fois moins forte que la précédente. Le résultat ressemble à un vrai paysage.

```math
\mathrm{FBM}(x, y) = \sum_{i=0}^{N-1} \frac{1}{2^i} \cdot \mathrm{noise}\!\big(2^i \cdot x,\ 2^i \cdot y\big)
```

Chaque terme s'appelle une **octave** (terme musical : doubler la fréquence revient à monter d'une octave). Avec 4-6 octaves on obtient un terrain crédible.

#### Applications

- **Heightmap de terrain** : Minecraft, *No Man's Sky*, *Terraria* utilisent du bruit fractal pour générer la carte d'élévation du sol.
- **Textures procédurales** : marbre, bois, nuages, eau — le motif "fibreux" d'un tronc vient d'une FBM filtrée.
- **Distribution d'objets** : seuiller un bruit ($N(x, y) > 0{,}7$ → "pose un arbre ici") donne des forêts naturelles, sans agglomérats.
- **Donjons et niveaux** : on utilise plutôt des algorithmes spécialisés.

**Lexique des algos de génération de niveau :**

- **BSP** (*Binary Space Partitioning*) : on découpe une zone en deux sous-zones, récursivement, jusqu'à obtenir des pièces. Utilisé par *Rogue* (1980) et la quasi-totalité des roguelikes.
- **Wave Function Collapse** (Maxim Gumin, 2016) : algorithme inspiré de la physique quantique où chaque case "choisit" un motif compatible avec ses voisines. Donne des résultats étonnamment cohérents à partir d'un simple échantillon.
- **Marche aléatoire** (*random walk*) : un agent virtuel se déplace au hasard et creuse les cases qu'il visite. Donne des cavernes organiques.

```csharp
float Terrain(float x, float z)
{
 float h = 0f;
 float amplitude = 1f, frequency = 0.01f;
 for (int i = 0; i < 5; i++)
 {
 h += amplitude * Mathf.PerlinNoise(x * frequency, z * frequency);
 amplitude *= 0.5f;
 frequency *= 2f;
 }
 return h;
}
```

### Physique des fluides

La **simulation de fluides** dans les jeux vidéo est une technique avancée qui permet de reproduire le comportement des **liquides** et des **gaz**. Les fluides sont généralement simulés à l'aide d'équations aux dérivées partielles, telles que les **équations de Navier-Stokes** :

```math
\rho \left( \frac{\partial \mathbf{v}}{\partial t} + \mathbf{v} \cdot \nabla \mathbf{v} \right) = -\nabla p + \mu \nabla^2 \mathbf{v} + \mathbf{f}
```

où $\rho$ est la densité du fluide, $\mathbf{v}$ son champ de vitesse, $p$ la pression, $\mu$ la viscosité dynamique et $\mathbf{f}$ les forces externes.

Les méthodes de résolution numérique, telles que la **méthode des différences finies**, la **méthode des éléments finis** ou les méthodes **SPH** (*Smoothed Particle Hydrodynamics*), sont utilisées pour résoudre ces équations et générer des animations réalistes de fluides.

### Écrans multiples et fenêtrage

Les jeux modernes offrent souvent la possibilité de jouer sur **plusieurs écrans** ou dans des **fenêtres redimensionnables**. Cette fonctionnalité nécessite une gestion avancée du rendu et de la résolution d'affichage, ainsi que la prise en charge de plusieurs moniteurs et configurations de fenêtres.

Les développeurs de jeux doivent tenir compte :

- de la **synchronisation** entre les écrans (taux de rafraîchissement potentiellement différents) ;
- des **performances graphiques** (un setup multi-écrans multiplie le nombre de pixels à rendre) ;
- des **ratios** d'affichage (16:9, 21:9 ultrawide, 32:9 super ultrawide) qui imposent un FOV adapté pour éviter la déformation.

### Intelligence artificielle avancée

L'**intelligence artificielle avancée** dans les jeux vidéo englobe des techniques telles que l'**apprentissage automatique**, la **planification**, la **prise de décision** et le **traitement du langage naturel**. Ces techniques permettent de créer des PNJ plus réalistes et convaincants, ainsi que des systèmes de jeu **dynamiques** et **adaptatifs**.

Les développeurs de jeux peuvent utiliser des bibliothèques et des frameworks d'IA spécifiques pour implémenter ces fonctionnalités, comme **TensorFlow**, **PyTorch**, **ONNX Runtime** ou les API d'**OpenAI**. Ces outils permettent d'entraîner des modèles d'apprentissage profond pour la reconnaissance d'image, la génération de texte, la synthèse vocale et d'autres tâches complexes.

> De plus en plus de jeux intègrent des **modèles génératifs** (LLM, diffusion) pour produire dialogues, missions ou textures à la volée.

### Rendu avancé

Le **rendu avancé** englobe les techniques modernes qui rapprochent l'image générée de la photographie. Cette sous-section les présente et donne, pour chacune, **la math sous-jacente** quand il y en a — pas juste le nom commercial.

#### Rendu basé sur la physique (PBR) et l'équation de rendu

> **PBR** = *Physically-Based Rendering*. Au lieu d'inventer des modèles d'éclairage à la louche (Phong, Lambert), on part de l'équation physique vraie qui décrit comment la lumière interagit avec une surface, puis on approxime intelligemment.

L'**équation de rendu** (Kajiya, 1986) décrit, pour une surface au point $\mathbf{x}$, la lumière sortante dans la direction $\boldsymbol{\omega}_o$ :

```math
L_o(\mathbf{x}, \boldsymbol{\omega}_o) = L_e(\mathbf{x}, \boldsymbol{\omega}_o) + \int_{\Omega} f_r(\mathbf{x}, \boldsymbol{\omega}_i, \boldsymbol{\omega}_o)\,L_i(\mathbf{x}, \boldsymbol{\omega}_i)\,(\boldsymbol{\omega}_i \cdot \mathbf{n})\,\mathrm{d}\boldsymbol{\omega}_i
```

où :

- $L_e$ est l'émission propre de la surface (matériau lumineux, écran).
- $L_i$ est la lumière incidente venue de la direction $\boldsymbol{\omega}_i$.
- $f_r$ est la **BRDF** (*Bidirectional Reflectance Distribution Function*) : combien de la lumière entrant par $\boldsymbol{\omega}_i$ ressort vers $\boldsymbol{\omega}_o$.
- $\boldsymbol{\omega}_i \cdot \mathbf{n}$ est le **terme de Lambert** : un faisceau qui frappe la surface à 45° apporte moins d'énergie au m² qu'un faisceau perpendiculaire.
- $\Omega$ est l'hémisphère au-dessus de la surface.

L'intégrale est insolvable analytiquement → on l'approxime. Les jeux temps réel utilisent une **BRDF Cook-Torrance microfacets** :

```math
f_r = \underbrace{\frac{c_\text{diff}}{\pi}}_\text{Lambertien} + \underbrace{\frac{D \cdot F \cdot G}{4\,(\boldsymbol{\omega}_o \cdot \mathbf{n})\,(\boldsymbol{\omega}_i \cdot \mathbf{n})}}_\text{Spéculaire microfacets}
```

avec :

- **D — distribution des normales** (GGX/Trowbridge-Reitz, le standard depuis ~2014) : décrit la "rugosité" perceptuelle de la surface.
- **F — Fresnel** (Schlick) : plus on regarde une surface en rasant, plus elle réfléchit comme un miroir. C'est ce qui fait briller le bord d'une bouteille.
- **G — masquage / ombrage** (Smith) : sur une surface très rugueuse, certaines microfacettes sont cachées par d'autres.

 **Paramètres PBR exposés à l'artiste.** Ce qu'on lui demande de peindre dans des textures :

- **Albedo** (RGB) : la couleur de base (sans aucune lumière).
- **Metallic** ($\in [0, 1]$) : 0 = diélectrique (bois, peau, peinture), 1 = métal pur (or, fer poli).
- **Roughness** ($\in [0, 1]$) : 0 = miroir, 1 = surface mate parfaitement diffuse.
- **Normal map** (RGB encodant un vecteur 3D) : perturbe la normale géométrique pour simuler des micro-bosses sans ajouter de polygones.

Avec ces 4 paramètres, on reproduit fidèlement 95 % des matériaux du monde physique (peau, métal, plastique, tissu, vitre, eau…). C'est ce qui rend les jeux modernes interopérables : un asset Substance Painter charge directement dans Unity, Unreal et Blender sans retravail.

#### Occlusion ambiante (*Ambient Occlusion*, AO)

Approximation peu coûteuse de l'éclairage indirect : on assombrit les zones où la lumière ambiante peinerait à pénétrer (coins, replis). L'**AO** estime un facteur $A(\mathbf{x}) \in [0, 1]$ multiplicatif :

```math
A(\mathbf{x}) = \frac{1}{\pi} \int_{\Omega} V(\mathbf{x}, \boldsymbol{\omega})\,(\boldsymbol{\omega} \cdot \mathbf{n})\,\mathrm{d}\boldsymbol{\omega}
```

où $V \in \{0, 1\}$ est la **visibilité** dans la direction $\boldsymbol{\omega}$ (1 si rien ne bloque, 0 sinon). En temps réel on en fait une approximation en screen-space :

- **SSAO** (*Screen-Space Ambient Occlusion*, Crytek 2007) : sample le depth buffer autour de chaque pixel.
- **HBAO** (*Horizon-Based*, NVIDIA 2008) : raffinement angulaire, plus précis.
- **GTAO** (*Ground-Truth*, 2016) : référence actuelle, presque indistinguable d'un rendu offline.

#### Tessellation et displacement mapping

> **Tessellation** : subdiviser dynamiquement un maillage en plus de triangles à l'approche du spectateur. **Displacement** : déplacer les nouveaux sommets selon une *heightmap* (texture en niveaux de gris) pour créer du vrai relief géométrique (et pas juste une normal map qui ment au rasterizer).

Combinés, ils créent un terrain ou une muraille qui semble découpée au burin, sans payer le coût mémoire d'un mesh aussi détaillé. Le pipeline GPU (DX11+, OpenGL 4.0+) fournit deux étages dédiés : **Hull Shader** + **Domain Shader**.

#### Ray tracing — le tracé de rayons

> **Idée**. Au lieu de projeter des triangles puis d'éclairer chaque pixel a posteriori, on tire un rayon depuis la caméra à travers chaque pixel et on regarde **ce qu'il rencontre** dans la scène. C'est l'**inversion physique** de la propagation de la lumière (les vrais photons partent des sources, pas de l'œil).

Pour un rayon $R(t) = \mathbf{O} + t\,\mathbf{D}$ et une sphère $\|\mathbf{P} - \mathbf{C}\|^2 = r^2$, l'intersection se résout par une simple équation du second degré :

```math
\|t\,\mathbf{D} + (\mathbf{O} - \mathbf{C})\|^2 = r^2
\;\Rightarrow\;
t^2(\mathbf{D}\cdot\mathbf{D}) + 2t\,\mathbf{D}\cdot(\mathbf{O}-\mathbf{C}) + \|\mathbf{O}-\mathbf{C}\|^2 - r^2 = 0
```

Pour des triangles, on utilise l'algorithme **Möller-Trumbore** (1997) qui donne directement les coordonnées barycentriques et la profondeur en quelques produits vectoriels. Les GPUs RTX/RDNA2+ embarquent une unité matérielle (**RT cores**) qui accélère ce test des centaines de fois.

#### Path tracing — la généralisation

Au lieu de s'arrêter à la première intersection, on **rebondit** : à chaque touche, on tire un nouveau rayon dans une direction échantillonnée selon la BRDF, jusqu'à atteindre une source lumineuse. C'est une **estimation Monte-Carlo** de l'intégrale de rendu :

```math
L_o \approx \frac{1}{N} \sum_{k=1}^{N} \frac{f_r(\boldsymbol{\omega}_i^{(k)}, \boldsymbol{\omega}_o)\,L_i(\boldsymbol{\omega}_i^{(k)})\,(\boldsymbol{\omega}_i^{(k)} \cdot \mathbf{n})}{p(\boldsymbol{\omega}_i^{(k)})}
```

où $p$ est la **densité de probabilité** d'échantillonnage. Plus $N$ est grand, moins l'image est bruitée. Quake II RTX, Cyberpunk 2077 Path Tracing et Portal RTX sont des path tracers temps réel ; ils s'appuient sur des **denoisers neuronaux** (NVIDIA OptiX) pour rendre une image propre avec seulement 1-2 échantillons par pixel.

#### Global illumination

Tous les rebonds **autres que la première intersection** forment l'**illumination globale**. En temps réel on l'approxime :

- **Lightmaps précalculées** (Quake, 1996) — les surfaces statiques ont leur éclairage cuit dans des textures. Coût zéro à l'exécution mais inutilisable sur géométrie dynamique.
- **Voxel cone tracing** (*VXGI*, NVIDIA 2014) — voxellise la scène, propage la lumière dans la grille.
- **Probes irradiance** (*Light Probes*, *DDGI*) — on échantillonne l'irradiance en quelques points stratégiques et on interpole.
- **Lumen** (Unreal Engine 5, 2022) — combinaison hybride de surface caching + screen tracing + (optionnel) ray tracing matériel.

#### Upscaling temporel — DLSS, FSR, XeSS

Le rendu d'image en 4K coûte 4× le rendu 1080p. La parade : rendre à résolution interne plus basse (souvent 1440p ou 1080p) puis **reconstruire** la 4K en exploitant les **frames précédentes**. C'est le rôle des techniques d'upscaling :

- **DLSS** (NVIDIA, 2018) : réseau convolutionnel entraîné offline sur des images 16K de référence.
- **FSR 2/3** (AMD, 2022) : algorithmique pur (heuristiques + reprojection + accumulation), tourne sur tout GPU.
- **XeSS** (Intel, 2022) : approche neurale similaire à DLSS, plus portable.
- **Frame Generation** (DLSS 3, FSR 3) : interpole / extrapole une frame intermédiaire à partir de deux frames rendues + des vecteurs de mouvement, pour doubler le framerate perçu.

Mathématiquement, ce sont des **filtres de reprojection temporelle** :

```math
C_t(\mathbf{p}) = \alpha\,C_t^\text{rendu}(\mathbf{p}) + (1 - \alpha)\,C_{t-1}(\mathbf{p} + \mathbf{v})
```

où $\mathbf{v}$ est le **vecteur de mouvement** (motion vector) qui dit où le pixel `p` se trouvait à la frame précédente. La pondération $\alpha$ (0.1 typique) limite le ghosting des objets en mouvement.

[ Retour en haut de page](#table-des-matières)

---

## Pipeline de rendu

Le **pipeline de rendu** est un processus séquentiel qui convertit les objets 3D et les textures du jeu en images 2D affichées à l'écran. Il comprend plusieurs étapes — depuis la transformation des objets 3D dans le repère du monde jusqu'à l'affichage final.

### Étapes du pipeline

```mermaid
graph TD
A(Objets 3D) --> B(Transformation)
B --> C(Projection)
C --> D(Calcul des ombres et de l'éclairage)
D --> E(Rendu des textures et effets spéciaux)
E --> F(Image 2D)
```

À un niveau plus détaillé, le pipeline d'un GPU moderne ressemble à :

```mermaid
graph LR
V[Vertex shader] --> T[Tessellation]
T --> G[Geometry shader]
G --> R[Rasterization]
R --> F[Fragment shader]
F --> O[Output merger]
```

### Culling et occlusion

Le **culling** et l'**occlusion** sont des techniques utilisées pour optimiser le rendu graphique en éliminant les objets ou les parties d'objets qui ne sont pas visibles à l'écran.

- Le **culling** se concentre sur l'élimination des **objets entiers** qui sont en dehors du champ de vision de la caméra (*frustum culling*) ou orientés à l'opposé (*backface culling*).
- L'**occlusion** élimine les **parties d'objets** qui sont cachées derrière d'autres objets (*occlusion culling*, *Z-buffer*, *Hi-Z*).

```mermaid
graph LR
A(Culling) -- Élimine les objets hors champ --> B(Optimisation du rendu)
C(Occlusion) -- Élimine les parties d'objets cachées --> B
```

### Shaders

Les **shaders** sont des programmes qui sont exécutés sur les **unités de traitement graphique** (GPU) pour déterminer les caractéristiques visuelles des objets affichés à l'écran.

Généralement écrits dans des langages spécifiques au GPU, tels que **GLSL** (*OpenGL Shading Language*), **HLSL** (*High-Level Shading Language*) ou **WGSL** (*WebGPU Shading Language*), ils permettent de créer des effets spéciaux, tels que les **réflexions**, les **ombres** et les **animations de texture**.

> Exemple : un effet de brouillard, où le shader applique un effet de flou et de couleur uniforme sur les pixels les plus éloignés de la caméra.

En somme, le but d'un shader est de **personnaliser l'apparence visuelle** des objets à l'écran. Nous allons ici aborder les vertex shaders, geometry shaders et les fragment shaders.

```mermaid
graph TD
A(Vertex shaders) --> B(Shaders)
C(Geometry shaders) --> B
D(Fragment shaders) --> B
B --> E(Effets spéciaux)
```

#### Vertex shaders

Les [vertex](https://github.com/tanguychenier/Terminal_3DEngine) shaders sont des programmes exécutés sur **chaque sommet** des objets lors de leur rendu. Ils sont utilisés pour transformer les positions des sommets, en appliquant des transformations linéaires sur les coordonnées des sommets.

> Les objets 3D sont généralement définis par un ensemble de **sommets**, qui sont reliés entre eux par des **arêtes** pour former des polygones, tels que des triangles ou des quadrilatères.
>
> Les vertex shaders sont appliqués à chaque sommet de ces polygones lors du rendu, pour déterminer la position finale de chaque sommet dans l'image affichée à l'écran.

##### Fonctionnement du vertex shader

Chaque sommet est représenté par un vecteur de position homogène $\mathbf{v}_h$, qui peut être transformé en un nouveau vecteur de position homogène $\mathbf{v}'_h$ par l'application d'une matrice de transformation homogène $M_{VS}$ représentant le vertex shader :

```math
\mathbf{v}'_h = M_{VS} \, \mathbf{v}_h
```

La matrice de transformation $M_{VS}$ peut être construite en combinant plusieurs types de transformations linéaires, telles que la translation, la rotation et la mise à l'échelle. Ces transformations peuvent être représentées par des matrices de transformation homogène 4×4.

Par exemple, pour effectuer une translation de vecteur $\mathbf{t} = (t_x, t_y, t_z)$, on peut construire la matrice de translation homogène $T$ :

```math
T = \begin{pmatrix} 1 & 0 & 0 & t_x \\ 0 & 1 & 0 & t_y \\ 0 & 0 & 1 & t_z \\ 0 & 0 & 0 & 1 \end{pmatrix}
```

On peut ensuite combiner plusieurs transformations en multipliant les matrices correspondantes. Par exemple, pour effectuer une translation suivie d'une rotation autour de l'axe des $y$ d'un angle $\theta$ :

```math
M_{VS} = R_y(\theta) \cdot T
```

où $R_y(\theta)$ est la matrice de rotation homogène autour de l'axe des $y$.

##### Pipeline de données

```mermaid
graph LR
A(Texture d'entrée) --> B(Shader)
B --> C(Cible de rendu)
C --> D(Texture de sortie)
```

Bien que les détails de ces opérations dépendent du langage de programmation utilisé pour écrire les shaders (GLSL, HLSL, WGSL...), ils peuvent également effectuer d'autres opérations sur les sommets : application de textures, génération de coordonnées de texture, ou envoi de données supplémentaires aux shaders de géométrie et de fragment.

#### Geometry shaders

Étape de traitement intermédiaire entre les vertex shaders et les fragment shaders dans le pipeline de rendu graphique, les **shaders de géométrie** offrent la possibilité de **générer de nouveaux éléments graphiques**, tels que des points, des lignes ou des triangles, à partir des primitives d'entrée.

Cette étape est facultative et peut être utilisée pour réaliser des effets complexes : déplacement de sommets, génération de géométrie procédurale, création d'ombres volumétriques, etc.

> Un cas concret pourrait être par exemple la **modélisation procédurale** pour générer ou modifier la géométrie d'un personnage en temps réel — créer des détails supplémentaires ou modifier la forme du personnage selon certaines conditions du jeu.

##### Fonctionnement du geometry shader

Considérons un exemple simple pour illustrer le fonctionnement des geometry shaders. Soit une ligne définie par deux points $A$ et $B$. Nous souhaitons **extruder** cette ligne pour former un tube de rayon $r$. Le geometry shader va générer un ensemble de triangles formant le tube.

Soit $\vec{AB} = \vec{B} - \vec{A}$. Nous commençons par calculer un vecteur $\vec{u}$ orthogonal à $\vec{AB}$ :

```math
\vec{u} = \begin{cases}
(\vec{AB}_y, -\vec{AB}_x, 0) & \text{si } \vec{AB}_z = 0 \\
(-\vec{AB}_z, 0, \vec{AB}_x) & \text{sinon}
\end{cases}
```

Ensuite, nous calculons un vecteur $\vec{v}$ orthogonal à $\vec{AB}$ et $\vec{u}$ en utilisant le produit vectoriel :

```math
\vec{v} = \vec{AB} \times \vec{u}
```

Nous normalisons les vecteurs $\vec{u}$ et $\vec{v}$ :

```math
\hat{u} = \frac{\vec{u}}{\|\vec{u}\|}, \quad \hat{v} = \frac{\vec{v}}{\|\vec{v}\|}
```

Soit $N$ le nombre de segments pour approximer le cercle du tube. Nous générons $N$ points $C_i$ et $D_i$ autour de chaque extrémité $A$ et $B$ :

```math
C_i = \vec{A} + r \cos \frac{2 \pi i}{N} \hat{u} + r \sin \frac{2 \pi i}{N} \hat{v}, \quad i = 0, 1, \dots, N-1
```

```math
D_i = \vec{B} + r \cos \frac{2 \pi i}{N} \hat{u} + r \sin \frac{2 \pi i}{N} \hat{v}, \quad i = 0, 1, \dots, N-1
```

Maintenant que nous avons les points autour de chaque extrémité, nous générons les triangles formant le tube. Pour chaque paire de points consécutifs $C_i$, $C_{i+1}$, $D_i$ et $D_{i+1}$, nous formons deux triangles : $(C_i, D_i, C_{i+1})$ et $(C_{i+1}, D_i, D_{i+1})$. Nous devons également traiter le cas où $i = N-1$ pour fermer le tube en connectant les points $C_0$, $C_{N-1}$, $D_0$ et $D_{N-1}$.

##### Démonstration

Pour démontrer que l'extrusion décrite précédemment forme un tube autour de la ligne $AB$, nous devons montrer que chaque point $C_i$ et $D_i$ se trouve à une distance $r$ de la ligne et que les triangles générés décrivent un tube continu.

###### Étape 1 — La distance entre chaque point $C_i$ et la ligne $AB$

Soit $M_i$ le point de la ligne $AB$ le plus proche de $C_i$. Le vecteur $\vec{M_i C_i}$ est orthogonal à $\vec{AB}$, donc leur produit scalaire est nul :

```math
\vec{AB} \cdot \vec{M_i C_i} = 0
```

En utilisant la définition des points $C_i$ :

```math
\vec{AB} \cdot \left(\vec{A} + r \cos \frac{2 \pi i}{N} \hat{u} + r \sin \frac{2 \pi i}{N} \hat{v} - \vec{A}\right) = 0
```

```math
\vec{AB} \cdot \left(r \cos \frac{2 \pi i}{N} \hat{u} + r \sin \frac{2 \pi i}{N} \hat{v}\right) = 0
```

Comme $\hat{u}$ et $\hat{v}$ sont orthogonaux à $\vec{AB}$, cette équation est vérifiée. La distance entre $C_i$ et $AB$ est donc $r$.

###### Étape 2 — La continuité du tube

Nous avons généré les triangles en connectant chaque paire de points consécutifs $C_i$, $C_{i+1}$, $D_i$ et $D_{i+1}$. Comme les points sont générés en suivant un cercle autour de chaque extrémité, cela garantit que les triangles forment un tube continu autour de la ligne $AB$. Le cas où $i = N-1$ permet de fermer le tube en connectant les points initiaux et finaux.

En conclusion, l'extrusion décrite forme un tube de rayon $r$ autour de la ligne $AB$, et les triangles générés décrivent un tube continu. ∎

#### Fragment shaders

Les **fragment shaders** permettent de déterminer la **couleur finale** de chaque pixel à afficher à l'écran, en prenant en compte les propriétés des matériaux, l'éclairage, les textures et d'autres facteurs.

> Les *fragments* sont créés par le processus de [rasterization](https://github.com/tanguychenier/Terminal_3DEngine), qui consiste à convertir la géométrie en pixels — chaque pixel de l'image est découpé en « fragments » qui sont ensuite traités par le fragment shader pour déterminer la couleur finale de ce pixel.
>
> Cet algorithme est donc chargé de calculer la couleur de chaque fragment en fonction des propriétés des matériaux, de l'éclairage, des textures et d'autres facteurs, avant que ces fragments ne soient finalement combinés pour créer l'image finale.

##### 1. Interpolation des attributs de sommet

Lorsque les sommets sont transformés par le vertex shader, ils sont accompagnés d'attributs tels que les coordonnées de texture, les normales et les couleurs. Ces attributs sont ensuite **interpolés** pour chaque fragment à l'intérieur du triangle.

Soit $A$, $B$ et $C$ les sommets du triangle avec leurs attributs respectifs $A_a$, $B_a$ et $C_a$. Pour un fragment $F$ à l'intérieur du triangle, les attributs interpolés $F_a$ sont déterminés en utilisant les **coordonnées barycentriques** $\alpha$, $\beta$ et $\gamma$ :

```math
F_a = \alpha A_a + \beta B_a + \gamma C_a
```

avec $\alpha + \beta + \gamma = 1$ et $0 \leq \alpha, \beta, \gamma \leq 1$.

##### 2. Calcul de l'éclairage

Le fragment shader doit également prendre en compte l'éclairage de la scène pour déterminer la couleur finale du fragment. Soit $L$ la direction de la source de lumière, $N$ la normale au fragment et $V$ la direction de la caméra. La couleur finale $C_f$ est déterminée en utilisant l'**équation de Phong**, qui est une combinaison de la composante ambiante, diffuse et spéculaire :

```math
C_f = k_a I_a + k_d (N \cdot L) I_d + k_s (R \cdot V)^n I_s
```

où :

- $k_a$, $k_d$, $k_s$ sont les coefficients d'éclairage ambiant, diffus et spéculaire ;
- $I_a$, $I_d$, $I_s$ sont les intensités de lumière ambiante, diffuse et spéculaire ;
- $R$ est la direction de réflexion de la lumière ;
- $n$ est l'**exposant de brillance** (*shininess*).

##### 3. Application des textures

Les fragment shaders peuvent également utiliser des textures pour déterminer la couleur finale du fragment. Soit $T(u, v)$ la couleur de la texture aux coordonnées de texture $(u, v)$. La couleur finale $C_t$ du fragment est alors déterminée en modulant la couleur interpolée $F_a$ avec la couleur de la texture :

```math
C_t = F_a \odot T(u, v)
```

où $\odot$ représente le **produit terme à terme** (modulation) des composantes de couleur.

##### 4. Combinaison des couleurs

Finalement, la couleur finale du fragment est déterminée en combinant les couleurs calculées à partir de l'éclairage et des textures :

```math
C_{\text{final}} = C_f \odot C_t
```

Cette couleur finale $C_{\text{final}}$ est ensuite utilisée pour déterminer la couleur du pixel à afficher à l'écran.

##### 5. Transparence

Les fragment shaders peuvent également gérer la **transparence** des objets. Pour cela, ils utilisent une valeur **alpha** pour chaque fragment, qui détermine l'opacité de ce fragment. La couleur finale $C_f$ du fragment est alors combinée avec la couleur du fond $C_b$ en utilisant la valeur alpha $a$ pour obtenir la couleur du pixel à afficher :

```math
C_{\text{pixel}} = C_f \odot a + C_b \odot (1 - a)
```

##### 6. Effets spéciaux

Les fragment shaders peuvent être utilisés pour créer des effets spéciaux, tels que des ombres, des reflets, des flous ou des effets de distorsion. Pour cela, il est souvent nécessaire de modifier la couleur finale du fragment de manière spécifique.

Par exemple, pour créer une ombre, la couleur finale du fragment peut être multipliée par un facteur d'ombre qui réduit l'intensité de la couleur. Pour créer un effet de flou, la couleur finale peut être calculée en moyennant les couleurs des fragments environnants.

###### Flou gaussien

La couleur finale $C_f$ peut être calculée en utilisant une **somme pondérée** de la couleur des fragments environnants :

> Une somme pondérée est une somme dans laquelle chaque terme est multiplié par un poids spécifique. Les couleurs sont représentées par des valeurs numériques, qui peuvent être considérées comme des « substances » numériques que l'on pondère.

```math
C_f(x, y) = \frac{1}{2\pi\sigma^2} \sum_{i=-k}^{k} \sum_{j=-k}^{k} w(i, j) \cdot C(x+i, y+j)
```

où $\sigma$ est l'écart-type de la distribution gaussienne, $k$ est la taille du filtre et $w(i, j)$ est la pondération de chaque fragment voisin $(i, j)$.

##### 7. Optimisations

- **Culling** : technique qui consiste à éviter le rendu de fragments qui ne sont pas visibles à l'écran. Par exemple, si un objet est entièrement masqué par un autre, on évite de le rendre afin de préserver la charge de calcul du GPU et améliorer les performances globales.
- **Discarding** : similaire au culling, mais s'applique aux fragments qui ne sont pas nécessaires pour l'image finale. Si un objet est partiellement masqué par un autre, seuls les fragments visibles doivent être rendus ; les fragments masqués peuvent être supprimés (« jetés »), réduisant la charge de calcul pour le GPU.
- **Simplification de la géométrie** (*Level of Detail*, LOD) : consiste à réduire le nombre de triangles nécessaires pour représenter un objet. Si un objet est suffisamment éloigné de la caméra, il peut être représenté par un nombre réduit de triangles sans affecter de manière significative la qualité de l'image.
- **Mipmapping** : utilise des versions pré-réduites des textures pour les objets distants, ce qui améliore les performances et réduit le *aliasing*.

[ Retour en haut de page](#table-des-matières)

---

## Pour aller plus loin

Voici quelques ressources reconnues pour approfondir les sujets abordés dans ce cours :

### Livres

- *Mathematics for 3D Game Programming and Computer Graphics* — Eric Lengyel
- *Real-Time Rendering* — Tomas Akenine-Möller, Eric Haines, Naty Hoffman et al.
- *Game Engine Architecture* — Jason Gregory
- *Physically Based Rendering: From Theory to Implementation* — Matt Pharr, Wenzel Jakob, Greg Humphreys ([disponible en ligne](https://www.pbr-book.org/))
- *Game Programming Patterns* — Robert Nystrom ([disponible en ligne](https://gameprogrammingpatterns.com/))

### Sites et tutoriels

- [Khan Academy — Mathématiques](https://fr.khanacademy.org/math) — bases solides en algèbre, trigonométrie, calcul.
- [LearnOpenGL](https://learnopengl.com/) — un excellent tutoriel pour comprendre le pipeline graphique en pratique.
- [Scratchapixel](https://www.scratchapixel.com/) — articles très approfondis sur le rendu et la 3D.
- [The Book of Shaders](https://thebookofshaders.com/) — introduction interactive aux shaders.
- [Catlike Coding](https://catlikecoding.com/unity/tutorials/) — tutoriels avancés Unity (rendu, mathématiques, shaders).

### Moteurs et frameworks à explorer

- [Unity](https://unity.com/), [Unreal Engine](https://www.unrealengine.com/), [Godot](https://godotengine.org/) — moteurs grand public.
- [Bevy](https://bevyengine.org/) (Rust), [MonoGame](https://www.monogame.net/) (C\#), [raylib](https://www.raylib.com/) (C) — frameworks plus légers, idéaux pour apprendre.

[ Retour en haut de page](#table-des-matières)

---

## Contribuer

Ce dépôt est en évolution permanente. Si vous repérez une coquille, une imprécision mathématique ou si vous souhaitez ajouter un schéma ou un exemple :

1. **Fork** ce dépôt
2. Créez une branche : `git checkout -b ameliore-section-xxx`
3. Faites vos modifications
4. Ouvrez une **Pull Request** avec une description claire

Toute contribution — correction, illustration, exemple de code, traduction — est la bienvenue.

## Licence

Ce projet est distribué sous licence **MIT**. Voir le fichier [LICENSE](LICENSE) pour plus d'informations.

---

[^1]: Des modifications peuvent survenir. — [Tanguy Chénier](https://www.linkedin.com/in/tanguy-chenier/).
