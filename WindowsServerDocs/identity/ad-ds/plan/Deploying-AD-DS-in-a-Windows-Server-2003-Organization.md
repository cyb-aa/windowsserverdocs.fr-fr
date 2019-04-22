---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: Déploiement des services ADDS dans une organisation Windows Server2003
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 033973ad7a726054f6c47c7154fa54a3767beab4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816360"
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>Déploiement des services ADDS dans une organisation Windows Server2003

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si votre organisation est en cours d’exécution Windows Server 2003 Active Directory, vous pouvez déployer Windows Server 2008 Active Directory domaine Services (AD DS) en effectuant d’une mise à niveau sur place de tout ou partie des systèmes d’exploitation de vos contrôleurs de domaine à  Windows Server 2008 ou en introduisant des contrôleurs de domaine exécutant Windows Server 2008 dans votre environnement.  
  
Avant de pouvoir ajouter un contrôleur de domaine exécutant Windows Server 2008 à un domaine Windows Server 2003 Active Directory existant, vous devez exécuter **adprep**, un outil de ligne de commande. Adprep étend le schéma AD DS, met à jour des descripteurs de sécurité par défaut des objets sélectionnés et ajoute de nouveaux objets répertoire comme requis par certaines applications. Adprep est disponible sur le disque d’installation de Windows Server 2008 (\sources\adprep\adprep.exe). Pour plus d’informations, consultez Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
L’illustration suivante montre les étapes de déploiement d’AD DS de Windows Server 2008 dans un environnement réseau qui est en cours d’exécution Windows Server 2003 Active Directory.  
  
![déployer dans une organisation de 2003 windows](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)  
  
> [!NOTE]  
> Si vous souhaitez définir le niveau fonctionnel du domaine ou forêt vers Windows Server 2008, tous les contrôleurs de domaine dans votre environnement doivent exécuter le système d’exploitation Windows Server 2008.  
  
Consolidation des domaines de ressources et les domaines de comptes qui sont mis à niveau sur place à partir d’un environnement Windows Server 2003 comme partie de votre déploiement AD DS de Windows Server 2008 peut nécessiter entre forêts ou intraforêt la restructuration de domaines. Restructuration de domaines AD DS entre forêts vous permet de réduire la complexité de la représentation de votre organisation dans AD DS, et il permet de réduire les coûts d’administration associés. Restructuration de domaines AD DS au sein d’une forêt vous aide à réduire les coûts d’administration pour votre organisation en réduisant le trafic de réplication, réduisant la quantité de l’administration des utilisateurs et groupe qui est requise et ce qui simplifie l’administration du groupe Stratégie. Pour plus d’informations, consultez le Guide de Migration ADMT 3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Pour obtenir la liste détaillée des tâches que vous pouvez utiliser pour planifier et déployer des services AD DS dans une organisation qui exécute Windows Server 2003 Active Directory, consultez [liste de vérification : Déploiement des services AD DS dans une organisation Windows Server 2003](https://technet.microsoft.com/library/cc771407.aspx).  
  


