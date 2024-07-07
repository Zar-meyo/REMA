# REMA

Lors de mes recherches pour effectuer ce projet, je suis tombé sur [cet article](https://aclanthology.org/W03-1103.pdf)
sur les systèmes hybrides pour la recommandation.
Ce système permettait de résoudre le problème du "cold start" entre autre.

Le système est découpé en deux parties, la partie collaborative et la partie basée sur le contenu.

J'ai commencé par la recommandation en fonction du contexte du film, j'ai choisi pour cela de fusionner les informations
des deux datasets disponibles pour récupérer les genres, les noms des personnes ayant travaillé sur le film, si le film
tout public, sa durée ainsi son année de sortie.
j'ai choisi le KNN pour cette partie, modèle simple, mais puissant, pour trouver les similarités entre les films en se basant sur les
caractèristiques listées avant.
J'ai pré-traité les données pour qu'il n'y ait plus que des types numériques, en transformant la liste de genre en créant
une colonne de booléen par genre. Il a cependant été impossible de faire pareil pour l'équipe du film, car s'il existe
moins de 20 genres de films, il y a plusieurs milliers de noms dans le dataset. J'ai donc choisi de garder seulement le
nombre de personnes pour différencier un film gros budget avec une grosse équipe qu'un indépendant avec une équipe réduite.

La deuxième partie se base sur SVD, modèle capable de capturer les relations latentes entre utilisateurs et items. 
Pour l'utiliser, je récupère les notes données par chaque utilisateur aux films. 
Puis avec la librarie `scikit-surprise`, j'entraîne le modèle SVD, j'ai utilisé cette librarie car l'implémentation à la
main de cette méthode mettait trop de temps à tourner sur mon ordinateur. En découpant les données en 80% entraînement,
20% test, j'obtiens un RMSE de 0.88.

Une fois les deux parties implémentées, j'ai combiné les deux pour créer mon système de recommandation pour deux utilisateurs.
Je commence par récupérer les films qui n'ont pas été vus par le couple, puis je note chaque film avec les deux méthodes
de recommandation pour obtenir un score, je mélange ensuite ces scores (pondérés avec des points) afin de calculer un score
final. Ce score final est utilisé pour recommander les 10 films qui seront le plus à même de plaire au couple.