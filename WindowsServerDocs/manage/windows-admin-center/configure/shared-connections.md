---
title: Configurer des connexions partagées pour tous les utilisateurs de la passerelle du centre d’administration Windows
description: Découvrez comment les administrateurs peuvent configurer la passerelle du centre d’administration Windows (Project Honolulu) une fois pour permettre à tous les utilisateurs de partager une seule liste de connexions.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c632c178e91b92e0a80d8c72e8ea0ce3ce37b502
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357288"
---
# <a name="configure-shared-connections-for-all-users-of-the-windows-admin-center-gateway"></a>Configurer des connexions partagées pour tous les utilisateurs de la passerelle du centre d’administration Windows

> S'applique à : Centre d’administration Windows-version préliminaire, Centre d’administration Windows

Avec la possibilité de configurer des connexions partagées, les administrateurs de passerelle peuvent configurer la liste des connexions une seule fois pour tous les utilisateurs d’une passerelle du centre d’administration Windows donnée. 

À partir de l’onglet **connexions partagées** des paramètres de la passerelle du centre d’administration Windows, les administrateurs de passerelle peuvent ajouter des serveurs, des clusters et des connexions de PC comme vous le feriez à partir de la page toutes les connexions, y compris la possibilité de baliser les connexions. Toutes les connexions et toutes les balises ajoutées dans la liste des connexions partagées s’affichent pour tous les utilisateurs de cette passerelle du centre d’administration Windows, à partir de la page toutes les connexions.
    ![](../media/shared-cnxns-1.png)

Quand un utilisateur du centre d’administration Windows accède à la page «toutes les connexions» après la configuration des connexions partagées, il voit ses connexions regroupées en deux sections: Connexions personnelles et partagées. Le groupe personnel est la liste des connexions d’un utilisateur spécifique et il est conservé dans les sessions du navigateur de cet utilisateur. Le groupe connexions partagées est le même pour tous les utilisateurs et ne peut pas être modifié à partir de la page toutes les connexions.
![](../media/shared-cnxns-2.png)