---
title: Configurer des connexions partagées pour tous les utilisateurs de la passerelle Windows Admin Center
description: Découvrez comment les administrateurs peuvent configurer en une fois la passerelle Windows Admin Center (projet Honolulu) pour permettre à tous les utilisateurs de partager une liste de connexions.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 943830a2743f7cfd3192a474eb36d57f734d3d34
ms.sourcegitcommit: 20d07170c7f3094c2fb4455f54b13ec4b102f2d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81269306"
---
# <a name="configure-shared-connections-for-all-users-of-the-windows-admin-center-gateway"></a>Configurer des connexions partagées pour tous les utilisateurs de la passerelle Windows Admin Center

> S'applique à : Windows Admin Center Preview, Windows Admin Center

Pour les administrateurs de passerelle, la possibilité de configurer des connexions partagées signifie qu’ils peuvent configurer en une fois une liste de connexions pour tous les utilisateurs d’une passerelle Windows Admin Center donnée. Cette fonctionnalité est disponible uniquement dans le mode service de Windows Admin Center.

Dans les paramètres de la passerelle Windows Admin Center, l’onglet **Connexions partagées** permet aux administrateurs de passerelle d’ajouter des serveurs, des clusters et des connexions PC comme vous le feriez dans la page Toutes les connexions. Ils peuvent également étiqueter des connexions. Toutes les connexions et les étiquettes ajoutées dans la liste Connexions partagées apparaissent pour l’ensemble des utilisateurs de cette passerelle Windows Admin Center dans leur page Toutes les connexions.
    ![](../media/shared-cnxns-1.png)

Quand un utilisateur Windows Admin Center accède à la page « Toutes les connexions » après la configuration de connexions partagées, il voit ses connexions regroupées en deux sections : personnelles et partagées. Le groupe de connexions personnelles correspond à la liste de connexions d’un utilisateur spécifique. Il est conservé tout au long des sessions de cet utilisateur dans le navigateur. Le groupe de connexions partagées est le même pour tous les utilisateurs et ne peut pas être modifié dans la page Toutes les connexions.
![](../media/shared-cnxns-2.png)