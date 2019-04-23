---
title: La mise à niveau de votre hôte de Session Bureau à distance vers Windows Server 2016
description: Cet article décrit comment mettre à niveau vos déploiements de Services Bureau à distance existants vers Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c9b98b8-4eca-4a39-b10b-2bac729f7f44
author: spatnaik
manager: scottman
ms.openlocfilehash: 0cf5af29d610ba64d045e10241fd39b01d3f7024
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856060"
---
# <a name="upgrading-your-remote-desktop-session-host-to-windows-server-2016"></a>La mise à niveau de votre hôte de Session Bureau à distance vers Windows Server 2016

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

> [!IMPORTANT]
> Toutes les applications doivent être désinstallées avant la mise à niveau et réinstallées après la mise à niveau pour éviter tout problème de compatibilité d’application qui peut augmenter en raison de la mise à niveau.

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Prise en charge des mises à niveau du système d’exploitation avec le rôle Services Bureau à distance est installé
Mises à niveau vers Windows Server 2016 sont pris en charge uniquement à partir de Windows Server 2012 R2 et Windows Server 2016 TP5.

## <a name="upgrading-a-rds-session-based-collection"></a>La mise à niveau d’une collection basée sur session des services Bureau à distance
Afin de réduire le temps d’arrêt au minimum, il est préférable de suivre les étapes ci-dessous lors de la mise à niveau d’une collection basée sur session des services Bureau à distance :

1. Identifier les serveurs de mise à niveau, par exemple, la moitié des serveurs dans la collection.
2. Empêcher les nouvelles connexions à ces serveurs en définissant **autoriser de nouvelles connexions** sur false.
3. Fermez toutes les sessions sur ces serveurs. 
4. Supprimer ces serveurs de la collection.
5. Mettre à niveau les serveurs vers Windows Server 2016.
6. Définissez **autoriser de nouvelles connexions** sur « false » sur les serveurs restants dans la collection.
7. Ajoutez les serveurs mis à niveau vers leurs collections correspondantes.
8. Supprimer le jeu restant de serveurs de mise à niveau à partir de la collection.
9. Définissez **autoriser de nouvelles connexions** sur « true » sur les serveurs mis à niveau dans la collection.
10. Désormais, mettre à niveau les serveurs restants dans le déploiement en suivant les étapes 3 à 9 ci-dessus.

## <a name="upgrading-a-standalone-rd-session-host-server"></a>La mise à niveau un serveur hôte de Session Bureau à distance d’autonome
Un serveur hôte de Session Bureau à distance d’autonome peut être mis à niveau à tout moment.