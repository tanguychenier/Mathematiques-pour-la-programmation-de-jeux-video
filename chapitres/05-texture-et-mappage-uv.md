[← Éclairage et ombres](04-eclairage-et-ombres.md) · [↑ Sommaire](../README.md#table-des-matières) · [Animation →](06-animation.md)

# 5. Texture et mappage UV

### Texture et coordonnées de texture

Les **textures** sont des images 2D appliquées sur des objets 3D pour donner l'illusion de détails tels que les couleurs, les motifs ou les reliefs.

Elles peuvent être utilisées pour représenter la **couleur** de base d'un objet, sa **brillance**, sa **rugosité**, sa **transparence**, etc.

Les **coordonnées de texture**, également appelées **coordonnées UV**, déterminent la manière dont une texture est mappée sur un objet 3D.

Pour appliquer une texture à un objet 3D, on attribue à chaque sommet de l'objet un ensemble de coordonnées UV, qui correspondent aux coordonnées $(u, v)$ dans l'image de texture. Les coordonnées UV varient généralement de $0$ à $1$. La convention de l'origine diffère selon l'API graphique : en OpenGL, $(0, 0)$ est le coin **inférieur gauche** ; en DirectX/Direct3D et dans la majorité des moteurs, $(0, 0)$ est le coin **supérieur gauche**. Cette différence explique souvent les textures qui apparaissent à l'envers verticalement lors d'un portage entre APIs.

### Mappage UV

Le **mappage UV** est le processus qui consiste à déterminer les coordonnées UV pour chaque sommet d'un objet 3D. Ce processus est souvent réalisé manuellement par des artistes 3D à l'aide de logiciels spécialisés, mais il existe également des algorithmes de mappage UV automatiques.

Il existe plusieurs techniques de mappage UV :

1. **Mappage planaire** : projette la texture sur l'objet 3D à partir d'un plan. Fonctionne bien pour les objets ayant une forme relativement plane, mais peut provoquer des distorsions et des étirements sur les objets plus complexes.
2. **Mappage cylindrique** : enroule la texture autour de l'objet 3D comme si elle était imprimée sur un cylindre. Fonctionne bien pour les objets cylindriques.
3. **Mappage sphérique** : projette la texture sur l'objet 3D à partir d'une sphère. Fonctionne bien pour les objets sphériques, mais peut provoquer des distorsions aux pôles.
4. **Mappage par morceaux** (*UV unwrapping*) : on découpe l'objet 3D en morceaux, puis on les déplie en 2D pour créer une représentation plane de l'objet. Cette technique permet de minimiser les distorsions, mais nécessite généralement un travail manuel minutieux pour obtenir de bons résultats.

[ Retour en haut de page](#table-des-matières)

---

---

[← Éclairage et ombres](04-eclairage-et-ombres.md) · [↑ Sommaire](../README.md#table-des-matières) · [Animation →](06-animation.md)
