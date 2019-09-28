---
title: Planification du déploiement de certificats de serveur
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1ec5bc315381f85434753f9becc94409a74271b7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356106"
---
# <a name="server-certificate-deployment-planning"></a>Planification du déploiement de certificats de serveur

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Avant de déployer des certificats de serveur, vous devez planifier les éléments suivants :  
  
-   [Planifier la configuration du serveur de base](#bkmk_basic)  
  
-   [Planifier l’accès au domaine](#bkmk_domain)  
  
-   [Planifier l’emplacement et le nom du répertoire virtuel sur votre serveur Web](#bkmk_virtual)  
  
-   [Planifier un enregistrement d’alias DNS (CNAMe) pour votre serveur Web](#bkmk_cname)  
  
-   [Planifier la configuration de CAPolicy. inf](#bkmk_capolicy)  
  
-   [Planifier la configuration des extensions CDP et AIA sur CA1](#bkmk_cdp)  
  
-   [Planifier l’opération de copie entre l’autorité de certification et le serveur Web](#bkmk_copy)  
  
-   [Planifier la configuration du modèle de certificat de serveur sur l’autorité de certification](#bkmk_template)  
  
## <a name="bkmk_basic"></a>Planifier la configuration du serveur de base  
Après avoir installé Windows Server 2016 sur les ordinateurs que vous envisagez d’utiliser en tant qu’autorité de certification et serveur Web, vous devez renommer l’ordinateur et affecter et configurer une adresse IP statique pour l’ordinateur local.  
  
Pour plus d’informations, consultez le Guide du [réseau de base](../../../core-network-guide/Core-Network-Guide.md)Windows Server 2016.  
  
## <a name="bkmk_domain"></a>Planifier l’accès au domaine  
Pour ouvrir une session sur le domaine, l’ordinateur doit être un ordinateur membre du domaine et le compte d’utilisateur doit être créé dans AD DS avant la tentative d’ouverture de session. En outre, la plupart des procédures de ce guide requièrent que le compte d’utilisateur soit membre des groupes Administrateurs de l’entreprise ou Admins du domaine dans Active Directory utilisateurs et ordinateurs. vous devez donc vous connecter à l’autorité de certification avec un compte disposant de l’appartenance au groupe appropriée.  
  
Pour plus d’informations, consultez le Guide du [réseau de base](../../../core-network-guide/Core-Network-Guide.md)Windows Server 2016.  
  
## <a name="bkmk_virtual"></a>Planifier l’emplacement et le nom du répertoire virtuel sur votre serveur Web  
Pour fournir un accès à la liste de révocation de certificats et au certificat de l’autorité de certification sur d’autres ordinateurs, vous devez stocker ces éléments dans un répertoire virtuel sur votre serveur Web. Dans ce guide, le répertoire virtuel se trouve sur le serveur Web WEB1. Ce dossier se trouve sur le lecteur « C : » et est nommé « PKI ». Vous pouvez localiser votre répertoire virtuel sur votre serveur Web à n’importe quel emplacement de dossier approprié pour votre déploiement.  
  
## <a name="bkmk_cname"></a>Planifier un enregistrement d’alias DNS (CNAMe) pour votre serveur Web  
Les enregistrements de ressource alias (CNAMe) sont également appelés enregistrements de ressources de noms canoniques. Avec ces enregistrements, vous pouvez utiliser plusieurs noms pour pointer vers un seul hôte, ce qui facilite l’hébergement d’un serveur protocole FTP (FTP) et d’un serveur Web sur le même ordinateur. Par exemple, les noms de serveurs connus (FTP, www) sont inscrits à l’aide d’enregistrements de ressource alias (CNAMe) mappés au nom d’hôte DNS (Domain Name System), tel que WEB1, pour l’ordinateur serveur qui héberge ces services.  
  
Ce guide fournit des instructions sur la configuration de votre serveur Web pour héberger la liste de révocation de certificats (CRL) pour votre autorité de certification (CA). Étant donné que vous pouvez également utiliser votre serveur Web à d’autres fins, par exemple pour héberger un site FTP ou Web, il est judicieux de créer un enregistrement de ressource alias dans DNS pour votre serveur Web. Dans ce guide, l’enregistrement CNAMe est nommé « PKI », mais vous pouvez choisir un nom approprié pour votre déploiement.  
  
## <a name="bkmk_capolicy"></a>Planifier la configuration de CAPolicy. inf  
Avant d’installer les services AD CS, vous devez configurer CAPolicy. inf sur l’autorité de certification avec des informations correctes pour votre déploiement. Un fichier CAPolicy. INF contient les informations suivantes :  
  
```  
[Version]  
Signature="$Windows NT$"  
[PolicyStatementExtension]  
Policies=InternalPolicy  
[InternalPolicy]  
OID=1.2.3.4.1455.67.89.5  
Notice="Legal Policy Statement"  
URL=https://pki.corp.contoso.com/pki/cps.txt  
[Certsrv_Server]  
RenewalKeyLength=2048  
RenewalValidityPeriod=Years  
RenewalValidityPeriodUnits=5  
CRLPeriod=weeks  
CRLPeriodUnits=1  
LoadDefaultTemplates=0  
AlternateSignatureAlgorithm=1  
```  
Vous devez planifier les éléments suivants pour ce fichier :  
  
-   **URL**. L’exemple de fichier CAPolicy. inf a une valeur d’URL de **https://pki.corp.contoso.com/pki/cps.txt** . Cela est dû au fait que le serveur Web dans ce guide est nommé WEB1 et possède un enregistrement de ressource CNAMe DNS de l’infrastructure à clé publique. Le serveur Web est également joint au domaine corp.contoso.com. En outre, il existe un répertoire virtuel sur le serveur Web nommé « PKI » dans lequel la liste de révocation de certificats est stockée. Assurez-vous que la valeur que vous fournissez pour l’URL dans votre fichier CAPolicy. inf pointe vers un répertoire virtuel sur votre serveur Web dans votre domaine.  
  
-   **RenewalKeyLength**. La longueur par défaut de la clé de renouvellement pour les services AD CS dans Windows Server 2012 est 2048. La longueur de clé que vous sélectionnez doit être aussi longue que possible tout en fournissant une compatibilité avec les applications que vous envisagez d’utiliser.  
  
-   **RenewalValidityPeriodUnits**. L’exemple de fichier CAPolicy. inf a une valeur RenewalValidityPeriodUnits de 5 ans. Cela est dû au fait que la durée de vie attendue de l’autorité de certification est d’environ dix ans. La valeur de RenewalValidityPeriodUnits doit refléter la période de validité globale de l’autorité de certification ou le nombre le plus élevé d’années pour lesquelles vous souhaitez effectuer l’inscription.  
  
-   **CRLPeriodUnits**. L’exemple de fichier CAPolicy. inf a une valeur CRLPeriodUnits de 1. Cela est dû au fait que l’intervalle d’actualisation de l’exemple de la liste de révocation de certificats dans ce guide est de 1 semaine. À la valeur d’intervalle que vous spécifiez avec ce paramètre, vous devez publier la liste de révocation de certificats sur l’autorité de certification dans le répertoire virtuel du serveur Web où vous stockez la liste de révocation de certificats et y accéder pour les ordinateurs qui se trouvent dans le processus d’authentification.  
  
-   **AlternateSignatureAlgorithm**. Ce fichier CAPolicy. inf implémente un mécanisme de sécurité amélioré en implémentant d’autres formats de signature. Vous ne devez pas implémenter ce paramètre si vous avez encore des clients Windows XP qui requièrent des certificats de cette autorité de certification.  
  
Si vous n’envisagez pas d’ajouter des autorités de certification secondaires à votre infrastructure de clé publique ultérieurement, et si vous souhaitez empêcher l’ajout d’autorités de certification secondaires, vous pouvez ajouter la clé PathLength à votre fichier CAPolicy. inf avec la valeur 0. Pour ajouter cette clé, copiez et collez le code suivant dans votre fichier :  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> Il n’est pas recommandé de modifier d’autres paramètres dans le fichier CAPolicy. inf, sauf si vous avez une raison particulière de le faire.  
  
## <a name="bkmk_cdp"></a>Planifier la configuration des extensions CDP et AIA sur CA1  
Quand vous configurez le point de distribution de liste de révocation de certificats (CDP) et les paramètres d’accès aux informations de l’autorité (AIA) sur CA1, vous avez besoin du nom de votre serveur Web et de votre nom de domaine. Vous avez également besoin du nom du répertoire virtuel que vous créez sur votre serveur Web où la liste de révocation de certificats (CRL) et le certificat de l’autorité de certification sont stockés.  
  
L’emplacement CDP que vous devez entrer pendant cette étape de déploiement a le format suivant :  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
Par exemple, si votre serveur Web est nommé WEB1 et que votre enregistrement CNAMe d’alias DNS pour le serveur Web est « PKI », que votre domaine est corp.contoso.com et que votre répertoire virtuel est nommé PKI, l’emplacement CDP est le suivant :  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
L’emplacement AIA que vous devez entrer est au format :  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
Par exemple, si votre serveur Web est nommé WEB1 et que votre enregistrement CNAMe d’alias DNS pour le serveur Web est « PKI », que votre domaine est corp.contoso.com et que votre répertoire virtuel est nommé PKI, l’emplacement AIA est le suivant :  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>Planifier l’opération de copie entre l’autorité de certification et le serveur Web  
Pour publier la liste de révocation de certificats et le certificat d’autorité de certification de l’autorité de certification dans le répertoire virtuel du serveur Web, vous pouvez exécuter la commande certutil-CRL après avoir configuré les emplacements CDP et AIA sur l’autorité de certification. Veillez à configurer les chemins d’accès corrects sous l’onglet **Extensions** des propriétés de l’autorité de certification avant d’exécuter cette commande à l’aide des instructions de ce guide. En outre, pour copier le certificat d’autorité de certification d’entreprise sur le serveur Web, vous devez avoir déjà créé le répertoire virtuel sur le serveur Web et configuré le dossier en tant que dossier partagé.  
  
## <a name="bkmk_template"></a>Planifier la configuration du modèle de certificat de serveur sur l’autorité de certification  
Pour déployer des certificats de serveur inscrits automatiquement, vous devez copier le modèle de certificat nommé **RAS et le serveur IAS**. Par défaut, cette copie est nommée **copie des serveurs RAS et IAS**. Si vous souhaitez renommer cette copie de modèle, planifiez le nom que vous souhaitez utiliser pendant cette étape de déploiement.  
  
> [!NOTE]  
> Les trois dernières sections de ce guide, qui vous permettent de configurer l’inscription automatique des certificats de serveur, d’actualiser les stratégie de groupe sur les serveurs et de vérifier que les serveurs ont reçu un certificat de serveur valide de l’autorité de certification-ne nécessitent pas de planification supplémentaire étapes.  
  


