[↑ Sommaire](../README.md#table-des-matières) · [Bases des mathématiques →](02-bases-des-mathematiques.md)

# 1. Introduction

Allumez n'importe quel jeu vidéo. Un personnage court, saute, se cogne contre un mur ; la lumière du soleil glisse sur son armure ; la caméra le suit en tournant autour de lui. Tout cela a l'air vivant. Pourtant, derrière l'écran, rien ne bouge vraiment et rien n'est vraiment éclairé. Il n'y a que des nombres, et une machine qui les recalcule des dizaines de fois par seconde pour décider quelle petite lumière colorée allumer à chaque point de l'écran.

C'est là tout le secret, et c'est le point de départ de ce cours : **un jeu vidéo, c'est des mathématiques qui défilent très vite.** Apprendre ces mathématiques, ce n'est donc pas une corvée à côté du jeu ; c'est apprendre la matière même dont le jeu est fait.

> **Que veut dire « 3D » ?** « 3D » est l'abréviation de « trois dimensions ». Une dimension, c'est une direction dans laquelle on peut se déplacer. Sur une feuille de papier, vous pouvez aller à gauche/droite et en haut/bas : deux directions, donc deux dimensions, on dit « 2D ». Dans une vraie pièce, vous pouvez en plus avancer/reculer : une troisième direction, donc trois dimensions, la « 3D ». Un jeu en 3D fait semblant de contenir un vrai volume comme une pièce, alors que l'écran, lui, reste une surface plate.

### L'écran est une grille, et chaque case est un nombre

Approchez votre oeil tout près de l'écran : l'image se découpe en une multitude de minuscules carrés de couleur, alignés en lignes et en colonnes comme les carreaux d'un cahier. Chacun de ces petits carrés s'appelle un pixel.

> **Que veut dire « pixel » ?** Un pixel est le plus petit point de couleur que l'écran sait afficher. Le mot vient de l'anglais « picture element », c'est-à-dire « élément d'image ». Imaginez une mosaïque faite de carreaux : de loin on voit un dessin, de près on voit les carreaux. Un pixel, c'est un carreau de la mosaïque qu'est votre écran.

La couleur d'un pixel n'est pas « rouge » ou « bleu » au sens où nous le dirions ; pour la machine, c'est un mélange chiffré de trois lumières : un peu de rouge, un peu de vert, un peu de bleu. On donne donc à chaque pixel trois nombres, par exemple « beaucoup de rouge, un peu de vert, pas de bleu » pour obtenir de l'orange. Faire un jeu qui s'affiche, c'est, au fond, calculer les bons nombres de couleur pour des millions de pixels, plusieurs fois par seconde. Et pour calculer une couleur, il faut d'abord savoir où sont les objets, comment ils sont tournés, et comment la lumière les frappe : voilà pourquoi tout commence par de la géométrie.

> **Que veut dire « géométrie » ?** La géométrie est la partie des mathématiques qui parle des formes, des positions et des distances : où sont les choses, à quelle distance, dans quel sens elles pointent. C'est exactement le genre de questions qu'un jeu se pose en permanence (« le monstre est-il devant le héros ? », « la balle touche-t-elle le mur ? »).

### Une position, c'est juste des nombres

Pour placer un objet dans le monde du jeu, l'ordinateur n'a pas de « gauche » ni de « là-bas » : il a besoin de chiffres. On choisit donc un point de départ, qu'on appelle l'origine, et on mesure tout par rapport à lui, comme on donnerait une adresse à partir de la porte d'entrée : « trois pas vers la droite, deux pas vers le fond, un pas vers le haut ». Ces quelques nombres mis ensemble forment ce qu'on appelle les **coordonnées** de l'objet.

> **Que veut dire « coordonnées » ?** Les coordonnées d'un point sont la liste de nombres qui disent exactement où il se trouve, comme les cases d'une bataille navale (« B3 ») disent où viser. En 2D il en faut deux (gauche/droite, haut/bas) ; en 3D il en faut trois (en ajoutant avant/arrière).

Si déplacer un objet revient à changer ses nombres de position, alors **déplacer**, **faire tourner** ou **agrandir** un personnage devient une affaire de calcul sur ces nombres. C'est précisément ce que font, sans relâche, les outils mathématiques que ce cours va construire un par un : les **vecteurs** pour représenter des positions et des déplacements, les **matrices** pour appliquer d'un coup une rotation ou un changement de taille, la **trigonométrie** pour les angles, et bien d'autres.

> **Que veut dire « vecteur » ?** Un vecteur est une flèche : il dit à la fois dans quelle direction aller et sur quelle longueur. « Trois mètres vers le nord » est un vecteur. Dans un jeu, la vitesse d'une voiture ou la direction d'un saut sont des vecteurs. Le chapitre suivant les détaille.

> **Que veut dire « matrice » ?** Une matrice est un tableau de nombres rangés en lignes et en colonnes, un peu comme une grille de Sudoku. Cela paraît abstrait, mais une matrice est en réalité une machine à transformer des positions : on lui donne les coordonnées d'un point, elle rend les coordonnées du point une fois déplacé, tourné ou redimensionné. Nous y reviendrons en détail.

> **Que veut dire « trigonométrie » ?** La trigonométrie est la branche des mathématiques qui relie les angles aux longueurs. C'est elle qui permet de répondre à « si je tourne de 30 degrés, de combien est-ce que je me décale vers la droite ? ». Indispensable dès qu'un objet pivote.

### Pourquoi recalculer tout, tout le temps ?

Un film est une suite d'images fixes qu'on fait défiler assez vite pour donner l'illusion du mouvement. Un jeu fait pareil, à une différence énorme près : ses images n'existent pas à l'avance. Comme le joueur peut, à tout instant, tourner à gauche plutôt qu'à droite, la machine doit **fabriquer chaque image au dernier moment**, en fonction de ce que le joueur vient de faire. On appelle chacune de ces images une frame, et on en produit souvent 30 ou 60 par seconde.

> **Que veut dire « frame » ?** Une frame est une image complète affichée à l'écran à un instant donné, comme une seule photo d'un film. Dire qu'un jeu tourne à « 60 frames par seconde » signifie qu'il calcule et affiche 60 images neuves chaque seconde.

Cette contrainte explique pourquoi un jeu a besoin de mathématiques à la fois justes et **rapides**. Si calculer une image prend trop de temps, le jeu saccade. Une bonne partie de ce cours consiste donc non seulement à trouver la bonne formule, mais aussi à trouver la formule qui donne le résultat assez vite pour tenir dans le temps d'une frame. C'est ce double objectif, juste et rapide, qui rendra utile chaque outil présenté dans les chapitres suivants.

Le chapitre suivant pose les fondations : compter les positions avec des coordonnées, manier les vecteurs et les matrices, et utiliser la trigonométrie. Tout le reste, l'éclairage, les ombres, les textures, l'animation, la physique, le réseau, s'appuiera dessus.

---

[↑ Sommaire](../README.md#table-des-matières) · [Bases des mathématiques →](02-bases-des-mathematiques.md)
