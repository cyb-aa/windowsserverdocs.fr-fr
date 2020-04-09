---
title: Hyper-V sur Windows Server
description: Fournit des liens vers des articles clés sur l’essai, la planification, le déploiement et la gestion d’Hyper-V.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 0baef6b8-598c-4fe0-9f31-5869fc4e0f69
author: kbdazure
ms.author: kathydav
ms.date: 10/07/2016
ms.openlocfilehash: e75f61fb957929e668b3a1663402786cc399abe8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853252"
---
# <a name="hyper-v-on-windows-server"></a>Hyper-V sur Windows Server

>S’applique à : Windows Server 2016, Windows Server 2019

Le rôle Hyper-V dans Windows Server vous permet de créer un environnement informatique virtualisé dans lequel vous pouvez créer et gérer des machines virtuelles. Vous pouvez exécuter plusieurs systèmes d’exploitation sur un même ordinateur physique et isoler les systèmes d’exploitation les uns des autres. Grâce à cette technologie, vous pouvez améliorer l’efficacité de vos ressources informatiques et libérer vos ressources matérielles.

Consultez les rubriques du tableau suivant pour en savoir plus sur Hyper-V sur Windows Server.

## <a name="hyper-v-resources-for-it-pros"></a>Ressources Hyper-V pour les professionnels de l’informatique

|Tâche |Ressources|
|---|---|
|![La case à cocher et l’icône de document pour afficher les spécifications sont satisfaites](media/All_Symbols_MeetsRequirements.png)|**Évaluer Hyper-V**<p>Présentation de la [technologie Hyper-V](Hyper-V-Technology-Overview.md) - <br />- [Nouveautés d’Hyper-V sur Windows Server](What-s-new-in-Hyper-V-on-Windows.md)<br />- [Configuration système requise pour Hyper-V sur Windows Server](System-requirements-for-Hyper-V-on-Windows.md)<br />- [systèmes d’exploitation invités Windows pris en charge pour Hyper-V](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md) <br />- [des machines virtuelles Linux et FreeBSD prises en charge](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)<br />- la [compatibilité des fonctionnalités par génération et invité](Hyper-V-feature-compatibility-by-generation-and-guest.md) <p>**Planifier Hyper-V**<p>- dois- [je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?](plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md) <br />- [planifier l’extensibilité d’Hyper-V dans Windows Server](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [planifier la mise en réseau Hyper-V dans Windows Server](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [planifier la sécurité Hyper-V dans Windows Server](plan/plan-hyper-v-security-in-windows-server.md)|
|![Icône curseur et rafale](media/All_Symbols_GetStarted.png)|**Prise en main d’Hyper-V**<p>- [Télécharger et installer Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)<p>**Option d’installation Server Core ou GUI de Windows Server 2019 en tant qu’ordinateur hôte d’ordinateur virtuel**<p>- [installer le rôle Hyper-V sur Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)<br />- [créer un commutateur virtuel pour les machines virtuelles Hyper-V](get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)<br />- [créer une machine virtuelle dans Hyper-V](get-started/Create-a-virtual-machine-in-Hyper-V.md)|
|![Icône personne et outils](media/All_Symbols_Administrator.png)|**Mettre à niveau les ordinateurs hôtes Hyper-V et les machines virtuelles**<p>- [mettre à niveau les nœuds de cluster Windows Server](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)<br />[version de la machine virtuelle - mise à niveau](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)<p>**Configurer et gérer Hyper-V**<p>- [configurer des hôtes pour la migration dynamique sans clustering de basculement](deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)<br />- de la [gestion à distance de nano Server](../../get-started/manage-nano-server.md)<br />- [choisir entre des points de contrôle standard ou de production](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)<br />- [activer ou désactiver les points de contrôle](manage/Enable-or-disable-checkpoints-in-Hyper-V.md)<br />- [gérer des machines virtuelles Windows avec PowerShell direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)<br />- [configurer la réplication Hyper-V](manage/Set-up-Hyper-V-Replica.md)|
|![Icône de bulles de conversation](media/All_Symbols_Chat.png)|**Blogs**<p>Découvrez les dernières publications des responsables de programme, des chefs de produit, des développeurs et des testeurs dans les équipes de virtualisation Microsoft et Hyper-V.<p>[blog sur la virtualisation](https://blogs.technet.com/b/virtualization/) - <br />[Blog - Windows Server](https://blogs.technet.com/b/windowsserver/)<br />- [blog sur la virtualisation de Ben Armstrong](https://blogs.msdn.com/b/virtual_pc_guy/) (Archivé)|
|![Icône de groupe d’utilisateurs](media/All_Symbols_Users_Group.png)|**Forum et groupes de discussion**<p>Vous avez des questions ? Communiquez avec vos pairs, MVP et l’équipe produit Hyper-V.<p>- [communauté Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)<br />[Forum TechNet sur - Windows Server Hyper-V](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserverhyperv)|

## <a name="related-technologies"></a>Technologies associées

Le tableau suivant répertorie les technologies que vous pouvez utiliser dans votre environnement informatique de virtualisation.

|Technologie|Description|
|--------------|---------------|
|[Client Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index)|La technologie de virtualisation incluse dans Windows 8, Windows 8.1 et Windows 10 que vous pouvez installer par le biais de **programmes et fonctionnalités** dans le **panneau de configuration**.|
|[Clustering avec basculement](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering)|Fonctionnalité Windows Server qui fournit une haute disponibilité pour les ordinateurs hôtes Hyper-V et les machines virtuelles.|
|[Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)|Composant de System Center qui fournit une solution de gestion pour le centre de donnes virtualisé. Vous pouvez configurer et gérer vos hôtes de virtualisation, vos réseaux et vos ressources de stockage afin de pouvoir créer et déployer des ordinateurs virtuels et des services sur des clouds privés que vous avez créés.|
|[Conteneurs Windows](https://docs.microsoft.com/virtualization/windowscontainers/)|Utilisez les conteneurs Windows Server et Hyper-V pour fournir des environnements standardisés pour les équipes de développement, de test et de production.|