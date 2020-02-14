# PHP / Symfony - Ynov B2 - Partiel

Vous allez **commencer** la réalisation d'un système de location de voitures en ligne.

Critères d'évaluation :

- Commandes vues en cours
- Connaissance des concepts enseignés
- Création et utilisation d'un service
- Création et personnalisation d'un formulaire
- Utilisation de l'authentification et du contrôle d'accès

## Mode de rendu

Lien vers votre dépôt Git, que je pourrai clôner.

## Dépôt Git

Votre dépôt contiendra un fichier `README.md` contenant les réponses aux questions suivantes :

>Les réponses aux questions doivent être placées dans le cadre de Symfony

1. Qu'est-ce qu'un container de services ? Quel est son rôle ?
2. Quelle est la différence entre les commandes `make:entity` et `make:user` lorsqu'on utilise la console Symfony ?
3. Quelle commande utiliser pour charger les fixtures dans la base de données ?
4. Résumez de manière simple le fonctionnement du système de versions "Semver"
5. Qu'est-ce qu'un `Repository` ? A quoi sert-il ?
6. Quelle commande utiliser pour voir la liste des routes ?
7. Dans un template Twig, quelle variable globale permet d'accéder à la requête courante, l'utilisateur courant, etc...?
8. Pour mettre à jour la structure de la base de données, quelles sont les 2 possibilités que nous avons abordées en cours ?
9. Quelle commande permet de créer une classe de contrôleur ?
10. Décrivez succintement l'outil Flex de Symfony

## Exercice

>## Vous utiliserez la version 4 de Symfony

Le but aujourd'hui est de ***commencer*** un système de location de voitures en ligne.

### Entités

>Nous n'avons pas vu les relations entre entités pour générer des tables liées par des clés étrangères, il n'est pas nécessaire de créer une table *Marque*, *City* ou *Categorie* par exemple

Vous allez créer 2 entités : **Advert** et **User**.

L'entité **User** servira à l'authentification.

#### Advert

| Champ | Type | Description |
|---|---|---|
|ID| INT (PRIMARY KEY) |
|title|VARCHAR(255)| Titre de l'annonce |
|description|TEXT| Description de l'annonce |
|city|VARCHAR(255)|
|car_year|INT| Année de première mise en circulation  de la voiture |
|nb_km|INT| Nombre de kilomètres de la voiture |
|nb_days|INT| Nombre de jours proposé pour la location |
|price|INT| Prix total pour la location |

#### User

>Vous utiliserez l'email comme nom d'utilisateur "unique"

| Champ | Type |
|---|---|
|ID| INT (PRIMARY KEY) |
|email|VARCHAR(255)|
|login|VARCHAR(255)|
|roles|JSON|
|password|VARCHAR(255)|

Le système évoluera plus tard pour intégrer des tables liées (modèle de la voiture, catégorie, aperçu du véhicule avec des photos, etc...) mais ce n'est pas le cadre des développements d'aujourd'hui.

### Fixtures

Générez les fixtures nécessaires pour ces entités.

>### Pour les annonces, regardez tout de suite le paragraphe "Service d'estimation de prix d'une annonce", vous l'utiliserez dans les fixtures
>
>Pour les utilisateurs, faites un administrateur et plusieurs utilisateurs "normaux" (administrateur : `ROLE_ADMIN`, utilisateur normal : `ROLE_USER` ou bien vide dans le champ `roles`) dans les fixtures

### Service d'estimation de prix d'une annonce

L'idée de ce début d'application est de pouvoir saisir une annonce, et que le système estime le prix à proposer en fonction de l'annonce saisie.

Vous allez donc développer une première version de l'algorithme d'estimation du prix.

#### Algorithme d'estimation v1

Votre service évaluera le prix de l'annonce en fonction de l'année de la voiture, le nombre de kilomètres et le nombre de jours de location.

> Vous êtes libres sur le choix de la formule, ce n'est pas un critère de notation capital. Vous devez renvoyer un `int` en prenant en compte les 3 arguments cités (année de la voiture, nombre de kilomètres, nombre de jours de location)
>
> Documentez-vous pour arrondir un résultat en PHP

Vous injecterez ensuite ce service dans vos fixtures, puis dans le contrôleur qui gèrera la création d'une nouvelle annonce à partir d'un formulaire (voir plus bas).

### Interface

>Vous utiliserez Bootstrap

L'application contiendra :

- Une page d'accueil
- Une page de liste des annonces
- Une page de saisie d'une nouvelle annonce
- Une page d'inscription
- Une page de connexion
- Une page d'administration avec la liste des utilisateurs **accessible uniquement par les utilisateurs administrateurs**

### Structure de l'interface

- Header avec un menu
- Contenu
- Footer

>Le menu contiendra un élément "Déconnexion" lorsqu'un utilisateur sera connecté

### Page d'inscription

La page d'inscription contiendra un formulaire de saisie se basant sur l'entité `User`.

>## Pour le type du champ mot de passe, vous vous documenterez sur le site de Symfony pour trouver un type permettant de saisir un mot de passe dans un champ et de le confirmer dans un autre

Le contrôleur associé enregistrera un nouvel utilisateur dans le système.

### Page de saisie d'une nouvelle annonce

La page de saisie d'une nouvelle annonce, accessible à tout le monde, permettra de saisir et enregistrer une nouvelle annonce dans le système.

Vous injecterez le service d'estimation de prix dans le contrôleur chargé d'enregistrer la nouvelle annonce.
