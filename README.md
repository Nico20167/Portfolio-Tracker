# Portfolio Tracker — Trade Republic

Outil de suivi de portefeuille pour investisseurs **Trade Republic**, accessible depuis un navigateur. Aucun compte, aucun abonnement. Vos données restent dans votre navigateur.

---

## Fonctionnalités

| Onglet | Ce que vous obtenez |
|---|---|
| **Vue d'ensemble** | Valeur totale, plus-value, TWRR, MWRR, performance par période (1M / MTD / YTD / Max) |
| **Positions** | Détail par ligne : parts, prix moyen, valeur actuelle, gain/perte — cliquez sur un ETF pour voir son graphique d'achat |
| **Évolution** | Courbe de valorisation du portefeuille, comparaison avec le S&P 500 (base 100), évolution par ETF |
| **Répartition** | Exposition géographique et sectorielle consolidée — cliquez sur une part pour voir quels ETFs contribuent et les principales entreprises |
| **Projection** | Simulation de capital à 1–40 ans selon 3 scénarios de rendement (4 %, 7 %, 10 %) |

---

## Comment l'utiliser

### 1. Exporter depuis Trade Republic

1. Ouvrez l'application Trade Republic ou [app.traderepublic.com](https://app.traderepublic.com)
2. Allez dans **Profil → Documents → Exportation de transactions**
3. Sélectionnez la période souhaitée et téléchargez le fichier CSV

### 2. Importer et enrichir

1. Ouvrez l'outil et glissez votre CSV sur la zone de dépôt (onglet **Accueil**)
2. Cliquez sur **Actualiser données ETF** — l'outil récupère les prix historiques et la répartition géographique/sectorielle (~30 secondes pour 6 ETFs)
3. Naviguez dans les onglets — vos données sont sauvegardées dans votre navigateur

> **Confidentialité** : aucune donnée n'est stockée sur un serveur. Le backend ne fait que traiter votre CSV et récupérer des données publiques (cours boursiers). Tout est conservé dans le `localStorage` de votre navigateur.

---

## ETFs supportés

L'outil reconnaît automatiquement les indices répliqués à partir du nom de l'ETF (Trade Republic) et propose des données de répartition géographique et sectorielle pour :

- MSCI World, MSCI Emerging Markets, MSCI India, MSCI ACWI
- STOXX Europe 600, MSCI Europe, CAC 40
- S&P 500, NASDAQ-100, Russell 2000
- HSCEI China, Bloomberg Europe Defense

---

## Architecture

```
Navigateur (ce repo)              Backend (API séparée)
─────────────────────             ──────────────────────────
index.html                        FastAPI (Python)
favicon.png           ◄──────►    - Parse CSV
                                  - Enrichissement JustETF
Données : localStorage            - Calcul analytics
(rien sur le serveur)
```

Le frontend est un fichier HTML statique hébergé sur **GitHub Pages**.  
Le backend est hébergé séparément (Render, Fly.io, etc.).

---

## Déploiement

### Frontend (ce repo)

Activez **GitHub Pages** dans les paramètres du repo :  
`Settings → Pages → Source : main / root`

L'outil sera disponible à `https://votre-compte.github.io/nom-du-repo`.

### Backend

Voir le repo [`portfolio-api`](https://github.com/votre-compte/portfolio-api) pour les instructions de déploiement du backend FastAPI.

---

## Stack technique

- **Frontend** : HTML / CSS / JavaScript vanilla, [Chart.js](https://www.chartjs.org/)
- **Backend** : Python, FastAPI, pandas, [justetf-scraping](https://github.com/druzsan/justetf-scraping)
- **Données** : [JustETF](https://www.justetf.com) (cours et composition des ETFs)
- **Stockage** : `localStorage` navigateur (aucune base de données)
