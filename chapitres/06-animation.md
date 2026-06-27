[← Texture et mappage UV](05-texture-et-mappage-uv.md) · [↑ Sommaire](../README.md#table-des-matières) · [Physique des jeux →](07-physique-des-jeux.md)

# 6. Animation

Un écran ne sait pas bouger. Il affiche une image fixe, puis une autre, puis une autre, très vite, et c'est notre œil qui recolle ces images figées en un mouvement continu, exactement comme un dessin animé feuilleté au coin des pages d'un cahier. Animer, c'est donc fabriquer la bonne image fixe à chaque instant. Tout le travail consiste à calculer, pour chaque vingtième ou soixantième de seconde, où se trouvent les bras, les jambes ou le visage d'un personnage.

> **Que veut dire « infographie » ?** C'est le mot savant pour « faire des images avec un ordinateur ». « Info » vient d'informatique (l'ordinateur), « graphie » vient de dessin. L'infographie, c'est l'art de dessiner avec une machine.

> **Que veut dire « 3D » ?** Un objet en **3D** (trois dimensions) a une largeur, une hauteur et une profondeur, comme un vrai jouet que l'on peut tourner dans la main et regarder de dos. Une image normale, plate comme une feuille de papier, est en **2D** (deux dimensions) : juste largeur et hauteur. Dans un jeu vidéo, le décor et les personnages sont des objets 3D que l'ordinateur regarde sous un certain angle pour en faire l'image plate affichée à l'écran.

Trois grandes recettes permettent de mettre tout cela en mouvement : l'**animation par squelette** (on bouge un pantin), l'**animation de forme** (on déforme doucement l'objet d'une posture à une autre) et la **cinématique inverse** (on dit où doit aller la main, et l'ordinateur retrouve tout seul comment plier le bras). Ce chapitre les explique l'une après l'autre.

### Animation par squelette

Pensez à une marionnette. À l'intérieur, des baguettes et des ficelles forment un squelette ; à l'extérieur, le tissu épouse ces baguettes. Quand le marionnettiste lève une baguette, le tissu se soulève avec elle. L'**animation par squelette** fait exactement pareil dans l'ordinateur : on cache un squelette à l'intérieur du personnage, et la peau visible suit les os quand on les bouge.

> **Que veut dire « animation par squelette » ?** C'est animer un personnage en lui mettant un squelette invisible (un assemblage d'os) et en bougeant ces os. La peau, les vêtements et les cheveux, collés aux os, se déplacent automatiquement. On l'appelle aussi animation par armature, et l'opération qui consiste à installer ce squelette dans le modèle porte un nom anglais courant, le *rigging*.

Chaque os est responsable d'un bout du personnage : l'os du bras commande la chair du bras, l'os de la cuisse commande la chair de la cuisse. Quand un os tourne ou se déplace, le morceau de peau qui lui est rattaché se déforme avec lui. C'est la méthode reine pour animer les **personnages** et les **créatures**, aussi bien dans les jeux vidéo que dans les films d'animation, parce qu'elle imite la façon dont notre propre squelette fait bouger notre corps.

Le squelette est un ensemble d'**os** (on dit aussi des **nœuds** ou des **articulations**, en anglais *joints*) accrochés les uns aux autres par des **liaisons rigides**.

> **Que veut dire « liaison rigide » ?** C'est une attache qui ne s'étire pas et ne se casse pas : la distance entre deux os reliés reste toujours la même, comme deux maillons d'une chaîne. L'avant-bras peut pivoter autour du coude, mais il ne peut pas s'éloigner du coude. C'est ce qui empêche un personnage de se transformer en bonhomme élastique.

Chaque os connaît sa **position** (où il se trouve dans l'espace) et son **orientation** (vers où il pointe et comment il est incliné). Ces deux informations sont rangées ensemble dans un petit tableau de nombres appelé matrice de transformation.

> **Que veut dire « matrice de transformation 4×4 » ?** Une **matrice** est juste un tableau de nombres rangés en lignes et en colonnes, comme une grille de mots croisés remplie de chiffres. Celle-ci fait 4 cases de large sur 4 cases de haut (d'où « 4×4 »). Une matrice « de transformation » est une grille spéciale : en la faisant rencontrer un point, elle déplace ce point, le fait tourner ou l'agrandit. C'est une machine à transformer rangée sous forme de tableau de nombres. (Les matrices sont expliquées en détail dans le chapitre sur les bases des mathématiques.)

Un os ne vit pas tout seul : il est suspendu à son **parent**, l'os juste au-dessus de lui dans le squelette (la main pend au bout de l'avant-bras, qui pend lui-même au bout du bras). Du coup, sa position dans le monde dépend de deux choses : où se trouve déjà son parent, et de combien lui-même tourne par rapport à ce parent. On combine ces deux informations en multipliant leurs matrices :

```math
T_\text{global} = T_\text{parent} \cdot T_\text{local}
```

> **Que veulent dire ces symboles ?** $`T_\text{local}`$ (lire « T local ») décrit le petit mouvement de l'os par rapport à son parent : par exemple « la main tourne de 30 degrés au bout de l'avant-bras ». $`T_\text{parent}`$ (« T parent ») décrit où le parent se trouve déjà dans le monde entier. $`T_\text{global}`$ (« T global ») est le résultat : où se trouve finalement notre os dans le monde, une fois additionnés tous les mouvements des os au-dessus de lui. Le point « $`\cdot`$ » entre les deux veut dire qu'on **multiplie** les deux matrices, ce qui revient à **enchaîner** les deux mouvements (« va où est le parent, PUIS tourne un peu en plus »).

> **Pourquoi multiplier dans cet ordre ?** Parce que bouger un os entraîne tous ses enfants avec lui, exactement comme lever l'épaule entraîne le coude, la main et chaque doigt. Pour savoir où finit la main, il faut donc d'abord savoir où l'épaule a emmené le bras (le parent), et seulement ensuite ajouter le pli propre de la main (le local). On part de la racine du squelette et on descend de parent en enfant, en multipliant à chaque étage : c'est ce qu'on appelle parcourir la hiérarchie des os.

> **Que veulent dire « racine » et « hiérarchie » ?** Les os forment un arbre généalogique : tout part d'un os de départ, la **racine** (souvent le bassin), d'où descendent le tronc, puis les bras, puis les avant-bras, et ainsi de suite jusqu'aux doigts. Cette organisation en familles d'os, du plus haut au plus bas, s'appelle la **hiérarchie**. Bouger un os du haut emporte automatiquement toute sa descendance, comme secouer une branche fait bouger toutes les feuilles qui en dépendent.

Animer le squelette, c'est tout simplement changer les matrices locales $`T_\text{local}`$ au fil du temps : à chaque image, on plie un peu plus le coude, on tourne un peu plus la tête, et le mouvement apparaît.

### Animation de forme

Le squelette est parfait pour des membres rigides qui pivotent, mais il devient maladroit pour une bouche qui sourit ou un sourcil qui se fronce : la peau du visage se déforme dans tous les sens, sans os bien net pour la commander. Pour ces cas-là, on utilise une autre recette. On sculpte d'avance plusieurs versions complètes de l'objet (un visage neutre, le même visage souriant, le même visage surpris) et on glisse en douceur de l'une à l'autre, comme un visage qui se métamorphose lentement dans un dessin animé.

> **Que veut dire « animation de forme » ?** C'est animer en mélangeant progressivement plusieurs formes toutes prêtes du même objet. On part d'une forme de départ et on se rapproche petit à petit d'une forme d'arrivée, jusqu'à devenir cette dernière. Le mot anglais pour cette transformation continue est le *morphing* (de l'idée de « métamorphose »).

C'est l'outil idéal pour ce qui change de forme de façon compliquée, comme les **visages** (sourires, clignements d'yeux) ou les **vêtements** qui ondulent.

Comment passe-t-on en douceur d'une forme à l'autre ? Grâce à l'**interpolation linéaire**.

> **Que veut dire « interpolation linéaire » ?** « Interpoler », c'est trouver les valeurs intermédiaires entre deux extrêmes. « Linéaire » veut dire qu'on avance à vitesse constante, en ligne droite, sans accélérer ni ralentir. Mélanger deux couleurs de peinture à parts égales pour obtenir la teinte du milieu, c'est exactement cela : à mi-chemin, on a moitié de l'une et moitié de l'autre.

> **Que veut dire « sommet » ?** Un objet 3D est en réalité un grillage de petits points reliés par des facettes, comme la résille d'un ballon de foot. Chaque point de ce grillage s'appelle un **sommet**. Un visage 3D peut en compter des milliers. Déformer la forme, c'est déplacer ces sommets.

L'astuce est que la forme de départ et la forme d'arrivée possèdent **exactement les mêmes sommets**, au même nombre et dans le même ordre ; seules leurs positions diffèrent. Pour fabriquer une forme intermédiaire, il suffit donc de calculer, pour chaque sommet, un point situé quelque part entre sa position de départ et sa position d'arrivée. Pour mélanger deux formes $`A`$ et $`B`$ avec un dosage $`t`$ (un nombre entre $`0`$ et $`1`$), on applique sommet par sommet :

```math
P_\text{interpolated} = (1 - t) \times P_A + t \times P_B
```

> **Que veulent dire ces symboles ?** $`P_A`$ est la position d'un sommet dans la forme de départ $`A`$, $`P_B`$ la position du **même** sommet dans la forme d'arrivée $`B`$, et $`P_\text{interpolated}`$ (« P interpolé ») la position mélangée que l'on cherche. $`t`$ est le **dosage**, ou facteur d'interpolation : c'est un curseur qui glisse de $`0`$ à $`1`$. Le signe $`\leq`$ se lit « inférieur ou égal à » (la pointe désigne le plus petit, le trait dessous autorise l'égalité), donc l'écriture $`0 \leq t \leq 1`$ veut simplement dire « $`t`$ reste compris entre $`0`$ et $`1`$, bornes incluses ».

> **Pourquoi cette formule donne-t-elle le bon mélange ?** Lisez-la comme une recette à deux ingrédients dont les doses font toujours un total de 1 (soit 100 %). La part de la forme $`A`$ vaut $`(1 - t)`$ et la part de la forme $`B`$ vaut $`t`$. Si le curseur est à $`t = 0`$, on prend $`1`$ de $`A`$ et $`0`$ de $`B`$ : on obtient $`A`$ pile. À $`t = 1`$, c'est l'inverse : on obtient $`B`$. À $`t = 0{,}5`$, on prend une moitié de chaque : on est pile au milieu. En faisant grandir $`t`$ de $`0`$ vers $`1`$ image après image, le sommet glisse en douceur de sa position de départ à sa position d'arrivée, et le visage passe doucement du neutre au sourire.

### Cinématique inverse

Imaginez votre bras. D'habitude, pour attraper un verre, votre cerveau ne calcule pas un par un les angles de l'épaule, du coude et du poignet : il pense simplement « ma main doit aller là-bas », et le bras se plie tout seul comme il faut. La **cinématique inverse** apprend cette même magie à l'ordinateur. On lui donne seulement l'endroit où la main doit arriver, et il retrouve à l'envers de combien chaque articulation doit tourner pour y parvenir.

> **Que veut dire « cinématique inverse » ?** La « cinématique », c'est l'étude du mouvement. Normalement (cinématique « directe »), on choisit les angles des articulations et on en déduit où finit la main : on plie le coude de tant, l'épaule de tant, et on regarde où aboutit la main. La cinématique **inverse** fait le chemin en sens contraire : on fixe d'abord où la main doit aller, et on cherche les angles qui produisent ce résultat. On la note souvent par ses initiales anglaises, **IK** (pour *Inverse Kinematics*).

> **Que veut dire « effecteur » ?** C'est le bout de la chaîne d'os que l'on veut amener à un endroit précis, en général la **main** ou le **pied** du personnage. C'est la partie qui « agit » sur le monde : la main qui saisit, le pied qui se pose. Tout le reste du bras ou de la jambe n'est là que pour amener correctement cet effecteur à destination.

C'est très utile dès que le décor n'est pas connu d'avance. Quand un personnage saisit un objet posé n'importe où, ou pose le pied sur un terrain bosselé sans que la semelle traverse le sol ou flotte dans le vide, l'IK ajuste les articulations en temps réel pour que main et pied tombent pile au bon endroit.

Le problème, c'est qu'il n'existe pas de formule directe donnant les angles d'un coup. On doit résoudre un **système d'équations non linéaires** qui relie les angles des articulations à la position finale de l'effecteur.

> **Que veut dire « système d'équations non linéaires » ?** Une **équation** est une devinette à trous : « quel nombre rend cette phrase vraie ? ». Un **système** d'équations, c'est plusieurs de ces devinettes à résoudre en même temps, avec une réponse qui les satisfait toutes. « Non linéaire » signifie que la réponse n'est pas proportionnelle aux réglages : doubler un angle ne double pas le déplacement de la main, parce que tourner un bras le fait voyager le long d'un cercle, pas d'une ligne droite. Ces devinettes-là n'ont pas de solution exacte facile ; on les résout donc par essais successifs, en s'approchant de la bonne réponse petit à petit.

Comme on ne sait pas calculer la réponse d'un coup, on la cherche par tâtonnements organisés. Trois grandes familles de méthodes existent.

#### Méthodes basées sur la Jacobienne

La première famille de méthodes raisonne par tout petits pas. L'idée est la suivante : si la main rate sa cible, regardons dans quelle direction la pousse chaque articulation quand on la tourne d'un poil, puis combinons ces directions pour viser un peu mieux, et recommençons. Posons d'abord les noms. On range tous les angles des articulations dans une seule liste, le **vecteur** des angles, noté $`\boldsymbol{\theta}`$.

> **Que veut dire « vecteur » ?** Ici, un **vecteur** est simplement une liste ordonnée de nombres rangés ensemble, comme une liste de courses où chaque ligne a sa place fixe. Le vecteur $`\boldsymbol{\theta} = (\theta_1, \dots, \theta_n)`$ regroupe tous les angles : $`\theta_1`$ (« thêta un ») est l'angle de la première articulation, $`\theta_2`$ celui de la deuxième, et ainsi de suite jusqu'à la dernière. La lettre grecque $`\theta`$ (thêta) sert traditionnellement à désigner un angle.

> **Que veulent dire les autres symboles ?** $`\mathbf{e}(\boldsymbol{\theta})`$ se lit « la position de l'effecteur quand les angles valent $`\boldsymbol{\theta}`$ » : on tourne les articulations comme indiqué, et $`\mathbf{e}`$ dit où la main atterrit du coup ($`\mathbf{e}`$ comme *end effector*, l'effecteur). $`\mathbf{e}_\text{cible}`$ est l'endroit où l'on **veut** que la main arrive. On cherche le réglage parfait des angles, noté $`\boldsymbol{\theta}^\star`$ (« thêta étoile », l'étoile marquant la solution recherchée), tel que la main tombe exactement sur la cible.

Le cœur de la méthode est un tableau spécial, la **Jacobienne**, qui répond à la question « si je bouge chaque angle d'un tout petit peu, de combien et dans quelle direction la main bouge-t-elle ? ». Elle relie une petite variation des angles à la petite variation de l'effecteur qui en résulte :

```math
\Delta \mathbf{e} \approx J\,\Delta \boldsymbol{\theta}
```

> **Que veulent dire ces symboles ?** La lettre grecque $`\Delta`$ (delta majuscule) veut dire « petit changement de » : $`\Delta \boldsymbol{\theta}`$ est un petit coup de pouce donné aux angles, et $`\Delta \mathbf{e}`$ le petit déplacement de la main qui en découle. Le signe $`\approx`$ se lit « à peu près égal » : la relation n'est exacte que pour des changements minuscules, d'où la nécessité d'avancer par petits pas. $`J`$ est la Jacobienne : la multiplier par le coup de pouce des angles prédit le déplacement de la main.

> **Que veut dire « Jacobienne » et que vient faire le symbole $`\partial`$ ?** Le symbole $`\partial`$ (lu « d rond ») désigne une **dérivée partielle** : $`\partial f / \partial x`$ signifie « de combien $`f`$ change quand on bouge **uniquement** $`x`$, en laissant tout le reste figé ». Une dérivée mesure une sensibilité, une vitesse de réaction ; « partielle » veut dire qu'on ne fait bouger qu'une seule chose à la fois. La **Jacobienne** $`J = \partial \mathbf{e} / \partial \boldsymbol{\theta}`$ est le tableau qui rassemble **toutes** ces sensibilités d'un coup : la case $`J_{ij} = \partial e_i / \partial \theta_j`$ dit de combien la $`i`$-ème coordonnée de la main réagit quand on touche au $`j`$-ème angle. Pour une chaîne à 3 articulations à plat qui place une main en 3D, $`J`$ est un tableau de 3 lignes sur 3 colonnes ; plus généralement $`J`$ a $`m`$ lignes (le nombre de coordonnées qui décrivent la position visée) et $`n`$ colonnes (le nombre d'articulations qu'on peut tourner).

> **Que veut dire « degré de liberté » ?** C'est une façon de bouger indépendante des autres. Une charnière de porte n'a qu'un degré de liberté (elle ne fait que pivoter dans un sens), tandis qu'une épaule en a plusieurs (elle tourne dans plusieurs directions). Plus une chaîne d'os a de degrés de liberté, plus elle peut adopter de poses différentes pour atteindre le même point, et plus le tableau $`J`$ compte de colonnes.

On répète alors la même petite correction encore et encore, jusqu'à ce que la main touche presque la cible. Chaque correction des angles se calcule à partir de l'erreur du moment (l'écart entre où l'on veut aller et où l'on est) en la faisant passer à l'envers par la Jacobienne :

```math
\Delta \boldsymbol{\theta} = J^{+}\,(\mathbf{e}_\text{cible} - \mathbf{e}(\boldsymbol{\theta}))
```

> **Que veut dire « répéter jusqu'à convergence » ?** Une méthode **itérative** est une méthode qui répète le même petit calcul en boucle, chaque tour partant du résultat du tour précédent pour faire un peu mieux. On dit qu'elle **converge** quand les corrections deviennent si petites que la réponse ne bouge presque plus : on est arrivé. C'est comme régler la température d'une douche par petits gestes successifs jusqu'à ce qu'elle soit parfaite.

> **Que veulent dire ces symboles ?** La parenthèse $`(\mathbf{e}_\text{cible} - \mathbf{e}(\boldsymbol{\theta}))`$ est l'**erreur** : la flèche qui va de la position actuelle de la main vers la cible (« là où je veux aller » moins « là où je suis »). $`J^{+}`$ (« J croix ») est une sorte d'inverse de la Jacobienne : la Jacobienne transforme un mouvement d'angles en mouvement de main, et $`J^{+}`$ fait le travail en sens contraire, transformant le déplacement de main souhaité (l'erreur) en correction d'angles $`\Delta \boldsymbol{\theta}`$ à appliquer. Ce $`J^{+}`$ s'appelle la **pseudo-inverse de Moore-Penrose**, du nom des mathématiciens qui l'ont définie.

> **Pourquoi une « pseudo »-inverse, et pas une vraie inverse ?** Inverser un tableau de nombres, c'est comme remonter le temps d'une opération : retrouver l'entrée à partir de la sortie. Mais la vraie inversion n'est possible que pour un tableau carré (autant de lignes que de colonnes) et bien sage. Or la Jacobienne est souvent rectangulaire (plus d'articulations que de coordonnées à viser) : il y a alors plusieurs façons de plier le bras pour le même résultat. La pseudo-inverse est l'outil qui sait quand même « inverser au mieux » ces tableaux non carrés, en choisissant une solution raisonnable parmi toutes les possibles.

Il y a plusieurs manières de fabriquer ce $`J^{+}`$, et chacune donne une variante de la méthode, avec ses qualités et ses défauts. Quelques mots de vocabulaire d'abord, pour lire les trois recettes sans s'arrêter.

> **Que veut dire « transposée » ?** Transposer un tableau, noté avec un petit $`T`$ en exposant ($`J^{T}`$), c'est le basculer en échangeant ses lignes et ses colonnes, comme si on le couchait sur le côté. C'est une opération immédiate, qui ne coûte presque rien à l'ordinateur.

> **Que veut dire « au sens des moindres carrés » ?** Quand plusieurs réponses sont possibles, on choisit celle qui rend l'erreur la plus petite possible, en mesurant cette erreur par la somme des carrés des écarts. On élève au carré pour que les écarts comptent toujours positivement (un dépassement et un manque ne s'annulent pas) et pour pénaliser plus fort les gros écarts. C'est le critère universel pour « coller au mieux » à un objectif.

> **Que veut dire la norme $`\|\Delta\boldsymbol{\theta}\|`$ ?** Les deux barres verticales $`\|\;\|`$ notent la **norme**, c'est-à-dire la « longueur » d'un vecteur, sa taille globale. Ici $`\|\Delta\boldsymbol{\theta}\|`$ mesure l'ampleur totale de la correction des angles. La rendre la plus petite possible, c'est demander à l'ordinateur de bouger le moins possible, donc d'obtenir le geste le plus économe et le plus naturel.

> **Que veut dire « singularité » (et un tableau « singulier » ou « mal conditionné ») ?** Une **singularité** est une posture coincée où le bras perd une liberté de mouvement : bras tendu à fond, il ne peut plus s'allonger davantage vers l'avant, quoi qu'on fasse aux angles. Le tableau de nombres devient alors **singulier** (non inversible, comme une porte dont la poignée tourne dans le vide) ou **mal conditionné** (presque dans cet état), si bien qu'une cible un peu trop loin réclame des angles gigantesques et le bras part dans une secousse violente.

- **Jacobienne transposée** : on remplace carrément $`J^{+}`$ par $`J^{T}`$.
  - *Avantages* : très peu coûteux (aucun tableau à inverser), et tellement simple à programmer que cela tient en quelques lignes.
  - *Inconvénients* : avance lentement vers la cible. Il faut choisir une taille de pas, notée $`\alpha`$ (la lettre grecque « alpha »), qui dose l'ampleur de chaque correction : trop petite, c'est interminable ; trop grande, ça dépasse la cible et oscille. Ce réglage est délicat, et la méthode se débrouille mal sur les longues chaînes d'os.

- **Pseudo-inverse** ($`J^{+} = J^{T}(JJ^{T})^{-1}`$) : la solution dite au sens des **moindres carrés**.
  - *Avantages* : avance vite vers la cible, et parmi toutes les poses qui atteignent le but, elle choisit celle qui bouge le moins les articulations, c'est-à-dire celle qui minimise la **norme** $`\|\Delta\boldsymbol{\theta}\|`$.
  - *Inconvénients* : devient instable près des **singularités**, par exemple quand le coude est complètement tendu : là, le tableau $`JJ^{T}`$ devient singulier ou mal conditionné, et le calcul s'emballe.

- **Damped Least Squares** (DLS, qui veut dire « moindres carrés amortis » ; c'est l'application à l'IK d'une idée appelée régularisation de Levenberg-Marquardt) : $`\Delta \boldsymbol{\theta} = J^{T}(JJ^{T} + \lambda^2 I)^{-1}\,(\mathbf{e}_\text{cible} - \mathbf{e})`$.
  - *Avantages* : le freinage $`\lambda`$ stabilise tout et fait disparaître les secousses aux postures coincées. C'est la recette classique des **solveurs** d'IK (les morceaux de programme qui résolvent ces équations), qu'on retrouve aussi bien dans le logiciel d'animation Maya que dans les moteurs de jeu.
  - *Inconvénients* : ce freinage laisse une petite erreur résiduelle près des singularités (la main n'atteint plus tout à fait la cible, elle s'arrête juste à côté), et la bonne valeur de $`\lambda`$ se trouve souvent à tâtons, par essais.

> **Que veut dire « amortir » (régulariser), et que sont $`\lambda`$ et $`I`$ ?** **Amortir**, c'est freiner les emballements, comme l'amortisseur d'une voiture absorbe les chocs au lieu de les laisser secouer la caisse. On y arrive en ajoutant un petit terme $`\lambda^2 I`$ à l'intérieur du calcul. La lettre grecque $`\lambda`$ (« lambda ») est un petit bouton de réglage : plus on l'augmente, plus on freine. $`I`$ est la **matrice identité**, un tableau tout simple qui ne change rien quand on le multiplie (l'équivalent du nombre 1 pour les tableaux) ; il sert juste de support pour glisser ce freinage au bon endroit. Ajouter ce terme empêche le tableau de devenir non inversible, donc supprime les secousses près des singularités. On appelle cette astuce **régulariser** : rendre un calcul fragile bien stable.

#### CCD : Cyclic Coordinate Descent

La deuxième famille évite complètement les tableaux de nombres et raisonne directement sur les angles, une articulation à la fois. C'est la méthode **CCD** (sigle anglais de *Cyclic Coordinate Descent*, « descente cyclique sur les coordonnées »). On part de l'articulation la plus proche de la main et on remonte vers l'épaule. Pour chacune, on se pose une question toute simple et on agit aussitôt : « en faisant tourner uniquement cette articulation, quel est le pivot qui amène la main le plus près possible de la cible ? ». On applique ce pivot, on passe à l'articulation suivante, et quand on a remonté toute la chaîne on recommence du début, encore et encore.

Ici, « parcourir la chaîne de l'effecteur vers la racine » veut dire remonter de la main vers l'épaule, c'est-à-dire du bout qui bouge le plus vers l'os de départ qui tient tout l'ensemble.

> **Que veut dire « rotation pure » ?** C'est un simple pivotement sur place, sans aucun glissement : l'articulation tourne autour de son point d'attache, comme une aiguille de montre tourne autour de son centre sans se déplacer. À chaque étape, CCD cherche l'angle de ce pivot qui aligne au mieux la direction « articulation vers main » sur la direction « articulation vers cible ».

> **Que veut dire « le coût en flops est négligeable » ?** Un **flop** est une opération de calcul élémentaire sur des nombres à virgule (une addition, une multiplication). Compter les flops, c'est estimer combien de petits calculs une méthode réclame, donc combien elle fatigue l'ordinateur. « Négligeable » veut dire ici « presque rien » : CCD est très léger et tourne sans peine, même sur des dizaines de personnages à l'écran en même temps.

Cette légèreté est son grand atout : l'implémentation tient en quelques dizaines de lignes et il n'y a aucune posture coincée à craindre. En contrepartie, les poses obtenues paraissent parfois peu naturelles, parce que l'articulation traitée en premier (souvent l'épaule) fait le gros du travail avant que le coude ne s'en mêle. CCD a été très répandu dans les jeux jusqu'au milieu des années 2010, et il sert encore aujourd'hui de **solution de repli** (en anglais *fallback*, la méthode de secours utilisée quand la principale échoue).

#### FABRIK : Forward And Backward Reaching Inverse Kinematics

La troisième famille ne pense plus du tout en angles. Elle voit la chaîne d'os comme une corde à nœuds : une suite de points reliés par des segments dont la longueur ne doit jamais changer. C'est la méthode **FABRIK** (sigle anglais de *Forward And Backward Reaching Inverse Kinematics*, soit « cinématique inverse par allers-retours »), proposée en 2011 par les chercheurs Aristidou et Lasenby. Plutôt que de tordre des angles, elle déplace directement les points en gardant les os bien **rigides** (de longueur constante), grâce à deux balayages de la chaîne, l'un dans un sens, l'autre dans l'autre.

1. **Aller** (*forward pass*) : on attrape le bout de la corde, l'effecteur, et on le pose pile sur la cible. Bien sûr les os ne font plus la bonne longueur ; alors on remonte point par point vers la racine en recalant chaque point pour rétablir la longueur exacte de chaque os.
2. **Retour** (*backward pass*) : du coup la racine a bougé, ce qui est interdit (l'épaule ne se balade pas). On la remet donc à sa place de départ, et on redescend toute la chaîne dans l'autre sens en rétablissant encore une fois chaque longueur d'os.

On enchaîne ces allers-retours jusqu'à convergence, c'est-à-dire jusqu'à ce que la main touche la cible et que tous les os aient retrouvé leur bonne taille (en général 5 à 10 passes suffisent pour une chaîne de 7 os). Ses atouts : aucun tableau de nombres à inverser, aucune posture coincée, et un rendu très naturel à l'œil. On peut même empêcher une articulation de se plier au-delà du raisonnable en lui imposant des **contraintes d'angle**.

> **Que veut dire « contrainte d'angle par projection » ?** Une **contrainte d'angle** est une limite imposée à un pli, pour rester réaliste : un genou humain ne se plie que dans un sens, jamais à l'envers. La **projection** est la façon de faire respecter cette limite : si un point se retrouve dans une position interdite, on le ramène au point autorisé le plus proche, comme on rattrape doucement un bras qui se tord trop loin. À chaque passe, FABRIK applique cette correction, ce qui garde les poses crédibles.

Cette méthode est très adoptée dans les moteurs de jeu. Elle est par exemple fournie telle quelle dans l'éditeur d'animation d'Unreal Engine (son *Animation Blueprint*), sous une brique toute prête nommée `FABRIK`.

[ Retour en haut de page](#table-des-matières)

---

---

[← Texture et mappage UV](05-texture-et-mappage-uv.md) · [↑ Sommaire](../README.md#table-des-matières) · [Physique des jeux →](07-physique-des-jeux.md)
