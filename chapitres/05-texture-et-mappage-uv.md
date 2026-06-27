[← Éclairage et ombres](04-eclairage-et-ombres.md) · [↑ Sommaire](../README.md#table-des-matières) · [Animation →](06-animation.md)

# 5. Texture et mappage UV

### Texture et coordonnées de texture

Imaginez un personnage en bois tout nu, sans peinture. Pour lui donner des yeux, une chemise à carreaux et la peau qui va bien, deux solutions : sculpter chaque détail dans le bois (long et lourd), ou bien découper une image en papier et la coller dessus comme un autocollant. En 3D, on choisit presque toujours la deuxième : l'autocollant s'appelle une **texture**.

> **Que veut dire « texture » ?** C'est une image plate (comme une photo) que l'on colle sur la surface d'un objet en relief pour faire croire qu'il a des détails. Le mot vient de l'idée de « tissu » : c'est l'habillage de l'objet.

> **Que veut dire « 2D » et « 3D » ?** « 2D » veut dire « à deux dimensions » : plat, comme une feuille de papier, qui a une largeur et une hauteur mais pas d'épaisseur. « 3D » veut dire « à trois dimensions » : un objet du vrai monde, comme une balle, qui a en plus une profondeur.

Une texture est donc une image plate (en 2D) que l'on applique sur un objet en relief (en 3D) pour donner l'**illusion** de détails : des couleurs, des motifs ou des petites bosses. On dit « illusion » parce que la surface reste lisse : c'est le dessin qui fait croire qu'il y a du relief, comme une nappe avec un faux motif de bois qui n'est pas du bois.

Pourquoi se donner cette peine plutôt que de modéliser chaque détail en relief ? Parce qu'un objet en 3D est fait de minuscules triangles, et qu'il en faudrait des millions pour sculpter le grain d'une peau ou les lettres d'une affiche. Coller une image revient beaucoup moins cher : l'ordinateur dessine peu de triangles et laisse l'image faire tout le travail des détails.

Une même texture peut servir à décrire plusieurs choses sur un objet :

- sa **couleur** de base (sa peinture : le rouge d'une pomme),
- sa **brillance** (est-ce que ça brille comme du métal poli, ou non comme du carton),
- sa **rugosité** (est-ce lisse comme du verre, ou rêche comme du papier de verre),
- sa **transparence** (est-ce qu'on voit à travers, comme une vitre, ou pas du tout).

Reste une question : quand on colle une image plate sur un objet en relief, comment l'ordinateur sait-il **quel coin de l'image** va sur **quel endroit de l'objet** ? C'est le rôle des coordonnées de texture.

> **Que veut dire « coordonnées » ?** Ce sont des nombres qui servent d'adresse pour repérer un point précis. Sur une carte au trésor en quadrillage, « colonne 3, ligne 5 » est une coordonnée : elle pointe une seule case. Ici, on s'en sert pour pointer un endroit précis dans l'image.

Les **coordonnées de texture**, aussi appelées **coordonnées UV**, sont justement l'adresse d'un point dans l'image. Elles indiquent à l'ordinateur, pour chaque morceau de l'objet, quel endroit de l'image plate il doit aller chercher.

> **Pourquoi les lettres « U » et « V » ?** Pour repérer un point dans l'image, il faut deux nombres : un pour dire « à quelle distance vers la droite » et un pour « à quelle distance vers le haut ». On les a appelés $`u`$ et $`v`$ tout simplement parce que ce sont les lettres juste avant $`x`$, $`y`$, $`z`$ dans l'alphabet, et que $`x`$, $`y`$, $`z`$ étaient déjà prises pour repérer les points de l'objet en 3D. On garde ainsi deux séries de noms qui ne se mélangent pas : $`x, y, z`$ pour l'objet, $`u, v`$ pour l'image.

> **Le symbole $`(u, v)`$.** C'est un couple de deux nombres écrits entre parenthèses. Le premier, $`u`$, dit jusqu'où aller vers la droite dans l'image ; le second, $`v`$, jusqu'où aller vers le haut. Ensemble, ils pointent un seul point de l'image, exactement comme « colonne, ligne » pointe une seule case.

Pour habiller un objet, on procède point par point. Un objet en 3D est en réalité décrit par une liste de coins, ses **sommets**, et l'on donne à chacun son adresse $`(u, v)`$ dans l'image.

> **Que veut dire « sommet » ?** C'est un coin de l'objet, un point précis de sa surface. Un cube, par exemple, a huit sommets (ses huit coins). En 3D, toute la forme est définie par ses sommets reliés entre eux ; on dit « un sommet », « des sommets » (en anglais : *vertex*, *vertices*).

À chaque sommet, on accroche donc une petite étiquette qui dit « pour ce coin-ci, va chercher la couleur à tel endroit de l'image ». L'ordinateur remplit ensuite tout l'espace entre les sommets en étalant l'image de proche en proche, un peu comme on tend un drap imprimé en le tenant par les coins.

Ces deux nombres $`u`$ et $`v`$ ne se comptent pas en pixels, mais vont toujours de $`0`$ à $`1`$. C'est une règle bien commode : $`0`$ veut dire « tout au bord » et $`1`$ « au bord opposé », quelle que soit la taille de l'image.

> **Pourquoi de $`0`$ à $`1`$ et pas en pixels ?** Parce qu'on ne veut pas avoir à changer toutes les adresses si l'on remplace l'image par une plus grande ou plus petite. En disant « $`0{,}5`$ », on veut dire « pile au milieu », que l'image fasse 100 ou 4000 points de large. La position est donnée en **fraction** du chemin total : $`0`$ = le début, $`0{,}5`$ = la moitié, $`1`$ = la fin. C'est comme dire « à mi-hauteur d'un mur » au lieu de « à 1 mètre 50 » : ça marche pour tous les murs.

> **Que veut dire « API graphique » ?** Une **API** (de l'anglais *Application Programming Interface*, « interface de programmation ») est l'ensemble des commandes qu'un programme peut adresser à la carte graphique pour lui dire quoi dessiner. C'est comme le menu d'un restaurant : la liste des plats que la cuisine sait préparer et le nom exact à prononcer pour les commander. OpenGL et DirectX sont deux de ces « menus », chacun avec ses habitudes.

Il y a cependant un détail qui rend fou plus d'un débutant : tout le monde n'est pas d'accord sur l'endroit où se trouve le point de départ $`(0, 0)`$ de l'image. Selon l'**API graphique** (le langage utilisé pour parler à la carte graphique), le coin de départ change.

> **Que veut dire « OpenGL » et « DirectX » ?** Ce sont deux grandes familles de commandes pour piloter la carte graphique. **OpenGL** fonctionne sur presque tous les appareils (Windows, Mac, Linux, téléphones). **DirectX** (dont la partie 3D s'appelle **Direct3D**) est la famille de Microsoft, utilisée surtout sur Windows et les consoles Xbox. Ce sont deux manières concurrentes de demander la même chose : afficher des images 3D.

> **Que veut dire « moteur » (de jeu) ?** Un **moteur de jeu** est une grosse boîte à outils toute prête (par exemple Unity ou Unreal) qui s'occupe pour le programmeur d'afficher les images, de gérer les collisions, le son, etc. Plutôt que de tout réécrire, on construit son jeu par-dessus le moteur, comme on construit une maison sur des fondations déjà coulées.

Concrètement : en OpenGL, le point $`(0, 0)`$ est le coin **inférieur gauche** de l'image ; en DirectX/Direct3D et dans la plupart des moteurs de jeu, $`(0, 0)`$ est le coin **supérieur gauche**. Autrement dit, les deux comptent la hauteur dans le sens inverse l'un de l'autre : pour l'un, on part du bas ; pour l'autre, du haut.

C'est ce désaccord qui explique un bug très courant : une texture qui apparaît **à l'envers de haut en bas** quand on déplace un jeu d'une API à l'autre. Rien n'est cassé ; simplement, ce que l'un appelle « le haut de l'image », l'autre l'appelle « le bas ». La solution habituelle est de retourner la texture verticalement, ou de remplacer $`v`$ par $`1 - v`$ pour inverser le sens du comptage.

### Mappage UV

Donner son adresse $`(u, v)`$ à chaque sommet, un par un, serait épuisant à la main sur un objet qui en compte des milliers. Le **mappage UV** est justement le travail qui consiste à décider, pour chaque sommet de l'objet, quelle est sa place dans l'image.

> **Que veut dire « mappage » ?** « Mapper », c'est faire correspondre. Un mappage relie chaque chose d'un côté à une chose précise de l'autre, comme un plan de table qui relie chaque invité à une chaise précise. Ici, on relie chaque coin de l'objet à un endroit précis de l'image.

Le mot vient de l'anglais *map*, « carte ». L'idée est exactement celle d'une carte de géographie : déplier un objet du relief (la Terre, ronde) sur une feuille plate (la carte), en notant à quel endroit du papier correspond chaque lieu réel.

Ce travail est souvent fait à la main par des **artistes 3D** dans des logiciels spécialisés, parce qu'un humain juge mieux où placer les coupures pour que l'habillage soit joli. Mais il existe aussi des **algorithmes** capables de le faire tout seuls.

> **Que veut dire « algorithme » ?** C'est une recette précise, une suite d'étapes que l'on suit dans l'ordre pour obtenir un résultat à coup sûr, comme une recette de cuisine. Un ordinateur ne sait faire que ça : exécuter des recettes très détaillées qu'on lui a données.

On choisit la méthode de mappage selon la **forme** de l'objet, car aucune méthode unique ne convient à tout. Voici les principales.

1. **Mappage planaire** : on projette l'image sur l'objet comme avec un projecteur de cinéma, en l'envoyant tout droit depuis un mur plat.

   > **Que veut dire « projeter » ?** C'est envoyer une image en ligne droite sur une surface, comme la lampe d'un projecteur envoie le film sur l'écran, ou comme votre ombre se dessine sur le mur quand vous êtes devant une lampe.

   > **Que veut dire « plan » et « planaire » ?** Un **plan** est une surface parfaitement plate qui s'étend dans toutes les directions, comme un mur ou le dessus d'une table, mais sans bord. « Planaire » veut dire « qui vient d'un plan » : ici, l'image arrive tout droit depuis une surface plate.

   Cette méthode marche très bien pour les objets assez plats (un mur, une affiche, le sol). Mais sur une forme bombée ou repliée, l'image arrive tout droit et n'épouse pas le relief : elle s'étire et se déforme sur les flancs, comme l'image d'un projecteur qui bave sur les côtés d'un ballon.

   > **Que veut dire « distorsion » et « étirement » ?** Une **distorsion** est une déformation : l'image devient tordue, pas fidèle à l'originale. Un **étirement** en est un cas précis : l'image est tirée comme une pâte à modeler, si bien qu'un rond devient un ovale et qu'un carré devient un rectangle aplati.

2. **Mappage cylindrique** : on enroule l'image autour de l'objet comme une étiquette autour d'une boîte de conserve.

   > **Que veut dire « cylindre » (et « cylindrique ») ?** Un **cylindre** est la forme d'une boîte de conserve ou d'un rouleau de papier : un tube tout droit. « Cylindrique » veut dire « en forme de cylindre ».

   C'est parfait pour les objets en forme de tube : un tronc d'arbre, un poteau, une bouteille. L'image fait le tour sans se déchirer ni trop s'étirer, exactement comme l'étiquette de la boîte de conserve épouse le tube.

3. **Mappage sphérique** : on projette l'image depuis une boule qui entoure l'objet, comme on enveloppe une orange dans du papier.

   > **Que veut dire « sphère » (et « sphérique ») ? Que sont les « pôles » ?** Une **sphère** est la forme d'une balle, parfaitement ronde dans toutes les directions. « Sphérique » veut dire « en forme de boule ». Les **pôles** sont les deux points tout en haut et tout en bas de la boule, comme le pôle Nord et le pôle Sud sur un globe terrestre.

   Cette méthode est idéale pour les objets ronds : une planète, une bille, une tête. Le seul ennui se trouve aux deux pôles : l'image s'y resserre et se chiffonne, exactement comme le papier d'emballage qui fait des plis serrés au sommet de l'orange quand on tente de la couvrir d'une seule feuille.

4. **Mappage par morceaux** (*dépliage UV*, en anglais *UV unwrapping*) : plutôt que de plaquer l'image d'un bloc, on découpe d'abord l'objet en plusieurs morceaux, puis on aplatit chaque morceau sur la feuille en 2D, ce qui donne une figure plate de tout l'objet, prête à être peinte.

   C'est exactement la méthode des cartes de géographie, ou d'un patron de couture : on découpe le relief le long de coutures bien choisies pour pouvoir l'étaler à plat sans le froisser. Parce que chaque morceau est presque plat, l'image se pose dessus sans presque se déformer : c'est la technique qui donne les distorsions les plus faibles. En contrepartie, bien placer les coupures demande un travail soigné, le plus souvent à la main, ce qui prend du temps mais offre le plus beau résultat.

Le tableau ci-dessous résume quelle méthode convient à quelle forme.

| Méthode | Forme idéale | Point faible |
|---|---|---|
| Planaire | objets plats (mur, sol) | s'étire sur les formes bombées |
| Cylindrique | tubes (tronc, bouteille) | se déforme en haut et en bas du tube |
| Sphérique | boules (planète, tête) | se chiffonne aux deux pôles |
| Par morceaux | toute forme compliquée | demande un travail manuel soigné |

[ Retour en haut de page](#table-des-matières)

---

---

[← Éclairage et ombres](04-eclairage-et-ombres.md) · [↑ Sommaire](../README.md#table-des-matières) · [Animation →](06-animation.md)
