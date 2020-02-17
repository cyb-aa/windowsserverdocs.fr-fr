---
title: 'Notes de publication : problèmes importants dans Windows Server 2016'
description: Résume les problèmes critiques nécessitant une solution de contournement pour éviter une panne, un blocage, un échec d’installation ou une perte de données.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 11/13/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: f4568e1781dbe385d8abe8a96f07841391506738
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822162"
---
# <a name="release-notes-important-issues-in-windows-server-2016"></a>Notes de publication : Principaux problèmes touchant Windows Server 2016

>S'applique à : Windows Server 2016

Ces notes de publication résument les problèmes les plus critiques du système d’exploitation Windows Server 2016 et expliquent, le cas échéant, comment les éviter ou les résoudre. Pour plus sur d’informations sur les modifications de l’interface, les nouvelles fonctionnalités et les correctifs de cette version, consultez [What’s New in Windows Server 2016](whats-new-in-windows-server-2016.md) (Nouveautés de Windows Server 2016) et les annonces publiées par les équipes en charge des différentes fonctionnalités. Sauf mention contraire, chaque problème signalé s’applique à toutes les éditions et options d’installation de Windows Server 2016.

Ce document est continuellement mis à jour. Les problèmes critiques nécessitant une solution de contournement sont ajoutés dès qu’ils sont identifiés, tout comme les solutions de contournement et les correctifs.

## <a name="express-updates-available-starting-in-november-2018-new"></a>Disponibilité des mises à jour Express à partir de novembre 2018 (NOUVEAU)

À partir de la mise à jour hebdomadaire du mardi (« Update Tuesday ») en novembre 2018, Windows publiera à nouveau les [mises à jour Express](express-updates.md) pour Windows Server 2016. Si vous utilisez WSUS et Configuration Manager, vous verrez de nouveau deux packages pour la mise à jour de Windows Server 2016 : une mise à jour complète et une mise à jour Express. Si vous souhaitez utiliser Express pour vos environnements de serveur, vous devez vérifier que le serveur a appliqué une mise à jour complète depuis novembre 2017 (KB 4048953) pour vous assurer que la mise à jour Express s’installe correctement. Si vous tentez une mise à jour Express sur un serveur qui n’a pas été mis à jour depuis la mise à jour 11B de 2017 (KB 4048953), vous rencontrerez des échecs répétés qui consomment de la bande passante et des ressources processeur dans une boucle infinie. Si ce problème survient, arrêtez la transmission de la mise à jour Express. Au lieu de ça, transmettez une mise à jour complète et récente pour arrêter la boucle de défaillance.

## <a name="server-core-installation-option"></a>Option d’installation minimale

[comment]: # (ID : 370 ; utilisateur procédant à la soumission : amason ; état : validé)

Lorsque vous installez Windows Server 2016 à l’aide de l’option d’installation Server Core (Installation minimale), le spouleur d’impression s’installe et démarre par défaut, même lorsque le rôle Serveur d’impression n’est pas installé.

Pour éviter cela, après le premier démarrage, désactivez le spouleur d’impression.

## <a name="containers"></a>Conteneurs

[comment]: # (ID : 371 ; utilisateur procédant à la soumission : taylorb ; état : validé)
- Avant d’utiliser des conteneurs, installez [Service de mise à jour de la pile pour Windows 10 Version 1607 : le 23 août 2016](https://support.microsoft.com/kb/3176936) ou les mises à jour ultérieures disponibles. Sinon, plusieurs problèmes peuvent survenir, notamment des échecs de création, de démarrage ou d’exécution de conteneurs, ainsi que des erreurs de type « CreateProcess a échoué dans Win32: le serveur RPC n’est pas disponible. »

[comment]: # (ID : 373 ; utilisateur procédant à la soumission : plang ; état : validé)
- Le fournisseur NanoServerPackage OneGet ne fonctionne pas dans les conteneurs Windows. Pour contourner ce problème, utilisez Find-NanoServerPackage et Save-NanoServerPackage sur un autre ordinateur (pas un conteneur) pour télécharger le package requis. Ensuite, copiez les packages dans le conteneur, puis installez-les.

## <a name="device-guard"></a>Device Guard

[comment]: # (ID : 369 ; utilisateur procédant à la soumission : nirb ; état : validé)
Si vous utilisez la protection basée sur la virtualisation de l’intégrité du code ou des machines virtuelles dotées d’une protection maximale (qui utilisent la protection basée sur la virtualisation de l’intégrité du code), vous devez avoir conscience que ces technologies peuvent être incompatibles avec certains appareils et applications. Vous devez tester ces configurations dans votre laboratoire avant d’activer les fonctionnalités dans les systèmes de production. Dans le cas contraire, une perte de données ou un arrêt inattendus pourraient se produire.

## <a name="microsoft-exchange"></a>Microsoft Exchange

[comment]: # (ID : 375 ; utilisateur procédant à la soumission : wgries ; état : validé)
Si vous essayez d’exécuter Microsoft Exchange 2016 CU3 sur Windows Server 2016, vous rencontrerez des erreurs dans le processus hôte IIS W3WP.exe. Il n’existe pas de solution de contournement à ce jour. Vous devez reporter le déploiement d’Exchange 2016 CU3 sur Windows Server 2016 jusqu’à ce qu’un correctif pris en charge soit disponible.

## <a name="remote-server-administration-tools-rsat"></a>Outils d’administration de serveur distant (RSAT)

[comment]: # (ID : 374 ; utilisateur procédant à la soumission : ryanpu ; état : validé)
Si vous exécutez une version de Windows 10 antérieure à la mise à jour anniversaire et que vous utilisez Hyper-V et des machines virtuelles avec un module de plateforme sécurisée (TPM) virtuel activé (comprenant des machines virtuelles dotées d’une protection maximale), puis que vous installez la version des outils d’administration de serveur distant fournie pour Windows Server 2016, les tentatives de démarrage de ces machines virtuelles échouent.

Pour éviter ce problème, mettez l’ordinateur client au niveau de la Mise à jour anniversaire de Windows 10 (au minimum) avant d’installer les outils d’administration de serveur distant. Si l’opération a déjà été effectuée, désinstallez les outils d’administration de serveur distant, mettez le client au niveau de la Mise à jour anniversaire de Windows 10, puis réinstallez les outils d’administration de serveur distant.

## <a name="shielded-virtual-machines"></a>Machines virtuelles dotées d’une protection maximale

[comment]: # (ID : 369 ; utilisateur procédant à la soumission : nirb ; état : validé)  
- Assurez-vous que vous avez installé toutes les mises à jour disponibles avant de déployer des machines virtuelles dotées d’une protection maximale en production.

- Si vous utilisez la protection basée sur la virtualisation de l’intégrité du code ou des machines virtuelles dotées d’une protection maximale (qui utilisent la protection basée sur la virtualisation de l’intégrité du code), vous devez avoir conscience que ces technologies peuvent être incompatibles avec certains appareils et applications. Vous devez tester ces configurations dans votre laboratoire avant d’activer les fonctionnalités dans les systèmes de production. Dans le cas contraire, une perte de données ou un arrêt inattendus pourraient se produire.

## <a name="start-menu"></a>Menu Démarrer

[comment]: # (ID : 372 ; utilisateur procédant à la soumission : samli ; état : validé)
Ce problème concerne les versions de Windows Server 2016 installées avec l’option Server with Desktop Experience (Serveur avec expérience utilisateur).

Si vous installez des applications qui ajoutent des raccourcis dans un dossier du menu **Démarrer**, ces raccourcis ne fonctionneront pas tant que vous n’avez pas fermé la session en cours et ouvert une nouvelle session.

Revenez à la rubrique principale de [Windows Server 2016](Windows-Server-2016.md).

## <a name="storport-performance"></a>Performances Storport

Suite à l’installation de Windows Server 2016, certains systèmes peuvent présenter des performances de stockage réduites par rapport à Windows Server 2012 R2.  Dans le cadre du développement de Windows Server 2016, un certain nombre de modifications ont été apportées pour améliorer la sécurité et la fiabilité de la plateforme. Certaines de ces modifications, telles que l’activation de Windows Defender par défaut, ont pour résultat un allongement des chemins d’accès des E/S, ce qui peut réduire les performances d’E/S dans certains scénarios d’usage et modèles. Microsoft déconseille de désactiver Windows Defender car il s’agit d’une couche de protection importante pour vos systèmes.  

## <a name="copyright"></a>Copyright

Ce document est fourni « en l'état ». Les informations contenues dans ce document, y compris les URL et autres références de sites Web, pourront faire l’objet de modifications sans préavis.  

Ce document ne vous fournit aucun droit légal de propriété intellectuelle de tout produit Microsoft. Vous pouvez copier le présent document pour une utilisation interne à des fins de référence.  

&copy; 2016 Microsoft Corporation. Tous droits réservés.  

Microsoft, Active Directory, Hyper-V, Windows et Windows Server sont soit des marques, soit des marques déposées de Microsoft Corporation aux États-Unis d’Amérique et/ou dans d’autres pays.  

Ce produit contient un logiciel de filtrage graphique qui est basé en partie sur le travail du groupe Independent JPEG Group.  

1.0
