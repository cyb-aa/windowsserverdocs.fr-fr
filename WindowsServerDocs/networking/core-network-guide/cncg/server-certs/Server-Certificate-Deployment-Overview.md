---
title: Présentation du déploiement de certificat de serveur
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4650c99325def63fd0df25989c661230fada3dac
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment-overview"></a>Présentation du déploiement de certificat de serveur

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique contient les sections suivantes.  
  
-   [Composants de déploiement de certificat de serveur](#bkmk_components)
  
-   [Présentation du processus de déploiement de certificat serveur](#bkmk_process)
  
## <a name="bkmk_components"></a>Composants de déploiement de certificat de serveur
Vous pouvez utiliser ce guide pour installer les Services de certificats ActiveDirectory (ADCS) comme une autorité de certification racine entreprise (CA) et d’inscrire des certificats de serveur pour les serveurs qui exécutent le service serveur NPS (Network Policy), routage et accès distant (RRAS), ou NPS et RRAS.


Si vous déployez SDN avec l’authentification basée sur les certificats, les serveurs doivent utiliser un certificat de serveur pour prouver leur identité à d’autres serveurs afin qu’ils réalisent des communications sécurisées.
  
L’illustration suivante montre les composants qui sont nécessaires pour déployer des certificats de serveur sur des serveurs dans votre infrastructure SDN.
  
![Infrastructure requis de déploiement des certificats de serveur](../../../media/Nps-Certs/Nps-Certs.jpg)  
  
> [!NOTE]  
> Dans l’illustration ci-dessus, plusieurs serveurs sont illustrées: DC1, CA1, WEB1 et SDN de nombreux serveurs. Ce guide fournit des instructions pour le déploiement et la configuration CA1 et WEB1 et pour la configuration de DC1, qui ce guide suppose que vous avez déjà installé sur votre réseau. Si vous n’avez pas déjà installé votre domaine ActiveDirectory, vous pouvez le faire à l’aide de la [Guide réseau de base](https://technet.microsoft.com/library/mt604042.aspx) pour Windows Server2016.  
  
Pour plus d’informations sur chaque élément indiqué dans l’illustration ci-dessus, consultez les rubriques suivantes:  
  
-   [CA1](#bkmk_ca1)  
  
-   [WEB1](#bkmk_web1)  
  
-   [DC1](#bkmk_dc1)  
  
-   [SERVEUR NPS1](#bkmk_nps1)  
  
### <a name="bkmk_ca1"></a>CA1 exécutant le rôle de serveur ADCS  
Dans ce scénario, l’autorité de certification racine d’entreprise (CA) est également une autorité de certification émettrice. L’autorité de certification émet des certificats aux ordinateurs de serveur qui disposent des autorisations de sécurité appropriées pour inscrire un certificat. Services de certificats ActiveDirectory (ADCS) est installé sur l’autorité de certification 1.  
  
Pour les réseaux de grande taille ou où les problèmes de sécurité fournissent une justification et une, vous pouvez séparer les rôles de racine d’autorité de certification et l’autorité de certification émettrice et déployer des autorités de certification secondaires qui sont des autorités de certification émettrices.  
  
Dans les déploiements plus sécurisés, l’autorité de certification racine d’entreprise est effectuée en mode hors connexion et sécurisé physiquement.   
  
#### <a name="capolicyinf"></a>CAPolicy.inf  
Avant d’installer les services ADCS, vous configurez le fichier CAPolicy.inf avec des paramètres spécifiques pour votre déploiement.  
  
#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>Copie de la **serveurs RAS et IAS** modèle de certificat  
Lorsque vous déployez des certificats de serveur, vous faire une copie de la **serveurs RAS et IAS** modèle de certificat, puis configurez le modèle en fonction de vos besoins et les instructions fournies dans ce guide.   
  
Vous utilisez une copie du modèle au lieu du modèle d’origine afin que la configuration du modèle d’origine est conservée pour une utilisation ultérieure possible. Vous configurez la copie de la **serveurs RAS et IAS** modèle afin que l’autorité de certification peut créer des certificats de serveur qu’elle émet aux groupes d’utilisateurs ActiveDirectory et les ordinateurs que vous spécifiez.  
  
#### <a name="additional-ca1-configuration"></a>Configuration supplémentaire CA1  
L’autorité de certification publie une liste de révocation de certificats (CRL) que les ordinateurs doivent vérifier pour vous assurer que les certificats qui sont présentés comme preuve d’identité sont des certificats valides et n’ont pas été révoqués. Vous devez configurer votre autorité de certification avec l’emplacement correct de la révocation de certificats afin que les ordinateurs savent où rechercher de la révocation des certificats pendant le processus d’authentification.  
  
### <a name="bkmk_web1"></a>Web1 exécutant le rôle de serveur Web Services (IIS)  
Sur l’ordinateur qui exécute le rôle serveur serveur Web (IIS), WEB1, vous devez créer un dossier dans l’Explorateur Windows à utiliser comme emplacement de la révocation de certificats et AIA.  
  
#### <a name="virtual-directory-for-the-crl-and-aia"></a>Répertoire virtuel pour la révocation de certificats et AIA  
Une fois que vous créez un dossier dans l’Explorateur Windows, vous devez configurer le dossier comme un répertoire virtuel dans le Gestionnaire des Services Internet (IIS), ainsi que la configuration de la liste de contrôle d’accès pour le répertoire virtuel pour permettre aux ordinateurs pour accéder à l’AIA et CRL après leur publication.  
  
### <a name="bkmk_dc1"></a>DC1 exécutant les rôles de serveur ADDS et DNS  
DC1 est le contrôleur de domaine et le serveur DNS sur votre réseau.  
  
#### <a name="group-policy-default-domain-policy"></a>Stratégie de domaine par défaut de stratégie de groupe  
Une fois que vous configurez le modèle de certificat sur l’autorité de certification, vous pouvez configurer la stratégie de domaine par défaut dans la stratégie de groupe afin que les certificats sont automatiquement inscrits aux serveurs NPS et RAS. Stratégie de groupe est configurée dans les services ADDS sur le serveur DC1.  
  
#### <a name="dns-alias-cname-resource-record"></a>Enregistrement de ressource alias (CNAME) de DNS  
Vous devez créer un enregistrement de ressource alias (CNAME) pour le serveur Web pour vous assurer que les autres ordinateurs peuvent trouver le serveur, ainsi que l’AIA et la révocation de certificats qui sont stockés sur le serveur. En outre, à l’aide d’un enregistrement de ressource CNAME alias flexibilité afin que vous pouvez utiliser le serveur Web à d’autres fins, telles que l’hébergement de sites Web et FTP.  
  
### <a name="bkmk_nps1"></a>Serveur NPS1 exécutant le service de rôle de serveur de stratégie réseau du rôle de serveur de stratégie réseau et Services d’accès  
Le serveur NPS est installé lorsque vous effectuez les tâches dans le Guide du réseau Windows Server2016 Core, par conséquent, avant d’effectuer les tâches dans ce guide, vous disposez déjà d’un ou plusieurs serveurs NPS est installé sur votre réseau.  
  
#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>Stratégie de groupe appliqués et certificats inscrits sur les serveurs  
Après avoir configuré le modèle de certificat et l’inscription automatique, vous pouvez actualiser la stratégie de groupe sur tous les serveurs cibles. À ce stade, les serveurs inscrivent le certificat de serveur à partir de l’autorité de certification 1.  
  
### <a name="bkmk_process"></a>Présentation du processus de déploiement de certificat serveur  
  
> [!NOTE]  
> Les détails de l’exécution de ces étapes sont fournies dans la section [déploiement de certificats de serveur](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md).  
  
Le processus de configuration de l’inscription de certificats de serveur se produit dans ces étapes:  
  
1.  Sur WEB1, installez le rôle de serveur Web (IIS).  
  
2.  Sur DC1, créez un enregistrement d’alias (CNAME) pour votre serveur Web, WEB1.  
  
3.  Configurer votre serveur Web pour héberger la liste CRL de l’autorité de certification, puis publier la liste de révocation et copiez le certificat d’autorité de certification racine d’entreprise dans le nouveau répertoire virtuel.  
  
4.  Sur l’ordinateur où vous prévoyez d’installer les services ADCS, affectez l’ordinateur une adresse IP statique, renommer l’ordinateur, joindre l’ordinateur au domaine, puis ouvrez une session sur l’ordinateur avec un compte d’utilisateur qui est membre des groupes Admins du domaine et administrateurs de l’entreprise.  
  
5.  Sur l’ordinateur où vous prévoyez d’installer les services ADCS, configurez le fichier CAPolicy.inf avec des paramètres qui sont spécifiques à votre déploiement.  
  
6.  Installer le rôle de serveur ADCS et effectuer une configuration supplémentaire de l’autorité de certification.  
  
7.  Copiez le certificat d’autorité de certification et de révocation de certificats à partir de l’autorité de certification 1dans le partage sur le serveur Web WEB1.  
  
8.  Sur l’autorité de certification, configurez une copie du modèle de certificat serveurs RAS et IAS. L’autorité de certification émet des certificats basés sur un modèle de certificat, vous devez donc configurer le modèle pour le certificat de serveur avant de l’autorité de certification peut émettre un certificat.  
  
9.  Configurer l’inscription automatique du certificat de serveur dans la stratégie de groupe. Lorsque vous configurez l’inscription automatique, tous les serveurs que vous avez spécifiée automatiquement avec les appartenances aux groupes ActiveDirectory reçoivent un certificat de serveur lors de l’actualisation de la stratégie de groupe sur chaque serveur. Si vous ajoutez des serveurs plus tard, ils recevront automatiquement un certificat de serveur, trop.  
  
10. Actualiser la stratégie de groupe sur les serveurs. Lorsque la stratégie de groupe est actualisée, les serveurs de recevoir le certificat de serveur, qui est basé sur le modèle que vous avez configuré à l’étape précédente. Ce certificat est utilisé par le serveur pour prouver son identité aux ordinateurs clients et d’autres serveurs au cours du processus d’authentification.  
  
    > [!NOTE]  
    > Tous les ordinateurs membres du domaine reçoivent automatiquement un certificat de l’autorité de certification racine d’entreprise sans la configuration de l’inscription automatique. Ce certificat est différent de celui que vous configurez et distribuez à l’aide de l’inscription automatique, le certificat de serveur. Certificat de l’autorité de certification est automatiquement installé dans le magasin de certificats Autorités de Certification racine de confiance pour tous les ordinateurs membres du domaine afin qu’ils approuvent les certificats émis par cette autorité de certification.   
  
10. Vérifiez que tous les serveurs ont inscrit un certificat de serveur valide.  
  


