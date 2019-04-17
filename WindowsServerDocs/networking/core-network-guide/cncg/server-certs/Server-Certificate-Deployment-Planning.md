---
title: Planification du déploiement de certificats de serveur
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: eacfa404da91d14a7a7328646c2320be8220000c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment-planning"></a>Planification du déploiement de certificats de serveur

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Avant de déployer des certificats de serveur, vous devez planifier les éléments suivants:  
  
-   [Planifier la configuration du serveur de base](#bkmk_basic)  
  
-   [Planifier l’accès au domaine](#bkmk_domain)  
  
-   [Planifier l’emplacement et le nom du répertoire virtuel sur votre serveur Web](#bkmk_virtual)  
  
-   [Planifier un enregistrement d’alias (CNAME) DNS pour votre serveur Web](#bkmk_cname)  
  
-   [Planifier la configuration de CAPolicy.inf](#bkmk_capolicy)  
  
-   [Planifier la configuration des extensions CDP et AIA sur CA1](#bkmk_cdp)  
  
-   [Planifier l’opération de copie entre l’autorité de certification et le serveur Web](#bkmk_copy)  
  
-   [Planifier la configuration du modèle de certificat de serveur sur l’autorité de certification](#bkmk_template)  
  
## <a name="bkmk_basic"></a>Planifier la configuration du serveur de base  
Une fois que vous installez Windows Server2016 sur les ordinateurs que vous prévoyez d’utiliser en tant que votre autorité de certification et le serveur Web, vous devez renommer l’ordinateur et affecter et configurer une adresse IP statique pour l’ordinateur local.  
  
Pour plus d’informations, consultez Windows Server2016 [Guide réseau de base](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_domain"></a>Planifier l’accès au domaine  
Pour vous connecter au domaine, l’ordinateur doit être un ordinateur membre du domaine et le compte d’utilisateur doit être créé dans ADDS avant la tentative d’ouverture de session. En outre, la plupart des procédures décrites dans ce guide requièrent que le compte d’utilisateur est membre des groupes Administrateurs de l’entreprise ou Admins du domaine dans ActiveDirectory utilisateurs et ordinateurs, donc vous devez vous connecter à l’autorité de certification avec un compte qui dispose de l’appartenance au groupe approprié.  
  
Pour plus d’informations, consultez Windows Server2016 [Guide réseau de base](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_virtual"></a>Planifier l’emplacement et le nom du répertoire virtuel sur votre serveur Web  
Pour fournir l’accès à la liste de révocation et le certificat d’autorité de certification sur d’autres ordinateurs, vous devez stocker ces éléments dans un répertoire virtuel sur votre serveur Web. Dans ce guide, le répertoire virtuel se trouve sur le serveur Web WEB1. Ce dossier se trouve sur le lecteur «C:» et est nommé «infrastructure à clé publique.» Vous pouvez le localiser votre répertoire virtuel sur votre serveur Web à n’importe quel emplacement de dossier qui est approprié pour votre déploiement.  
  
## <a name="bkmk_cname"></a>Planifier un enregistrement d’alias (CNAME) DNS pour votre serveur Web  
Enregistrements de ressource alias (CNAME) sont parfois appelés des enregistrements de ressource de nom canonique. Avec ces enregistrements, vous pouvez utiliser plusieurs noms pour pointer vers un seul ordinateur hôte, ce qui facilite le faire des éléments tels que l’ordinateur hôte d’un serveur de protocole FTP (File Transfer) et un serveur Web sur le même ordinateur. Par exemple, les noms de serveurs bien connus (ftp, www) sont enregistrés à l’aide des enregistrements de ressource alias (CNAME) qui sont mappées au système DNS (Domain Name) nom d’hôte, tels que WEB1, pour l’ordinateur du serveur qui héberge ces services.  
  
Ce guide fournit des instructions pour la configuration de votre serveur Web pour héberger la liste de révocation de certificats (CRL) pour votre autorité de certification (CA). Étant donné que vous souhaiterez peut-être également d’utiliser votre serveur Web à d’autres fins, comme pour héberger un FTP ou un site Web, il est conseillé de créer un enregistrement de ressource alias dans DNS pour votre serveur Web. Dans ce guide, l’enregistrement CNAME est nommé «infrastructure à clé publique», mais vous pouvez choisir un nom qui est approprié pour votre déploiement.  
  
## <a name="bkmk_capolicy"></a>Planifier la configuration de CAPolicy.inf  
Avant d’installer les services ADCS, vous devez configurer CAPolicy.inf sur l’autorité de certification avec les informations sont correctes pour votre déploiement. Un fichier CAPolicy.inf contient les informations suivantes:  
  
```  
[Version]  
Signature="$Windows NT$"  
[PolicyStatementExtension]  
Policies=InternalPolicy  
[InternalPolicy]  
OID=1.2.3.4.1455.67.89.5  
Notice="Legal Policy Statement"  
URL=http://pki.corp.contoso.com/pki/cps.txt  
[Certsrv_Server]  
RenewalKeyLength=2048  
RenewalValidityPeriod=Years  
RenewalValidityPeriodUnits=5  
CRLPeriod=weeks  
CRLPeriodUnits=1  
LoadDefaultTemplates=0  
AlternateSignatureAlgorithm=1  
```  
Vous devez planifier les éléments suivants pour ce fichier:  
  
-   **URL**. L’exemple de fichier CAPolicy.inf a la valeur URL **http://pki.corp.contoso.com/pki/cps.txt**. Il s’agit, car le serveur Web dans ce guide est nommé WEB1 et a un enregistrement de ressource CNAME DNS de l’infrastructure à clé publique. Le serveur Web est également joint au domaine corp.contoso.com. En outre, il est un répertoire virtuel sur le serveur Web nommé «infrastructure à clé publique «où se trouve la liste de révocation de certificats. Vérifiez que la valeur que vous fournissez pour l’URL dans votre fichier CAPolicy.inf pointe vers un répertoire virtuel sur votre serveur Web dans votre domaine.  
  
-   **RenewalKeyLength**. La longueur de clé de renouvellement par défaut des services ADCS dans Windows Server2012 est 2048. La longueur de clé que vous sélectionnez doit être aussi longtemps que possible tout en assurant la compatibilité avec les applications que vous prévoyez d’utiliser.  
  
-   **RenewalValidityPeriodUnits**. L’exemple de fichier CAPolicy.inf a une valeur de RenewalValidityPeriodUnits de 5ans. Il s’agit, car la durée de vie prévue de l’autorité de certification est de 10ans environ. La valeur de RenewalValidityPeriodUnits doit refléter la période de validité globale de l’autorité de certification ou le plus grand nombre d’années pour lequel vous voulez fournir l’inscription.  
  
-   **CRLPeriodUnits**. L’exemple de fichier CAPolicy.inf a la valeur CRLPeriodUnits 1. Il s’agit de l’intervalle d’actualisation de la liste de révocation de certificats dans ce guide exemple étant 1 semaine. La valeur d’intervalle que vous spécifiez avec ce paramètre, vous devez publier la liste CRL sur l’autorité de certification dans le répertoire virtuel du serveur Web où vous stockez la liste de révocation et assurer l’accès pour les ordinateurs qui se trouvent dans le processus d’authentification.  
  
-   **AlternateSignatureAlgorithm**. Cette CAPolicy.inf implémente un mécanisme de sécurité améliorées en implémentant les autres formats de signature. Vous ne devez pas implémenter ce paramètre si vous avez des clients WindowsXP qui nécessitent des certificats à partir de cette autorité de certification.  
  
Si vous ne comptez pas sur l’ajout de toutes les autorités de certification secondaires pour votre infrastructure à clé publique à un moment ultérieur, et si vous souhaitez empêcher l’ajout de toutes les autorités de certification secondaires, vous pouvez ajouter la clé PathLength dans votre fichier CAPolicy.inf avec la valeur 0. Pour ajouter cette clé, copiez et collez le code suivant dans votre fichier:  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> Il est déconseillé de modifier tous les autres paramètres dans le fichier CAPolicy.inf, sauf si vous avez une raison spécifique de le faire.  
  
## <a name="bkmk_cdp"></a>Planifier la configuration des extensions CDP et AIA sur CA1  
Lorsque vous configurez le Point de Distribution de liste de révocation de certificats (CRL) (CDP) et les paramètres d’accès des informations autorité (AIA) sur CA1, vous devez le nom de votre serveur Web et votre nom de domaine. Vous devez également le nom du répertoire virtuel que vous créez sur votre serveur Web où sont stockés la liste de révocation de certificats (CRL) et le certificat d’autorité de certification.  
  
L’emplacement du CDP que vous devez entrer pendant cette étape de déploiement est au format:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
Par exemple, si votre serveur Web est nommé WEB1 et votre DNS enregistrement d’alias CNAME pour le serveur Web est «infrastructure à clé publique», votre domaine est corp.contoso.com et votre répertoire virtuel pki, l’emplacement du CDP est:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
L’emplacement AIA que vous devez entrer est au format:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
Par exemple, si votre serveur Web est nommé WEB1 et votre DNS enregistrement d’alias CNAME pour le serveur Web est «infrastructure à clé publique», votre domaine est corp.contoso.com et votre répertoire virtuel pki, l’emplacement AIA est:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>Planifier l’opération de copie entre l’autorité de certification et le serveur Web  
Pour publier le certificat d’autorité de certification et de révocation de certificats à partir de l’autorité de certification pour le répertoire virtuel du serveur Web, vous pouvez exécuter la commande de révocation de certificats - certutil après avoir configuré les emplacements CDP et AIA sur l’autorité de certification. Veillez à configurer les chemins d’accès correctes sur les propriétés de l’autorité de certification **Extensions** onglet avant d’exécuter cette commande en suivant les instructions de ce guide. En outre, pour copier le certificat d’autorité de certification entreprise sur le serveur Web, vous devez avoir déjà créé le répertoire virtuel sur le serveur Web et configuré le dossier comme un dossier partagé.  
  
## <a name="bkmk_template"></a>Planifier la configuration du modèle de certificat de serveur sur l’autorité de certification  
Pour déployer des certificats de serveur inscrit automatiquement, vous devez copier le modèle de certificat nommé **serveur RAS et IAS**. Par défaut, cette copie est nommée **copie de serveur RAS et IAS**. Si vous souhaitez renommer cette copie du modèle, planifiez le nom que vous souhaitez utiliser au cours de cette étape de déploiement.  
  
> [!NOTE]  
> Les sections trois derniers déploiement dans ce guide, qui vous permettent de configurer l’inscription automatique du certificat de serveur, d’actualiser la stratégie de groupe sur les serveurs et vérifient que les serveurs ont reçu un certificat de serveur valide à partir de l’autorité de certification - ne nécessitent pas d’étapes de planification supplémentaires.  
  


