---
title: Client Bureau à distance - configuration prise en charge
description: Découvrez les PC que vous pouvez accéder à l’aide de clients Bureau à distance
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884690"
---
# <a name="remote-desktop-client---supported-configuration"></a>Client Bureau à distance - configuration prise en charge

## <a name="supported-pcs"></a>PC pris en charge
Vous pouvez vous connecter aux ordinateurs qui exécutent les systèmes d’exploitation Windows suivants :
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
- Windows Server 2016
- Windows Multipoint Server 2011
- Windows Multipoint Server 2012
- Windows Small Business Server 2008
- Windows Small Business Server 2011

Les ordinateurs suivants peuvent exécuter la passerelle des services Bureau à distance :

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Small Business Server 2011

Les systèmes d’exploitation suivants peut être utilisé en tant que serveurs d’accès Web de bureau à distance ou de RemoteApp :
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

## <a name="unsupported-windows-versions-and-editions"></a>Éditions et Versions de Windows non pris en charge

Le client Bureau à distance se connectera pas à ces Versions de Windows et les éditions :

- Windows 7 Édition Starter
- Accueil de Windows 7
- Accueil de Windows 8
- Accueil de Windows 8.1
- Windows 10 Famille

Si vous souhaitez accéder aux ordinateurs qui ont une de ces versions de Windows installées, nous vous recommandons de que vous mettre à niveau vers une version de Windows qui prend en charge le protocole RDP.

## <a name="rd-gateway-messaging-is-not-supported"></a>Messagerie de passerelle Bureau à distance n’est pas pris en charge.
Client Bureau à distance ne prend pas en charge la messagerie de passerelle Bureau à distance. Vérifiez que le bureau ressource stratégie d’accès distant (RD RAP) pour votre serveur de passerelle Bureau à distance ne spécifie pas **autoriser uniquement les ordinateurs avec prise en charge pour la messagerie de passerelle Bureau à distance** ou vous ne serez pas en mesure de se connecter.