---
title: "Créer un filtre de fichiers"
description: "Cet article explique comment créer un filtre de fichiers"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c1f261eb926eca3ead58b87aeb00a5060b9d957c
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="create-a-file-screen"></a>Créer un filtre de fichiers

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2

Lorsque vous créez un filtre de fichiers, vous pouvez choisir d’enregistrer un modèle de filtre de fichiers basé sur les propriétés de filtre de fichiers personnalisées que vous définissez. L’avantage de cette méthode est qu’un lien est conservé entre les filtres de fichiers et le modèle qui a servi à les créer, de sorte qu’à l’avenir, les modifications apportées au modèle s'appliquent à tous les filtres de fichiers qui en dérivent. Cette fonctionnalité simplifie la mise en œuvre de modifications dans la stratégie de stockage, puisque toutes les mises à jour peuvent s’effectuer au même endroit.

## <a name="to-create-a-file-screen-with-custom-properties"></a>Pour créer un filtre de fichiers avec des propriétés personnalisées

1.  Dans **Gestion du filtrage de fichiers**, cliquez sur le nœud **Filtres de fichiers**.

2.  Cliquez avec le bouton droit sur **Filtres de fichiers**, puis cliquez sur **Créer un filtre de fichiers** (ou sélectionnez **Créer un filtre de fichiers** à partir du volet **Actions**). La boîte de dialogue **Créer un filtre de fichiers** s’affiche.

3.  Sous **Chemin d'accès du filtre de fichiers**, tapez le nom du dossier auquel s’applique le filtre de fichiers ou accédez-y. Le filtre de fichiers s’applique au dossier sélectionné et à tous ses sous-dossiers.

4.  Sous **Comment voulez-vous configurer les propriétés du filtre de fichiers**, cliquez sur **Définir les propriétés de filtre de fichiers personnalisées**, puis cliquez sur **Propriétés personnalisées**. La boîte de dialogue **Propriétés du filtre de fichiers** s’affiche.

5.  Si vous souhaitez copier les propriétés d’un modèle existant de manière à les utiliser comme base de votre filtre de fichiers, sélectionnez un modèle dans la liste déroulante **Copier les propriétés du modèle**. Cliquez ensuite sur **Copier**.

    Dans la boîte de dialogue **Propriétés du filtre de fichiers**, modifiez ou définissez les valeurs suivantes sous l'onglet **Paramètres**:

6.  Sous **Type de filtrage**, cliquez sur l'option **Filtrage actif** ou **Filtrage passif**. (Le filtrage actif empêche les utilisateurs d’enregistrer des fichiers qui sont membres de groupes de fichiers bloqués et génère des notifications lorsque les utilisateurs tentent d’enregistrer des fichiers non autorisés. Le filtrage passif envoie les notifications configurées, mais n’empêche pas les utilisateurs d’enregistrer des fichiers.)

7.  Sous **Groupes de fichiers**, sélectionnez chaque groupe de fichiers que vous souhaitez inclure dans votre filtre de fichiers. (Pour activer la case à cocher du groupe de fichiers, double-cliquez sur le nom du groupe.)

    Si vous souhaitez afficher les types de fichiers inclus et exclus par un groupe de fichiers, cliquez sur le nom du groupe, puis cliquez sur **Modifier**. Pour créer un groupe de fichiers, cliquez sur **Créer**.

8.  Vous pouvez également configurer le **Gestionnaire de ressources de serveur de fichiers** pour qu'il génère une ou plusieurs notifications en définissant les options sous les onglets **Message électronique**, **Journal des événements**, **Commande** et **Rapport**. Pour plus d’informations sur les options de notification du filtre de fichiers, voir [Créer un modèle de filtre de fichiers](create-file-screen-template.md).

9.  Après avoir sélectionné toutes les propriétés du filtre de fichiers que vous souhaitez utiliser, cliquez sur **OK** pour fermer la boîte de dialogue **Propriétés du filtre de fichiers**.

10. Dans la boîte de dialogue **Créer un filtre de fichiers**, cliquez sur **Créer** pour enregistrer le filtre de fichiers. La boîte de dialogue **Enregistrer les propriétés personnalisées sous forme de modèle** s'affiche.

11. Sélectionnez le type de filtre de fichiers personnalisé que vous souhaitez créer:

    -   Pour enregistrer un modèle qui repose sur ces propriétés personnalisées (recommandé), cliquez sur **Enregistrer les propriétés personnalisées sous forme de modèle** et entrez un nom pour le modèle. Cette option applique le modèle au nouveau filtre de fichiers. Vous pouvez utiliser le modèle pour créer d'autres filtres de fichiers à l’avenir. Vous pourrez ultérieurement mettre à jour les filtres de fichiers automatiquement en mettant à jour le modèle.
    -   Si vous ne souhaitez pas enregistrer un modèle lorsque vous enregistrez le filtre de fichiers, cliquez sur **Enregistrer le filtre de fichiers personnalisé sans créer de modèle**.

12. Cliquez sur **OK**.

## <a name="see-also"></a>Articles associés

-   [Gestion du filtrage de fichiers](file-screening-management.md)
-   [Définir des groupes de fichiers pour le filtrage](define-file-groups-for-screening.md)
-   [Créer un modèle de filtre de fichiers](create-file-screen-template.md)
-   [Modifier les propriétés du modèle de filtre de fichiers](edit-file-screen-template-properties.md)


