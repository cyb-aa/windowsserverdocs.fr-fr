---
title: Opérations de mises à jour
description: Rubrique de Windows Server Update Service (WSUS) - la gestion des mises à jour, y compris le processus d’approbation
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
ms.openlocfilehash: 4d99e006a03e12d7201390748aec8671236cf297
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822550"
---
# <a name="updates-operations"></a>Opérations de mises à jour

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Une fois les mises à jour ont été synchronisées avec votre serveur WSUS, ils seront analysés automatiquement pertinents sur les ordinateurs clients du serveur. Toutefois, vous devez approuver les mises à jour avant de les déployer sur les ordinateurs sur votre réseau. Lorsque vous approuvez une mise à jour, vous dites en fait WSUS ce qu’il faut en faire (vos choix sont **installer** ou **refuser** pour une nouvelle mise à jour). Vous pouvez approuver des mises à jour pour le **tous les ordinateurs** groupe ou pour des sous-groupes. Si vous n’approuvez pas une mise à jour, son état d’approbation reste **ne pas approuvé**, et votre serveur WSUS permet aux clients d’évaluer s’ils ont besoin de la mise à jour.

Si votre serveur WSUS s’exécute en mode réplica, vous ne serez pas en mesure d’approuver les mises à jour sur votre serveur WSUS. Pour plus d’informations sur le mode réplica, consultez [mode réplica de WSUS en cours d’exécution](running-wsus-replica-mode.md).

## <a name="approving-updates"></a>Approbation des mises à jour
Vous pouvez approuver l’installation des mises à jour pour tous les ordinateurs de votre réseau WSUS ou de groupes d’ordinateurs différents. Après l’approbation d’une mise à jour, vous pouvez effectuer une des opérations suivantes (ou plus) :

-   Appliquer cette approbation sur des groupes enfants, le cas échéant.

-   Définir une échéance pour une installation automatique. Lorsque vous sélectionnez cette option, vous définissez des dates et heures pour installer les mises à jour, substituer des paramètres sur les ordinateurs clients. En outre, vous pouvez spécifier une date révolue pour la date limite si vous souhaitez approuver une mise à jour immédiatement (à installer la prochaine fois que les ordinateurs clients contactent le serveur WSUS).

-   Supprimer une mise à jour installée si cette mise à jour prend en charge la suppression.

Il existe deux points importants dont vous devez prendre en compte :

-   Tout d’abord, Impossible de définir une échéance pour une installation automatique pour une mise à jour si l’entrée d’utilisateur est requise (par exemple, en spécifiant un paramètre concernant la mise à jour). Pour déterminer si une mise à jour nécessitera l’entrée d’utilisateur, regardez la **peut demander la saisie utilisateur** champ dans les propriétés de mise à jour pour une mise à jour affiché sur le **met à jour** page. Vérifiez également un message dans le **approuver les mises à jour** case à cocher, «**la mise à jour sélectionnée nécessite une entrée utilisateur et ne prend pas en charge l’échéance d’installation**. »

-   S’il existe des mises à jour pour le composant de serveur WSUS, vous ne pouvez pas approuver des autres mises à jour sur les systèmes clients jusqu'à ce que la mise à jour WSUS est approuvée. Vous voyez ce message d’avertissement dans la boîte de dialogue Approuver les mises à jour : « Il existe des mises à jour WSUS qui n’ont pas été approuvées. Vous devez approuver les mises à jour WSUS avant d’approuver cette mise à jour. » Vous devez dans ce cas, cliquez sur le nœud mises à jour WSUS et assurez-vous que toutes les mises à jour dans cette vue ont été approuvées avant de renvoyer les mises à jour générales.

#### <a name="to-approve-updates"></a>Pour approuver les mises à jour

1.  Dans la console d’administration WSUS, cliquez sur **mises à jour** puis cliquez sur **toutes les mises à jour**.

2.  Dans la liste des mises à jour, sélectionnez la mise à jour que vous souhaitez approuver et avec le bouton droit (ou accédez au volet Actions), et dans la boîte de dialogue Approuver les mises à jour, sélectionnez le groupe d’ordinateurs pour lesquels vous souhaitez approuver la mise à jour, cliquez sur la flèche en regard de celle-ci.

3.  Sélectionnez **approuvée pour l’installation**, puis cliquez sur **approuver**.

4.  Le **progression de l’approbation** fenêtre affiche la progression de l’approbation. Lorsque le processus est terminé, le **fermer** bouton s’affiche. Cliquez sur **Fermer**.

5.  Vous pouvez sélectionner une échéance en double-cliquant sur la mise à jour, en sélectionnant le groupe d’ordinateurs approprié, en cliquant sur la flèche en regard de celle-ci, puis en cliquant sur **échéance**.

    -   Vous pouvez sélectionner un des délais standards (une semaine, deux semaines, un mois), ou vous pouvez cliquer sur **personnalisé** pour spécifier une date et une heure.

    -   Si vous souhaitez une mise à jour à installer dès que le contact d’ordinateurs client du serveur, cliquez sur **personnalisé**, puis définissez une date et une heure à la date et heure actuelles ou à l’autre dans le passé.

#### <a name="to-approve-multiple-updates"></a>Pour approuver plusieurs mises à jour

1.  Dans la console d’administration WSUS, cliquez sur **mises à jour** puis cliquez sur **toutes les mises à jour**.

2.  Pour sélectionner plusieurs mises à jour contiguës, appuyez sur **MAJ** lors de la sélection des mises à jour. Pour sélectionner plusieurs mises à jour non contiguës, appuyez et maintenez **CTRL** lors de la sélection des mises à jour.

3.  Avec le bouton droit de la sélection et cliquez sur **approuver**. Le **approuver les mises à jour** boîte de dialogue s’ouvre avec le **l’état d’approbation** définie sur **conserver les approbations existantes** et le **OK** bouton désactivé.

4.  Vous pouvez modifier les approbations pour des groupes individuels, mais cela ne sera pas d’affecter les approbations de l’enfant. Sélectionnez le groupe pour lequel vous souhaitez modifier l’approbation, puis cliquez sur la flèche à sa gauche. Dans le menu contextuel, cliquez sur **approuvée pour l’installation**.

5.  L’approbation pour le groupe sélectionné devient **installer**. S’il existe des groupes enfants, leur approbation reste **conserver l’approbation existante**. Pour modifier l’approbation pour les groupes enfants, cliquez sur le groupe et cliquez sur la flèche à sa gauche. Dans le menu contextuel, cliquez sur **appliquer aux enfants**.

6.  Pour définir un enfant spécifique à hériter des tous les son approbation, cliquez sur l’enfant et cliquez sur la flèche à sa gauche. Dans le menu contextuel, cliquez sur **identique au Parent**. Si vous définissez un enfant hérite les approbations, mais que vous ne modifiez pas les approbations de parent, l’enfant hérite les approbations existantes du parent.

7.  Si vous souhaitez que le comportement de l’approbation à modifier pour tous les enfants, approuver **tous les ordinateurs**, puis choisissez **appliquer aux enfants**.

8.  Cliquez sur **OK** après avoir défini toutes vos approbations. Le **progression de l’approbation** fenêtre affiche la progression de l’approbation. Lorsque le processus est terminé, le **fermer** bouton sera disponible. Cliquez sur **Fermer**.

## <a name="declining-updates"></a>Refus des mises à jour
Si vous sélectionnez cette option, la mise à jour est supprimé de la liste des mises à jour disponibles par défaut et le serveur WSUS n’offre pas de la mise à jour aux clients, soit pour la version d’évaluation ou d’installation. Vous pouvez atteindre cette option en sélectionnant une mise à jour ou un groupe de mises à jour et effectuant un clic droit ou allant dans le volet Actions. Mises à jour refusées seront affiche dans la liste des mises à jour uniquement si vous sélectionnez **refusé** dans la liste d’approbation lorsque vous spécifiez le filtre pour obtenir la liste mise à jour sous **vue**.

#### <a name="to-decline-updates"></a>Pour refuser des mises à jour

1.  Dans la console d’administration WSUS, cliquez sur **mises à jour**, puis cliquez sur **toutes les mises à jour**.

2.  Dans la liste des mises à jour, sélectionnez un ou plusieurs mises à jour que vous souhaitez refuser.

3.  Sélectionnez **refuser**, puis cliquez sur **Oui** sur le message de confirmation.

## <a name="cleaning-up-declined-updates"></a>nettoyage des mises à jour refusées
Mises à jour refusées continuent à consommer des ressources du serveur WSUS. Vous devez exécuter le nettoyage du serveur de l’Assistant pour supprimer des mises à jour refusées à partir de la base de données WSUS. Consultez : [Le nettoyage de serveur Assistant](the-server-cleanup-wizard.md), pour plus d’informations.

## <a name="reinstating-declined-updates"></a>Réactivation des mises à jour refusées
Après qu’une mise à jour a été refusée, vous pouvez toujours le réactiver.

#### <a name="to-reinstate-declined-updates"></a>Pour rétablir les mises à jour refusées

1.  Dans la console d’administration WSUS, cliquez sur **mises à jour** puis cliquez sur **toutes les mises à jour**.

2.  modifier **approbation** à **refusé** et cliquez sur **Actualiser**. Charge de la liste des mises à jour refusées.

3.  Dans la liste des mises à jour, sélectionnez un ou plusieurs mises à jour refusées que vous souhaitez rétablir.

4.  Pour rétablir une mise à jour particulière, un clic droit sur la mise à jour puis **approuver**. Dans le **approuver les mises à jour** boîte de dialogue, cliquez sur **OK** à réappliquer l’état d’approbation par défaut « Non approuvés ». La mise à jour s’affichent dans la liste en tant que **ne pas approuvé** au lieu de refusé.

Après qu’une mise à jour refusée a été nettoyé à l’aide de l’Assistant de nettoyage de serveur WSUS, il sera supprimé à partir du serveur WSUS et n’apparaît plus dans la vue met à jour tous les. Vous pouvez réimporter refusé, nettoyées des mises à jour à partir du catalogue Microsoft Update. Pour plus d’informations, consultez [WSUS et le Site du catalogue](wsus-and-the-catalog-site.md).

## <a name="change-an-approved-update-to-not-approved"></a>modifier une mise à jour approuvées non approuvées
Si une mise à jour a été approuvée et que vous décidez de ne pas l’installer pour l’instant et à la place vouloir l’enregistrer pour une date ultérieure, vous pouvez modifier la mise à jour dans l’état non approuvé. Cela signifie que la mise à jour restera dans la liste par défaut des mises à jour disponibles et signalera la conformité du client, mais ne sera pas installé sur les clients.

#### <a name="to-change-an-update-from-approved-to-not-approved"></a>Pour modifier une mise à jour à partir d’approuvées non approuvées

1.  Dans la console d’administration WSUS, cliquez sur **mises à jour**, puis cliquez sur **toutes les mises à jour**.

2.  Dans la liste des mises à jour, sélectionnez un ou plusieurs mises à jour approuvées que vous souhaitez modifier ne pas approuvées.

3.  Dans le menu contextuel ou la **Actions** volet, sélectionnez **pas approuvé**, puis cliquez sur **Oui** sur le message de confirmation.

## <a name="approving-updates-for-removal"></a>L’approbation des mises à jour pour la suppression
Vous pouvez approuver une mise à jour pour la suppression (autrement dit, pour désinstaller une mise à jour déjà installé). Cette option est disponible uniquement si la mise à jour est déjà installé et qu’il prend en charge la suppression. Vous pouvez spécifier une date limite pour la mise à jour doivent être désinstallées, ou spécifier une date révolue pour la date limite si vous souhaitez supprimer la mise à jour immédiatement (la prochaine fois les ordinateurs clients contactent le serveur WSUS).

Il est IMPORTANT de mentionner que pas toutes les mises à jour prend en charge la suppression. Vous pouvez voir si une mise à jour prend en charge la suppression en sélectionnant une mise à jour individuel et en examinant le **détails** volet. Sous **des détails supplémentaires**, vous verrez la **amovible** catégorie. Si la mise à jour ne peut pas être supprimé via WSUS, dans certains cas elle peut être supprimée avec **ajouter ou supprimer des programmes** de **le panneau de configuration**.

#### <a name="to-approve-updates-for-removal"></a>Pour approuver les mises à jour pour la suppression

1.  Dans la console d’administration WSUS, cliquez sur **mises à jour** puis cliquez sur **toutes les mises à jour**.

2.  Dans la liste des mises à jour, sélectionnez un ou plusieurs mises à jour que vous souhaitez approuver la suppression et effectuez un clic droit (ou accédez à la **Actions** volet).

3.  Dans le **approuver les mises à jour** boîte de dialogue, sélectionnez le groupe d’ordinateurs à partir de laquelle vous souhaitez supprimer la mise à jour, puis cliquez sur la flèche en regard de celle-ci.

4.  Sélectionnez **approuvées pour la suppression**, puis cliquez sur le **supprimer** bouton.

5.  Une fois l’approbation de la suppression terminée, vous pouvez sélectionner une échéance en double-cliquant sur la mise à jour une fois de plus, en sélectionnant le groupe d’ordinateurs approprié et puis en cliquant sur la flèche en regard de celle-ci. Puis sélectionnez **échéance**. Vous pouvez sélectionner l’un des délais standards (une semaine, deux semaines, un mois), ou vous pouvez cliquer sur **personnalisé** pour sélectionner une date et heure spécifiques.

6.  Si vous souhaitez une mise à jour doit être supprimée dès que le contact d’ordinateurs client du serveur, cliquez sur **personnalisé**et définir une date dans le passé.

## <a name="approving-updates-automatically"></a>Approuver automatiquement les mises à jour
Vous pouvez configurer votre serveur WSUS pour l’approbation automatique de certaines mises à jour. Vous pouvez également spécifier l’approbation automatique des révisions de mises à jour existantes lorsqu’elles sont disponibles. Cette option est sélectionnée par défaut. Une révision est une version d’une mise à jour qui a eu des modifications apportées à ce dernier (par exemple, elle a peut-être expiré ou ses règles de mise en application peuvent avoir changé). Si vous choisissez de ne pas approuver la version révisée d’une mise à jour automatiquement, WSUS utilisera la version antérieure, et vous devez approuver manuellement la révision de mise à jour.

Vous pouvez créer des règles qui applique automatiquement votre serveur WSUS pendant la synchronisation. Vous spécifiez quelles mises à jour que vous souhaitez approuver automatiquement pour l’installation, par classification de mise à jour, par produit et par groupe d’ordinateurs. Cela s’applique uniquement aux nouvelles mises à jour, par opposition aux mises à jour révisées. Vous pouvez également spécifier une échéance de l’approbation de mise à jour, qui définit un nombre de jours et une heure spécifique de l’offre avant la mise à jour approuvée est installé à la date d’échéance. Ces paramètres sont disponibles dans le **Options** volet, sous **approbations automatiques**.

#### <a name="to-automatically-approve-updates"></a>Pour approuver automatiquement les mises à jour

1.  Dans la console d’administration WSUS, cliquez sur **Options**, puis cliquez sur **approbations automatiques**.

2.  Dans **Règles de mise à jour**, cliquez sur **Nouvelle règle**.

3.  Dans le **ajouter une règle** boîte de dialogue, sous **étape 1 : sélectionnez les propriétés**, indiquez si vous souhaitez utiliser **quand une mise à jour est dans une classification précise** ou **quand une mise à jour est dans un produit spécifique** (ou les deux) en tant que critères. Si vous le souhaitez, sélectionnez s’il faut **définir une échéance** pour l’approbation.

4.  Dans **étape 2 : modifier les propriétés** cliquez sur ces propriétés pour sélectionner les Classifications, produits et les groupes d’ordinateurs pour lesquels vous souhaitez des approbations automatiques, le cas échéant. Si vous le souhaitez, choisissez l’approbation de mise à jour à l’échéance jour et l’heure.

5.  Dans **étape 3 : Indiquez une zone de nom**, tapez un nom unique pour la règle.

6.  Cliquez sur **OK**.

Règles d’approbation automatique ne seront applique pas aux mises à jour nécessitant un contrat de licence utilisateur final (CLUF) qui n’a pas encore été accepté sur le serveur. Si vous trouvez que l’application d’une règle d’approbation automatique n’entraîne pas toutes les mises à jour appropriées doivent être approuvées, vous devez approuver ces mises à jour manuellement.

## <a name="automatically-approving-revisions-to-updates-and-declining-expired-updates"></a>Approuver automatiquement les révisions des mises à jour et refus des mises à jour expirées
La section des approbations automatiques du volet Options contient une option par défaut pour automatiquement approuver les révisions des mises à jour approuvées. Vous pouvez également définir votre serveur WSUS pour refuser automatiquement les mises à jour expirées. Si vous choisissez de ne pas approuver la version révisée d’une mise à jour automatiquement, votre serveur WSUS utilisera l’ancienne révision, et vous devez approuver manuellement la révision de mise à jour.

> [!NOTE]
> Une révision est une version d’une mise à jour qui a changé (par exemple, il a peut-être expiré ou ont mis à jour des règles d’applicabilité).

#### <a name="to-automatically-approve-revisions-to-updates-and-decline-expired-updates"></a>Pour approuver les révisions des mises à jour et refuser automatiquement les mises à jour expirées

1.  Dans la console d’administration WSUS, cliquez sur **Options**, puis cliquez sur **approbations automatiques**.

2.  Sur le **avancé** onglet, vérifiez que les deux **approuver automatiquement les nouvelles révisions des mises à jour approuvées** et **refuser automatiquement les mises à jour lorsqu’une nouvelle révision entraîne leur date d’expiration** sont sélectionnés.

3.  Cliquez sur OK.

    > [!NOTE]
    > En conservant les valeurs par défaut pour ces options permet de que maintenir de bonnes performances sur votre réseau WSUS. Si vous ne souhaitez pas refuser automatiquement des mises à jour ayant expirés, il se peut que vous devez vous assurer de leur refuser manuellement sur une base périodique.

## <a name="automatically-declining-superseded-updates"></a>Refuser automatiquement les mises à jour remplacées
Lorsque vous approuvez une nouvelle mise à jour qui en remplace une mise à jour existant qui est approuvée automatiquement, la mise à jour remplacée devient « Non Applicable » sur un ordinateur ou un appareil une fois la mise à jour plus récente a été installé. Dans la console WSUS, vous pouvez vérifier qu’une mise à jour est non Applicable pour tous les ordinateurs. Quand c’est le cas, la mise à jour peut être refusée en toute sécurité. en outre, la mise à jour peut être automatiquement refusée lorsque vous exécutez l’Assistant de nettoyage de serveur WSUS.

Pour rechercher des mises à jour remplacées, vous pouvez sélectionner la colonne d’indicateur « Remplacé » dans la vue toutes les mises à jour et de tri sur cette colonne. Il y aura quatre groupes :

-   Mises à jour qui n’ont jamais été remplacé (une icône vide).

-   Mises à jour qui ont été remplacées, mais jamais remplacé une autre mise à jour (il s’agit d’une icône avec un carré bleu en bas).

-   Mises à jour qui ont été remplacées et avaient remplacé une autre mise à jour (il s’agit d’une icône avec un carré bleu au milieu).

-   Mises à jour qui ont remplacé une autre mise à jour (il s’agit d’une icône avec un carré bleu en haut).

Il n’existe aucune fonctionnalité dans Windows Server Update Services qu’automatiquement refuse mises à jour remplacées lors de l’approbation d’une mise à jour plus récente. Il est recommandé de définir tout d’abord l’approbation à « Non approuvés » et ensuite utiliser l’Assistant de nettoyage de serveur pour refuser automatiquement la mise à jour lorsque les conditions d’utilisation ont été satisfaites. Pour plus d'informations, consultez : [Le nettoyage de serveur Assistant](the-server-cleanup-wizard.md).

## <a name="approving-superseding-or-superseded-updates"></a>Remplacement d’approbation ou de mises à jour remplacées
En règle générale, une mise à jour qui remplace d’autres mises à jour effectue une ou plusieurs des opérations suivantes :

-   Optimise, améliore ou s'ajoute au correctif fourni par une ou plusieurs mises à jour précédemment publiées.

-   Améliore l'efficacité de son package de fichiers de mise à jour, qui est installé sur les ordinateurs clients si la mise à jour est approuvée pour l'installation. Par exemple, la mise à jour remplacée peut contenir des fichiers qui ne sont plus pertinents au correctif ou aux systèmes d'exploitation pris en charge par la nouvelle mise à jour, de sorte que ces fichiers ne sont plus inclus dans le package de fichiers de la mise à jour de remplacement.

-   Met à jour des versions plus récentes des systèmes d’exploitation. Il est également IMPORTANT de noter que la mise à jour de remplacement ne prenne pas en charge les versions antérieures des systèmes d’exploitation.

À l’inverse, une mise à jour est remplacée par une autre mise à jour effectue les opérations suivantes :

-   Résout un problème semblable à celle de la mise à jour qui la remplace. Toutefois, la mise à jour qui la remplace peut améliorer le correctif qui fournit la mise à jour remplacée.

-   Met à jour les versions antérieures des systèmes d’exploitation. Dans certains cas, ces versions de systèmes d’exploitation sont ne sont plus mis à jour par la mise à jour de remplacement.

Dans le volet de détails d’une mise à jour individuelles, une icône d’information et un message en haut indique qu’il remplace ou est remplacée par une autre mise à jour. En outre, vous pouvez déterminer quelles mises à jour remplacent ou sont remplacées par la mise à jour en examinant le **mises à jour prioritaires cette mise à jour** et **mises à jour remplacées par cette mise à jour** entrées dans le **des détails supplémentaires** section de la **propriétés**. Volet de détails d’une mise à jour s’affiche sous la liste des mises à jour.

WSUS n’effectue pas automatiquement mises à jour remplacée de refus, et il est recommandé de ne pas supposer que les mises à jour remplacées doivent être refusées en faveur de la nouvelle mise à jour remplaçante. Avant de refuser une mise à jour remplacée, assurez-vous qu’il n’est plus nécessaire par un de vos ordinateurs clients. Voici quelques exemples de scénarios dans lesquels vous devrez peut-être installer une mise à jour remplacée :

-   Si une mise à jour de remplacement prend en charge que les nouvelles versions du système d’exploitation, et certains de vos ordinateurs clients exécutent des versions antérieures du système d’exploitation.

-   Si une mise à jour de remplacement a une applicabilité plus limitée que la mise à jour qu’elle remplace, ce qui la rend inappropriée pour certains ordinateurs clients.

-   Si une mise à jour ne remplace plus une mise à jour précédemment publiée en raison de nouvelles modifications. Il est possible que via les modifications apportées à chaque version, une mise à jour ne remplace plus une mise à jour qu’elle remplaçait précédemment dans une version antérieure. Dans ce scénario, vous verrez toujours un message concernant la mise à jour remplacée, même si la mise à jour qui la remplace a été remplacée par une mise à jour qui ne le fait pas.


