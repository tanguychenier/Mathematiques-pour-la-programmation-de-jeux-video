[← Réseau et multijoueur](09-reseau-et-multijoueur.md) · [↑ Sommaire](../README.md#table-des-matières) · [Pipeline de rendu →](11-pipeline-de-rendu.md)

# 10. Techniques avancées

Un jeu vidéo moderne doit parfois fabriquer une planète entière, faire couler de l'eau qui éclabousse, ou peindre une image presque aussi vraie qu'une photo. Pour réussir ces prouesses, les programmeurs assemblent des outils mathématiques puissants. Les voici, un par un, avec à chaque fois la recette de cuisine qui se cache derrière le nom savant.

### Génération procédurale et bruit

Imaginez devoir dessiner à la main chaque montagne, chaque arbre et chaque caillou d'un monde grand comme un vrai continent. Personne n'aurait assez d'une vie. La **génération procédurale** résout ce problème : au lieu de tout dessiner soi-même, on écrit une recette de calcul qui fabrique le décor (le terrain, les images plaquées sur les objets, les niveaux, les régions) toute seule.

> **Que veut dire « algorithme » ?** C'est une suite d'instructions précises, dans l'ordre, comme une recette de gâteau : « casse deux œufs, ajoute la farine, mélange ». L'ordinateur suit la recette à la lettre et obtient toujours le même résultat.

> **Que veut dire « biome » ?** C'est un grand type de paysage avec son ambiance : une forêt, un désert, une banquise, une jungle. Dans un jeu, chaque biome a ses arbres, ses couleurs et ses animaux.

Cette recette s'appuie le plus souvent sur des **fonctions de bruit** dites **déterministes** : des calculs qui rendent toujours le même résultat pour une même question, tout en ayant l'air désordonnés.

> **Que veut dire « déterministe » ?** Cela veut dire « sans surprise » : si vous reposez exactement la même question, vous obtenez exactement la même réponse, comme une calculatrice qui répond toujours $`4`$ quand on tape $`2 + 2`$. C'est pratique dans un jeu : deux joueurs sur deux ordinateurs différents voient le même monde, parce que la recette donne le même résultat partout.

> **Que veut dire « bruit » en programmation graphique ?** Pas un son ici. C'est une **fonction mathématique**, c'est-à-dire une machine à calculer qui prend des nombres en entrée et en ressort un autre. On la note $`f(x, y)`$ : pour chaque endroit du plan repéré par ses deux coordonnées $`x`$ (gauche-droite) et $`y`$ (avant-arrière), elle rend une valeur qui semble désordonnée mais que l'on peut retrouver à volonté. Pour la voir, on la dessine en nuances de gris : valeur basse, on peint du noir ; valeur haute, on peint du blanc. Le bruit qui nous intéresse est en plus **continu** : on peut zoomer dessus sans jamais voir de marche d'escalier, les valeurs glissent doucement de l'une à l'autre.
>
> **Pourquoi le « bruit » sert-il à fabriquer des décors ?** Parce que la nature elle-même est faite de variations douces et un peu hasardeuses : aucune colline n'est parfaitement lisse, aucune n'est parfaitement en dents de scie. Un bruit continu imite exactement ce genre de désordre tranquille, donc le terrain qu'on en tire a l'air naturel.

#### Bruit blanc

> **Que veut dire « bruit blanc » ?** C'est le bruit le plus simple : à chaque case, on tire une valeur au hasard sans regarder du tout les cases voisines. Le nom vient de la **lumière blanche**, qui mélange toutes les couleurs à parts égales, et du **son blanc**, le chuintement régulier d'une radio mal réglée. À l'œil, c'est la fameuse « neige » d'une vieille télévision sans image : chaque point est tiré au sort tout seul, sans aucun lien avec ses voisins.

> **Que veut dire « pixel » ?** C'est le plus petit point coloré d'une image à l'écran. Une image, c'est une grille de milliers de pixels, comme une mosaïque de minuscules carreaux de couleur.

Écrivons cela en langage mathématique :

```math
N_\text{blanc}(x, y) = \mathrm{hash}(x, y) \in [0, 1]
```

> **Que veut dire « hash » ?** C'est une moulinette qui transforme des nombres en un autre nombre, d'une façon brouillonne mais toujours pareille. Donnez-lui la case $`(3, 5)`$, elle rend par exemple $`0{,}71`$ ; redemandez $`(3, 5)`$, elle rend encore $`0{,}71`$. Le résultat a l'air tiré au sort, mais il est en réalité fixé d'avance : voilà comment on obtient du « hasard » qui ne change jamais.

> **Le symbole $`\in`$.** Il se lit « appartient à » et veut dire « se trouve dans ». L'écriture $`\in [0, 1]`$ signifie donc « la valeur reste comprise entre $`0`$ et $`1`$ », bornes incluses. Ici, $`0`$ correspond au noir et $`1`$ au blanc.

> **Pourquoi le bruit blanc est-il inutilisable tout seul ?** Parce que chaque point ignore ses voisins : deux points côte à côte peuvent être l'un tout noir, l'autre tout blanc. Pour un terrain, le sol bondirait à $`100`$ mètres de haut puis retomberait à zéro d'un pas à l'autre, impossible à jouer. Le bruit blanc sert donc seulement de **réserve de hasard** : d'autres recettes plus malignes viennent ensuite y mettre de l'ordre, c'est-à-dire forcer les points voisins à se ressembler. Le **bruit de valeur** relie les valeurs des coins par une pente douce, le **bruit de Perlin** relie des directions, et le **bruit fractal** empile plusieurs couches de tailles différentes.

> **Que veut dire « interpoler » ?** C'est deviner la valeur entre deux valeurs connues, en suivant une pente régulière. S'il fait $`20`$ degrés à midi et $`24`$ degrés deux heures plus tard, on interpole $`22`$ degrés au bout d'une heure, pile au milieu. C'est l'outil qui change un semis de points isolés en une surface lisse.

#### Bruit de Perlin

> **Que veut dire « bruit de Perlin » ?** C'est une fonction de bruit douce et sans cassure, inventée par **Ken Perlin** pour fabriquer les images du film *Tron* en 1982, puis montrée à tous lors d'un grand congrès d'images de synthèse, le SIGGRAPH, en 1985. Vue de dessus, elle ressemble vraiment à un paysage : des collines et des vallées qui s'enchaînent en douceur. Perlin a même reçu un Oscar technique en 1997 pour cette trouvaille. Aujourd'hui, c'est la base de presque tous les terrains des jeux à grand monde ouvert (*Minecraft*, *No Man's Sky*, *Terraria*), souvent dans la version améliorée Simplex présentée plus bas.

Voici l'idée, sans formule. On quadrille le plan en cases. À chaque coin de case, on accroche une petite flèche qui pointe dans une direction tirée « au hasard fixé », c'est-à-dire toujours la même pour ce coin. Quand on veut connaître la valeur du bruit en un point posé quelque part dans une case, on regarde les quatre coins de cette case et on mélange leurs influences, en donnant plus de poids aux coins les plus proches du point.

> **Que veut dire « vecteur gradient » ?** Un **vecteur**, c'est une flèche : il a une direction et une longueur. Ici, le mot **gradient** désigne simplement la direction de la pente accrochée à un coin, comme une girouette qui montrerait « par là, ça monte ». Chaque coin a sa propre flèche, et c'est ce petit jeu de flèches qui crée le relief.

Concrètement, pour un point $`(x, y)`$ :

1. Trouver la case qui contient $`(x, y)`$ ;
2. Pour chaque coin de la case, retrouver sa flèche grâce au hash ;
3. Pour chaque coin, calculer le **produit scalaire** entre sa flèche et la flèche qui va du coin jusqu'au point $`(x, y)`$ ;
4. **Interpoler** ces résultats avec une fonction d'adoucissement, ici la fonction `smootherstep`.

> **Que veut dire « produit scalaire » ?** C'est une opération entre deux flèches qui répond à la question « pointent-elles dans le même sens ? ». Le résultat est un seul nombre : grand et positif si les deux flèches vont dans la même direction, nul si elles forment un angle droit, négatif si elles s'opposent. C'est l'outil qui dit à chaque coin combien il « tire » le terrain vers le haut ou vers le bas à l'endroit demandé.

> **Que veut dire « fonction d'adoucissement » (easing) ?** C'est une petite fonction qui remplace une pente droite par une courbe en S, pour que le passage d'une valeur à l'autre démarre tout doux, accélère au milieu, puis ralentisse à l'arrivée, comme une voiture qui ne fait pas de à-coups. Sans elle, les frontières entre cases se verraient.

La fonction `smootherstep` utilisée ici est :

```math
\mathrm{smootherstep}(t) = 6t^5 - 15t^4 + 10t^3
```

> **Que veut dire $`t^5`$ et la lettre $`t`$ ?** La lettre $`t`$ est juste un nombre qui avance de $`0`$ (le début) à $`1`$ (la fin), comme une barre de progression. L'écriture $`t^5`$ se lit « $`t`$ puissance $`5`$ » et veut dire « $`t`$ multiplié cinq fois par lui-même » : $`t^5 = t \times t \times t \times t \times t`$. Mélanger ainsi plusieurs puissances de $`t`$ donne la jolie courbe en S recherchée.

> **Que veut dire « bruit de Simplex » ?** C'est la version améliorée que Perlin a proposée en 2001. Au lieu de quadriller le plan en carrés, elle le pave en triangles (et en pyramides à quatre faces, les tétraèdres, dans l'espace). Deux avantages : il y a moins de calculs quand on travaille en trois ou quatre dimensions, et le résultat ne laisse plus apparaître de vilains alignements le long des axes, ces sortes de « rails » que l'œil repère. Petit souci d'histoire : Perlin avait déposé un brevet qui a longtemps découragé son emploi ; ce brevet a expiré, et les versions libres OpenSimplex et OpenSimplex2 servent aujourd'hui de référence.

> **Que veut dire « dimension » (3D, 4D) ?** Une dimension, c'est une direction indépendante dans laquelle on peut se déplacer. Une ligne en a une, une feuille de papier deux (gauche-droite et haut-bas), notre monde trois (on ajoute la profondeur). En jeu, on ajoute parfois le temps comme quatrième direction, pour fabriquer un bruit qui bouge.

#### Bruit fractal (FBM : *Fractional Brownian Motion*)

> **Que veut dire « FBM » ?** Ces trois lettres viennent de l'anglais *Fractional Brownian Motion*, le « mouvement brownien fractionnaire ». Derrière ce nom intimidant se cache une idée toute simple, que l'on observe dans un vrai paysage : il y a des montagnes, puis des collines posées sur ces montagnes, puis des rochers sur ces collines, puis des cailloux sur ces rochers. Autrement dit, **du détail à toutes les tailles**. La FBM imite cela en empilant plusieurs couches du même bruit, chacune deux fois plus fine et deux fois plus discrète que la précédente. Le résultat ressemble enfin à un vrai relief.

```math
\mathrm{FBM}(x, y) = \sum_{i=0}^{N-1} \frac{1}{2^i} \cdot \mathrm{noise}\!\big(2^i \cdot x,\ 2^i \cdot y\big)
```

> **Le symbole $`\sum`$.** Cette grande lettre grecque (un « S » majuscule, pour « Somme ») veut dire « additionne tout ». L'écriture $`\sum_{i=0}^{N-1}`$ se lit « fais varier $`i`$ de $`0`$ jusqu'à $`N-1`$, et additionne ce qui suit pour chaque valeur de $`i`$ ». C'est juste une façon courte d'écrire « première couche, plus deuxième couche, plus troisième couche, et ainsi de suite ».
>
> **Comment lire le reste de la formule.** Le facteur $`1/2^i`$ rend chaque couche plus discrète : $`1`$ pour la première, puis la moitié, puis le quart. Le facteur $`2^i`$ devant le $`x`$ et le $`y`$ rétrécit le motif : la deuxième couche est deux fois plus serrée, la troisième quatre fois, ce qui ajoute des détails de plus en plus fins.

Chaque couche s'appelle une **octave**, mot emprunté à la musique : doubler la fréquence d'un son, c'est monter d'une octave, exactement comme on double ici la finesse à chaque étage. Avec quatre à six octaves, on obtient déjà un terrain convaincant.

> **Que veut dire « fréquence » ?** C'est le nombre de fois qu'un motif se répète sur une distance donnée. Une basse fréquence donne de grandes ondulations bien espacées (les montagnes) ; une haute fréquence donne plein de petites bosses rapprochées (les cailloux).

#### Applications

- **Carte d'altitude (heightmap)** : *Minecraft*, *No Man's Sky* et *Terraria* utilisent un bruit fractal pour décider de la hauteur du sol en chaque point.
- **Images de matière calculées** : marbre, bois, nuages, eau. Le veinage d'un tronc, par exemple, vient d'une FBM retravaillée.
- **Répartition des objets** : on pose une règle de seuil, c'est-à-dire une note à dépasser. Si le bruit vaut $`N(x, y) > 0{,}7`$ à un endroit, on y plante un arbre ; sinon, on laisse le sol nu. On obtient ainsi des forêts naturelles, sans gros paquets d'arbres collés.
- **Donjons et niveaux** : ici, on préfère des recettes faites sur mesure.

> **Que veut dire « carte d'altitude » (heightmap) ?** C'est une image en nuances de gris où la couleur d'un point code sa hauteur : plus c'est clair, plus le terrain est haut. L'ordinateur lit cette image comme une carte de relief et soulève le sol en conséquence.

Voici quelques recettes faites sur mesure pour fabriquer des niveaux :

- **BSP** (de l'anglais *Binary Space Partitioning*, « découpage de l'espace en deux ») : on coupe une zone en deux, puis chaque moitié en deux à son tour, encore et encore, jusqu'à obtenir des pièces. Employé par *Rogue* en 1980 et par presque tous les jeux de la même famille.
- **Effondrement de fonction d'onde** (*Wave Function Collapse*, Maxim Gumin, 2016) : une recette inspirée de la physique des particules, où chaque case « choisit » un motif compatible avec ses voisines, de proche en proche. À partir d'un simple exemple, elle produit des décors étonnamment cohérents.
- **Marche aléatoire** (*random walk*) : un petit explorateur imaginaire se promène au hasard et creuse chaque case qu'il traverse, ce qui donne des grottes aux formes naturelles.

> **Que veut dire « récursivement » ?** Cela veut dire « en se rappelant soi-même » : on applique la même opération sur le résultat de l'opération précédente, encore et encore, comme des poupées russes emboîtées. Couper une zone en deux, puis couper chaque moitié de la même façon, c'est récursif.

Voici comment cette idée s'écrit pour de vrai, en langage C# dans le moteur de jeu Unity. Le programme empile cinq octaves de bruit de Perlin (la fonction `Mathf.PerlinNoise` est le bruit de Perlin tout prêt fourni par Unity), en divisant la force par deux et en doublant la finesse à chaque tour de boucle, exactement comme dans la formule de la FBM :

> **Que veut dire « moteur de jeu » ?** C'est une grosse boîte à outils logicielle, comme Unity ou Unreal, qui fournit déjà l'affichage, le son, la physique et bien d'autres briques. Le créateur de jeu n'a plus qu'à assembler ces briques au lieu de tout réécrire de zéro.

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

Faire couler de l'eau, monter de la fumée ou ondoyer de la lave de façon crédible, voilà ce que vise la **simulation de fluides**. Le mot **fluide** désigne tout ce qui coule, c'est-à-dire les liquides comme l'eau et les gaz comme la fumée. Pour décrire leur mouvement, les physiciens utilisent une équation célèbre, les **équations de Navier-Stokes** :

```math
\rho \left( \frac{\partial \mathbf{v}}{\partial t} + \mathbf{v} \cdot \nabla \mathbf{v} \right) = -\nabla p + \mu \nabla^2 \mathbf{v} + \mathbf{f}
```

Elle a l'air effrayante, mais elle dit en réalité une phrase simple : « la masse d'un petit bout de fluide, multipliée par son accélération, est égale à la somme des forces qui le poussent ». C'est la même loi que pour une bille qu'on lance, appliquée à chaque goutte. Voici ce que désignent les lettres :

> **Que veulent dire les lettres de l'équation ?** $`\rho`$ (la lettre grecque « rhô ») est la **densité**, c'est-à-dire la quantité de matière tassée dans un petit volume : l'eau est plus dense que l'air. $`\mathbf{v}`$ écrit en gras est la **vitesse** du fluide, une flèche qui indique vers où et à quelle allure il file. $`p`$ est la **pression**, la force avec laquelle le fluide pousse sur ce qui l'entoure. $`\mu`$ (« mu ») est la **viscosité**, le côté « épais » d'un liquide : le miel a une grande viscosité, l'eau une petite. $`\mathbf{f}`$ regroupe les **forces extérieures**, comme la gravité qui tire tout vers le bas.

> **Pourquoi écrit-on certaines lettres en gras ?** Le gras signale une flèche (un vecteur), qui a une direction. Une lettre normale signale un simple nombre. La vitesse $`\mathbf{v}`$ est en gras parce qu'elle a un sens ; la pression $`p`$ est un nombre, donc en maigre.

> **Que veut dire « équation aux dérivées partielles » ?** Une **dérivée** mesure une vitesse de changement : « de combien telle chose grandit quand telle autre avance d'un cran ». Une dérivée est **partielle** quand on ne fait varier qu'une seule chose à la fois en bloquant les autres, comme régler la chaleur d'un four sans toucher au minuteur. Une équation aux dérivées partielles relie ces vitesses de changement entre elles ; elle décrit comment un fluide évolue à la fois dans l'espace et dans le temps.

> **Le symbole $`\nabla`$.** On le lit « nabla ». C'est une machine qui prend un champ de nombres (par exemple la pression en chaque point) et rend en chaque point une flèche pointant vers là où ce nombre grimpe le plus vite, comme une bille qui montrerait toujours la pente la plus raide. Cette flèche s'appelle le **gradient**.
>
> **Que veut dire « champ » ?** Un champ, c'est une valeur attribuée à chaque point de l'espace, comme la température dans toutes les pièces d'une maison. Un **champ scalaire** range un simple nombre en chaque point (la température) ; un **champ de vitesse** range une flèche en chaque point (le courant de l'eau).
>
> **Le symbole $`\nabla^2`$.** On l'appelle le « laplacien ». En chaque point, il compare la valeur du point à la moyenne de ses voisins immédiats. S'il vaut zéro, le point est dans la moyenne ; sinon, il forme une bosse ou un creux. Cette comparaison « moi par rapport à mes voisins » revient partout où quelque chose se répand : la chaleur, l'épaisseur d'un liquide, les vagues.
>
> **Que veut dire $`\partial \mathbf{v}/\partial t`$ ?** C'est la vitesse à laquelle la vitesse change au fil du temps en un point donné, autrement dit l'accélération du fluide à cet endroit. Le rond $`\partial`$ est juste le « d » des dérivées partielles.

Pour résoudre une telle équation sur un ordinateur, on ne peut pas calculer chaque goutte parfaitement : on ruse avec des méthodes approchées. Les trois grandes familles :

> **Que veut dire « numérique » ici ?** Cela veut dire « avec des nombres approchés calculés pas à pas par l'ordinateur », par opposition à une solution exacte écrite avec des formules. On coupe le problème en tout petits morceaux et on additionne.

- **Différences finies** : on quadrille l'espace en petites cases régulières et on remplace chaque vitesse de changement par une simple soustraction entre cases voisines. C'est facile à programmer et cela convient quand la grille reste fixe pendant que le fluide la traverse (on parle d'approche d'Euler).
- **Éléments finis** : on découpe l'espace en triangles ou en pyramides de tailles variées et on cherche la solution morceau par morceau. C'est plus précis sur des formes compliquées, mais plus lourd à calculer ; on s'en sert dans l'industrie et pour simuler finement les tissus.
- **SPH** (de l'anglais *Smoothed Particle Hydrodynamics*, « hydrodynamique des particules lissées ») : ici, pas de grille du tout. Le fluide est représenté par un nuage de petites billes qui transportent chacune leur vitesse et leur pression, et chaque bille tient compte de ses voisines proches (on parle d'approche de Lagrange, qui suit la matière). C'est la méthode reine pour l'eau et le sang dans les jeux.

> **Euler ou Lagrange ?** Deux façons de regarder un fleuve. À la manière d'**Euler**, on plante des bouées fixes et on note l'eau qui passe à chaque bouée : la grille ne bouge pas. À la manière de **Lagrange**, on lâche des canards en plastique et on suit chacun dans sa dérive : ce sont les particules qui voyagent.

### Écrans multiples et fenêtrage

Beaucoup de jeux laissent jouer sur **plusieurs écrans** côte à côte, ou dans une **fenêtre que l'on agrandit à la souris**. Cela demande au programme de savoir s'adapter à toutes les tailles d'affichage et à tous les moniteurs branchés. Trois soucis se posent alors :

- la **synchronisation** : deux écrans ne se rafraîchissent pas forcément au même rythme, et il faut éviter qu'ils se contredisent ;
- la **puissance graphique** : deux ou trois écrans, c'est deux ou trois fois plus de points à calculer chaque seconde ;
- la **forme de l'écran** : un écran carré, large ou très large oblige à régler la largeur de vue pour ne pas déformer l'image.

> **Que veut dire « taux de rafraîchissement » ?** C'est le nombre de fois par seconde où l'écran redessine son image complète. À $`60`$ hertz, l'image est repeinte $`60`$ fois par seconde ; plus ce nombre est élevé, plus le mouvement paraît fluide.

> **Que veut dire « FOV » (largeur de vue) ?** Ces lettres viennent de l'anglais *Field Of View*, le « champ de vision » : c'est l'angle, mesuré en degrés, de tout ce que la caméra du jeu embrasse à la fois, comme l'ouverture de vos yeux. Trop étroit, on a l'impression de regarder par un tube ; trop large sur un petit écran, les bords s'étirent et se déforment.

### Intelligence artificielle avancée

Pour rendre les personnages contrôlés par la machine plus crédibles, on fait appel à des techniques d'intelligence artificielle plus poussées : l'**apprentissage automatique**, la **planification** (préparer une suite d'actions pour atteindre un but) et le **traitement du langage**, qui permet à un personnage de comprendre et de produire des phrases. Grâce à elles, les personnages réagissent de façon vivante et le jeu s'adapte au joueur.

> **Que veut dire « PNJ » ?** Ce sont les lettres de « personnage non joueur » : tous les personnages du jeu que vous ne dirigez pas vous-même, comme le marchand du village ou les ennemis. C'est l'ordinateur qui les anime.

> **Que veut dire « apprentissage automatique » ?** C'est l'art d'apprendre à une machine en lui montrant beaucoup d'exemples, plutôt qu'en lui dictant des règles. On lui montre des milliers de photos de chats, et elle finit par reconnaître un chat toute seule, comme un enfant qui apprend en regardant.

Pour cela, les studios utilisent des boîtes à outils logicielles spécialisées (par exemple TensorFlow, PyTorch ou ONNX Runtime) qui servent à **entraîner** des modèles, c'est-à-dire à les faire répéter sur des exemples jusqu'à ce qu'ils deviennent bons : reconnaître une image, écrire un texte, fabriquer une voix.

> De plus en plus de jeux intègrent des **modèles génératifs** pour inventer à la volée des dialogues, des missions ou des images.

> **Que veut dire « modèle génératif » ?** C'est une intelligence artificielle qui ne se contente pas de reconnaître, mais qui **crée** du neuf : une phrase, une image, une musique. Les **grands modèles de langage** (en anglais *LLM*) inventent du texte ; les modèles de **diffusion** fabriquent des images en partant d'un brouillon tout flou qu'ils nettoient peu à peu.

### Rendu avancé

Le mot **rendu** désigne l'étape où l'ordinateur transforme une scène en trois dimensions (des objets, des lumières, une caméra) en une image plate affichée à l'écran, comme un photographe transforme une scène réelle en photo. Le **rendu avancé** rassemble les techniques modernes qui rendent cette image presque aussi vraie qu'une photographie. Chacune cache une idée mathématique, que voici à chaque fois, et pas seulement un nom à la mode.

#### Rendu basé sur la physique (PBR) et l'équation de rendu

> **Que veut dire « PBR » ?** Ces lettres viennent de l'anglais *Physically-Based Rendering*, le « rendu fondé sur la physique ». Plutôt que d'imiter la lumière au jugé avec de vieilles recettes approximatives, on part de la vraie loi physique qui dit comment la lumière rebondit sur une surface, puis on la simplifie juste assez pour que l'ordinateur la calcule à temps. Résultat : un métal a vraiment l'air d'un métal, une peau d'une peau.

Cette vraie loi porte un nom : l'**équation de rendu**, écrite par James Kajiya en 1986. Elle calcule, pour un point $`\mathbf{x}`$ d'une surface, la quantité de lumière qui repart vers l'œil dans une direction notée $`\boldsymbol{\omega}_o`$ :

```math
L_o(\mathbf{x}, \boldsymbol{\omega}_o) = L_e(\mathbf{x}, \boldsymbol{\omega}_o) + \int_{\Omega} f_r(\mathbf{x}, \boldsymbol{\omega}_i, \boldsymbol{\omega}_o)\,L_i(\mathbf{x}, \boldsymbol{\omega}_i)\,(\boldsymbol{\omega}_i \cdot \mathbf{n})\,\mathrm{d}\boldsymbol{\omega}_i
```

> **Le symbole $`\int`$.** C'est une **intégrale**, autrement dit une « addition sans fin ». Le symbole $`\sum`$ vu plus haut additionnait des morceaux comptables un par un ; l'intégrale, elle, additionne la contribution de **tous** les points d'un domaine continu, sans en oublier un seul, comme si l'on découpait une surface en une infinité de tranches minuscules et qu'on en faisait la somme. Ici, $`\int_\Omega \dots\,\mathrm{d}\boldsymbol{\omega}`$ additionne la lumière qui arrive de **toutes** les directions au-dessus de la surface.

> **Que veut dire l'oméga, en petit et en grand ?** L'oméga en gras et minuscule, $`\boldsymbol{\omega}`$, est une **direction** : une flèche de longueur $`1`$ qui pointe vers un endroit du ciel. L'oméga majuscule, $`\Omega`$, est l'**ensemble de toutes ces directions** au-dessus de la surface, c'est-à-dire la moitié de ciel visible, qu'on appelle un **hémisphère** (comme la moitié d'un ballon). On range les directions de cet hémisphère à l'aide de deux angles, $`\theta`$ et $`\phi`$, exactement comme on repère un point sur le globe terrestre avec sa latitude et sa longitude.

où chaque morceau a un rôle :

- $`L_e`$ est la lumière que la surface **émet elle-même**, quand c'est un objet lumineux comme une lampe ou un écran.
- $`L_i`$ est la lumière qui **arrive** sur la surface depuis la direction $`\boldsymbol{\omega}_i`$.
- $`f_r`$ est la **BRDF** : elle indique quelle part de la lumière entrant par la direction $`\boldsymbol{\omega}_i`$ ressort vers la direction $`\boldsymbol{\omega}_o`$. C'est la carte d'identité optique d'une matière : un miroir, un mur de plâtre et un velours rouge réagissent chacun à leur façon.
- $`\boldsymbol{\omega}_i \cdot \mathbf{n}`$ est le **facteur d'inclinaison** : une lumière qui frappe la surface de biais l'éclaire moins fort qu'une lumière qui tombe droit dessus, exactement comme le soleil chauffe plus à midi qu'au coucher.

> **Que veut dire « BRDF » ?** Ces lettres viennent de l'anglais et décrivent une fonction qui répond, pour chaque couple de directions « d'où vient la lumière » et « où part la lumière », à la question « combien passe ? ». Ses cousines : la **BTDF** s'occupe de la lumière qui **traverse** un objet transparent comme le verre ou l'eau, et la **BSDF** réunit les deux, ce qui entre, ce qui ressort et ce qui traverse.

> **Que veut dire « la normale » $`\mathbf{n}`$ ?** C'est la flèche qui sort perpendiculairement d'une surface, comme un mât planté tout droit sur un terrain. Elle indique « le dehors » de la surface et sert à mesurer sous quel angle la lumière la frappe.

Cette intégrale ne se résout pas avec une jolie formule exacte : on est obligé de l'**approcher**. Les jeux en temps réel emploient pour cela une recette appelée **BRDF de Cook-Torrance à microfacettes**, dont voici la forme habituelle :

```math
f_r(\boldsymbol{\omega}_i, \boldsymbol{\omega}_o) = \underbrace{\frac{c_\text{diff}}{\pi}}_\text{Lambertien} + \underbrace{\frac{D(\mathbf{h}) \cdot F(\boldsymbol{\omega}_o, \mathbf{h}) \cdot G(\boldsymbol{\omega}_i, \boldsymbol{\omega}_o)}{4\,(\mathbf{n}\cdot\boldsymbol{\omega}_i)\,(\mathbf{n}\cdot\boldsymbol{\omega}_o)}}_\text{Spéculaire microfacets}
```

Le terme de gauche (« lambertien ») est la lumière mate, celle qui se diffuse également dans toutes les directions, comme sur une feuille de papier. Le terme de droite (« spéculaire ») est le reflet brillant, comme le point de lumière qui glisse sur une pomme cirée. La flèche $`\mathbf{h}`$ qui apparaît dans ce reflet est la **flèche du milieu**, posée pile entre la direction de la lumière et celle de l'œil :

```math
\mathbf{h} = \dfrac{\boldsymbol{\omega}_i + \boldsymbol{\omega}_o}{\|\boldsymbol{\omega}_i + \boldsymbol{\omega}_o\|}
```

> **Que veut dire « microfacette » ?** Vue de loin, une surface paraît lisse, mais au microscope elle est couverte d'une multitude de minuscules miroirs penchés dans tous les sens, les microfacettes. Plus ils sont bien rangés, plus le reflet est net ; plus ils partent en tous sens, plus le reflet s'étale et la surface paraît mate.

> **Que veut dire « cosinus » ?** Le cosinus est un nombre, entre $`-1`$ et $`1`$, qui mesure à quel point deux flèches regardent dans la même direction : $`1`$ quand elles sont parfaitement alignées, $`0`$ quand elles forment un angle droit, $`-1`$ quand elles sont opposées. Le produit scalaire de deux flèches de longueur $`1`$ donne justement ce cosinus.

> **Que veut dire « dénominateur » ?** Dans une fraction comme $`\frac{3}{4}`$, le nombre du bas ($`4`$) est le dénominateur : c'est ce par quoi on divise. Quand il s'approche de zéro, la fraction s'emballe vers l'infini, et c'est ce danger qu'il faut surveiller.

Les deux cosinus $`\mathbf{n}\cdot\boldsymbol{\omega}_i`$ et $`\mathbf{n}\cdot\boldsymbol{\omega}_o`$ se trouvent au dénominateur, donc en bas de la fraction. On les garde toujours positifs (en prenant leur **valeur absolue**, c'est-à-dire en oubliant le signe), pour deux raisons : un cosinus négatif voudrait dire qu'on observe le dos de la surface, ce qui n'a pas de sens ici, et un cosinus trop proche de zéro ferait exploser la fraction. Voici enfin les trois ingrédients $`D`$, $`F`$ et $`G`$, le vrai secret de cuisine du rendu moderne :

```math
D_\text{GGX}(\mathbf{h}) = \frac{\alpha^2}{\pi\,\big[(\mathbf{n}\cdot\mathbf{h})^2(\alpha^2 - 1) + 1\big]^2}, \qquad \alpha = \text{roughness}^2
```

```math
F_\text{Schlick}(\boldsymbol{\omega}_o, \mathbf{h}) = F_0 + (1 - F_0)\,(1 - \mathbf{h}\cdot\boldsymbol{\omega}_o)^5
```

```math
G_\text{Smith}(\boldsymbol{\omega}_i, \boldsymbol{\omega}_o) = G_1(\boldsymbol{\omega}_i)\,G_1(\boldsymbol{\omega}_o), \qquad G_1(\boldsymbol{\omega}) = \frac{\mathbf{n}\cdot\boldsymbol{\omega}}{(\mathbf{n}\cdot\boldsymbol{\omega})(1 - k) + k}, \quad k = \frac{(\text{roughness} + 1)^2}{8}
```

> **Note.** La formule de $`k`$ ci-dessus (Karis, Unreal 4) est l'approximation pour les **lumières directes**. Pour l'éclairage à base d'image (*IBL*, *Image-Based Lighting*), on utilise $`k = \text{roughness}^2 / 2`$ afin d'éviter un biais sur les surfaces lisses.

Chacune de ces trois lettres répond à une question simple sur le reflet :

- **$`D`$, la répartition des petits miroirs** : combien de microfacettes sont orientées pile dans la bonne direction $`\mathbf{h}`$ pour renvoyer le reflet vers l'œil ? C'est cette lettre qui dessine la forme de la tache brillante.
- **$`F`$, l'effet Fresnel** : pourquoi une surface devient-elle plus réfléchissante quand on la regarde de biais ? Pensez à un lac : à vos pieds vous voyez le fond, mais au loin, à ras de l'eau, la surface devient un miroir. Le nombre $`F_0`$ dit à quel point la surface réfléchit déjà quand on la regarde droit dessus.
- **$`G`$, l'auto-ombre** : sur une surface bosselée, certains petits miroirs se cachent les uns les autres et se font de l'ombre. Cette lettre retire la lumière ainsi perdue.

> **Que veut dire « effet Fresnel » ?** C'est le fait que toute surface réfléchit beaucoup plus la lumière quand on la regarde par la tranche, presque à plat, que quand on la regarde de face. C'est pourquoi une route mouillée brille au loin et pas à vos pieds.

##### GGX vs Beckmann vs Trowbridge-Reitz : trois NDF, une histoire

Le terme $`D`$ a une petite histoire à trois épisodes. Avant tout, un mot de vocabulaire.

> **Que veut dire « NDF » ?** Ce sont les initiales anglaises de « fonction de distribution des normales ». En clair, c'est la formule qui compte, parmi tous les minuscules miroirs d'une surface, combien pointent dans chaque direction. C'est exactement le rôle de la lettre $`D`$.

> **Que veut dire « reflet serré » ou « reflet étalé » ?** Le reflet, c'est le point brillant qu'on voit sur un objet éclairé. Sur un objet très lisse, ce point est petit et net (serré) ; sur un objet un peu rugueux, il s'étale en un halo doux. Une bonne formule doit savoir étaler ce halo de façon naturelle.

- **Beckmann** (1963) : la toute première NDF, fondée sur un modèle de surface en cloche. Son défaut : le reflet tombe trop vite à zéro sur les bords, donc il paraît trop dur pour un métal poli. On l'emploie encore pour les calculs de très haute précision, hors temps réel.
- **Trowbridge-Reitz** (1975), redécouverte sous le nom de **GGX** (Walter et ses collègues, 2007) : son reflet s'éteint plus doucement, avec un halo qui s'étale joliment, ce qui paraît bien plus naturel. Le studio Disney l'a adoptée en 2012, Unreal en 2013, et depuis c'est la référence dans tous les moteurs de jeu.
- **GGX anisotrope** : une variante avec deux réglages de rugosité au lieu d'un, pour des matières dont les rayures vont toutes dans le même sens, comme le métal brossé ou un disque rayé. Cela coûte un petit calcul de plus pour un effet superbe.

> **Que veut dire « anisotrope » ?** Cela veut dire « qui ne réagit pas pareil selon la direction ». Un métal brossé reflète la lumière en une traînée allongée parce que ses fines rayures sont toutes orientées dans le même sens : voilà de l'anisotropie.

Détail amusant : Trowbridge-Reitz et GGX sont en réalité **exactement la même formule**. L'équipe de 2007 a redécouvert sans le savoir la formule de 1975 et lui a donné un nouveau nom, et c'est pourquoi les deux appellations cohabitent encore aujourd'hui.

En pratique, l'artiste n'écrit aucune de ces formules. On lui demande seulement de peindre quatre réglages dans des images, et le moteur s'occupe du reste :

- **La couleur de base** (en anglais *albedo*) : la teinte de la matière, telle qu'elle serait sans aucune lumière dessus, donnée en rouge-vert-bleu.
- **Le côté métallique** (un nombre de $`0`$ à $`1`$) : $`0`$ pour les matières non métalliques comme le bois, la peau ou la peinture, $`1`$ pour un métal pur comme l'or ou le fer poli.
- **La rugosité** (un nombre de $`0`$ à $`1`$) : $`0`$ pour une surface lisse comme un miroir, $`1`$ pour une surface mate comme une craie.
- **La carte de relief** (en anglais *normal map*) : une image qui penche localement la normale pour faire croire à plein de petites bosses, sans réellement ajouter de matière.

> **Que veut dire « rouge-vert-bleu » (RGB) ?** Toute couleur d'écran se fabrique en mélangeant trois lumières de base : du rouge, du vert et du bleu, chacune plus ou moins forte. C'est la même idée que mélanger trois pots de peinture pour obtenir toutes les teintes.

> **Que veut dire « rugosité » ?** C'est le côté rugueux ou lisse d'une surface au toucher. Plus c'est rugueux, plus la lumière part dans tous les sens et plus l'objet paraît mat ; plus c'est lisse, plus il devient brillant comme un miroir.

> **Que veut dire « polygone » ?** Dans un jeu, chaque objet en trois dimensions est construit comme un origami : un assemblage de petits triangles plats, les polygones. Plus il y en a, plus l'objet est détaillé, mais plus il coûte cher à calculer.

Avec ces quatre réglages, on reproduit presque toutes les matières du monde réel : peau, métal, plastique, tissu, vitre, eau. C'est aussi ce qui permet de réutiliser le même travail partout : une matière peinte dans un logiciel se recharge presque telle quelle dans Unity, Unreal ou Blender.

#### Occlusion ambiante (*Ambient Occlusion*, AO)

Regardez le coin d'une pièce, le creux entre deux doigts ou le dessous d'un meuble : ces recoins sont toujours un peu plus sombres, parce que la lumière ambiante y arrive moins bien. L'**occlusion ambiante** (en anglais *Ambient Occlusion*) imite ce petit assombrissement à bas prix. Elle calcule pour chaque point un nombre $`A(\mathbf{x})`$ entre $`0`$ (recoin tout sombre) et $`1`$ (point bien dégagé) par lequel on multiplie l'éclairage :

```math
A(\mathbf{x}) = \frac{1}{\pi} \int_{\Omega} V(\mathbf{x}, \boldsymbol{\omega})\,(\boldsymbol{\omega} \cdot \mathbf{n})\,\mathrm{d}\boldsymbol{\omega}
```

En clair, on regarde tout autour du point, dans chaque direction du ciel, et on compte la part de ciel qui n'est pas bouchée par un obstacle. Le terme $`V`$ vaut $`1`$ quand la vue est dégagée dans cette direction et $`0`$ quand quelque chose la bloque.

> **Que veut dire « occlusion » ?** C'est le fait de boucher, de cacher. Une chose en occulte une autre quand elle se place devant et l'empêche d'être vue ou éclairée.

Calculer cela pour de vrai serait trop lent en plein jeu. On le devine donc à partir de l'image déjà dessinée, ce qu'on appelle travailler « à l'écran » :

- **SSAO** (2007) : pour chaque point de l'image, on jette un coup d'œil aux profondeurs des points voisins déjà calculés pour deviner s'il est dans un creux.
- **HBAO** (2008) : une version plus fine, qui mesure mieux les angles.
- **GTAO** (2016) : la meilleure aujourd'hui, presque impossible à distinguer d'un vrai calcul lent.

> **Que veut dire travailler « à l'écran » (screen-space) ?** Cela veut dire se servir uniquement de l'image plate déjà affichée, et de la profondeur enregistrée pour chaque point, au lieu de réexaminer toute la scène en trois dimensions. C'est beaucoup plus rapide, au prix de quelques approximations.

#### Harmoniques sphériques : l'éclairage ambiant compressé

> **Que veut dire « harmoniques sphériques » ?** Imaginez que vous vouliez décrire toute la lumière ambiante qui baigne un point, c'est-à-dire de quelle couleur et de quelle force le ciel l'éclaire dans chaque direction. C'est une information énorme. Les **harmoniques sphériques** sont une petite collection de motifs lumineux de base, comme une palette de quelques teintes simples, que l'on dose et que l'on additionne pour reconstituer cet éclairage tout autour. L'astuce : quelques dosages suffisent à retrouver une bonne approximation de la lumière, au lieu de tout retenir.

> **Que veut dire « projeter » ici, et un « coefficient » ?** Projeter, c'est mesurer combien de chaque motif de base est présent dans la lumière réelle, comme on dirait « il y a deux doses de bleu et une de jaune ». Chacune de ces doses est un **coefficient** : un simple nombre. Au lieu de garder l'image complète du ciel, on ne garde que cette poignée de nombres.

Mathématiquement, n'importe quelle fonction qui donne une valeur dans chaque direction de l'espace se réécrit comme une somme de ces motifs, chacun dosé par son coefficient :

```math
f(\theta, \varphi) = \sum_{\ell = 0}^{\infty} \sum_{m = -\ell}^{\ell} c_\ell^m\,Y_\ell^m(\theta, \varphi)
```

Les $`Y_\ell^m`$ sont les fameux motifs de base (les harmoniques sphériques) et les $`c_\ell^m`$ leurs doses. En théorie il en faudrait une infinité (c'est le sens du symbole $`\infty`$, l'infini, posé au sommet de la somme), mais dans un jeu on s'arrête très vite : on ne garde que les neuf premiers motifs. Cela fait **neuf nombres pour chaque couleur** rouge, vert et bleu, soit vingt-sept nombres en tout. Et c'est assez pour retrouver l'éclairage ambiant doux d'une scène avec une erreur d'à peine un pour cent.

> **Le symbole $`\infty`$.** C'est le signe de l'infini, « sans fin ». Posé au-dessus d'une somme, il signifie qu'on additionnerait une infinité de termes ; dans la pratique on s'arrête bien avant, ici au neuvième.

> **Que veut dire « tronquer » ?** C'est couper court, ne garder que le début. On tronque la somme infinie en n'en conservant que ses premiers termes, les plus importants, et on jette le reste, négligeable.

Pour calculer la lumière réellement reçue par une surface, il suffit alors de combiner ces vingt-sept nombres, une opération minuscule pour la carte graphique, au lieu d'aller relire une grosse image du ciel point par point :

```math
E(\mathbf{n}) \approx \sum_{\ell = 0}^{2} \sum_{m = -\ell}^{\ell} c_\ell^m\,Y_\ell^m(\mathbf{n})
```

> **Que veut dire « shader » ?** C'est un petit programme exécuté par la carte graphique pour décider de la couleur de chaque point de l'image. C'est lui qui applique l'éclairage, les ombres et les reflets, des millions de fois par image.

C'est exactement cette astuce de compression qui se cache derrière les sondes de lumière (*Light Probes*) d'Unity et les dispositifs équivalents d'Unreal : vingt-sept petits nombres remplacent toute la lumière d'ambiance d'un lieu.

#### Échantillonnage par importance : comment Monte-Carlo ne diverge pas

Comme l'intégrale de rendu additionne une infinité de directions, on ne peut pas toutes les visiter. On en pioche donc quelques-unes au hasard et on en fait la moyenne : c'est la méthode dite de **Monte-Carlo**. Le hic, c'est qu'elle est lente à se calmer : pour diviser le grain de l'image par deux, il faut quatre fois plus de tirages. D'où des milliers de tirages par point pour une image propre. La grande astuce pour aller plus vite s'appelle l'**échantillonnage par importance** : au lieu de tirer les directions complètement au hasard, on vise surtout celles qui apportent le plus de lumière.

> **Que veut dire « Monte-Carlo » ?** C'est une méthode qui estime une quantité en tirant beaucoup d'exemples au hasard, puis en faisant la moyenne, comme on devinerait le nombre de bonbons rouges dans un grand bocal en en piochant quelques poignées. Le nom vient du célèbre casino de Monte-Carlo, clin d'œil au hasard.

> **Que veut dire « échantillon » ?** C'est un seul exemple tiré au hasard parmi tous les possibles, comme une cuillerée goûtée dans une marmite. Plus on goûte de cuillerées, plus on connaît le goût de la soupe entière.

> **Que veut dire « variance » et « grain » ?** La variance, c'est l'ampleur des écarts entre les tirages : grande variance, les résultats sautent dans tous les sens et l'image fourmille de petits points clairs et sombres, le **grain**. Viser les directions utiles réduit ces écarts, donc le grain.

```math
L_o \approx \frac{1}{N} \sum_{k=1}^{N} \frac{f_r(\boldsymbol{\omega}_i^{(k)}, \boldsymbol{\omega}_o)\,L_i(\boldsymbol{\omega}_i^{(k)})\,(\boldsymbol{\omega}_i^{(k)} \cdot \mathbf{n})}{p(\boldsymbol{\omega}_i^{(k)})}
```

Dans la formule, on divise chaque tirage par sa probabilité $`p`$ d'avoir été choisi, ce qui corrige le fait d'avoir visé certaines directions plus souvent que d'autres. Tout l'art consiste à bien choisir cette façon de viser. Trois recettes reviennent souvent :

> **Que veut dire « viser au prorata » de la lumière ?** Cela veut dire « tirer plus souvent là où il y a le plus à gagner ». Si on cherche de l'or, on creuse surtout là où la rivière brille, pas n'importe où au hasard. Viser uniformément, à l'inverse, ce serait creuser partout avec la même chance.

- **Viser vers le haut de la surface** : on tire plus souvent les directions proches de la verticale, là où une surface mate reçoit et renvoie le plus de lumière. C'est le réglage idéal pour les matières diffuses.
- **Viser le reflet** : pour une surface brillante, on tire surtout dans la direction du reflet. Sur un objet presque miroir, tirer au hasard raterait presque toujours le minuscule point brillant ; en visant droit dessus, on l'attrape du premier coup et l'image devient nette beaucoup plus vite.
- **Combiner deux visées** : on mélange intelligemment la visée vers les reflets et la visée vers les sources de lumière, pour bien gérer à la fois les surfaces brillantes et les grandes lampes. Cela évite les **lucioles**, ces points blancs isolés qui clignotent sur une image mal calculée.

> **Pourquoi les techniques modernes nettoient-elles l'image si vite ?** Les jeux à tracé de rayons (comme le mode chemin de lumière de *Cyberpunk* ou *Quake II RTX*) ne calculent qu'un ou deux tirages par point, ce qui donne une image très grenue, puis confient le nettoyage à un réseau de neurones. Or ce nettoyeur ne réussit que parce que les rares tirages visaient déjà les directions utiles : c'est la rencontre des deux astuces, le bon ciblage et le nettoyage automatique, qui rend ces images possibles en temps réel.

#### Anti-aliasing : la guerre contre l'escalier

Regardez le bord penché d'un toit dessiné sur un écran : au lieu d'une ligne nette, vous voyez un petit escalier de pixels. Ce défaut s'appelle l'**aliasing**, ou **crénelage** en français. Il survient quand on essaie de représenter un détail fin avec trop peu de points pour le capturer correctement. Une règle mathématique, le **critère de Nyquist**, le dit précisément : pour saisir un détail sans le déformer, il faut au moins deux points de mesure par variation du détail. En dessous, l'image fourmille de marches d'escalier, de motifs parasites et de scintillements quand on bouge. Quatre familles d'outils luttent contre cela.

> **Que veut dire « critère de Nyquist » ?** C'est une loi qui fixe le nombre minimum de mesures nécessaires pour saisir fidèlement quelque chose qui varie. Pour suivre une roue qui tourne en photo, il faut la photographier assez souvent ; trop peu, et elle semble tourner à l'envers. Pareil pour les détails d'une image : trop peu de points, et le bord se transforme en escalier.

- **SSAA**, la force brute. On dessine toute l'image bien plus grande que nécessaire, puis on la réduit en faisant la moyenne des points : les escaliers se fondent en bords doux. C'est la meilleure qualité possible, mais multiplier la taille coûte tellement cher qu'on ne s'en sert presque jamais en jeu, sauf pour une belle capture d'écran.
- **MSAA**, une version maligne. Au lieu de tout dessiner en plus grand, on ne calcule la couleur qu'une fois par point, mais on note plus finement, sur les bords des objets, quelle part de chaque point est réellement couverte par l'objet. Très efficace sur le contour des objets, mais sans effet sur les détails des textures ni sur les petits reflets brillants. Et il s'accommode mal des moteurs modernes qui calculent l'éclairage en deux temps, car il faudrait alors garder beaucoup trop d'informations en mémoire.

```math
C_\text{pixel} = \frac{1}{k} \sum_{j=1}^{k} \mathbb{1}[\text{sample}_j \text{ couvert}] \cdot C_\text{shader}
```

> **Le symbole $`\mathbb{1}[\dots]`$.** C'est un interrupteur : il vaut $`1`$ quand ce qui est écrit entre les crochets est vrai, et $`0`$ sinon. Ici, il vaut $`1`$ si le petit point de mesure tombe bien sur l'objet, $`0`$ s'il tombe à côté. La formule fait alors simplement la moyenne des points couverts pour adoucir le bord.

- **FXAA**, un coup de pinceau final. Une fois l'image entièrement dessinée, ce procédé repère les contours en cherchant où la luminosité change brusquement, puis les floute légèrement dans le bon sens. C'est très rapide et bon marché, le résultat est un peu flou, mais il gomme tous les types d'escaliers, textures comprises.
- **TAA**, le standard d'aujourd'hui. Il mêle l'image actuelle aux images précédentes, en se servant des vecteurs de mouvement pour savoir où chaque point se trouvait juste avant. À chaque image, il ajoute un point de mesure légèrement décalé, et l'accumulation finit par lisser les bords. C'est la même formule de recombinaison dans le temps que celle des techniques d'agrandissement plus bas :

> **Que veut dire « image » (frame) et « vecteur de mouvement » ?** Un jeu redessine l'écran des dizaines de fois par seconde ; chacune de ces images successives est une **frame**, comme une photo dans une pellicule de cinéma. Le **vecteur de mouvement** est une petite flèche, calculée pour chaque point, qui indique où il se trouvait sur la frame précédente : elle permet de retrouver le même morceau de décor d'une image à l'autre, même quand la caméra bouge.

> **Comment lire cette formule ?** $`C_t(\mathbf{p})`$ est la couleur finale du point $`\mathbf{p}`$ à l'instant présent. On la fabrique en mélangeant une petite part de l'image fraîchement calculée et une grande part de la couleur retenue de l'image d'avant, retrouvée grâce à la flèche de mouvement. Le réglage $`\alpha`$ (la lettre grecque « alpha ») fixe le dosage : à $`0{,}1`$, on garde neuf dixièmes du passé et un dixième de neuf.

Avec ce faible dosage, un point immobile cumule en une dizaine d'images une dizaine de mesures décalées : on atteint presque la qualité de la force brute, pour le prix d'une seule image. Le défaut bien connu de cette méthode est la **traînée fantôme** : derrière un objet qui se déplace, une trace de son ancienne couleur peut rester collée. Pour l'effacer, on surveille les couleurs du petit voisinage de chaque point dans l'image fraîche, et on force la couleur héritée du passé à rester dans ces limites ; si elle en sort, c'est que la scène a vraiment changé là, donc on la jette et le fantôme disparaît. Les versions modernes confient ce mélange à un réseau de neurones, parfois en agrandissant l'image au passage.

#### Tessellation et displacement mapping

> **Que veut dire « tessellation » ?** C'est l'action de découper chaque gros triangle d'un objet en une foule de petits triangles, mais seulement quand on s'approche de l'objet, là où l'œil réclame du détail. De loin, on garde peu de triangles pour aller vite.

> **Que veut dire « displacement » (déplacement de relief) ?** Une fois les petits triangles créés, on soulève chacun de leurs sommets d'une hauteur lue dans une carte d'altitude. On sculpte ainsi un vrai relief en trois dimensions, contrairement à une simple carte de relief qui se contente de tricher sur la lumière sans changer la forme.

> **Que veut dire « maillage » ?** C'est le squelette d'un objet 3D : l'ensemble de ses points reliés en triangles, comme le grillage d'une volière qui dessine la forme avant qu'on la recouvre.

Voyons comment on place mathématiquement les nouveaux sommets. À l'intérieur d'un triangle, on repère un point par trois nombres $`(u, v, w)`$ qui disent à quel point on penche vers chacun des trois coins, et qui s'additionnent toujours à $`1`$. Avec ces trois poids, on calcule la position du sommet, puis on le pousse vers l'extérieur d'une hauteur $`h`$ :

```math
P(u, v, w) = u\,V_0 + v\,V_1 + w\,V_2 + h(u, v, w)\,\mathbf{n}(u, v, w)
```

La hauteur $`h`$ est lue dans la carte d'altitude et $`\mathbf{n}`$ est la normale, la flèche qui indique le dehors. Ces trois poids $`(u, v, w)`$ portent un nom, les **coordonnées barycentriques** : c'est la façon habituelle de désigner un point à l'intérieur d'un triangle par sa proximité à chaque coin.

> **Que veut dire « coordonnées barycentriques » ?** C'est une recette de mélange pour situer un point dans un triangle, comme un dosage de trois couleurs. Tout au coin du haut, le dosage est « une part en haut, zéro ailleurs » ; au centre, c'est « un tiers, un tiers, un tiers ». Les trois parts s'additionnent toujours à un tout.

Mises ensemble, ces deux techniques sculptent un terrain ou un mur qui paraît taillé au burin, sans avoir à stocker en mémoire un objet aussi détaillé en permanence. Les cartes graphiques récentes possèdent des étages spécialisés pour faire ce découpage à la volée.

> **Que veut dire « carte graphique » (GPU) ?** C'est le composant de l'ordinateur spécialisé dans le dessin des images. Là où le processeur principal fait un peu de tout, la carte graphique excelle à répéter le même petit calcul sur des millions de points à la fois, ce qui est exactement ce qu'exige l'affichage d'un jeu.

#### Ray tracing : le tracé de rayons

> **L'idée du tracé de rayons.** Plutôt que d'aplatir les objets à l'écran puis de les éclairer après coup, on imagine partir de l'œil et envoyer un rayon droit à travers chaque point de l'écran, comme une canne à pêche lancée dans la scène. On regarde alors le premier objet que le rayon touche, et on calcule sa couleur à cet endroit. C'est l'inverse du trajet réel de la lumière, qui part des lampes et non de l'œil ; mais comme la lumière se comporte pareil dans un sens et dans l'autre, le résultat est identique. C'est d'ailleurs ainsi que dessinaient déjà les peintres de la Renaissance avec leurs fils tendus.

Le rayon s'écrit $`R(t) = \mathbf{O} + t\,\mathbf{D}`$. C'est juste une façon de dire « je pars du point de départ $`\mathbf{O}`$ (l'œil) et j'avance dans la direction $`\mathbf{D}`$ ». Le nombre $`t`$ mesure la distance parcourue : plus $`t`$ grandit, plus on s'éloigne le long du rayon. Pour savoir si ce rayon touche une boule, on cherche la valeur de $`t`$ qui place le point pile sur la surface de la boule, ce qui donne une équation du second degré (une équation avec un terme au carré) :

```math
\|t\,\mathbf{D} + (\mathbf{O} - \mathbf{C})\|^2 = r^2
\;\Rightarrow\;
t^2(\mathbf{D}\cdot\mathbf{D}) + 2t\,\mathbf{D}\cdot(\mathbf{O}-\mathbf{C}) + \|\mathbf{O}-\mathbf{C}\|^2 - r^2 = 0
```

Les objets des jeux ne sont pourtant pas faits de boules, mais de triangles. Pour savoir si un rayon touche un triangle, on emploie une recette rapide et célèbre, l'algorithme de **Möller-Trumbore** (1997). Elle trouve d'un coup où le rayon perce le triangle et à quelle distance, sans détour inutile. On part du rayon et du triangle de coins $`V_0, V_1, V_2`$, et on note $`\mathbf{e}_1`$ et $`\mathbf{e}_2`$ les deux flèches qui longent deux côtés du triangle depuis le coin $`V_0`$. On cherche alors les trois inconnues $`t`$ (la distance) et $`(u, v)`$ (la position dans le triangle) :

```math
\mathbf{O} + t\,\mathbf{D} = V_0 + u\,\mathbf{e}_1 + v\,\mathbf{e}_2 \quad\Leftrightarrow\quad
\begin{pmatrix} -\mathbf{D} & \mathbf{e}_1 & \mathbf{e}_2 \end{pmatrix}\begin{pmatrix} t \\ u \\ v \end{pmatrix} = \mathbf{O} - V_0
```

> **Que veut dire « matrice » ?** C'est un tableau de nombres rangés en lignes et en colonnes. Le grand tableau entre parenthèses dans la formule ci-dessus est une matrice ; elle sert ici à poser les trois inconnues en une seule équation bien rangée.

Pour résoudre ce tableau, on dispose de deux outils. Le **produit vectoriel**, noté $`\times`$, qui à partir de deux flèches en fabrique une troisième perpendiculaire aux deux premières, comme le pouce qui se dresse quand l'index et le majeur pointent dans deux directions. Et le **déterminant**, noté $`\det`$, un nombre qui mesure le volume tordu par un tableau et qui vaut zéro pile quand l'équation n'a pas de solution unique. En les combinant et en posant trois flèches d'aide $`\mathbf{p} = \mathbf{D} \times \mathbf{e}_2`$, $`\mathbf{T} = \mathbf{O} - V_0`$ et $`\mathbf{q} = \mathbf{T} \times \mathbf{e}_1`$, on obtient directement les réponses :

> **Que veut dire « produit vectoriel » ?** C'est une opération entre deux flèches qui rend une nouvelle flèche perpendiculaire au plan des deux premières. Sa longueur dit l'aire qu'elles dessinent, et sa direction dit de quel côté ce plan est tourné. C'est l'outil parfait pour fabriquer une normale.

> **Que veut dire « déterminant » ?** C'est un nombre calculé à partir d'un tableau carré de nombres. S'il vaut zéro, c'est le signal que l'équation est mal posée (par exemple un rayon qui glisse parallèlement au triangle sans jamais le percer) ; sinon, la solution existe et est unique.

```math
t = \frac{\mathbf{q} \cdot \mathbf{e}_2}{\mathbf{p} \cdot \mathbf{e}_1}, \qquad u = \frac{\mathbf{p} \cdot \mathbf{T}}{\mathbf{p} \cdot \mathbf{e}_1}, \qquad v = \frac{\mathbf{q} \cdot \mathbf{D}}{\mathbf{p} \cdot \mathbf{e}_1}
```

On sait alors que le triangle est touché lorsque $`u`$ et $`v`$ sont positifs et que leur somme ne dépasse pas $`1`$ (le point reste bien à l'intérieur du triangle) et que la distance $`t`$ est positive (le triangle est devant l'œil, pas derrière). Tout ce test ne demande qu'une trentaine d'opérations, autant dire trois fois rien pour un ordinateur. Les cartes graphiques récentes possèdent même de minuscules circuits gravés exprès pour ce calcul, qui le font à toute vitesse.

Reste un problème de taille : une scène peut contenir des millions de triangles, et tester un rayon contre chacun serait ruineux. On range donc les triangles dans un classement astucieux qui évite la quasi-totalité des tests inutiles.

> **Que veut dire « BVH » ?** C'est un rangement en poupées russes de boîtes. On enferme tout le décor dans une grande boîte, qui contient des boîtes plus petites, qui en contiennent d'autres, jusqu'aux triangles. Pour savoir ce qu'un rayon touche, on regarde d'abord s'il entre dans la grande boîte ; si oui, on n'ouvre que les sous-boîtes traversées, et ainsi de suite. On élimine d'un coup d'immenses parties du décor sans les examiner en détail, ce qui transforme des millions de tests en une poignée.

> **Que veut dire « boîte englobante » (AABB) ?** C'est la plus petite boîte rectangulaire, aux faces bien droites alignées sur les axes, qui contient entièrement un objet. Elle est très rapide à tester : si le rayon rate la boîte, il rate forcément l'objet à l'intérieur, et on s'arrête là.

> **Les rangements de l'espace, pour aller plus vite.** Dès qu'il y a beaucoup d'objets à tester (pour les collisions, le tracé de rayons, ou pour ne pas dessiner ce qui est hors de l'écran), on les classe dans une structure en arbre, comme un grand dossier qui contient des sous-dossiers. Les principales :
>
> - **BVH** : le rangement en boîtes emboîtées décrit juste au-dessus. C'est la référence pour le tracé de rayons en temps réel.
> - **Octree** : un rangement où l'on coupe chaque cube en huit petits cubes égaux, encore et encore. Parfait pour quadriller l'espace de façon régulière. En deux dimensions, son cousin coupe chaque carré en quatre.
> - **KD-tree** : un rangement qui coupe l'espace en deux à chaque étape, tantôt en largeur, tantôt en hauteur, tantôt en profondeur. Très efficace pour retrouver l'objet le plus proche d'un point.
> - **BSP** : un rangement qui coupe l'espace par un plan placé librement, dans n'importe quel sens. Inventé pour le jeu Doom en 1993, il permettait à l'ordinateur de l'époque d'afficher les murs dans le bon ordre, du plus lointain au plus proche, très rapidement. On s'en sert encore pour fabriquer des niveaux.

> **Que veut dire « arbre » en informatique ?** C'est une façon de ranger des éléments par embranchements successifs, comme un arbre généalogique : une racine en haut, qui se sépare en branches, qui se séparent encore, jusqu'aux feuilles. Suivre une seule branche au lieu de tout regarder fait gagner énormément de temps.

#### Path tracing : la généralisation

Dans la vraie vie, la lumière ne touche pas une seule surface : elle ricoche partout. Un mur rouge teinte légèrement de rose le plafond blanc d'à côté. Le **tracé de chemins** (path tracing) imite ce va-et-vient : au lieu de s'arrêter au premier objet touché, le rayon **rebondit**, encore et encore, en repartant à chaque fois dans une nouvelle direction, jusqu'à tomber sur une lampe. On répète l'opération un grand nombre de fois et on en fait la moyenne, à la manière de Monte-Carlo, pour calculer la couleur finale :

```math
L_o \approx \frac{1}{N} \sum_{k=1}^{N} \frac{f_r(\boldsymbol{\omega}_i^{(k)}, \boldsymbol{\omega}_o)\,L_i(\boldsymbol{\omega}_i^{(k)})\,(\boldsymbol{\omega}_i^{(k)} \cdot \mathbf{n})}{p(\boldsymbol{\omega}_i^{(k)})}
```

Le terme $`p`$ est la chance qu'avait chaque direction d'être tirée, et le nombre $`N`$ compte les rebonds tentés : plus $`N`$ est grand, plus l'image est lisse. Des jeux comme *Quake II RTX*, le mode chemin de lumière de *Cyberpunk 2077* ou *Portal RTX* font cela en temps réel. Pour y arriver, ils ne calculent qu'un ou deux rebonds par point, ce qui donne une image très grenue, puis ils confient le nettoyage final à un réseau de neurones entraîné à deviner l'image propre à partir de l'image grenue.

> **Que veut dire « réseau de neurones » ?** C'est un programme inspiré du cerveau, fait d'une multitude de petites unités de calcul reliées entre elles. On ne lui dicte pas de règles : on l'entraîne en lui montrant des milliers d'exemples (ici, des images grenues et leur version propre), et il apprend petit à petit à faire le travail tout seul.

#### Global illumination

Toute la lumière qui a ricoché au moins une fois avant d'arriver à l'œil forme ce qu'on appelle l'**illumination globale** : c'est elle qui donne cette ambiance chaude et réaliste, faite de reflets colorés et de douces lueurs indirectes. La calculer en entier coûte cher, alors en temps réel on la devine par plusieurs astuces.

> **Que veut dire « illumination globale » ?** C'est l'éclairage complet d'une scène, en tenant compte non seulement de la lumière qui vient directement des lampes, mais aussi de toute la lumière qui a rebondi sur les murs, le sol et les objets avant d'arriver. C'est la différence entre une pièce sans vie et une pièce baignée d'une lumière naturelle.

- **Éclairage cuit à l'avance** : pour les décors qui ne bougent jamais, on calcule l'éclairage une fois pour toutes, à la fabrication du jeu, et on le peint directement dans les textures. Cela ne coûte plus rien à jouer, mais ne marche pas sur les objets qui bougent.
- **Suivi de cônes dans une grille de cubes** : on remplit la scène de petits cubes (les *voxels*, comme un univers façon Minecraft très fin) et on y répand la lumière. Pour mesurer l'éclairage d'un point, on lance non pas un rayon fin mais un cône qui s'évase, plus rapide à calculer. C'est approximatif, mais bien plus véloce qu'un vrai tracé de rayons.
- **Sondes de lumière** : on mesure l'éclairage en quelques points bien choisis de la scène, on le range sous forme de ces fameux vingt-sept petits nombres vus plus haut, et chaque point de l'image mélange les sondes les plus proches. Certaines versions remettent ces mesures à jour en continu pour suivre une lumière qui change.
- **Lumen** (Unreal Engine 5) : une recette hybride qui combine plusieurs de ces astuces à la fois.

> **Que veut dire « voxel » ?** C'est l'équivalent en trois dimensions d'un pixel : un petit cube de matière. Empiler des voxels, c'est construire un monde comme avec des cubes de jeu, à la manière de Minecraft.

#### Upscaling temporel : DLSS, FSR, XeSS

Dessiner une image en très haute définition coûte beaucoup plus cher qu'une image ordinaire : quatre fois plus de points à calculer pour passer de la définition courante à la très grande. L'astuce consiste à dessiner l'image en plus petit, ce qui est rapide, puis à l'**agrandir** intelligemment en s'aidant des images précédentes. Voilà le métier des techniques d'agrandissement.

> **Que veut dire « définition » d'une image (4K, 1080p) ?** C'est le nombre de points qui composent l'image : plus il est grand, plus l'image est fine et détaillée. La très haute définition, dite 4K, contient quatre fois plus de points que la définition courante, dite 1080p, donc quatre fois plus de calculs.

- **DLSS** (NVIDIA) : un réseau de neurones entraîné à l'avance sur de très belles images de référence, qui a appris à recréer les détails manquants quand on lui présente une petite image.
- **FSR** (AMD) : une recette faite de règles de calcul plutôt que d'apprentissage, qui a l'avantage de fonctionner sur n'importe quelle carte graphique.
- **XeSS** (Intel) : une approche par réseau de neurones proche de DLSS, conçue pour tourner sur le plus de matériels possible.
- **Génération d'images** : ici, on ne se contente pas d'agrandir, on **invente** une image entière intercalée entre deux images calculées, à partir des vecteurs de mouvement, pour que l'animation paraisse deux fois plus fluide.

Sous le capot, toutes reposent sur la même formule de recombinaison dans le temps déjà rencontrée plus haut :

```math
C_t(\mathbf{p}) = \alpha\,C_t^\text{rendu}(\mathbf{p}) + (1 - \alpha)\,C_{t-1}(\mathbf{p} + \mathbf{v})
```

La flèche $`\mathbf{v}`$ est de nouveau le vecteur de mouvement, qui indique où se trouvait le point sur l'image précédente, et le dosage $`\alpha`$ règle la part de neuf et de passé, en gardant juste ce qu'il faut d'ancien pour lisser sans laisser de traînée.

[ Retour en haut de page](#table-des-matières)

---

---

[← Réseau et multijoueur](09-reseau-et-multijoueur.md) · [↑ Sommaire](../README.md#table-des-matières) · [Pipeline de rendu →](11-pipeline-de-rendu.md)
