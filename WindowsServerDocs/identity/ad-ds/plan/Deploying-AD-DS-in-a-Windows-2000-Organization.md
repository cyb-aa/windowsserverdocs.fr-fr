---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: Déploiement des services ADDS dans une organisation Windows2000
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cad5deb32a31f15277c3e0e985d5b7d9b07856aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408911"
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>Déploiement des services ADDS dans une organisation Windows2000

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si votre organisation exécute actuellement Windows 2000 Active Directory, vous pouvez déployer Windows Server 2008 Active Directory Domain Services (AD DS) en effectuant une mise à niveau sur place de tout ou partie des systèmes d’exploitation des contrôleurs de domaine vers Windows Server 2008 ou en introduisant des contrôleurs de domaine exécutant Windows Server 2008 dans votre environnement.  
  
Avant de pouvoir ajouter un contrôleur de domaine exécutant Windows Server 2008 à un domaine Windows 2000 Active Directory existant, vous devez exécuter **adprep**, un outil de ligne de commande. Adprep étend le schéma AD DS, met à jour les descripteurs de sécurité par défaut des objets sélectionnés et ajoute de nouveaux objets d’annuaire comme requis par certaines applications. Adprep est disponible sur le disque d’installation de Windows Server 2008 (\sources\adprep\adprep.exe). Pour plus d’informations, voir adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
> [!NOTE]  
> Si vous souhaitez effectuer une mise à niveau sur place d’un contrôleur de domaine Windows 2000 AD DS existant vers Windows Server 2008, vous devez d’abord mettre à niveau le serveur vers Windows Server 2003, puis le mettre à niveau vers Windows Server 2008.  
  
L’illustration suivante montre les étapes de déploiement de Windows Server 2008 AD DS dans un environnement réseau qui exécute actuellement Windows 2000 Active Directory.  
  
![déploiement dans une organisation Windows 2000](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> Si vous souhaitez définir le niveau fonctionnel du domaine ou de la forêt sur Windows Server 2008, tous les contrôleurs de domaine de votre environnement doivent exécuter le système d’exploitation Windows Server 2008.  
  
La consolidation des domaines de ressources et de comptes qui sont mis à niveau à partir d’un environnement Windows 2000 dans le cadre de votre déploiement de Windows Server 2008 AD DS peut nécessiter une restructuration de domaines interforêt ou à l’origine. La restructuration AD DS domaines entre les forêts vous permet de réduire la complexité de votre organisation et les coûts administratifs associés. La restructuration AD DS domaines au sein d’une forêt vous aide à réduire la charge administrative de votre organisation en réduisant le trafic de réplication, en réduisant la quantité d’administration des utilisateurs et des groupes requise et en simplifiant l’administration de stratégie de groupe. Pour plus d’informations, consultez le Guide de migration d’ADMT v 3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Pour obtenir la liste des tâches détaillées que vous pouvez utiliser pour planifier et déployer des AD DS dans une organisation qui exécute actuellement Windows 2000 Active Directory, consultez [liste de vérification : déploiement de AD DS dans une organisation 2000 Windows](https://technet.microsoft.com/library/cc732737.aspx).  
  


