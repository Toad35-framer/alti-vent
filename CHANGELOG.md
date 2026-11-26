# Changelog Alti-Vent

## v3.3.0 — Novembre 2026 — Soutien Ko-fi

### Ajouts
- Bouton discret **« ☕ Payer un coup »** en pied de page, à côté de
  « Tester les accès IGN » : ouvre la page Ko-fi de l'auteur
  (ko-fi.com/jeanpineau) dans un nouvel onglet
- Petite fenêtre de remerciement, **non bloquante**, toutes les
  10 ouvertures de l'outil (10, 20, 30…) : un message tiré au hasard
  parmi six, avec les boutons « Payer un café » et « Plus tard »
  (la touche Échap la ferme aussi)

### Confidentialité
- Le nombre d'ouvertures est compté **uniquement dans votre navigateur**
  (`localStorage`, clé `nb_utilisations_alti-vent`) ; aucune donnée
  n'est envoyée à un serveur
- En navigation privée ou sans stockage local : ni compteur, ni fenêtre

### Inchangé
- Calculs EC1, analyse, vue 3D, exports et accès IGN (v3.2)


## v3.2.0 — Novembre 2026 — Correction automatique des altitudes littorales

En bord de mer, le MNT IGN ne renvoie pas toujours la valeur « mer »
habituelle (−99999). Certaines zones non couvertes reviennent avec
−9999, −1000 ou d'autres valeurs aberrantes, que l'outil prenait pour
du vrai terrain : pentes faussées dans la rosace, puits dans la vue 3D.

### Correctif — même règle pour la rosace et la vue 3D
- **Zones non couvertes** : toute altitude inférieure à −100 m est
  reconnue comme hors couverture et traitée comme mer à 0 m (le point
  le plus bas de France métropolitaine est vers −10 m)
- **Bord de mer** : une altitude négative est recalée à 0 m si elle est
  à moins d'environ 150 m de la mer, ou si elle appartient à une bande
  négative continue qui rejoint la mer (estran, vasière)
- **Vraies dépressions conservées** : une zone négative séparée de la
  mer par du terrain ≥ 0 m (Camargue, polder fermé) n'est pas modifiée ;
  un message la signale et laisse l'utilisateur juger

### Transparence des corrections
- Fin d'analyse : nombre de points recalés et de points hors couverture
  à valeur atypique
- Fenêtre « Point du profil » : valeur reçue de l'IGN affichée
  (ex. « Hors couverture IGN (valeur reçue : −9999 m) »)
- Tableau d'altitudes 3D : cases recalées en **bleu**, valeur IGN
  d'origine au survol ; « Réinitialiser aux valeurs IGN » annule la
  correction, « Recalculer le maillage » la réapplique
- Les boutons de correction manuelle (rosace et 🌊) restent disponibles

### Inchangé
- Calculs EC1 (pentes, cas 2/3/4, H, Lu, X, ΔAc) hors zones littorales
- Accès Géoplateforme et diagnostic « Tester les accès IGN » (v3.1)


## v3.1.0 — Novembre 2026 — Accès Géoplateforme fiabilisés + vue 3D

Le site Géoportail a été remplacé par cartes.gouv.fr. Les services de
données IGN (data.geopf.fr) restent en place mais appliquent une limite
stricte de 5 requêtes par seconde : au-delà, réponse HTTP 429 et blocage
de 5 secondes, ce qui interrompait l'analyse.

### Accès IGN (correctif)
- Tous les accès IGN regroupés dans un bloc unique (`GPF`) en tête de script
- File d'attente commune à l'altimétrie (profils EC1 et maillage 3D),
  cadence limitée à environ 3 requêtes par seconde
- Nouvelles tentatives automatiques sur limite de débit (429), panne
  serveur ou coupure réseau, avec délai maximal par requête
- Contrôle du format des réponses altimétriques avant tout calcul
- Repli automatique en méthode POST si le service refuse GET (HTTP 405)
- Tuiles WMTS : délai maximal de 8 s, repli sur le nom de pyramide
  `PM_0_19` ; un fond de carte en échec ne bloque plus l'analyse
- Messages d'erreur explicites dans la zone d'état
- Nouveau bouton **« Tester les accès IGN »** en pied de page : teste
  chaque service depuis le poste de l'utilisateur

### Liens
- Lien « Géoportail » de la fenêtre *Point du profil* remplacé par un
  lien **cartes.gouv.fr**

### Nouveautés de la v3 (première publication sur ce dépôt)
- **Vue 3D** du terrain (Three.js r128) : maillage paramétrable, fond de
  carte drapé, rosace suivant le relief, coupes de profil
- **Recherche par adresse** et géocodage inverse (API Géoplateforme)
- **Épingle de repositionnement** du site sur la carte, avec recalcul
  complet de l'orographie

### Inchangé
- Calculs EC1 (pentes, cas 2/3/4, H, Lu, X, ΔAc) : résultats identiques
  à la v3 sur les mêmes données altimétriques


## v2.3.0 — Mai 2026 — Polissage final + bascule licence CC BY-NC-ND 4.0

### Licence
- **Changement de licence** : passage de **MIT** à **Creative Commons
  BY-NC-ND 4.0** (Attribution, Pas d'utilisation commerciale, Pas de
  modification)
- Logo CC BY-NC-ND inline (SVG vectoriel) dans le footer + lien vers le
  texte officiel
- Mention de licence enrichie dans les exports SVG et DOCX (avec badges
  colorés dans le DOCX)
- Rappel de licence dans la dernière étape du tutoriel onboarding

### Accessibilité (Lot 7.2)
- Style `:focus-visible` global (outline bleu + box-shadow, invisible
  à la souris)
- 22 `aria-label` sur tous les éléments interactifs
- 5 zones `aria-live=polite` pour les statuts dynamiques
- Rayons SVG de la rosace : focalisables (Tab), activables (Entrée/Espace),
  `aria-label` détaillé par rayon
- `role=radiogroup` + `aria-checked` synchronisé sur basemap-toggle
- Classe `.sr-only` disponible pour textes lecteur d'écran

### Identité visuelle (Lot 7.3)
- Favicon SVG inline : rose des vents + relief stylisé + flèche Nord
  jaune doré (aux couleurs du brand)

### Microcopy (Lot 8.3)
- Tournures condescendantes retirées
- Messages d'erreur reformulés en messages utiles (avec exemple correct)
- Libellés boutons harmonisés ("Exporter…" partout)
- Footer enrichi du disclaimer obligatoire en italique ambre
- Fiche ΔAc complémentaire reformulée pour un ton plus pédagogique


## v2.2.x — Mai 2026 — Vagues d'enrichissement

- Animation vent avec modèle physique de couche limite (18 lignes,
  α(h) = 1 − (h/h_max)^0.7)
- Légende couleurs rosace (vert <5 % / orange 5-9 % / rouge >9 %)
- Pente affichée en % puis en ° (cohérent EC1)
- Suppression de la suggestion automatique de cas EC1 (pédagogie active)
- Pentes par rayon (EC1 R/2) au-dessus des coupes — cohérence rosace
- Échelles altimétriques latérales recalées sur les bornes réelles
  Chart.js (`chart.scales.y.min/max`)
- Export DOCX entièrement refondu : texte natif + 3 images enrichies
  (rosace + profils avec pentes par rayon + échelles altimétriques)
- ΔAc complémentaire (hors norme) sur les 4 directions sélectionnées


## v2.0.0 → v2.2.0 — Mai 2026 — Roadmap V2 (lots 1 à 6)

- Lot 1 : refonte UI (badges étapes, paste-input, slider rayon)
- Lot 2 : pédagogie (tooltips, glossaire, "Pourquoi ?", "Le saviez-vous ?")
- Lot 3 : profil et analyse (fiches résumées, arbre de décision,
  animation vent, popup détails point, mini-flèche pente)
- Lot 4 : rosace et interactions (hover, surbrillance, alerte altitudes
  aberrantes, icônes fonds de carte)
- Lot 5 : tableau (tri par colonne, filtres, surbrillance 4 directions,
  CSV intégré)
- Lot 6 : confort utilisateur (raccourcis clavier, localStorage,
  onboarding 6 étapes, barre de progression)


## v1.0.0 — Mai 2026 — Première version publique

Outil web complet pour l'analyse orographique selon NF EN 1991-1-4.

### Fonctionnalités principales
- Rosace orographique avec API IGN Géoplateforme (RGE ALTI® 1 m)
- Fonds de carte multiples : photo aérienne, plan IGN, analyse hauteur
  (heatmap altimétrique), profil OSM (catégories de terrain), aucun fond
- Sélection manuelle du cas EC1 (02, 03, 04) avec aide visuelle (schémas norme)
- Profils-diamètre principal + perpendiculaire avec azimuts et cotes d'altitudes
- Calcul des paramètres H, Lu, X (X signé) sur les 2 diamètres
- Méthode ΔAc complète pour le cas 02 (Annexe Nationale)
- Catégories de terrain (faisceau ±15° via OpenStreetMap / Overpass API)
- Correction littorale automatique + bouton manuel pour altitudes < 0 m
  aberrantes
- Exports CSV (synthèse par direction), SVG (page A4) et DOCX (note de calcul)
- Champ "coller GPS" multi-format
- Disclaimer pédagogique intégré dans l'outil et tous les exports
