# Information provenant du client 

(notes de réunion)



## But du projet :

Implémenter une interface utilisateur qui permettra de répertorier des crimes dans un environnement distribué.
Document de travail utilisé pour accomplir la tâche du policier : [fichier Excel construit et utilisé par le client](Echantillon1mois.xlsm)

Dans ce document : 

* Chaque onglet représente une journée du mois. Ces pages sont identiques dans leur format et comportement.
* Les derniers onglets sont des données de références et une page de rapport statistique
Actuellement, pour chaque infraction commise, le sergent doit remplir une ligne dans le fichier Excel. Chaque ligne devra ultérieurement être approuvé par les services administratifs et par le sergent. 

L’objectif de ce projet est de créer une application qui permettra aux employés du poste de police de la Sureté du Québec de Saint-Hyacinthe de journaliser les informations quotidiennes dans un environnement distribué (serveur de fichier). Actuellement, les employés du poste de police utilisent un classeur Excel situé sur un serveur de fichiers qui ne permet pas d’utiliser les fonctions avancées de Excel dans un environnement distribué. C'est à dire qu'une seule personne à la fois peut modifier ce fichier et c'est un frein important à la productivité.

L'objectif est donc de refaire l'équivalent du document Excel de manière à ce qu'il soit collaboratif et que plusieurs personnes puissent l'ouvrir en même temps. Les contraintes de sécurité de la Sûreté du Québec sont importantes et l'ampleur du projet doit rester très petite. 

Les usagers Windows utilisant déjà un double authentification avec clé USB au moment de leur connexions à Windows, il n'y a pas d'enjeu de sécurité d'accès ou de confidentialité autre que les permissions déjà en place sur poste client et le serveur de fichier. De plus, ces usagers sont asermentés et responsables des limites de leurs actions. Il n'y a donc pas d'enjeu de sécurité sur les rôles internes de l'application. Ces rôles sont surtout là pour optimiser l'ergonomie du travail.

Les normes de sécurité et les processus de prise en charge d'un logiciel par les services informatiques de la SQ sont très lourds. L'utilisation d'un serveur de base de données ou même l'installation d'un logiciel sur un poste de travail rend le projet trop gros pour les bénéfices prévisibles.

Afin de respecter ces contraintes, nous proposons donc de créer une application autonome (exécutable autonome, sans installation) qui sera sur le serveur de fichier. Cette application créera et utilisera une une base de données SQLite qui contiendra les données mensuelles sous forme de fichier sur ce même serveur. L'application s'exécutant avec les privilèges de l'usager Windows déjà authentifié, il sera facile d'identifier l'usager courant pour optimiser l'interface.

## Comportements

Lors du démarrage de l'application, une fenêtre apparaît pour permettre à l'utilisateur de se connecter. Attention, vous devez cacher le mot de passe.

### Ajouter un utilisateur

Une fois connecté, si l’utilisateur est un Sergent(Groupe = 1) alors il a accès au bouton qui lui permet de créer un nouvel utilisateur. Encore une fois, vous devez utiliser les procédures stockées.
En plus du nom d’utilisateur, du mot de passe, du prénom et du nom de l’utilisateur, vous devez spécifier à quel groupe il appartient :
*  Groupe = 0 : Utilisateur normal
*  Groupe = 1 : Sergent
*  Groupe = 2 : Services administratifs

### Affichage des lignes du registre
Après s’être connecté, un utilisateur peut ajouter des lignes au registre. Par défaut, il est positionné sur la date du jour. Dans ce cas, seule les lignes du registre de la date du jour apparaîtront. Si l’utilisateur sélectionne une nouvelle date, les données sont rafraîchies.
De plus, pour voir les modifications des autres utilisateurs, les lignes du jour seront mise à jour périodiquement (au 20 secondes).
Modifier /Ajout une ligne du registre
Il est possible de modifier toutes les informations d’une ligne sauf la validation par les services administratif et la validation du sergent. Seule les personnes ayant l’autorisation nécessaire peut modifier ces cellules. Si un utilisateur ayant les droits modifie cette option, son id est sauvegardé dans la case approprié.

Pour les cellules :
*  Nature de l’événement(non-modifiable),
*  Matricule assigné(non-modifiable),
*  Ville→Code municipalité(modifiable)
*  Rue(modifiable)

Ces données doivent être saisie à partir de « combobox ». Pour les combobox modifiable, des valeurs sont proposé à l’utilisateur pour accélérer la saisie. On doit seulement sauvegarder la valeur du texte entrées dans les champs approprié.

Lors de la saisie d’une ligne, si on n’entre pas toutes les informations mais avec le même numéro d’entrée que la ligne précédente, on utilisera ces données pour remplir les autres champs.
La modification d’une ligne est sauvegardée lorsque le focus quitte une ligne en particulier.

Si une erreur de concurrence survient, un message devra avertir l’utilisateur du choix qu’il a à faire.

### Supprimer une ligne du registre
Pour supprimer une ligne, il suffit de sélectionner la ligne et d’appuyer sur la touche supprimer sur le clavier. Attentions aux problèmes de concurrence.

## Divers

*  Prévoir une sauvegarde automatique
*  Il y a trois couleurs différentes pour les codes d'entrées soit de soir, de jour et de nuit
*  Recherche automatique des combos par rapport aux autres feuilles
*  Possibilités de se connecter + cocher pour approuver. Devrait conserver un journal des événements. Impossible d'enlever un coche sauf la personne qui l'a mis.
*  Possibilités de faire un tableau croisé dynamique par rapport aux données
*  Savoir le nombre de dossier par patrouilleur par fréquence.
*  Savoir le nombre de dossier par ville
*  Il faut pouvoir ajouter/supprimer Tous les éléments dans lesBD.
*  Il est possible qu'un même dossier remplisse plusieurs lignes. On met des traits ou des guillemets pour dire que c'est la séquence.
*  Il est possible qu'il n'y ait pas de numéro de patrouilleur.
*  Il est possible que quelqu'un d'un autre poste viennent travailler soit on l'ajoute soit on ne met rien.
*  Il est possible d'ajouter un commentaire dans la section Nature de l'événement...sans modifier la catégorie.
*  On peut avoir plusieurs événements pour un même code, avec plusieurs patrouilleurs. Il faut lier les événements au bon patrouilleur.
*  Il faut pouvoir rentrer plusieurs données.
*  Il faut pouvoir imprimer les documents et bien gérer les pages.
*  Il pouvoir afficher les données pour une certaines dates.
*  Il faut pouvoir chercher par fréquence. Afficher tous les événements pour une journée, semaine, mois, etc. (intervalle).
*  Ajouter une zone de commentaire.
*  Ajouter les codes municipaux et que la ville suive
*  Avoir deux colonnes pour la nature de l'événement et le code. Remplir en fonction de.
*  Enlever la colonne Date et l'afficher en gros à quelque part.
*  Incrémenter automatiquement Le numéro d'entrée lors de la saisie d'une nouvelle ligne.
*  Attention, si la valeur est effacée et qu'on crée une nouvelle ligne, en tenir compte.
*  La base de données doit être un fichier localiser sur le réseau.
*  Ne pas permettre de supprimer une ligne de registre si on n'est pas administrateur.
*  Créer une interface pour permettre de modifier les employés.
*  Une interface pour modifier les codes MIP.
*  Permettre d'ajouter des rues + et si le temps les villes.
*  Voir si on peut faire un retour de chariot automatique dans les commentaires pour voir tous les commentaires.
*  Ajouter les initiales des personnes qui ont autorisé (ADMIN + Sergent)
*  Modifier l'entête des cases à cocher pour ADM, S/R
*  Lors de l'impression, imprimer sur le verso le numéro d'entrée et les commentaires associés