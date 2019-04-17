---
title: "Prise en charge des déploiements plus importants"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.openlocfilehash: c60e5f73c88a225fbd1067992894f9d20da745ad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
#<a name="support-for-larger-deployments"></a>Prise en charge des déploiements plus importants

>S’applique à: Windows Server2016Essentials

> [!IMPORTANT]  
> Les fonctionnalités décrites dans cette rubrique fonctionnent uniquement sur Windows Server2016 avec le rôle expérience Essentials est activé et non à la référence SKU de Windows Server2016Essentials.


WindowsServerEssentials prend désormais en charge les déploiements plus importants avec:

- plusieurs domaines
- plusieurs contrôleurs de domaine
- possibilité de spécifier un contrôleur de domaine désigné
- Prise en charge jusqu'à 500utilisateurs et 500périphériques

##<a name="support-for-multiple-domains"></a>Prise en charge de plusieurs domaines

Serveur Windows2012R2 Essentials prend en charge qu’un seul domaine par serveur, ce qui est nécessaire, et le serveur Essentials doit être la racine de la forêt. Un domaine et forêt sont toujours nécessaires, le rôle expérience Windows Server2016Essentials peut désormais être déployé sur Windows Server2016 Standard ou Datacenter pour prendre en charge plusieurs domaines.

## <a name="support-for-multiple-domain-controllers"></a>Prise en charge de plusieurs contrôleurs de domaine

 WindowsServerEssentials2012R2 bloque tous les services qui tirent parti d’Azure ActiveDirectory, par exemple Office 365, où plusieurs contrôleurs de domaine est déployé. La raison est que la synchronisation de compte et mot de passe entre les contrôleurs de domaine local et Azure ActiveDirectory peut entraîner des informations d’identification compte avec des mots de passe qui ne sont pas synchronisées. Cette limitation a été supprimée dans Windows Server2016Essentials.

##<a name="ability-to-specify-a-designated-domain-controller"></a>Possibilité de spécifier un contrôleur de domaine désigné

Vous pouvez désormais choisir un contrôleur de domaine désigné qui sera améliorer les temps de récupération des objets de domaine ActiveDirectory, ainsi que coordonner la synchronisation de changement de compte sur les autres contrôleurs de domaine dans le domaine.

Désigné du contrôleur de domaine par défaut sera le même serveur qui exécute le rôle serveur expérience WindowsServerEssentials. Si ce serveur est un serveur membre, ce qui signifie qu’il n’est pas un contrôleur de domaine, puis la valeur par défaut, le contrôleur de domaine désigné sera déterminé automatiquement basé sur les tests du contrôleur de domaine dans le domaine a la plus faible latence du réseau pour le serveur qui exécute le rôle serveur expérience Windows Server. Si vous souhaitez modifier manuellement le serveur qui est le contrôleur de domaine désigné, vous pouvez le faire dans **paramètres** dans les **tableau de bord WindowsServerEssentials** comme illustré ci-dessous.

![Une capture d’écran montrant les paramètres du Panneau de commande au premier plan et le tableau de bord WindowsServerEssentials en arrière-plan. La page de contrôleur de domaine désigné du Panneau de configuration de paramètres est sélectionnée.](media/larger-deployments-1.PNG)

##<a name="support-for-500-users-and-500-devices"></a>Prise en charge de 500utilisateurs et 500périphériques
-------------------------------------

Le nombre maximal d’utilisateurs pris en charge et des périphériques dans Windows Server2012R2 Essentials est 25 et 50, respectivement. Avec l’introduction du rôle serveur expérience WindowsServerEssentials, cette limite a été augmentée à 100utilisateurs et 200appareils.

Windows Server2016Essentials prend en charge 500utilisateurs et 500périphériques. Rendre cet possible est une mise à jour pour l’infrastructure du fournisseur et le contrôle de liste d’objets pour qu’ils mettent en cache et rendu rapidement de longues listes d’objets utilisateur et périphérique. En outre, une fonctionnalité de recherche et de filtre a été ajoutée pour trouver rapidement l’utilisateur ou le périphérique peut être recherchées (voir Figure5-2). Vous trouverez des fonctionnalités de recherche et de filtrage dans le **tableau de bord WindowsServerEssentials**, **accès Web à distance**et le magasin Windows et Windows Phone **mon serveur** applications.

![Une capture d’écran illustrant l’utilisation de la fonctionnalité de recherche du tableau de bord WindowsServerEssentials pour rechercher la chaîne «d5c». Les résultats de la recherche incluent deux fichiers et les dossiers et les deux utilisateurs.](media/larger-deployments-2.PNG)

Une capture d’écran illustrant l’utilisation de la fonctionnalité de recherche du tableau de bord WindowsServerEssentials pour rechercher la chaîne «d5c». Les résultats de la recherche incluent deux fichiers et les dossiers et les deux utilisateurs.

> [!NOTE]  
> Alors que la limite utilisateur et périphérique pris en charge a augmenté pour le rôle de serveur WindowsServerEssentials, la limite de prise en charge pour le reste de sauvegarde de client à 75.

<a name="see-also"></a>Voir aussi
--------
[Prise en main WindowsServerEssentials](get-started.md)