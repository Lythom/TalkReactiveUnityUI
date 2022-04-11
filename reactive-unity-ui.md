# la réactivité

- qu'est-ce que ça veut dire réactif ? 
* avec une boucle update c'est réactif 
* avec des données immuables
* avec un pub sub
* notion de source de vérité 
* notion de responsibilité
* inversion de dépendance ? Ce n'est pas l'événement qui est responsable de mettre à jour les éléments visuels et la donnée (mode impératif), c'est la donnée qui déclenche l'événement et chaque élément s'abonne à la donnée dont il a besoin (déclaratif)
Réactivité = Déclarativité = Responsabilité du composant de gérer son rendu

# pour la ui

- pourquoi ça concerne principalement la ui ? 
= mise en cache pour optimiser le rendu. 
Montrer PBS de perfs lorsqu'on update la ui chaque frame. 
- Le world est souvent rerendu intégralement chaque frame (déplacements de camera)
- ça peut aussi fonctionner pour de l'invalidation de cache d'éléments du world

# implementation

- donnée immuable + mémoization
Mentionner redux
Peu adaptée dans le contexte du jeu vidéo, 
=> pèse sur le garbage collector. Montrer la réutilisation de l'espace mémoire.
=> Utile uniquement pour la UI = un state UI et un state game différents ? → pb de compatibilité des algo et d'invalidation de la UI sur le state de jeu
=> Problème de compétence, d'outillage et d'adhésion lié aux données immuables

- unirx
On la fait pour city Invaders, plusieurs écrans en prod sur ce mécanique la. Demo (juste après) 
Pull based ou push based? 
https://stackoverflow.com/questions/58471593/how-async-streams-compares-to-reactive-extension
=> Puissant mais difficile à lire et à écrire
=> Très difficile à déboguer
    - émission asynchrone: on ne connait pas l'origine du changement de donnée ni le contexte (ou difficilement). Ex: de capture d'erreur dans CI

- update loop avec des ifs
Fait sur A Time Paradox
=> Ok car petit jeu avec peu de UI
=> Verbeux
=> Contre intuitif (même si performant) + coût d'un check de game object

- wrapper réactif au state
réactive de unitask
Pull based: le consumer traite la file de producer à son rythme, ici par l'utilisation d'un await
La solution retenue pour notre dernier projet
Serialization : mesurer le poids supplémentaire en binaire et en json. 
Mesurer la différence de perfs ? 

# demo

À quoi ça ressemble rx
- écran d'équipement, liste de sélection, écran des lieux et problèmes d abonnements multiples
- problème de debugging 
- besoin inherent d asynchrone qui rend le debug encore plus dur, perte de contexte. Vraiment lié à unirx
- l'organisation de l'état du jeu reste éventuellement chaotiques, quelle est la source of truth ? 

À quoi ça ressemble avec un wrapper, quel genre d'architecture est ce que ça permet (composant connecté)
Probleme de rigidité de la structure. 
Problème des listes à taille variables. 
State monolithique
Mise à jour recursive
Préservation de l'espace mémoire. 
Affichage du state dans l'inspecteur unity
Serialization 

Refactoring de la généralisation du connected component? 



