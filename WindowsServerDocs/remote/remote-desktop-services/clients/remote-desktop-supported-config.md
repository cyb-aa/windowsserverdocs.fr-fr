---
title: Client Bureau à distance - configuration prise en charge
description: Découvrez quels sont les PC auxquels vous pouvez accéder avec les clients Bureau à distance
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bb932dad-6f74-484f-8f7b-dd957b615d44
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d38008b6387385917ad21ce7e169b8ff3f4d18ba
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63748963"
---
# <a name="remote-desktop-client---supported-configuration"></a>Client Bureau à distance - configuration prise en charge

## <a name="supported-pcs"></a>PC pris en charge
Vous pouvez vous connecter aux ordinateurs exécutant les systèmes d’exploitation Windows suivants :
- Windows 10 Professionnel
- Windows 10 Entreprise
- Windows 8 Entreprise
- Windows 8 Professionnel
- Windows 7 Professionnel
- Windows 7 Entreprise
- Windows 7 Édition Intégrale
- Windows 7 Édition Intégrale
- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Multipoint Server 2011
- Windows Multipoint Server 2012
- Windows Small Business Server 2008
- Windows Small Business Server 2011

Les ordinateurs suivants peuvent exécuter la passerelle des services Bureau à distance :

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Small Business Server 2011

Les systèmes d’exploitation suivants peuvent servir de serveurs RD Web Access ou RemoteApp :
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

## <a name="unsupported-windows-versions-and-editions"></a>Éditions et versions de Windows non prises en charge

Le client Bureau à distance ne se connectera pas aux versions et éditions de Windows suivantes :

- Windows 7 Édition Starter
- Windows 7 Famille
- Windows 8 FamilleHome
- Windows 8.1 Famille
- Windows 10 Famille

Si vous souhaitez accéder à des ordinateurs pourvus de l’une de ces versions de Windows, nous vous recommandons de la mettre à jour vers une version prenant en charge le protocole RDP.

## <a name="rd-gateway-messaging-is-not-supported"></a>La messagerie de la passerelle des services Bureau à distance n’est pas prise en charge
Le client Bureau à distance ne prend pas en charge la messagerie de la passerelle des services Bureau à distance. Vérifiez que la stratégie d’accès de ressources Bureau à distance (RD RAP) pour votre serveur de passerelle Bureau à distance ne spécifie pas **Autoriser uniquement les ordinateurs prenant en charge la messagerie de la passerelle des services Bureau à distance**, ou vous ne serez pas en mesure de vous connecter.