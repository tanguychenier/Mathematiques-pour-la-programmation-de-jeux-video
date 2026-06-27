[← Réseau et multijoueur](09-reseau-et-multijoueur.md) · [↑ Sommaire](../README.md#table-des-matières) · [Pipeline de rendu →](11-pipeline-de-rendu.md)

# 10. Techniques avancées

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

> **Pourquoi c'est inutilisable seul.** Comme chaque pixel est indépendant, deux pixels voisins peuvent avoir des valeurs totalement différentes. Si on l'utilisait pour un terrain, le sol monterait à 100 m puis redescendrait à 0 d'un mètre à l'autre — injouable. Le bruit blanc sert de **source de valeurs pseudo-aléatoires** à partir de laquelle des constructions plus sophistiquées créent une continuité spatiale : le **bruit de valeur** en interpole les valeurs entre sommets de grille, le **bruit de Perlin** interpole des gradients, le **bruit fractal** en superpose plusieurs couches à différentes fréquences.

#### Bruit de Perlin

> **Définition.** Le **bruit de Perlin** est une fonction continue et lisse, inventée par **Ken Perlin** pour générer les textures du film *Tron* (1982) et présentée publiquement au SIGGRAPH 1985. Elle ressemble à un terrain vu de dessus : des vallées et des collines qui se succèdent sans cassure. Perlin a reçu un Oscar technique en 1997 pour son invention. C'est aujourd'hui la base de la génération de terrain procédurale dans la majorité des jeux à monde ouvert (*Minecraft*, *No Man's Sky*, *Terraria*…), souvent dans sa variante Simplex décrite ci-dessous.

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

- **Heightmap de terrain** : *Minecraft*, *No Man's Sky*, *Terraria* utilisent du bruit fractal pour générer la carte d'élévation du sol.
- **Textures procédurales** : marbre, bois, nuages, eau — le motif "fibreux" d'un tronc vient d'une FBM filtrée.
- **Distribution d'objets** : seuiller un bruit ($N(x, y) > 0{,}7$ → "pose un arbre ici") donne des forêts naturelles, sans agglomérats.
- **Donjons et niveaux** : on utilise plutôt des algorithmes spécialisés.

**Lexique des algos de génération de niveau :**

- **BSP** (*Binary Space Partitioning*) : on découpe une zone en deux sous-zones, récursivement, jusqu'à obtenir des pièces. Utilisé par *Rogue* (1980) et la quasi-totalité des roguelikes.
- **Wave Function Collapse** (Maxim Gumin, 2016) : algorithme inspiré de la physique quantique où chaque case "choisit" un motif compatible avec ses voisines. Donne des résultats étonnamment cohérents à partir d'un simple échantillon.
- **Marche aléatoire** (*random walk*) : un agent virtuel se déplace au hasard et creuse les cases qu'il visite. Donne des cavernes organiques.

Exemple d'implémentation d'une FBM de terrain en C#/Unity (utilise `Mathf.PerlinNoise`, l'implémentation du bruit de Perlin fournie par le moteur Unity) :

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

> **Notations.** $\nabla$ (lu "nabla") est l'**opérateur gradient** : appliqué à un champ scalaire $f(x, y, z)$, il renvoie le **vecteur des dérivées partielles** $\nabla f = (\partial f/\partial x,\ \partial f/\partial y,\ \partial f/\partial z)$, qui pointe dans la direction de plus forte croissance. $\nabla^2 = \nabla \cdot \nabla$ est le **laplacien** (somme des dérivées secondes pures : $\partial^2 f/\partial x^2 + \partial^2 f/\partial y^2 + \partial^2 f/\partial z^2$) — il mesure à quel point un point "diffère de la moyenne de ses voisins" et apparaît dès qu'on modélise diffusion, viscosité ou propagation d'onde. $\partial \mathbf{v}/\partial t$ est la dérivée partielle de la vitesse **par rapport au temps** : c'est l'accélération locale du fluide en un point fixe.
>
> **Les trois méthodes numériques classiques.**
>
> - **Différences finies** : on quadrille l'espace en grille régulière et on remplace chaque dérivée par une différence entre cases voisines ($\partial f / \partial x \approx (f_{i+1} - f_{i-1}) / (2h)$). Simple à coder, marche bien pour des fluides Eulériens (la grille est fixe, le fluide la traverse).
> - **Éléments finis** (FEM) : on découpe l'espace en triangles/tétraèdres irréguliers et on cherche la solution sous forme d'une combinaison de fonctions de base locales. Plus précis sur des géométries complexes, mais lourd — utilisé en simulation industrielle, en cloth/soft-body précis (Houdini, Marvelous Designer).
> - **SPH** (*Smoothed Particle Hydrodynamics*) : approche **Lagrangienne** où le fluide est représenté par un nuage de particules qui transportent vitesse et pression. Chaque particule échantillonne ses voisines avec un noyau lissant. Standard pour l'eau et le sang dans les jeux (Position Based Fluids de NVIDIA, Liquid Simulation de Houdini).

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

> **Notations.** Le symbole $\int$ est une **intégrale** : intuitivement, "une somme continue" sur un domaine. Là où $\sum_i$ additionne un nombre fini ou dénombrable de termes, $\int_a^b f(x)\,\mathrm{d}x$ additionne $f(x)$ pour **tous** les $x \in [a, b]$, en pondérant par un élément de longueur $\mathrm{d}x$ infinitésimal — géométriquement, c'est l'aire sous la courbe. $\int_\Omega \dots\,\mathrm{d}\boldsymbol{\omega}$ est une intégrale sur un domaine $\Omega$ (ici l'hémisphère, ensemble de toutes les directions au-dessus de la surface) : on additionne la contribution lumineuse de **toutes** les directions possibles. La lettre $\boldsymbol{\omega}$ (oméga gras) désigne ici un **vecteur direction unitaire** ; $\Omega$ (oméga majuscule) désigne le **domaine** (l'hémisphère). Les angles $\theta$ (latitude/co-latitude, depuis le pôle) et $\phi$ (longitude/azimut, autour du pôle) sont les **coordonnées sphériques** standards utilisées pour paramétrer les directions sur la sphère.

où :

- $L_e$ est l'émission propre de la surface (matériau lumineux, écran).
- $L_i$ est la lumière incidente venue de la direction $\boldsymbol{\omega}_i$.
- $f_r$ est la **BRDF** (*Bidirectional Reflectance Distribution Function* — fonction de distribution de la réflectance bidirectionnelle) : combien de la lumière entrant par $\boldsymbol{\omega}_i$ ressort vers $\boldsymbol{\omega}_o$. C'est *la* signature optique d'un matériau : un miroir, un mur de plâtre et un velours rouge ont chacun leur BRDF caractéristique. Variantes : **BTDF** (*Transmittance*, lumière transmise à travers la surface, pour le verre/l'eau) et **BSDF** (*Scattering*, somme BRDF + BTDF — la fonction complète).
- $\boldsymbol{\omega}_i \cdot \mathbf{n}$ est le **terme de Lambert** : un faisceau qui frappe la surface à 45° apporte moins d'énergie au m² qu'un faisceau perpendiculaire.
- $\Omega$ est l'hémisphère au-dessus de la surface.

L'intégrale est insolvable analytiquement → on l'approxime. Les jeux temps réel utilisent une **BRDF Cook-Torrance microfacets** dont la forme canonique met en évidence le rôle des deux cosinus au dénominateur :

```math
f_r(\boldsymbol{\omega}_i, \boldsymbol{\omega}_o) = \underbrace{\frac{c_\text{diff}}{\pi}}_\text{Lambertien} + \underbrace{\frac{D(\mathbf{h}) \cdot F(\boldsymbol{\omega}_o, \mathbf{h}) \cdot G(\boldsymbol{\omega}_i, \boldsymbol{\omega}_o)}{4\,(\mathbf{n}\cdot\boldsymbol{\omega}_i)\,(\mathbf{n}\cdot\boldsymbol{\omega}_o)}}_\text{Spéculaire microfacets}
```

où $\mathbf{h} = \dfrac{\boldsymbol{\omega}_i + \boldsymbol{\omega}_o}{\|\boldsymbol{\omega}_i + \boldsymbol{\omega}_o\|}$ est le **half-vector** (vecteur médian). Les deux cosinus $\mathbf{n}\cdot\boldsymbol{\omega}_i$ et $\mathbf{n}\cdot\boldsymbol{\omega}_o$ doivent **toujours** être pris en valeur absolue (ou clampés à $[\epsilon, 1]$) — un cosinus négatif signifie qu'on regarde le dos de la surface, et un cosinus proche de $0$ produit une singularité que le terme $G$ doit annuler. Les formules explicites (le "secret de cuisine" du PBR moderne) :

```math
D_\text{GGX}(\mathbf{h}) = \frac{\alpha^2}{\pi\,\big[(\mathbf{n}\cdot\mathbf{h})^2(\alpha^2 - 1) + 1\big]^2}, \qquad \alpha = \text{roughness}^2
```

```math
F_\text{Schlick}(\boldsymbol{\omega}_o, \mathbf{h}) = F_0 + (1 - F_0)\,(1 - \mathbf{h}\cdot\boldsymbol{\omega}_o)^5
```

```math
G_\text{Smith}(\boldsymbol{\omega}_i, \boldsymbol{\omega}_o) = G_1(\boldsymbol{\omega}_i)\,G_1(\boldsymbol{\omega}_o), \qquad G_1(\boldsymbol{\omega}) = \frac{\mathbf{n}\cdot\boldsymbol{\omega}}{(\mathbf{n}\cdot\boldsymbol{\omega})(1 - k) + k}, \quad k = \frac{(\text{roughness} + 1)^2}{8}
```

> **Note.** La formule de $k$ ci-dessus (Karis, Unreal 4) est l'approximation pour les **lumières directes**. Pour l'éclairage à base d'image (*IBL*, *Image-Based Lighting*), on utilise $k = \text{roughness}^2 / 2$ afin d'éviter un biais sur les surfaces lisses.

avec :

- **D — distribution des normales** (GGX/Trowbridge-Reitz, le standard depuis ~2014) : densité statistique des microfacettes alignées avec $\mathbf{h}$. La queue lourde de GGX (cf. ci-dessous) reproduit fidèlement les *highlights* étendues d'un métal brossé.
- **F — Fresnel** (Schlick) : approximation rationnelle des équations de Fresnel exactes. $F_0$ est la **réflectance à incidence normale**, dépendant du matériau ($\approx 0{,}04$ pour la plupart des diélectriques, $= $ couleur d'albedo pour les métaux).
- **G — masquage / ombrage** (Smith, formulation Schlick-GGX de Karis pour Unreal 4) : sur une surface très rugueuse, certaines microfacettes sont cachées par leurs voisines. La factorisation de Smith $G = G_1(\omega_i)\,G_1(\omega_o)$ découple masquage et ombrage. Le paramètre $k$ ci-dessus est l'approximation directe (analytique) de la version *correlated* utilisée en temps réel.

##### GGX vs Beckmann vs Trowbridge-Reitz : trois NDF, une histoire

Le terme $D$ a connu trois générations chez les chercheurs et chez les artistes :

- **Beckmann** (1963) : la NDF d'origine, dérivée d'un modèle physique gaussien des hauteurs de microfacettes. Décroissance exponentielle, donc *highlight* trop "serré" et coupure trop nette pour un métal poli. Elle reste utilisée en simulation optique offline.
- **Trowbridge-Reitz** (1975) et sa réincarnation **GGX** (Walter et al., 2007) : queue plus lourde, *highlights* plus naturelles. Disney l'a adoptée dans son fameux *principled BRDF* (SIGGRAPH 2012), suivi par Unreal 4 (Karis, 2013) — depuis, c'est le standard de fait dans tous les engines.
- **GGX-anisotropique** : variante à deux paramètres de rugosité ($\alpha_x, \alpha_y$) qui modélise le cuir brossé, le velours, les disques métalliques rayés. Coût : un produit scalaire de plus, pour un gain visuel énorme sur un matériau "horloger".

L'ironie historique : Trowbridge-Reitz et GGX sont **mathématiquement identiques**. Walter et ses co-auteurs ont en pratique redécouvert la NDF de 1975, l'ont nommée d'après les initiales internes « Generalized-Trowbridge-Reitz » (selon une rumeur persistante chez Pixar) puis l'ont popularisée. D'où la cohabitation des deux noms dans la littérature graphique récente.

 **Paramètres PBR exposés à l'artiste.** Ce qu'on lui demande de peindre dans des textures :

- **Albedo** (RGB) : la couleur de base (sans aucune lumière).
- **Metallic** ($\in [0, 1]$) : 0 = diélectrique (bois, peau, peinture), 1 = métal pur (or, fer poli).
- **Roughness** ($\in [0, 1]$) : 0 = miroir, 1 = surface mate parfaitement diffuse.
- **Normal map** (RGB encodant un vecteur 3D) : perturbe la normale géométrique pour simuler des micro-bosses sans ajouter de polygones.

Avec ces quatre paramètres, on couvre la majorité des matériaux du monde physique : peau, métal, plastique, tissu, vitre, eau, etc. C'est aussi ce qui rend les *assets* modernes interopérables : une texture peinte dans Substance Painter peut être chargée à peu près telle quelle dans Unity, Unreal ou Blender.

#### Occlusion ambiante (*Ambient Occlusion*, AO)

Approximation peu coûteuse de l'éclairage indirect : on assombrit les zones où la lumière ambiante peinerait à pénétrer (coins, replis). L'**AO** estime un facteur $A(\mathbf{x}) \in [0, 1]$ multiplicatif :

```math
A(\mathbf{x}) = \frac{1}{\pi} \int_{\Omega} V(\mathbf{x}, \boldsymbol{\omega})\,(\boldsymbol{\omega} \cdot \mathbf{n})\,\mathrm{d}\boldsymbol{\omega}
```

où $V \in \{0, 1\}$ est la **visibilité** dans la direction $\boldsymbol{\omega}$ (1 si rien ne bloque, 0 sinon). En temps réel on en fait une approximation en screen-space :

- **SSAO** (*Screen-Space Ambient Occlusion*, Crytek 2007) : sample le depth buffer autour de chaque pixel.
- **HBAO** (*Horizon-Based*, NVIDIA 2008) : raffinement angulaire, plus précis.
- **GTAO** (*Ground-Truth*, 2016) : référence actuelle, presque indistinguable d'un rendu offline.

#### Harmoniques sphériques — l'éclairage ambiant compressé

> **Harmoniques sphériques (SH)** : famille de fonctions de base définies sur la sphère unité, analogues à la série de Fourier mais en 2 angles ($\theta, \varphi$) au lieu d'un seul. Elles permettent de **projeter** une fonction quelconque sur la sphère — typiquement la lumière incidente ambiante — sur un nombre fini de coefficients, puis de reconstruire une approximation lisse à partir de ces coefficients.

Toute fonction $f : S^2 \to \mathbb{R}$ (où $S^2$ désigne la **sphère unité** — l'ensemble des directions dans l'espace 3D, soit les vecteurs de norme 1 — et $\mathbb{R}$ l'ensemble des réels) se décompose en :

```math
f(\theta, \varphi) = \sum_{\ell = 0}^{\infty} \sum_{m = -\ell}^{\ell} c_\ell^m\,Y_\ell^m(\theta, \varphi)
```

où les $Y_\ell^m$ sont les **harmoniques sphériques** (orthonormées sur la sphère). En infographie temps réel, on tronque à $\ell \le 2$ — il ne reste alors que **9 coefficients par canal RGB** (donc 27 floats) qui suffisent à reproduire l'éclairage ambiant diffus avec une erreur $\le 1\%$ pour des surfaces lambertiennes (Ramamoorthi & Hanrahan, 2001). Concrètement, dans une *probe* de Unity ou Unreal, ces 27 floats encodent **toute** la lumière ambiante d'une cubemap haute résolution.

L'évaluation de l'**irradiance** $E(\mathbf{n})$ (lumière reçue par une surface de normale $\mathbf{n}$) se réduit alors à un **produit scalaire 9D** dans le shader — une poignée de cycles, à comparer à l'échantillonnage d'une cubemap pour chaque fragment :

```math
E(\mathbf{n}) \approx \sum_{\ell = 0}^{2} \sum_{m = -\ell}^{\ell} c_\ell^m\,Y_\ell^m(\mathbf{n})
```

C'est cette représentation qui se cache derrière les **Light Probes** d'Unity, les **Irradiance Volumes** d'Unreal et les **Lightmaps SH** de Frostbite — autrement dit, la quasi-totalité des dispositifs d'éclairage ambiant compressé qu'on rencontre dans les moteurs modernes.

#### Échantillonnage par importance — comment Monte-Carlo ne diverge pas

L'estimateur Monte-Carlo de l'équation de rendu donné plus haut converge à $1/\sqrt{N}$, ce qui exige des milliers d'échantillons par pixel pour un résultat propre. La technique-clé pour réduire massivement la **variance** est l'**importance sampling** : tirer les directions de rebond $\boldsymbol{\omega}_i$ avec une distribution $p(\boldsymbol{\omega}_i)$ **proportionnelle à l'intégrande** plutôt qu'uniformément sur la sphère.

```math
L_o \approx \frac{1}{N} \sum_{k=1}^{N} \frac{f_r(\boldsymbol{\omega}_i^{(k)}, \boldsymbol{\omega}_o)\,L_i(\boldsymbol{\omega}_i^{(k)})\,(\boldsymbol{\omega}_i^{(k)} \cdot \mathbf{n})}{p(\boldsymbol{\omega}_i^{(k)})}
```

Trois choix usuels de PDF, chacun adapté à un terme :

- **Cosine-weighted hemisphere** : $p(\boldsymbol{\omega}) = \cos\theta / \pi$. Optimale pour un terme lambertien $f_r = \rho/\pi$ : la pondération $\cos\theta$ disparaît exactement, donc tous les échantillons contribuent uniformément. Tirage par disque concentrique (Shirley, 1997) en deux uniformes $(u_1, u_2) \in [0,1]^2$.
- **GGX importance sampling** : tirer le half-vector $\mathbf{h}$ selon la NDF $D(\mathbf{h})$ puis réfléchir $\boldsymbol{\omega}_o$ par $\mathbf{h}$. Indispensable pour les *highlights* spéculaires fines : sur un matériau quasi miroir (rugosité ≈ 0,05), un échantillonnage uniforme de la sphère a très peu de chances de tomber dans le pic spéculaire, alors qu'un *importance sampling* sur $D$ y concentre les tirages naturellement et converge avec un nombre d'échantillons radicalement plus faible.
- **Multiple Importance Sampling (MIS)** : combine deux PDFs (souvent BRDF + lumière) en pondérant par la *balance heuristic* de Veach (1995). C'est ce qui permet à un path tracer moderne de gérer correctement à la fois les surfaces très spéculaires *et* les sources de lumière étendues, sans firefly.

> **Pourquoi DLSS et FSR convergent aussi vite.** Les *denoisers* neuronaux des path tracers temps réel (*Cyberpunk* RTX, *Quake II* RTX) opèrent sur des images rendues à 1 ou 2 échantillons par pixel, mais ces échantillons sont déjà *importance-sampled*. Sans cette pré-concentration des tirages dans les directions énergétiquement utiles, 1 ou 2 samples seraient trop bruités même pour un réseau de neurones bien entraîné : c'est la combinaison des deux qui rend le path tracing temps réel réellement praticable aujourd'hui.

#### Anti-aliasing — la guerre contre l'escalier

L'**aliasing** (en français : *crénelage*) apparaît dès qu'on échantillonne un signal continu (un triangle, une texture) à une fréquence inférieure à sa **fréquence de Nyquist** : $f_\text{sample} \ge 2\,f_\text{signal}$. Un triangle avec une arête fine ou une texture haute fréquence produit alors des marches d'escalier, du *moiré*, du *crawling* en mouvement. Quatre familles de techniques :

- **SSAA** (*Super-Sampling Anti-Aliasing*) — la force brute. On rend la scène à $k\times$ la résolution puis on *downsample* par moyenne. Coût mémoire et calcul $\times k^2$. Référence absolue en qualité, jamais utilisé en jeu temps réel sauf en mode "screenshot".
- **MSAA** (*Multi-Sample AA*) — version optimisée. On évalue le **fragment shader une seule fois par pixel** mais on stocke $k$ échantillons de **profondeur** et de **couverture** (typiquement 2× ou 4×). Très efficace pour les arêtes de polygones, mais sans effet sur les textures ni sur les *highlights* spéculaires fins ou les contours d'*alpha-test*. S'intègre difficilement au *deferred shading* moderne (non impossible, mais complexe et coûteux) : il faudrait stocker $k$ échantillons de chaque attribut géométrique dans le G-buffer, ce qui devient prohibitif en mémoire.

```math
C_\text{pixel} = \frac{1}{k} \sum_{j=1}^{k} \mathbb{1}[\text{sample}_j \text{ couvert}] \cdot C_\text{shader}
```

- **FXAA** (*Fast Approximate AA*, Lottes/NVIDIA, publié vers 2011) — *post-process* sur l'image finale. Détecte les contours par opérateur de **luma gradient** puis flou directionnel le long de l'arête. Très bon marché ($\sim 0{,}5$ ms en 1080p), résultat un peu flou, **gère tous les types d'aliasing** (textures incluses).
- **TAA** (*Temporal AA*) — le standard actuel. Combine l'image courante avec les images précédentes **reprojetées** via les *motion vectors*, en accumulant un sous-échantillon différent à chaque frame. Mathématiquement, c'est exactement la formule de reprojection temporelle vue dans la section DLSS :

```math
C_t(\mathbf{p}) = \alpha\,C_t^\text{rendu}(\mathbf{p}) + (1 - \alpha)\,C_{t-1}(\mathbf{p} + \mathbf{v}_\text{motion})
```

Avec $\alpha \approx 0{,}1$, après une dizaine de frames un pixel statique a accumulé une dizaine de sous-échantillons différents : on obtient à peu près la qualité d'un SSAA 10× pour le coût d'un seul rendu par frame. Le défaut classique du TAA est le *ghosting* : sur un objet qui se découvre, une couleur fantôme issue des frames précédentes reste collée derrière lui. Le *neighborhood clamping* (Karis, 2014) atténue cet effet en bornant la couleur historique aux valeurs minimales et maximales rencontrées dans le voisinage spatial du pixel à la frame courante : si la couleur historique sort de cette enveloppe (donc si la frame courante a vraiment changé), elle est clampée et le fantôme disparaît. **DLAA** (*Deep-Learning Anti-Aliasing*, NVIDIA — TAA opérant à **résolution native** dont la combinaison spatio-temporelle est gérée par un réseau de neurones, sans upscaling) et **TSR** (*Temporal Super Resolution* d'Unreal Engine 5 — TAA qui fait également de l'*upscaling*) sont des évolutions modernes de TAA.

#### Tessellation et displacement mapping

> **Tessellation** : subdiviser dynamiquement un maillage en plus de triangles à l'approche du spectateur. **Displacement** : déplacer les nouveaux sommets selon une *heightmap* (texture en niveaux de gris) pour créer du vrai relief géométrique (et pas juste une normal map qui ment au rasterizer).

Mathématiquement, la subdivision se fait dans l'**espace barycentrique** d'un patch (triangle ou quad). Pour chaque triangle d'entrée de sommets $V_0, V_1, V_2$ et un facteur de tessellation $T$, le *Domain Shader* est invoqué avec un triplet barycentrique $(u, v, w)$ avec $u + v + w = 1$, et reconstruit le sommet :

```math
P(u, v, w) = u\,V_0 + v\,V_1 + w\,V_2 + h(u, v, w)\,\mathbf{n}(u, v, w)
```

où $h$ est lue dans la *heightmap* et $\mathbf{n}$ est la normale interpolée. C'est l'extension naturelle des coordonnées barycentriques (déjà utilisées par le fragment shader pour interpoler les attributs) à un échantillonnage **dense** du patch.

Combinés, tessellation et displacement créent un terrain ou une muraille qui semble découpée au burin, sans payer le coût mémoire d'un mesh aussi détaillé. Le pipeline GPU (DX11+, OpenGL 4.0+) fournit deux étages dédiés : **Hull Shader** + **Domain Shader**. Les *Mesh Shaders* (DX12 Ultimate, 2020) étendent encore le concept en remplaçant tout le front-end par un programme arbitraire — plus de vertex/hull/domain, juste un shader qui crache des *meshlets*.

#### Ray tracing — le tracé de rayons

> **Idée.** Au lieu de projeter des triangles puis d'éclairer chaque pixel a posteriori, on tire un rayon depuis la caméra à travers chaque pixel et on regarde ce qu'il rencontre dans la scène. C'est l'inverse mathématique de la propagation réelle de la lumière (les photons physiques partent des sources, pas de l'œil), mais comme la BRDF est **symétrique** par rapport aux deux directions, les deux formulations donnent le même résultat.

Pour un rayon $R(t) = \mathbf{O} + t\,\mathbf{D}$ et une sphère $\|\mathbf{P} - \mathbf{C}\|^2 = r^2$, l'intersection se résout par une simple équation du second degré :

```math
\|t\,\mathbf{D} + (\mathbf{O} - \mathbf{C})\|^2 = r^2
\;\Rightarrow\;
t^2(\mathbf{D}\cdot\mathbf{D}) + 2t\,\mathbf{D}\cdot(\mathbf{O}-\mathbf{C}) + \|\mathbf{O}-\mathbf{C}\|^2 - r^2 = 0
```

Pour des triangles, on utilise l'algorithme **Möller-Trumbore** (1997) qui donne directement les coordonnées barycentriques et la profondeur sans construire explicitement le plan du triangle. Soit un rayon $R(t) = \mathbf{O} + t\,\mathbf{D}$ et un triangle $(V_0, V_1, V_2)$. On pose $\mathbf{e}_1 = V_1 - V_0$, $\mathbf{e}_2 = V_2 - V_0$ et on résout :

```math
\mathbf{O} + t\,\mathbf{D} = V_0 + u\,\mathbf{e}_1 + v\,\mathbf{e}_2 \quad\Leftrightarrow\quad
\begin{pmatrix} -\mathbf{D} & \mathbf{e}_1 & \mathbf{e}_2 \end{pmatrix}\begin{pmatrix} t \\ u \\ v \end{pmatrix} = \mathbf{O} - V_0
```

La règle de Cramer combinée à l'identité du **produit mixte** $(\mathbf{a} \times \mathbf{b}) \cdot \mathbf{c} = \det(\mathbf{a}, \mathbf{b}, \mathbf{c})$ — où $\det$ est le **déterminant** (scalaire associé à une matrice carrée qui mesure le facteur de contraction/dilatation des volumes par la transformation, et qui s'annule ssi la matrice n'est pas inversible) — donne, en posant $\mathbf{p} = \mathbf{D} \times \mathbf{e}_2$, $\mathbf{T} = \mathbf{O} - V_0$, $\mathbf{q} = \mathbf{T} \times \mathbf{e}_1$ :

```math
t = \frac{\mathbf{q} \cdot \mathbf{e}_2}{\mathbf{p} \cdot \mathbf{e}_1}, \qquad u = \frac{\mathbf{p} \cdot \mathbf{T}}{\mathbf{p} \cdot \mathbf{e}_1}, \qquad v = \frac{\mathbf{q} \cdot \mathbf{D}}{\mathbf{p} \cdot \mathbf{e}_1}
```

Le triangle est touché si $\mathbf{p} \cdot \mathbf{e}_1 \ne 0$ (rayon non parallèle), $u \ge 0$, $v \ge 0$, $u + v \le 1$ et $t > t_\text{min}$. Le coût se résume à 5 produits scalaires, 2 produits vectoriels et 1 division, soit une trentaine de flops par test. Les GPUs récents (côté NVIDIA à partir des séries RTX, côté AMD à partir de l'architecture RDNA 2) embarquent une unité matérielle dédiée (les **RT cores**, *Ray Tracing cores* — circuits spécialisés qui calculent l'intersection rayon/triangle directement en silicium plutôt que via les unités de calcul génériques) et l'enchaînent avec une descente dans la **BVH** (*Bounding Volume Hierarchy* — arbre dont chaque nœud englobe ses enfants dans une boîte AABB ; pour tester un rayon contre un million de triangles, on commence par tester la racine, puis on descend récursivement seulement dans les enfants intersectés, ce qui réduit la complexité de $O(n)$ à $O(\log n)$ par rayon).

> **Structures spatiales d'accélération.** Quand on a beaucoup d'objets ou de triangles à tester (collision, ray tracing, culling), on les organise dans une structure hiérarchique :
>
> - **BVH** : arbre de boîtes englobantes, expliqué ci-dessus. Le standard pour le ray tracing temps réel.
> - **Octree** : arbre où chaque nœud a **8** enfants — on subdivise un cube en 8 sous-cubes égaux, récursivement. Idéal pour partitionner uniformément l'espace 3D (utilisé par voxel cone tracing, par les colliders de Unity, par les particules SPH). En 2D, l'analogue est le *quadtree* (4 enfants).
> - **KD-tree** (*K-Dimensional tree*) : arbre binaire qui découpe alternativement selon $x$, $y$, $z$… au plan médian des points qu'il contient. Excellent pour la recherche du plus proche voisin (*nearest neighbor*) et le ray tracing offline (PBRT).
> - **BSP** (*Binary Space Partitioning*) : arbre binaire où chaque nœud découpe l'espace par un **plan arbitraire** (pas forcément aligné). Inventé pour Doom (1993) où il permettait au CPU de l'époque de trier les murs back-to-front en $O(n)$ au lieu de $O(n \log n)$. Toujours utilisé pour la génération de niveaux de roguelike (cf. section *Génération procédurale*).

#### Path tracing — la généralisation

Au lieu de s'arrêter à la première intersection, on **rebondit** : à chaque touche, on tire un nouveau rayon dans une direction échantillonnée selon la BRDF, jusqu'à atteindre une source lumineuse. C'est une **estimation Monte-Carlo** de l'intégrale de rendu :

```math
L_o \approx \frac{1}{N} \sum_{k=1}^{N} \frac{f_r(\boldsymbol{\omega}_i^{(k)}, \boldsymbol{\omega}_o)\,L_i(\boldsymbol{\omega}_i^{(k)})\,(\boldsymbol{\omega}_i^{(k)} \cdot \mathbf{n})}{p(\boldsymbol{\omega}_i^{(k)})}
```

où $p$ est la **densité de probabilité** d'échantillonnage. Plus $N$ est grand, moins l'image est bruitée. *Quake II RTX*, le mode *Path Tracing* de *Cyberpunk 2077* et *Portal RTX* sont des path tracers temps réel ; ils s'appuient sur des **denoisers neuronaux** — des réseaux de neurones (typiquement NVIDIA OptiX) entraînés à transformer une image très bruitée (1 à 2 échantillons par pixel) en une image propre, en exploitant les corrélations spatiales et temporelles que produisent les BRDF et les vecteurs de mouvement.

#### Global illumination

Tous les rebonds **autres que la première intersection** forment l'**illumination globale**. En temps réel on l'approxime :

- **Lightmaps précalculées** (Quake, 1996) — les surfaces statiques ont leur éclairage cuit dans des textures. Coût zéro à l'exécution mais inutilisable sur géométrie dynamique.
- **Voxel cone tracing** (*VXGI* — *Voxel Global Illumination*, NVIDIA 2014) — on **voxellise** la scène (on la découpe en cubes 3D, les *voxels*, comme un Minecraft à très haute résolution) et on propage la lumière dans cette grille. Pour échantillonner l'irradiance d'un point, on tire un **cône** (au lieu d'un rayon fin) qui s'élargit en avançant et lit les voxels à différents niveaux de mipmap — d'où l'expression *cone tracing*. Compromis : approximatif, mais 100× plus rapide qu'un vrai ray tracing par rebond.
- **Probes irradiance** (*Light Probes*, **DDGI** — *Dynamic Diffuse Global Illumination*, NVIDIA 2019) — on échantillonne l'irradiance en quelques points stratégiques (les *probes*) répartis dans la scène, on stocke les coefficients d'harmoniques sphériques, et chaque pixel interpole entre les probes voisines. DDGI met à jour les probes en continu via du ray tracing pour s'adapter aux changements de lumière dynamiques.
- **Lumen** (Unreal Engine 5, 2022) — combinaison hybride de surface caching + screen tracing + (optionnel) ray tracing matériel.

#### Upscaling temporel — DLSS, FSR, XeSS

Le rendu d'image en 4K coûte 4× le rendu 1080p. La parade : rendre à résolution interne plus basse (souvent 1440p ou 1080p) puis **reconstruire** la 4K en exploitant les **frames précédentes**. C'est le rôle des techniques d'upscaling :

- **DLSS** (NVIDIA, 2018) : réseau de neurones entraîné *offline* sur des images haute résolution de référence. DLSS 1 utilisait un réseau convolutionnel (CNN, spécialisé dans la reconnaissance de motifs locaux) ; DLSS 2+ s'appuie sur une architecture plus sophistiquée combinant reprojection temporelle et débruitage neuronal.
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

---

[← Réseau et multijoueur](09-reseau-et-multijoueur.md) · [↑ Sommaire](../README.md#table-des-matières) · [Pipeline de rendu →](11-pipeline-de-rendu.md)
