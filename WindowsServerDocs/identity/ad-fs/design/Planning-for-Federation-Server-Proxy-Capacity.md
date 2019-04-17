---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: "Planification de capacité des serveurs Proxy de fédération"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e57f34b173c10e9e753c7f3b8dcd88d7bf6742c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-federation-server-proxy-capacity"></a>Planification de capacité des serveurs Proxy de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Planification de capacité pour les serveurs proxy de fédération vous permet d’évaluer:  
  
-   La configuration matérielle appropriée pour chaque serveur proxy de fédération.  
  
-   Le nombre de serveurs de fédération et les serveurs proxy de fédération pour placer dans chaque organisation.  
  
Serveurs proxy de fédération redirige les jetons de sécurité à partir d’un serveur de fédération protégé dans le réseau d’entreprise aux utilisateurs fédérés. L’objectif du déploiement d’un serveur proxy de fédération consiste à permettre aux utilisateurs externes pour se connecter à un serveur de fédération. Il ne prend pas réellement signature des jetons ou d’écriture dans la base de données de configuration ADFS. Par conséquent, la configuration matérielle requise pour le serveur proxy de fédération est généralement inférieure à la configuration matérielle requise pour un serveur de fédération.  
  
Étant donné que chaque demande à un serveur proxy de fédération entraîne une demande à un serveur de fédération ou d’une batterie de serveurs de fédération, planification de capacité pour les serveurs de fédération et les serveurs proxy de fédération doit être effectuée en parallèle.  
  
Estimation des connexion-ins pointe par seconde pour le serveur proxy de fédération tenu de comprendre les modèles d’utilisation des utilisateurs fédérés qui se connectent via le serveur proxy de fédération. Dans de nombreux déploiements, les utilisateurs fédérés qui se connectent à l’aide du serveur proxy de fédération sont trouvent sur Internet. Vous pouvez estimer les modules de connexion maximale par seconde en examinant les modèles d’utilisation de ces utilisateurs fédérés sur les applications Web existantes qui sont protégées par ADFS.  
  
> [!NOTE]  
> Pour les déploiements de production, nous vous recommandons d’un minimum de deux serveurs proxys de fédération pour chaque instance de batterie de serveurs de serveur de fédération que vous déployez.  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>Estimer le nombre de serveurs proxy de fédération nécessaires pour votre organisation  
Avant que vous pouvez estimer le nombre d’ordinateurs virtuels proxy du serveur de fédération ADFS requis, vous devez d’abord déterminer le nombre total de serveurs de fédération que vous allez déployer dans votre organisation. Pour plus d’informations sur la procédure à suivre, voir [planification de la capacité du serveur de fédération](Planning-for-Federation-Server-Capacity.md).  
  
Une fois que vous avez décidé du nombre de serveurs de fédération, multipliez ce nombre de serveurs par le pourcentage de l’authentification fédérée entrante demande que vous attendez sont effectués par les utilisateurs externes \ (situés en dehors de l’entreprise Updatable). La valeur de ce calcul fournit avec estimation du nombre de serveurs proxy de fédération qui gère les demandes d’authentification entrantes pour vos utilisateurs externes.  
  
Par exemple, si le nombre de serveurs de fédération recommandée est de 3 et que vous pensez que le nombre total de demandes d’authentification qui seront effectuées par les utilisateurs externes sera environ 60% du nombre total de demandes d’authentification fédérée, le calcul est égal 1,8 \ (3 X. 60\) auquel vous pouvez arrondir jusqu'à 2.  Par conséquent, dans ce cas, vous devez déployer deux ordinateurs virtuels federation server proxy pour prendre en compte la charge des demandes d’authentification utilisateur externe pour les serveurs de trois fédération.  
  
Lors des tests effectués par l’équipe du produit ADFS, l’utilisation du processeur globale sur chaque serveur proxy de fédération a été détectée est sensiblement inférieure à l’utilisation du processeur qui a été observée sur les serveurs de fédération pour la même batterie de serveurs.  Dans un test, tandis qu’un processeur de serveur de fédération a été indiquant qu’il a été complètement saturé, le processeur pour un serveur proxy de fédération qui fournit les services de proxy pour ce même batterie de serveurs a été observé à 20% d’utilisation uniquement. Par conséquent, nos tests ont révélé que la charge sur le processeur d’un serveur proxy de fédération, qui utilise des caractéristiques matérielles similaires comme indiqué plus haut dans cette section, peut gérer raisonnablement la charge de traitement pour environ trois serveurs de fédération.  
  
Toutefois, pour des raisons de tolérance de pannes, nous recommandons un minimum de deux serveurs proxys de fédération pour chaque batterie de serveurs de fédération que vous déployez.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
