## Jaccard

**Principe** : J(A,B) = |A∩B| / |A∪B|, sur les ensembles de mots uniques.

**Complexité** : O(n), très rapide.

**Avantages** : simple, déterministe, explicable, insensible à la réorganisation.

**Limites** :
- Insensible au sens : échoue sur la paraphrase et les synonymes
- Sensible aux stopwords qui gonflent artificiellement le score de manière
  non pertinente pour le contenu

**Résultat empirique** :
| Cas de test | Score Jaccard |
|---|---|
| Phrase incluse dans une autre | 67% |
| Paraphrase pure (mêmes idées, mots différents) | 15% |
| Document QHSE réel vs sa reformulation (91 vs 111 mots) | 23,93% |

**Conclusion** : Jaccard seul est insuffisant pour répondre au besoin du
cahier des charges ("robuste aux reformulations"). Utile en complément
d'autres méthodes (ex. détection rapide de quasi-duplication brute), pas
comme méthode unique.

**Amélioration possible non testée** : suppression des stopwords avant calcul.


## Levenshtein 

**Principe** : nombre minimal d'opérations (insertion/suppression/substitution)
au niveau caractère pour transformer un texte en un autre.

**Complexité** : O(n×m) — coûteux sur des documents longs.

**Avantages** : précis pour comparer des chaînes courtes (identifiants,
titres, correction orthographique).

**Limites** :
- Travaille au niveau caractère, pas au niveau mot/sens → mauvaise
  gestion de la paraphrase (un synonyme plus long = distance énorme)
- Coût de calcul élevé sur documents longs, peu adapté à l'industrialisation
- Score peu explicable sur du texte long (contrairement à Jaccard)

**Résultat empirique** :
| Cas de test | Score |
|---|---|
| Document QHSE original vs paraphrase (même corpus que Jaccard) | 33,71% |

**Conclusion** : score plus élevé que Jaccard sur ce cas, mais ce n'est pas
un signe de meilleure performance — c'est un artefact du comptage au niveau
caractère (chevauchements fortuits), sans réelle interprétabilité. Non
recommandé pour la comparaison de documents entiers.