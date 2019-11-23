---
title: Créer un modèle de quota
description: Cet article explique comment créer un modèle de quota pour définir une limite d’espace de stockage
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 28a64c77d09bffeccbbc94ba7648d1bc0227e945
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394156"
---
# <a name="create-a-quota-template"></a>Créer un modèle de quota

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Un *modèle de quota* définit une limite d’espace, le type de quota (inconditionnel ou conditionnel) et éventuellement un ensemble de notifications qui seront automatiquement générées lorsque l’utilisation du quota aura atteint les niveaux de seuil définis.

En créant des quotas exclusivement à partir de modèles, vous pouvez centraliser leur gestion en mettant à jour les modèles au lieu de répliquer les modifications dans chaque quota. Cette fonctionnalité simplifie la mise en œuvre de modifications dans la stratégie de stockage, puisque toutes les mises à jour peuvent s’effectuer au même endroit.

## <a name="to-create-a-quota-template"></a>Pour créer un modèle de quota

1.  Dans **Gestion de quota**, cliquez sur le nœud **Modèles de quotas**.

2.  Cliquez avec le bouton droit sur **Modèles de quotas**, puis cliquez sur **Créer un modèle de quota** (ou sélectionnez **Créer un modèle de quota** dans le volet **Actions**). La boîte de dialogue **Créer un modèle de quota** s’affiche.

3.  Si vous souhaitez copier les propriétés d’un modèle existant de manière à les utiliser comme base d’un nouveau modèle, sélectionnez un modèle dans la liste déroulante **Copier les propriétés du modèle de quota**. Cliquez ensuite sur **Copier**.

    Que vous ayez choisi d’utiliser les propriétés d’un modèle existant ou que vous créiez un modèle, modifiez ou définissez les valeurs suivantes sous l’onglet **Paramètres** :

4.  Dans la zone de texte **Nom du modèle**, entrez le nom du nouveau modèle.

5.  Dans la zone de texte **Étiquette**, entrez une description facultative qui s’affichera en regard de tout quota dérivé du modèle.

6.  Sous **Limite d’espace** :

    -   Dans la zone de texte **Limite**, entrez un nombre et choisissez une unité (Ko, Mo, Go ou To) afin de spécifier la limite d’espace du quota.
    -   Cliquez sur l’option **Quota inconditionnel** ou **Quota conditionnel**. (Un quota inconditionnel empêche les utilisateurs d’enregistrer des fichiers une fois que la limite d’espace est atteinte et génère des notifications lorsque le volume de données atteint un seuil configuré. Un quota conditionnel n’applique pas la limite du quota, mais génère toutes les notifications configurées.)

7.  Vous pouvez configurer une ou plusieurs notifications de seuil facultatives pour votre modèle de quota, comme décrit dans la procédure suivante. Après avoir sélectionné toutes les propriétés du modèle de quota que vous souhaitez utiliser, cliquez sur **OK** pour enregistrer le modèle.

## <a name="setting-optional-notification-thresholds"></a>Définition de seuils de notification facultatifs

Lorsque le stockage dans un dossier ou un volume atteint un niveau de seuil que vous définissez, le Gestionnaire de ressources du serveur de fichiers peut envoyer des messages électroniques à des administrateurs ou utilisateurs spécifiques, consigner un événement, exécuter une commande ou un script ou générer des rapports. Vous pouvez configurer plusieurs types de notification pour chaque seuil et définir plusieurs seuils pour un quota (ou un modèle de quota). Par défaut, aucune notification n'est générée.

Par exemple, vous pouvez configurer des seuils pour envoyer un message électronique à l’administrateur et aux utilisateurs qui souhaiteraient savoir quand un dossier atteint 85 % de sa limite de quota et envoyer une autre notification lorsque la limite de quota est atteinte. Vous pouvez également exécuter un script qui utilise la commande **dirquota.exe** pour augmenter automatiquement la limite de quota quand un seuil est atteint.

> [!Important]
> Pour envoyer des notifications par courrier électronique et configurer les rapports de stockage avec des paramètres appropriés à votre environnement de serveur, vous devez d’abord définir les options générales du Gestionnaire de ressources du serveur de fichiers. Pour plus d’informations, voir [Définition des options du Gestionnaire de ressources du serveur de fichiers](setting-file-server-resource-manager-options.md)

**Pour configurer des notifications que le serveur de fichiers Gestionnaire des ressources générera à un seuil de quota**

1. Dans la boîte de dialogue **Créer un modèle de quota**, sous **Seuils de notification**, cliquez sur **Ajouter**. La boîte de dialogue **Ajouter un seuil** s’affiche.

2. Pour définir un pourcentage de la limite de quota qui générera une notification :

   Dans la zone de texte **Générer des notifications lorsque l’utilisation atteint (%)** , entrez un pourcentage de la limite de quota pour le seuil de notification. (Le pourcentage par défaut du premier seuil de notification est de 85 %.)

3. Pour configurer les notifications par courrier électronique :

   Sous l'onglet **Message électronique**, définissez les options suivantes :

   - Pour avertir les administrateurs qu’un seuil a été atteint, cochez la case **Envoyer un courrier électronique aux administrateurs suivants**, puis entrez les noms des comptes d’administration qui recevront les notifications. Utilisez le format <em>account@domain</em> et séparez les différents comptes par des points-virgules.
   - Pour envoyer un courrier électronique à la personne ayant enregistré le fichier qui atteint le seuil de quota, cochez la case **Envoyer un message à l'utilisateur qui dépasse le seuil**.
   - Pour configurer le message, modifiez le contenu par défaut de la ligne d'objet et du corps du message. Le texte entre crochets insère les informations de variables sur l’événement de quota qui a provoqué la notification. Par exemple, la variable **\[propriétaire d’e/s Source\]** insère le nom de l’utilisateur qui a enregistré le fichier ayant atteint le seuil de quota. Pour insérer d'autres variables dans le texte, cliquez sur **Insérer une variable**.
   - Pour configurer des en-têtes supplémentaires (notamment De, Cc, Cci et Répondre), cliquez sur **Autres en-têtes de courrier électronique**.

4. Pour consigner un événement :

   Sous l'onglet **Journal des événements**, cochez la case **Envoyer un avertissement au journal des événements**, puis modifiez l’entrée de journal par défaut.

5. Pour exécuter une commande ou un script :

   Sous l'onglet **Commande**, cochez la case **Exécuter cette commande ou ce script**. Puis tapez la commande, ou cliquez sur **Parcourir** pour rechercher l’emplacement où le script est stocké. Vous pouvez également entrer des arguments de commande, sélectionner un répertoire de travail pour la commande ou le script, ou modifier le paramètre de sécurité de la commande.

6. Pour générer un ou plusieurs rapports de stockage :

   Sous l'onglet **Rapport**, cochez la case **Générer les rapports**, puis sélectionnez les rapports à générer. (Vous pouvez choisir un ou plusieurs destinataires de messagerie administrative pour le rapport ou envoyer le rapport à l’utilisateur qui a atteint le seuil.)

   Le rapport est enregistré à l’emplacement par défaut des rapports d’incidents. Vous pouvez modifier cet emplacement dans la boîte de dialogue **Options du Gestionnaire de ressources du serveur de fichiers**.

7. Cliquez sur **OK** pour enregistrer vos seuils de notification.

8. Répétez ces étapes si vous souhaitez configurer d'autres seuils de notification pour le modèle de quota.

## <a name="see-also"></a>Voir également

-   [Gestion de quota](quota-management.md)
-    [Définition des options du Gestionnaire de ressources du serveur de fichiers](setting-file-server-resource-manager-options.md)
-   [Modifier les propriétés du modèle de quota](edit-quota-template-properties.md)
-   [Outils en ligne de commande](command-line-tools.md)


