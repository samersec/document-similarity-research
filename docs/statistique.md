Méthode statistique (TF-IDF + similarité cosinus) :

Étude et implémentation d'une pondération des mots fondée sur leur fréquence documentaire et leur rareté dans le corpus, en rupture avec l'approche lexicale où chaque mot avait un poids uniforme
Score obtenu : 59,51 % sur le corpus QHSE de test, en nette progression par rapport aux résultats lexicaux (23,93 % à 33,71 %)
Analyse critique du résultat : la progression du score s'explique par le mécanisme mathématique du cosinus et la prise en compte de la fréquence des termes, non par une compréhension sémantique — la méthode demeure une approche bag-of-words, insensible aux synonymes et au contexte
Limite méthodologique identifiée : la faible taille du corpus de test (2 documents) restreint la portée discriminante de l'IDF ; validation à confirmer sur un corpus élargi
Conclusion : approche statistiquement plus pertinente que le lexical, mais insuffisante en l'état pour répondre à l'exigence de robustesse aux reformulations — retenue comme baseline de référence et candidate pour une approche hybride ultérieure


