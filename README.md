# PROGRAMMATION ORIENTEE OBJET Labyrinthe en C++.

# Contexte & Objectif : 

Projet individuel (L1, UE Programmation Orientée Objet / Mini-projet). Création d'un jeu de labyrinthe en mode console avec génération aléatoire, déplacement manuel et résolution automatique. 


# Détails Techniques & Concepts Clés :

Affichage : Utilisation de la bibliothèque <ncurses.h> pour le rendu graphique en mode texte et la capture des entrées clavier.

Structures de données : Tableaux dynamiques pour la grille (Case), et structure de Pile (LIFO) pour les algorithmes de parcours.

Algorithmique : Parcours en profondeur (DFS) pour la génération de labyrinthes parfaits et la recherche de chemin. 


# Cheminement de réalisation (Synopsis) :

Initialisation et I/O : Création des fonctions pour lire un labyrinthe depuis un fichier texte (format matriciel avec portes W, N, S, E) et l'écrire dans un fichier.

Moteur de jeu (Déplacement) : Implémentation de la boucle de jeu capturant les flèches directionnelles. Vérification des collisions avec les murs et marquage du chemin parcouru (étoiles).

Génération Aléatoire (My_Creation) : Algorithme partant de (0,0), empilant les cases visitées, ouvrant des portes vers des voisins non visités aléatoires, et dépilant (backtracking) en cas d'impasse.

Résolution Automatique (explorer) : Utilisation d'une seconde pile pour explorer le labyrinthe de (0,0) jusqu'à la sortie (p-1, q-1) en marquant et démarquant les cases pour trouver le chemin optimal.

Gestion de l'abandon : Association de la touche F1 pour afficher la solution (le chemin de la pile) et F2 pour réinitialiser la partie.




I. À partir d’une grille rectangulaire p × q, je peux créer un labyrinthe en ajoutant un certain nombre de murs pour séparer deux cases voisines. Je conviens en outre que ce labyrinthe est entouré de murs sur toute sa périphérie, de sorte qu’il n’est pas possible de sortir de la grille. Pour l’affichage, j’utilise la bibliothèque <ncureses.h>, que je dois compiler en linkant avec la librairie ncurses. Les fonctions suivantes sont mises à ma disposition :
•	void ShowLab(laby & L) ;
•	void Mark_Case(int i, int j) ;
•	void UMark_Case(int i, int j) ;
•	int LireCommande();
•	void clear();
•	void mvprintw(int ligne, int colonne, const char *). 
La fonction « int LireCommande() » est une fonction qui retourne :
1 si j'appuie sur la touche F1*
1. 3 si j'appuie sur la touche gauche
2. 9 si j'appuie sur la touche droite
3. 12 si j'appuie sur la touche haut
4. 6 si j'appuie sur la touche bas 
5. Afin que je puisse tester mon programme, un fichier contenant un labyrinthe sous format texte m'a été fourni. Voici le format proposé : p q W N S E W N S E W N S E ….. p : le nombre de lignes q : le nombre de colonnes si W==1 alors la porte ouest est ouverte si N==1 alors la porte Nord est ouverte.

II. La fonction laby * lire_Laby(laby & L) est donnée dans le fichier « votre_prog.cpp ». Ce fichier sera mon point de départ. Pour écrire un labyrinthe dans un fichier, j'écris la fonction void ecrire_Laby(char *fn) et je la teste.

III. Pour le déplacement dans le labyrinthe, j'écris la fonction «void deplacement(laby & L) », qui marque la case (0,0) et qui marque les cases les unes après les autres selon la commande de l’utilisateur. La commande de l’utilisateur est saisie grâce à la fonction « LireCommande() », décrite ci-dessus. Une commande invalide conduit à l’arrêt de la partie : « échec ». Si l’utilisateur arrive à la destination, c’est-à-dire à la case (p-1,q-1), alors il « gagne ».

IV. Pour créer un labyrinthe aléatoire parfait, je commence par créer un labyrinthe vide avec toutes les portes fermées. Ensuite, je choisis une case arbitraire et je la marque comme visitée. Puis, je détermine les cases voisines qui n'ont pas encore été visitées, j'en choisis une au hasard et je la marque comme visitée également. Enfin, j'ouvre la porte entre la case initiale et la nouvelle case visitée. Je répète ce processus avec la nouvelle case visitée jusqu'à ce qu'il n'y ait plus de case voisine non visitée. À ce stade, je reviens à la case précédente jusqu'à ce que je revienne à la case de départ. Si je ne peux plus continuer, le labyrinthe est terminé. Voici ma fonction :

laby* My_Creation(int p, int q) {
laby* L = new laby(p, q); // Créer un labyrinthe dynamiquement
Pile p; // Créer une pile
p.empiler({0, 0}); // Empiler la case (0,0)
while (!p.vide()) {
auto [i, j] = p.depiler(); // Dépiler la case courante
for (auto [di, dj] : {{-1, 0}, {1, 0}, {0, -1}, {0, 1}}) { // Parcourir les cases voisines
int ni = i + di, nj = j + dj;
if (L->accessible(i, j, ni, nj) && !L->visite(ni, nj)) { // Vérifier si la case est accessible et non visitée
L->visite(ni, nj) = true; // Marquer la case comme visitée
L->ouvrir(i, j, ni, nj); // Ouvrir la porte entre la case courante et la case voisine
p.empiler({i, j}); // Empiler la case courante
p.empiler({ni, nj}); // Empiler la case voisine
break;
}
}
}
return L;
}

V. Maintenant que le labyrinthe est créé, je vais écrire une fonction qui cherche un chemin entre la case de départ et la case de destination. Pour cela, je vais utiliser une pile dans laquelle je vais empiler la case de départ en la marquant comme explorée. Ensuite, je vais entrer dans une boucle où je vais dépiler la case courante et vérifier si elle a des cases voisines accessibles et non encore explorées. Si c'est le cas, je vais en choisir une au hasard, la marquer comme explorée, l'empiler et recommencer au début de la boucle. Si toutes les cases voisines sont déjà explorées, je ne fais rien. La boucle s'arrête lorsque j'empile la case de destination. La fonction retourne la pile contenant le chemin entre la case de départ et la case de destination.


Pile* explorer(laby& L) {
Pile* p = new Pile(); // Créer une pile
L.visite(0, 0) = true; // Marquer la case de départ comme visitée
p->empiler({0, 0}); // Empiler la case de départ
while (!p->vide()) {
auto [i, j] = p->depiler(); // Dépiler la case de départ

VI. j'écris une fonction qui réinitialise l'affichage et affiche le labyrinthe ainsi que le chemin qui mène de Dep à Dest : void affiche_chemin(Pile & P).

VII. je dois écrire une fonction jeu qui permet de : générer un labyrinthe, permettre au joueur de se déplacer (fonction déplacement) pour trouver un chemin. En cas d'abandon (touche F1), j'ai le choix entre : afficher la solution (affiche_chemin), modifier la fonction explorer pour qu'elle affiche les différentes tentatives d'exploration. Je marque les cases (void Mark_Case(int i, int j); au fur et à mesure de leurs empilements et je dé-marque quand je dépile (UMark_Case(int i, int j) ;).
