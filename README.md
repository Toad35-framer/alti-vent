# Alti-Vent — Rosace orographique selon NF EN 1991-1-4

Outil web d'analyse automatique du relief autour d'un site de construction pour la détermination du coefficient orographique **c₀(z)** selon la norme **NF EN 1991-1-4 §4.3.3 et Annexe A** (Eurocode 1 — actions du vent sur les structures).

> ⚠ **Avertissement** : Cet outil est fourni à des fins pédagogiques et d'assistance au pré-dimensionnement. Les résultats doivent être vérifiés par un professionnel qualifié.

---

## 🌬 Présentation

Pour calculer la pression dynamique de pointe du vent au sens de l'Eurocode 1, il faut vérifier si l'orographie du terrain (collines, falaises, escarpements) amplifie la vitesse du vent au-dessus du site. Cette vérification se fait **direction par direction** sur un rayon de 500 m à 1 km autour du site, en analysant la pente moyenne du terrain au vent.

**Alti-Vent** automatise toute cette analyse :

- Interrogation de l'API IGN Géoplateforme pour récupérer les altitudes du relief en haute résolution (1 m, RGE ALTI®)
- Tracé d'une **rosace** colorée de 12, 16 ou 24 directions selon la pente moyenne sur chaque azimut
- Identification automatique du **cas EC1 applicable** par direction (terrain plat / obstacles variés / colline / falaise)
- Calcul des paramètres géométriques **H, Lu, X** ou de l'altitude relative **ΔAc**
- Croisement avec les données **OpenStreetMap** (via Overpass API) pour estimer les catégories de terrain (0, II, IIIa, IIIb, IV) selon EC1
- Export d'une **page d'analyse** prête à insérer dans une note de calcul (SVG ou DOCX)
- **Vue 3D** du terrain avec fond de carte drapé et rosace suivant le relief
- **Recherche par adresse** et repositionnement du site d'un clic sur la carte

L'outil tourne **intégralement dans le navigateur** : pas de serveur, pas d'inscription, pas d'envoi de données. Il suffit d'un navigateur récent et d'une connexion internet (pour les appels API IGN et OSM).

---

## ✨ Fonctionnalités

### Carte et rosace
- Fond de carte **Photo aérienne IGN**, **Plan IGN**, **Analyse hauteur** (carte thermique d'altitude générée côté client), **Profil OSM** (catégories de terrain) ou **aucun fond**
- Rosace des pentes moyennes avec code couleur (vert < 3° / orange 3-5° / rouge > 5°)
- Bouton **"Corriger les altitudes < 0 m"** pour traiter les éventuelles valeurs aberrantes du MNT (zones côtières)

### Saisie du site
- Champs latitude / longitude séparés
- Champ **« coller GPS »** acceptant tous les formats cartes.gouv.fr, Google Maps, etc. (`48.5510, -2.7350`, `N48.5510 W2.7350`, `48,5510 ; -2,7350`...)
- Choix du rayon (500 m à 1 km), pas angulaire (15° à 45°), exemples préchargés

### Panneau Profil altimétrique
- Sélection manuelle du **cas de calcul EC1** (Cas 02, 03 ou 04) avec aide visuelle (schémas extraits de la norme)
- Affichage de **deux profils-diamètre** :
  - Diamètre principal (axe d'azimut sélectionné)
  - Diamètre secondaire (perpendiculaire)
- Cotes d'altitudes aux 5 points clés (−1000, −500, 0, +500, +1000 m)
- Surbrillance des 2 diamètres analysés sur la rosace
- **Toggle catégorie de terrain** (requiert pas angulaire = 15° pour la résolution EC1 ±15°)

### Analyse
- Pente moyenne sur chaque diamètre
- Pour le cas 02 : formule complète **Am = (2·Ac + AN1+AN2 + ... + AO2) / 10** avec les 8 valeurs cardinales détaillées
- Pour les cas 03/04 : paramètres **H, Lu, X signé** sur les 2 diamètres
- Pourcentages par catégorie EC1 (faisceau ±15°) basés sur les polygones OSM

### Exports
- **CSV** : synthèse par direction (1 ligne par azimut) + récap ΔAc global
- **SVG** : page d'analyse A4 portrait complète (rosace + 2 profils + paramètres + catégories)
- **DOCX** : version Word prête à insérer dans une note de calcul

---

## 🚀 Utilisation

### Option 1 — En ligne (recommandé)

L'outil est publié via GitHub Pages :

👉 **https://Toad35-framer.github.io/alti-vent/**

### Option 2 — En local

1. Téléchargez le fichier [`index.html`](./index.html) (ou clonez ce dépôt)
2. Ouvrez-le dans un navigateur récent (Chrome, Firefox, Edge, Safari)
3. Une connexion internet est nécessaire pour les requêtes vers l'API IGN et OSM

L'outil est entièrement contenu dans un seul fichier HTML : aucune installation, aucune dépendance à installer manuellement (Chart.js, JSZip et Three.js sont chargés via CDN à la volée).

---

## 🧭 Conformité normative

| Référence | Mise en œuvre |
|---|---|
| NF EN 1991-1-4 §4.3.3 | Seuil de pente moyenne 3° (≈ 5 %) pour déclencher le calcul de c₀ |
| NF EN 1991-1-4 Annexe A | Détermination de H, Lu, X selon Figure A.2 (colline) et Figure A.3 (falaise) |
| Annexe Nationale française | Méthode ΔAc pour le cas 2 (orographie variée), calcul sur 8 points cardinaux à 500 m et 1000 m |
| Catégories de terrain §4.3.2 | Faisceau ±15° autour de la direction du vent, classification 0 / II / IIIa / IIIb / IV |

**Limites connues** :
- La distinction colline en chaîne (cas 3) / colline isolée (cas 5) nécessite une vérification visuelle de la largeur perpendiculaire au vent (≥ 10·H pour chaîne)
- Les catégories de terrain sont estimées à partir des tags OSM (`landuse`, `natural`, `building`, `place`, `leisure`) avec une priorité de classification mer > centre-ville > urbain > forêt/village > campagne. Une vérification manuelle reste recommandée pour les sites particuliers
- Le clamp littoral automatique préserve les dépressions terrestres réelles ; pour les zones où le MNT renvoie des bathymetries aberrantes (-1000 m parfois en bord de mer), utiliser le bouton "Corriger les altitudes < 0 m"

---

## 📂 Structure du dépôt

```
alti-vent/
├── index.html              ← l'outil complet, version en cours (autonome, ~400 KB)
├── alti-vent-v3.1.0.html   ← archive de la version 3.1.0
├── alti-vent-v2.3.0.html   ← archive de la version 2.3.0
├── alti-vent-v1.0.0.html   ← archive de la version 1.0.0
├── CHANGELOG.md            ← historique des versions
├── LICENSE                 ← licence CC BY-NC-ND 4.0
└── README.md               ← ce fichier
```

---

## 🔧 Sources de données

- **IGN Géoplateforme — RGE ALTI®** : MNT haute résolution (1 m) sur la France métropolitaine, DROM et collectivités d'outre-mer
- **IGN Géoplateforme — WMTS** : photos aériennes (BD ORTHO) et fond Plan IGN
- **IGN Géoplateforme — Géocodage** : recherche par adresse et géocodage inverse

Les services IGN limitent le débit (5 requêtes par seconde pour l'altimétrie). En cas de doute sur un accès, le bouton **« Tester les accès IGN »**, en pied de page de l'outil, indique quel service ne répond pas.
- **OpenStreetMap** (tuiles + Overpass API) : occupation du sol pour les catégories de terrain
- **Norme NF EN 1991-1-4** : schémas illustrant les cas 02, 03, 04 (modal d'aide)

---

## 📜 Licence

[![Licence CC BY-NC-ND 4.0](https://licensebuttons.net/l/by-nc-nd/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.fr)

Cette œuvre est mise à disposition sous **licence Creative Commons
Attribution - Pas d'Utilisation Commerciale - Pas de Modification 4.0
International** ([CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.fr)).

Vous pouvez **partager** (copier et redistribuer) l'œuvre par tous moyens
et sous tous formats, à condition de :

- **Attribution (BY)** — créditer l'œuvre (Jean Pineau, Alti-Vent, NF EN 1991-1-4) et inclure un lien vers la licence
- **Pas d'utilisation commerciale (NC)** — usage non commercial uniquement
- **Pas de modification (ND)** — pas de distribution d'œuvre modifiée

Voir [LICENSE](./LICENSE) pour le texte complet.

> **Avertissement** — Cet outil est fourni à des fins pédagogiques et
> d'assistance au pré-dimensionnement. Les résultats doivent être
> vérifiés par un professionnel qualifié. L'auteur ne saurait être tenu
> responsable des conséquences d'une utilisation du logiciel pour un
> dimensionnement réel sans vérification indépendante.

---

## 👤 Auteurs

- **Jean Pineau** — Charpentier bois (Bretagne) — conception et spécifications
- **Claude** (Anthropic) — assistant de développement

---

## 🤝 Contribution

Les retours, suggestions et corrections sont bienvenus via les **Issues** GitHub ou en m'envoyant directement un message. Si vous identifiez un cas où l'outil donne un résultat erroné par rapport à un calcul manuel EC1, merci de joindre les coordonnées du site et la valeur attendue.
