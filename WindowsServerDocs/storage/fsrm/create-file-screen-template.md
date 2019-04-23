---
title: Créer un modèle de filtre de fichiers
description: Cet article explique comment créer un modèle de filtre de fichiers
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b06597bce0b88ed5a2e98ad45d0cbc355d1b13fc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858350"
---
# <a name="create-a-file-screen-template"></a>Créer un modèle de filtre de fichiers

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Un *modèle de filtre de fichiers* définit un ensemble de groupes de fichiers à filtrer, le type de filtrage à effectuer (actif ou passif) et le cas échéant, un ensemble de notifications générées automatiquement si un utilisateur enregistre, ou tente d’enregistrer, un fichier non autorisé.

Le Gestionnaire de ressources du serveur de fichiers peut envoyer des messages électroniques à des administrateurs ou utilisateurs spécifiques, consigner un événement, exécuter une commande ou un script ou générer des rapports. Vous pouvez configurer plusieurs types de notification pour un événement du filtre de fichiers.

En créant des filtres de fichiers exclusivement à partir de modèles, vous pouvez centraliser leur gestion en mettant à jour les modèles au lieu de répliquer les modifications dans chaque filtre de fichiers. Cette fonctionnalité simplifie la mise en œuvre de modifications dans la stratégie de stockage, puisque toutes les mises à jour peuvent s’effectuer au même endroit.

> [!Important]
> Pour envoyer des notifications par courrier électronique et configurer les rapports de stockage avec des paramètres appropriés à votre environnement de serveur, vous devez d’abord définir les options générales du Gestionnaire de ressources du serveur de fichiers. Pour plus d’informations, voir [Définition des options du Gestionnaire de ressources du serveur de fichiers](setting-file-server-resource-manager-options.md).

## <a name="to-create-a-file-screen-template"></a>Pour créer un modèle de filtre de fichiers

1.  Dans **Gestion du filtrage de fichiers**, cliquez sur le nœud **Modèles de filtres de fichiers**.

2.  Cliquez avec le bouton droit sur **Modèles de filtres de fichiers**, puis cliquez sur **Créer un modèle de filtre de fichiers** (ou sélectionnez **Créer un modèle de filtre de fichiers** à partir du volet **Actions**). La boîte de dialogue **Créer un modèle de filtre de fichiers** s’affiche.

3.  Si vous souhaitez copier les propriétés d’un modèle existant de manière à les utiliser comme base d’un nouveau modèle, sélectionnez un modèle dans la liste déroulante **Copier les propriétés du modèle**, puis cliquez sur **Copier**.

    Que vous ayez choisi d’utiliser les propriétés d’un modèle existant ou que vous créiez un modèle, modifiez ou définissez les valeurs suivantes sous l’onglet **Paramètres** :

4.  Dans la zone de texte **Nom du modèle**, entrez le nom du nouveau modèle.

5.  Sous **Type de filtrage**, cliquez sur l'option **Filtrage actif** ou **Filtrage passif**. (Le filtrage actif empêche les utilisateurs d’enregistrer des fichiers qui sont membres de groupes de fichiers bloqués et génère des notifications lorsque les utilisateurs tentent d’enregistrer des fichiers non autorisés. Le filtrage passif envoie les notifications configurées, mais n’empêche pas les utilisateurs d’enregistrer des fichiers).

6.  Pour spécifier les groupes de fichiers à filtrer :

    Sous **Groupes de fichiers**, sélectionnez chaque groupe de fichiers que vous souhaitez inclure. (Pour activer la case à cocher du groupe de fichiers, double-cliquez sur le nom du groupe.)

    Si vous souhaitez afficher un groupe de fichiers contient et exclut les types de fichiers et cliquez sur l’étiquette de groupe de fichiers, puis cliquez sur **modifier**. Pour créer un groupe de fichiers, cliquez sur **créer**.

    Vous pouvez également configurer le Gestionnaire de ressources de serveur de fichiers pour qu'il génère une ou plusieurs notifications en définissant les options suivantes sous les onglets **Message électronique**, **Journal des événements**, **Commande** et **Rapport**.

7.  Pour configurer les notifications par courrier électronique :

    Sous l'onglet **Message électronique**, définissez les options suivantes :

    -   Pour avertir les administrateurs qu’un utilisateur ou une application tente d'enregistrer un fichier non autorisé, cochez la case **Envoyer un courrier électronique aux administrateurs suivants**, puis entrez les noms des comptes d’administration qui recevront les notifications. Utilisez le format *compte*@*domaine* et séparez les différents comptes par des points-virgules.
    -   Pour envoyer un courrier électronique à l’utilisateur qui a tenté d’enregistrer le fichier, cochez la case **Courrier électronique à l’utilisateur qui a tenté d’enregistrer un fichier non autorisé**.
    -   Pour configurer le message, modifiez le contenu par défaut de la ligne d'objet et du corps du message. Le texte entre crochets insère les informations de variables sur l’événement de filtre de fichiers qui a provoqué la notification. Par exemple, le \[ **propriétaire de la Source d’e/s** \] variable insère le nom de l’utilisateur qui a tenté d’enregistrer un fichier non autorisé. Pour insérer d'autres variables dans le texte, cliquez sur **Insérer une variable**.
    -   Pour configurer des en-têtes supplémentaires (notamment De, Cc, Cci et Répondre), cliquez sur **Autres en-têtes de courrier électronique**.

8.  Pour consigner une erreur dans le journal des événements lorsqu’un utilisateur tente d’enregistrer un fichier non autorisé :

    Sous l'onglet **Journal des événements**, cochez la case **Envoyer un avertissement au journal des événements**, puis modifiez l’entrée de journal par défaut.

9.  Pour exécuter une commande ou un script lorsqu’un utilisateur tente d’enregistrer un fichier non autorisé :

    Sous l'onglet **Commande**, cochez la case **Exécuter cette commande ou ce script**. Puis tapez la commande, ou cliquez sur **Parcourir** pour rechercher l’emplacement où le script est stocké. Vous pouvez également entrer des arguments de commande, sélectionner un répertoire de travail pour la commande ou le script, ou modifier le paramètre de sécurité de la commande.

10. Pour générer un ou plusieurs rapports de stockage lorsqu’un utilisateur tente d’enregistrer un fichier non autorisé :

    Sous l'onglet **Rapport**, cochez la case **Générer les rapports**, puis sélectionnez les rapports à générer. (Vous pouvez choisir un ou plusieurs destinataires de messagerie administrative pour le rapport ou envoyer le rapport à l’utilisateur qui a tenté d’enregistrer le fichier.)

    Le rapport est enregistré à l’emplacement par défaut des rapports d’incidents. Vous pouvez modifier cet emplacement dans la boîte de dialogue **Options du Gestionnaire de ressources du serveur de fichiers**.

11. Après avoir sélectionné toutes les propriétés du modèle de fichier que vous souhaitez utiliser, cliquez sur **OK** pour enregistrer le modèle.

## <a name="see-also"></a>Voir aussi

-   [Gestion des filtres de fichiers](file-screening-management.md)
-   [Options de paramètre File Server Resource Manager](setting-file-server-resource-manager-options.md)
-   [Modifier les propriétés de modèle de filtre de fichier](edit-file-screen-template-properties.md)

