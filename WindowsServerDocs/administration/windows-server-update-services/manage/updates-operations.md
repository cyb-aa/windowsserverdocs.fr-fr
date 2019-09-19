---
title: Opérations de mises à jour
description: 'Rubrique Windows Server Update Service (WSUS) : comment gérer les mises à jour, y compris le processus d’approbation'
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cb7ff54-3014-4e91-842a-a7b831ea59ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2618586fdc38588bb58e122116345eb88a680a3f
ms.sourcegitcommit: 6423dfa9cecb3b06bdd563cae113c3e80a4ec330
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105064"
---
# <a name="updates-operations"></a>Opérations de mises à jour

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Une fois les mises à jour synchronisées sur votre serveur WSUS, elles sont automatiquement analysées en vue de leur pertinence sur les ordinateurs clients du serveur. Toutefois, vous devez approuver les mises à jour avant qu’elles ne soient déployées sur les ordinateurs de votre réseau. Lorsque vous approuvez une mise à jour, vous indiquez à WSUS comment l’utiliser (vos choix sont **installer** ou **refuser** pour une nouvelle mise à jour). Vous pouvez approuver les mises à jour pour le groupe **tous les ordinateurs** ou pour les sous-groupes. Si vous n’approuvez pas une mise à jour, son état d’approbation reste **non approuvé**et votre serveur WSUS permet aux clients d’évaluer s’ils ont besoin de la mise à jour.

Si votre serveur WSUS s’exécute en mode réplica, vous ne pourrez pas approuver les mises à jour sur votre serveur WSUS. Pour plus d’informations sur le mode réplica, consultez [exécution du mode RÉPLICA WSUS](running-wsus-replica-mode.md).

## <a name="approving-updates"></a>Approbation des mises à jour
Vous pouvez approuver l’installation des mises à jour pour tous les ordinateurs de votre réseau WSUS ou pour différents groupes d’ordinateurs. Après l’approbation d’une mise à jour, vous pouvez effectuer une ou plusieurs des opérations suivantes :

-   Appliquez cette approbation aux groupes enfants, le cas échéant.

-   Définissez une échéance pour l’installation automatique. Lorsque vous sélectionnez cette option, vous définissez des heures et des dates spécifiques pour installer les mises à jour, en remplaçant les paramètres sur les ordinateurs clients. En outre, vous pouvez spécifier une date passée pour l’échéance si vous souhaitez approuver immédiatement une mise à jour (à installer la prochaine fois que les ordinateurs clients contactent le serveur WSUS).

-   Supprimer une mise à jour installée si cette mise à jour prend en charge la suppression.

Vous devez garder à l’esprit deux points importants :

-   Tout d’abord, vous ne pouvez pas définir une échéance d’installation automatique pour une mise à jour si l’entrée de l’utilisateur est requise (par exemple, en spécifiant un paramètre correspondant à la mise à jour). Pour déterminer si une mise à jour nécessite une entrée d’utilisateur, examinez le champ **peut demander une entrée d’utilisateur** dans les propriétés de mise à jour d’une mise à jour affichée sur la page **mises à jour** . Recherchez également un message dans la zone **approuver les mises à jour** qui indique «**la mise à jour sélectionnée nécessite une entrée d’utilisateur et ne prend pas en charge l’échéance**de l’installation. »

-   Si des mises à jour ont été apportées au composant serveur WSUS, vous ne pouvez pas approuver d’autres mises à jour des systèmes clients tant que la mise à jour WSUS n’a pas été approuvée. Ce message d’avertissement s’affiche dans la boîte de dialogue approuver les mises à jour : «Des mises à jour WSUS n’ont pas été approuvées. Vous devez approuver les mises à jour WSUS avant d’approuver cette mise à jour.» Dans ce cas, vous devez cliquer sur le nœud mises à jour WSUS et vous assurer que toutes les mises à jour de cette vue ont été approuvées avant de revenir aux mises à jour générales.

#### <a name="to-approve-updates"></a>Pour approuver des mises à jour

1.  Dans la console d’administration WSUS, cliquez sur **mises à jour** , puis sur **toutes les mises à jour**.

2.  Dans la liste des mises à jour, sélectionnez la mise à jour que vous souhaitez approuver, cliquez avec le bouton droit (ou accédez au volet Actions), puis dans la boîte de dialogue approuver les mises à jour, sélectionnez le groupe d’ordinateurs pour lequel vous souhaitez approuver la mise à jour, puis cliquez sur la flèche en regard de celle-ci.

3.  Sélectionnez **approuvé pour l’installation**, puis cliquez sur **approuver**.

4.  La fenêtre progression de l' **approbation** affiche la progression vers la fin de l’approbation. Une fois le processus terminé, le bouton **Fermer** s’affiche. Cliquez sur **Fermer**.

5.  Vous pouvez sélectionner une échéance en cliquant avec le bouton droit sur la mise à jour, en sélectionnant le groupe d’ordinateurs approprié, en cliquant sur la flèche en regard de celle-ci, puis en cliquant sur **échéance**.

    -   Vous pouvez sélectionner l’une des échéances standard (une semaine, deux semaines, un mois) ou vous pouvez cliquer sur **personnalisé** pour spécifier une date et une heure.

    -   Si vous souhaitez qu’une mise à jour soit installée dès que les ordinateurs clients contactent le serveur, cliquez sur **personnalisé**, puis définissez une date et une heure sur la date et l’heure actuelles ou sur une date par le passé.

#### <a name="to-approve-multiple-updates"></a>Pour approuver plusieurs mises à jour

1.  Dans la console d’administration WSUS, cliquez sur **mises à jour** , puis sur **toutes les mises à jour**.

2.  Pour sélectionner plusieurs mises à jour contiguës, appuyez sur **MAJ** tout en sélectionnant mises à jour. Pour sélectionner plusieurs mises à jour non contiguës, maintenez la **touche Ctrl** enfoncée tout en sélectionnant les mises à jour.

3.  Cliquez avec le bouton droit sur la sélection, puis cliquez sur **approuver**. La boîte de dialogue **approuver les mises à jour** s’ouvre avec l' **État approbation** défini sur **conserver les approbations existantes** et le bouton **OK** désactivé.

4.  Vous pouvez modifier les approbations pour les groupes individuels, mais cela n’affectera pas les approbations enfants. Sélectionnez le groupe pour lequel vous souhaitez modifier l’approbation, puis cliquez sur la flèche à sa gauche. Dans le menu contextuel, cliquez sur **approuvé pour l’installation**.

5.  L’approbation du groupe sélectionné passe à **installer**. S’il existe des groupes enfants, leur approbation conserve l' **approbation existante**. Pour modifier l’approbation des groupes enfants, cliquez sur le groupe, puis sur la flèche de gauche. Dans le menu contextuel, cliquez sur **appliquer aux enfants**.

6.  Pour définir un enfant spécifique afin qu’il hérite de toute son approbation du parent, cliquez sur l’enfant et cliquez sur la flèche à sa gauche. Dans le menu contextuel, cliquez sur **identique au parent**. Si vous définissez un enfant de manière à hériter des approbations, mais que vous ne modifiez pas les approbations parentes, l’enfant hérite des approbations existantes du parent.

7.  Si vous souhaitez que le comportement d’approbation change pour tous les enfants, approuvez **tous les ordinateurs**, puis choisissez **appliquer aux enfants**.

8.  Cliquez sur **OK** après avoir défini toutes vos approbations. La fenêtre progression de l' **approbation** affiche la progression vers la fin de l’approbation. Une fois le processus terminé, le bouton **Fermer** est disponible. Cliquez sur **Fermer**.

## <a name="declining-updates"></a>Refus des mises à jour
Si vous sélectionnez cette option, la mise à jour est supprimée de la liste par défaut des mises à jour disponibles et le serveur WSUS n’offre pas la mise à jour aux clients, que ce soit pour l’évaluation ou l’installation. Vous pouvez accéder à cette option en sélectionnant une mise à jour ou un groupe de mises à jour, puis en cliquant avec le bouton droit ou en accédant au volet Actions. Les mises à jour refusées s’affichent dans la liste des mises à jour uniquement si vous sélectionnez **refusé** dans la liste des approbations lorsque vous spécifiez le filtre de la liste de mises à jour sous **affichage**.

#### <a name="to-decline-updates"></a>Pour refuser des mises à jour

1.  Dans la console d’administration WSUS, cliquez sur **mises à jour**, puis sur **toutes les mises à jour**.

2.  Dans la liste des mises à jour, sélectionnez une ou plusieurs mises à jour que vous souhaitez refuser.

3.  Sélectionnez **refuser**, puis cliquez sur **Oui** dans le message de confirmation.

## <a name="cleaning-up-declined-updates"></a>Nettoyage des mises à jour refusées
Les mises à jour refusées continuent à consommer des ressources de serveur WSUS. Vous devez exécuter l’Assistant Nettoyage de serveur pour supprimer les mises à jour refusées de la base de données WSUS. Consultez : [L’Assistant Nettoyage de serveur](the-server-cleanup-wizard.md), pour plus d’informations.

## <a name="reinstating-declined-updates"></a>Réactivation des mises à jour refusées
Une fois qu’une mise à jour a été refusée, vous pouvez toujours la réinstaller.

#### <a name="to-reinstate-declined-updates"></a>Pour rétablir les mises à jour refusées

1.  Dans la console d’administration WSUS, cliquez sur **mises à jour** , puis sur **toutes les mises à jour**.

2.  Remplacez **approbation** par **refusé** , puis cliquez sur **Actualiser**. La liste des mises à jour refusées est chargée.

3.  Dans la liste des mises à jour, sélectionnez une ou plusieurs mises à jour refusées que vous souhaitez rétablir.

4.  Pour rétablir une mise à jour particulière, cliquez avec le bouton droit sur la mise à jour, puis sélectionnez **approuver**. Dans la boîte de dialogue **approuver les mises à jour** , cliquez sur **OK** pour appliquer à nouveau l’état d’approbation par défaut « non approuvé ». La mise à jour s’affiche dans la liste comme **non approuvée** au lieu de refuser.

Une fois qu’une mise à jour refusée a été nettoyée à l’aide de l’Assistant de nettoyage de serveur WSUS, elle sera supprimée du serveur WSUS et n’apparaîtra plus dans la vue toutes les mises à jour. Vous pouvez réimporter les mises à jour refusées, nettoyées à partir du catalogue Microsoft Update. Pour plus d’informations, consultez [WSUS et le site du catalogue](wsus-and-the-catalog-site.md).

## <a name="change-an-approved-update-to-not-approved"></a>Modifier une mise à jour approuvée en non approuvée
Si une mise à jour a été approuvée et que vous décidez de ne pas l’installer pour l’instant, et que vous souhaitez plutôt l’enregistrer ultérieurement, vous pouvez modifier la mise à jour en lui attribuant l’état non approuvé. Cela signifie que la mise à jour sera conservée dans la liste par défaut des mises à jour disponibles et signalera la conformité du client, mais ne sera pas installée sur les clients.

#### <a name="to-change-an-update-from-approved-to-not-approved"></a>Pour remplacer l’approbation d’une mise à jour par non approuvée

1.  Dans la console d’administration WSUS, cliquez sur **mises à jour**, puis sur **toutes les mises à jour**.

2.  Dans la liste des mises à jour, sélectionnez une ou plusieurs mises à jour approuvées que vous souhaitez modifier en non approuvées.

3.  Dans le menu contextuel ou le volet **actions** , sélectionnez **non approuvé**, puis cliquez sur **Oui** dans le message de confirmation.

## <a name="approving-updates-for-removal"></a>Approbation des mises à jour à supprimer
Vous pouvez approuver la suppression d’une mise à jour (autrement dit, pour désinstaller une mise à jour déjà installée). Cette option est disponible uniquement si la mise à jour est déjà installée et prend en charge la suppression. Vous pouvez spécifier une échéance pour la désinstallation de la mise à jour, ou spécifier une date antérieure de l’échéance si vous souhaitez supprimer la mise à jour immédiatement (la prochaine fois que les ordinateurs clients contactent le serveur WSUS).

Il est IMPORTANT de mentionner que toutes les mises à jour ne prennent pas en charge la suppression. Vous pouvez voir si une mise à jour prend en charge la suppression en sélectionnant une mise à jour individuelle et en examinant le volet d' **informations** . Sous **Détails supplémentaires**, vous verrez la catégorie **amovible** . Si la mise à jour ne peut pas être supprimée par le biais de WSUS, dans certains cas elle peut être supprimée à l’aide de la fonction **Ajout/suppression de programmes** du **panneau de configuration**.

#### <a name="to-approve-updates-for-removal"></a>Pour approuver les mises à jour à supprimer

1.  Dans la console d’administration WSUS, cliquez sur **mises à jour** , puis sur **toutes les mises à jour**.

2.  Dans la liste des mises à jour, sélectionnez une ou plusieurs mises à jour que vous souhaitez approuver pour la suppression, puis cliquez dessus avec le bouton droit (ou accédez au volet **actions** ).

3.  Dans la boîte de dialogue **approuver les mises à jour** , sélectionnez le groupe d’ordinateurs dont vous souhaitez supprimer la mise à jour, puis cliquez sur la flèche en regard de celle-ci.

4.  Sélectionnez **approuvé pour la suppression**, puis cliquez sur le bouton **supprimer** .

5.  Une fois l’approbation de suppression terminée, vous pouvez sélectionner une échéance en cliquant avec le bouton droit sur la mise à jour, en sélectionnant le groupe d’ordinateurs approprié, puis en cliquant sur la flèche en regard de celle-ci. Sélectionnez ensuite **échéance**. Vous pouvez sélectionner l’une des échéances standard (une semaine, deux semaines, un mois) ou cliquer sur **personnalisé** pour sélectionner une date et une heure spécifiques.

6.  Si vous souhaitez qu’une mise à jour soit supprimée dès que les ordinateurs clients contactent le serveur, cliquez sur **personnalisé**et définissez une date dans le passé.

## <a name="approving-updates-automatically"></a>Approbation automatique des mises à jour
Vous pouvez configurer votre serveur WSUS pour l’approbation automatique de certaines mises à jour. Vous pouvez également spécifier l’approbation automatique des révisions des mises à jour existantes dès qu’elles sont disponibles. Cette option est sélectionnée par défaut. Une révision est une version d’une mise à jour à laquelle des modifications ont été apportées (par exemple, elle a peut-être expiré ou ses règles de mise en application ont peut-être été modifiées). Si vous ne choisissez pas d’approuver automatiquement la version révisée d’une mise à jour, WSUS utilise l’ancienne version, et vous devez approuver manuellement la révision de mise à jour.

Vous pouvez créer des règles qui seront appliquées automatiquement par votre serveur WSUS lors de la synchronisation. Vous spécifiez les mises à jour que vous souhaitez approuver automatiquement pour l’installation, par classification des mises à jour, par produit et par groupe d’ordinateurs. Cela s’applique uniquement aux nouvelles mises à jour, par opposition aux mises à jour modifiées. Vous pouvez également spécifier une échéance d’approbation des mises à jour, qui définit un nombre de jours et une heure spécifique de l’offre avant l’échéance de la mise à jour approuvée : installée. Ces paramètres sont disponibles dans le volet **options** , sous **approbations automatiques**.

#### <a name="to-automatically-approve-updates"></a>Pour approuver automatiquement les mises à jour

1.  Dans la console d’administration WSUS, cliquez sur **options**, puis sur **approbations automatiques**.

2.  Dans **Règles de mise à jour**, cliquez sur **Nouvelle règle**.

3.  Dans la boîte de dialogue **Ajouter une règle** , sous **étape 1 : sélectionnez des propriétés**, indiquez si une **mise à jour se trouve dans une classification spécifique** ou si **une mise à jour se trouve dans un produit spécifique** (ou les deux) comme critère. Éventuellement, indiquez si vous souhaitez **définir une échéance** pour l’approbation.

4.  Dans **étape 2 : modifiez les propriétés** , cliquez sur les propriétés soulignées pour sélectionner les classifications, les produits et les groupes d’ordinateurs pour lesquels vous souhaitez des approbations automatiques, le cas échéant. Si vous le souhaitez, choisissez le jour et l’heure d’échéance de l’approbation des mises à jour.

5.  À **l’étape 3 : Spécifiez un nom**, tapez un nom unique pour la règle.

6.  Cliquez sur **OK**.

Les règles d’approbation automatique ne s’appliquent pas aux mises à jour nécessitant un contrat de licence utilisateur final (CLUF) qui n’a pas encore été accepté sur le serveur. Si vous constatez que l’application d’une règle d’approbation automatique n’entraîne pas l’approbation de toutes les mises à jour appropriées, vous devez approuver ces mises à jour manuellement.

## <a name="automatically-approving-revisions-to-updates-and-declining-expired-updates"></a>Approbation automatique des révisions des mises à jour et refus des mises à jour expirées
La section approbations automatiques du volet Options contient une option par défaut permettant d’approuver automatiquement les révisions des mises à jour approuvées. Vous pouvez également configurer votre serveur WSUS de façon à ce qu’il refuse automatiquement les mises à jour expirées. Si vous choisissez de ne pas approuver automatiquement la version révisée d’une mise à jour, votre serveur WSUS utilisera l’ancienne révision et vous devez approuver manuellement la révision de mise à jour.

> [!NOTE]
> Une révision est une version d’une mise à jour qui a changé (par exemple, elle a peut-être expiré ou a mis à jour des règles de mise en application).

#### <a name="to-automatically-approve-revisions-to-updates-and-decline-expired-updates"></a>Pour approuver automatiquement les révisions des mises à jour et refuser les mises à jour expirées

1.  Dans la console d’administration WSUS, cliquez sur **options**, puis sur **approbations automatiques**.

2.  Dans l’onglet **avancé** , assurez-vous que les deux options **approuver automatiquement les nouvelles révisions des mises à jour approuvées** et **refuser automatiquement les mises à jour lorsqu’une nouvelle révision entraîne l’expiration de ces** dernières sont sélectionnées.

3.  Cliquez sur OK.

    > [!NOTE]
    > Conserver les valeurs par défaut pour ces options vous permet de maintenir de bonnes performances sur votre réseau WSUS. Si vous ne souhaitez pas que les mises à jour expirées soient refusées automatiquement, vous devez veiller à les refuser manuellement de façon périodique.

## <a name="automatically-declining-superseded-updates"></a>Refus automatique des mises à jour remplacées
Lorsque vous approuvez une nouvelle mise à jour qui remplace une mise à jour existante qui est approuvée automatiquement, la mise à jour remplacée devient « non applicable » sur un ordinateur ou un périphérique une fois que la mise à jour la plus récente a été installée. Vous pouvez vérifier dans la console WSUS qu’une mise à jour ne s’applique pas à tous les ordinateurs. Lorsque c’est le cas, la mise à jour peut être refusée en toute sécurité. en outre, la mise à jour peut être refusée automatiquement lorsque vous exécutez l’Assistant de nettoyage de serveur WSUS.

Pour rechercher les mises à jour remplacées, vous pouvez sélectionner la colonne d’indicateur « remplacé » dans la vue toutes les mises à jour et effectuer un tri sur cette colonne. Il y aura quatre groupes :

-   Mises à jour qui n’ont jamais été remplacées (icône vide).

-   Mises à jour qui ont été remplacées, mais qui n’ont jamais remplacé une autre mise à jour (icône avec un carré bleu en bas).

-   Les mises à jour qui ont été remplacées et qui ont remplacé une autre mise à jour (une icône avec un carré bleu au milieu).

-   Mises à jour qui ont remplacé une autre mise à jour (icône avec un carré bleu en haut).

Il n’existe aucune fonctionnalité dans Windows Server Update Services qui refuse automatiquement les mises à jour remplacées lors de l’approbation d’une mise à jour plus récente. Il est recommandé de commencer par définir l’approbation sur « non approuvé », puis d’utiliser l’Assistant Nettoyage de serveur pour refuser automatiquement la mise à jour lorsque toutes les conditions pertinentes ont été satisfaites. Pour plus d'informations, voir : [Assistant de nettoyage de serveur](the-server-cleanup-wizard.md).

## <a name="approving-superseding-or-superseded-updates"></a>Approbation des mises à jour remplacées ou remplacées
En règle générale, une mise à jour qui remplace d’autres mises à jour effectue une ou plusieurs des opérations suivantes :

-   Optimise, améliore ou s'ajoute au correctif fourni par une ou plusieurs mises à jour précédemment publiées.

-   Améliore l'efficacité de son package de fichiers de mise à jour, qui est installé sur les ordinateurs clients si la mise à jour est approuvée pour l'installation. Par exemple, la mise à jour remplacée peut contenir des fichiers qui ne sont plus pertinents au correctif ou aux systèmes d'exploitation pris en charge par la nouvelle mise à jour, de sorte que ces fichiers ne sont plus inclus dans le package de fichiers de la mise à jour de remplacement.

-   Met à jour les versions plus récentes des systèmes d’exploitation. Il est également IMPORTANT de noter que la mise à jour de remplacement peut ne pas prendre en charge les versions antérieures des systèmes d’exploitation.

À l’inverse, une mise à jour qui est remplacée par une autre mise à jour effectue les opérations suivantes :

-   Résout un problème semblable à celui de la mise à jour qui la remplace. Toutefois, la mise à jour qui la remplace peut améliorer le correctif fourni par la mise à jour remplacée.

-   Met à jour les versions antérieures des systèmes d’exploitation. Dans certains cas, ces versions des systèmes d’exploitation ne sont plus mises à jour par la mise à jour de remplacement.

Dans le volet de détails d’une mise à jour individuelle, une icône d’information et un message en haut indiquent qu’elle remplace ou est remplacée par une autre mise à jour. En outre, vous pouvez déterminer les mises à jour qui remplacent ou qui sont remplacées par la mise à jour en examinant les mises à jour qui **remplacent cette mise à jour** et les mises à jour **remplacées par cette mise à jour** dans la section **Détails supplémentaires** de la  **Propriétés**. Le volet d’informations d’une mise à jour s’affiche sous la liste des mises à jour.

WSUS ne refuse pas automatiquement les mises à jour remplacées et il est recommandé de ne pas supposer que les mises à jour remplacées doivent être refusées en faveur de la nouvelle mise à jour de remplacement. Avant de refuser une mise à jour remplacée, assurez-vous qu’elle n’est plus nécessaire pour les ordinateurs clients. Voici quelques exemples de scénarios dans lesquels vous devrez peut-être installer une mise à jour remplacée :

-   Si une mise à jour de remplacement prend en charge uniquement les versions plus récentes d’un système d’exploitation et que certains de vos ordinateurs clients exécutent des versions antérieures du système d’exploitation.

-   Si une mise à jour de remplacement a une applicabilité plus limitée que la mise à jour qu’elle remplace, ce qui la rendrait inappropriée pour certains ordinateurs clients.

-   Si une mise à jour ne remplace plus une mise à jour précédemment publiée en raison de nouvelles modifications. Il est possible que les modifications apportées à chaque version ne remplacent plus une mise à jour qu’elle remplaçait précédemment dans une version antérieure. Dans ce scénario, vous verrez toujours un message sur la mise à jour remplacée, même si la mise à jour qui la remplace a été remplacée par une mise à jour qui ne le fait pas.


