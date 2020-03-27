---
title: Présentation du déploiement de certificats de serveur
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 63cc9e3b347635aaf631169b887b0e4c0dd9e989
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318231"
---
# <a name="server-certificate-deployment-overview"></a>Présentation du déploiement de certificats de serveur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique contient les sections suivantes.  
  
-   [Composants de déploiement de certificat de serveur](#bkmk_components)
  
-   [Vue d’ensemble du processus de déploiement de certificat de serveur](#bkmk_process)
  
## <a name="server-certificate-deployment-components"></a><a name="bkmk_components"></a>Composants de déploiement de certificat de serveur
Vous pouvez utiliser ce guide pour installer les services de certificats Active Directory (AD CS) en tant qu’autorité de certification racine d’entreprise et pour inscrire des certificats de serveur sur des serveurs qui exécutent le serveur NPS (Network Policy Server), le service de routage et d’accès à distance (RRAS), NPS et RRAS.


Si vous déployez SDN avec l’authentification basée sur les certificats, les serveurs sont tenus d’utiliser un certificat de serveur pour prouver leur identité à d’autres serveurs afin d’assurer des communications sécurisées.
  
L’illustration suivante montre les composants requis pour déployer des certificats de serveur sur les serveurs de votre infrastructure SDN.
  
![Infrastructure requise pour le déploiement de certificats de serveur](../../../media/Nps-Certs/Nps-Certs.jpg)  
  
> [!NOTE]  
> Dans l’illustration ci-dessus, plusieurs serveurs sont représentés : DC1, CA1, WEB1 et plusieurs serveurs SDN. Ce guide fournit des instructions sur le déploiement et la configuration de CA1 et WEB1, ainsi que sur la configuration de DC1, que ce guide suppose que vous avez déjà installé sur votre réseau. Si vous n’avez pas encore installé votre domaine Active Directory, vous pouvez le faire à l’aide du [Guide du réseau de base](https://technet.microsoft.com/library/mt604042.aspx) pour Windows Server 2016.  
  
Pour plus d’informations sur chaque élément représenté dans l’illustration ci-dessus, consultez les rubriques suivantes :  
  
-   [CA1](#bkmk_ca1)  
  
-   [WEB1](#bkmk_web1)  
  
-   [CD1](#bkmk_dc1)  
  
-   [LE serveur NPS1](#bkmk_nps1)  
  
### <a name="ca1-running-the-ad-cs-server-role"></a><a name="bkmk_ca1"></a>CA1 exécutant le rôle de serveur AD CS  
Dans ce scénario, l’autorité de certification racine d’entreprise est également une autorité de certification émettrice. L’autorité de certification émet des certificats aux ordinateurs serveurs disposant des autorisations de sécurité appropriées pour inscrire un certificat. Les services de certificats Active Directory (AD CS) sont installés sur CA1.  
  
Pour les réseaux plus grands ou pour lesquels des problèmes de sécurité justifient, vous pouvez séparer les rôles de l’autorité de certification racine et de l’autorité de certification émettrice, et déployer les autorités de certification secondaires qui émettent des autorités de certification.  
  
Dans les déploiements les plus sécurisés, l’autorité de certification racine d’entreprise est mise hors connexion et sécurisée physiquement.   
  
#### <a name="capolicyinf"></a>CAPolicy. inf  
Avant d’installer les services AD CS, vous configurez le fichier CAPolicy. inf avec des paramètres spécifiques pour votre déploiement.  
  
#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>Copie du modèle de certificat des **serveurs RAS et IAS**  
Lorsque vous déployez des certificats de serveur, vous créez une copie du modèle de certificat des **serveurs RAS et IAS** , puis vous configurez le modèle en fonction de vos besoins et des instructions contenues dans ce guide.   
  
Vous utilisez une copie du modèle plutôt que le modèle d’origine afin que la configuration du modèle d’origine soit préservée pour une utilisation future possible. Vous configurez la copie du modèle **RAS et des serveurs IAS** afin que l’autorité de certification puisse créer des certificats de serveur qu’elle envoie aux groupes dans Active Directory utilisateurs et ordinateurs que vous spécifiez.  
  
#### <a name="additional-ca1-configuration"></a>Configuration CA1 supplémentaire  
L’autorité de certification publie une liste de révocation de certificats que les ordinateurs doivent vérifier pour s’assurer que les certificats qui leur sont présentés comme preuve d’identité sont des certificats valides et qu’ils n’ont pas été révoqués. Vous devez configurer votre autorité de certification avec l’emplacement correct de la liste de révocation de certificats afin que les ordinateurs sachent où rechercher la liste de révocation de certificats pendant le processus d’authentification.  
  
### <a name="web1-running-the-web-services-iis-server-role"></a><a name="bkmk_web1"></a>WEB1 exécutant le rôle serveur services Web (IIS)  
Sur l’ordinateur qui exécute le rôle serveur serveur Web (IIS), WEB1, vous devez créer un dossier dans l’Explorateur Windows pour l’utiliser comme emplacement pour la liste de révocation de certificats et AIA.  
  
#### <a name="virtual-directory-for-the-crl-and-aia"></a>Répertoire virtuel pour la liste de révocation de certificats et AIA  
Après avoir créé un dossier dans l’Explorateur Windows, vous devez le configurer en tant que répertoire virtuel dans le gestionnaire de Internet Information Services (IIS), ainsi que configurer la liste de contrôle d’accès pour le répertoire virtuel afin de permettre aux ordinateurs d’accéder aux AIA et aux listes de révocation de certificats. après leur publication.  
  
### <a name="dc1-running-the-ad-ds-and-dns-server-roles"></a><a name="bkmk_dc1"></a>DC1 exécutant les rôles de serveur AD DS et DNS  
DC1 est le contrôleur de domaine et le serveur DNS sur votre réseau.  
  
#### <a name="group-policy-default-domain-policy"></a>stratégie de groupe la stratégie de domaine par défaut  
Après avoir configuré le modèle de certificat sur l’autorité de certification, vous pouvez configurer la stratégie de domaine par défaut dans stratégie de groupe afin que les certificats soient automatiquement inscrits aux serveurs NPS et RAS. Stratégie de groupe est configurée dans AD DS sur le serveur DC1.  
  
#### <a name="dns-alias-cname-resource-record"></a>Enregistrement de ressource d’alias DNS (CNAMe)  
Vous devez créer un enregistrement de ressource alias (CNAMe) pour le serveur Web pour vous assurer que les autres ordinateurs peuvent trouver le serveur, ainsi que l’AIA et la liste de révocation des certificats stockés sur le serveur. En outre, l’utilisation d’un enregistrement de ressource CNAMe CNAMe offre une certaine flexibilité afin que vous puissiez utiliser le serveur Web à d’autres fins, telles que l’hébergement de sites Web et FTP.  
  
### <a name="nps1-running-the-network-policy-server-role-service-of-the-network-policy-and-access-services-server-role"></a><a name="bkmk_nps1"></a>LE serveur NPS1 exécutant le service de rôle serveur NPS (Network Policy Server) du rôle serveur services de stratégie et d’accès réseau  
Le serveur NPS est installé lorsque vous effectuez les tâches décrites dans le Guide du réseau de base de Windows Server 2016. par conséquent, avant d’effectuer les tâches décrites dans ce guide, vous devez déjà disposer d’un ou de plusieurs NPSs installés sur votre réseau.  
  
#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>stratégie de groupe appliqué et certificat inscrit aux serveurs  
Une fois que vous avez configuré le modèle de certificat et l’inscription automatique, vous pouvez actualiser les stratégie de groupe sur tous les serveurs cibles. À ce stade, les serveurs inscrivent le certificat de serveur à partir de CA1.  
  
### <a name="server-certificate-deployment-process-overview"></a><a name="bkmk_process"></a>Vue d’ensemble du processus de déploiement de certificat de serveur  
  
> [!NOTE]  
> Les détails relatifs à l’exécution de ces étapes sont fournis dans la section [déploiement de certificats de serveur](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md).  
  
Le processus de configuration de l’inscription de certificats de serveur se déroule selon les étapes suivantes :  
  
1.  Sur WEB1, installez le rôle serveur Web (IIS).  
  
2.  Sur DC1, créez un enregistrement d’alias (CNAMe) pour votre serveur Web, WEB1.  
  
3.  Configurez votre serveur Web pour héberger la liste de révocation de certificats de l’autorité de certification, puis publiez la liste de révocation de certificats et copiez le certificat d’autorité de certification racine d’entreprise dans le nouveau  
  
4.  Sur l’ordinateur sur lequel vous prévoyez d’installer les services AD CS, attribuez à l’ordinateur une adresse IP statique, renommez l’ordinateur, joignez l’ordinateur au domaine, puis ouvrez une session sur l’ordinateur avec un compte d’utilisateur membre du groupe administrateurs du domaine et administrateurs de l’entreprise.  
  
5.  Sur l’ordinateur sur lequel vous prévoyez d’installer les services AD CS, configurez le fichier CAPolicy. inf avec les paramètres spécifiques à votre déploiement.  
  
6.  Installez le rôle serveur AD CS et effectuez une configuration supplémentaire de l’autorité de certification.  
  
7.  Copiez la liste de révocation de certificats et le certificat d’autorité de certification à partir de CA1 dans le partage sur le serveur Web WEB1.  
  
8.  Sur l’autorité de certification, configurez une copie du modèle de certificat des serveurs RAS et IAS. Comme l’autorité de certification émet des certificats basés sur un modèle de certificat, vous devez configurer le modèle pour le certificat de serveur pour que l’autorité de certification puisse émettre un certificat.  
  
9.  Configurez l’inscription automatique de certificat de serveur dans stratégie de groupe. Quand vous configurez l’inscription automatique, tous les serveurs que vous avez spécifiés avec Active Directory appartenances à des groupes reçoivent automatiquement un certificat de serveur lorsque stratégie de groupe sur chaque serveur est actualisé. Si vous ajoutez d’autres serveurs ultérieurement, ils recevront automatiquement un certificat de serveur.  
  
10. Actualisez stratégie de groupe sur les serveurs. Lorsque stratégie de groupe est actualisé, les serveurs reçoivent le certificat de serveur, qui est basé sur le modèle que vous avez configuré à l’étape précédente. Ce certificat est utilisé par le serveur pour prouver son identité aux ordinateurs clients et aux autres serveurs pendant le processus d’authentification.  
  
    > [!NOTE]  
    > Tous les ordinateurs membres du domaine reçoivent automatiquement le certificat de l’autorité de certification racine d’entreprise sans la configuration de l’inscription automatique. Ce certificat est différent du certificat de serveur que vous configurez et distribuez à l’aide de l’inscription automatique. Le certificat de l’autorité de certification est automatiquement installé dans le magasin de certificats des autorités de certification racines de confiance pour tous les ordinateurs membres du domaine afin qu’ils approuvent les certificats émis par cette autorité de certification.   
  
10. Vérifiez que tous les serveurs ont inscrit un certificat de serveur valide.  
  


