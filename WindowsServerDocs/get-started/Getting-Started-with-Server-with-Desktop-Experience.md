---
title: Installation avec Expérience utilisateur
description: 'Explique comment obtenir et installer un serveur avec l’option Serveur avec Expérience utilisateur '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 01/18/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b38b8a0-4dfc-4130-be00-fc58bba99595
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: cf67a1c9675191936a6150bb950c59e6f99b54ad
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810696"
---
# <a name="install-server-with-desktop-experience"></a>Installation avec Expérience utilisateur
> S'applique à : Windows Server 2016
  

Quand vous installez Windows Server 2016 à l’aide de l’Assistant Installation, vous pouvez choisir entre **Windows Server 2016** et **Windows Server (Serveur avec Expérience utilisateur)** . L’option Serveur avec Expérience utilisateur est l’équivalent Windows Server 2016 de l’option d’installation complète disponible dans Windows Server 2012 R2 avec la fonctionnalité Expérience utilisateur installée. Si vous n’effectuez pas de choix dans l’Assistant Installation, **Windows Server 2016** est installé ; il s’agit de l’option d’**installation minimale** (Server Core).

L’option Serveur avec Expérience utilisateur permet d’installer l’interface utilisateur standard et tous les outils, y compris les fonctionnalités d’expérience client nécessitant une installation distincte dans Windows Server2012 R2. Les rôles serveur et les fonctionnalités sont installés à l’aide du Gestionnaire de serveur ou d’autres méthodes. Cette option nécessite plus d’espace disque que l’option d’installation minimale et présente des besoins en maintenance plus élevés. Nous vous recommandons donc de choisir l’installation minimale si vous n’avez pas besoin d’utiliser les éléments d’interface utilisateur et les outils de gestion graphiques que l’option Serveur avec Expérience utilisateur fournit en plus. Si vous pensez pouvoir vous passer des éléments supplémentaires, voir [Installation minimale de Windows Server](Getting-Started-with-Server-Core.md). Pour bénéficier d’une option encore plus allégée, voir [Installer Nano Server](Getting-Started-with-Nano-Server.md).

> [!NOTE]
>
> À la différence des versions précédentes de Windows Server, vous ne pouvez pas basculer entre l’installation minimale et l’installation avec Expérience utilisateur après l’installation. Si vous procédez à l’installation avec Expérience utilisateur et décidez ultérieurement d’utiliser l’installation minimale, vous devez procéder à une nouvelle installation.

**Interface utilisateur :** interface utilisateur graphique standard (interpréteur de commandes graphique de serveur). L’interpréteur de commandes graphique de serveur inclut le nouvel interpréteur de Windows 10. Les fonctionnalités Windows spécifiques installées par défaut avec cette option sont User-Interfaces-Infra, Server-GUI-Shell, Server-GUI-Mgmt-Infra, InkAndHandwritingServices, ServerMediaFoundation et Expérience utilisateur. Bien que ces fonctionnalités apparaissent bien dans le Gestionnaire de serveur dans cette version, leur désinstallation n’est pas prise en charge et elles ne seront pas disponibles dans les futures versions.

**Installation, configuration et désinstallation des rôles serveur en local :** avec le Gestionnaire de serveur ou Windows PowerShell

**Installation, configuration et désinstallation des rôles serveur à distance :** avec le Gestionnaire de serveur, le serveur distant, les outils d’administration de serveur distant (RSAT) ou Windows PowerShell

**Microsoft Management Console : installé**

## <a name="installation-scenarios"></a>Scénarios d’installation

### <a name="evaluation"></a>Évaluation
Vous pouvez obtenir une version d’évaluation, avec licence de 180jours, de Windows Server dans la page [Windows Server Evaluations (Évaluations de Windows Server)](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016). Choisissez l’option **Windows Server 2016 | 64 bits ISO** pour le téléchargement ou rendez-vous dans le **Virtual Lab Windows Server 2016**.

> [!IMPORTANT]  
> Pour les versions de Windows Server 2016 antérieures à la version 14393.0.161119-1705.RS1_REFRESH, vous pouvez uniquement effectuer cette conversion d’une version d’évaluation vers une version commerciale si Windows Server 2016 a été installé à l’aide de l’option Serveur avec Expérience de bureau (et non l’option d’installation minimale). Avec la version 14393.0.161119-1705. RS1_REFRESH et les versions ultérieures, vous pouvez convertir les versions d’évaluation vers une version commerciale, quelle que soit l’option d’installation utilisée.


### <a name="clean-installation"></a>Nouvelle installation

Pour installer l’option Serveur avec Expérience utilisateur à partir du média, insérez le média dans un lecteur, redémarrez l’ordinateur et exécutez Setup.exe. Dans l’Assistant qui s’ouvre, sélectionnez **Windows Server (Serveur avec Expérience utilisateur)** (Standard ou Datacenter), puis exécutez l’Assistant.

### <a name="upgrade"></a>Mettre à niveau/Mise à niveau
Une **mise à niveau** correspond au passage d’une version de système d’exploitation existante à une version plus récente, sur le même matériel.

Si vous disposez déjà d’une installation complète du produit Windows Server approprié, vous pouvez le mettre à niveau vers une installation Serveur avec Expérience utilisateur de l’édition appropriée de Windows Server 2016, comme indiqué ci-dessous.

> [!IMPORTANT]  
> Dans cette version, la mise à niveau convient le mieux aux machines virtuelles qui ne nécessitent pas de pilotes de matériel OEM spécifiques pour que l’opération réussisse. Dans les autres cas, la migration est l’option recommandée.  

- Les mises à niveau sur place des architectures 32 bits vers des architectures 64 bits ne sont pas prises en charge. Toutes les éditions de Windows Server2016 sont des éditions 64bits uniquement.
- Les mises à niveau sur place d’une langue vers une autre ne sont pas prises en charge.
- Si le serveur est un contrôleur de domaine, voir [Mettre à niveau des contrôleurs de domaine vers Windows Server2012 R2 et Windows Server2012](https://technet.microsoft.com/library/hh994618.aspx) pour obtenir des informations importantes.
- Les mises à niveau depuis des versions préliminaires de Windows Server 2016 ne sont pas prises en charge. Effectuez une nouvelle installation de Windows Server2016.
- Les mises à niveau qui passent d’une installation minimale à une installation Serveur avec Expérience utilisateur (et vice versa) ne sont pas prises en charge.

Si vous ne voyez pas votre version actuelle dans la colonne de gauche, cela signifie que la mise à niveau vers cette version de Windows Server2016 n’est pas prise en charge.

Si vous voyez plusieurs éditions dans la colonne de droite, la mise à niveau vers **toutes** ces éditions est prise en charge depuis la même version de départ.

|Si vous exécutez cette édition:|Vous pouvez effectuer une mise à niveau vers ces éditions :|  
|-------------------|----------|  
|Windows Server 2012 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server2012R2 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Essentials|WindowsServer2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|

Pour connaître les nombreuses options supplémentaires pour le passage à Windows Server 2016, telles que la conversion de licence pour les éditions avec licence en volume ou les éditions d’évaluation, voir [Options de mise à niveau](Supported-Upgrade-Paths.md).

### <a name="migration"></a>Migration
Une **migration** consiste à passer d’un système d’exploitation existant à Windows Server 2016 en effectuant une nouvelle installation sur une autre configuration matérielle ou machine virtuelle, puis en transférant les charges de travail de l’ancien serveur sur le nouveau serveur. La migration, qui dépend des rôles serveur installés, est décrite en détail dans l’article [Windows Server Installation, Upgrade, and Migration](https://technet.microsoft.com/windowsserver/dn458795) (Installation, mise à niveau et migration de Windows Server).

La capacité de migration varie selon le rôle serveur. Le tableau suivant présente les options de mise à niveau et de migration de rôle serveur spécifiquement pour le passage à Windows Server2016. Pour accéder à des guides de migration des rôles individuels, rendez-vous sur la page [Migration de rôles et de fonctionnalités dans Windows Server](https://technet.microsoft.com/windowsserver/jj554790.aspx). Pour plus d’informations sur l’installation et les mises à niveau, voir [Installation, mise à niveau et migration de Windows Server](https://technet.microsoft.com/windowsserver/dn458795).

|Rôle serveur|Mise à niveau possible depuis Windows Server 2012 R2 ?|Mise à niveau possible depuis Windows Server 2012 ?|Migration prise en charge?|Migration possible sans temps d’arrêt?|  
|-------------------|----------|--------------|--------------|----------|  
|Services de certificats Active Directory| Oui|    Oui|    Oui|    Non|
|Services de domaine Active Directory|  Oui|    Oui|    Oui|    Oui|
|Services de fédération Active Directory (AD FS)|  Non| Non| Oui|    Non (de nouveaux nœuds doivent être ajoutés à la batterie de serveurs)|
|Services AD LDS (Active Directory Lightweight Directory Services)|   Oui|    Oui|    Oui|    Oui|
|Services AD RMS (Active Directory Rights Management Services)|   Oui|    Oui|    Oui|    Non|
|Cluster de basculement|Oui, avec le processus de [mise à niveau propagée du système d’exploitation de cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade), qui inclut la mise en pause, le drainage et la suppression de nœuds, la mise à niveau vers Windows Server 2016 et le retour au cluster d’origine. Oui, lorsque le serveur est supprimé par le cluster lors de la mise à niveau, puis ajouté à un autre cluster.|Pas lorsque le serveur fait partie d’un cluster. Oui, lorsque le serveur est supprimé par le cluster lors de la mise à niveau, puis ajouté à un autre cluster.  |Oui|Pas pour les clusters de basculement Windows Server 2012. Oui, pour les clusters de basculement Windows Server 2012 R2 avec des machines virtuelles Hyper-V ou les clusters de basculement Windows Server 2012 R2 exécutant le rôle Serveur de fichiers avec montée en puissance parallèle. Voir [Mise à niveau propagée du système d’exploitation de cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).|
|Services de fichiers et de stockage| Oui|    Oui|    Variable selon la sous-fonctionnalité|  Non|
|Services d’impression et de télécopie|    Non| Non| Oui (Printbrm.exe)| Non|
|Services Bureau à distance|   Oui, pour tous les sous-rôles, mais la batterie de serveurs en mode mixte n’est pas prise en charge.|   Oui, pour tous les sous-rôles, mais la batterie de serveurs en mode mixte n’est pas prise en charge.|   Oui|    Non|
|Serveur Web (IIS)|  Oui|    Oui|    Oui|    Non|
|Expérience Windows Server Essentials|  Oui|    N/A– Nouvelle fonctionnalité|  Oui|    Non|
|Windows Server Update Services|    Oui|    Oui|    Oui|    Non|
|Dossiers de travail|  Oui|    Oui|    Oui|    Oui, depuis un cluster Windows Server 2012 R2 avec la [mise à niveau propagée du système d’exploitation de cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).|

> [!IMPORTANT]  
> Une fois que l’exécution du programme d’installation est terminée et que vous avez installé tous les rôles et fonctionnalités de serveur dont vous avez besoin, recherchez et installez les mises à jour disponibles pour Windows Server 2016 à l’aide de Windows Update ou d’autres méthodes de mise à jour.

---------------------------------------
Si vous avez besoin d’une autre option d’installation, ou si vous avez terminé l’installation et que vous êtes prêt à déployer des charges de travail spécifiques, vous pouvez retourner à la [page principale de Windows Server2016](Windows-Server-2016.md).