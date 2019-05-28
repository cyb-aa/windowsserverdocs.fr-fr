---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: Planification de la capacité des serveurs proxy de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c3efbb4081336ebfdfe9d3ab8a2b91412aa82dee
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191078"
---
# <a name="planning-for-federation-server-proxy-capacity"></a>Planification de la capacité des serveurs proxy de fédération

Planification de capacité pour les serveurs proxy de fédération permet d’évaluer :  
  
-   La configuration matérielle appropriée pour chaque serveur proxy de fédération.  
  
-   Le nombre de serveurs de fédération et les serveurs proxy de fédération à placer dans chaque organisation.  
  
Serveurs proxy de fédération redirige les jetons de sécurité à partir d’un serveur de fédération protégé dans le réseau d’entreprise pour les utilisateurs fédérés. L’objectif du déploiement d’un serveur proxy de fédération consiste à permettre aux utilisateurs externes pour se connecter à un serveur de fédération. Il fait le pas signer les jetons ou d’écriture dans la base de données de configuration AD FS. Par conséquent, la configuration matérielle requise pour le serveur proxy de fédération est généralement inférieure à la configuration matérielle requise pour un serveur de fédération.  
  
Étant donné que chaque demande adressée à un serveur proxy de fédération entraîne une demande à un serveur de fédération ou de la batterie de serveurs de fédération, la planification de capacité pour les serveurs de fédération et les serveurs proxy de fédération doit être effectuée en parallèle.  
  
Estimation de la connexion pointe\-ins par seconde pour le serveur proxy de fédération implique de comprendre les modèles d’utilisation des utilisateurs fédérés qui seront connecteront via le serveur proxy de fédération. Dans de nombreux déploiements, les utilisateurs fédérés qui se connectent à l’aide du serveur proxy de fédération sont situés sur Internet. Vous pouvez estimer la connexion pointe\-ins par seconde en examinant les modèles d’utilisation de ces les utilisateurs sur les applications Web existantes qui seront protégées par AD FS fédérés.  
  
> [!NOTE]  
> Pour les déploiements de production, nous recommandons un minimum de deux serveurs proxy de fédération pour chaque instance de batterie de serveurs de serveur de fédération déployée.  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>Estimer le nombre de serveurs proxy de fédération nécessaires pour votre organisation  
Avant que vous pouvez estimer le nombre de machines de proxy de serveur de fédération AD FS requise, vous devez d’abord déterminer le nombre total de serveurs de fédération que vous allez déployer dans votre organisation. Pour plus d’informations sur la procédure à suivre, consultez [planification de la capacité du serveur de fédération](Planning-for-Federation-Server-Capacity.md).  
  
Une fois que vous avez décidé du nombre de serveurs de fédération, multiplier ce nombre de serveurs en fonction du pourcentage de l’authentification fédérée entrant demande à laquelle sera établie à partir des utilisateurs externes \(situé en dehors du réseau d’entreprise\). La valeur de ce calcul vous fournira avec le nombre estimé de serveurs proxy de fédération qui gère les demandes d’authentification entrantes pour vos utilisateurs externes.  
  
Par exemple, si le nombre de serveurs de fédération recommandée est 3, et vous attendez à ce que le nombre total de demandes d’authentification qui vont être apportées par les utilisateurs externes sera environ 60 % du nombre total de demandes d’authentification fédérée, vos calcul serait égale à 1.8 \(3 X.60\) dont vous pouvez arrondir jusqu'à 2.  Dans ce cas, vous devrez par conséquent, déployez deux machines de proxy de serveur de fédération pour prendre en compte la charge des demandes d’authentification utilisateur externe pour les serveurs de fédération de trois.  
  
Dans les tests effectués par l’équipe de produit AD FS, l’utilisation globale du processeur sur chaque serveur proxy de fédération a été trouvée pour être nettement plus faible que l’utilisation du processeur qui a été observé sur les serveurs de fédération pour la même batterie de serveurs.  Dans un seul test, tandis qu’un processeur de serveur de fédération a été indiquant qu’il a été complètement saturé, l’UC pour un serveur proxy de fédération en fournissant des services de proxy pour ce même batterie de serveurs a été observée au seul 20 % d’utilisation. Par conséquent, nos tests ont révélé que la charge sur l’UC d’un serveur proxy de fédération, qui utilise les spécifications matérielles similaires comme indiqué plus haut dans cette section, peut permettre de gérer la charge de traitement pour environ trois serveurs de fédération.  
  
Toutefois, à des fins de tolérance de panne, nous recommandons un minimum de deux serveurs proxy de fédération pour chaque batterie de serveurs de fédération que vous déployez.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
