---
title: Configurer des connexions partagées pour tous les utilisateurs de la passerelle du centre d’administration Windows
description: Découvrez comment les administrateurs peuvent configurer la passerelle du centre d’administration Windows (Project Honolulu) une fois pour permettre à tous les utilisateurs de partager une seule liste de connexions.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 46075154a225590376458881768ae86f74a572ad
ms.sourcegitcommit: 286e3181ebd2cb9d7dc7fe651858a4e0d61d153f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68300725"
---
# <a name="configure-shared-connections-for-all-users-of-the-windows-admin-center-gateway"></a>Configurer des connexions partagées pour tous les utilisateurs de la passerelle du centre d’administration Windows

> S'applique à : Centre d’administration Windows-version préliminaire, Centre d’administration Windows

Avec la possibilité de configurer des connexions partagées, les administrateurs de passerelle peuvent configurer la liste des connexions une seule fois pour tous les utilisateurs d’une passerelle du centre d’administration Windows donnée. 

À partir de l’onglet **connexions partagées** des paramètres de la passerelle du centre d’administration Windows, les administrateurs de passerelle peuvent ajouter des serveurs, des clusters et des connexions de PC comme vous le feriez à partir de la page toutes les connexions, y compris la possibilité de baliser les connexions. Toutes les connexions et toutes les balises ajoutées dans la liste des connexions partagées s’affichent pour tous les utilisateurs de cette passerelle du centre d’administration Windows, à partir de la page toutes les connexions.
    ![](../media/shared-cnxns-1.png)

Quand un utilisateur du centre d’administration Windows accède à la page «toutes les connexions» après la configuration des connexions partagées, il voit ses connexions regroupées en deux sections: Connexions personnelles et partagées. Le groupe personnel est la liste des connexions d’un utilisateur spécifique et il est conservé dans les sessions du navigateur de cet utilisateur. Le groupe connexions partagées est le même pour tous les utilisateurs et ne peut pas être modifié à partir de la page toutes les connexions.
![](../media/shared-cnxns-2.png)