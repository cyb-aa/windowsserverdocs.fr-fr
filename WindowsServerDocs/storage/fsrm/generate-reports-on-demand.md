---
title: "Générer des rapports à la demande"
description: "Cet article explique comment générer des rapports à la demande pour analyser l’utilisation du disque sur le serveur"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e91bfbc306d1d2712f7b35ec48114b3a8a84ec83
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="generate-reports-on-demand"></a>Générer des rapports à la demande

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2

Pendant les opérations quotidiennes, vous pouvez utiliser l'option **Générer les rapports maintenant** pour générer un ou plusieurs rapports à la demande. Avec ces rapports, vous pouvez analyser les différents aspects de l’utilisation actuelle du disque sur le serveur. Les données actuelles sont collectées avant la génération des rapports.

Lorsque vous générez des rapports à la demande, les rapports sont enregistrés dans un emplacement par défaut que vous spécifiez dans la boîte de dialogue **Option du Gestionnaire de ressources du serveur de fichiers**, mais aucune tâche de rapport n'est créée pour une utilisation ultérieure. Vous pouvez afficher les rapports immédiatement après qu’ils ont été générés ou les envoyer par courrier électronique à un groupe d’administrateurs.

> [!Note]
> Si vous choisissez d’ouvrir les rapports immédiatement, vous devez attendre que les rapports soient générés. Le temps de traitement varie en fonction du type de rapport et de l’étendue des données.

## <a name="to-generate-reports-immediately"></a>Pour générer des rapports immédiatement

1.  Cliquez sur le nœud **Gestion des rapports de stockage**.

2.  Cliquez avec le bouton droit sur **Gestion des rapports de stockage**, puis cliquez sur **Générer les rapports maintenant** (ou sélectionnez **Générer les rapports maintenant** à partir du volet **Actions**). La boîte de dialogue **Propriétés des tâches de rapports de stockage** s'affiche.

3.  Pour sélectionner les volumes ou les dossiers sur lesquels vous voulez générer des rapports:

    -   Sous **Étendue**, cliquez sur **Ajouter**.
    -   Recherchez le volume ou le dossier sur lequel vous souhaitez générer les rapports, sélectionnez-le, puis cliquez sur **OK** pour ajouter le chemin d’accès à la liste.
    -   Ajoutez autant de volumes ou de dossiers que vous souhaitez inclure dans les rapports. (Pour supprimer un volume ou un dossier, cliquez sur le chemin d’accès, puis sur **Supprimer**).

4.  Pour spécifier les rapports à générer:

     -   Sous **Données de rapport**, sélectionnez chaque rapport à inclure.

    Pour modifier les paramètres d’un rapport:

    -   Cliquez sur l’étiquette du rapport, puis sur **Modifier les paramètres**.
    -   Dans la boîte de dialogue **Paramètres du rapport**, modifiez les paramètres selon vos besoins, puis cliquez sur **OK**.
    -  Pour afficher la liste des paramètres de tous les rapports sélectionnés, cliquez sur **Consulter les rapports sélectionnés**, puis cliquez sur **Fermer**.
 
5.  Pour spécifier les formats d'enregistrement des rapports:

    -  Sous **Formats des rapports**, sélectionnez un ou plusieurs formats pour les rapports planifiés. Par défaut, les rapports sont générés en Dynamic HTML (DHTML). Vous pouvez également sélectionner les formats HTML, XML, CSV et texte. Les rapports sont enregistrés dans l’emplacement par défaut des rapports à la demande.

6.  Pour envoyer aux administrateurs les copies des rapports par courrier électronique:

    -  Sous l'onglet **Remise**, cochez la case **Envoyer des rapports aux administrateurs suivants**, puis entrez les noms des comptes administratifs qui recevront les rapports. 
    - Utilisez le format *account@domain* et séparez les différents comptes par des points-virgules.

7.  Pour collecter les données et générer les rapports, cliquez sur **OK**. La boîte de dialogue **Générer des rapports de stockage** s'affiche.

8.  Sélectionnez la manière dont vous souhaitez générer les rapports à la demande:

    -   Si vous voulez afficher les rapports immédiatement après leur génération, cliquez sur **Attendre que les rapports soient générés avant de les afficher**. Chaque rapport s'ouvre dans sa propre fenêtre.
    -   Pour afficher les rapports ultérieurement, cliquez sur **Générer des rapports en arrière-plan**.

    Les deux options enregistrent les rapports et, si vous avez activé la remise par courrier électronique, envoient les rapports aux administrateurs dans les formats que vous avez sélectionnés.

## <a name="see-also"></a>Articles associés

-   [Gestion des rapports de stockage](storage-reports-management.md)
-   [Définition des options du Gestionnaire de ressources du serveur de fichiers](setting-file-server-resource-manager-options.md)

