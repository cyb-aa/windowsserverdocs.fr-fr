---
title: Déployer des certificats de serveur pour des déploiements câblés et sans fil 802.1X
description: Cette rubrique fait partie du guide des certificats de serveur de déploiement pour les déploiements de sans fil et câblé à 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2d4cdcd11e0eb334064ddefec0eda775ffccff2c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446468"
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Déployer des certificats de serveur pour des déploiements câblés et sans fil 802.1X

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser ce guide pour déployer des certificats de serveur sur vos serveurs d’infrastructure accès à distance et serveur NPS (Network Policy Server).   

Ce guide contient les sections suivantes.  

-   [Conditions préalables pour l’utilisation de ce guide](#bkmk_pre)  

-   [Ce que ce guide ne contient pas](#bkmk_not)  

-   [Présentations des technologies](#bkmk_tech)  

-   [Présentation du déploiement de certificat de serveur](Server-Certificate-Deployment-Overview.md)  

-   [Planification du déploiement de certificats de serveur](Server-Certificate-Deployment-Planning.md)  

-   [Déploiement de certificats de serveur](Server-Certificate-Deployment.md)  

### <a name="digital-server-certificates"></a>**Certificats de serveur numériques**  
Ce guide fournit des instructions sur l’utilisation des Services de certificats Active Directory (AD CS) pour inscrire automatiquement des certificats pour l’accès à distance et les serveurs d’infrastructure de serveur NPS. Les services AD CS vous permet de générer une infrastructure à clé publique (PKI) et de fournir un chiffrement à clé publique, les certificats numériques et les fonctions de signature numérique pour votre organisation.  

Lorsque vous utilisez des certificats de serveur numériques pour l’authentification entre les ordinateurs de votre réseau, les certificats fournissent :   

1. Confidentialité grâce au cryptage.  
2. Intégrité via des signatures numériques.  
3. Authentification en associant des clés de certificat à des comptes utilisateur, ordinateur ou un périphérique sur un réseau d’ordinateurs.  

### <a name="server-types"></a>**Types de serveur**  
À l’aide de ce guide, vous pouvez déployer des certificats de serveur pour les types suivants de serveurs.  
- Les serveurs qui exécutent le service d’accès à distance, DirectAccess ou les serveurs de réseau privé virtuel (VPN) standard, et qui sont membres de la **serveurs RAS et IAS** groupe.  
- Les serveurs qui exécutent le service de serveur NPS (Network Policy Server) qui sont membres de la **serveurs RAS et IAS** groupe.  

### <a name="advantages-of-certificate-autoenrollment"></a>**Avantages de l’inscription automatique de certificat**  
L’inscription automatique des certificats de serveur, également appelé l’inscription automatique, offre les avantages suivants.  

- L’autorité de certification (CA) les services AD CS inscrit automatiquement un certificat de serveur à tous les serveurs NPS et accès à distance.  
- Tous les ordinateurs du domaine reçoivent automatiquement votre certificat d’autorité de certification, qui est installé dans les autorités de Certification racine approuvées stocker sur chaque ordinateur membre du domaine. Pour cette raison, tous les ordinateurs dans le domaine d’approuvent les certificats émis par votre autorité de certification. Cette approbation permet à vos serveurs d’authentification à prouver leur identité entre eux et de s’engager dans des communications sécurisées.  
- Autre que l’actualisation de stratégie de groupe, la reconfiguration manuelle de chaque serveur n’est pas nécessaire.  
- Chaque certificat de serveur inclut l’objectif de l’authentification du serveur et l’objectif de l’authentification du Client dans les extensions de l’utilisation de clé améliorée (EKU).  
- Extensibilité. Après avoir déployé votre autorité de certification racine d’entreprise grâce à ce guide, vous pouvez développer votre infrastructure à clé publique (PKI) en ajoutant les autorités de certification Enterprise.  
- Facilité de gestion. Vous pouvez gérer les services AD CS à l’aide de la console AD CS ou à l’aide de scripts et les commandes Windows PowerShell.  
- Simplicité. Vous spécifiez les serveurs qui inscrivent des certificats de serveur à l’aide de comptes de groupe Active Directory et l’appartenance au groupe.   
- Lorsque vous déployez des certificats de serveur, les certificats sont basés sur un modèle que vous configurez les instructions de ce guide. Cela signifie que vous pouvez personnaliser les modèles de certificats différents pour des types de serveurs, ou vous pouvez utiliser le même modèle pour tous les certificats de serveur que vous souhaitez émettre.  

## <a name="bkmk_pre"></a>Conditions préalables pour l’utilisation de ce guide  

Ce guide fournit des instructions sur la façon de déployer des certificats de serveur à l’aide des services AD CS et le rôle de serveur serveur Web (IIS) dans Windows Server 2016. Voici la configuration requise pour exécuter les procédures décrites dans ce guide.  

- Vous devez déployer un réseau de base à l’aide du Guide du réseau Windows Server 2016 Core, ou vous devez déjà disposer les technologies fournies dans le Guide du réseau installé et fonctionne correctement sur votre réseau. Ces technologies incluent TCP/IP v4, DHCP, Active Directory domaine Services (AD DS), DNS et NPS.  
  >[!NOTE]
  >Le Guide du réseau Windows Server 2016 Core est disponible dans la bibliothèque technique Windows Server 2016. Pour plus d’informations, consultez [Guide du réseau](../../../core-network-guide/Core-Network-Guide.md).

- Vous devez lire la section de planification de ce guide pour vous assurer que vous êtes prêt pour ce déploiement avant d’effectuer le déploiement.  
- Vous devez effectuer les étapes de ce guide dans l’ordre dans lequel elles sont présentées. Ne pas passer directement et déployer votre autorité de certification sans les étapes conduisant à déployer le serveur ou votre déploiement échoue.  
- Vous devez être prêt à déployer deux serveurs sur votre réseau - un serveur sur lequel vous allez installer les services AD CS en tant qu’une autorité de certification racine d’entreprise et un serveur sur lequel vous installerez le serveur Web (IIS) afin que votre autorité de certification peut publier la liste de révocation de certificats (CRL) sur la Web se m.   

>[!NOTE]  
>Vous êtes prêt à affecter une adresse IP statique pour les serveurs Web et les services AD CS que vous déployez avec ce guide, ainsi que pour nommer les ordinateurs en fonction de vos conventions d’affectation de noms d’organisation. En outre, vous devez joindre les ordinateurs à votre domaine.  

## <a name="bkmk_not"></a>Ce que ce guide ne fournit pas  
Ce guide ne fournit pas d’obtenir des instructions complètes pour concevoir et déployer une infrastructure à clé publique (PKI) à l’aide des services AD CS. Il est recommandé de consulter la documentation des services AD CS et la documentation de conception d’infrastructure à clé publique avant de déployer les technologies de ce guide.   

## <a name="bkmk_tech"></a>Présentations des technologies  
Voici des présentations des technologies pour les services AD CS et serveur Web (IIS).  

### <a name="active-directory-certificate-services"></a>Services de certificats Active Directory  
Les services AD CS dans Windows Server 2016 fournit des services personnalisables pour créer et gérer les certificats X.509 utilisés dans les systèmes de sécurité logiciels employant des technologies de clé publique. Les organisations peuvent utiliser AD CS pour améliorer la sécurité en liant l’identité d’une personne, un appareil ou un service à une clé publique correspondante. Les services AD CS comprend également des fonctionnalités qui vous permettent de gérer l’inscription de certificats et la révocation d’une variété d’environnements évolutifs.  

Pour plus d’informations, consultez [présentation des Services de certificats Active Directory](https://technet.microsoft.com/library/hh831740.aspx) et [Guide de conception d’Infrastructure de clé publique](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).  

### <a name="web-server-iis"></a>Serveur Web (IIS)  

Le rôle de serveur Web (IIS) dans Windows Server 2016 fournit une plate-forme sécurisée, facile à gérer, modulaire et extensible pour héberger des sites Web, services et applications de manière fiable. Avec IIS, vous pouvez partager des informations avec des utilisateurs sur Internet, un intranet ou un extranet. IIS est une plateforme web unifiée qui intègre IIS, ASP.NET, les services FTP, PHP et Windows Communication Foundation (WCF).  

Pour plus d’informations, consultez [vue d’ensemble du serveur Web (IIS)](https://technet.microsoft.com/library/hh831725.aspx).  
