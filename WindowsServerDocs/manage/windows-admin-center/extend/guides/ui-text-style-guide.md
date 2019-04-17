---
title: Guide de style pour la conception et le texte de l’interface utilisateur Windows Admin Center
description: L’interface utilisateur de Windows Admin Center de conception et le texte de guide de style SDK
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ab5bee55975b803a77db0b6cdb179b76590e1d83
ms.sourcegitcommit: 56cccb09a35b2d3eef9056e2406c3a1762d28682
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4739979"
---
# Guide de style pour la conception et le texte de l’interface utilisateur Windows Admin Center

>S’applique à: Windows Admin Center

Cette rubrique décrit l’approche générale de la rédaction du texte de l’interface utilisateur pour Windows Admin Center, ainsi que certaines de nos conventions et méthodes spécifiques.

Windows Admin Center et toutes les extensions doivent respecter les [principes en matière de voix de Microsoft](https://docs.microsoft.com/style-guide/brand-voice-above-all-simple-human) afin que l’expérience soit facile à utiliser et conviviale. Ce guide de style s’appuie sur ces principes en matière de voix, ainsi que sur le [Microsoft Writing Style Guide](https://docs.microsoft.com/style-guide/welcome/), donc veillez à consulter ces deux ressources pour des informations sur des éléments tels que l'[accessibilité](https://docs.microsoft.com/style-guide/accessibility/accessibility-guidelines-requirements), les [acronymes](https://docs.microsoft.com/style-guide/acronyms) et le [choix de mots](https://docs.microsoft.com/style-guide/word-choice/) tels que [s'il vous plaît](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/p/please) et [désolé](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/s/sorry).

## Boutons

- Boutons doivent être en un seul mot dans la mesure du possible, en particulier si vous souhaitez localiser votre outil. Deux ou trois est OK, mais essayez d’éviter plus. Si vous avez quatre mots ou plus, il serait préférable d’utiliser un contrôle de lien.
- Les étiquettes de bouton doivent être concises, spécifiques et explicites. Au lieu d’un bouton générique «Soumettre», utilisez un verbe correspondant à l’action de l’utilisateur, tel que «Créer», «Supprimer», «Ajouter», «Formater», etc.
- Si un bouton suit une question, son étiquette doit clairement correspondre à la question (généralement «Oui» ou «Non»).

## Mise en majuscules

Nous suivons le style de Microsoft concernant la [mise en majuscules](https://docs.microsoft.com/style-guide/capitalization): utiliser le style Majuscule en début de phrase pour quasiment tout.

| Élément d’interface utilisateur              |Mise en majuscules|Commentaires|
|-------------------------|--------------|--------|
|Badges (par exemple, PRÉVERSION) |Tout en majuscules      ||
|Tout le reste          |Majuscule en début de phrase|Toutefois, il existe certaines exceptions où nous exposons des propriétés d'objet à partir de WMI ou PowerShell se trouvant en dehors de notre contrôle.|

## Deux-points

Utilisez deux-points pour présenter des listes. Exemple :

    Choose one of the following:
    Cats
    Dogs
    Quokkas

N’utilisez pas les deux-points dans le texte de l’interface utilisateur lorsqu’une étiquette est sur une autre ligne à partir de la chose étiquettes ou lorsqu’il existe une distinction claire entre l’étiquette et la chose qu’il est l’étiquetage.

Utilisez les deux-points dans le texte de l’interface utilisateur lorsqu’une étiquette est sur la même ligne que le texte, il est l’étiquette et vous devez conserver les deux éléments de s’exécuter simultanément.

## Messages de confirmation

Les boîtes de dialogue de confirmation sont utiles lors de la poursuite peut avoir des résultats inattendus, par exemple, une perte de données. Elles doivent contenir des informations utiles paragraphe entraînant un résultat clair, en particulier pour les événements qui ne peut pas être annulée. 

- Assurez-vous qu’une confirmation est nécessaire. S’il n’existe aucune nouvelles informations à proposer (par exemple, «vous êtes sûr?»), puis un message de confirmation ne peut pas être nécessaire.  
- Vérifiez que le client souhaite de procéder à l’action.
- Assurez-vous que l’instruction principale (en-tête) et le texte descriptif (corps) ne sont pas redondants.
- Dans l’en-tête, définissez les résultats possibles comme une question ou une instruction sur ce qui va se passer ensuite. Par exemple, «effacer toutes les données sur ce lecteur? ou «Vous êtes sur le point d’effacer toutes vos données».
- Ajouter des détails dans le corps. S’il existe une variable, telles que le nom de l’élément que vous êtes sur la modification, vous pouvez l’inclure ici.
- Incluez une question simple (soit dans l’en-tête ou dans le corps) qui encadre un choix clair entre les deux boutons d’action.
- Un choix complexes, utilisez des boutons, encourager la lecture minutieuse Oui/non. Un choix plus simple, utiliser les boutons qui sont spécifiques à l’action, telles que tout supprimer ou annuler.

## Expériences de première exécution

La première fois qu’un utilisateur visite une page, vous avez la possibilité de les aider à prendre en main votre outil. Il peut s'agir des éléments suivants:

- Une chaîne de texte dans une page vide avec de courtes instructions sur la prise en main: par exemple, «sélectionnez Ajouter pour ajouter une application».
- Un lien vers le contrôle qui guide l’utilisateur pour la prise en main: par exemple, «Ajouter une application pour commencer».
- Une petite animation ou une courte vidéo montrant à l’utilisateur comment commencer

Voici quelques conseils de notre guide de style Windows:

### 1. Soyez utile

- Évitez le style et le langage marketing.
- Lorsque vous effectuez une démonstration ou proposez quelque chose, vérifiez que le résultat final est clair; se contenter de montrer au client comment effectuer une opération n’est pas efficace s'il ne sait pas quel en est le but.
- Ne présentez pas de conseils si le client n’en a pas besoin.

### 2. Il vaut mieux montrer que dire

Gardez le texte le plus simple possible (pensez à de petites animations ou à des vidéos).

### 3. Ne noyez pas l'utilisateur dans les détails

- Limitez les fenêtres contextuelles et les conseils à 4par session d’utilisation combinés, y compris les notifications système et les notifications de shell.
- Assurez-vous que la synchronisation des fenêtres contextuelles est utile.
- N'empêchez pas le client d’effectuer une opération.
- Assurez-vous que les fenêtres contextuelles sont faciles à ignorer.

### 4. Conservez le contexte

- Les enseignements sont plus efficaces lorsqu'ils sont présentés au bon moment.
- Si vous créez des didacticiels ou des diaporamas, restez concret.
- Évitez tout «discours» marketing: concentrez-vous sur des conseils et des astuces spécifiques.
- Donnez la possibilité aux clients de revenir au didacticiel ultérieurement, le cas échéant (les utilisateurs ne retiennent souvent pas les informations la première fois, mais les instructions d’installation ne peuvent être pertinentes qu’une seule fois).
- Un message d'état vide est l’endroit idéal pour l'information et/ou la convivialité: gardez-le simple et informatif.

### 5. Réduisez la configuration fastidieuse au minimum

Lorsque vous avez besoin que le client effectue une autre action pour obtenir une valeur ajoutée complète (s'inscrire à un service en ligne, etc.), rendez-la aussi simple que possible.

- Le message doit être court et direct.
- Évitez de le décourager. Si possible, donnez-lui un moyen de se connecter à partir de l’endroit où il se trouve.
- Si possible, proposez-lui de le faire plus tard, puis rappelez-le lui ultérieurement.
- Si vous le faites sortir de son expérience, donnez-lui un moyen de revenir rapidement et facilement.

## Liens d’aide

Voici quelques conseils de notre guide de style Windows:

### Lorsque nous devons pour fournir un lien d’aide?

Pratiquement jamais. Fournir un lien aide uniquement lorsque:

- Il existe une question évidente et importante que les clients sont susceptibles d’avoir pendant qu’ils se trouvent dans l’interface utilisateur de la réponse à ce qui leur permettra de réussir à la tâche de l’interface utilisateur. 
- Il n'est pas assez de place dans l’interface utilisateur pour fournir la quantité d’informations nécessaires pour les utilisateurs de réussir à la tâche de l’interface utilisateur. 

### Où doit aider aux liens s’affichent? 

- Liens de texte doivent apparaître aussi proche de l’élément d’interface utilisateur est dirigée vers le contenu d’aide que possible. 
- Si vous devez fournir un lien de texte qui s’applique à un écran de l’interface utilisateur entière, placez-le en bas à gauche de l’écran. 
- Si vous fournissez un lien par le biais d’un bouton d’aide (?), l’info-bulle doit être «Aide».

### Quelle URL devons-nous utiliser?

Liez jamais directement à une adresse web, utilisez plutôt un service de redirection.

Les développeurs Microsoft doivent utiliser un FWLink sauf quand il est un lien d’aide que les utilisateurs doivent taper manuellement, dans ce cas, utilisez un lien aka.ms (à condition que la cible de l’URL est un site Web qui reconnaît automatiquement les paramètres régionaux de navigateur, par exemple, Docs.microsoft.com)

### Recommandations en matière de texte 

- Utiliser des phrases complètes.
- N’incluez pas de ponctuation à l’exception des points d’interrogation de fin. 
- Vous n’avez pas besoin d’utiliser le même texte que le titre de la tâche; Utilisez un texte qui convient dans le contexte de l’interface utilisateur, mais assurez-vous qu’il existe une connexion logique entre les deux. Exemple : 
- Lien d’aide: quels sont les risques autorisant les exceptions? 
- Titre de la rubrique d’aide: «Permettant à un programme de communiquer via le pare-feu Windows»
- Soyez aussi précis que possible sur le contenu de la rubrique d’aide. 
    - Notre style
        - Comment le pare-feu Windows permet-il de protéger mon ordinateur?
        - Pourquoi les points forts peuvent améliorer une image
    - Pas notre style
        - Plus d’informations sur le pare-feu Windows
        - En savoir plus sur la gestion des couleurs
        - En savoir plus
- Utilisez la phrase entière pour le texte du lien, pas seulement les mots clés. 
    - Notre style 
        - [Quels sont les risques autorisant les exceptions?]()
    - Pas notre style
        - Quels sont les [risques autorisant les exceptions]()? 

## Messages d’erreur

Voici quelques recommandations adaptée à partir du Guide de Style Windows:

Écriture d’un message bon est un équilibre entre fournir suffisamment explication mais n’est pas trop technique; entre l’informelle et personnalisable, mais pas ennuyeux ou offensantes.

### Recommandations générales

Utilisez un seul message par cas d’erreur.

#### En-têtes

- Soyez concis et expliquez concise l’origine du problème ou **dans l’idéal, la procédure à suivre**. <br>Des surfaces d’interface utilisateur peuvent comporter des en-têtes qui tronquer au lieu d’habillage lorsqu’ils sont trop de temps, par conséquent, surveillez pour ces.
- Utiliser la solution dans l’en-tête s’il s’agit d’une étape simple.
- Assurez-vous que le titre est directement lié au bouton dans le cas où le lecteur ignore le corps du texte.
- Évitez d’utiliser «Un problème est survenu» dans les en-têtes, si vous n’avez aucun autre choix. Être plus spécifique sur le problème.
- Pour éviter l’utilisation de variables (par exemple, les noms de fichiers, dossiers et d’application) dans les en-têtes. Placez-les dans le corps.

#### Body

- Si l’en-tête explique suffisamment le problème ou la solution, vous n’avez pas besoin le texte de corps.
- Ne répétez pas le titre dans le message formulation légèrement différente.
- Communiquer clairement et concise ce qui est la solution.
- Concentrez-vous sur ce qui donne les faits tout d’abord.
- Ne blâme pas les utilisateurs de l’erreur.
- S’il existe un code d’erreur associé à l’erreur et si vous pensez que, y compris le code d’erreur peuvent vous aider au client ou le support Microsoft pour rechercher le problème, incluez pas directement sous le texte de corps et écrire comme suit:

    Code d’erreur: n°

    Si le client a toutes les informations nécessaires pour résoudre l’erreur sans le code, vous n’avez pas besoin pour l’inclure.

#### Boutons

- Écrire du texte du bouton afin qu’il soit une réponse spécifique à l’instruction principale. Si tel n’est pas possible, utilisez «Fermer» pour le texte du bouton abandon du (au lieu de «OK» ou «Terminé»).
- Si vous avez plusieurs boutons, vérifiez le bouton gauche l’action de que l’utilisateur est encouragée à prendre. Rendre le bouton à l’extrême droite l’action plus classique, par exemple, «Annuler».

#### Liens d’aide

Prendre en compte uniquement les liens d’aide pour les messages d’erreur que vous ne pouvez pas effectuez spécifiques et exploitables.

## Texte de l’état null

Voici quelques aide à partir du Guide de Style Windows.

État NULL se produit lorsque le contenu ou données client est absent à partir d’une application ou une fonctionnalité, lorsqu’aucun résultats après une recherche, ou lorsque l’informations sont manquantes dans un formulaire, par exemple, les informations d’une transaction de facturation.

### Recommandations

 - Si possible, utilisez les situations d’état null en tant qu’opportunité pour former les utilisateurs sur l’utilisation de la fonctionnalité (par exemple, découvrez comment ajouter de la musique, le cas pour rechercher des images, etc..)  
- Si vous avez un titre dans votre interface utilisateur, expliquez l’action à effectuer pour «corriger» l’état null (par exemple, «ajouter certains musique») 
- Amusez-vous avec le texte. Cet espace peut être une opportunité de fournir du plaisir dans la mesure où il ne sera probablement pas visible plusieurs fois. 
- Évitez «Est seuls ici.» Cela est sud et a été vous en abusez. 
- Éviter les questions telles que «N’ont pas de connected votre imprimante?» OK à utiliser une seule fois, mais ce format a tendance à obtenir vous en abusez et questions posées charge/pression supplémentaire sur le client. Il peut également paraître évident. 
- Diverses dans le texte d’état null est une bonne chose. 

### Exemples

- «Ajouter un utilisateur en tant que favori, et vous verrez les ici».
- «D’accord des succès ou vous êtes particulièrement fiers des extraits de jeu? Les ajouter à votre showcase.»
- De «personne dans une partie encore. Démarrez un!»
- «Lorsque quelqu'un vous ajoute en tant qu’ami, vous verrez les ici.»
- «Lorsque vous fasse le travail comme déverrouiller des succès, enregistrez les extraits de jeu et ajouter des amis, vous verrez qu’il tout ici.»
- «Vos amis favoris seront affiche ici, afin de pouvoir voir lorsqu’ils sont en ligne et jusqu'à ce qu’ils font.»

## Ponctuation

- Aucune ponctuation finale (points, points d’interrogation) pour des titres ou des phrases incomplètes. Une exception est dans une boîte de dialogue de confirmation dans laquelle le titre pose une question
- Utilisez les instructions du Guide de Style Microsoft sur les [points](https://docs.microsoft.com/style-guide/punctuation/periods) et les [points d’interrogation](https://docs.microsoft.com/style-guide/punctuation/question-marks).

## Messages d’état

Les messages d’état se composent de notifications et de messages contextuels (toast).

|Type de chaîne         | Notes                               |
|------------        |-------------------------------------|
|Toast               |Majuscule en début de phrase avec ponctuation de fin: de préférence avec une variable d’objet pour que les utilisateurs comprennent à quel objet le message s’applique au cas où ils seraient sortis de l’objet|
|Titre de notification|Majuscule en début de phrase sans ponctuation finale (il s’agit d’un titre): de préférence avec une variable d’objet|
|Détails de la notification|Phrases complètes, de préférence avec un lien vers l’interface utilisateur qui affiche l’objet|

Voici quelques recommandations détaillées sur les messages de notification:

|Type de chaîne         | Notes                               |
|------------        |-------------------------------------|
|Démarré             |Omettez dans la mesure du possible: généralement vous pouvez vous contenter de passer au message en cours afin de réduire les distractions.|
|En cours         |Commencez avec le verbe de l’action que vous effectuez et terminez par des points de suspension pour indiquer une opération en cours. Voici un exemple:<br> *Création en cours du volume «Données client»...*|
|Réussite             |Commencez par l'action que le logiciel vient d'effectuer et finissez par «réussi(e)». Voici un exemple:<br> *Création du volume «Données client» réussie.*|
|Échec             |Démarrez avec «Impossible de» et terminez par ce que le logiciel n’a pas pu faire. Voici un exemple:<br> *Impossible de créer le volume «Données client».*|

## Info-bulles

Les info-bulles bonnes brièvement décrivent les contrôles sans étiquette ou fournissent un peu des informations supplémentaires pour les contrôles étiquetés, lorsque cela est utile. Ils peuvent également aider les clients à naviguer dans l’interface utilisateur en offrant supplémentaires: non redondantes: informations sur les étiquettes de contrôle, les icônes, les liens, etc..

Les info-bulles doivent être utilisés avec parcimonie ou pas du tout. Elles peuvent être une interruption au client, donc n’incluez pas une info-bulle simplement se répète une étiquette de l’état ou les plus subtiles. Il doit toujours ajouter des informations précieuses.

|    Contexte                                 |    Comment écrire les info-bulles    |
|    -----------------------                 |    -------------------------    |
|Lorsqu’un contrôle ou un élément d’interface utilisateur est sans étiquette …|Utilisez un simple et descriptive groupe nominal. Exemple :<br> Mise en surbrillance du stylet |
|Lorsqu’un élément d’interface utilisateur est étiqueté, mais son objectif a besoin de clarifications …|<ul><li>Décrivez brièvement ce que vous pouvez effectuer avec cet élément d’interface utilisateur. </li><li>Utiliser le formulaire de verbe impératif. Par exemple, «rechercher du texte dans ce fichier» (pas «recherche du texte dans ce fichier»).</li><li>N’incluez pas ponctuation finale sauf s’il existe plusieurs phrases complètes.</li> </ul>|
|Lorsqu’une étiquette de texte est tronqué ou susceptible de tronquer dans certaines langues …|<ul><li>Fournissez l’étiquette non tronquée dans l’info-bulle.</li><li>Facultatif: Sur une autre ligne, fournissent une description de clarifier, mais uniquement si nécessaire.</li><li>Ne fournissez pas une info-bulle si les informations non tronquées sont fournie un autre emplacement sur la page ou d’un flux.</li></ul>|
|Si un raccourci clavier est disponible.|<ul><li>Facultatif: Fournir le raccourci clavier entre parenthèses après l’étiquette ou une expression descriptive, par exemple, «Imprimer (Ctrl + P)» ou «Rechercher du texte dans ce fichier (Ctrl + F)»</li><li>Il est OK pour ajouter un raccourci clavier utiles à une info-bulle clarifier, mais évitez d’ajouter une info-bulle uniquement pour afficher un raccourci clavier. </li></ul>|