[← Graphiques informatiques](03-graphiques-informatiques.md) · [↑ Sommaire](../README.md#table-des-matières) · [Texture et mappage UV →](05-texture-et-mappage-uv.md)

# 4. Éclairage et ombres

Les techniques d'éclairage et de gestion des ombres sont basées sur des concepts mathématiques et physiques qui permettent de **simuler la manière dont la lumière interagit** avec les objets et l'environnement.

### Sources de lumière

Les sources de lumière sont des entités qui émettent de la lumière dans une scène. Les principales sources utilisées dans les jeux vidéo et les graphiques 3D sont :

1. **Lumière directionnelle** : représente une source située à une distance infinie, comme le soleil. Tous les rayons lumineux sont parallèles et ont la même intensité. Souvent utilisée pour simuler la lumière du jour.
2. **Lumière ponctuelle** : émet de la lumière dans toutes les directions à partir d'un point dans l'espace. L'intensité diminue avec la distance à la source, généralement proportionnelle à l'**inverse du carré de la distance**.
3. **Lumière spot** : émet de la lumière dans une direction conique à partir d'un point dans l'espace. Souvent utilisée pour simuler les projecteurs ou les lampes torches.

### Modèles d'éclairage

Les modèles d'éclairage décrivent comment la lumière interagit avec les objets et les surfaces. Voici quelques-uns des modèles les plus couramment utilisés :

1. **Modèle d'éclairage de Phong** : basé sur trois composantes — l'éclairage **ambiant**, l'éclairage **diffus** et l'éclairage **spéculaire**. L'éclairage ambiant est une constante qui simule la lumière indirecte réfléchie par l'environnement. L'éclairage diffus est proportionnel à l'angle entre la normale de la surface et la direction de la lumière. L'éclairage spéculaire dépend de l'angle entre la direction de la lumière réfléchie et la direction de la caméra.
2. **Modèle d'éclairage de Lambert** : modèle diffus pur (antérieur à Phong), qui ne prend en compte que l'éclairage ambiant et l'éclairage diffus. Moins réaliste qu'un modèle avec composante spéculaire mais plus rapide à calculer — un choix approprié pour les jeux vidéo sur des systèmes à faible puissance de calcul.
3. **Modèle d'éclairage de Blinn-Phong** : variante du modèle de Phong qui remplace le calcul de la direction de réflexion exacte par un *half-vector* (vecteur médian entre la lumière et la caméra) pour calculer l'éclairage spéculaire. Moins coûteux que Phong, avec un rendu spéculaire légèrement différent, et souvent préféré dans les jeux vidéo.

### Ombres

Les **ombres** sont des zones où la lumière est bloquée par un objet. Elles ajoutent de la profondeur et du réalisme à une scène. Voici quelques techniques couramment utilisées :

1. **Ombres portées** (*Shadow mapping*) : on crée une carte des profondeurs (*depth map*) à partir de la perspective de la source de lumière. Cette carte stocke la distance entre la source de lumière et le point le plus proche qui la bloque. Lors du rendu, on compare la distance entre la source de lumière et le point courant avec la distance stockée dans la carte. Si la distance courante est supérieure, le point est dans l'ombre.
2. **Ombres volumétriques** (*Volumetric shadows*) : simulent les ombres en calculant l'atténuation de la lumière lorsqu'elle traverse des objets semi-transparents, comme la fumée ou la brume. Donnent un aspect réaliste aux scènes où la lumière interagit avec des particules en suspension dans l'air.
3. **Ombres douces** (*Soft shadows*) : ombres qui présentent un flou progressif en s'éloignant de l'objet qui les projette. On simule plusieurs sources de lumière proches les unes des autres, ou on utilise des techniques de filtrage pour adoucir les bords des ombres portées.
4. **Ray tracing** : technique de rendu avancée qui simule le comportement de la lumière en traçant des rayons depuis la caméra jusqu'à la source de lumière, en prenant en compte les **réflexions** et les **réfractions**. Permet de générer des ombres, des reflets et des effets de lumière globale très réalistes — mais coûteux en temps de calcul. De plus en plus utilisé dans les jeux vidéo grâce à l'évolution des cartes graphiques et des algorithmes de rendu.

[ Retour en haut de page](#table-des-matières)

---

---

[← Graphiques informatiques](03-graphiques-informatiques.md) · [↑ Sommaire](../README.md#table-des-matières) · [Texture et mappage UV →](05-texture-et-mappage-uv.md)
