---
title: Gérer l’accès à distance
description: Cette rubrique fournit des informations sur la gestion de l’accès à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1459819a-b1b6-4800-8770-4a85d02c7a2b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2b9065b2d4541063c8cd6f09d47f48a9ba7833e1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404658"
---
# <a name="manage-remote-access"></a>Gérer l’accès à distance

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Le scénario de déploiement de l’administration à distance des clients DirectAccess utilise DirectAccess pour gérer des clients sur Internet. Cette section décrit le scénario, y compris ses phases, ses rôles, ses fonctionnalités et les liens vers des ressources supplémentaires.  
  
Windows Server 2016 et Windows Server 2012 combinent DirectAccess et le réseau privé virtuel (RRAS) du service de routage et d’accès à distance (RRAS) en un seul rôle d’accès à distance.   
  
> [!NOTE]  
> Outre cette rubrique, les rubriques suivantes sur l’administration de l’accès à distance sont disponibles.  
>   
> -   [Utiliser l’analyse et la gestion de comptes de l’accès à distance](monitoring-and-accounting/Use-Remote-Access-Monitoring-and-Accounting.md)  
> -   [Gérer les clients DirectAccess à distance](manage-remote-clients/Manage-DirectAccess-Clients-Remotely.md)  
  
## <a name="BKMK_OVER"></a>Description du scénario  
Les ordinateurs clients DirectAccess sont connectés à l’intranet lorsqu’ils sont connectés à Internet, indépendamment du fait que l’utilisateur soit connecté à l’ordinateur ou non. Ils peuvent être administrés en tant que ressources intranet et actualisés à l’aide de modifications de stratégie de groupe, de mises à jour du système d’exploitation, de mises à jour de logiciel anti-programme malveillant et d’autres modifications organisationnelles.  
  
Dans certains cas, les serveurs ou ordinateurs intranet doivent établir des connexions avec les clients DirectAccess. Par exemple, les techniciens du support technique peuvent utiliser des connexions Bureau à distance pour se connecter à des clients DirectAccess distants et les dépanner. Ce scénario vous permet de conserver votre solution d’accès à distance existante pour la connectivité des utilisateurs, tout en utilisant DirectAccess pour l’administration à distance.  
  
DirectAccess fournit une configuration qui prend en charge la gestion à distance des clients DirectAccess. Vous pouvez utiliser l’option de l’Assistant Déploiement qui limite la création de stratégies à celles nécessaires pour la gestion à distance des ordinateurs clients.  
  
> [!NOTE]  
> Dans ce déploiement, les options de configuration de niveau utilisateur, comme le tunneling forcé, l’intégration de la protection d’accès réseau (NAP) et l’authentification à deux facteurs, ne sont pas disponibles.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Le scénario de déploiement de l’administration à distance des clients DirectAccess inclut les étapes suivantes en termes de planification et de configuration :  
  
### <a name="plan-the-deployment"></a>Planifier le déploiement  
la planification de ce scénario ne présente que quelques conditions requises en termes d’ordinateur et de réseau. Elles comprennent :  
  
-   **Topologie de réseau et serveur** : avec DirectAccess, vous pouvez placer votre serveur d’accès à distance à la périphérie de votre intranet ou derrière un pare-feu ou un périphérique de traduction d’adresses réseau (NAT).  
  
-   **Serveur d’emplacement réseau DirectAccess** : Les clients DirectAccess utilisent le serveur Emplacement réseau pour déterminer s'ils se trouvent sur le réseau interne. Le serveur Emplacement réseau peut être installé sur le serveur DirectAccess ou sur un autre serveur.  
  
-   **Clients DirectAccess** : Identifiez les ordinateurs gérés qui seront configurés en tant que clients DirectAccess.  
  
### <a name="configure-the-deployment"></a>Configurer le déploiement  
La configuration de votre déploiement comprend un certain nombre d’étapes. Elles incluent notamment :  
  
1.  **Configurer l’infrastructure** : configurez les paramètres DNS, joignez les ordinateurs clients et serveur à un domaine, si nécessaire, et configurez les groupes de sécurité Active Directory.  
  
    Dans ce scénario de déploiement, les objets de stratégie de groupe (GPO) sont créés automatiquement par l’accès à distance. Pour les options de GPO de certificat avancé, consultez [déploiement de l’accès à distance avancé](assetId:///3475e527-541f-4a34-b940-18d481ac59f6).  
  
2.  **Configurer le serveur d’accès à distance et les paramètres réseau** : Configurez les cartes réseau, les adresses IP et le routage.  
  
3.  **Configurer les paramètres de certificats** : Dans ce scénario de déploiement, l’Assistant Prise en main crée des certificats auto-signés. il n’est donc pas nécessaire de configurer l’infrastructure de certificat plus avancée.  
  
4.  **Configurer le serveur Emplacement réseau** :  Dans ce scénario, le serveur Emplacement réseau est installé sur le serveur d’accès à distance.  
  
5.  **Planifier les serveurs d’administration DirectAccess** : Les administrateurs peuvent administrer à distance les ordinateurs clients DirectAccess situés en dehors du réseau d’entreprise à l’aide d’Internet. Les serveurs d’administration incluent les ordinateurs utilisés lors de l’administration à distance des clients (tels que les serveurs de mise à jour).  
  
6.  **Configurer le serveur d’accès à distance** : installez le rôle Accès à distance et exécutez l’Assistant Mise en route de DirectAccess pour configurer DirectAccess.  
  
7.  **Vérifier le déploiement** : testez un client pour vous assurer s’il peut se connecter au réseau interne et à Internet à l’aide de DirectAccess.  
  
## <a name="BKMK_APP"></a>Applications pratiques  
Le déploiement d’un serveur d’accès à distance individuel pour la gestion des clients DirectAccess fournit les avantages suivants :  
  
-   **Simplicité d'accès** : Les ordinateurs clients gérés exécutant Windows 8 ou Windows 7 peuvent être configurés en tant qu’ordinateurs clients DirectAccess. Ces clients peuvent accéder aux ressources réseau internes via DirectAccess chaque fois qu’ils sont connectés à Internet, sans avoir à utiliser une connexion VPN. Les ordinateurs clients qui n’exécutent pas l’un de ces systèmes d’exploitation peuvent se connecter au réseau interne via VPN. DirectAccess et VPN sont gérés dans la même console et avec le même jeu d’Assistants.  
  
-   **Facilité de gestion** : les ordinateurs clients DirectAccess qui sont connectés à Internet peuvent être gérés à distance par des administrateurs d’accès à distance via DirectAccess, même si ces ordinateurs ne font pas partie du réseau interne de l’entreprise. Les ordinateurs clients qui ne répondent pas aux spécifications de l’entreprise peuvent être automatiquement mis à jour par les serveurs d’administration. Un ou plusieurs serveurs d’accès à distance peuvent être gérés à partir d’une seule console de gestion de l’accès à distance.  
  
## <a name="BKMK_NEW"></a>Rôles et fonctionnalités inclus dans ce scénario  
Le tableau suivant répertorie les fonctionnalités et rôles requis pour ce scénario :  
  
|Rôle ou fonctionnalité|Prise en charge de ce scénario|  
|----------|-----------------|  
|*Rôle accès à distance*|Ce rôle est installé et désinstallé à l’aide de la console du Gestionnaire de serveur ou de Windows PowerShell. Ce rôle englobe DirectAccess, qui était auparavant une fonctionnalité de Windows Server 2008 R2, et les Services de routage et d'accès à distance, qui étaient auparavant un service de rôle sous le rôle serveur Services de stratégie et d'accès réseau. Le rôle Accès à distance est constitué de deux composants :<br /><br />1.  VPN DirectAccess et des Services de routage et d'accès à distance(RRAS) : DirectAccess et VPN sont gérés dans la console de gestion de l’accès à distance.<br />2.  RRAS : les fonctionnalités sont gérées dans la console Accès à distance et routage.<br /><br />Le rôle de serveur Accès à distance dépend des fonctionnalités suivantes :<br /><br />-Serveur Web (IIS) : requis pour configurer le serveur Emplacement réseau et la sonde web par défaut.<br />-Base de données interne Windows : utilisée pour la gestion locale des comptes sur le serveur d'accès à distance.|  
|Fonctionnalité des outils de gestion de l’accès à distance|Cette fonctionnalité est installée comme suit :<br /><br />-Par défaut, sur un serveur d’accès à distance lorsque le rôle accès à distance est installé et prend en charge l’interface utilisateur de la console de gestion à distance.<br />-En tant qu’option sur un serveur qui n’exécute pas le rôle serveur accès à distance. Dans ce cas, elle est utilisée pour la gestion à distance d’un serveur d’accès à distance.<br /><br />Cette fonctionnalité est constituée des éléments suivants :<br /><br />-Interface utilisateur graphique d’accès à distance et outils en ligne de commande<br />-Module d’accès à distance pour Windows PowerShell<br /><br />Les dépendances incluent :<br /><br />-Console de gestion des stratégies de groupe<br />-Kit d’administration du gestionnaire des connexions (CMAK) RAS<br />-Windows PowerShell 3,0<br />-Outils et infrastructure de gestion graphique|  
  
## <a name="BKMK_HARD"></a>Configuration matérielle requise  
La configuration matérielle requise pour ce scénario comprend les éléments suivants :  
  
### <a name="server-requirements"></a>Configuration requise du serveur  
  
-   Un ordinateur qui répond à la configuration matérielle requise pour Windows Server 2016. Pour plus d’informations, voir [Configuration système requise](https://technet.microsoft.com/windows-server-docs/get-started/system-requirements-and-installation)pour Windows Server 2016.  
  
-   Au moins une carte réseau doit être installée et activée sur le serveur. Une seule carte doit être connectée au réseau interne de l’entreprise et une seule carte doit être connectée au réseau externe (Internet).  
  
-   Si le protocole Teredo est requis pour la transition d’IPv6 vers IPv4, la carte externe du serveur nécessite deux adresses IPv4 publiques consécutives. Si une seule carte réseau est disponible, seul le protocole IP-HTTPS peut être utilisé pour la transition.  
  
-   Au moins un contrôleur de domaine. Les serveurs d’accès à distance et les clients DirectAccess doivent être membres d’un domaine.  
  
-   Une autorité de certification est requise sur le serveur si vous ne souhaitez pas utiliser de certificats auto-signés pour IP-HTTPS ou le serveur Emplacement réseau, ou si vous souhaitez utiliser des certificats clients pour l’authentification IPsec des clients.  
  
### <a name="client-requirements"></a>Configuration requise du client  
  
-   Un ordinateur client doit exécuter Windows 10 ou Windows 8 ou Windows 7.  
  
### <a name="infrastructure-and-management-server-requirements"></a>Configuration requise en termes d'infrastructure et de serveurs d'administration  
  
-   Lors de la gestion à distance des ordinateurs clients DirectAccess, les clients initient des communications avec des serveurs d’administration tels que les contrôleurs de domaine, les serveurs System Center Configuration et les serveurs d’autorité HRA (Health Registration Authority). Ces serveurs fournissent des services comprenant des mises à jour Windows et d’antivirus et la conformité du client avec la protection d’accès réseau (NAP). Les serveurs requis doivent être déployés préalablement au déploiement de l’accès à distance.  
  
-   Un serveur DNS exécutant Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 avec SP2 est requis.  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
La configuration logicielle requise pour ce scénario comprend les éléments suivants :  
  
### <a name="server-requirements"></a>Configuration requise du serveur  
  
-   Le serveur d’accès à distance doit être membre d’un domaine. Le serveur peut être déployé en périphérie du réseau interne ou derrière un pare-feu de périmètre ou un autre périphérique.  
  
-   Si le serveur d’accès à distance se trouve derrière un pare-feu de périmètre ou un périphérique NAT, ce périphérique doit être configuré de manière à autoriser le trafic en direction et en provenance du serveur d’accès à distance.  
  
-   Les administrateurs qui déploient un serveur d’accès à distance doivent disposer des autorisations d’administrateur locales sur le serveur, ainsi que des autorisations d’utilisateur de domaine. L’administrateur doit également disposer des autorisations pour les objets de stratégie de groupe qui sont utilisés pour le déploiement de DirectAccess. Afin de tirer parti des fonctionnalités qui limitent le déploiement de DirectAccess sur les ordinateurs portables uniquement, les autorisations Admins du domaine permettant de créer un filtre WMI sur le contrôleur de domaine sont requises.  
  
-   Si le serveur d’emplacement réseau ne se trouve pas sur le serveur d’accès à distance, il est nécessaire d’utiliser un serveur distinct pour l’exécuter.  
  
### <a name="remote-access-client-requirements"></a>Configuration requise pour les clients d'accès à distance  
  
-   Les clients DirectAccess doivent appartenir au domaine. Les domaines qui contiennent des clients peuvent appartenir à la même forêt que celle du serveur d'accès à distance ou ils peuvent avoir une relation d'approbation bidirectionnelle avec la forêt ou le domaine du serveur d'accès à distance.  
  
-   Un groupe de sécurité Active Directory est requis afin de contenir les ordinateurs qui seront configurés en tant que clients DirectAccess. Les ordinateurs ne doivent pas être inclus dans plus d’un groupe de sécurité contenant des clients DirectAccess. Si les clients sont inclus dans plusieurs groupes, la résolution de noms des demandes client ne fonctionnera pas comme prévu.  
  

