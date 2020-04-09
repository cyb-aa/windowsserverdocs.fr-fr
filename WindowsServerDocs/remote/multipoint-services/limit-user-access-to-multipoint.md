---
title: Limiter l’accès utilisateur au serveur
description: Découvrez comment accorder ou refuser l’accès à MultiPoint services pour les utilisateurs et les groupes
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 4cabd4f1-a764-4be6-bc6e-0a5f5566390c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: ddc852eea3ed17cd354cc87c79d82066f989bd5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820252"
---
# <a name="limit-users-access-to-the-multipoint-server"></a>Limiter l’accès des utilisateurs au serveur multipoint
Si vous joignez MultiPoint Server à un domaine Active Directory ou que vous utilisez des comptes d’utilisateurs locaux, tous les utilisateurs ont accès à MultiPoint Server par défaut. Avant de permettre aux utilisateurs de se connecter aux stations dans votre environnement MultiPoint services, vous devez restreindre l’accès au serveur.  
  
Tout utilisateur du groupe Bureau à distance utilisateurs peut se connecter à MultiPoint Server. Par défaut, le groupe d’utilisateurs tout le monde est membre du groupe d’utilisateurs Bureau à distance, et par conséquent, chaque utilisateur local et utilisateur de domaine peut se connecter au serveur MultiPoint. Pour restreindre l’accès à MultiPoint Server, supprimez le groupe d’utilisateurs tout le monde du Bureau à distance utilisateurs, puis ajoutez des utilisateurs ou des groupes spécifiques au groupe d’utilisateurs Bureau à distance.  
  
## <a name="add-or-remove-users-or-groups-to-the-remote-desktop-users-group"></a>Ajouter ou supprimer des utilisateurs ou des groupes dans le groupe utilisateurs Bureau à distance  
  
1.  Dans l’écran d' **Accueil** , ouvrez gestion de l' **ordinateur**.  
  
2.  Dans l’arborescence de la console, sous **utilisateurs et groupes locaux**, cliquez sur **groupes**.  
  
3.  Double-cliquez sur **Bureau à distance utilisateurs**et suivez les instructions pour ajouter ou supprimer des utilisateurs.  
  
    -   Pour restreindre l’accès général au serveur, supprimez le groupe tout le monde.  
  
    -   Pour permettre aux utilisateurs de votre serveur MultiPoint d’accéder aux stations, ajoutez chaque compte local ou chaque compte d’utilisateur ou de groupe de domaine au groupe d’utilisateurs Bureau à distance.  