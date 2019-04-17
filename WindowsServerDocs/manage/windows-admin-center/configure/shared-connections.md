---
title: Configurer les connexions partagées pour tous les utilisateurs de la passerelle Windows Admin Center
description: Découvrez comment les administrateurs peuvent configurer leur passerelle Windows Admin Center (projet Honolulu) qu’une seule fois pour permettre à tous les utilisateurs de partager une seule liste de connexions.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1bdafa0d23934666fc0f6985249502e4960dc38e
ms.sourcegitcommit: 74d9eee13386a039186a70cdc97dc0b82e74bbf4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2019
ms.locfileid: "9271016"
---
# Configurer les connexions partagées pour tous les utilisateurs de la passerelle Windows Admin Center

> S’applique à: Windows Admin Center Preview

Avec la possibilité de configurer les connexions partagées, les administrateurs de passerelle peuvent configurer la liste des connexions une fois pour tous les utilisateurs d’une passerelle Windows Admin Center donnée. 

Sous l’onglet **Connexions partagées** de passerelle Windows Admin Center paramètres, les administrateurs de passerelle peuvent ajouter les serveurs, les clusters et connexions au PC comme vous le feriez à partir de la page connexions toutes les, notamment la possibilité de connexions de balise. Les connexions et balises ajoutées dans la liste des connexions partagées apparaîtront pour tous les utilisateurs de cette passerelle Windows Admin Center, à partir de leur toutes les pages de connexions.
    ![](../media/shared-cnxns-1.png)

Lorsque n’importe quel utilisateur Windows Admin Center accède à la page «Toutes les connexions» une fois que les connexions partagées ont été configurées, ils verront leurs connexions regroupées en deux sections: connexions personnels et partagés. Le groupe personnel est la liste de connexion d’un utilisateur spécifique et est conservé dans toutes les sessions de navigateur de cet utilisateur. Le groupe de connexions Shared est identique sur tous les utilisateurs et ne peut pas être modifié à partir de la page de toutes les connexions.
![](../media/shared-cnxns-2.png)