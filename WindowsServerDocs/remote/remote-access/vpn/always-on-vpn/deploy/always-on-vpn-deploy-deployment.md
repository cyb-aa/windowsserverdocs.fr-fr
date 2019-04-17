---
title: Déployer VPN Toujours actif (AlwaysOn)
description: Cette rubrique fournit des instructions détaillées pour le déploiement de toujours actif (AlwaysOn) dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15c60f2986d3837c5c6e03f9e0a25c7e0a4e0cc0
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067132"
---
# Déployer VPN Toujours actif (AlwaysOn)

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #0171; [ **Précédent:** obtenir des informations sur la toujours actif (AlwaysOn) les fonctionnalités avancées](always-on-vpn-adv-options.md)<br>
& #0187; [ **Suivant:** étape 1. Commencer à planifier le déploiement de toujours actif (AlwaysOn)](always-on-vpn-deploy-planning.md)

Dans cette section, vous en savoir plus sur le flux de travail pour le déploiement de connexions toujours actif (AlwaysOn) pour les ordinateurs de clients Windows 10 joints au domaine à distance. Si vous souhaitez **configurer l’accès conditionnel** ajuster la façon dont les utilisateurs VPN accèdent à vos ressources, voir [l’accès conditionnel pour une connexion VPN à l’aide d’Azure AD](../../ad-ca-vpn-connectivity-windows10.md). Pour plus d’informations sur l’accès conditionnel pour une connexion VPN à l’aide d’Azure AD, voir [l’accès conditionnel dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). 


Le diagramme suivant illustre le processus de workflow pour les différents scénarios lors du déploiement toujours actif (AlwaysOn): 

_Cliquez sur l’image pour agrandir l’image_.

<a href="../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow.png" alt="Full-sized view of the Always On VPN deployment workflow" target="_blank">![thumbnail](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)
</a> 

>[!IMPORTANT]
>Pour ce déploiement, il n’est pas obligatoire que vos serveurs d’infrastructure, tels que les ordinateurs exécutant les Services de domaine Active Directory, Services de certificats Active Directory et Network Policy Server, exécutez Windows Server 2016. Vous pouvez utiliser les versions antérieures de Windows Server, par exemple, Windows Server 2012 R2, pour les serveurs d’infrastructure et pour le serveur qui exécute l’accès à distance.

## [Étape1. Planifier le déploiement de VPN Toujours actif (AlwaysOn)](always-on-vpn-deploy-planning.md)

Dans cette étape, vous commencez à planifier et préparer votre déploiement toujours actif (AlwaysOn). Avant d’installer le rôle de serveur d’accès à distance sur l’ordinateur que vous avez l’intention d’utiliser comme serveur VPN. Après une planification appropriée, vous pouvez déployer VPN Toujours actif (AlwaysOn) et éventuellement configurer l’accès conditionnel pour une connectivité VPN à l’aide d’Azure AD.

## [Étape2. Configurer l'infrastructure des serveurs VPN Toujours actif (AlwaysOn)](vpn-deploy-server-infrastructure.md)

Dans cette étape, vous installez et configurez les composants côté serveur nécessaires pour prendre en charge de la connexion VPN. Les composants côté serveur incluent la configuration d’infrastructure à clé publique pour distribuer les certificats utilisés par les utilisateurs, le serveur VPN et le serveur NPS.  Vous configurez également RRAS pour prendre en charge les connexions de protocole IKEv2 et le serveur NPS pour exécuter une autorisation pour des connexions VPN.

Pour configurer l’infrastructure de serveur, vous devez effectuer les tâches suivantes:
- **Sur un serveur configuré avec les Services de domaine Active Directory:** Activer l’inscription automatique de certificat dans la stratégie de groupe pour les ordinateurs et les utilisateurs, créer le groupe d’utilisateurs VPN, le groupe de serveurs VPN et le groupe de serveurs NPS et ajouter des membres à chaque groupe.
- **Sur un serveur de certificats Active Directory autorité de certification:** Créez les modèles de certificats d’authentification utilisateur, l’authentification du serveur VPN et l’authentification du serveur NPS.
- **Sur les clients de Windows 10 joints au domaine:** S’inscrire et valider des certificats d’utilisateur.

## [Étape3. Configurer le serveur d'accès à distance pour VPN Toujours actif (AlwaysOn)](vpn-deploy-ras.md)

Dans cette étape, vous configurez un VPN accès à distance pour autoriser les connexions VPN IKEv2, refuser les connexions à partir d’autres protocoles VPN et affectez un pool d’adresses IP statiques pour l’émission d’adresses IP à la connexion des clients VPN autorisés.

Pour configurer l’accès distant, vous devez effectuer les tâches suivantes:
- S’inscrire et valider le certificat du serveur VPN
- Installer et configurer l’accès à distance VPN

## [Étape4. Installer et configurer le serveur NPS](vpn-deploy-nps.md)

Dans cette étape, vous installez le serveur NPS (Network Policy Server) à l’aide de Windows PowerShell ou l’Assistant de fonctionnalités et un serveur gestionnaire ajouter des rôles. Vous configurez également NPS pour gérer tous les d’authentification, l’autorisation et les droits de gestion des comptes de demande de connexion qu’il reçoit à partir du serveur VPN.

Pour configurer un serveur NPS, vous devez effectuer les tâches suivantes:
- Inscrire le serveur NPS dans Active Directory
- Configurer RADIUS prise en compte pour votre serveur NPS
- Ajoutez le serveur VPN en tant que Client RADIUS dans NPS
- Configurer la stratégie réseau dans NPS
- Inscription automatique du certificat de serveur NPS

## [Étape5. Configurer DNS et des paramètres de pare-feu pour toujours sur VPN](vpn-deploy-dns-firewall.md)

Dans cette étape, vous configurez DNS et le pare-feu paramètres. Lorsque les clients VPN distants connectés, ils utilisent les mêmes serveurs DNS par vos clients internes, ce qui leur permet de résoudre les noms de la même manière que le reste de vos stations de travail internes. 

## [Étape6. Configurer les connexions VPN Toujours actif (AlwaysOn) du client Windows10](vpn-deploy-client-vpn-connections.md)

Dans cette étape, vous configurez les ordinateurs clients de Windows 10 pour communiquer avec cette infrastructure avec une connexion VPN. Vous pouvez utiliser plusieurs technologies pour configurer des clients VPN Windows 10, y compris Windows PowerShell, System Center Configuration Manager et Intune. Les trois nécessitent un profil VPN XML pour configurer les paramètres VPN appropriés. 

## [Étape7. (Facultatif) Configurer l’accès conditionnel pour une connexion VPN](../../ad-ca-vpn-connectivity-windows10.md) 
Cette étape facultative, vous pouvez affiner VPN autorisé comment les utilisateurs accéder à vos ressources. Avec l’accès conditionnel à Azure AD pour une connexion VPN, vous pouvez protéger les connexions VPN. Accès conditionnel est un moteur d’évaluation basé sur une stratégie qui vous permet de créer des règles d’accès pour n’importe quel Azure AD application connectée. Pour plus d’informations, voir [l’accès conditionnel Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).


## Étape suivante
[Étape 1. Planifier le déploiement de toujours actif (AlwaysOn)](always-on-vpn-deploy-planning.md): avant d’installer le rôle de serveur d’accès à distance sur l’ordinateur que vous avez l’intention d’utiliser comme serveur VPN. Après une planification appropriée, vous pouvez déployer VPN Toujours actif (AlwaysOn) et éventuellement configurer l’accès conditionnel pour une connectivité VPN à l’aide d’Azure AD.  



---