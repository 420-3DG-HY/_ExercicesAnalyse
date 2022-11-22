# Cas d'utilisations


```plantuml
actor "chef d'équipe" as ChefE
actor "chef de table" as ChefT
actor Croupier

left to right direction

usecase "Brise table" as bt

usecase "Assigne joueur" as aj

ChefE --  bt
ChefE -- (Assigner croupier)

Croupier -- (NotifieÉlimination)

ChefT -- (Ferme table)
ChefT -- aj

NotifieÉlimination ..>  bt :extends
NotifieÉlimination ..>  aj :extends
```

[Créez une fiche (fichier distinct) par cas d'utilisation]
[Mettez ici les liens vers les fiches de chacun des cas]

* [UC01 Notifie élimination](UC01.md)
* [UC02 Fermer table](UC02.md)
* [UC03 Assigner joueur](UC03.md)
* [UC04 Briser table](UC04.md)
* [UC05 Assigner croupier](UC05.md)