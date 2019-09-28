---
title: Guide de style pour la conception et le texte de l’interface utilisateur Windows Admin Center
description: Texte de l’interface utilisateur du centre d’administration Windows et SDK du Guide de style de conception
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 159202fcb8c6125134154094e67e862ce8eb12ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357030"
---
# <a name="windows-admin-center-ui-text-and-design-style-guide"></a>Guide de style pour la conception et le texte de l’interface utilisateur Windows Admin Center

>S'applique à : Windows Admin Center

Cette rubrique décrit l’approche générale de la rédaction du texte de l’interface utilisateur pour Windows Admin Center, ainsi que certaines de nos conventions et méthodes spécifiques.

Windows Admin Center et toutes les extensions doivent respecter les [principes en matière de voix de Microsoft](https://docs.microsoft.com/style-guide/brand-voice-above-all-simple-human) afin que l’expérience soit facile à utiliser et conviviale. Ce guide de style s’appuie sur ces principes en matière de voix, ainsi que sur le [Microsoft Writing Style Guide](https://docs.microsoft.com/style-guide/welcome/), donc veillez à consulter ces deux ressources pour des informations sur des éléments tels que l'[accessibilité](https://docs.microsoft.com/style-guide/accessibility/accessibility-guidelines-requirements), les [acronymes](https://docs.microsoft.com/style-guide/acronyms) et le [choix de mots](https://docs.microsoft.com/style-guide/word-choice/) tels que [s'il vous plaît](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/p/please) et [désolé](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/s/sorry).

## <a name="buttons"></a>Boutons

- Boutons doivent être en un seul mot dans la mesure du possible, en particulier si vous souhaitez localiser votre outil. Deux ou trois sont corrects, mais essayez d’éviter plus longtemps. Si vous avez quatre mots ou plus, il est préférable d’utiliser un contrôle de lien.
- Les étiquettes de bouton doivent être concises, spécifiques et explicites. Au lieu d’un bouton générique « Soumettre », utilisez un verbe correspondant à l’action de l’utilisateur, tel que « Créer », « Supprimer », « Ajouter », « Formater », etc.
- Si un bouton suit une question, son étiquette doit clairement correspondre à la question (généralement « Oui » ou « Non »).

## <a name="capitalization"></a>Mise en majuscules

Nous suivons le style de Microsoft concernant la [mise en majuscules](https://docs.microsoft.com/style-guide/capitalization) : utiliser le style Majuscule en début de phrase pour quasiment tout.

| Élément d’interface utilisateur              |Mise en majuscules|Commentaires|
|-------------------------|--------------|--------|
|Badges (par exemple, PRÉVERSION) |Tout en majuscules      ||
|Tout le reste          |Majuscule en début de phrase|Toutefois, il existe certaines exceptions où nous exposons des propriétés d'objet à partir de WMI ou PowerShell se trouvant en dehors de notre contrôle.|

## <a name="colons"></a>Virgules

Utilisez les deux-points pour introduire des listes. Exemple :

    Choose one of the following:
    Cats
    Dogs
    Quokkas

N’utilisez pas de signe deux-points dans le texte de l’interface utilisateur lorsqu’une étiquette est sur une autre ligne que celle qu’elle étiquette ou lorsqu’il existe une distinction claire entre l’étiquette et l’étiquette.

Utilisez le signe deux-points dans le texte de l’interface utilisateur lorsqu’une étiquette se trouve sur la même ligne que le texte qu’elle étiquette et que vous devez conserver les deux éléments en cours d’exécution.

## <a name="confirmation-messages"></a>Messages de confirmation

Les boîtes de dialogue de confirmation sont utiles lorsque la poursuite de l’opération peut entraîner des résultats inattendus, tels que la perte de données. Ils doivent contenir des informations utiles et pouvant être analysées avec un résultat clair, en particulier pour les événements qui ne peuvent pas être inversés. 

- Assurez-vous qu’une confirmation est nécessaire. S’il n’existe pas de nouvelles informations à proposer (par exemple, « êtes-vous sûr ? »), un message de confirmation n’est peut-être pas nécessaire.  
- Vérifiez que le client souhaite poursuivre l’action.
- Assurez-vous que l’instruction principale (titre) et le texte explicatif (corps) ne sont pas redondants.
- Dans l’en-tête, définissez les résultats possibles comme une question ou une instruction sur ce qui se passe ensuite. Par exemple, «effacer toutes les données sur ce lecteur ? ou « vous êtes sur le ou l’effacement de toutes vos données ».
- Ajoutez des détails dans le corps. S’il existe une variable, telle que le nom de l’élément que vous êtes en train de modifier, incluez-la ici.
- Incluez une question simple (dans l’en-tête ou dans le corps) qui encadre un choix clair entre deux boutons d’action.
- Pour un choix complexe, utilisez les boutons Oui/non, qui encouragent la lecture. Pour un choix plus simple, utilisez des boutons spécifiques à l’action, tels que supprimer tout ou annuler.

## <a name="first-run-experiences"></a>Expériences de première exécution

La première fois qu’un utilisateur visite une page, vous avez la possibilité de les aider à prendre en main votre outil. Il peut s'agir des éléments suivants :

- Une chaîne de texte dans une page vide avec de courtes instructions sur la prise en main : par exemple, « sélectionnez Ajouter pour ajouter une application ».
- Un lien vers le contrôle qui guide l’utilisateur pour la prise en main : par exemple, « Ajouter une application pour commencer ».
- Une petite animation ou une courte vidéo montrant à l’utilisateur comment commencer

Voici quelques conseils de notre guide de style Windows :

### <a name="1-be-helpful"></a>1. Être efficace

- Évitez le style et le langage marketing.
- Lorsque vous faites une démonstration ou Suggérez un nom, assurez-vous que le résultat final est clair ; le fait de montrer simplement au client comment faire une action n’est pas efficace s’il ne sait pas pourquoi.
- Ne présentez pas de conseils si le client n’en a pas besoin.

### <a name="2-show-dont-tell"></a>2. Afficher, ne pas dire

Gardez le texte le plus simple possible (pensez à de petites animations ou à des vidéos).

### <a name="3-dont-overwhelm"></a>3. Ne vous inquiétez pas

- Limitez les fenêtres contextuelles et les conseils à 4 par session d’utilisation combinés, y compris les notifications système et les notifications de shell.
- Assurez-vous que la synchronisation des fenêtres contextuelles est utile.
- N’empêchez pas le client d’effectuer une opération.
- Assurez-vous que les fenêtres contextuelles sont faciles à ignorer.

### <a name="4-keep-it-contextual"></a>4. Rester contextuel

- Les enseignements sont plus efficaces lorsqu'ils sont présentés au bon moment.
- Si vous créez des didacticiels ou des diaporamas, restez concret.
- Évitez tout « discours » marketing : concentrez-vous sur des conseils et des astuces spécifiques.
- Permet aux clients de revenir ultérieurement au didacticiel, si nécessaire (les gens ne conservent souvent pas les informations la première fois, mais les instructions de configuration ne sont pertinentes qu’une seule fois).
- Un message d'état vide est l’endroit idéal pour l'information et/ou la convivialité : gardez-le simple et informatif.

### <a name="5-minimize-painful-setup"></a>5. Réduire la configuration pénible

Lorsque vous avez besoin que le client effectue une autre action pour obtenir une valeur ajoutée complète (s'inscrire à un service en ligne, etc.), rendez-la aussi simple que possible.

- Le message doit être court et direct.
- Évitez de le décourager. Si possible, donnez-lui un moyen de se connecter à partir de l’endroit où il se trouve.
- Si possible, proposez-lui de le faire plus tard, puis rappelez-le lui ultérieurement.
- Si vous le faites sortir de son expérience, donnez-lui un moyen de revenir rapidement et facilement.

## <a name="help-links"></a>Liens d’aide

Voici quelques conseils de notre guide de style Windows :

### <a name="when-should-we-provide-a-help-link"></a>Quand dois-je fournir un lien d’aide ?

Presque jamais. Fournissez un lien d’aide uniquement lorsque :

- Il existe une question évidente et importante que les clients sont susceptibles d’avoir lorsqu’ils se trouvent dans l’interface utilisateur, la réponse qui les aidera à parvenir à la tâche de l’interface utilisateur. 
- Il n’y a pas assez d’espace dans l’interface utilisateur pour fournir la quantité d’informations nécessaires pour que les utilisateurs puissent se faire correctement au niveau de la tâche d’interface utilisateur. 

### <a name="where-should-help-links-appear"></a>Où les liens d’aide doivent-ils s’afficher ? 

- Les liens de texte doivent apparaître à proximité de l’élément d’interface utilisateur sur lequel l’aide est dirigée le plus possible. 
- Si vous devez fournir un lien de texte qui s’applique à l’intégralité de l’écran de l’interface utilisateur, placez-le en bas à gauche de l’écran. 
- Si vous fournissez un lien via un bouton d’aide ( ?), l’info-bulle doit être « Help ».

### <a name="what-url-should-we-use"></a>Quelle URL dois-je utiliser ?

N’appliquez jamais une liaison directe à une adresse Web, à la place, utilisez un service de redirection.

Les développeurs Microsoft doivent utiliser un FWLink, sauf lorsqu’il s’agit d’un lien d’aide que les utilisateurs peuvent avoir à taper manuellement. dans ce cas, utilisez un lien aka.ms (tant que la cible de l’URL est un site Web qui reconnaît automatiquement les paramètres régionaux du navigateur, comme Docs.microsoft.com)

### <a name="text-guidelines"></a>Directives relatives au texte 

- Utilisez des phrases entières.
- N’incluez pas de ponctuation de fin, sauf pour les points d’interrogation. 
- Vous n’avez pas besoin d’utiliser le même texte que le titre de la tâche. Utilisez du texte qui donne un sens dans le contexte de l’interface utilisateur, mais assurez-vous qu’il existe une connexion logique entre les deux. Exemple : 
- Lien d’aide : Quels sont les risques liés à l’autorisation des exceptions ? 
- Titre de la rubrique d’aide : « Autoriser un programme à communiquer via le pare-feu Windows »
- Soyez aussi précis que possible sur le contenu de la rubrique d’aide. 
    - Notre style
        - Comment le pare-feu Windows contribue-t-il à protéger mon ordinateur ?
        - Pourquoi les mises en évidence peuvent améliorer une image
    - Pas notre style
        - Plus d’informations sur le pare-feu Windows
        - En savoir plus sur la gestion des couleurs
        - En savoir plus
- Utilisez la phrase entière pour le texte du lien, pas seulement les mots clés. 
    - Notre style 
        - [Quels sont les risques liés à l’autorisation des exceptions ?]()
    - Pas notre style
        - Quels sont les [risques liés à l’autorisation des exceptions]()? 

## <a name="error-messages"></a>Messages d’erreur

Voici quelques conseils adaptés à partir du Guide de style Windows :

L’écriture d’un bon message est un juste équilibre entre fournir suffisamment d’explications, mais pas être trop technique. entre un usage fortuit et un homme, mais pas ennuyeux ou offensant.

### <a name="general-guidelines"></a>Recommandations générales

Utilisez un message par cas d’erreur.

#### <a name="headings"></a>En-têtes

- Tenez-vous informé et expliquez de façon concise le problème ou l' **idéal**. <br>Certaines surfaces de l’interface utilisateur peuvent avoir des en-têtes tronqués au lieu d’être encapsulés lorsqu’ils sont trop longs, donc gardez un œil pour ceux-ci.
- Utilisez la solution dans l’en-tête s’il s’agit d’une étape simple.
- Assurez-vous que l’en-tête est directement associé au bouton au cas où le lecteur ignore le corps du texte.
- Évitez d’utiliser « un problème est survenu » dans les en-têtes, sauf si vous n’avez pas d’autre choix. Soyez plus précis sur le problème.
- Évitez d’utiliser des variables (telles que les noms de fichier, de dossier et d’application) dans les en-têtes. Placez-les dans le corps.

#### <a name="body"></a>Corps

- Si le titre présente suffisamment d’explications sur le problème ou la solution, vous n’avez pas besoin de texte du corps.
- Ne répétez pas le titre dans le message avec un libellé légèrement différent.
- Communiquez clairement et de façon concise la solution.
- Concentrez-vous d’abord sur la présentation des faits.
- Ne vous inquiétez pas des utilisateurs pour l’erreur.
- Si un code d’erreur est associé à l’erreur et si vous pensez que l’inclusion du code d’erreur peut aider le client ou le support Microsoft à rechercher le problème, incluez-le directement sous le corps du texte et écrivez-le comme suit :

    Code d’erreur : ####

    Si le client dispose de toutes les informations nécessaires pour résoudre l’erreur sans le code, vous n’avez pas besoin de l’inclure.

#### <a name="buttons"></a>Boutons

- Écrire le texte du bouton afin qu’il s’agisse d’une réponse spécifique à l’instruction principale. Si ce n’est pas possible, utilisez « fermer » pour le texte du bouton à ignorer (au lieu de « OK » ou « terminé »).
- Si vous avez plus d’un bouton, définissez le bouton le plus à gauche comme l’action que l’utilisateur est encouragé à prendre. Faites du bouton le plus à droite l’action la plus conservatrice, par exemple « annuler ».

#### <a name="help-links"></a>Liens d’aide

Ne tenez compte que des liens d’aide pour les messages d’erreur que vous ne pouvez pas rendre spécifiques et exploitables.

## <a name="null-state-text"></a>Texte d’État null

Voici une aide du Guide de style Windows.

L’État null se produit lorsque des données client ou du contenu sont absentes d’une application ou d’une fonctionnalité, quand aucun résultat n’est retourné après une recherche, ou lorsque des informations obligatoires sont manquantes dans un formulaire, telles que des informations de facturation pour une transaction.

### <a name="guidelines"></a>Recommandations

- Si possible, utilisez des situations d’État null pour informer les utilisateurs de l’utilisation de la fonctionnalité (par exemple, ajouter de la musique, où trouver des images, etc.)  
  - Si vous avez un titre dans votre interface utilisateur, Expliquez l’action à entreprendre pour « corriger » l’État null (par exemple, « ajouter une musique ») 
  - Amusez-vous avec le texte. Cet espace peut être une opportunité de fournir le plaisir, car il ne sera probablement pas visible plusieurs fois. 
  - Évitez « c’est impossible ici ». Il s’agit d’un Sad qui a été utilisé. 
  - Évitez les questions telles que « vous n’êtes pas connecté à votre imprimante ? » Vous pouvez utiliser une seule fois, mais ce format tend à être utilisé, et les questions posent davantage de charge/pression sur le client. Cela peut également paraître condescendant. 
  - La diversité dans le texte d’État null est une bonne chose. 

### <a name="examples"></a>Exemples

- « Ajoutez quelqu’un en tant que favori, et vous les verrez ici. »
- «Avez-vous des réalisations ou des clips de jeu que vous êtes particulièrement fier ? Ajoutez-les à votre vitrine.»
- «Personne n’a encore été dans un tiers. Démarrez-en un !»
- « Quand quelqu’un vous ajoute comme ami, vous les verrez ici. »
- « Quand vous effectuez des tâches de déverrouillage, enregistrez des clips de jeu et ajoutez des amis, vous le verrez ici. »
- « Vos amis préférés s’affichent ici pour vous permettre de voir quand ils sont en ligne et à quoi ils sont confrontés ».

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
|En cours         |Commencez avec le verbe de l’action que vous effectuez et terminez par des points de suspension pour indiquer une opération en cours. Voici un exemple :<br> *Création du volume « données client »...*|
|Succès             |Commencez par l'action que le logiciel vient d'effectuer et finissez par « réussi(e) ». Voici un exemple :<br> *Le volume « données client » a été créé.*|
|Échec             |Démarrez avec « Impossible de » et terminez par ce que le logiciel n’a pas pu faire. Voici un exemple :<br> *Impossible de créer le volume « données client ».*|

## <a name="tooltips"></a>Info-bulles

Les bonnes info-bulles décrivent brièvement les contrôles sans étiquette ou fournissent un peu d’informations supplémentaires pour les contrôles étiquetés, lorsque cela est utile. Ils peuvent également aider les clients à naviguer dans l’interface utilisateur en offrant des informations supplémentaires, non redondantes, sur les étiquettes de contrôle, les icônes, les liens, etc.

Les info-bulles doivent être utilisées avec parcimonie ou pas du tout. Il peut s’agir d’une interruption du client. n’incluez donc pas d’info-bulle qui répète simplement une étiquette ou indique la valeur évidente. Elle doit toujours ajouter des informations précieuses.

|    Contexte                                 |    Comment écrire les info-bulles    |
|    -----------------------                 |    -------------------------    |
|Lorsqu’un contrôle ou un élément d’interface utilisateur n’est pas labellisé...|Utilisez une phrase nominale simple et descriptive. Exemple :<br> Mise en surbrillance du stylet |
|Lorsqu’un élément d’interface utilisateur est étiqueté, mais que son objectif a besoin d’être clarifié...|<ul><li>Décrivez brièvement ce que vous pouvez faire avec cet élément d’interface utilisateur. </li><li>Utilisez le format de verbe impératif. Par exemple, « Rechercher le texte dans ce fichier » (et non « recherche le texte dans ce fichier »).</li><li>N’incluez pas de ponctuation finale à moins qu’il y ait plusieurs phrases complètes.</li> </ul>|
|Quand une étiquette de texte est tronquée ou est susceptible d’être tronquée dans certaines langues...|<ul><li>Fournissez l’étiquette non tronquée dans l’info-bulle.</li><li>Facultatif : Sur une autre ligne, fournissez une description de clarification, mais uniquement si nécessaire.</li><li>Ne fournissez pas d’info-bulle si les informations non tronquées sont fournies ailleurs sur la page ou le Flow.</li></ul>|
|Si un raccourci clavier est disponible...|<ul><li>Facultatif : Indiquez le raccourci clavier entre parenthèses à la suite de l’étiquette ou de l’expression descriptive, par exemple « Imprimer (Ctrl + P) » ou « Rechercher le texte dans ce fichier (Ctrl + F) »</li><li>Il est recommandé d’ajouter un raccourci clavier utile à une info-bulle clarify, mais d’éviter d’ajouter une info-bulle uniquement pour afficher un raccourci clavier. </li></ul>|