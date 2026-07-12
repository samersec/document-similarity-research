## 12/07/2026 — Test Jaccard sur documents réalistes (EPI original vs paraphrase)

Objectif : valider le comportement de Jaccard sur un cas proche du terrain.

Documents : datasets/corpus-synthetique/doc_original.txt et doc_paraphrase.txt
(même sujet - port des EPI -, vocabulaire largement différent).

Résultat : Score Jaccard = 23,93% (39 mots communs / vocabulaire de 91 et 111 mots)

Interprétation : confirme empiriquement la limite théorique identifiée -
Jaccard sous-estime fortement la similarité de documents reformulés, même
quand le sujet et les idées sont identiques.

Piste d'amélioration identifiée (non testée) : suppression des stopwords
avant calcul. Hypothèse : le score baisserait encore, car une partie des
mots communs actuels sont des mots grammaticaux (aux, avant, chaque...)
qui gonflent artificiellement l'intersection sans refléter le contenu réel.