# Angel IA

## Description du service

**Angel-IA** est un service qui permet de proposer à une personne agée des activités réalisées par un avatar basé sur 'Ready Player Me' comme :

-	Discuter sur un sujet qui a été discuté par le passé 
-	Discuter sur un sujet personnel suivant ce que l'on sait de la personne : quelles sont les nouvelles des enfants, quelles sont les nouvelles de la famille, ....
-	Demander à la personne de raconter un événement, son enfance, un fait marquant
-	Donner les nouvelles locales
-	Donner les nouvelles nationales 
-	Donner les nouvelles internationales 
-	Donner les nouvelles économiques (boursière, ...)
-	Donner la météo du jour et de demain
-	Réciter une/des histoire courte
-	Réciter une/des histoire drôle
-	Réciter une légende 
-	Réciter une poésie
-   Réciter une prière
-	Réciter des anecdotes
-	Décrire une découverte récente, un avis d'expert sur un sujet
-	Rappeler les rendez-vous à venir
-	Rappeler un anniversaire à venir
-	Appeler par what’s app une personne ou un groupe de la famille avec qui il n’y a pas eu de conversation récente 
-	Faire des recommandations comme « n’oubliez pas vos médicaments » quand la personne est à table ou bien « ne donner que des croquettes au chien » quand elle le nourrit ou « il va faire très beau, c’est le moment de faire une promenade »
-	Proposer un jeu de mémoire
-	Proposer un jeu de devinettes
-	Proposer de montrer des photos ou des vidéos récupérer de la sélection d’activité et de les trier en fonction de la liste des activités 
-	Proposer de diffuser de la musique
-	Proposer de diffuser une radio
-	Proposer d’allumer ou éteindre la télé 
-	Lister les programmes télé ou radio intéressants
-	Suggérer des idées de repas avec des recettes simples
-	Proposer de faire un récapitulatif de ce que la personne a fait avant dans la journée ou la veille ou dans la semaine
-	Proposer des assouplissements ou des mouvements de gym

En fonction de ce que fait la personne; et pas tout le temps mais de façon raisonnée et espacée entre les activités proposées, le service Angel-IA active sur un écran de tablette un avatar qui propose ces activités.

Ces propositions d’activités ne sont possibles qu’en fonction de ce que fait la personne ou vient de faire la personne (par exemple pas de gym quand elle mange ou quand elle vient juste de se reveiller).

La personne peut également activer automatiquement l'avatar quand elle dit ´Angel’ et alors lui poser une question ou demander l'activation d'un service.

Un paramétrage inital permet de configurer :
- laccès au réseau wifi
- 5 à 10 photos de la personne (1.visage de face, 2.visage de dos, 3.visage de profil droit, 4.visage de profil gauche, 5.visage de face 45° droit, 6.visage de face 45° gauche, 7.vue d'ensemble de face, 8.vue d'ensemble de dos, 9.vue d'ensemble de profil droit, 10.vue d'ensemble de profil gauche)
- la fréquence quotidienne possible des propositions d’activité entre 0 à 50
- la fréquence hebdomadaire possible des propositions d’activité entre 0 à 150
- le niveau de difficulté intellectuelle entre 1 à 5 (1:très facile, 2:facile, 3: moyen, 4: difficile, 5: très difficile)
- le niveau de difficulté physique entre 1 à 5 (1:très facile, 2:facile, 3: moyen, 4: difficile, 5: très difficile)
- le prénom de la personne
- la date de naissance de la personne
- l'annuaire avec : type (enfant,peti-enfant,arrière petit-enfant,conjoint,cousin,neveu,ex,ami,médecin,soignant), prénom, nom, sexe, date de naissance, adresse, identifiant what's app, numéro de téléphone
- hobbies
- musiques préférées
- radios préférées
- émissions télé préférées

## Composants

Pour réaliser cela, le service se base sur 6 composants principaux :

- [angel-dl4j-detection-models](https://github.com/rbaudu/angel-dl4j-detection-models) : créateur et entraineur de modèles compatibles nd4j à partir d'images et audios pré-définis classés en activité ; ce composant contient également les scripts pour intégrer de nouvelles images et audios et les positionner.
- [angel-server-capture](https://github.com/rbaudu/angel-server-capture) : Serveur de capture, synchronisation et analyse d'activités en temps réel à partir de flux vidéo et audio avec l'aide des modèles créés par le composant précédent 'dl4j-detection-models'.
- [angel-virtual-assistant](https://github.com/rbaudu/angel-virtual-assistant) : Assistant interactif proactif avancé qui surveille les activités de la personne et propose des recommandations contextuelles d'activités adaptées aux besoins de l'utilisateur
- [angel-update-service](https://github.com/rbaudu/angel-update-service) : Service web hébergé chez Ovh qui fournit les mises à jour de contenus statiques (histoires, recettes, ..) et dynamiques (nouvelles, météo, découvertes, ...) à l'assistant virtuel
- [angel-install-on-pi](https://github.com/rbaudu/angel-install-on-pi) : programme d'installation et description du service Angel-IA sur un Raspberry Pi 5 via une architecture basé sur un host Alpine Linux et Chromium et 3 containers hébergeant respectivement 'angel-virtual-assistant', 'angel-server-capture' et nginx'
- [synovant-website](https://github.com/rbaudu/synovant-website) : le site web de Synovant avec les informations sur Angel-IA

Le projet '**angel-dl4j-detection-models**' est une application java basé sur ND4J qui permet de construire et entrainer des modèles de deep learning analysant des vidéos (images, sons, présence de personne) pour déterminer ce que fait la personne âgée. Ce projet s'exécute sur un ordinateur puissant de développement et prototypage. Il produit des modèles qui sont utilisés par le projet 'angel-server-capture'.
Les projets '**angel-server-capture**' et '**angel-virtual-assistant**' sont des applications Java Spring boot qui s'exécute sur une tablette (Windows) ou un PC portable (Windows, Linux) ou un Raspberry Pi 5 (Chromium Kiosk + Docker) installé au domicile de la personne âgée. Au démarrage de la tablette/PC/Pi, les 2 applications 'angel-server-capture' et 'angel-virtual-assistant' sont lancées :

- Si l'initialisation des paramètres n'est pas faite ou incomplète, elle est proposée,
- dès qu'elle est faite, l'avatar est lancé et indique ce qu'il peut faire puis il se met en mode veille (écran noir avec heure affichée)
- en tâche de fond, le programme 'ange-server-capture' analyse les vidéos et transmet en continu l'activité détectée de la personne
- en tâche de fond, le programme 'angel-virtual-assistant' reçoit les activités et décide de proposer ou non une activité à la personne en prenant en compte de nombreux paramètres (activités courantes, dates, heures, agendas, activités précédentes, temps, historiques des activités, présence d'autres personnes, besoins des animaux de compagnie, ...) 
- dès que l'algorithme du programme 'angel-virtual-assistant' a décidé qu'une activité peut être proposée, il active l'avatar qui propose l'activité.
- à tout moment, la personne âgée peut dire 'angel' pour activer l'avatar et lui demander quelque chose (questions, discussions ou activités)
- régulièrement, le programme 'angel-virtual-assistant' interroge le service web 'angel-update-service' pour obtenir du contenus dynamiques mis à jour (actualités internationals, actualités nationales, actualités régionales, météo du jour, météo de demain, météo de la semaine, programme TV du jour, programme radio du jour) et de nouveaux contenus statiques (recettes, histoires courtes, légendes, poésies, biographies, histoires drôles, découvertes scientifiques, chroniques historiques, mythes, prières boudhiste/chrétienne/hindou/juive/musulmane/multi-confessionnelle, des devinettes, des énigmes, des exrcices de mémoire, des charades, de la musique, des chansons)


Le projet 'angel-update-service' est un service web de mise à jour qui s'appuie sur diverses APIs et IA LLMs pour obtenir des contenus dynamiques et statiques. Il produit des packages qui sont fournis au programme 'angel-virtual-assistant' installé chez les personnes âgées. Il fournit également des mises à jour logiciel de 'angel-virtual-assistant' et 'angel-server-capture' ainsi que de nouveaux modèles d'IA pour l'anamlyse vidéo.

Pour le packaging des programmes 'angel-virtual-assistant' et 'angel-server-capture' sur les tablettes/pc/Pi, il est prévu d'avoir un système d'installation. Pour l'instant, il n'existe que pour Raspberry Pi 5 dans le projet '**angel-installation-on-pi5**'.

Le projet '**synovant website**' est le site web de Synovant.com qui décrit les missions de Synovant et également L'assistant Angel-IA en proposant à ce stade de participer à un projet pilote.