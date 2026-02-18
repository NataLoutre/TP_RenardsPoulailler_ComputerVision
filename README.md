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
- [4. Conclusion](#4-conclusion)

<br/>

Les diapositives du projet sont disponibles dans le document : ```Présentation_RenardsPoulailler_ComputerVision_CRUEYZE_HAROUNYAN_PADRINO_ZARGOUNI.pptx```
<br/>

---

## 1. Introduction au projet
<p align="justify">
On a tous connu une gentille poule, morte dans d’atroces souffrances à cause d’un… Renard. 
La vôtre s’appelait sans doute « poupoule », « poulette », «Bérénice »…

C'est pourquoi l'entreprise <b>Poul-ai-ller</b> a mis en place son système NugGet-Information. 

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

- [A. Les modèles non-retenus](#a-les-modèles-non-retenus)
- [B. Les modèles entrainés](#b-les-modèles-entrainés)

### A. Les modèles non-retenus

<p align="justify">
Nous avons testé le modèle <b>SAM2</b> (Segment Anything Model 2). Il s'agit d'un modèle de segmentation guidée par prompt : il ne fait pas de détection d’objets autonome (pas de bounding boxes classifiées).

Or SAM2 ne fait pas de détection multi-classes ou de classification, donc nous nous sommes donc uniquement intéressés à des modèles de détection.
</p>

<p align="center">
  <img src="Images_Readme\3_SAM2.png"
  width="80%"/><br>
  <em>Figure 5 – Test de segmentation via SAM2</em>
</p>
<br>

<p align="justify">
Nous avons par la suite testé le modèle <b>YOLOv8</b> pré-entraîné sur COCO. Il s'agit d'un modèle de détection d’objets temps réel entrainé sur 80 classes présente dans le dataset COCO.

Néanmoins en raison de faibles performances sur des domaines spécifiques et de l'absence de classes "poule" et "renard" nous avons décliné cette option.
</p>

<p align="center">
  <img src="Images_Readme\3_YOLO8.png"
  width="100%"/><br>
  <em>Figure 6 – Test de détection via YOLO8 pré-entrainé sur le dataset COCO</em>
</p>

### B. Les modèles entrainés
Nous avons finalement sélectionné 3 modèles pour l'entrainement : 
 <ul>
  <li>YOLO 11 : l'entrainement se trouve dans <code>YOLO\Inference_YOLO_11.ipynb</code></li>
  <li>YOLO 26 : l'entrainement se trouve dans <code>YOLO\Inference_YOLO_26.ipynb</code></li>
  <li>RF-DETR : l'entrainement se trouve dans <code>RF-DETR\RFDETR.ipynb</code></li>
</ul>

<p align="justify">
Chaque modèle a été entrainé sur l'ensemble <i>d'entrainement</i> et des métriques et résultats ont été déduits des ensembles de <i>validation</i> et de <i>test</i>.

Afin de tester la robustesse de nos modèles face à de nouvelles images, des tests on été réalisés sur deux "types" d'images provenant de sources extérieures au dataset :
<ul>
  <li>Un test sur des images avec <b>d'autres volatiles</b> que des poules (ici des oies) : les ressources images sont <code>Images_test\Poules_Oie.png</code> et <code>Images_test\Poules_Oies_2.png</code></li>

  <li>Un test sur des images avec <b>beaucoup de poules</b> pour véfifier si le modèle peut détecter plus de 3/4 poules : les ressources images sont <code>Images_test\Poules_Multiple.png</code> et <code>Images_test\Poules_Nombreuses.png</code></li>
</ul>

</p>

Les scripts d'entrainement s'articulent selon :
```text
Entrainement ──▶ Load des données et entrainement
             ├─▶ Métriques d'évaluation

Validation ────▶ Métriques de validation

Test ──────────▶ Métriques de test
             ├─▶ Test sur des images du dataset
             ├─▶ Test sur des images hors dataset
```

---
## 4. Conclusion
<p align="justify">
<i>Quelle est donc la meilleure solution pour nos poules ?
</i>

Afin de tester la robustesse de nos modèles face à de nouvelles images, des tests on été réalisés sur deux "types" d'images provenant de sources extérieures au dataset :
<ul>
  <li><b>YOLO-11</b> est le modèle le plus rapide et le moins coûteux, a de très bons résultats sur les images de test du dataset mais il manque de rappel : il ne détecte pas tous les oiseaux (difficulté de généralisation sur des images hors dataset).</li>
  
  <li><b>YOLO-26</b> a les scores de précision le plus faible et génère des faux positifs, notamment en « hallucinant » des renards, ce qui peut entraîner des alertes inutiles.</li>

  <li><b>RF-DETR</b> offre les meilleures performances globales, mais son implémentation est nettement plus coûteuse en ressources (30 Go contre 10Go pour YOLO-11 et 12Go pour YOLO-26).</li>
</ul>

</p>

<p align="center">
<i>Merci d'avoir fait appel à <b>Poul-ai-ller</b> !!!</i>
</p>
<p align="center">
  <img src="Images_Readme\logo_poulailler.jpg" width="60%"/>
  <br>
</p>

<br>
<p align="center">
Et merci pour cette année 😉💚
</p>