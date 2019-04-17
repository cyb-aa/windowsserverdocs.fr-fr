---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: "Scénario Access-Denied Assistance"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d6b8d8a094aa86fd5be78d2eae13bae50df3fd79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-access-denied-assistance"></a>Scénario: Access-Denied Assistance

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Les utilisateurs recevront un message accès refusé lorsqu’ils essaient d’accéder aux fichiers partagés et des dossiers sur un serveur de fichiers pour lesquels ils n’ont pas d’autorisations. Souvent, les administrateurs n’ont pas le contexte approprié pour résoudre le problème d’accès, ce qui rend difficile résoudre le problème.  
  
## <a name="scenario-description"></a>Description du scénario  
Accès refusé assistance est une nouvelle fonctionnalité de Windows Server2012, qui fournit des méthodes suivantes pour résoudre les problèmes liés à l’accès aux fichiers et dossiers:  
  
-   **Assistance automatique.** Si un utilisateur peut déterminer le problème et corriger le problème pour qu’ils puissent obtenir l’accès demandé, l’impact professionnel est faible et aucune exception spéciale n’est nécessaires dans la stratégie d’accès centralisée. Assistance en cas d’accès refusé fournit un message de refus d’accès que les administrateurs de serveur de fichiers peuvent personnaliser avec des informations spécifiques à leur organisation. Par exemple, un administrateur peut définir le message afin que les utilisateurs peuvent demander l’accès à partir d’un propriétaire des données sans impliquer l’administrateur de serveur de fichiers.  
  
-   **Assistance par le propriétaire des données.** Vous pouvez définir une liste de distribution pour les dossiers partagés et configurez-le afin que le propriétaire du dossier reçoit une notification par courrier électronique lorsqu’un utilisateur doit accéder. Si le propriétaire des données ignore comment aider l’utilisateur à accéder, le propriétaire peut transférer ces informations à l’administrateur de serveur de fichiers.  
  
-   **Assistance par l’administrateur de serveur de fichiers.** Ce type d’assistance est disponible lorsque l’utilisateur ne peut pas résoudre un problème et ne peut aider au propriétaire des données.  Windows Server2012 fournit une interface utilisateur dans laquelle les administrateurs de serveur de fichiers peuvent afficher les autorisations effectives pour un utilisateur sur un fichier ou dossier afin qu’il soit plus facile de dépanner des problèmes d’accès.  
  
Accès refusé d’assistance dans Windows Server2012 fournit les administrateurs de serveur de fichiers les détails d’accès appropriés afin qu’ils puissent identifier le problème et les outils appropriés afin qu’ils peuvent apporter des modifications de configuration pour satisfaire la demande d’accès. Par exemple, un utilisateur peut suivre ce processus pour accéder à un fichier qu’ils n’ont actuellement pas accès à:  
  
-   L’utilisateur tente de lire un fichier dans le dossier partagé \\\financeshares, mais le serveur affiche un message d’accès refusé.  
  
-    Windows Server2012 affiche les informations de l’assistance d’accès refusé à l’utilisateur une option pour demander une assistance.  
  
-   Si l’utilisateur demande l’accès à la ressource, le serveur envoie un message électronique avec les informations de demande d’accès au propriétaire du dossier.  
  
Vous trouverez des informations de planification pour la configuration d’accès refusé une assistance [planifier denied Assistance](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1).  
  
Vous trouverez les étapes sur la configuration d’accès refusé une assistance [déployer l’Assistance et #40; étapes de démonstration et #41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Ce scénario fait partie du scénario de contrôle d’accès dynamique. Pour plus d’informations sur le contrôle d’accès dynamique, voir:  
  
-   [Contrôle d’accès dynamique: Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>Applications pratiques  
Accès refusé d’assistance dans Windows Server2012 contribue au contrôle d’accès dynamique en offrant aux utilisateurs la possibilité de demander l’accès aux fichiers et dossiers partagés directement à partir d’un message d’accès refusé.  
  
## <a name="BKMK_NEW"></a>Fonctionnalités incluses dans ce scénario  
Le tableau suivant répertorie les fonctionnalités qui font partie de ce scénario et décrit la prise en charge.  
  
|Fonctionnalité|Comment il prend en charge ce scénario|  
|-----------|---------------------------------|  
|[Vue d’ensemble du Gestionnaire de ressources du serveur de fichiers](https://technet.microsoft.com/library/hh831701.aspx)|Assistance en cas d’accès refusé peut être configuré à l’aide de la console du Gestionnaire de ressources de serveur de fichiers sur le serveur de fichiers.|  
|[Vue d’ensemble des Services de stockage et de fichier](https://technet.microsoft.com/library/hh831487.aspx)|Gestionnaire de ressources du serveur de fichiers est un service de rôle Services de fichiers et stockage, et il est composé d’un ensemble de fonctionnalités qui peuvent être utilisés pour administrer les serveurs de fichiers sur votre réseau.|  
  


