---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: Scénario Access-Denied Assistance
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d6b8d8a094aa86fd5be78d2eae13bae50df3fd79
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829090"
---
# <a name="scenario-access-denied-assistance"></a>Scénario : assistance en cas d'accès refusé

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les utilisateurs reçoivent un message de refus d'accès quand ils essaient d'accéder à des dossiers et fichiers partagés sur un serveur de fichiers pour lequel ils n'ont aucune autorisation. Souvent, les administrateurs ne disposent pas du contexte approprié, ce qui rend difficile la résolution du problème d'accès.  
  
## <a name="scenario-description"></a>Description du scénario  
Accès refusé assistance est une nouvelle fonctionnalité dans Windows Server 2012, qui fournit les méthodes suivantes pour résoudre les problèmes liés à l’accès aux fichiers et dossiers :  
  
-   **Auto-assistance.** Si l'utilisateur peut identifier le problème et y remédier pour pouvoir obtenir l'accès demandé, l'impact professionnel est faible et aucune exception spéciale n'est nécessaire dans la stratégie d'accès centralisée. L'assistance en cas d'accès refusé fournit un message de refus d'accès personnalisable par les administrateurs du serveur de fichiers avec des informations spécifiques à leur organisation. Par exemple, un administrateur peut définir le message pour que les utilisateurs puissent demander l'accès à un propriétaire de données sans faire appel à l'administrateur du serveur de fichiers.  
  
-   **Assistance de la part du propriétaire des données.** Vous pouvez définir une liste de distribution pour les dossiers partagés et la configurer pour que le propriétaire du dossier reçoive une notification par courrier électronique quand un utilisateur a besoin d'y accéder. Si le propriétaire des données ignore comment aider l'utilisateur a obtenir l'accès nécessaire, il peut transférer ces informations à l'administrateur du serveur de fichiers.  
  
-   **Assistance de la part de l'administrateur du serveur de fichiers.** Ce type d'assistance est disponible quand l'utilisateur ne peut pas résoudre un problème et que le propriétaire des données ne peut pas l'aider.  Windows Server 2012 fournit une interface utilisateur où les administrateurs de serveur de fichiers peuvent afficher les autorisations effectives pour un utilisateur sur un fichier ou dossier afin qu’il soit plus facile à résoudre les problèmes d’accès.  
  
Accès refusé pour une assistance dans Windows Server 2012 fournit des administrateurs de serveur de fichiers les détails de l’accès approprié afin qu’ils puissent identifier le problème et les outils appropriés pour apporter les modifications de configuration pour répondre à la demande d’accès. Par exemple, un utilisateur peut suivre le processus ci-dessous pour accéder à un fichier auquel il n'a pas accès actuellement :  
  
-   L’utilisateur tente de lire un fichier dans le \\\financeshares un dossier partagé, mais le serveur affiche un message accès refusé.  
  
-    Windows Server 2012 affiche les informations de l’assistance en cas refusé l’accès à l’utilisateur avec une option pour demander une assistance.  
  
-   Si l'utilisateur demande l'accès à la ressource, le serveur envoie un message électronique avec les informations de demande d'accès au propriétaire du dossier.  
  
Vous trouverez des informations sur la planification de la configuration de l’assistance en cas d’accès refusé dans [Plan for Access-Denied Assistance](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1).  
  
Vous trouverez les étapes de configuration de l’assistance en cas d’accès refusé [déployer l’Assistance &#40;étapes de démonstration&#41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Ce scénario fait partie du scénario de contrôle d'accès dynamique. Pour plus d'informations sur le contrôle d'accès dynamique, voir :  
  
-   [Contrôle d’accès dynamique : Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>Cas pratiques  
Accès refusé pour une assistance dans Windows Server 2012 contribue au contrôle d’accès dynamique en permettant aux utilisateurs la possibilité de demander l’accès aux fichiers et dossiers partagés directement à partir d’un message accès refusé.  
  
## <a name="BKMK_NEW"></a>Fonctionnalités incluses dans ce scénario  
Le tableau ci-dessous répertorie les fonctionnalités incluses dans ce scénario et détaille la manière dont elles prennent en charge ce dernier.  
  
|Fonctionnalité|Prise en charge de ce scénario|  
|-----------|---------------------------------|  
|[Vue d’ensemble de la file Server Resource Manager](https://technet.microsoft.com/library/hh831701.aspx)|L'assistance en cas d'accès refusé peut être configurée à l'aide de la console Gestionnaire de ressources du serveur de fichiers sur le serveur de fichiers.|  
|[Présentation des Services de stockage et de fichier](https://technet.microsoft.com/library/hh831487.aspx)|Le Gestionnaire de ressources du serveur de fichiers est un service de rôle Services de fichiers et de stockage. Il est composé d'un ensemble de fonctionnalités pouvant servir à administrer les serveurs de fichiers sur votre réseau.|  
  


