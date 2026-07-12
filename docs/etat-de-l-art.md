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