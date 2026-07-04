# Synergy Map

Cartographie locale de votre réseau de contacts avec détection visuelle des
synergies business (graphe interactif).

## Lancer le projet

```bash
npm install
npm run dev
```

Puis ouvrir http://localhost:5173

## Stack

- Vite + React 18 + TypeScript + Tailwind CSS 4
- Cytoscape.js (+ layout fcose) pour le graphe
- Zustand + LocalStorage pour la persistance (clé `synergy-map-storage`)
- PapaParse pour le CSV, parseur vCard maison

## Fonctionnalités

- Import `.vcf` / `.csv` (mapping auto des colonnes nom/prénom/email/téléphone,
  dédoublonnage par email), ajout manuel, jeu de démonstration.
- Enrichissement business : Secteur (unique), Cibles (multi), Besoins (multi).
  Les contacts non enrichis apparaissent en gris pointillé.
- Moteur de synergies (liens dérivés, jamais stockés) :
  - **Règle 1 — Partage de cible** (pointillé ambre, non orienté) : même cible,
    secteurs différents.
  - **Règle 2 — Chaîne de valeur** (flèche indigo, orienté A→B) : le secteur de A
    répond à un besoin de B selon la matrice éditable (bouton « Taxonomie »).
- Filtres par type de synergie et focus par secteur ; masquage manuel de liens.
- Export JSON de sauvegarde.

## Architecture

```
src/
├── types.ts               # Modèle de données
├── data/defaults.ts       # Taxonomie par défaut + démo
├── engine/synergies.ts    # Moteur de règles (pur, testable)
├── store/useStore.ts      # Zustand + persistance LocalStorage
├── import/vcfParser.ts    # Parseur vCard
├── import/csvParser.ts    # Parseur CSV (PapaParse)
└── components/            # GraphCanvas, SidePanel, modales
```

## v2 — Profil réseau & mises en relation

- Vocabulaire libre : ajoutez secteur/cible/besoin directement depuis la fiche contact (« Autre… »).
- Champs Profession et Société.
- **Apports au réseau** : ce que chacun peut offrir. Règle 3 (flèche verte) :
  l'apport de A correspond au besoin de B.
- **Relations « connaît »** : déclarez qui connaît qui (traits gris). Le moteur
  suggère alors des mises en relation de degré 2 : « A peut vous présenter X ».
- Fiche « C'est moi » : marquez votre profil pour voir vos opportunités.
- Invitations : email / SMS / copie d'un message présentant le concept.
- Import depuis le carnet d'adresses du téléphone (Contact Picker — Chrome
  Android en https/localhost uniquement ; sinon export .vcf).
