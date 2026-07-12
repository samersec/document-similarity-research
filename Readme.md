# Évaluation de la similarité de contenu entre documents

Stage de recherche — Groupe Imagine Human, Direction Innovation
Durée : 1,5 à 2 mois

## Contexte

Les documents du groupe (procédures QHSE, rapports, réponses clients) présentent
fréquemment des versions multiples, des duplications partielles ou des copies
adaptées. Il n'existe aujourd'hui aucun outil interne permettant de mesurer
objectivement le degré de similarité de contenu entre deux documents ou plus.

## Problématique

Comment concevoir et implémenter une méthode fiable, explicable et
industrialisable permettant d'évaluer un pourcentage de similarité de contenu
entre documents, indépendamment de leur format d'origine, et robuste aux
reformulations, réorganisations et variations mineures de mise en forme ?

## Objectifs du stage

- Étudier plusieurs familles d'approches (lexicale, statistique, sémantique,
  structurelle, hybride) et documenter leurs avantages/limites.
- Sélectionner et justifier l'approche optimale au regard de la précision,
  du coût de calcul et de l'explicabilité.
- Implémenter un prototype fonctionnel (backend de calcul + interface front)
  permettant d'importer des documents, lancer une comparaison et visualiser
  les résultats.
- Livrer une documentation technique permettant une reprise ou une
  industrialisation ultérieure.

## Méthodologie de travail

Le projet suit une démarche de recherche en 8 phases, aucune implémentation
n'étant lancée avant validation du choix de méthode (fin de Phase 7).

| Phase | Nom | Statut |
|---|---|---|
| 1 | Compréhension du sujet | ✅ Terminée |
| 2 | État de l'art | 🔜 En cours |
| 3 | Sélection des méthodes candidates | ⬜ À venir |
| 4 | Protocole expérimental | ⬜ À venir |
| 5 | Expérimentations | ⬜ À venir |
| 6 | Analyse des résultats | ⬜ À venir |
| 7 | Choix de l'approche finale | ⬜ À venir |
| 8 | Conception & implémentation de l'application | ⬜ À venir |

## Livrables attendus

- Document de synthèse comparant les approches étudiées (méthodologie +
  résultats chiffrés)
- Prototype fonctionnel (backend + frontend), déployable en conteneur Docker
- Code source versionné avec documentation d'installation et d'utilisation
- Documentation technique de l'architecture et des choix de conception
- Support de présentation de fin de stage

## Structure du dépôt

```
.
├── README.md
├── docs/
│   ├── etat-de-l-art.md
│   ├── decisions-techniques.md
│   ├── protocole-experimental.md
│   ├── journal-recherche.md
│   └── comptes-rendus/
├── notebooks/
├── datasets/
│   └── corpus-synthetique/
├── src/
└── .gitignore
```

## Suivi

Points d'avancement hebdomadaires. Voir `docs/comptes-rendus/` pour l'historique
et `docs/journal-recherche.md` pour le détail des séances de travail.