# [Tansoftware](https://www.tansoftware.com) - Mathématiques pour la programmation de jeux vidéo

[![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png)](https://fr.wikipedia.org/wiki/Fran%C3%A7ais) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE) [![Made with Markdown](https://img.shields.io/badge/Made%20with-Markdown-1f425f.svg)](https://www.markdownguide.org/) [![Topic](https://img.shields.io/badge/Topic-Game%20Math-brightgreen.svg)](https://en.wikipedia.org/wiki/Video_game_graphics) [^1]

> Un cours complet, en français, qui rassemble les fondations mathématiques nécessaires pour comprendre, concevoir et programmer un jeu vidéo moderne, du vecteur 2D jusqu'au pipeline de rendu d'un GPU.

---

## Table des matières

1. [Introduction](chapitres/01-introduction.md)
2. [Bases des mathématiques](chapitres/02-bases-des-mathematiques.md)
   - [Coordonnées cartésiennes](chapitres/02-bases-des-mathematiques.md#coordonnées-cartésiennes)
   - [Précision flottante : ce que tout dev de jeu doit savoir](chapitres/02-bases-des-mathematiques.md#précision-flottante--ce-que-tout-dev-de-jeu-doit-savoir)
   - [Aléa et déterminisme](chapitres/02-bases-des-mathematiques.md#aléa-et-déterminisme)
   - [Trigonométrie](chapitres/02-bases-des-mathematiques.md#trigonométrie)
   - [Vecteurs](chapitres/02-bases-des-mathematiques.md#vecteurs)
   - [Interpolation](chapitres/02-bases-des-mathematiques.md#interpolation)
   - [Matrices](chapitres/02-bases-des-mathematiques.md#matrices)
   - [Transformations](chapitres/02-bases-des-mathematiques.md#transformations)
   - [Géométrie linéaire](chapitres/02-bases-des-mathematiques.md#géométrie-linéaire)
3. [Graphiques informatiques](chapitres/03-graphiques-informatiques.md)
   - [Graphiques vectoriels et bitmap](chapitres/03-graphiques-informatiques.md#graphiques-vectoriels-et-bitmap)
   - [Résolution et profondeur de couleur](chapitres/03-graphiques-informatiques.md#résolution-et-profondeur-de-couleur)
   - [Espaces de couleur](chapitres/03-graphiques-informatiques.md#espaces-de-couleur)
   - [Formats de fichier d'image](chapitres/03-graphiques-informatiques.md#formats-de-fichier-dimage)
4. [Éclairage et ombres](chapitres/04-eclairage-et-ombres.md)
   - [Sources de lumière](chapitres/04-eclairage-et-ombres.md#sources-de-lumière)
   - [Modèles d'éclairage](chapitres/04-eclairage-et-ombres.md#modèles-déclairage)
   - [Ombres](chapitres/04-eclairage-et-ombres.md#ombres)
5. [Texture et mappage UV](chapitres/05-texture-et-mappage-uv.md)
   - [Texture et coordonnées de texture](chapitres/05-texture-et-mappage-uv.md#texture-et-coordonnées-de-texture)
   - [Mappage UV](chapitres/05-texture-et-mappage-uv.md#mappage-uv)
6. [Animation](chapitres/06-animation.md)
   - [Animation par squelette](chapitres/06-animation.md#animation-par-squelette)
   - [Animation de forme](chapitres/06-animation.md#animation-de-forme)
   - [Cinématique inverse](chapitres/06-animation.md#cinématique-inverse)
7. [Physique des jeux](chapitres/07-physique-des-jeux.md)
   - [Simulation physique](chapitres/07-physique-des-jeux.md#simulation-physique)
   - [Détection de collision](chapitres/07-physique-des-jeux.md#détection-de-collision)
   - [Résolution de collision](chapitres/07-physique-des-jeux.md#résolution-de-collision)
8. [Intelligence artificielle](chapitres/08-intelligence-artificielle.md)
   - [Comportement de base](chapitres/08-intelligence-artificielle.md#comportement-de-base)
   - [Navigation](chapitres/08-intelligence-artificielle.md#navigation)
   - [Apprentissage automatique](chapitres/08-intelligence-artificielle.md#apprentissage-automatique)
9. [Réseau et multijoueur](chapitres/09-reseau-et-multijoueur.md)
   - [Modèles de réseau](chapitres/09-reseau-et-multijoueur.md#modèles-de-réseau)
   - [Protocoles de communication](chapitres/09-reseau-et-multijoueur.md#protocoles-de-communication)
   - [Programmation de jeu multijoueur](chapitres/09-reseau-et-multijoueur.md#programmation-de-jeu-multijoueur)
10. [Techniques avancées](chapitres/10-techniques-avancees.md)
   - [Génération procédurale et bruit](chapitres/10-techniques-avancees.md#génération-procédurale-et-bruit)
   - [Physique des fluides](chapitres/10-techniques-avancees.md#physique-des-fluides)
   - [Écrans multiples et fenêtrage](chapitres/10-techniques-avancees.md#écrans-multiples-et-fenêtrage)
   - [Intelligence artificielle avancée](chapitres/10-techniques-avancees.md#intelligence-artificielle-avancée)
   - [Rendu avancé](chapitres/10-techniques-avancees.md#rendu-avancé)
11. [Pipeline de rendu](chapitres/11-pipeline-de-rendu.md)
   - [Étapes du pipeline](chapitres/11-pipeline-de-rendu.md#étapes-du-pipeline)
   - [Culling et occlusion](chapitres/11-pipeline-de-rendu.md#culling-et-occlusion)
   - [Shaders](chapitres/11-pipeline-de-rendu.md#shaders)

---

## Pour aller plus loin

Voici quelques ressources reconnues pour approfondir les sujets abordés dans ce cours :

### Livres

- *Mathematics for 3D Game Programming and Computer Graphics*, Eric Lengyel
- *Real-Time Rendering*, Tomas Akenine-Möller, Eric Haines, Naty Hoffman et al.
- *Game Engine Architecture*, Jason Gregory
- *Physically Based Rendering: From Theory to Implementation*, Matt Pharr, Wenzel Jakob, Greg Humphreys ([disponible en ligne](https://www.pbr-book.org/))
- *Game Programming Patterns*, Robert Nystrom ([disponible en ligne](https://gameprogrammingpatterns.com/))

### Sites et tutoriels

- [Khan Academy, Mathématiques](https://fr.khanacademy.org/math), bases solides en algèbre, trigonométrie, calcul.
- [LearnOpenGL](https://learnopengl.com/), un excellent tutoriel pour comprendre le pipeline graphique en pratique.
- [Scratchapixel](https://www.scratchapixel.com/), articles très approfondis sur le rendu et la 3D.
- [The Book of Shaders](https://thebookofshaders.com/), introduction interactive aux shaders.
- [Catlike Coding](https://catlikecoding.com/unity/tutorials/), tutoriels avancés Unity (rendu, mathématiques, shaders).

### Moteurs et frameworks à explorer

- [Unity](https://unity.com/), [Unreal Engine](https://www.unrealengine.com/), [Godot](https://godotengine.org/), moteurs grand public.
- [Bevy](https://bevyengine.org/) (Rust), [MonoGame](https://www.monogame.net/) (C\#), [raylib](https://www.raylib.com/) (C), frameworks plus légers, idéaux pour apprendre.

[ Retour en haut de page](#table-des-matières)

---

## Contribuer

Ce dépôt est en évolution permanente. Si vous repérez une coquille, une imprécision mathématique ou si vous souhaitez ajouter un schéma ou un exemple :

1. **Fork** ce dépôt
2. Créez une branche : `git checkout -b ameliore-section-xxx`
3. Faites vos modifications
4. Ouvrez une **Pull Request** avec une description claire

Toute contribution (correction, illustration, exemple de code, traduction) est la bienvenue.

## Licence

Ce projet est distribué sous licence **MIT**. Voir le fichier [LICENSE](LICENSE) pour plus d'informations.

---

[^1]: Des modifications peuvent survenir. [Tanguy Chénier](https://www.linkedin.com/in/tanguy-chenier/).
