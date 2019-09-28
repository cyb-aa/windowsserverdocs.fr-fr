---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: Planification de la capacité des serveurs proxy de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: eedb0f2ae4b6f600eb578c5db857cc1d79bccbd1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407980"
---
# <a name="planning-for-federation-server-proxy-capacity"></a>Planification de la capacité des serveurs proxy de fédération

La planification de la capacité pour les serveurs proxys de Fédération vous aide à estimer les éléments suivants :  
  
-   La configuration matérielle requise pour chaque serveur proxy de Fédération.  
  
-   Le nombre de serveurs de Fédération et de serveurs proxy de Fédération à placer dans chaque organisation.  
  
Les serveurs proxys de Fédération redirigent les jetons de sécurité d’un serveur de Fédération protégé du réseau d’entreprise vers les utilisateurs fédérés. L’objectif du déploiement d’un serveur proxy de Fédération est de permettre aux utilisateurs externes de se connecter à un serveur de Fédération. Il ne signe pas réellement les jetons et n’écrit pas dans les données de la base de données de configuration AD FS. Par conséquent, la configuration matérielle requise pour le serveur proxy de Fédération est généralement inférieure à la configuration matérielle requise pour un serveur de Fédération.  
  
Toutes les demandes adressées à un serveur proxy de Fédération entraînant une demande adressée à un serveur de Fédération ou à une batterie de serveurs de Fédération, la planification de la capacité des serveurs de Fédération et des serveurs proxys de Fédération doit être effectuée en parallèle.  
  
L’estimation du signe maximal @ no__t-0ins par seconde pour le serveur proxy de Fédération requiert une compréhension des modèles d’utilisation des utilisateurs fédérés qui se connecteront via le serveur proxy de Fédération. Dans de nombreux déploiements, les utilisateurs fédérés qui se connectent à l’aide du serveur proxy de Fédération se trouvent sur Internet. Vous pouvez estimer le pic @ no__t-0ins par seconde en examinant les modèles d’utilisation de ces utilisateurs fédérés sur les applications Web existantes qui seront protégées par AD FS.  
  
> [!NOTE]  
> Pour les déploiements de production, nous recommandons un minimum de deux serveurs proxys de Fédération pour chaque instance de batterie de serveurs de Fédération que vous déployez.  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>Estimer le nombre de serveurs proxys de Fédération requis pour votre organisation  
Avant de pouvoir estimer le nombre de AD FS ordinateurs proxy de serveur de Fédération requis, vous devez d’abord déterminer le nombre total de serveurs de Fédération que vous allez déployer dans votre organisation. Pour plus d’informations sur la façon de procéder, consultez [planification de la capacité du serveur de Fédération](Planning-for-Federation-Server-Capacity.md).  
  
Une fois que vous avez choisi le nombre de serveurs de Fédération, multipliez ce nombre de serveurs par le pourcentage de demandes d’authentification fédérée entrantes que vous prévoyez d’effectuer à partir d’utilisateurs externes \(located en dehors du réseau d’entreprise @ no__t-1. La valeur de ce calcul vous fournira le nombre estimé de serveurs proxys de Fédération qui traiteront les demandes d’authentification entrantes pour vos utilisateurs externes.  
  
Par exemple, si le nombre de serveurs de Fédération recommandés est 3 et que vous vous attendez à ce que le nombre total de demandes d’authentification effectuées par les utilisateurs externes soit environ 60% du nombre total de demandes d’authentification fédérée, votre le calcul est égal à 1,8 \(3 X .60 @ no__t-1, que vous pouvez arrondir à 2.  Par conséquent, dans ce cas, vous devez déployer deux machines proxy de serveur de Fédération pour tenir compte de la charge des demandes d’authentification de l’utilisateur externe pour les trois serveurs de Fédération.  
  
Dans les tests effectués par l’équipe de produit AD FS, l’utilisation globale du processeur sur chaque serveur proxy de Fédération a été jugée nettement inférieure à l’utilisation du processeur observée sur les serveurs de Fédération pour la même batterie de serveurs.  Dans un test, alors qu’un processeur du serveur de fédération indique qu’il a été complètement saturé, le processeur pour un serveur proxy de Fédération fournissant des services proxy pour cette même batterie a été observé à une utilisation de 20% seulement. Par conséquent, nos tests ont révélé que la charge sur le processeur d’un proxy de serveur de Fédération, qui utilise des spécifications matérielles similaires comme indiqué plus haut dans cette section, pouvait raisonnablement gérer la charge de traitement d’environ trois serveurs de Fédération.  
  
Toutefois, pour des raisons de tolérance de panne, nous vous recommandons de disposer au minimum de deux serveurs proxy de Fédération pour chaque batterie de serveurs de Fédération que vous déployez.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
