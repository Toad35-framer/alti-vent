# Changelog Alti-Vent

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
