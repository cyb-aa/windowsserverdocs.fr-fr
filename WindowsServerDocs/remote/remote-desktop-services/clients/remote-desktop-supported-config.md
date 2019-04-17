---
title: Client de bureau à distance - configuration prises en charge
description: Découvrez les ordinateurs que vous pouvez accéder à l’aide de clients de bureau à distance
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
ms.sourcegitcommit: 96e968bbe8dc50ebb1535ae1c8ce92fa73c83171
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "1978046"
---
# <a name="remote-desktop-client---supported-configuration"></a>Client de bureau à distance - configuration prises en charge

## <a name="supported-pcs"></a>PC pris en charge
Vous pouvez vous connecter à des ordinateurs qui exécutent des systèmes d’exploitation Windows suivants:
- Windows10 Professionnel
- Windows10 Entreprise
- Windows8Entreprise
- Windows8Professionnel
- Windows7Professionnel
- Windows7Entreprise
- Windows7 Édition Intégrale
- Windows7 Édition Intégrale
- WindowsServer2008
- Windows Server 2008 R2
- Windows Server2012
- WindowsServer2012R2
- Windows Server2016
- Windows Multipoint Server 2011
- Windows Multipoint Server 2012
- Windows Small Business Server 2008
- Windows Small Business Server2011

Les ordinateurs suivants peuvent exécuter la passerelle des services Bureau à distance:

- WindowsServer2008
- Windows Server 2008 R2
- Windows Server2012
- WindowsServer2012R2
- Windows Server2016
- Windows Small Business Server2011

Les systèmes d’exploitation suivants peuvent servir de serveurs d’accès Web de bureau à distance ou RemoteApp:
- Windows Server 2008 R2
- Windows Server2012
- WindowsServer2012R2
- Windows Server2016

## <a name="unsupported-windows-versions-and-editions"></a>Éditions et Versions de Windows non pris en charge

Le client Bureau à distance se connectera pas à ces éditions et Versions de Windows:

- Windows7ÉditionStarter
- Accueil Windows 7
- Accueil Windows 8
- Accueil Windows 8.1
- Windows10 Famille

Si vous souhaitez accéder aux ordinateurs sur lesquels une de ces versions de Windows installées, nous vous recommandons de que mise à niveau vers une version Windows qui prend en charge RDP.

## <a name="rd-gateway-messaging-is-not-supported"></a>Messagerie de la passerelle des services Bureau à distance n’est pas pris en charge.
Client Bureau à distance ne gère pas la messagerie de la passerelle des services Bureau à distance. Vérifiez que l’à distance Bureau ressource accès stratégie Bureau à distance pour votre serveur de passerelle des services Bureau à distance ne spécifie pas **Autoriser uniquement les ordinateurs avec prise en charge pour la messagerie de passerelle Bureau à distance** ou que vous ne serez pas en mesure de se connecter.