---
title: Guide de style pour la conception et le texte de l’interface utilisateur Windows Admin Center
description: L’interface utilisateur de Windows Admin Center de texte et la conception de guide de style SDK
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: be41267d6584002ebf87e5fe828a41575d305e1b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445910"
---
# <a name="windows-admin-center-ui-text-and-design-style-guide"></a>Guide de style pour la conception et le texte de l’interface utilisateur Windows Admin Center

>S'applique à : Windows Admin Center

Cette rubrique décrit l’approche générale de la rédaction du texte de l’interface utilisateur pour Windows Admin Center, ainsi que certaines de nos conventions et méthodes spécifiques.

Windows Admin Center et toutes les extensions doivent respecter les [principes en matière de voix de Microsoft](https://docs.microsoft.com/style-guide/brand-voice-above-all-simple-human) afin que l’expérience soit facile à utiliser et conviviale. Ce guide de style s’appuie sur ces principes en matière de voix, ainsi que sur le [Microsoft Writing Style Guide](https://docs.microsoft.com/style-guide/welcome/), donc veillez à consulter ces deux ressources pour des informations sur des éléments tels que l'[accessibilité](https://docs.microsoft.com/style-guide/accessibility/accessibility-guidelines-requirements), les [acronymes](https://docs.microsoft.com/style-guide/acronyms) et le [choix de mots](https://docs.microsoft.com/style-guide/word-choice/) tels que [s'il vous plaît](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/p/please) et [désolé](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/s/sorry).

## <a name="buttons"></a>Boutons

- Boutons doivent être en un seul mot dans la mesure du possible, en particulier si vous souhaitez localiser votre outil. Deux ou trois est OK, mais essayez d’éviter de plus. Si vous avez quatre mots ou plus, il serait préférable d’utiliser un contrôle de lien.
- Les étiquettes de bouton doivent être concises, spécifiques et explicites. Au lieu d’un bouton générique « Soumettre », utilisez un verbe correspondant à l’action de l’utilisateur, tel que « Créer », « Supprimer », « Ajouter », « Formater », etc.
- Si un bouton suit une question, son étiquette doit clairement correspondre à la question (généralement « Oui » ou « Non »).

## <a name="capitalization"></a>Mise en majuscules

Nous suivons le style de Microsoft concernant la [mise en majuscules](https://docs.microsoft.com/style-guide/capitalization) : utiliser le style Majuscule en début de phrase pour quasiment tout.

| Élément d’interface utilisateur              |Mise en majuscules|Commentaires|
|-------------------------|--------------|--------|
|Badges (par exemple, PRÉVERSION) |Tout en majuscules      ||
|Tout le reste          |Majuscule en début de phrase|Toutefois, il existe certaines exceptions où nous exposons des propriétés d'objet à partir de WMI ou PowerShell se trouvant en dehors de notre contrôle.|

## <a name="colons"></a>Deux-points

Utiliser des signes deux-points pour introduire des listes. Exemple :

    Choose one of the following:
    Cats
    Dogs
    Quokkas

N’utilisez pas signes deux-points dans les textes d’interface utilisateur quand une étiquette est sur une autre ligne à partir de la chose étiquettes ou lorsqu’il existe une distinction claire entre l’étiquette et la chose qu’il est l’étiquetage.

Utilisez signes deux-points dans les textes d’interface utilisateur quand une étiquette est sur la même ligne que le texte, il est l’étiquette et vous avez besoin éviter les deux éléments ensemble.

## <a name="confirmation-messages"></a>Messages de confirmation

Boîtes de dialogue de confirmation sont utiles lors de la poursuite peut avoir des résultats inattendus, tels que la perte de données. Elles doivent contenir des informations analysables, utile avec un résultat clair, en particulier pour les événements qui ne peut pas être annulée. 

- Assurez-vous qu’une confirmation est nécessaire. S’il n’existe aucune information de nouveau pour offrir (par exemple, « êtes-vous ? »), puis un message de confirmation n’est peut-être pas nécessaire.  
- Vérifiez que le client souhaite poursuivre l’action.
- Assurez-vous que l’instruction principale (titre) et le texte explicatif (corps) ne sont pas redondantes.
- Dans l’en-tête, définissez les résultats possibles en tant qu’une question ou une instruction sur ce qui se produira ensuite. Par exemple, « effacer toutes les données sur ce lecteur ? ou « Vous êtes sur le point d’effacer toutes vos données ».
- Ajouter des détails dans le corps. S’il existe une variable, telles que le nom de l’élément que vous êtes sur la modification, vous pouvez l’inclure ici.
- Inclure une simple question (soit dans l’en-tête ou dans le corps) qui encadre un choix clair entre les deux boutons d’action.
- Pour un choix complexe, utilisez Oui/non boutons, encourager la lecture minutieuse. Pour un choix plus simple, utilisez les boutons qui sont spécifiques à l’action, telles que supprimer tous ou sur Annuler.

## <a name="first-run-experiences"></a>Expériences de première exécution

La première fois qu’un utilisateur visite une page, vous avez la possibilité de les aider à prendre en main votre outil. Il peut s'agir des éléments suivants :

- Une chaîne de texte dans une page vide avec de courtes instructions sur la prise en main : par exemple, « sélectionnez Ajouter pour ajouter une application ».
- Un lien vers le contrôle qui guide l’utilisateur pour la prise en main : par exemple, « Ajouter une application pour commencer ».
- Une petite animation ou une courte vidéo montrant à l’utilisateur comment commencer

Voici quelques conseils de notre guide de style Windows :

### <a name="1-be-helpful"></a>1. Être efficace

- Évitez le style et le langage marketing.
- Lorsque vous effectuez une démonstration ou proposez quelque chose, vérifiez que le résultat final est clair ; se contenter de montrer au client comment effectuer une opération n’est pas efficace s'il ne sait pas quel en est le but.
- Ne présentez pas de conseils si le client n’en a pas besoin.

### <a name="2-show-dont-tell"></a>2. Afficher, ne dites pas

Gardez le texte le plus simple possible (pensez à de petites animations ou à des vidéos).

### <a name="3-dont-overwhelm"></a>3. Ne pas surcharger

- Limitez les fenêtres contextuelles et les conseils à 4 par session d’utilisation combinés, y compris les notifications système et les notifications de shell.
- Assurez-vous que la synchronisation des fenêtres contextuelles est utile.
- N'empêchez pas le client d’effectuer une opération.
- Assurez-vous que les fenêtres contextuelles sont faciles à ignorer.

### <a name="4-keep-it-contextual"></a>4. Conservez-la contextuelles

- Les enseignements sont plus efficaces lorsqu'ils sont présentés au bon moment.
- Si vous créez des didacticiels ou des diaporamas, restez concret.
- Évitez tout « discours » marketing : concentrez-vous sur des conseils et des astuces spécifiques.
- Donnez la possibilité aux clients de revenir au didacticiel ultérieurement, le cas échéant (les utilisateurs ne retiennent souvent pas les informations la première fois, mais les instructions d’installation ne peuvent être pertinentes qu’une seule fois).
- Un message d'état vide est l’endroit idéal pour l'information et/ou la convivialité : gardez-le simple et informatif.

### <a name="5-minimize-painful-setup"></a>5. Réduire le programme d’installation pénible

Lorsque vous avez besoin que le client effectue une autre action pour obtenir une valeur ajoutée complète (s'inscrire à un service en ligne, etc.), rendez-la aussi simple que possible.

- Le message doit être court et direct.
- Évitez de le décourager. Si possible, donnez-lui un moyen de se connecter à partir de l’endroit où il se trouve.
- Si possible, proposez-lui de le faire plus tard, puis rappelez-le lui ultérieurement.
- Si vous le faites sortir de son expérience, donnez-lui un moyen de revenir rapidement et facilement.

## <a name="help-links"></a>Liens d’aide

Voici quelques conseils de notre guide de style Windows :

### <a name="when-should-we-provide-a-help-link"></a>Lorsque nous devons pour fournir un lien d’aide ?

Presque jamais. Fournir un lien d’aide uniquement lorsque :

- Il existe une question évidente et importante que les clients sont susceptibles d’avoir lorsqu’ils sont dans l’interface utilisateur de la réponse à ce qui les aideront à réussir la tâche de l’interface utilisateur. 
- Ne comporte pas assez de place dans l’interface utilisateur pour fournir la quantité d’informations nécessaires pour les utilisateurs à réussir à la tâche de l’interface utilisateur. 

### <a name="where-should-help-links-appear"></a>Où doit liens d’aide apparaissent ? 

- Liens de texte doivent apparaître comme proche de l’élément d’interface utilisateur qui est dirigée vers l’aide que possible. 
- Si vous devez fournir un lien de texte qui s’applique à un écran de l’interface utilisateur entière, placez-le en bas à gauche de l’écran. 
- Si vous fournissez un lien via un bouton d’aide ( ?), l’info-bulle doit être « Help ».

### <a name="what-url-should-we-use"></a>Quelle URL devons-nous utiliser ?

Ne jamais lier directement à une adresse web, à la place utiliser un service de la redirection.

Les développeurs Microsoft doivent utiliser un FWLink, sauf si elle est un lien d’aide que les utilisateurs doivent taper manuellement, dans ce cas, utilisez un lien aka.ms (tant que la cible de l’URL est un site Web qui reconnaît automatiquement le paramètres régionaux du navigateur, telles que Docs.microsoft.com)

### <a name="text-guidelines"></a>Instructions de texte 

- Utiliser des phrases complètes.
- N’incluez pas de ponctuation, à l’exception des points d’interrogation de fin. 
- Vous n’avez pas besoin d’utiliser le même texte en tant que le titre de la tâche ; utiliser le texte qui est pertinent dans le contexte de l’interface utilisateur, mais veillez à ce qu’il existe une connexion logique entre les deux. Exemple : 
- Lien d’aide : Quels sont les risques d’autoriser les exceptions ? 
- Titre de rubrique d’aide : « Autoriser un programme à communiquer via le pare-feu de Windows »
- Être aussi précis que possible sur le contenu de la rubrique d’aide. 
    - Notre style
        - Comment Windows Firewall m’aide-t-il à protéger mon ordinateur ?
        - Pourquoi les points importants peuvent améliorer une image
    - Pas notre style
        - Plus d’informations sur le pare-feu de Windows
        - En savoir plus sur la gestion des couleurs
        - En savoir plus
- Utiliser la phrase entière pour le texte de lien, pas seulement les mots clés. 
    - Notre style 
        - [Quels sont les risques d’autoriser les exceptions ?]()
    - Pas notre style
        - Quelles sont les [les risques d’autoriser les exceptions]()? 

## <a name="error-messages"></a>Messages d’erreur

Voici quelques conseils adaptés à partir du Guide de Style Windows :

Écriture d’un message correct est un compromis entre fournir suffisamment explication mais n’est pas trop techniques ; entre l’informels et personnalisable, mais pas gênant ou offensant.

### <a name="general-guidelines"></a>Recommandations générales

Utiliser un message par cas d’erreur.

#### <a name="headings"></a>En-têtes

- Laissez cette brève et expliquer plus concise le problème ou **dans l’idéal, que faire**. <br>Des surfaces d’interface utilisateur peuvent avoir des en-têtes de tronquer au lieu d’encapsuler lorsqu’ils sont trop longs, donc tenez-vous pour ces.
- Utiliser la solution dans l’en-tête s’il s’agit d’une étape simple.
- Assurez-vous que l’en-tête est liée directement sur le bouton dans le cas où le lecteur ignore le texte du corps.
- Évitez d’utiliser « Un problème est survenu » dans les en-têtes, si vous ne disposez pas d’autre choix. Être plus précis sur le problème.
- Évitez les variables (tels que les noms de fichier, dossier et application) dans les en-têtes. Placez-les dans le corps.

#### <a name="body"></a>Corps

- Si l’en-tête suffisamment explique que le problème ou la solution, vous n’avez pas besoin corps de texte.
- Ne répétez pas le titre dans le message avec un libellé légèrement différent.
- Communiquer clairement et concise ce qui est la solution.
- Concentrez-vous sur l’octroi de tout d’abord les faits.
- Les utilisateurs de l’erreur intuitives.
- S’il existe un code d’erreur associé à l’erreur et si vous pensez qu’y compris le code d’erreur peuvent vous aider au client ou le support technique Microsoft à effectuer des recherches sur le problème, incluez-le directement sous le texte du corps et l’écrire comme suit :

    Code d’erreur : ###

    Si le client possède toutes les informations nécessaires pour résoudre l’erreur sans le code, vous n’avez pas besoin de l’inclure.

#### <a name="buttons"></a>Boutons

- Écrire du texte du bouton afin qu’il soit une réponse spécifique à l’instruction principale. Si tel n’est pas possible, utilisez « Fermer » pour le texte du bouton licenciement (au lieu de « OK » ou « Terminé »).
- Si vous avez plusieurs boutons, vous pouvez créer le bouton plus à gauche l’action de que l’utilisateur est encouragé à prendre. Rendre le bouton plus à droite de l’action plus conservatrice, comme « Cancel ».

#### <a name="help-links"></a>Liens d’aide

Prendre en compte uniquement les liens d’aide pour les messages d’erreur que vous ne pouvez apporter spécifiques et exploitables.

## <a name="null-state-text"></a>Texte de l’état null

Voici certains aide dans le Guide de Style Windows.

État NULL se produit lorsque les données client ou le contenu est absent à partir d’une application ou une fonctionnalité, lorsque aucune résultats ne sont retournés après une recherche, ou lorsque le manque des informations à partir d’un formulaire, telles que la facturation des informations pour une transaction.

### <a name="guidelines"></a>Recommandations

- Si possible, utilisez les situations d’état null comme une opportunité pour former des personnes sur l’utilisation de la fonctionnalité (par exemple, comment ajouter de la musique, où pour rechercher des images, etc.)  
  - Si vous avez un titre dans votre interface utilisateur, expliquent l’action à entreprendre pour « réparer » de l’état null (par exemple, « ajouter une musique ») 
  - Amusez-vous avec le texte. Cet espace peut être une opportunité pour fournir la satisfaction dans la mesure où il sera probablement pas vu plusieurs fois. 
  - Éviter les « Il est vide ici. » Il s’agit triste et a été utilisée de manière excessive. 
  - Éviter des questions telles que « Le n’avez pas connecté votre imprimante ? » OK pour utiliser une seule fois, mais ce format a tendance à obtenir galvaudés et questions solliciter davantage la charge/sur le client. Il peut également se sentent évident. 
  - Plusieurs types de texte de l’état null est une bonne chose. 

### <a name="examples"></a>Exemples

- « Ajouter un utilisateur en tant que favori, et vous pouvez les voir ici ».
- « Ores et déjà primes ni jeu clips vous sommes particulièrement heureux de Ajoutez-les à votre vitrine. »
- « Personne dans encore un tiers. Démarrer un ! »
- « Quand quelqu'un vous ajoute comme friend, vous verrez les ici. »
- « Lorsque vous fasse le travail comme déverrouiller des primes, enregistrer des clips de jeu, et ajouter des amis, vous verrez qu’il tous ici. »
- « Vos amis favoris seront afficheront ici, afin que vous puissiez voir lorsqu’ils sont en ligne et ce qu’elles jusqu'à. »

## <a name="punctuation"></a>Ponctuation

- Aucune ponctuation finale (points, points d’interrogation) pour des titres ou des phrases incomplètes. Une exception est dans une boîte de dialogue de confirmation dans laquelle le titre pose une question
- Utilisez les instructions du Guide de Style Microsoft sur les [points](https://docs.microsoft.com/style-guide/punctuation/periods) et les [points d’interrogation](https://docs.microsoft.com/style-guide/punctuation/question-marks).

## <a name="status-messages"></a>Messages d'état

Les messages d’état se composent de notifications et de messages contextuels (toast).

|Type de chaîne         | Notes                               |
|------------        |-------------------------------------|
|Toast               |Majuscule en début de phrase avec ponctuation de fin : de préférence avec une variable d’objet pour que les utilisateurs comprennent à quel objet le message s’applique au cas où ils seraient sortis de l’objet|
|Titre de notification|Majuscule en début de phrase sans ponctuation finale (il s’agit d’un titre) : de préférence avec une variable d’objet|
|Détails de la notification|Phrases complètes, de préférence avec un lien vers l’interface utilisateur qui affiche l’objet|

Voici quelques recommandations détaillées sur les messages de notification :

|Type de chaîne         | Notes                               |
|------------        |-------------------------------------|
|Démarré             |Omettez dans la mesure du possible : généralement vous pouvez vous contenter de passer au message en cours afin de réduire les distractions.|
|En cours         |Commencez avec le verbe de l’action que vous effectuez et terminez par des points de suspension pour indiquer une opération en cours. Voici un exemple :<br> *Création du volume « Données client »...*|
|Opération réussie             |Commencez par l'action que le logiciel vient d'effectuer et finissez par « réussi(e) ». Voici un exemple :<br> *Créé avec succès le volume « Données client ».*|
|Échec             |Démarrez avec « Impossible de » et terminez par ce que le logiciel n’a pas pu faire. Voici un exemple :<br> *Impossible de créer le volume « Données client ».*|

## <a name="tooltips"></a>Info-bulles

Brièvement, bonne info-bulles décrivent les contrôles sans étiquette ou fournissent quelques informations supplémentaires pour les contrôles étiquetés, lorsque cela est utile. Ils peuvent également aider les clients à naviguer de l’interface utilisateur en offrant supplémentaires, non redondantes : plus d’informations sur les étiquettes de contrôle, des icônes, des liens, etc.

Info-bulles doivent être utilisés avec parcimonie ou pas du tout. Ils peuvent être une interruption au client, afin de ne pas inclure une info-bulle qui simplement se répète une étiquette ou États l’évident. Il doit toujours ajouter des informations très utiles.

|    Contexte                                 |    Comment écrire les info-bulles    |
|    -----------------------                 |    -------------------------    |
|Quand un contrôle ou un élément d’interface utilisateur est sans étiquette...|Utiliser une expression nominale simple et descriptif. Exemple :<br> Mise en surbrillance du stylet |
|Quand un élément d’interface utilisateur est étiqueté, mais son objectif doit clarification...|<ul><li>Décrivez brièvement ce que vous pouvez faire avec cet élément d’interface utilisateur. </li><li>Utilisez la forme verbe impératif. Par exemple, « rechercher du texte dans ce fichier » (pas « recherche du texte dans ce fichier »).</li><li>N’incluez pas ponctuation sauf s’il existe plusieurs phrases complètes.</li> </ul>|
|Quand une étiquette de texte est tronqué ou probable à tronquer dans certaines langues...|<ul><li>Fournir l’étiquette non tronqué dans l’info-bulle.</li><li>Facultatif : Sur une autre ligne, fournissez une description de clarification, mais uniquement si nécessaire.</li><li>Ne fournissez pas une info-bulle si les informations non tronquée sont fournie ailleurs sur la page ou le flux.</li></ul>|
|S’il existe un raccourci clavier...|<ul><li>Facultatif : Fournir le raccourci clavier dans les parenthèses qui suivent l’étiquette ou une expression descriptive, par exemple « Print (Ctrl + P) » ou « Rechercher du texte dans ce fichier (Ctrl + F) »</li><li>Il est OK pour ajouter un raccourci clavier utiles à une info-bulle éclaircissement, mais évitez d’ajouter une info-bulle uniquement pour afficher un raccourci clavier. </li></ul>|