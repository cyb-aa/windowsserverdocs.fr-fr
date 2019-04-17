---
title: Déployer des certificats de serveur pour 802. 1 X câblés et sans fil des déploiements
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fda9b2b53d088cc389e98cf96fecd748419bc6e2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Déployer des certificats de serveur pour 802. 1 X câblés et sans fil des déploiements

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser ce guide pour déployer des certificats de serveur sur vos serveurs d’infrastructure accès à distance et le serveur NPS (Network Policy).   

Ce guide contient les sections suivantes.  

-   [Configuration requise pour l’utilisation de ce guide](#bkmk_pre)  

-   [Ce que ce guide ne fournit pas](#bkmk_not)  

-   [Vues d’ensemble de la technologie](#bkmk_tech)  

-   [Présentation du déploiement de certificat de serveur](Server-Certificate-Deployment-Overview.md)  

-   [Planification du déploiement de certificats de serveur](Server-Certificate-Deployment-Planning.md)  

-   [Déploiement de certificats de serveur](Server-Certificate-Deployment.md)  

### **<a name="digital-server-certificates"></a>Certificats de serveur numérique**  
Ce guide fournit des instructions d’utilisation des Services de certificats ActiveDirectory (ADCS) pour inscrire automatiquement des certificats pour l’accès à distance et les serveurs d’infrastructure NPS. Les services ADCS vous permet de générer une infrastructure à clé publique (PKI) et de fournir un chiffrement à clé publique, des certificats numériques et des fonctions de signature pour votre organisation.  

Lorsque vous utilisez des certificats de serveur numériques pour l’authentification entre les ordinateurs sur votre réseau, les certificats fournissent:   

1. Confidentialité par le chiffrement.  
2. Intégrité par le biais de signatures numériques.  
3. Authentification en associant des clés de certificat avec des comptes d’ordinateur, un utilisateur ou périphérique sur un réseau d’ordinateurs.  

### **<a name="server-types"></a>Types de serveurs**  
À l’aide de ce guide, vous pouvez déployer des certificats de serveur pour les types suivants de serveurs.  
- Serveurs qui exécutent le service d’accès à distance, DirectAccess ou les serveurs de réseau privé virtuel (VPN) standard, et qui sont membres de la **serveurs RAS et IAS** groupe.  
- Les serveurs qui exécutent le service de serveur NPS (Network Policy) qui sont membres de la **serveurs RAS et IAS** groupe.  

### **<a name="advantages-of-certificate-autoenrollment"></a>Avantages de l’inscription automatique de certificat**  
L’inscription automatique de certificats de serveur, également appelé l’inscription automatique, offre les avantages suivants.  

- L’autorité de certification ADCS (CA) inscrit automatiquement un certificat de serveur pour tous les serveurs NPS et accès à distance.  
- Tous les ordinateurs du domaine reçoivent automatiquement votre certificat d’autorité de certification, qui est installé dans les autorités de Certification racine de confiance stocker sur chaque ordinateur membre du domaine. Pour cette raison, tous les ordinateurs dans le domaine approuvent les certificats émis par votre autorité de certification. Cette approbation permet à vos serveurs d’authentification prouver leur identité entre eux et participer à des communications sécurisées.  
- Autres que l’actualisation de la stratégie de groupe, la reconfiguration manuelle de chaque serveur n’est pas nécessaire.  
- Chaque certificat de serveur inclut le rôle d’authentification du serveur et l’objectif de l’authentification du Client dans les extensions d’utilisation de clé améliorée (EKU).  
- Évolutivité. Après avoir déployé votre autorité de certification racine d’entreprise dans ce guide, vous pouvez développer votre infrastructure à clé publique (PKI) en ajoutant des autorités de certification secondaire d’entreprise.  
- Facilité de gestion. Vous pouvez gérer les services ADCS à l’aide de la console des services ADCS ou à l’aide de scripts et les commandes Windows PowerShell.  
- Simplicité. Vous spécifiez les serveurs qui inscrivent des certificats de serveur à l’aide des comptes de groupes ActiveDirectory et l’appartenance au groupe.   
- Lorsque vous déployez des certificats de serveur, les certificats sont basés sur un modèle que vous configurez les instructions de ce guide. Cela signifie que vous pouvez personnaliser les modèles de certificats différents pour des types de serveurs, ou vous pouvez utiliser le même modèle pour tous les certificats de serveur que vous souhaitez émettre.  

## <a name="bkmk_pre"></a>Configuration requise pour l’utilisation de ce guide  

Ce guide fournit des instructions sur la façon de déployer des certificats de serveur à l’aide des services ADCS et le rôle serveur serveur Web (IIS) dans Windows Server2016. Voici la configuration requise pour exécuter les procédures de ce guide.  

- Vous devez déployer un réseau de base à l’aide du Guide du réseau Windows Server2016 Core, ou vous devez avoir déjà les technologies fournies dans le Guide du réseau de base installé et fonctionne correctement sur votre réseau. Ces technologies incluent TCP/IP v4, DHCP, Services ActiveDirectory domaine (ADDS), DNS et NPS.  
>[!NOTE]
>Le Guide du réseau Windows Server2016 Core est disponible dans la bibliothèque technique Windows Server2016. Pour plus d’informations, voir [Guide réseau de base](../../../core-network-guide/Core-Network-Guide.md).

- Vous devez lire la section de planification de ce guide pour vous assurer que vous êtes prêt pour ce déploiement avant d’effectuer le déploiement.  
- Vous devez effectuer les étapes de ce guide dans l’ordre dans lequel elles sont présentées. Ne pas sauter et déployer votre autorité de certification sans effectuer les étapes qui conduisent au déploiement du serveur ou votre déploiement échouera.  
- Vous devez être prêt à déployer deux nouveaux serveurs sur votre réseau - un serveur sur lequel vous installez les services ADCS en tant qu’une autorité de certification racine d’entreprise et un serveur sur lequel vous allez installer le serveur Web (IIS) afin que votre autorité de certification peut publier la liste de révocation de certificats (CRL) sur le serveur Web.   

>[!NOTE]  
>Vous êtes prêt à affecter une adresse IP statique pour les serveurs Web et les services ADCS que vous déployez avec ce guide, ainsi que pour les ordinateurs en fonction de votre organisation conventions d’affectation de nom. En outre, vous devez joindre les ordinateurs de votre domaine.  

## <a name="bkmk_not"></a>Ce que ce guide ne fournit pas  
Ce guide ne fournit pas d’instructions complètes pour concevoir et déployer une infrastructure à clé publique (PKI) à l’aide des services ADCS. Il est recommandé de consulter la documentation ADCS et la documentation de conception d’infrastructure à clé publique avant de déployer les technologies de ce guide.   

## <a name="bkmk_tech"></a>Vues d’ensemble de la technologie  
Voici les vues d’ensemble de la technologie pour les services ADCS et de serveur Web (IIS).  

### <a name="active-directory-certificate-services"></a>Services de certificats ActiveDirectory  
Les services ADCS dans Windows Server2016des services personnalisables pour créer et gérer les certificats X.509 qui sont utilisés dans les systèmes de sécurité logiciels employant des technologies de clé publiques. Organisations peuvent utiliser les services ADCS pour améliorer la sécurité en liant l’identité d’une personne, un périphérique ou un service à une clé publique correspondante. Les services ADCS comprend également des fonctionnalités qui vous permettent de gérer l’inscription de certificats et révocation dans une variété d’environnements évolutives.  

Pour plus d’informations, voir [présentation des Services de certificats ActiveDirectory](https://technet.microsoft.com/library/hh831740.aspx) et [Guide de conception d’Infrastructure à clé publique ](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).  

### <a name="web-server-iis"></a>Serveur Web (IIS)  

Le rôle de serveur Web (IIS) dans Windows Server2016 fournit une plateforme sécurisée, facile à gérer, modulaire et extensible héberger de manière fiable des sites Web, services et applications. Avec IIS, vous pouvez partager des informations avec des utilisateurs sur Internet, un intranet ou un extranet. IIS est une plateforme web unifiée qui intègre IIS, ASP.NET, services FTP, PHP et WindowsCommunicationFoundation (WCF).  

Pour plus d’informations, voir [vue d’ensemble du serveur Web (IIS)](https://technet.microsoft.com/library/hh831725.aspx).  
