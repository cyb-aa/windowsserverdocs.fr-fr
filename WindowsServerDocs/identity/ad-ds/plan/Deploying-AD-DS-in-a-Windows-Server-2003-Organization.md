---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: "Déploiement des services AD DS dans un Windows Server2003"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: df1267ded5ece95dd5a3ab17e4ec6ad5d87a2f12
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>Déploiement des services AD DS dans un Windows Server2003

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Si votre organisation exécute actuellement Windows Server2003 ActiveDirectory, vous pouvez déployer Windows Server2008 ActiveDirectory Services de domaine (ADDS) par l’exécution d’une mise à niveau sur place de tout ou partie des systèmes d’exploitation de contrôleurs de domaine vers Windows Server2008 ou en introduisant des contrôleurs de domaine exécutant Windows Server2008dans votre environnement.  
  
Avant de pouvoir ajouter un contrôleur de domaine exécutant Windows Server2008 à un domaine Windows Server2003 ActiveDirectory existant, vous devez exécuter **adprep**, un outil de ligne de commande. Adprep étend le schéma ADDS, mises à jour des descripteurs de sécurité par défaut des objets sélectionnés et ajoute de nouveaux objets répertoire comme requis par certaines applications. Adprep est disponible sur le disque d’installation de Windows Server2008 (\sources\adprep\adprep.exe). Pour plus d’informations, voir Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
L’illustration suivante montre les étapes de déploiement d’ADDS Windows Server2008dans un environnement réseau qui exécute Windows Server2003 ActiveDirectory.  
  
![Déployer dans une organisation de 2003windows](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)  
  
> [!NOTE]  
> Si vous souhaitez définir le niveau fonctionnel du domaine ou de la forêt vers Windows Server2008, tous les contrôleurs de domaine dans votre environnement doivent exécuter le système d’exploitation Windows Server2008.  
  
Consolidation des domaines de ressources et les domaines de comptes qui sont mis à niveau en place à partir d’un environnement Windows Server2003 comme partie de votre déploiement ADDS Windows Server2008 peut nécessiter entre forêts ou la restructuration de domaines intraforêt sur plusieurs domaines. Restructuration de domaines entre forêts ADDS vous permet de réduire la complexité de la représentation de votre organisation dans les services ADDS, et il permet de réduire les coûts d’administration. Restructuration de domaines ADDS au sein d’une forêt vous permet de réduire les coûts d’administration pour votre organisation en ce qui réduit le trafic de réplication, ce qui réduit la quantité de l’administration des utilisateurs et groupe qui est requise et simplification de l’administration de stratégie de groupe. Pour plus d’informations, voir le Guide de Migration ADMT 3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Pour obtenir une liste détaillée des tâches que vous pouvez utiliser pour planifier et déployer les services ADDS dans une organisation qui exécute Windows Server2003 ActiveDirectory, voir [liste de vérification: déploiement des services ADDS dans une organisation Windows Server2003](https://technet.microsoft.com/library/cc771407.aspx).  
  


