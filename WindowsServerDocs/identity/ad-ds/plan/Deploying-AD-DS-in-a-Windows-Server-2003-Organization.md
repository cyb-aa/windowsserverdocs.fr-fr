---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: Déploiement des services ADDS dans une organisation Windows Server2003
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7426ba8fa3ea5077510655ea4877d0e0b69226ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408906"
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>Déploiement des services ADDS dans une organisation Windows Server2003

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si votre organisation exécute actuellement Windows Server 2003 Active Directory, vous pouvez déployer Windows Server 2008 Active Directory Domain Services (AD DS) en effectuant une mise à niveau sur place de tout ou partie des systèmes d’exploitation des contrôleurs de domaine vers  Windows Server 2008 ou en introduisant des contrôleurs de domaine exécutant Windows Server 2008 dans votre environnement.  
  
Avant de pouvoir ajouter un contrôleur de domaine exécutant Windows Server 2008 à un domaine Windows Server 2003 Active Directory existant, vous devez exécuter **adprep**, un outil de ligne de commande. Adprep étend le schéma AD DS, met à jour les descripteurs de sécurité par défaut des objets sélectionnés et ajoute de nouveaux objets d’annuaire comme requis par certaines applications. Adprep est disponible sur le disque d’installation de Windows Server 2008 (\sources\adprep\adprep.exe). Pour plus d’informations, voir adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
L’illustration suivante montre les étapes de déploiement de Windows Server 2008 AD DS dans un environnement réseau qui exécute actuellement Windows Server 2003 Active Directory.  
  
![déployer dans une organisation Windows 2003](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)  
  
> [!NOTE]  
> Si vous souhaitez définir le niveau fonctionnel du domaine ou de la forêt sur Windows Server 2008, tous les contrôleurs de domaine de votre environnement doivent exécuter le système d’exploitation Windows Server 2008.  
  
La consolidation des domaines de ressources et des domaines de compte mis à niveau à partir d’un environnement Windows Server 2003 dans le cadre de votre déploiement de Windows Server 2008 AD DS peut nécessiter une restructuration de domaines interforêt ou à l’origine. La restructuration de AD DS domaines entre les forêts vous permet de réduire la complexité de la représentation de votre organisation dans AD DS et contribue à réduire les coûts administratifs associés. La restructuration AD DS domaines au sein d’une forêt vous permet de réduire la charge administrative de votre organisation en réduisant le trafic de réplication, en réduisant la quantité d’administration des utilisateurs et des groupes requise et en simplifiant l’administration du groupe. Renvoi. Pour plus d’informations, consultez le Guide de migration d’ADMT v 3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Pour obtenir la liste des tâches détaillées que vous pouvez utiliser pour planifier et déployer des AD DS dans une organisation qui exécute Windows Server 2003 Active Directory, consultez [Checklist : Déploiement de AD DS dans une organisation Windows Server 2003 @ no__t-0.  
  


