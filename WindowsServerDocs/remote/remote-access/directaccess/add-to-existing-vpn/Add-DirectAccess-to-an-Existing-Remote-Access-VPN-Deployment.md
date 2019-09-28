---
title: Add DirectAccess to an Existing Remote Access (VPN) Deployment
description: Cette rubrique fait partie du guide ajouter DirectAccess à un déploiement d’accès à distance (VPN) existant pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5db01f7-1ae0-46f2-9be7-8d9e121446b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9266acfb38c65711d6d0b12e2b6223a8a4e91746
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388798"
---
# <a name="add-directaccess-to-an-existing-remote-access-vpn-deployment"></a>Add DirectAccess to an Existing Remote Access (VPN) Deployment

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016
  
## <a name="BKMK_OVER"></a>Description du scénario  
Dans ce scénario, un seul ordinateur exécutant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 est configuré en tant que serveur DirectAccess avec les paramètres recommandés une fois que vous avez déjà installé et configuré le VPN. Si vous souhaitez configurer DirectAccess avec des fonctionnalités d’entreprise, telles qu’un cluster à charge équilibrée, un déploiement multisite ou une authentification client à deux facteurs, suivez le scénario décrit dans cette rubrique pour configurer un serveur unique, puis configurer l’entreprise. comme décrit dans [déployer l’accès à distance dans une entreprise](../../ras/Deploy-Remote-Access-in-an-Enterprise.md).  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Pour configurer un serveur d'accès à distance unique, plusieurs étapes de planification et de déploiement sont nécessaires.  
  
### <a name="planning-steps"></a>Étapes de planification  
La planification comporte les deux phases suivantes :  
  
1.  **Planifier l’infrastructure d’accès à distance**  
  
    Au cours de cette phase, vous décrivez les étapes de planification requises pour configurer l'infrastructure réseau avant de commencer le déploiement de l'accès à distance. Cette phase comprend la planification de la topologie de réseau et de serveurs, des certificats, du DNS (Domain Name System), de la configuration d'Active Directory et des objets de stratégie de groupe, ainsi que du serveur d'emplacement réseau DirectAccess.  
  
2.  **Planifier le déploiement de l’accès à distance**  
  
    Au cours de cette phase, vous décrivez les étapes de planification requises pour préparer le déploiement de l'accès à distance. Cette phase inclut la planification des ordinateurs clients d'accès à distance, des conditions requises d'authentification des serveurs et clients, ainsi que des serveurs d'infrastructure.  
  
### <a name="deployment-steps"></a>Étapes de déploiement  
Le déploiement comporte les trois phases suivantes :  
  
1.  **Configurer l’infrastructure d’accès à distance**  
  
    Au cours de cette phase, vous configurez le réseau et le routage, les paramètres de pare-feu (le cas échéant), les certificats, les serveurs DNS, Active Directory et les paramètres des objets de stratégie de groupe, ainsi que le serveur d'emplacement réseau DirectAccess.  
  
2.  **Configurer les paramètres du serveur d’accès à distance**  
  
    Au cours de cette phase, vous configurez les ordinateurs clients d'accès à distance, le serveur d'accès à distance et les serveurs d'infrastructure.  
  
3.  **Vérifier le déploiement**  
  
    Au cours de cette phase, vous vérifiez que le déploiement fonctionne comme prévu.  
  
## <a name="BKMK_APP"></a>Applications pratiques  
Le déploiement d’un serveur d’accès à distance individuel présente les caractéristiques suivantes :  
  
-   **Facilité d’accès**  
  
    Les ordinateurs clients gérés exécutant Windows 8 et Windows 7 peuvent être configurés en tant qu’ordinateurs clients DirectAccess. Ces clients peuvent accéder aux ressources réseau internes via DirectAccess chaque fois qu'ils se trouvent sur Internet, sans avoir à se connecter à une connexion VPN. Les ordinateurs clients qui n'exécutent pas l'un de ces systèmes d'exploitation peuvent se connecter au réseau interne via un VPN. DirectAccess et VPN sont gérés dans la même console et avec le même jeu d’Assistants.  
  
-   **Facilité de gestion**  
  
    Les ordinateurs clients DirectAccess qui ont accès à Internet peuvent être gérés à distance par des administrateurs d'accès à distance via DirectAccess, même si ces ordinateurs ne font pas partie du réseau interne de l'entreprise. Les ordinateurs clients qui ne répondent pas aux spécifications de l’entreprise peuvent être automatiquement mis à jour par les serveurs d’administration.  
  
## <a name="BKMK_NEW"></a>Rôles et fonctionnalités requis pour ce scénario  
Le tableau suivant répertorie les fonctionnalités et rôles requis pour ce scénario :  
  
|Rôle/fonctionnalité|Prise en charge de ce scénario|  
|---------|-----------------|  
|Rôle Accès à distance|Le rôle est installé et désinstallé à l'aide de la console du Gestionnaire de serveur ou de Windows PowerShell. Ce rôle englobe DirectAccess, qui était auparavant une fonctionnalité de Windows Server 2008 R2, et les Services de routage et d'accès à distance, qui étaient auparavant un service de rôle sous le rôle serveur Services de stratégie et d'accès réseau. Le rôle Accès à distance est constitué de deux composants :<br /><br />1.  VPN DirectAccess et des Services de routage et d'accès à distance(RRAS) : géré dans la console de gestion de l'accès à distance.<br />2.  Routage RRAS : géré dans la console Accès à distance et routage.<br /><br />Le rôle serveur Accès à distance dépend des fonctionnalités de serveur suivantes :<br /><br />-Serveur Web Internet Information Services (IIS) : requis pour configurer le serveur emplacement réseau sur le serveur d’accès à distance et la sonde Web par défaut.<br />-Base de données interne Windows : utilisée pour la gestion locale des comptes sur le serveur d'accès à distance.|  
|Fonctionnalité des outils de gestion de l’accès à distance|Cette fonctionnalité est installée comme suit :<br /><br />-Par défaut, sur un serveur d’accès à distance lorsque le rôle accès à distance est installé. Elle prend en charge l'interface utilisateur de la console de gestion à distance et les applets de commande Windows PowerShell ;<br />-Installé éventuellement sur un serveur qui n’exécute pas le rôle serveur accès à distance. Dans ce cas, elle est utilisée pour la gestion à distance d'un ordinateur d'accès à distance qui exécute DirectAccess et le réseau privé virtuel (VPN).<br /><br />La fonctionnalité des outils de gestion de l’accès à distance est constituée des éléments suivants :<br /><br />-Interface utilisateur graphique d’accès à distance<br />-Module d’accès à distance pour Windows PowerShell<br /><br />Les dépendances incluent :<br /><br />-Console de gestion des stratégies de groupe<br />-Kit d’administration du gestionnaire des connexions (CMAK) RAS<br />-Windows PowerShell 3,0<br />-Outils et infrastructure de gestion graphique|  
  
## <a name="BKMK_HARD"></a>Configuration matérielle requise  
La configuration matérielle requise pour ce scénario comprend les éléments suivants :  
  
**Configuration requise du serveur**  
  
-   Un ordinateur qui répond à la configuration matérielle requise pour Windows Server 2012.  
  
-   Sur le serveur, au moins une carte réseau doit être installée, activée et connectée au réseau interne. Quand deux cartes sont utilisées, la première doit être connectée au réseau interne de l’entreprise et la seconde doit être connectée au réseau externe (Internet).  
  
-   Si le protocole Teredo est requis pour la transition d’IPv4 vers IPv6, la carte externe du serveur nécessite deux adresses IPv4 publiques consécutives. L'Assistant Activation de DirectAccess n'active pas Teredo, même si deux adresses IP consécutives sont présentes. Si une seule adresse IP est disponible, seul le protocole IP-HTTPS peut être utilisé pour la transition.  
  
-   Au moins un contrôleur de domaine. Le serveur d’accès à distance et les clients DirectAccess doivent être membres d’un domaine.  
  
-   L'Assistant Activation de DirectAccess requiert des certificats pour IP-HTTPS et le serveur d'emplacement réseau. Si le VPN SSTP utilise déjà un certificat, celui-ci est réutilisé pour IP-HTTPS. Si le VPN SSTP n'est pas configuré, vous pouvez configurer un certificat pour IP-HTTPS ou utiliser un certificat auto-signé créé automatiquement. Pour le serveur d'emplacement réseau, vous pouvez configurer un certificat ou utiliser un certificat auto-signé créé automatiquement.  
  
**Configuration requise du client**  
  
-   Un ordinateur client doit exécuter Windows 8 ou Windows 7.  
  
    > [!NOTE]  
    > Seuls les systèmes d'exploitation suivants peuvent être utilisés en tant que clients DirectAccess : Windows Server 2012, Windows Server 2008 R2, Windows 8 entreprise, Windows 7 entreprise et Windows 7 édition intégrale.  
  
**Configuration requise pour le serveur d’infrastructure et de gestion**  
  
-   Lors de la gestion à distance des ordinateurs clients DirectAccess, les clients initient des communications avec des serveurs d'administration tels que les contrôleurs de domaine, les serveurs de configuration System Center et les serveurs d'autorité HRA (Health Registration Authority) pour des services incluant les mises à jour Windows et d'antivirus, ainsi que la mise en conformité des clients de protection d'accès réseau (NAP). Les serveurs requis doivent être déployés préalablement au déploiement de l'accès à distance.  
  
-   Si l'accès à distance requiert la conformité de la protection d'accès réseau (NAP) pour les clients, les serveurs NPS (Network Policy Server) et l'autorité HRA (Health Registration Authority) doivent être déployés préalablement au déploiement de l'accès à distance.  
  
-   Un serveur DNS exécutant Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 avec SP2 est requis.  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
La configuration logicielle requise pour ce scénario comprend les éléments suivants :  
  
**Configuration requise du serveur**  
  
-   Le serveur d’accès à distance doit être membre d’un domaine. Le serveur peut être déployé en périphérie du réseau interne ou derrière un pare-feu de périmètre ou un autre périphérique.  
  
-   Si le serveur d'accès à distance se trouve derrière un pare-feu de périmètre ou un périphérique de traduction d'adresses réseau (NAT), ce périphérique doit être configuré de manière à autoriser le trafic en direction et en provenance du serveur d'accès à distance.  
  
-   La personne qui déploie l'accès à distance sur le serveur doit disposer des autorisations d'administrateur local sur le serveur, ainsi que des autorisations d'utilisateur de domaine. L'administrateur doit également disposer des autorisations pour les objets de stratégie de groupe qui sont utilisés dans le déploiement de DirectAccess. L'utilisation des fonctionnalités qui limitent un déploiement de DirectAccess sur les ordinateurs portables uniquement nécessite de disposer des autorisations permettant de créer un filtre WMI sur le contrôleur de domaine.  
  
**Configuration requise du client d’accès à distance**  
  
-   Les clients DirectAccess doivent appartenir au domaine. Les domaines qui contiennent des clients peuvent appartenir à la même forêt que celle du serveur d'accès à distance ou ils peuvent avoir une relation d'approbation bidirectionnelle avec la forêt ou le domaine du serveur d'accès à distance.  
  
-   Un groupe de sécurité Active Directory est requis afin de contenir les ordinateurs qui seront configurés en tant que clients DirectAccess. Si vous ne spécifiez aucun groupe de sécurité lors de la configuration des paramètres des clients DirectAccess, l'objet de stratégie de groupe client est appliqué par défaut sur tous les ordinateurs portables (qui sont compatibles avec DirectAccess) inclus dans le groupe de sécurité Ordinateurs du domaine. Seuls les systèmes d'exploitation suivants peuvent être utilisés en tant que clients DirectAccess :  Windows Server 2012, Windows Server 2008 R2, Windows 8 entreprise, Windows 7 entreprise et Windows 7 édition intégrale.  
  
    > [!NOTE]  
    > Nous vous recommandons de créer un groupe de sécurité pour chaque domaine qui contient les ordinateurs qui seront configurés en tant que clients DirectAccess.  
  

  

