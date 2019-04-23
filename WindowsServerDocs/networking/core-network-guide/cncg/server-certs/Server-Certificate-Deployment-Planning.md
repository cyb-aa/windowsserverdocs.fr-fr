---
title: Planification du déploiement de certificats de serveur
description: Cette rubrique fait partie du guide des certificats de serveur de déploiement pour les déploiements de sans fil et câblé à 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0d14ed33c9bf389433f59774c04ff15e37256cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839530"
---
# <a name="server-certificate-deployment-planning"></a>Planification du déploiement de certificats de serveur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Avant de déployer des certificats de serveur, vous devez planifier les éléments suivants :  
  
-   [Planifier la configuration du serveur de base](#bkmk_basic)  
  
-   [Planifier l’accès de domaine](#bkmk_domain)  
  
-   [Planifier l’emplacement et le nom du répertoire virtuel sur votre serveur Web](#bkmk_virtual)  
  
-   [Planifier un enregistrement d’alias (CNAME) DNS pour votre serveur Web](#bkmk_cname)  
  
-   [Planifier la configuration du fichier CAPolicy.inf](#bkmk_capolicy)  
  
-   [Planifier la configuration des extensions CDP et AIA sur CA1](#bkmk_cdp)  
  
-   [Planifier l’opération de copie entre l’autorité de certification et le serveur Web](#bkmk_copy)  
  
-   [Planifiez la configuration de modèle de certificat de serveur sur l’autorité de certification](#bkmk_template)  
  
## <a name="bkmk_basic"></a>Planifier la configuration du serveur de base  
Après avoir installé Windows Server 2016 sur les ordinateurs que vous prévoyez d’utiliser en tant que votre autorité de certification et le serveur Web, vous devez renommer l’ordinateur et affecter et configurer une adresse IP statique pour l’ordinateur local.  
  
Pour plus d’informations, consultez Windows Server 2016 [Guide du réseau](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_domain"></a>Planifier l’accès de domaine  
Pour vous connecter au domaine, l’ordinateur doit être un ordinateur membre du domaine et le compte d’utilisateur doit être créé dans AD DS avant la tentative d’ouverture de session. En outre, la plupart des procédures de ce guide nécessitent que le compte d’utilisateur est membre des groupes Administrateurs de l’entreprise ou Admins du domaine dans Active Directory utilisateurs et ordinateurs, donc vous devez vous connecter à l’autorité de certification avec un compte qui dispose de l’appartenance au groupe approprié.  
  
Pour plus d’informations, consultez Windows Server 2016 [Guide du réseau](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_virtual"></a>Planifier l’emplacement et le nom du répertoire virtuel sur votre serveur Web  
Pour fournir l’accès à la liste de révocation et le certificat d’autorité de certification sur d’autres ordinateurs, vous devez stocker ces éléments dans un répertoire virtuel sur votre serveur Web. Dans ce guide, le répertoire virtuel se trouve sur le serveur Web WEB1. Ce dossier se trouve sur le lecteur « C: » et est nommé « pki ». Vous pouvez localiser votre répertoire virtuel sur votre serveur Web à n’importe quel emplacement de dossier qui est approprié pour votre déploiement.  
  
## <a name="bkmk_cname"></a>Planifier un enregistrement d’alias (CNAME) DNS pour votre serveur Web  
Les enregistrements de ressource alias (CNAME) sont parfois appelés enregistrements de ressource de nom canonique. Avec ces enregistrements, vous pouvez utiliser plusieurs noms pour pointer vers un seul hôte, ce qui vous permet d’effectuer des opérations telles que l’hôte à la fois un serveur de transfert de protocole FTP (File) et un serveur Web sur le même ordinateur. Par exemple, les noms de serveurs bien connus (ftp, www) sont inscrits à l’aide d’enregistrements de ressource alias (CNAME) qui mappent au système DNS (Domain Name) nom d’hôte, par exemple WEB1, pour l’ordinateur du serveur qui héberge ces services.  
  
Ce guide fournit des instructions sur la configuration de votre serveur Web pour héberger la liste de révocation de certificats (CRL) pour votre autorité de certification (CA). Étant donné que vous pouvez également utiliser votre serveur Web à d’autres fins, par exemple pour héberger un site FTP ou Web, il est judicieux de créer un enregistrement de ressource alias dans DNS pour votre serveur Web. Dans ce guide, l’enregistrement CNAME est nommé « infrastructure à clé publique », mais vous pouvez choisir un nom qui convient à votre déploiement.  
  
## <a name="bkmk_capolicy"></a>Planifier la configuration du fichier CAPolicy.inf  
Avant d’installer les services AD CS, vous devez configurer le fichier CAPolicy.inf sur l’autorité de certification avec les informations correctes pour votre déploiement. Un fichier CAPolicy.inf contient les informations suivantes :  
  
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
  
-   **URL**. L’exemple de fichier CAPolicy.inf a la valeur URL **https://pki.corp.contoso.com/pki/cps.txt**. Il s’agit, car le serveur Web dans ce guide est nommé WEB1 et a un enregistrement de ressource CNAME DNS de l’infrastructure à clé publique. Le serveur Web est également joint au domaine corp.contoso.com. En outre, il est un répertoire virtuel sur le serveur Web nommé « infrastructure à clé publique » où se trouve la liste de révocation de certificats. Vérifiez que la valeur que vous fournissez pour l’URL dans votre fichier CAPolicy.inf fichier pointe vers un répertoire virtuel sur votre serveur Web dans votre domaine.  
  
-   **RenewalKeyLength**. La longueur de clé de renouvellement par défaut pour les services AD CS dans Windows Server 2012 est 2048. La longueur de clé que vous sélectionnez doit être aussi longue que possible tout en assurant la compatibilité avec les applications que vous souhaitez utiliser.  
  
-   **RenewalValidityPeriodUnits**. L’exemple de fichier CAPolicy.inf a une valeur de RenewalValidityPeriodUnits de 5 ans. Il s’agit, car la durée de vie prévue de l’autorité de certification est d’environ dix ans. La valeur de RenewalValidityPeriodUnits doit refléter la période de validité globale de l’autorité de certification ou le plus grand nombre d’années pour lequel vous souhaitez fournir l’inscription.  
  
-   **CRLPeriodUnits**. L’exemple de fichier CAPolicy.inf a 1 comme valeur CRLPeriodUnits. Il s’agit, car la liste de révocation de certificats dans ce guide dans l’intervalle d’actualisation de l’exemple est de 1 semaine. À la valeur d’intervalle que vous spécifiez avec ce paramètre, vous devez publier la liste CRL sur l’autorité de certification dans le répertoire virtuel du serveur Web dans lequel vous stockez la liste de révocation et assurer l’accès pour les ordinateurs qui se trouvent dans le processus d’authentification.  
  
-   **AlternateSignatureAlgorithm**. Ce fichier CAPolicy.inf implémente un mécanisme de sécurité améliorée en implémentant les autres formats de signature. Vous ne devez pas implémenter ce paramètre si vous avez toujours des clients Windows XP qui nécessitent des certificats à partir de cette autorité de certification.  
  
Si vous ne prévoyez pas sur l’ajout de toutes les autorités de certification à votre infrastructure à clé publique à une date ultérieure, et si vous souhaitez empêcher l’ajout de toutes les autorités de certification, vous pouvez ajouter la clé PathLength à votre fichier CAPolicy.inf avec une valeur égale à 0. Pour ajouter cette clé, copiez et collez le code suivant dans votre fichier :  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> Il est déconseillé de modifier d’autres paramètres dans le fichier CAPolicy.inf, sauf si vous avez une raison spécifique pour effectuer cette opération.  
  
## <a name="bkmk_cdp"></a>Planifier la configuration des extensions CDP et AIA sur CA1  
Lorsque vous configurez le Point de Distribution de liste de révocation de certificats (CRL) (CDP) et les paramètres d’accès d’informations d’autorité (AIA) sur CA1, vous devez le nom de votre serveur Web et votre nom de domaine. Vous devez également le nom du répertoire virtuel que vous créez sur votre serveur Web où sont stockés la liste de révocation de certificats (CRL) et le certificat d’autorité de certification.  
  
L’emplacement du CDP que vous devez entrer au cours de cette étape de déploiement est au format :  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
Par exemple, si votre serveur Web est nommé WEB1 et votre enregistrement d’alias CNAME pour le serveur Web de serveur DNS est « infrastructure à clé publique », votre domaine est corp.contoso.com et que votre répertoire virtuel est nommé pki, l’emplacement du CDP est :  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
L’emplacement AIA que vous devez entrer a le format :  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
Par exemple, si votre serveur Web est nommé WEB1 et votre enregistrement d’alias CNAME pour le serveur Web de serveur DNS est « infrastructure à clé publique », votre domaine est corp.contoso.com et que votre répertoire virtuel est nommé pki, l’emplacement AIA est :  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>Planifier l’opération de copie entre l’autorité de certification et le serveur Web  
Pour publier le certificat d’autorité de certification et de révocation de certificats à partir de l’autorité de certification pour le répertoire virtuel du serveur Web, vous pouvez exécuter la commande - crl certutil après avoir configuré les emplacements CDP et AIA sur l’autorité de certification. Vérifiez que vous configurez les chemins d’accès corrects sur les propriétés de l’autorité de certification **Extensions** onglet avant d’exécuter cette commande en suivant les instructions de ce guide. En outre, pour copier le certificat d’AC d’entreprise sur le serveur Web, vous devez avoir déjà créé le répertoire virtuel sur le serveur Web et configuré le dossier comme dossier partagé.  
  
## <a name="bkmk_template"></a>Planifiez la configuration de modèle de certificat de serveur sur l’autorité de certification  
Pour déployer des certificats de serveur inscrit automatiquement, vous devez copier le modèle de certificat nommé **serveur RAS et IAS**. Par défaut, cette copie est nommée **copie de serveur RAS et IAS**. Si vous souhaitez renommer cette copie du modèle, le nom que vous souhaitez utiliser au cours de cette étape de déploiement du plan.  
  
> [!NOTE]  
> Les sections de trois dernières déploiement dans ce guide, qui vous permettent de configurer l’inscription automatique du certificat de serveur, d’actualiser la stratégie de groupe sur les serveurs et vérifiez que les serveurs ont reçu un certificat de serveur valide à partir de l’autorité de certification - ne nécessitent pas une planification supplémentaire étapes.  
  


