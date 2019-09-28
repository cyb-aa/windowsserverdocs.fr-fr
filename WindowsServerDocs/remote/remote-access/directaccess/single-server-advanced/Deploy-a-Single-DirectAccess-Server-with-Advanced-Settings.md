---
title: Déployer un serveur DirectAccess unique avec des paramètres avancés
description: Cette rubrique fait partie du guide déployer un serveur DirectAccess unique avec des paramètres avancés pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b211a9ca-1208-4e1f-a0fe-26a610936c30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8ccb91973dfb3493b534bdbc8fc4e2bcb26b26b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404960"
---
# <a name="deploy-a-single-directaccess-server-with-advanced-settings"></a>Déployer un serveur DirectAccess unique avec des paramètres avancés

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique fournit une introduction au scénario DirectAccess qui utilise un serveur DirectAccess unique et vous permet de déployer DirectAccess avec des paramètres avancés.  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>Avant de commencer le déploiement, consultez la liste des configurations non prises en charge, des problèmes connus et des configurations requises  
Vous pouvez utiliser les rubriques suivantes pour passer en revue les conditions préalables et d’autres informations avant de déployer DirectAccess.  
  
-   [Configurations non prises en charge DirectAccess](../../../remote-access/directaccess/DirectAccess-Unsupported-Configurations.md)  
  
-   [Conditions préalables pour le déploiement de DirectAccess](../../../remote-access/directaccess/Prerequisites-for-Deploying-DirectAccess.md)  
  
## <a name="BKMK_OVER"></a>Description du scénario  
Dans ce scénario, un seul ordinateur exécutant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 est configuré en tant que serveur DirectAccess avec des paramètres avancés.  
  
> [!NOTE]  
> Pour configurer un déploiement de base avec des paramètres simples uniquement, consultez [Déploiement d'un serveur DirectAccess unique à l'aide de l'Assistant Mise en route](../../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md). Ce scénario simple permet de configurer DirectAccess avec les paramètres par défaut en utilisant un Assistant, sans qu'il soit nécessaire de configurer des paramètres d'infrastructure, tels qu'une autorité de certification ou des groupes de sécurité Active Directory.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Pour configurer un serveur DirectAccess unique avec des paramètres avancés, vous devez effectuer plusieurs étapes de planification et déploiement.  
  
### <a name="prerequisites"></a>Prérequis  
Avant de commencer, vous pouvez passer en revue la configuration requise suivante.  
  
-   Le Pare-feu Windows doit être activé sur tous les profils.  
  
-   Le serveur DirectAccess est le serveur Emplacement réseau.  
  
-   Vous voulez que tous les ordinateurs sans fil figurant dans le domaine où vous installez le serveur DirectAccess prennent en charge DirectAccess. Quand vous déployez DirectAccess, il est activé automatiquement sur tous les ordinateurs portables présents dans le domaine actuel.  
  
> [!IMPORTANT]  
> Certaines technologies et configurations ne sont pas prises en charge lorsque vous déployez DirectAccess.  
>   
> -   Le protocole ISATAP (Intra-Site Automatic Tunnel Addressing Protocol) n'est pas pris en charge dans le réseau d'entreprise. Si vous utilisez le protocole ISATAP, vous devez le supprimer et utiliser le protocole IPv6 natif.  
  
### <a name="planning-steps"></a>Étapes de planification  
La planification comporte les deux phases suivantes :  
  
1.  **Planification de l'infrastructure DirectAccess**. Cette phase décrit la planification requise pour configurer l'infrastructure réseau avant de commencer le déploiement de DirectAccess. Elle inclut la planification de la topologie du réseau et des serveurs, la planification des certificats, de DNS, de la configuration d'Active Directory et des objets de stratégie de groupe (GPO), ainsi que du serveur Emplacement réseau DirectAccess.  
  
2.  **Planification du déploiement de DirectAccess**. Cette phase décrit les étapes de planification requises pour préparer le déploiement de DirectAccess. Elle inclut la planification des ordinateurs clients DirectAccess, de la configuration requise pour l'authentification des serveurs et clients, des paramètres VPN, des serveurs d'infrastructure, et des serveurs d'applications et d'administration.  
  
### <a name="deployment-steps"></a>Étapes de déploiement  
Le déploiement comporte les trois phases suivantes :  
  
1.  **Configuration de l'infrastructure DirectAccess**. Cette phase inclut la configuration du réseau et du routage, des paramètres de pare-feu (s'il y a lieu), des certificats, des serveurs DNS, des paramètres Active Directory et GPO, et du serveur Emplacement réseau DirectAccess.  
  
2.  **Configuration des paramètres du serveur DirectAccess**. Cette phase inclut les étapes requises pour configurer les ordinateurs clients DirectAccess, le serveur DirectAccess, les serveurs d'infrastructure et les serveurs d'applications et d'administration.  
  
3.  **Vérification du déploiement**. Cette phase inclut des étapes pour vérifier le déploiement de DirectAccess.  
  
Pour obtenir les étapes de déploiement détaillées, voir [Installer et configurer les fonctionnalités avancées de DirectAccess](../../../remote-access/directaccess/single-server-advanced/Install-and-Configure-Advanced-DirectAccess.md).  
  
## <a name="BKMK_APP"></a>Applications pratiques  
Le déploiement d'un serveur DirectAccess individuel présente les caractéristiques suivantes :  
  
-   **Options d'ergonomie**. Les ordinateurs clients gérés exécutant Windows 10, Windows 8.1, Windows 8 et Windows 7 peuvent être configurés en tant qu’ordinateurs clients DirectAccess. Ces clients peuvent accéder aux ressources réseau internes via DirectAccess chaque fois qu’ils se trouvent sur Internet sans avoir à se connecter via une connexion VPN. Les ordinateurs clients qui n’exécutent pas l’un de ces systèmes d’exploitation peuvent se connecter au réseau interne via VPN.  
  
-   **Gestion facile**. Les ordinateurs clients DirectAccess qui se trouvent sur Internet peuvent être gérés à distance par des administrateurs d'accès à distance via DirectAccess, même si ces ordinateurs ne figurent pas dans le réseau interne de l'entreprise. Les ordinateurs clients qui ne répondent pas aux spécifications de l’entreprise peuvent être automatiquement mis à jour par les serveurs d’administration. DirectAccess et VPN sont gérés dans la même console et avec le même jeu d’Assistants. En outre, un ou plusieurs serveurs DirectAccess peuvent être gérés à partir d'une seule console de gestion de l'accès à distance.  
  
## <a name="BKMK_NEW"></a>Rôles et fonctionnalités requis pour ce scénario  
Le tableau suivant répertorie les fonctionnalités et rôles requis pour ce scénario :  
  
|Rôle/fonctionnalité|Prise en charge de ce scénario|  
|---------|-----------------|  
|Rôle Accès à distance|Le rôle est installé et désinstallé à l’aide de la console du Gestionnaire de serveur ou de Windows PowerShell. Ce rôle englobe à la fois DirectAccess et le service de routage et d'accès à distance (RRAS). Le rôle Accès à distance est constitué de deux composants :<br/><br/>1.  VPN DirectAccess et RRAS. DirectAccess et VPN sont gérés ensemble dans la console de gestion de l’accès à distance.<br/>2.  Routage RRAS. Les fonctionnalités de routage RRAS sont gérées dans la console de routage et d’accès distant héritée.<br /><br />Le rôle Serveur d'accès à distance dépend des rôles/fonctionnalités de serveur suivants :<br/><br/> -Serveur Web Internet Information Services (IIS) : cette fonctionnalité est requise pour configurer le serveur emplacement réseau sur le serveur DirectAccess, ainsi que la sonde Web par défaut.<br/> -Base de données interne Windows. Utilisé pour la gestion de comptes locale sur le serveur DirectAccess.|  
|Fonctionnalité des outils de gestion de l’accès à distance|Cette fonctionnalité est installée comme suit :<br /><br />-Elle est installée par défaut sur un serveur DirectAccess lorsque le rôle accès à distance est installé et prend en charge l’interface utilisateur de la console de gestion à distance et les applets de commande Windows PowerShell.<br />-Il peut éventuellement être installé sur un serveur qui n’exécute pas le rôle serveur DirectAccess. Dans ce cas, elle est utilisée pour la gestion à distance d’un ordinateur d’accès à distance qui exécute DirectAccess et le réseau privé virtuel (VPN).<br /><br />La fonctionnalité des outils de gestion de l’accès à distance est constituée des éléments suivants :<br /><br />-Interface utilisateur graphique (GUI) d’accès à distance<br />-Module d’accès à distance pour Windows PowerShell<br /><br />Les dépendances incluent :<br /><br />-Console de gestion des stratégies de groupe<br />-Kit d’administration du gestionnaire des connexions (CMAK) RAS<br />-Windows PowerShell 3,0<br />-Outils et infrastructure de gestion graphique|  
  
## <a name="BKMK_HARD"></a>Configuration matérielle requise  
La configuration matérielle requise pour ce scénario comprend les éléments suivants :  
  
-   Configuration requise du serveur :  
  
    -   Un ordinateur qui répond à la configuration matérielle requise pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
    -   Sur le serveur, au moins une carte réseau doit être installée, activée et connectée au réseau interne. Lorsque deux cartes sont utilisées, une carte doit être connectée au réseau interne de l’entreprise et l’autre carte doit être connectée au réseau externe (Internet ou un réseau privé).  
  
    -   Si le protocole Teredo est requis pour la transition d’IPv4 vers IPv6, la carte externe du serveur nécessite deux adresses IPv4 publiques consécutives. Si une seule adresse IP est disponible, seul le protocole IP-HTTPS peut être utilisé pour la transition.  
  
    -   Au moins un contrôleur de domaine. Le serveur et les clients DirectAccess doivent être membres d'un domaine.  
  
    -   Une autorité de certification est requise si vous ne souhaitez pas utiliser de certificats auto-signés pour IP-HTTPS ou le serveur Emplacement réseau, ou si vous souhaitez utiliser des certificats clients pour l'authentification IPsec des clients. Vous pouvez aussi demander des certificats à une autorité de certification publique.  
  
    -   Si le serveur Emplacement réseau ne se trouve pas sur le serveur DirectAccess, il est nécessaire d'utiliser un serveur web distinct pour l'exécuter.  
  
-   Configuration requise du client :  
  
    -   Un ordinateur client doit exécuter Windows 10, Windows 8 ou Windows 7.  
  
        > [!NOTE]  
        > Les systèmes d’exploitation suivants peuvent être utilisés en tant que clients DirectAccess : Windows 10, Windows Server 2012 R2, Windows Server 2012, Windows 8 entreprise, Windows 7 entreprise ou Windows 7 édition intégrale.  
  
-   Configuration requise en termes d’infrastructure et de serveurs d’administration :  
  
    -   Lors de la gestion à distance des ordinateurs clients DirectAccess, les clients initient des communications avec des serveurs d’administration tels que les contrôleurs de domaine, les serveurs System Center Configuration et les serveurs d’autorité HRA (Health Registration Authority) pour des services incluant les mises à jour Windows et d’antivirus, ainsi que la mise en conformité des clients de protection d’accès réseau. Les serveurs requis doivent être déployés préalablement au déploiement de l’accès à distance.  
  
    -   Si l'accès à distance nécessite la conformité des clients de protection d'accès réseau, les serveurs NPS et HRS doivent être déployés préalablement au déploiement de l'accès à distance.  
  
    -   Lorsque le réseau VPN est activé, un serveur DHCP est requis pour allouer automatiquement des adresses IP aux clients VPN si un pool d’adresses statiques n’est pas utilisé.  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
Plusieurs conditions sont requises pour ce scénario :  
  
-   Configuration requise du serveur :  
  
    -   Le serveur DirectAccess doit être membre d'un domaine. Le serveur peut être déployé en périphérie du réseau interne ou derrière un pare-feu de périmètre ou un autre périphérique.  
  
    -   Si le serveur DirectAccess se trouve derrière un pare-feu de périmètre ou un périphérique NAT, ce périphérique doit être configuré de manière à autoriser le trafic en direction et en provenance du serveur DirectAccess.  
  
    -   La personne qui déploie l’accès à distance sur le serveur doit disposer des autorisations d’administrateur local sur le serveur, ainsi que des autorisations d’utilisateur de domaine. L’administrateur doit également disposer des autorisations pour les objets de stratégie de groupe utilisés dans le déploiement de DirectAccess. L’utilisation des fonctionnalités qui limitent le déploiement de DirectAccess sur les ordinateurs portables uniquement nécessite de disposer des autorisations permettant de créer un filtre WMI sur le contrôleur de domaine.  
  
-   Configuration requise pour les clients d’accès à distance :  
  
    -   Les clients DirectAccess doivent appartenir au domaine. Les domaines contenant des clients peuvent appartenir à la même forêt que le serveur DirectAccess ou ils peuvent avoir une relation d'approbation bidirectionnelle avec la forêt ou le domaine du serveur DirectAccess.  
  
    -   Un groupe de sécurité Active Directory est requis afin de contenir les ordinateurs qui seront configurés en tant que clients DirectAccess. Si aucun groupe de sécurité n’est spécifié lors de la configuration des paramètres des clients DirectAccess, le client GPO est appliqué par défaut sur tous les ordinateurs portables inclus dans le groupe de sécurité Ordinateurs du domaine.  
  
        > [!NOTE]  
        > Nous vous recommandons de créer un groupe de sécurité pour chaque domaine qui contient des ordinateurs clients DirectAccess.  
  
        > [!IMPORTANT]  
        > Si vous avez activé Teredo dans votre déploiement DirectAccess et que vous souhaitez fournir l’accès aux clients Windows 7, assurez-vous que les clients sont mis à niveau vers Windows 7 avec SP1. Les clients qui utilisent Windows 7 RTM ne pourront pas se connecter via Teredo. Toutefois, ces clients seront en mesure de se connecter au réseau d'entreprise via IP-HTTPS.  
  
## <a name="BKMK_LINKS"></a>Voir aussi  
Le tableau suivant fournit des liens vers des ressources supplémentaires.  
  
|Type de contenu|Références|  
|--------|-------|  
|**Déploiement**|[Chemins de déploiement de DirectAccess dans Windows Server](../../../remote-access/directaccess/DirectAccess-Deployment-Paths-in-Windows-Server.md)<br /><br />[Déployer un serveur DirectAccess unique à l’aide de l’Assistant Prise en main](../../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|  
|**Outils et paramètres**|[Applets de commande PowerShell pour l’accès à distance](https://technet.microsoft.com/library/hh918399.aspx)|  
|**Ressources de la communauté**|[Guide de survie DirectAccess](https://social.technet.microsoft.com/wiki/contents/articles/23210.directaccess-survival-guide.aspx)<br /><br />[Entrées wiki DirectAccess](https://go.microsoft.com/fwlink/?LinkId=236871)|  
|**Technologies connexes**|[Fonctionnement d’IPv6](https://technet.microsoft.com/library/cc781672(v=WS.10).aspx)|  
  


