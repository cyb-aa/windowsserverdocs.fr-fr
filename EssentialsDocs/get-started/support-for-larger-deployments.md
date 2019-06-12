---
title: Prise en charge des déploiements plus importants
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07d0c4c6-3e92-4969-82b8-105e46ab8d97
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a99698519524c3b5050dc534d61921560522528c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433871"
---
# <a name="support-for-larger-deployments"></a>Prise en charge des déploiements plus importants

>S'applique à : WindowsServer2016 Essentials

> [!IMPORTANT]  
> Les fonctionnalités décrites dans cette rubrique fonctionnent uniquement sur Windows Server 2016 avec le rôle expérience Essentials activé et non avec la référence SKU de Windows Server 2016 Essentials.


Windows Server Essentials prend désormais en charge les déploiements plus importants avec :

- plusieurs domaines
- plusieurs contrôleurs de domaine
- possibilité de spécifier un contrôleur de domaine désigné
- prise en charge jusqu'à 500 utilisateurs et 500 appareils

## <a name="support-for-multiple-domains"></a>Prise en charge de domaines multiples

Serveur de Windows 2012 R2 Essentials prend en charge qu’un seul domaine par serveur, ce qui est requis, et le serveur Essentials doit être la racine de la forêt. Pendant un domaine et une forêt sont toujours nécessaires, le rôle expérience Windows Server 2016 Essentials peut désormais être déployé sur Windows Server 2016 Standard ou centre de données pour prendre en charge de plusieurs domaines.

## <a name="support-for-multiple-domain-controllers"></a>Prise en charge de plusieurs contrôleurs de domaine

 Windows Server Essentials 2012 R2 bloque tous les services qui tirent parti d’Azure Active Directory, telles qu’Office 365, où plusieurs contrôleurs de domaine est déployé. La raison est que la synchronisation de compte et mot de passe entre les contrôleurs de domaine locaux et Azure Active Directory peut entraîner aux informations d’identification de compte avec les mots de passe qui ne sont pas synchronisés. Cette limitation a été supprimée dans Windows Server 2016 Essentials.

## <a name="ability-to-specify-a-designated-domain-controller"></a>possibilité de spécifier un contrôleur de domaine désigné

Vous pouvez maintenant choisir un contrôleur de domaine désigné qui sera améliorer les temps de récupération des objets de domaine Active Directory, ainsi que coordonner la synchronisation de la modification du compte entre autres contrôleurs de domaine dans le domaine.

Désigné du contrôleur de domaine par défaut sera le même serveur qui exécute le rôle serveur expérience Windows Server Essentials. Si ce serveur est un serveur membre, ce qui signifie qu’il n’est pas un contrôleur de domaine, puis la valeur par défaut, le contrôleur de domaine désignée est automatiquement déterminé en fonction de test quel contrôleur de domaine dans le domaine a la latence réseau la plus faible vers le serveur exécutant le Rôle de serveur d’expérience Windows Server. Si vous souhaitez modifier manuellement le serveur qui est le contrôleur de domaine désigné, vous pouvez effectuer dans **paramètres** dans le **tableau de bord Windows Server Essentials** comme indiqué ci-dessous.

![Une capture d’écran montrant les paramètres du Panneau de commande au premier plan et le tableau de bord Windows Server Essentials en arrière-plan. La page de contrôleur de domaine désigné du Panneau de paramètres est actuellement sélectionnée.](media/larger-deployments-1.PNG)

## <a name="support-for-500-users-and-500-devices"></a>Prise en charge de 500 utilisateurs et 500 appareils
-------------------------------------

Le nombre maximal d’utilisateurs pris en charge et d’appareils dans Windows Server 2012 R2 Essentials est 25 et 50, respectivement. Avec l’introduction du rôle serveur expérience Windows Server Essentials, cette limite a été augmentée à 100 utilisateurs et 200 appareils.

Windows Server 2016 Essentials prend en charge 500 utilisateurs et 500 appareils. Ce qui en fait possible est une mise à jour pour l’infrastructure du fournisseur et le contrôle de liste d’objets afin de mettre en cache et restituer rapidement les grandes listes d’objets utilisateur et appareil. En outre, une fonctionnalité de recherche et de filtre a été ajoutée à trouver rapidement l’utilisateur ou l’appareil que peuvent être recherchées (voir Figure 5-2). Vous trouverez la fonctionnalité de recherche et de filtre dans le **tableau de bord Windows Server Essentials**, **accès Web à distance**et le magasin de Windows et Windows Phone **mon serveur** applications.

![Une capture d’écran illustrant l’utilisation de la fonctionnalité de recherche du tableau de bord Windows Server Essentials pour rechercher la chaîne « d5c ». Les résultats de la recherche incluent les deux fichiers et les dossiers et les deux utilisateurs.](media/larger-deployments-2.PNG)

Une capture d’écran illustrant l’utilisation de la fonctionnalité de recherche du tableau de bord Windows Server Essentials pour rechercher la chaîne « d5c ». Les résultats de la recherche incluent les deux fichiers et les dossiers et les deux utilisateurs.

> [!NOTE]  
> Bien que la limite d’utilisateurs et d’appareils pris en charge a augmenté pour le rôle de serveur Windows Server Essentials, la limite prise en charge pour le reste de sauvegarde de client à 75.

<a name="see-also"></a>Voir aussi
--------
[Prise en main de Windows Server Essentials](get-started.md)