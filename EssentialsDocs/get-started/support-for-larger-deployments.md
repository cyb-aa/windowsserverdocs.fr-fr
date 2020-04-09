---
title: Prise en charge des déploiements plus importants
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 07d0c4c6-3e92-4969-82b8-105e46ab8d97
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c54defee45e8950d878ba70f627c1e645a2c8586
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817822"
---
# <a name="support-for-larger-deployments"></a>Prise en charge des déploiements plus importants

>S’applique à : Windows Server 2016 Essentials

> [!IMPORTANT]  
> Les fonctionnalités décrites dans cette rubrique fonctionnent uniquement sur Windows Server 2016 avec le rôle expérience Essentials activé, et non sur la référence SKU Windows Server 2016 Essentials.


Windows Server Essentials prend désormais en charge les déploiements plus importants avec :

- domaines multiples
- plusieurs contrôleurs de domaine
- possibilité de spécifier un contrôleur de domaine désigné
- prise en charge d’un maximum de 500 utilisateurs et de 500 appareils

## <a name="support-for-multiple-domains"></a>Prise en charge de domaines multiples

Windows Server 2012 R2 Essentials prend en charge un seul domaine par serveur, ce qui est requis et le serveur Essentials doit être la racine de la forêt. Lorsqu’un domaine et une forêt sont toujours requis, le rôle expérience Windows Server 2016 Essentials peut désormais être déployé sur Windows Server 2016 standard ou Datacenter pour prendre en charge plusieurs domaines.

## <a name="support-for-multiple-domain-controllers"></a>Prise en charge de plusieurs contrôleurs de domaine

 Windows Server Essentials 2012 R2 bloque tous les services qui exploitent Azure Active Directory, comme Office 365, où plusieurs contrôleurs de domaine sont déployés. Cela est dû au fait que la synchronisation des comptes et des mots de passe entre les contrôleurs de domaine locaux et les Azure Active Directory peut entraîner des informations d’identification de compte avec des mots de passe qui ne sont pas synchronisés. Cette limitation a été supprimée dans Windows Server 2016 Essentials.

## <a name="ability-to-specify-a-designated-domain-controller"></a>possibilité de spécifier un contrôleur de domaine désigné

Vous pouvez maintenant choisir un contrôleur de domaine désigné pour améliorer les temps de récupération des objets de domaine Active Directory, ainsi que pour coordonner la synchronisation des modifications de compte sur d’autres contrôleurs de domaine du domaine.

Votre contrôleur de domaine désigné par défaut sera le même serveur que celui qui exécute le rôle de serveur expérience Windows Server Essentials. Si ce serveur est un serveur membre, ce qui signifie qu’il ne s’agit pas d’un contrôleur de domaine, le contrôleur de domaine désigné par défaut sera automatiquement déterminé en fonction du test du contrôleur de domaine du domaine ayant la latence réseau la plus faible pour le serveur exécutant le Rôle serveur expérience Windows Server. Si vous souhaitez changer manuellement le serveur qui est le contrôleur de domaine désigné, vous pouvez le faire dans les **paramètres** du **tableau de bord Windows Server Essentials** , comme indiqué ci-dessous.

![Capture d’écran montrant les paramètres panneau de configuration au premier plan et le tableau de bord Windows Server Essentials en arrière-plan. La page contrôleur de domaine désigné du panneau de configuration paramètres est actuellement sélectionnée.](media/larger-deployments-1.PNG)

## <a name="support-for-500-users-and-500-devices"></a>Prise en charge des utilisateurs 500 et 500
-------------------------------------

Le nombre maximal d’utilisateurs et d’appareils pris en charge dans Windows Server 2012 R2 Essentials est respectivement 25 et 50. Avec l’introduction du rôle de serveur expérience Windows Server Essentials, cette limite a été augmentée pour 100 utilisateurs et 200 appareils.

Windows Server 2016 Essentials prend en charge 500 utilisateurs et 500 appareils. Cette possibilité est une mise à jour des contrôles de l’infrastructure du fournisseur et de la liste des objets afin de mettre en cache et de restituer rapidement des listes d’objets d’utilisateurs et d’appareils de grande taille. En outre, une fonctionnalité de recherche et de filtre a été ajoutée pour trouver rapidement l’utilisateur ou l’appareil que vous recherchez (voir la figure 5-2). Vous trouverez des fonctionnalités de recherche et de filtre dans le **tableau de bord Windows Server Essentials**, les **accès Web à distance**et les applications Windows et Windows Phone stockent **mes serveurs** .

![Capture d’écran montrant l’utilisation de la fonctionnalité de recherche du tableau de bord Windows Server Essentials pour rechercher la chaîne « d5c ». Les résultats de cette recherche incluent deux fichiers et dossiers, ainsi que deux utilisateurs.](media/larger-deployments-2.PNG)

Capture d’écran montrant l’utilisation de la fonctionnalité de recherche du tableau de bord Windows Server Essentials pour rechercher la chaîne « d5c ». Les résultats de cette recherche incluent deux fichiers et dossiers, ainsi que deux utilisateurs.

> [!NOTE]  
> Bien que la limite d’utilisateurs et d’appareils pris en charge ait augmenté pour le rôle serveur Windows Server Essentials, la limite prise en charge pour la sauvegarde du client reste à 75.

<a name="see-also"></a>Voir aussi
--------
[Prise en main de Windows Server Essentials](get-started.md)