# Famille hybride — ce qu'on en pense

## L'idée, racontée simplement

Après avoir testé toutes les familles précédentes, un constat revient
toujours : chaque méthode a un vrai point fort... et un vrai point faible.
Jaccard est rapide et honnête, mais aveugle au sens. SBERT comprend le sens,
mais reste une boîte noire et coûte plus cher en calcul.

L'idée hybride part d'une question toute simple : *et si on ne choisissait
pas entre les deux ?* Plutôt que de sacrifier un avantage pour en gagner un
autre, on les fait travailler ensemble. Un peu comme un mécanicien qui
utilise à la fois un outil rapide pour un premier diagnostic et un outil
plus précis pour confirmer — pas l'un ou l'autre, les deux, chacun à sa
place.

## Comment ça marche, concrètement

On prend le score de deux méthodes (par exemple Jaccard et SBERT) et on les
mélange, avec un curseur qui dit combien on fait confiance à chacune :

```
score final = (poids × score Jaccard) + ((1 − poids) × score SBERT)
```

Avec nos vrais résultats, si on donne 30% de confiance à Jaccard et 70% à
SBERT :

```
0,3 × 23,93 % + 0,7 × 88,24 % = 68,95 %
```

Ce n'est pas un chiffre magique — c'est juste un exemple pour montrer le
mécanisme. Le vrai travail sera de trouver le bon dosage, pas de deviner un
nombre au hasard.

## Pourquoi c'est plus malin que de juste garder "le meilleur score"

On a déjà eu cette discussion : SBERT gagne largement sur le score brut. Mais
un score plus haut ne veut pas dire une meilleure solution dans l'absolu.
Garder un peu de Jaccard dans le mélange, c'est garder :

- de la **vitesse** — un filtre rapide avant de lancer quelque chose de plus
  lourd sur beaucoup de documents
- de la **transparence** — pouvoir montrer à quelqu'un "voici les mots
  exacts en commun", plutôt qu'un chiffre sorti d'un modèle qu'on ne peut
  pas vraiment expliquer
- un **filet de sécurité** — si SBERT se trompe sur un cas particulier
  (vocabulaire très technique, sigle spécifique au métier), l'autre méthode
  peut rattraper le coup

## Ce qu'il faut retenir

L'hybride n'est pas une nouvelle méthode à tester comme les autres — c'est
plutôt une façon de **combiner intelligemment** ce qu'on a déjà appris des
autres familles.