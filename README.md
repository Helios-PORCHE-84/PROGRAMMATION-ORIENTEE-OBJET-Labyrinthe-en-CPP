# PROGRAMMATION ORIENTEE OBJET Labyrinthe en C++

Contexte & Objectif : 

Projet individuel (L1, UE Programmation Orientée Objet / Mini-projet). Création d'un jeu de labyrinthe en mode console avec génération aléatoire, déplacement manuel et résolution automatique. 


Détails Techniques & Concepts Clés :

Affichage : Utilisation de la bibliothèque <ncurses.h> pour le rendu graphique en mode texte et la capture des entrées clavier.

Structures de données : Tableaux dynamiques pour la grille (Case), et structure de Pile (LIFO) pour les algorithmes de parcours.

Algorithmique : Parcours en profondeur (DFS) pour la génération de labyrinthes parfaits et la recherche de chemin. 


Cheminement de réalisation (Synopsis) :

Initialisation et I/O : Création des fonctions pour lire un labyrinthe depuis un fichier texte (format matriciel avec portes W, N, S, E) et l'écrire dans un fichier.

Moteur de jeu (Déplacement) : Implémentation de la boucle de jeu capturant les flèches directionnelles. Vérification des collisions avec les murs et marquage du chemin parcouru (étoiles).

Génération Aléatoire (My_Creation) : Algorithme partant de (0,0), empilant les cases visitées, ouvrant des portes vers des voisins non visités aléatoires, et dépilant (backtracking) en cas d'impasse.

Résolution Automatique (explorer) : Utilisation d'une seconde pile pour explorer le labyrinthe de (0,0) jusqu'à la sortie (p-1, q-1) en marquant et démarquant les cases pour trouver le chemin optimal.

Gestion de l'abandon : Association de la touche F1 pour afficher la solution (le chemin de la pile) et F2 pour réinitialiser la partie.

