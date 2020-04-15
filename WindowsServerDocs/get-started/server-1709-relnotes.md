---
title: Notes de publication - Points importants sur Windows Server, version 1709
description: Résume les problèmes critiques nécessitant une solution de contournement pour éviter une panne, un blocage, un échec d’installation ou une perte de données.
ms.prod: windows-server
ms.date: 04/23/2018
ms.technology: server-general
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: fea4b259986d1ca6e2f992168f7b0c2e1a177916
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826102"
---
# <a name="release-notes-important-issues-in-windows-server-version-1709"></a>Notes de publication : Problèmes importants dans Windows Server, version 1709

>S'applique à : Canal semi-annuel Windows Server

Ces notes de publication résument les problèmes les plus critiques du système d’exploitation Windows Server&reg; et expliquent, le cas échéant, comment les éviter ou les résoudre. Pour plus d’informations sur les modifications de l’interface, les nouvelles fonctionnalités et les correctifs de cette version, consultez [Nouveautés de Windows Server, version 1709](whats-new-in-windows-server-1709.md) et les annonces publiées par les équipes en charge des différentes fonctionnalités. Sauf mention contraire, chaque problème signalé s’applique à toutes les éditions et options d’installation de Windows Server 2016.  

Ce document est continuellement mis à jour. Les problèmes critiques nécessitant une solution de contournement sont ajoutés dès qu’ils sont identifiés, tout comme les solutions de contournement et les correctifs.  
  
## <a name="storage-spaces-direct"></a>Espaces de stockage directs
[comment]: # (ID : inconnu ; Demandeur : stevenek ; État : terminé)  
Les espaces de stockage direct ne sont pas inclus dans Windows Server, version 1709. Si vous appelez *Enable-ClusterStorageSpacesDirect* ou son alias *Enable-ClusterS2D* sur un serveur exécutant Windows Server, version 1709, vous recevez le message d’erreur « L’opération demandée n’est pas prise en charge ».

Par ailleurs, l’introduction de serveurs exécutant Windows Server, version 1709 dans un déploiement d’espaces de stockage direct Windows Server 2016 n’est pas prise en charge.

Le modèle de publication de Windows Server offre une nouvelle option pour s’aligner avec les modèles comparables de publication et de maintenance de [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview) et d’[Office 365 ProPlus](https://support.office.com/article/Overview-of-the-upcoming-changes-to-Office-365-ProPlus-update-management-78b33779-9356-4cdf-9d2c-08350ef05cca?ui=en-US&rs=en-US&ad=US). Les versions du canal semi-annuel offriront de nouvelles fonctionnalités aux clients qui veulent adopter une cadence rapide et de nouvelles versions seront publiées deux fois par an, au printemps et en automne.

Le canal semi-annuel de Windows Server est axé sur les conteneurs et les scénarios d’application bénéficiant d’une innovation plus rapide. Pour plus d’informations, consultez ce [blog](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update). Les clients qui recherchent des rôles d’infrastructure, tels que les espaces de stockage direct, doivent utiliser les versions de canal de maintenance à long terme, comme Windows Server 2016 (disponible dès maintenant) et [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (disponible dans le courant de l’année). Nous nous engageons à créer la plateforme la plus performante possible pour l’infrastructure hyperconvergée, et nous continuons à développer de nouvelles fonctionnalités et à améliorer les fonctionnalités existantes en fonction de vos commentaires. 

Les espaces de stockage direct ont été introduits dans Windows Server 2016 et constituent la base de notre plateforme hyperconvergée. Nous sommes ravis de l’adoption réussie de la plateforme hyperconvergée Microsoft et nous restons dévoués à nos clients.

Nous avons tenu compte de vos commentaires et nous mettons tout en œuvre pour livrer les [prochaines innovations](https://blogs.technet.microsoft.com/windowsserver/2017/09/07/sneak-peek-2-windows-server-version-1709-hyper-converged-infrastructure/) de notre plateforme hyperconvergée. Ces fonctionnalités sont disponibles dès aujourd’hui dans les builds [Windows Insiders](https://insider.windows.com/for-business/), et nous vous invitons à les tester et à partager vos commentaires. Pour les clients recherchant une solution hyperconvergée validée, nous recommandons le programme [Windows Server Software Defined](https://microsoft.com/wssd).
