[← Texture et mappage UV](05-texture-et-mappage-uv.md) · [↑ Sommaire](../README.md#table-des-matières) · [Physique des jeux →](07-physique-des-jeux.md)

# 6. Animation

L'animation en infographie consiste à créer l'illusion de **mouvement** ou de **changement** d'un objet ou d'une scène 3D au fil du temps.

Les techniques les plus courantes sont l'**animation par squelette**, l'**animation de forme** et la **cinématique inverse**.

### Animation par squelette

L'**animation par squelette** (*Rigging*), également appelée animation par armature, consiste à définir une structure osseuse (ou armature) pour un objet 3D et à manipuler cette structure pour créer des mouvements.

Chaque os de l'armature est associé à une partie de l'objet 3D et déforme cette partie lorsqu'il est déplacé ou orienté. L'animation par squelette est largement utilisée pour animer des **personnages** et des **créatures** dans les jeux vidéo et les films d'animation.

Une armature est un ensemble de **nœuds** (appelés *joints* ou *os*) reliés entre eux par des **liaisons rigides**. Les nœuds ont des positions 3D et des orientations, généralement représentées par des matrices de transformation 4×4. Pour déterminer la position et l'orientation d'un nœud, on utilise la relation suivante :

```math
T_\text{global} = T_\text{parent} \cdot T_\text{local}
```

où $`T_\text{parent}`$ est la matrice de transformation globale du nœud parent, $`T_\text{local}`$ est la matrice de transformation locale du nœud actuel, et $`T_\text{global}`$ est la matrice de transformation globale du nœud actuel.

L'animation d'une armature consiste à modifier les matrices de transformation locale des nœuds au fil du temps, créant ainsi des mouvements.

### Animation de forme

L'**animation de forme**, également appelée *morphing* ou interpolation de formes, consiste à interpoler entre différentes formes d'un objet 3D pour créer des animations. Cette technique est souvent utilisée pour animer des objets dont la géométrie change de manière complexe, comme les **visages** ou les **vêtements**.

Cette technique repose généralement sur l'**interpolation linéaire** entre les positions des sommets des différentes formes. Pour interpoler entre deux formes $`A`$ et $`B`$ à un facteur d'interpolation $`t`$, où $`0 \leq t \leq 1`$, on utilise la formule suivante :

```math
P_\text{interpolated} = (1 - t) \times P_A + t \times P_B
```

où $`P_\text{interpolated}`$ est la position interpolée du sommet, et $`P_A`$ et $`P_B`$ sont les positions du sommet dans les formes $`A`$ et $`B`$, respectivement.

### Cinématique inverse

La **cinématique inverse** (*Inverse Kinematics*, IK) est une technique d'animation utilisée pour calculer les angles des articulations d'une armature en fonction de la **position désirée** d'un effecteur (généralement la main ou le pied d'un personnage).

Cette technique est particulièrement utile pour les animations interactives — par exemple, lorsqu'un personnage saisit un objet ou marche sur un terrain irrégulier.

La cinématique inverse nécessite la résolution d'un **système d'équations non linéaires** décrivant les positions et les orientations des nœuds de l'armature. Trois grandes familles d'algorithmes permettent de l'approcher :

#### Méthodes basées sur la Jacobienne

Soit $`\boldsymbol{\theta} = (\theta_1, \dots, \theta_n)`$ le vecteur des angles articulaires et $`\mathbf{e}(\boldsymbol{\theta})`$ la position de l'effecteur (fonction non-linéaire). On cherche $`\boldsymbol{\theta}^\star`$ tel que $`\mathbf{e}(\boldsymbol{\theta}^\star)`$ atteigne la cible $`\mathbf{e}_\text{cible}`$. La **Jacobienne** $`J = \partial \mathbf{e} / \partial \boldsymbol{\theta}`$ relie une petite variation des angles à une petite variation de l'effecteur : $`\Delta \mathbf{e} \approx J\,\Delta \boldsymbol{\theta}`$.

> **Notation.** Le symbole $`\partial`$ (lu "d rond") désigne une **dérivée partielle** : $`\partial f / \partial x`$ signifie "comment $`f`$ varie quand on bouge **uniquement** $`x`$, en gardant les autres variables fixes". C'est la généralisation aux fonctions à plusieurs variables de la dérivée classique $`\mathrm{d}f / \mathrm{d}x`$. La **Jacobienne** d'une fonction vectorielle $`\mathbf{e}(\boldsymbol{\theta})`$ est la matrice qui regroupe **toutes** les dérivées partielles : $`J_{ij} = \partial e_i / \partial \theta_j`$. Pour une chaîne IK à 3 articulations planaires (un degré de liberté chacune) qui produit une position 3D en sortie, $`J`$ est une matrice $`3 \times 3`$ ; en général, $`J`$ est de taille $`m \times n`$ où $`m`$ est la dimension de l'espace de l'effecteur et $`n`$ le nombre total de degrés de liberté.

La mise à jour itérative des angles s'écrit, à chaque pas, en fonction de l'erreur courante et d'une forme de l'inverse de la Jacobienne :

```math
\Delta \boldsymbol{\theta} = J^{+}\,(\mathbf{e}_\text{cible} - \mathbf{e}(\boldsymbol{\theta}))
```

où $`J^{+}`$ est la **pseudo-inverse** de Moore-Penrose. Selon la manière dont on approche $`J^{+}`$, on obtient trois variantes :

- **Jacobienne transposée** : on remplace $`J^{+}`$ par $`J^{T}`$.
  - *Avantages* : très peu coûteux (pas d'inversion de matrice), implémentation triviale.
  - *Inconvénients* : convergence lente, le pas de descente $`\alpha`$ est difficile à régler, comportement médiocre sur les chaînes longues.

- **Pseudo-inverse** ($`J^{+} = J^{T}(JJ^{T})^{-1}`$) : solution au sens des moindres carrés.
  - *Avantages* : convergence rapide, solution minimisant la norme $`\|\Delta\boldsymbol{\theta}\|`$.
  - *Inconvénients* : instable près des **singularités** (coude tendu, par exemple) où $`JJ^{T}`$ devient singulière ou mal conditionnée.

- **Damped Least Squares** (DLS — application du principe de régularisation de Levenberg-Marquardt à l'IK) : $`\Delta \boldsymbol{\theta} = J^{T}(JJ^{T} + \lambda^2 I)^{-1}\,(\mathbf{e}_\text{cible} - \mathbf{e})`$.
  - *Avantages* : le terme d'amortissement $`\lambda`$ régularise le système et supprime les instabilités aux singularités ; c'est l'ossature classique des solveurs IK qu'on retrouve aussi bien dans Maya que dans les solveurs IK des moteurs de jeu et des outils d'animation.
  - *Inconvénients* : introduit une erreur résiduelle au voisinage des singularités (l'effecteur ne peut plus atteindre exactement la cible) ; le réglage de $`\lambda`$ est souvent empirique.

#### CCD — Cyclic Coordinate Descent

Itérativement, on parcourt la chaîne de l'effecteur vers la racine et, pour chaque articulation, on calcule la rotation pure qui aligne le segment « articulation vers effecteur » avec « articulation vers cible ». L'implémentation tient en quelques dizaines de lignes, le coût en flops est négligeable et il n'y a pas de singularité à gérer. En revanche, les poses obtenues sont parfois peu naturelles (l'épaule fait l'essentiel du travail avant le coude). Très répandu dans les jeux jusqu'au milieu des années 2010, et encore utilisé comme solution de repli (*fallback*).

#### FABRIK — Forward And Backward Reaching Inverse Kinematics

Introduit par Aristidou & Lasenby (2011), FABRIK traite la chaîne comme un ensemble de longueurs **rigides** plutôt que de tordre des angles :

1. **Forward pass** : on déplace l'effecteur sur la cible, puis on fait remonter chaque articulation vers la racine en préservant les longueurs des os.
2. **Backward pass** : on remet la racine à sa position initiale et on redescend la chaîne en préservant les longueurs.

On itère jusqu'à convergence (typiquement 5-10 passes pour une chaîne de 7 os). Avantages : pas de Jacobienne à manipuler, pas de problème de singularité, un comportement visuellement très naturel, et la possibilité d'imposer des **contraintes d'angle** par simple projection à chaque passe. Largement adopté côté moteur de jeu — il est par exemple disponible directement dans l'Animation Blueprint d'Unreal Engine sous le nœud `FABRIK`.

[ Retour en haut de page](#table-des-matières)

---

---

[← Texture et mappage UV](05-texture-et-mappage-uv.md) · [↑ Sommaire](../README.md#table-des-matières) · [Physique des jeux →](07-physique-des-jeux.md)
