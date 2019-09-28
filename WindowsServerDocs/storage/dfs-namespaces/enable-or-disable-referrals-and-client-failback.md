---
title: Activer ou désactiver les références et la restauration du client
description: Cet article décrit comment activer ou désactiver les références et la restauration du client.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e7dd11b530c61e2536db425d3e85e0fbe458d349
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386219"
---
# <a name="enable-or-disable-referrals-and-client-failback"></a>Activer ou désactiver les références et la restauration du client

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Une référence correspond à une liste de serveurs, organisée selon un ordre particulier, qu’un ordinateur client reçoit d’un contrôleur de domaine ou d’un serveur d’espace de noms lorsque l’utilisateur accède à une racine d’espace de noms ou à un dossier DFS avec cibles. Une fois en possession de la référence, l’ordinateur tente d’accéder au premier serveur de la liste. Si ce serveur n’est pas disponible, l’ordinateur client tente d’accéder au suivant. Si un serveur devient indisponible, vous pouvez configurer les clients pour qu’ils soient restaurés automatiquement sur le serveur préféré dès que celui-ci est disponible.

Les sections suivantes fournissent des informations sur la façon d’activer ou de désactiver des références, ou d’activer la restauration du client :

## <a name="enable-or-disable-referrals"></a>Activer ou désactiver des références

En désactivant la référence d’un serveur d’espaces de noms ou d’une cible de dossier, vous pouvez empêcher la redirection des utilisateurs vers ce serveur d’espaces de noms ou cette cible de dossier. Cela est utile si vous devez mettre temporairement un serveur hors connexion pour des raisons de maintenance.

-   Pour activer ou désactiver des références vers une cible de dossier, procédez comme suit :

    1.  Dans l’arborescence de la console Gestion du système de fichiers distribués DFS, sous le nœud **Espaces de noms**, cliquez sur un dossier contenant des cibles, puis dans le volet **Détails**, cliquez sur l’onglet **Cibles de dossier**.
    2.  Cliquez avec le bouton droit sur la cible de dossier de votre choix, puis cliquez sur **Désactiver la cible du dossier** ou sur **Activer la cible du dossier**.

-   Pour activer ou désactiver des références vers un serveur d’espaces de noms, procédez comme suit :

    1.  Dans l’arborescence de la console Gestion du système de fichiers distribués DFS, sélectionnez l’espace de noms approprié, puis cliquez sur l’onglet **Serveurs d’espaces de noms**.
    2.  Cliquez avec le bouton droit sur le serveur d’espaces de noms de votre choix, puis cliquez sur **Désactiver le serveur d’espaces de noms** ou sur **Activer le serveur d’espaces de noms**.


> [!TIP]
> Pour activer ou désactiver les références à l’aide de Windows PowerShell, utilisez les applets de commande [Set-DfsnRootTarget – State](https://technet.microsoft.com/library/jj884266.aspx) ou [Set-DfsnServerConfiguration](https://technet.microsoft.com/library/jj884277.aspx) , qui ont été introduites dans Windows Server 2012.

## <a name="enable-client-failback"></a>Activer la restauration automatique du client

Si une cible devient indisponible, vous pouvez configurer les clients pour qu’ils soient restaurés automatiquement sur cette cible après sa restauration. Pour que la restauration automatique fonctionne, les ordinateurs clients doivent remplir les conditions requises décrites dans la rubrique suivante : [Passez en revue la configuration requise pour les espaces de noms DFS](https://technet.microsoft.com/library/cc771913(v=ws.11).aspx).


> [!NOTE]
> Pour activer la restauration du client sur une racine d’espace de noms à l’aide de Windows PowerShell, utilisez l’applet de commande [Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx). Pour activer la restauration du client sur un dossier DFS, utilisez l’applet de commande [Set-DfsnFolder](https://technet.microsoft.com/library/jj884283.aspx).


## <a name="to-enable-client-failback-for-a-namespace-root"></a>Pour activer la restauration automatique des clients sur une racine d’espace de noms

1.  Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestion du système de fichiers distribués DFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un espace de noms, puis cliquez sur **Propriétés**.

3.  Sous l’onglet **Références**, activez la case à cocher **Restauration automatique des clients sur les cibles préférées**.

Les dossiers avec cibles héritent, de la racine de l’espace de noms, les paramètres de restauration automatique des clients. Si la restauration automatique des clients est désactivée sur la racine de l’espace de noms, utilisez la procédure suivante pour permettre la restauration automatique du client sur un dossier avec cibles.

## <a name="to-enable-client-failback-for-a-folder-with-targets"></a>Pour activer la restauration automatique des clients sur un dossier avec cibles

1.  Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestion du système de fichiers distribués DFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un dossier avec cibles, puis cliquez sur **Propriétés**.

3.  Sous l’onglet **Références**, activez la case à cocher **Restauration automatique des clients sur les cibles préférées**.

## <a name="see-also"></a>Voir aussi 

-   [Optimisation des espaces de noms DFS](tuning-dfs-namespaces.md)
-   [Examiner la configuration requise du client des espaces de noms DFS](https://technet.microsoft.com/library/cc771913(v=ws.11).aspx)
-   [Déléguer les autorisations de gestion pour les espaces de noms DFS](delegate-management-permissions-for-dfs-namespaces.md)