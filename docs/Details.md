# Analyse détaillée


```plantuml

hide circle


class Croupier {
    NotifieÉlimination(joueur)
}

class "Chef de table" as ChefTable {
    Ferme(table)
    Assigne(joueur, table)

}

Croupier --> ChefTable #lightblue : NotifieÉlimination 
class Table{
    DateTime : BriséeÀ
    int : numéro
}

ChefTable --> "1..4" Table #lightblue : surveille
class Tournoi {
    Date : date
    Lieu : lieu
    int : minJoueurs
    int : maxJoueurs
}

class Joueur {
    DateTime : ÉliminéÀ
    string : nom
    string : Membre
}

class "Chef d'équipe" as ChefEquipe {

    Brise(table)
    Assigne(croupier,Table)
}

class Équipe {

}

class Assignation {
     DateTime : début
     DateTime : fin
}

Équipe o-- "1..2" ChefEquipe
Table o- "0..1" Croupier
Équipe o-- "*" Croupier
Équipe o-- ChefTable

ChefTable o-- "*" Table


Assignation o-- "0..10" Joueur
Assignation o-- "1" Table

Tournoi o-- "0..250" Joueur : inscrits
Tournoi o-- "1..25" Table

Tournoi o-- "1" Équipe
```
