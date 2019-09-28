---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: Scénario d’assistance en cas d’accès refusé
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d3c1354c54cf421e59d6b37a44ce703f52b6f4b7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357482"
---
# <a name="scenario-access-denied-assistance"></a>Scénario : assistance en cas d'accès refusé

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les utilisateurs reçoivent un message de refus d'accès quand ils essaient d'accéder à des dossiers et fichiers partagés sur un serveur de fichiers pour lequel ils n'ont aucune autorisation. Souvent, les administrateurs ne disposent pas du contexte approprié, ce qui rend difficile la résolution du problème d'accès.  
  
## <a name="scenario-description"></a>Description du scénario  
L’assistance en cas d’accès refusé est une nouvelle fonctionnalité de Windows Server 2012, qui offre les méthodes suivantes pour résoudre les problèmes liés à l’accès aux fichiers et aux dossiers :  
  
-   **Auto-assistance.** Si l'utilisateur peut identifier le problème et y remédier pour pouvoir obtenir l'accès demandé, l'impact professionnel est faible et aucune exception spéciale n'est nécessaire dans la stratégie d'accès centralisée. L'assistance en cas d'accès refusé fournit un message de refus d'accès personnalisable par les administrateurs du serveur de fichiers avec des informations spécifiques à leur organisation. Par exemple, un administrateur peut définir le message pour que les utilisateurs puissent demander l'accès à un propriétaire de données sans faire appel à l'administrateur du serveur de fichiers.  
  
-   **Assistance de la part du propriétaire des données.** Vous pouvez définir une liste de distribution pour les dossiers partagés et la configurer pour que le propriétaire du dossier reçoive une notification par courrier électronique quand un utilisateur a besoin d'y accéder. Si le propriétaire des données ignore comment aider l'utilisateur a obtenir l'accès nécessaire, il peut transférer ces informations à l'administrateur du serveur de fichiers.  
  
-   **Assistance de la part de l'administrateur du serveur de fichiers.** Ce type d'assistance est disponible quand l'utilisateur ne peut pas résoudre un problème et que le propriétaire des données ne peut pas l'aider.  Windows Server 2012 fournit une interface utilisateur dans laquelle les administrateurs de serveur de fichiers peuvent afficher les autorisations effectives pour un utilisateur sur un fichier ou un dossier afin de faciliter la résolution des problèmes d’accès.  
  
L’assistance en cas d’accès refusé dans Windows Server 2012 fournit aux administrateurs de serveurs de fichiers les informations d’accès pertinentes afin qu’elles puissent déterminer le problème et les outils appropriés afin de pouvoir apporter des modifications de configuration pour répondre à la demande d’accès. Par exemple, un utilisateur peut suivre le processus ci-dessous pour accéder à un fichier auquel il n'a pas accès actuellement :  
  
-   L’utilisateur tente de lire un fichier dans le dossier partagé \\ \ financeshares, mais le serveur affiche un message indiquant que l’accès est refusé.  
  
-    Windows Server 2012 affiche les informations d’assistance en cas d’accès refusé à l’utilisateur avec une option pour demander de l’aide.  
  
-   Si l'utilisateur demande l'accès à la ressource, le serveur envoie un message électronique avec les informations de demande d'accès au propriétaire du dossier.  
  
Vous trouverez des informations sur la planification de la configuration de l’assistance en cas d’accès refusé dans [Plan for Access-Denied Assistance](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1).  
  
Vous trouverez des instructions sur la configuration de l’assistance en refus d’accès dans les [étapes &#40;&#41;de démonstration déployer l’assistance en refus d’accès](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Ce scénario fait partie du scénario de contrôle d'accès dynamique. Pour plus d'informations sur le contrôle d'accès dynamique, voir :  
  
-   [Contrôle d’accès dynamique : Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>Cas pratiques  
L’assistance en refus d’accès dans Windows Server 2012 contribue aux Access Control dynamiques en donnant aux utilisateurs la possibilité de demander l’accès à des fichiers et des dossiers partagés directement à partir d’un message de refus d’accès.  
  
## <a name="BKMK_NEW"></a>Fonctionnalités incluses dans ce scénario  
Le tableau ci-dessous répertorie les fonctionnalités incluses dans ce scénario et détaille la manière dont elles prennent en charge ce dernier.  
  
|Fonctionnalité|Prise en charge de ce scénario|  
|-----------|---------------------------------|  
|[Vue d’ensemble du serveur de fichiers Gestionnaire des ressources](https://technet.microsoft.com/library/hh831701.aspx)|L'assistance en cas d'accès refusé peut être configurée à l'aide de la console Gestionnaire de ressources du serveur de fichiers sur le serveur de fichiers.|  
|[Vue d’ensemble des services de stockage et de fichiers](https://technet.microsoft.com/library/hh831487.aspx)|Le Gestionnaire de ressources du serveur de fichiers est un service de rôle Services de fichiers et de stockage. Il est composé d'un ensemble de fonctionnalités pouvant servir à administrer les serveurs de fichiers sur votre réseau.|  
  


