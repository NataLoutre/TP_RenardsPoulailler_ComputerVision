<h1 align="center">Projet Computer Vision :  Détection de renards dans les poulaillers</h1>

<p align="center">
  <i>Ingé 3I - 19/02/2026
</i>
</p>

<p align="center">
    CRUEYZE Raphaël - HAROUNYAN Natalène - PADRINO Gabriel - ZARGOUNI Yacine
</p>

<p align="center">
  <img src="https://img.shields.io/badge/python-3.10+-green" />
<br/>

---

## 📑 Table des matières

- [1. Introduction au projet](#1-introduction-au-projet)
- [2. Dataset personnalisé](#2-dataset-personnalisé)
- [3. Entrainement des différents modèles](#3-entrainement-des-différents-modèles)

<br/>

---

## 1. Introduction au projet
<p align="justify">
On a tous connu une gentille poule, morte dans d’atroces souffrances à cause d’un… Renard. 
La vôtre s’appelait sans doute « poupoule », « poulette », «Bérénice »…

C'est pourquoi l'entreprise <b>Poul-ai-ller</b> à mis en place son système NugGet-Information. 

Grâce à lui, fini les nuits stressantes à imaginer le pire pour vos poules : vous pouvez suivre en direct votre poulailler et être averti lorsqu’un renard pointe le bout de son nez ! 
</p>

<p align="center">
  <img src="Images_Readme\logo_poulailler.jpg" width="80%"/>
  <br>
  <em>Figure 1 – Logo du projet Chicken & Fox Detection</em>
</p>

<br/>

---
## 2. Dataset personnalisé
<p align="justify">
Le dataset COCO ne contenant pas les classes chicken et fox, nous avons constitué un dataset personnalisé à partir de 15 vidéos YouTube variées. Nous avons extrait uniquement les séquences pertinentes (7 à 10 secondes) afin d’obtenir des images représentatives.

Le dataset inclut des images en RGB et en niveaux de gris, avec des points de vue variés et des individus à différentes échelles pour garantir une bonne diversité.

Les annotations ont toutes été réalisées manuellement. Le jeu de données final comprend :
 <ul>
  <li>1041 images</li>
  <li>3114 bounding boxes</li>
  <li>3 classes : chicken, fox et background</li>
</ul>

Les images ont ensuite été réaprties en groupes d'entrainement (_train_) de validation (_valid_) et de _test_ avec une proportion respective de 70%, 20% et 10%.
</p>

<p align="center">
  <img src="Images_Readme\2_trainTestVal.png"/>
  <br>
  <em>Figure 2 – Séparation des images train, valid et test</em>
</p>

<table align="center">
  <tr>
    <td align="center">
      <img src="Images_Readme\2_heatMap.png" width="90%"/><br>
      <em>Figure 3 – Carte de chaleur des annotations</em>
    </td>
    <td align="center">
      <img src="Images_Readme\2_histogram.png" width="100%"/><br>
      <em>Figure 4 – Histogramme du nombre d'objets par image</em>
    </td>
  </tr>
</table>

---
## 3. Entrainement des différents modèles
Test de SAM

Test de YOLO 8 pré entrainé sur COCO 

Nous avons finalement sélectionné 3 modèles : 
 <ul>
  <li>YOLO 11</li>
  <li>YOLO 26</li>
  <li>RF-DETR</li>
</ul>

