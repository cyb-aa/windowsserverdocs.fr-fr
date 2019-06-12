---
title: Déployer VPN Toujours actif (AlwaysOn)
description: Cette rubrique fournit des instructions détaillées pour le déploiement VPN Always On dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6849d8547885b10fb32a133f09a82b6f133a535e
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749661"
---
# <a name="deploy-always-on-vpn"></a>Déployer VPN Toujours actif (AlwaysOn)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** En savoir plus sur le VPN Always On fonctionnalités avancées](always-on-vpn-adv-options.md)
- [**prochain :** Étape 1. Commencez à planifier le déploiement VPN Always On](always-on-vpn-deploy-planning.md)

Dans cette section, vous en savoir plus sur le flux de travail pour le déploiement de connexions VPN Always On pour les ordinateurs de clients Windows 10 joints au domaine distants. Si vous souhaitez **configurer l’accès conditionnel** pour affiner la façon dont les utilisateurs VPN accéder à vos ressources, consultez [l’accès conditionnel pour la connectivité VPN à l’aide d’Azure AD](../../ad-ca-vpn-connectivity-windows10.md). Pour en savoir plus sur l’accès conditionnel pour la connectivité VPN à l’aide d’Azure AD, consultez [l’accès conditionnel dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). 

Le diagramme suivant illustre le processus de flux de travail pour les différents scénarios lors du déploiement de VPN Always On :

![Organigramme du flux de travail du déploiement VPN Always On](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)

>[!IMPORTANT]
>Pour ce déploiement, il n’est pas obligatoire que vos serveurs d’infrastructure, tels que des ordinateurs exécutant les Services de domaine Active Directory, Services de certificats Active Directory et serveur NPS, exécutent Windows Server 2016. Vous pouvez utiliser les versions antérieures de Windows Server, tels que Windows Server 2012 R2, pour les serveurs d’infrastructure et pour le serveur qui a accès à distance est en cours d’exécution.

## <a name="step-1-plan-the-always-on-vpn-deploymentalways-on-vpn-deploy-planningmd"></a>[Étape 1. Planifier le déploiement de VPN Toujours actif (AlwaysOn)](always-on-vpn-deploy-planning.md)

Dans cette étape, vous commencer à planifier et préparer votre déploiement VPN Always On. Avant d’installer le rôle de serveur d’accès à distance sur l’ordinateur que vous avez l’intention d’utiliser comme un serveur VPN. Après une planification appropriée, vous pouvez déployer VPN Toujours actif (AlwaysOn) et éventuellement configurer l’accès conditionnel pour une connectivité VPN à l’aide d’Azure AD.

## <a name="step-2-configure-the-always-on-vpn-server-infrastructurevpn-deploy-server-infrastructuremd"></a>[Étape 2. Configurer l’infrastructure des serveurs VPN Toujours actif (AlwaysOn)](vpn-deploy-server-infrastructure.md)

Dans cette étape, vous installez et configurez les composants côté serveur nécessaires pour prendre en charge de la connexion VPN. Les composants côté serveur incluent la configuration d’infrastructure à clé publique pour distribuer les certificats utilisés par les utilisateurs, le serveur VPN et le serveur NPS.  Vous également configurer RRAS pour prendre en charge les connexions IKEv2 et le serveur NPS pour gérer l’autorisation pour les connexions VPN.

Pour configurer l’infrastructure de serveur, vous devez effectuer les tâches suivantes :

- **Sur un serveur configuré avec les Services de domaine Active Directory :** Activer l’inscription automatique de certificat dans la stratégie de groupe pour les ordinateurs et les utilisateurs, créez le groupe d’utilisateurs de VPN, le groupe de serveurs VPN et le groupe de serveurs NPS et ajouter des membres à chaque groupe.
- **Sur une autorité de certification de serveur de certificats Active Directory :** Créer les modèles de certificats d’authentification utilisateur, l’authentification du serveur VPN et l’authentification du serveur NPS.
- **Sur les clients Windows 10 joints au domaine :** Inscrire et valider les certificats d’utilisateur.

## <a name="step-3-configure-the-remote-access-server-for-always-on-vpnvpn-deploy-rasmd"></a>[Étape 3. Configurer le serveur d’accès à distance pour VPN Toujours actif (AlwaysOn)](vpn-deploy-ras.md)

Dans cette étape, vous configurez les VPN d’accès distant pour autoriser les connexions VPN IKEv2, refuser les connexions à partir d’autres protocoles VPN et affecter un pool d’adresses IP statiques pour l’émission d’adresses IP à la connexion des clients VPN autorisés.

Pour configurer l’accès distant, vous devez effectuer les tâches suivantes :

- Inscrire et de valider le certificat de serveur VPN
- Installer et configurer l’accès à distance VPN

## <a name="step-4-install-and-configure-the-nps-servervpn-deploy-npsmd"></a>[Étape 4. Installer et configurer le serveur NPS](vpn-deploy-nps.md)

Dans cette étape, vous installez le serveur NPS (Network Policy Server) à l’aide de Windows PowerShell ou l’Assistant de fonctionnalités et un gestionnaire serveur ajouter des rôles. Vous configurez également NPS pour gérer tous les authentification, autorisation et les droits de gestion des comptes pour la demande de connexion qu’il reçoit à partir du serveur VPN.

Pour configurer NPS, vous devez effectuer les tâches suivantes :

- Inscrire le serveur NPS dans Active Directory
- Configurer la gestion de comptes pour votre serveur NPS RADIUS
- Ajouter le serveur VPN en tant que Client RADIUS dans NPS
- Configurer la stratégie réseau dans NPS
- Inscrire automatiquement le certificat de serveur NPS

## <a name="step-5-configure-dns-and-firewall-settings-for-always-on-vpnvpn-deploy-dns-firewallmd"></a>[Étape 5. Configurer DNS et les paramètres de pare-feu pour toujours sur VPN](vpn-deploy-dns-firewall.md)

Dans cette étape, vous configurez DNS et pare-feu paramètres. Lorsque les clients VPN distants se connectent, ils utilisent les mêmes serveurs DNS par vos clients internes, ce qui leur permet de résoudre les noms de la même manière que le reste de vos postes de travail internes. 

## <a name="step-6-configure-windows-10-client-always-on-vpn-connectionsvpn-deploy-client-vpn-connectionsmd"></a>[Étape 6. Configurer les connexions VPN Toujours actif (AlwaysOn) du client Windows 10](vpn-deploy-client-vpn-connections.md)

Dans cette étape, vous configurez les ordinateurs clients Windows 10 pour communiquer avec cette infrastructure avec une connexion VPN. Vous pouvez utiliser plusieurs technologies pour configurer les clients VPN Windows 10, y compris Windows PowerShell, System Center Configuration Manager et Intune. Les trois nécessitent un profil VPN XML pour configurer les paramètres VPN appropriés.

## <a name="step-7-optional-configure-conditional-access-for-vpn-connectivityad-ca-vpn-connectivity-windows10md"></a>[Étape 7. (Facultatif) Configurer l’accès conditionnel pour la connectivité VPN](../../ad-ca-vpn-connectivity-windows10.md)

Dans cette étape facultative, vous pouvez affiner VPN autorisé comment les utilisateurs accèdent à vos ressources. Accès conditionnel Azure AD pour la connectivité VPN, vous pouvez protéger les connexions VPN. Accès conditionnel est un moteur d’évaluation basée sur des stratégies qui vous permet de créer des règles d’accès pour n’importe quel Azure AD connecté d’application. Pour plus d’informations, consultez [accès conditionnel Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

## <a name="next-step"></a>Étape suivante

[Étape 1. Planifier le déploiement VPN Always On](always-on-vpn-deploy-planning.md): Avant d’installer le rôle de serveur d’accès à distance sur l’ordinateur que vous avez l’intention d’utiliser comme un serveur VPN. Après une planification appropriée, vous pouvez déployer VPN Toujours actif (AlwaysOn) et éventuellement configurer l’accès conditionnel pour une connectivité VPN à l’aide d’Azure AD.  
