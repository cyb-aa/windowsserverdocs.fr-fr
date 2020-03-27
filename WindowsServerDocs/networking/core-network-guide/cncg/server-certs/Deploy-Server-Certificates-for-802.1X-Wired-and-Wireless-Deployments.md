---
title: Déployer des certificats de serveur pour des déploiements câblés et sans fil 802.1X
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0636fc321b4e94351628fd577526a8e81b4fc4cf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318312"
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Déployer des certificats de serveur pour des déploiements câblés et sans fil 802.1X

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser ce guide pour déployer des certificats de serveur sur vos serveurs d’infrastructure d’accès à distance et de serveur de stratégie réseau (NPS).   

Ce guide contient les sections suivantes.  

-   [Conditions préalables à l’utilisation de ce guide](#bkmk_pre)  

-   [Ce que ce guide ne contient pas](#bkmk_not)  

-   [Vues d’ensemble de la technologie](#bkmk_tech)  

-   [Présentation du déploiement de certificats de serveur](Server-Certificate-Deployment-Overview.md)  

-   [Planification du déploiement de certificats de serveur](Server-Certificate-Deployment-Planning.md)  

-   [Déploiement de certificats de serveur](Server-Certificate-Deployment.md)  

### <a name="digital-server-certificates"></a>**Certificats de serveur numérique**  
Ce guide fournit des instructions sur l’utilisation des services de certificats Active Directory (AD CS) pour inscrire automatiquement des certificats sur des serveurs d’infrastructure NPS et d’accès à distance. Les services AD CS vous permettent de créer une infrastructure à clé publique (PKI) et de fournir un chiffrement à clé publique, des certificats numériques et des fonctionnalités de signature numérique pour votre organisation.  

Lorsque vous utilisez des certificats de serveur numérique pour l’authentification entre des ordinateurs de votre réseau, les certificats fournissent les éléments suivants :   

1. Confidentialité par le biais du chiffrement.  
2. L’intégrité par le biais de signatures numériques.  
3. Authentification en associant des clés de certificat à des comptes d’ordinateur, d’utilisateur ou de périphérique sur un réseau informatique.  

### <a name="server-types"></a>**Types de serveurs**  
À l’aide de ce guide, vous pouvez déployer des certificats de serveur sur les types de serveurs suivants.  
- Les serveurs qui exécutent le service d’accès à distance, qui sont des serveurs DirectAccess ou des serveurs de réseau privé virtuel (VPN) standard, et qui sont membres du groupe de **serveurs RAS et IAS** .  
- Les serveurs qui exécutent le service NPS (Network Policy Server) qui sont membres du groupe de **serveurs RAS et IAS** .  

### <a name="advantages-of-certificate-autoenrollment"></a>**Avantages de l’inscription automatique des certificats**  
L’inscription automatique de certificats de serveur, également appelée inscription automatique, offre les avantages suivants.  

- L’autorité de certification AD CS inscrit automatiquement un certificat de serveur sur tous vos serveurs NPS et d’accès à distance.  
- Tous les ordinateurs du domaine reçoivent automatiquement votre certificat d’autorité de certification, qui est installé dans le magasin autorités de certification racines de confiance sur chaque ordinateur membre du domaine. Pour cette raison, tous les ordinateurs du domaine approuvent les certificats émis par votre autorité de certification. Cette approbation permet à vos serveurs d’authentification de prouver leurs identités les uns aux autres et d’intervenir dans des communications sécurisées.  
- En dehors de l’actualisation des stratégie de groupe, la reconfiguration manuelle de chaque serveur n’est pas nécessaire.  
- Chaque certificat de serveur comprend à la fois l’authentification du serveur et le rôle d’authentification du client dans les extensions d’utilisation améliorée de la clé.  
- Extensibilité. Après avoir déployé votre autorité de certification racine d’entreprise à l’aide de ce guide, vous pouvez étendre votre infrastructure à clé publique (PKI) en ajoutant des autorités de certification secondaires d’entreprise.  
- Facilité de gestion. Vous pouvez gérer les services AD CS à l’aide de la console AD CS ou à l’aide de commandes et de scripts Windows PowerShell.  
- Simplicité. Vous spécifiez les serveurs qui inscrivent les certificats de serveur à l’aide des comptes de groupe Active Directory et de l’appartenance à un groupe.   
- Lorsque vous déployez des certificats de serveur, les certificats sont basés sur un modèle que vous configurez à l’aide des instructions de ce guide. Cela signifie que vous pouvez personnaliser différents modèles de certificats pour des types de serveurs spécifiques, ou vous pouvez utiliser le même modèle pour tous les certificats de serveur que vous souhaitez émettre.  

## <a name="prerequisites-for-using-this-guide"></a><a name="bkmk_pre"></a>Conditions préalables à l’utilisation de ce guide  

Ce guide fournit des instructions sur le déploiement de certificats de serveur à l’aide des services AD CS et du rôle serveur Web (IIS) dans Windows Server 2016. Voici les conditions préalables à suivre pour effectuer les procédures décrites dans ce guide.  

- Vous devez déployer un réseau de base à l’aide du Guide du réseau de base Windows Server 2016, ou les technologies fournies dans le Guide du réseau de base doivent déjà être installées et fonctionnent correctement sur votre réseau. Ces technologies sont les suivantes : TCP/IP V4, DHCP, Active Directory Domain Services (AD DS), DNS et NPS.  
  >[!NOTE]
  >Le Guide du réseau de base Windows Server 2016 est disponible dans la bibliothèque technique de Windows Server 2016. Pour plus d’informations, consultez le [Guide du réseau de base](../../../core-network-guide/Core-Network-Guide.md).

- Vous devez lire la section relative à la planification de ce guide pour vous assurer que vous êtes prêt pour ce déploiement avant d’effectuer le déploiement.  
- Vous devez effectuer les étapes de ce guide dans l’ordre dans lequel elles sont présentées. Ne passez pas à l’étape suivante et déployez votre autorité de certification sans effectuer les étapes de déploiement du serveur, sinon votre déploiement échouera.  
- Vous devez être prêt à déployer deux nouveaux serveurs sur votre réseau : un serveur sur lequel vous allez installer les services AD CS en tant qu’autorité de certification racine d’entreprise, et un serveur sur lequel vous installerez le serveur Web (IIS) afin que votre autorité de certification puisse publier la liste de révocation de certificats sur le Web. serveurs.   

>[!NOTE]  
>Vous êtes prêt à affecter une adresse IP statique aux serveurs Web et AD CS que vous déployez à l’aide de ce guide, ainsi qu’à nommer les ordinateurs en fonction des conventions d’affectation de noms de votre organisation. En outre, vous devez joindre les ordinateurs à votre domaine.  

## <a name="what-this-guide-does-not-provide"></a><a name="bkmk_not"></a>Ce que ce guide ne fournit pas  
Ce guide ne fournit pas d’instructions complètes pour la conception et le déploiement d’une infrastructure à clé publique (PKI) à l’aide des services AD CS. Avant de déployer les technologies de ce guide, il est recommandé de consulter la documentation AD CS et la documentation relative à la conception de l’infrastructure à clé publique.   

## <a name="technology-overviews"></a><a name="bkmk_tech"></a>Vues d’ensemble de la technologie  
Voici les vues d’ensemble des technologies pour les services AD CS et les serveurs Web (IIS).  

### <a name="active-directory-certificate-services"></a>Services de certificats Active Directory  
Les services AD CS dans Windows Server 2016 fournissent des services personnalisables pour la création et la gestion des certificats X. 509 utilisés dans les systèmes de sécurité logicielle qui utilisent des technologies de clé publique. Les organisations peuvent utiliser les services AD CS pour améliorer la sécurité en liant l’identité d’une personne, d’un périphérique ou d’un service à une clé publique correspondante. Les services AD CS incluent également des fonctionnalités qui vous permettent de gérer l’inscription et la révocation de certificats dans divers environnements évolutifs.  

Pour plus d’informations, consultez [Active Directory vue d’ensemble des services de certificats](https://technet.microsoft.com/library/hh831740.aspx) et guide de conception de l’infrastructure à [clé publique](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).  

### <a name="web-server-iis"></a>Serveur web (IIS)  

Le rôle de serveur Web (IIS) dans Windows Server 2016 fournit une plateforme sécurisée, facile à gérer, modulaire et extensible pour héberger de manière fiable des sites Web, des services et des applications. Avec IIS, vous pouvez partager des informations avec des utilisateurs sur Internet, un intranet ou un extranet. IIS est une plateforme Web unifiée qui intègre les services IIS, ASP.NET, FTP, PHP et Windows Communication Foundation (WCF).  

Pour plus d’informations, consultez [vue d’ensemble du serveur Web (IIS)](https://technet.microsoft.com/library/hh831725.aspx).  
