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

## MinHash / k-shingling

**Principe** : estimation rapide du score de Jaccard sur des k-shingles
(séquences de k mots consécutifs) via des signatures de hachage compactes,
sans comparer les ensembles complets.

**Complexité** : O(n) pour la génération de signature, comparaison de
signatures quasi instantanée — conçu pour la scalabilité (grands volumes de
documents).

**Avantages** : capture l'ordre des mots (contrairement à Jaccard classique),
scalable à grande échelle, mémoire réduite.

**Limites** :
- Résultat approché, pas exact
- Très sévère sur la paraphrase (score plus bas encore que Jaccard classique)
- Complexité de mise en œuvre supérieure (choix de k, nombre de permutations)

**Résultat empirique** :
| Cas de test | Score |
|---|---|
| Document QHSE réel vs sa reformulation (k=2) | 3,12 % |

**Conclusion** : confirme et amplifie la limite des méthodes lexicales face
à la paraphrase. Utile pour la détection de quasi-duplication à grande
échelle (ex. déduplication de masse), pas pour évaluer une similarité de
sens.

## TF-IDF + Similarité cosinus

**Principe** : chaque mot reçoit un poids selon deux facteurs — sa fréquence
dans le document (TF) et sa rareté dans l'ensemble du corpus (IDF). Chaque
document devient un vecteur de poids, comparé à un autre document via la
similarité cosinus (angle entre les deux vecteurs, indépendant de la
longueur des documents).

**Formule** :
- TF-IDF(w, d) = TF(w, d) × IDF(w), avec IDF(w) = log(N / n_w)
- similarité cosinus(A, B) = (A · B) / (‖A‖ × ‖B‖)

**Complexité** : faible, pas d'entraînement de modèle, calcul direct sur le
corpus.

**Avantages** :
- Meilleur rapport simplicité/pertinence que les méthodes lexicales
- Donne plus de poids aux mots rares et informatifs qu'aux mots génériques
- Reste explicable (les mots contribuant le plus au score sont identifiables)
- Pas d'entraînement lourd, léger à mettre en œuvre

**Limites** :
- Reste une approche bag-of-words : ignore le sens, un synonyme reste un mot
  différent
- Ignore l'ordre des mots et le contexte
- L'IDF perd sa pertinence sur un corpus de test trop petit (ex : 2
  documents seulement, comme dans nos premiers tests)

**Résultat empirique** :
| Cas de test | Score |
|---|---|
| Document QHSE original vs sa reformulation | 59,51 % |

**Conclusion** : nette amélioration par rapport aux méthodes lexicales
(23,93 % à 33,71 %), grâce à la pondération des mots et au mécanisme du
cosinus. Reste toutefois une approche de surface : le score plus élevé ne
traduit pas une compréhension du sens, mais un calcul mathématique plus
tolérant. Insuffisant seul face à l'exigence de robustesse aux
reformulations, mais bonne baseline intermédiaire avant le sémantique.

## SBERT (Sentence-BERT) — première méthode sémantique

**Principe** : modèle de réseau de neurones pré-entraîné sur des millions de
phrases, qui transforme un texte en un vecteur numérique représentant son
*sens* (pas juste ses mots). Deux textes de sens proche auront des vecteurs
proches, même avec un vocabulaire presque totalement différent. La
comparaison se fait ensuite avec la même similarité cosinus que pour TF-IDF.

**Modèle utilisé** : `paraphrase-multilingual-MiniLM-L12-v2` (modèle
multilingue, adapté au français, léger).

**Complexité / coût de calcul** : plus élevé que le lexical/statistique —
nécessite le chargement d'un modèle pré-entraîné (quelques centaines de Mo)
et une inférence par le réseau de neurones à chaque comparaison.

**Avantages** :
- Comprend le sens : reconnaît des synonymes et reformulations sans mot
  commun
- Robuste à la paraphrase, à la réorganisation, au changement de structure
- Modèles multilingues disponibles (utile si besoin de comparer des
  documents traduits)

**Limites** :
- Coût de calcul et mémoire plus élevés que les méthodes précédentes
- Moins explicable : impossible de dire "le score est de 88% à cause de tel
  mot", contrairement à Jaccard ou TF-IDF
- Dépend de la qualité et du domaine d'entraînement du modèle pré-entraîné
  (à vérifier sur du vocabulaire QHSE très spécifique)

**Résultat empirique** :
| Cas de test | Score |
|---|---|
| Document QHSE original vs sa reformulation | 88,24 % |

**Conclusion** : premier bond significatif de tous les tests réalisés
jusqu'ici. Confirme la capacité de cette famille à répondre à l'exigence
centrale du cahier des charges (robustesse aux reformulations). Score élevé
mais pas 100%, ce qui reste cohérent (les documents restent formulés
différemment, pas identiques en tout point).

## Famille structurelle 

**Principe** : contrairement aux autres familles qui comparent le contenu
textuel (mots, poids statistiques ou sens), la famille structurelle compare
l'**organisation** du document : titres, sous-titres, hiérarchie des
sections, ordre des parties. Deux documents peuvent être jugés similaires
s'ils suivent le même plan, même avec un contenu textuel différent — et
inversement, jugés différents malgré un contenu proche si leur organisation
diffère.

**Techniques associées** : comparaison d'arbres de sections (structure
hiérarchique des titres), analyse de graphes de dépendance syntaxique,
comparaison de gabarits/templates de documents.

**Avantages** :
- Détecte les documents copiés depuis un même template ou modèle, même si
  le contenu interne change
- Complémentaire aux autres familles : capture une dimension (l'organisation)
  qu'aucune des méthodes lexicale/statistique/sémantique ne regarde
- Pertinent pour des documents normés (procédures QHSE, rapports suivant un
  format imposé)

**Limites** :
- Ne dit rien sur le sens ou la pertinence du contenu : deux documents à la
  structure identique mais au contenu contradictoire seraient jugés
  similaires
- Nécessite une étape de parsing préalable (extraction fiable des titres et
  niveaux hiérarchiques depuis PDF/DOCX) — un prérequis technique en soi,
  absent des autres familles
- Peu pertinente seule ; s'utilise presque toujours en complément d'une
  méthode sémantique ou statistique

**Cas d'usage typique** : détection de documents dérivés d'un même template,
vérification de conformité à un format normé.

**Statut dans ce projet — non testée empiriquement.** Justification :
- Non identifiée comme axe prioritaire dans le cahier des charges officiel,
  qui met l'accent sur les familles lexicale, statistique et sémantique
- Nécessite un prérequis technique disproportionné par rapport au temps
  disponible sur le stage (1,5-2 mois) : extraction fiable de la structure
  documentaire avant toute comparaison possible
- Utilité principalement complémentaire (jamais solution unique), donc moins
  prioritaire qu'une méthode utilisable seule
- La théorie (principe, avantages, limites) reste documentée ici pour
  assurer une couverture complète de l'état de l'art, conformément à la
  méthodologie de recherche du stage