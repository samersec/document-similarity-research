# Conclusion — Famille des méthodes lexicales

*Synthèse des 3 méthodes étudiées : Jaccard, Levenshtein, MinHash/k-shingling*

## Rappel des résultats empiriques

Testées sur le même corpus (procédure QHSE originale vs sa reformulation
complète, même sujet et mêmes idées) :

| Méthode | Unité comparée | Score obtenu |
|---|---|---|
| Jaccard | Mots isolés (sans ordre) | 23,93 % |
| Levenshtein | Caractères (avec position) | 33,71 % |
| MinHash (k=2) | Paires de mots consécutifs (avec ordre) | 3,12 % |

Les trois méthodes ont un objectif commun (mesurer une similarité de texte),
mais des unités de mesure différentes — ce qui explique l'écart important
entre les scores, sans qu'aucune ne soit "fausse" : chacune répond à une
définition différente et plus ou moins stricte de "similaire".

## Ce que ces méthodes capturent bien

- La **duplication brute** : copier-coller identique ou quasi identique
- Un **premier filtre rapide** avant un traitement plus coûteux
- Des cas d'usage précis hors comparaison de documents entiers : correction
  orthographique, comparaison d'identifiants ou de titres courts
  (Levenshtein), déduplication à très grande échelle (MinHash)

## Ce que ces méthodes ne capturent pas

- Le **sens** d'un texte : aucune des trois ne comprend qu'un mot peut être
  remplacé par un synonyme ou une tournure différente sans changer le fond
- La **paraphrase** : sur des documents qui disent la même chose avec des
  mots différents, les trois méthodes sous-estiment fortement la similarité
  réelle (3 % à 34 % alors que le contenu est quasi identique)
- Le **contexte** : aucune ne relie les mots à leur sens dans la phrase

