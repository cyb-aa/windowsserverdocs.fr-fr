---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: "Déploiement des services ADDS dans une organisation Windows2000"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1c46ff6aee03eee4169dbfe0cdff99febad312d2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>Déploiement des services ADDS dans une organisation Windows2000

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Si votre organisation est en cours d’exécution Windows2000 ActiveDirectory, vous pouvez déployer Windows Server2008 ActiveDirectory Services de domaine (ADDS) par l’exécution d’une mise à niveau sur place de tout ou partie des systèmes d’exploitation de contrôleurs de domaine vers Windows Server2008 ou en introduisant des contrôleurs de domaine exécutant Windows Server2008dans votre environnement.  
  
Avant de pouvoir ajouter un contrôleur de domaine exécutant Windows Server2008 à un domaine Windows2000 ActiveDirectory existant, vous devez exécuter **adprep**, un outil de ligne de commande. Adprep étend le schéma ADDS, mises à jour des descripteurs de sécurité par défaut des objets sélectionnés et ajoute de nouveaux objets répertoire comme requis par certaines applications. Adprep est disponible sur le disque d’installation de Windows Server2008 (\sources\adprep\adprep.exe). Pour plus d’informations, voir Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
> [!NOTE]  
> Si vous souhaitez effectuer une mise à niveau sur place d’un contrôleur de domaine ADDS de Windows2000 existant vers Windows Server2008, vous devez tout d’abord mettre à niveau le serveur vers Windows Server2003 et puis la mettre à niveau vers Windows Server2008.  
  
L’illustration suivante montre les étapes pour le déploiement de Windows Server2008 ADDS dans un environnement réseau qui est en cours d’exécution ActiveDirectory Windows2000.  
  
![Déploiement dans une organisation de 2000windows](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> Si vous souhaitez définir le niveau fonctionnel du domaine ou de la forêt vers Windows Server2008, tous les contrôleurs de domaine dans votre environnement doivent exécuter le système d’exploitation Windows Server2008.  
  
Consolidation des domaines de compte et de ressources qui sont mis à niveau en place à partir d’un environnement Windows2000dans le cadre de votre ADDS de Windows Server2008 déploiement peut nécessiter entre forêts ou la restructuration de domaines intraforêt sur plusieurs domaines. Restructuration de domaines entre forêts ADDS vous permet de réduire la complexité de votre organisation et les coûts d’administration. Restructuration de domaines ADDS au sein d’une forêt vous aide à réduire les coûts d’administration pour votre organisation en ce qui réduit le trafic de réplication, ce qui réduit la quantité de l’administration des utilisateurs et groupe qui est requise et simplification de l’administration de stratégie de groupe. Pour plus d’informations, voir le Guide de Migration ADMT 3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Pour obtenir une liste détaillée des tâches que vous pouvez utiliser pour planifier et déployer les services ADDS dans une organisation qui est en cours d’exécution Windows2000 ActiveDirectory, voir [liste de vérification: déploiement des services ADDS dans une organisation Windows2000](https://technet.microsoft.com/library/cc732737.aspx).  
  


