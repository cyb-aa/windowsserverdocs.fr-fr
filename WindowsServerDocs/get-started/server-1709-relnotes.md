---
title: Notes de publication--Points importants sur WindowsServer, version1709
description: Résume les problèmes critiques nécessitant une solution de contournement pour éviter une panne, un blocage, un échec d’installation ou une perte de données.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 04/23/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 4eebc498289a81c7f27fcf4b84d81ae13bc38e4f
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133855"
---
# Notes de publication: points importants sur WindowsServer, version1709

>S’applique à: Windows Server (canal semi-annuel)

Ces notes de publication résument les problèmes les plus critiques du système d’exploitation Windows Server&reg; et expliquent, le cas échéant, comment les éviter ou les résoudre. Pour plus d’informations sur les modifications de l’interface, les nouvelles fonctionnalités et les correctifs de cette version, consultez [Nouveautés de WindowsServer, version1709](whats-new-in-windows-server-1709.md) et les annonces publiées par les équipes en charge des différentes fonctionnalités. Sauf mention contraire, chaque problème signalé s’applique à toutes les éditions et options d’installation de WindowsServer2016.  

Ce document est continuellement mis à jour. Les problèmes critiques nécessitant une solution de contournement sont ajoutés dès qu’ils sont identifiés, tout comme les solutions de contournement et les correctifs.  
  
## Espaces de stockage direct
[comment]: # (ID: inconnu; Auteur de la soumission: stevenek; état: validé)  
Les espaces de stockage direct ne sont pas inclus dans WindowsServer, version1709. Si vous appelez *Enable-ClusterStorageSpacesDirect* ou son alias *Enable-ClusterS2D* sur un serveur exécutant WindowsServer, version1709, vous recevez le message d’erreur «L’opération demandée n’est pas prise en charge».

Par ailleurs, l’introduction de serveurs exécutant WindowsServer, version1709dans un déploiement d’espaces de stockage direct WindowsServer2016 n’est pas prise en charge.

Le modèle de publication de Windows Server offre une nouvelle option pour s'aligner avec les modèles comparables de publication et de maintenance de [Windows10](https://docs.microsoft.com/windows/deployment/update/waas-overview) et d'[Office365 ProPlus](https://support.office.com/article/Overview-of-the-upcoming-changes-to-Office-365-ProPlus-update-management-78b33779-9356-4cdf-9d2c-08350ef05cca?ui=en-US&rs=en-US&ad=US). Les versions du canal semi-annuel offriront de nouvelles fonctionnalités aux clients qui veulent adopter une cadence rapide et de nouvelles versions seront publiées deux fois par an, au printemps et en automne.

Le canal semi-annuel de Windows Server se concentre sur les conteneurs et les scénarios d’application bénéficiant d’une innovation plus rapide, consultez le [blog](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update) pour plus d’informations. Les clients recherchant des rôles d’infrastructure, telles que des espaces de stockage Direct, doivent utiliser des versions du canal de maintenance à long terme telles que Windows Server 2016 (désormais disponible) et [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (sera disponible plus tard cette année). Nous nous sommes engagés à la création de la meilleure plate-forme d’une infrastructure hyperconvergée, et nous continuons à développer de nouvelles fonctionnalités et améliorer existants en fonction de vos commentaires. 

Les espaces de stockage direct ont été introduits dans WindowsServer2016 et constituent la base de notre plateforme hyperconvergée. Nous avons été ravis par l’adoption positive de la plateforme hyperconvergée Microsoft et nous restons engagés auprès de nos clients.

Nous avez à l’écoute de vos commentaires et que vous travaillez pour offrir la [ensuite définir des innovations](https://blogs.technet.microsoft.com/windowsserver/2017/09/07/sneak-peek-2-windows-server-version-1709-hyper-converged-infrastructure/) pour notre plateforme hyperconvergée. Ces fonctionnalités sont disponibles dès aujourd'hui dans les builds [Windows Insiders](https://insider.windows.com/for-business/) , et nous vous invitons à les tester et partager vos commentaires. Pour les clients recherchant une solution hyperconvergée validée, nous recommandons le programme [WindowsServer Software Defined](http://microsoft.com/wssd).
