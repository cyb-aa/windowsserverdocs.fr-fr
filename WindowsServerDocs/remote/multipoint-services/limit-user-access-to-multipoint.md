---
title: Limiter l’accès utilisateur au serveur
description: Découvrez comment accorder ou refuser l’accès à MultiPoint Services pour les utilisateurs et groupes
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cabd4f1-a764-4be6-bc6e-0a5f5566390c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 1466e19152847a6c7d88f77162c50ec73a5a7d27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830810"
---
# <a name="limit-users-access-to-the-multipoint-server"></a>Limiter l’accès des utilisateurs au serveur Multipoint
Si vous joindre le serveur MultiPoint à un domaine Active Directory ou que vous utilisez des comptes d’utilisateurs locaux, tous les utilisateurs ont accès au serveur MultiPoint par défaut. Avant d’autoriser les utilisateurs à ouvrir une session sur les stations dans votre environnement MultiPoint Services, vous devez restreindre l’accès au serveur.  
  
N’importe quel utilisateur dans le groupe utilisateurs du Bureau à distance pouvez vous connecter au serveur MultiPoint. Par défaut, le groupe d’utilisateurs tout le monde est un membre du groupe utilisateurs du Bureau à distance, et par conséquent, chaque utilisateur local et un utilisateur de domaine peuvent se connecter au serveur MultiPoint. Pour restreindre l’accès à MultiPoint Server, supprimez tout le monde utilisateur groupe du groupe utilisateurs du Bureau à distance, puis ajouter des utilisateurs ou groupes spécifiques au groupe utilisateurs du Bureau à distance.  
  
## <a name="add-or-remove-users-or-groups-to-the-remote-desktop-users-group"></a>Ajouter ou supprimer des utilisateurs ou des groupes au groupe utilisateurs du Bureau à distance  
  
1.  À partir de la **Démarrer** écran, ouvrez **gestion de l’ordinateur**.  
  
2.  Dans l’arborescence de la console, sous **utilisateurs et groupes locaux**, cliquez sur **groupes**.  
  
3.  Double-cliquez sur **Remote Desktop Users**et suivez les instructions pour ajouter ou supprimer des utilisateurs.  
  
    -   Pour restreindre l’accès général sur le serveur, supprimez le groupe tout le monde.  
  
    -   Pour donner aux utilisateurs d’accéder à votre serveur MultiPoint stations de, ajoutez chaque compte local ou chaque compte d’utilisateur ou un groupe de domaine au groupe utilisateurs du Bureau à distance.  