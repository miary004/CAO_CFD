README – Jour 4 : Analyse thermique d’un dissipateur (avec et sans congés)
Objectif

Réaliser une simulation thermique stationnaire sur un dissipateur à ailettes pour visualiser la distribution de température lorsqu’une source chaude (80 °C) est appliquée à sa base, avec convection naturelle sur les faces exposées à l’air ambiant (20 °C).
Outils utilisés

    FreeCAD 1.0.2 – Atelier FEM, solveur CalculiX (analyse thermomécanique couplée)

    Matériau : Aluminium 6061‑T6 (propriétés issues de la bibliothèque FreeCAD)

Démarche et difficultés rencontrées
1. Première tentative – Modèle avec congés (Fillet002)
    Géométrie : dissipateur comportant des congés de raccordement entre ailettes et base.
    Maillage : tétraédrique du second ordre, taille ≈ 5 mm.
    Résultat : échec – CalculiX a renvoyé l’erreur nonpositive jacobian determinant in element 143716 (image vokoscreenNG-2026-02-18_16-17-09.954.png).

Cause identifiée : les congés créent des zones de forte courbure. Avec un maillage trop grossier, certains éléments se distordent (jacobien négatif), ce qui bloque le solveur.
2. Deuxième tentative – Modèle simplifié (sans congés)
    -Suppression des congés → arêtes vives, géométrie plus facile à mailler.
    -Mêmes conditions aux limites et même matériau.
    -Succès : le calcul aboutit, les résultats sont cohérents (images vokoscreenNG-2026-02-18_16-05-21.048.png et vokoscreenNG-2026-02-18_16-06-26.029.png).

Conditions aux limites appliquées
    -Température imposée (face inférieure) : 80 °C (353,15 K)
    -Convection sur toutes les faces externes (ailettes + faces latérales) :
    -Coefficient d’échange h = 10 W/(m²·K)
    -Température ambiante T_inf = 20 °C (293,15 K)
    -Température initiale (optionnelle) : 20 °C

Résultats obtenus (modèle sans congés)
Grandeur	Valeur
Température maximale	80 °C (base imposée)
Température minimale	environ 45 °C (extrémité des ailettes)
Gradient thermique	Régulier le long des ailettes (visualisation sur les captures)

vokoscreenNG-2026-02-18_16-06-26.029.png
Figure 1 – Distribution de température (échelle en Kelvin)

vokoscreenNG-2026-02-18_16-05-21.048.png
Figure 2 – Visualisation complémentaire (peut-être flux ou coupe)*

Interprétation
    -La chaleur se propage de la base vers les extrémités des ailettes.
    -La température diminue progressivement grâce à la convection naturelle.
    -La conception sans congés est fonctionnelle et permet de valider le comportement thermique global.
    -Les congés pourront être réintroduits ultérieurement avec un maillage localement raffiné pour améliorer la précision.

Enseignements du jour
    -La qualité du maillage est cruciale – des éléments distordus (jacobien négatif) bloquent le solveur.
    -Simplifier la géométrie est une stratégie efficace pour obtenir un premier résultat valide.
    -L’analyse thermomécanique couplée de FreeCAD (via CalculiX) nécessite la masse volumique, même pour un problème purement thermique.
    -Savoir interpréter les erreurs (code -11, jacobien négatif) permet de les corriger rapidement.

Fichiers du projet
    -Dissipateur_conge.FCStd – version avec congés (ne tourne pas)
    -Dissipateur.FCStd – version sans congés (simulation réussie)
    -Captures d’écran des résultats
