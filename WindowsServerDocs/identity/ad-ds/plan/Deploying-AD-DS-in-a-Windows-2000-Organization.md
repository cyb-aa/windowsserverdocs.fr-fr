---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: Déploiement des services AD DS dans une organisation Windows 2000
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5015c0ca2c01fca966927af4207a7e3501d9cd39
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854280"
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>Déploiement des services AD DS dans une organisation Windows 2000

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si votre organisation est en cours d’exécution Windows 2000 Active Directory, vous pouvez déployer Windows Server 2008 Active Directory domaine Services (AD DS) à l’exécution d’une mise à niveau sur place de tout ou partie des systèmes d’exploitation de vos contrôleurs de domaine pour Windows Server 2008 ou en introduisant des contrôleurs de domaine exécutant Windows Server 2008 dans votre environnement.  
  
Avant de pouvoir ajouter un contrôleur de domaine exécutant Windows Server 2008 à un domaine Windows 2000 Active Directory existant, vous devez exécuter **adprep**, un outil de ligne de commande. Adprep étend le schéma AD DS, met à jour des descripteurs de sécurité par défaut des objets sélectionnés et ajoute de nouveaux objets répertoire comme requis par certaines applications. Adprep est disponible sur le disque d’installation de Windows Server 2008 (\sources\adprep\adprep.exe). Pour plus d’informations, consultez Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
> [!NOTE]  
> Si vous souhaitez effectuer une mise à niveau sur place d’un contrôleur de domaine Windows 2000 AD DS existant vers Windows Server 2008, vous devez tout d’abord mettre à niveau le serveur vers Windows Server 2003 et ensuite la mise à niveau vers Windows Server 2008.  
  
L’illustration suivante montre les étapes pour déployer Windows Server 2008 AD DS dans un environnement réseau qui est en cours d’exécution Windows 2000 Active Directory.  
  
![déploiement dans une organisation de 2000 windows](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> Si vous souhaitez définir le niveau fonctionnel du domaine ou forêt vers Windows Server 2008, tous les contrôleurs de domaine dans votre environnement doivent exécuter le système d’exploitation Windows Server 2008.  
  
Consolidation des domaines de ressources et des comptes qui sont mis à niveau sur place à partir d’un environnement Windows 2000 en tant que partie de votre AD DS de Windows Server 2008 déploiement peut nécessiter entre forêts ou intraforêt la restructuration de domaines. Restructuration de domaines AD DS entre forêts vous permet de réduire la complexité de votre organisation et les coûts administratifs associés. Restructuration de domaines AD DS au sein d’une forêt vous aide à réduire les coûts d’administration pour votre organisation en réduisant le trafic de réplication, réduisant la quantité de l’administration des utilisateurs et groupe qui est requise et ce qui simplifie l’administration de Stratégie de groupe. Pour plus d’informations, consultez le Guide de Migration ADMT 3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Pour obtenir la liste détaillée des tâches que vous pouvez utiliser pour planifier et déployer des services AD DS dans une organisation qui est en cours d’exécution Windows 2000 Active Directory, consultez [liste de vérification : Déploiement des services AD DS dans une organisation de 2000 Windows](https://technet.microsoft.com/library/cc732737.aspx).  
  


