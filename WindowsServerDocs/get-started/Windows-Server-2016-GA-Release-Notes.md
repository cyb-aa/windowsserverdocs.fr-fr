---
title: 'Notes de publication : problèmes importants dans Windows Server 2016'
description: Résume les problèmes critiques nécessitant une solution de contournement pour éviter une panne, un blocage, un échec d’installation ou une perte de données.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 11/13/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 5a25a9152298a38ad77a377a87b71917ee586947
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878650"
---
# <a name="release-notes-important-issues-in-windows-server-2016"></a>Notes de publication : Problèmes importants dans Windows Server 2016

>S'applique à : Windows Server 2016

Ces notes de publication résument les problèmes les plus critiques du système d’exploitation Windows Server&reg; 2016 et expliquent, le cas échéant, comment les éviter ou les résoudre. Pour plus sur d’informations sur les modifications de l’interface, les nouvelles fonctionnalités et les correctifs de cette version, consultez [What’s New in Windows Server 2016](what-s-new-in-windows-server-2016.md) (Nouveautés de Windows Server 2016) et les annonces publiées par les équipes en charge des différentes fonctionnalités. Sauf mention contraire, chaque problème signalé s’applique à toutes les éditions et options d’installation de Windows Server2016.  

Ce document est continuellement mis à jour. Les problèmes critiques nécessitant une solution de contournement sont ajoutés dès qu’ils sont identifiés, tout comme les solutions de contournement et les correctifs.  

## <a name="express-updates-available-starting-in-november-2018-new"></a>Express mises à jour disponibles à compter de novembre 2018 (nouveau)

En commençant par le novembre 2018 mise à jour de « Mettre à jour le mardi », Windows à nouveau publiera [mises à jour rapides](express-updates.md) pour Windows Server 2016. Si vous utilisez WSUS et System Center Configuration Manager (SCCM) vous verrez une fois encore deux packages pour la mise à jour de Windows Server 2016 : une mise à jour complète et une mise à jour Express. Si vous souhaitez utiliser Express pour vos environnements de serveurs, vous devez vérifier que le serveur a mis une mise à jour complète depuis novembre 2017 (KB # 4048953) pour garantir que la mise à jour Express s’installe correctement. Si vous essayez une mise à jour Express sur un serveur qui n’a pas été mis à jour depuis la mise à jour de 11 b 2017 (KB # 4048953), vous verrez des échecs répétés qui consomment de la bande passante et des ressources de processeur dans une boucle infinie. Si vous obtenez dans ce scénario, arrêtez d’envoyer la mise à jour Express et transmettre à la place une récente mise à jour complète pour arrêter la boucle de défaillance.  

## <a name="server-core-installation-option"></a>Option d’installation minimale
[comment]: # (ID: 370 ; Demandeur : amason ; état : déconnecté)  
Lorsque vous installez Windows Server 2016 à l’aide de l’option d’installation Server Core (Installation minimale), le spouleur d’impression s’installe et démarre par défaut, même lorsque le rôle Serveur d’impression n’est pas installé.

Pour éviter cela, après le premier démarrage, désactivez le spouleur d’impression.


## <a name="containers"></a>Conteneurs  

[comment]: # (ID: 371 ; Demandeur : taylorb ; état : déconnecté)  
- Avant d’utiliser des conteneurs, installez [mise à jour de pile de maintenance pour Windows 10 Version 1607 : Le 23 août 2016](https://support.microsoft.com/en-us/kb/3176936) ou les mises à jour ultérieures qui sont disponibles. Sinon, un certain nombre de problèmes peut se produire, notamment les échecs de création, démarrage ou exécuter des conteneurs et des erreurs similaires à « CreateProcess a échoué dans Win32 : Le serveur RPC n’est pas disponible. »

[comment]: # (ID: 373 ; Demandeur : plang ; état : déconnecté)  
- Le fournisseur NanoServerPackage OneGet ne fonctionne pas dans les conteneurs Windows. Pour contourner ce problème, utilisez Find-NanoServerPackage et Save-NanoServerPackage sur un autre ordinateur (pas un conteneur) pour télécharger le package requis. Ensuite, copiez les packages dans le conteneur, puis installez-les.

## <a name="device-guard"></a>Device Guard
[comment]: # (ID: 369 ; Demandeur : nirb ; état : déconnecté)
Si vous utilisez la protection basée sur la virtualisation de l’intégrité du code ou des machines virtuelles dotées d’une protection maximale (qui utilisent la protection basée sur la virtualisation de l’intégrité du code), vous devez avoir conscience que ces technologies peuvent être incompatibles avec certains appareils et applications. Vous devez tester ces configurations dans votre laboratoire avant d’activer les fonctionnalités dans les systèmes de production. Dans le cas contraire, une perte de données ou un arrêt inattendus pourraient se produire.

## <a name="microsoft-exchange"></a>Microsoft Exchange
[comment]: # (ID: 375 ; Demandeur : wgries ; état : déconnecté)
Si vous essayez d’exécuter Microsoft Exchange 2016 CU3 sur Windows Server 2016, vous rencontrerez des erreurs dans le processus hôte IIS W3WP.exe. Il n’existe pas de solution de contournement à ce jour. Vous devez reporter le déploiement d’Exchange 2016 CU3 sur Windows Server 2016 jusqu’à ce qu’un correctif pris en charge soit disponible.

## <a name="remote-server-administration-tools-rsat"></a>Outils d’administration de serveur distant (RSAT)
[comment]: # (ID: 374 ; Demandeur : ryanpu ; état : déconnecté)
Si vous exécutez une version de Windows10 antérieure à la mise à jour anniversaire et que vous utilisez Hyper-V et des machines virtuelles avec un module de plateforme sécurisée (TPM) virtuel activé (comprenant des machines virtuelles dotées d’une protection maximale), puis que vous installez la version des outils d’administration de serveur distant fournie pour Windows Server2016, les tentatives de démarrage de ces machines virtuelles échouent.

Pour éviter ce problème, mettez l’ordinateur client au niveau de la Mise à jour anniversaire de Windows10 (au minimum) avant d’installer les outils d’administration de serveur distant. Si l’opération a déjà été effectuée, désinstallez les outils d’administration de serveur distant, mettez le client au niveau de la Mise à jour anniversaire de Windows10, puis réinstallez les outils d’administration de serveur distant.


## <a name="shielded-virtual-machines"></a>Machines virtuelles dotées d’une protection maximale
[comment]: # (ID: 369 ; Demandeur : nirb ; état : déconnecté)  
- Assurez-vous que vous avez installé toutes les mises à jour disponibles avant de déployer des machines virtuelles dotées d’une protection maximale en production.

- Si vous utilisez la protection basée sur la virtualisation de l’intégrité du code ou des machines virtuelles dotées d’une protection maximale (qui utilisent la protection basée sur la virtualisation de l’intégrité du code), vous devez avoir conscience que ces technologies peuvent être incompatibles avec certains appareils et applications. Vous devez tester ces configurations dans votre laboratoire avant d’activer les fonctionnalités dans les systèmes de production. Dans le cas contraire, une perte de données ou un arrêt inattendus pourraient se produire.


## <a name="start-menu"></a>Menu Démarrer
[comment]: # (ID: 372 ; Demandeur : samli ; état : déconnecté)
Ce problème concerne les versions de Windows Server2016 installées avec l’option Server with Desktop Experience (Serveur avec expérience utilisateur).

Si vous installez des applications qui ajoutent des raccourcis dans un dossier du menu Démarrer, ces raccourcis ne fonctionneront pas tant que vous n’avez pas fermé la session en cours et ouvert une nouvelle session.



Revenez à la rubrique principale de [Windows Server2016](Windows-Server-2016.md).

## <a name="storport-performance"></a>Performances Storport
Certains systèmes peuvent présenter une baisse des performances de stockage lorsqu’ils exécutent une nouvelle installation de Windows Server 2016, par rapport à Windows Server 2012 R2.  Le développement de Windows Server 2016 a donné lieu à un certain nombre de modifications pour améliorer la sécurité et la fiabilité de la plateforme. Certaines de ces modifications, telles que l’activation de Windows Defender par défaut, ont pour résultat un allongement des chemins d’accès des E-S, ce qui peut réduire les performances d’E-S dans certains scénarios d’usage et modèles. Microsoft ne recommande pas de désactiver Windows Defender, car c’est une couche de protection importante de vos systèmes.  

## <a name="copyright"></a>Copyright  
Ce document est fourni «en l'état». Les informations et points de vue contenus dans ce document, y compris les URL et autres références à des sites web, peuvent faire l'objet de modifications sans préavis.  

Ce document ne vous fournit aucun droit légal de propriété intellectuelle de tout produit Microsoft. Vous pouvez copier le présent document pour une utilisation interne à des fins de référence.  

&copy;2016 Microsoft Corporation. Tous droits réservés.  

Microsoft, Active Directory, Hyper-V, Windows et WindowsServer sont soit des marques, soit des marques déposées de Microsoft Corporation aux États-Unis d’Amérique et/ou dans d’autres pays.  

Ce produit contient un logiciel de filtrage graphique qui est basé en partie sur le travail du groupe IndependentJPEGGroup.  


1.0  
