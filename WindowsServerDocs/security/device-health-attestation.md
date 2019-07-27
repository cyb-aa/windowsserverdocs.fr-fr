---
title: Attestation d’intégrité de l’appareil
H1: na
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e7b77a4-1c6a-4c21-8844-0df89b63f68d
author: brianlic-msft
ms.date: 10/12/2016
ms.openlocfilehash: 888992366f8a722c4834f23e08a393c829b47a26
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544623"
---
# <a name="device-health-attestation"></a>Attestation d’intégrité de l’appareil

>S'applique à : Windows Server 2016

Introduite dans Windows 10 version 1507, l’attestation d’intégrité de l’appareil inclut ce qui suit :

-   Elle s’intègre à l’infrastructure de gestion des appareils mobiles Windows 10 conformément aux [normes Open Mobile Alliance (OMA)](http://openmobilealliance.org/).

-   Elle prend en charge les appareils qui ont un module de plateforme sécurisée (TPM) configuré dans un microprogramme ou un format discret.

-   Elle permet aux entreprises d’élever la sécurité de leur organisation au niveau d’une sécurité attestée et surveillée du matériel, avec un impact minime ou inexistant sur le coût d’exploitation.

À compter de Windows Server 2016, vous pouvez maintenant exécuter le service Attestation d’intégrité de l’appareil en tant que rôle serveur au sein de votre organisation. Utilisez cette rubrique pour savoir comment installer et configurer le rôle serveur Attestation d’intégrité de l’appareil.

## <a name="overview"></a>Vue d'ensemble

Vous pouvez utiliser le service Attestation d’intégrité de l’appareil pour évaluer l’intégrité des appareils suivants :
  
-   Appareils Windows 10 et Windows 10 Mobile qui prennent en charge TPM 1.2 ou 2.0.  
-   Appareils locaux gérés à l’aide d’Active Directory avec accès Internet, appareils gérés à l’aide d’Active Directory sans accès Internet, appareils gérés par un Azure Active Directory ou un déploiement hybride qui utilise à la fois Active Directory et Azure Active Directory.


### <a name="dha-service"></a>Service Attestation d’intégrité de l’appareil

Le service Attestation d’intégrité de l’appareil valide les journaux TPM et PCR d’un appareil, puis émet un rapport d’attestation d’intégrité de l’appareil. Microsoft propose le service Attestation d’intégrité de l’appareil de trois façons :

- **Service cloud Attestation d’intégrité de l’appareil** : service d’attestation d’intégrité de l’appareil géré par Microsoft qui est gratuit, à charge géo-équilibrée et optimisé pour l’accès depuis différentes régions du monde.

- **Service Attestation d’intégrité de l’appareil local** : nouveau rôle serveur introduit dans Windows Server 2016. Disponible gratuitement pour les clients qui possèdent une licence Windows Server 2016.

- **Service cloud Attestation d’intégrité de l’appareil Azure** : hôte virtuel dans Microsoft Azure. Pour cela, vous avez besoin d’un hôte virtuel et de licences pour le service local Attestation d’intégrité de l’appareil.

Le service Attestation d’intégrité de l’appareil s’intègre aux solutions de gestion des appareils mobiles et offre ce qui suit : 

-   Il combine les informations qu’elles reçoivent des appareils (par le biais des canaux de communication de gestion des appareils existants) avec le rapport d’attestation d’intégrité de l’appareil.
-   Il permet de prendre une décision en matière de sécurité plus sûre et fiable selon les données attestées et protégées par le matériel.

Voici un exemple d’utilisation du service Attestation d’intégrité de l’appareil pour élever le niveau de sécurité des ressources de votre organisation.

1. Vous créez une stratégie qui vérifie la configuration/les attributs de démarrage suivants :
   - Démarrage sécurisé
   - BitLocker
   - Logiciel anti-programme malveillant à lancement anticipé
2. La solution de gestion des appareils mobiles applique cette stratégie et déclenche une mesure corrective, selon les données de rapport d’attestation d’intégrité de l’appareil.  Par exemple, elle peut vérifier les points suivants :
   - Le démarrage sécurisé a été activé, l’appareil a chargé du code de confiance authentique et le chargeur de démarrage Windows n’était pas falsifié.
   - Le démarrage sécurisé a correctement vérifié la signature numérique du noyau Windows et les composants qui ont été chargés pendant le démarrage de l’appareil.
   - Le démarrage mesuré a créé une piste d’audit protégée par le module de plateforme sécurisée qui a pu être vérifiée à distance.
   - BitLocker a été activé et a protégé les données quand l’appareil s’est éteint.
   - Un logiciel anti-programme malveillant à lancement anticipé a été activé dès les premiers stades du démarrage et il surveille l’exécution.
  
#### <a name="dha-cloud-service"></a>Service cloud Attestation d’intégrité de l’appareil

Le service cloud Attestation d’intégrité de l’appareil offre les avantages suivants :

-   Il passe en revue les journaux de démarrage des appareils TCG et PCR qu’il reçoit d’un appareil inscrit avec une solution de gestion des appareils mobiles. 
-   Il crée un rapport résistant aux falsifications et de mise en évidence des falsifications (rapport d’attestation d’intégrité de l’appareil) qui décrit le mode de démarrage de l’appareil en fonction des données collectées et protégées par la puce du module de plateforme sécurisée (TPM) d’un appareil. 
-   Il remet le rapport d’attestation d’intégrité de l’appareil au serveur de gestion des appareils mobiles qui l’a demandé dans un canal de communication protégé.

#### <a name="dha-on-premises-service"></a>Service local Attestation d’intégrité de l’appareil

Le service local Attestation d’intégrité de l’appareil offre toutes les fonctionnalités offertes par le service cloud Attestation d’intégrité de l’appareil.  Il permet également aux clients de :

 - optimiser les performances en exécutant le service Attestation d’intégrité de l’appareil dans votre propre centre de données,
 - vérifier que le rapport d’attestation d’intégrité de l’appareil ne quitte pas votre réseau.

#### <a name="dha-azure-cloud-service"></a>Service cloud Attestation d’intégrité de l’appareil Azure

Ce service fournit les mêmes fonctionnalités que le service local Attestation d’intégrité de l’appareil, si ce n’est que le service de cloud Attestation d’intégrité de l’appareil Azure s’exécute en tant qu’hôte virtuel dans Microsoft Azure.

### <a name="dha-validation-modes"></a>Modes de validation d’attestation d’intégrité de l’appareil

Vous pouvez configurer le service local Attestation d’intégrité de l’appareil pour qu’il s’exécute en mode de validation EKCert ou AIKCert. Quand le service émet un rapport, il indique s’il a été émis en mode de validation AIKCert ou EKCert. Les modes de validation EKCert et AIKCert offrent la même garantie de sécurité tant que la chaîne EKCert de confiance est tenue à jour.

#### <a name="ekcert-validation-mode"></a>Mode de validation EKCert

Le mode de validation EKCert est optimisé pour les appareils utilisés dans les organisations qui ne sont pas connectées à Internet. Les appareils qui se connectent à un service Attestation d’intégrité de l’appareil s’exécutant en mode de validation EKCert n’ont **pas** d’accès direct à Internet.

Quand le service s’exécute en mode de validation EKCert, il s’appuie sur une chaîne d’approbation gérée par l’entreprise devant être mise à jour régulièrement (entre 5 et 10 fois par an). 

Microsoft publie des packages agrégés d’autorités de certification (AC) racines de confiance et intermédiaires pour les fabricants de TPM approuvés (dès qu’ils sont disponibles) dans une archive accessible publiquement au format .cab. Vous devez télécharger le flux, valider son intégrité et l’installer sur le serveur exécutant le service Attestation d’intégrité de l’appareil.

Exemple d’archive [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925).

#### <a name="aikcert-validation-mode"></a>Mode de validation AIKCert

Le mode de Validation AIKCert est optimisé pour les environnements d’exploitation qui ont accès à Internet. Les appareils qui se connectent à un service Attestation d’intégrité de l’appareil s’exécutant en mode de validation AIKCert doivent avoir un accès direct à Internet et être en mesure d’obtenir un certificat AIK auprès de Microsoft. 

## <a name="install-and-configure-the-dha-service-on-windows-server-2016"></a>Installer et configurer le service Attestation d’intégrité de l’appareil sur Windows Server 2016

Utilisez les sections suivantes pour installer et configurer le service Attestation d’intégrité de l’appareil sur Windows Server 2016.

### <a name="prerequisites"></a>Prérequis

Pour configurer et vérifier un service local Attestation d’intégrité de l’appareil, vous avez besoin de ce qui suit :

- Un serveur qui exécute Windows Server 2016.
- Un ou plusieurs appareils clients Windows 10 avec un module de plateforme sécurisée (TPM) (1.2 ou 2.0) qui se trouve dans un état transparent/prêt et qui exécute la build la plus récente de Windows Insider.
- Déterminez si vous allez exécuter le mode de validation EKCert ou AIKCert.
- Les certificats suivants :
  - **Certificat SSL d’attestation d’intégrité de l’appareil** : certificat SSL x.509 lié à une racine approuvée d’entreprise avec une clé privée exportable. Ce certificat protège les communications de données d’attestation d’intégrité de l’appareil en transit, notamment les communications de serveur à serveur (service d’attestation d’intégrité de l’appareil et serveur de gestion des appareils mobiles) et de serveur à client (service d’attestation d’intégrité de l’appareil et appareil Windows 10).
  - **Certificat de signature d’attestation d’intégrité de l’appareil** : certificat x.509 lié à une racine approuvée d’entreprise avec une clé privée exportable. Le service Attestation d’intégrité de l’appareil utilise ce certificat pour la signature numérique. 
  - **Certificat de chiffrement d’attestation d’intégrité de l’appareil** : certificat x.509 lié à une racine approuvée d’entreprise avec une clé privée exportable. Le service Attestation d’intégrité de l’appareil utilise également ce certificat pour le chiffrement. 


### <a name="install-windows-server-2016"></a>Installer Windows Server 2016

Installez Windows Server 2016 en utilisant la méthode d’installation de votre choix, comme les services de déploiement Windows ou en exécutant le programme d’installation à partir d’un média de démarrage, d’une clé USB ou du système de fichiers local. S’il s’agit de la première fois que vous configurez le service local Attestation d’intégrité de l’appareil, vous devez installer Windows Server 2016 à l’aide de l’option d’installation **Expérience utilisateur**.

### <a name="add-the-device-health-attestation-server-role"></a>Ajouter le rôle serveur Attestation d’intégrité de l’appareil

Vous pouvez installer le rôle serveur Attestation d’intégrité de l’appareil et ses dépendances à l’aide du Gestionnaire de serveur. 

Une fois que vous avez installé Windows Server 2016, l’appareil redémarre et ouvre le Gestionnaire de serveur. Si le Gestionnaire de serveur ne démarre pas automatiquement, cliquez sur **Démarrer**, puis sur **Gestionnaire de serveur**.

1.  Cliquez sur **Ajouter des rôles et des fonctionnalités**.
2.  Dans la page **Avant de commencer** , cliquez sur **Suivant**.
3.  Dans la page **Sélectionner le type d’installation**, cliquez sur **Installation basée sur un rôle ou une fonctionnalité**, puis sur **Suivant**.
4.  Dans la page **Sélectionner le serveur de destination**, cliquez sur **Sélectionner un serveur du pool de serveurs**, sélectionnez le serveur, puis cliquez sur **Suivant**.
5.  Dans la page **Sélectionner des rôles de serveurs**, cochez la case **Attestation d’intégrité de l’appareil**.
6.  Cliquez sur **Ajouter des fonctionnalités** pour installer d’autres services et fonctionnalités de rôle requis.
7.  Cliquez sur **Suivant**.
8.  Dans la page **Sélectionner les fonctionnalités** , cliquez sur **Suivant**.
9.  Dans la page **Rôle de serveur web (IIS)** , cliquez sur **Suivant**.
10. Dans la page **Sélectionner des services de rôle**, cliquez sur **Suivant**.
11. Dans la page **Service d’attestation d’intégrité de l’appareil**, cliquez sur **Suivant**.
12. Dans la page **Confirmer les sélections d’installation** , cliquez sur **Installer**.
13. Une fois l’installation terminée, cliquez sur **Fermer**.

### <a name="install-the-signing-and-encryption-certificates"></a>Installer les certificats de signature et de chiffrement

Utilisez le script Windows PowerShell suivant pour installer les certificats de signature et de chiffrement. Pour plus d’informations sur l’empreinte numérique [, consultez Procédure: Récupérez l’empreinte numérique d'](https://msdn.microsoft.com/library/ms734695.aspx)un certificat.

```
$key = Get-ChildItem Cert:\LocalMachine\My | Where-Object {$_.Thumbprint -like "<thumbprint>"}
$keyname = $key.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
$keypath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\" + $keyname
icacls $keypath /grant <username>`:R
  
#<thumbprint>: Certificate thumbprint for encryption certificate or signing certificate
#<username>: Username for web service app pool, by default IIS_IUSRS
```

### <a name="install-the-trusted-tpm-roots-certificate-package"></a>Installer le package de certification racine de confiance TPM

Pour installer le package de certification racine de confiance TPM, vous devez l’extraire, supprimer toutes les chaînes de confiance qui ne sont pas approuvées par votre organisation, puis exécuter setup.cmd.

#### <a name="download-the-trusted-tpm-roots-certificate-package"></a>Télécharger le package de certification racine de confiance TPM

Avant d’installer le package de certificats, vous pouvez télécharger la dernière liste des racines TPM approuvées à partir de [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925).

> **Important :** Avant d’installer le package, vérifiez qu’il est signé numériquement par Microsoft.

#### <a name="extract-the-trusted-certificate-package"></a>Extraire le package de certification de confiance
Extrayez le package de certification de confiance en exécutant les commandes suivantes.
```
mkdir .\TrustedTpm
expand -F:* .\TrustedTpm.cab .\TrustedTpm
```

#### <a name="remove-the-trust-chains-for-tpm-vendors-that-are-not-trusted-by-your-organization-optional"></a>Supprimez les chaînes d’approbation pour les fournisseurs TPM *non* approuvés par votre organisation (facultatif).

Supprimez les dossiers des chaînes d’approbation de fournisseurs TPM ne sont pas approuvées par votre organisation.

> **Remarque :** Si vous utilisez le mode de certificat AIK, le dossier Microsoft est requis pour valider les certificats AIK émis par Microsoft.

#### <a name="install-the-trusted-certificate-package"></a>Installer le package de certification de confiance
Installez le package de certification de confiance en exécutant le script d’installation à partir du fichier .cab.

```
.\setup.cmd
```

### <a name="configure-the-device-health-attestation-service"></a>Configurer le service Attestation d’intégrité de l’appareil

Vous pouvez utiliser Windows PowerShell pour configurer le service local Attestation d’intégrité de l’appareil.

```
Install-DeviceHealthAttestation -EncryptionCertificateThumbprint <encryption> -SigningCertificateThumbprint <signing> -SslCertificateStoreName My -SslCertificateThumbprint <ssl> -SupportedAuthenticationSchema "<schema>"

#<encryption>: Thumbprint of the encryption certificate
#<signing>: Thumbprint of the signing certificate
#<ssl>: Thumbprint of the SSL certificate
#<schema>: Comma-delimited list of supported schemas including AikCertificate, EkCertificate, and AikPub
```

### <a name="configure-the-certificate-chain-policy"></a>Configurer la stratégie de chaînes de certificats

Configurez la stratégie de chaînes de certificats en exécutant le script Windows PowerShell suivant.

```
$policy = Get-DHASCertificateChainPolicy
$policy.RevocationMode = "NoCheck"
Set-DHASCertificateChainPolicy -CertificateChainPolicy $policy
```

## <a name="dha-management-commands"></a>Commandes de gestion d’attestation d’intégrité de l’appareil

Voici quelques exemples Windows PowerShell pour vous aider à gérer le service Attestation d’intégrité de l’appareil.

### <a name="configure-the-dha-service-for-the-first-time"></a>Configurer le service Attestation d’intégrité de l’appareil pour la première fois

```
Install-DeviceHealthAttestation -SigningCertificateThumbprint "<HEX>" -EncryptionCertificateThumbprint "<HEX>" -SslCertificateThumbprint "<HEX>" -Force
```

### <a name="remove-the-dha-service-configuration"></a>Supprimer la configuration du service Attestation d’intégrité de l’appareil

```
Uninstall-DeviceHealthAttestation -RemoveSslBinding -Force
```
### <a name="get-the-active-signing-certificate"></a>Obtenir le certificat de signature actif

```
Get-DHASActiveSigningCertificate
```
### <a name="set-the-active-signing-certificate"></a>Définir le certificat de signature actif

```
Set-DHASActiveSigningCertificate -Thumbprint "<hex>" -Force
```

> **Remarque :** Ce certificat doit être déployé sur le serveur qui exécute le service DHA dans le magasin de certificats **LocalMachine\My** . Quand le certificat de signature actif est défini, le certificat de signature actif existant est déplacé vers la liste des certificats de signature inactifs.

### <a name="list-the-inactive-signing-certificates"></a>Répertorier les certificats de signature inactifs
```
Get-DHASInactiveSigningCertificates
```

### <a name="remove-any-inactive-signing-certificates"></a>Supprimer tous les certificats de signature inactifs
```
Remove-DHASInactiveSigningCertificates -Force
Remove-DHASInactiveSigningCertificates  -Thumbprint "<hex>" -Force
```

> **Remarque :** *Un* seul certificat inactif (de tout type) peut exister dans le service à tout moment. Les certificats doivent être supprimés de la liste de certificats inactifs une fois qu’ils sont devenus inutiles.

### <a name="get-the-active-encryption-certificate"></a>Obtenir le certificat de chiffrement actif

```
Get-DHASActiveEncryptionCertificate
```

### <a name="set-the-active-encryption-certificate"></a>Définir le certificat de chiffrement actif

```
Set-DHASActiveEncryptionCertificate -Thumbprint "<hex>" -Force
```

Le certificat doit être déployé sur l’appareil dans le magasin de certificats **LocalMachine\My**. 

Quand le certificat de chiffrement actif est défini, le certificat de chiffrement actif existant est déplacé vers la liste des certificats de chiffrement inactif.

### <a name="list-the-inactive-encryption-certificates"></a>Répertorier les certificats de chiffrement inactifs

```
Get-DHASInactiveEncryptionCertificates
```
### <a name="remove-any-inactive-encryption-certificates"></a>Supprimer tous les certificats de chiffrement inactifs

```
Remove-DHASInactiveEncryptionCertificates -Force
Remove-DHASInactiveEncryptionCertificates -Thumbprint "<hex>" -Force 
```

### <a name="get-the-x509chainpolicy-configuration"></a>Obtenir la configuration X509ChainPolicy 

```
Get-DHASCertificateChainPolicy
```
### <a name="change-the-x509chainpolicy-configuration"></a>Modifier la configuration X509ChainPolicy

```
$certificateChainPolicy = Get-DHASInactiveEncryptionCertificates
$certificateChainPolicy.RevocationFlag = <X509RevocationFlag>
$certificateChainPolicy.RevocationMode = <X509RevocationMode>
$certificateChainPolicy.VerificationFlags = <X509VerificationFlags>
$certificateChainPolicy.UrlRetrievalTimeout = <TimeSpan>
Set-DHASCertificateChainPolicy = $certificateChainPolicy
```

## <a name="dha-service-reporting"></a>Rapports du service Attestation d’intégrité de l’appareil

Voici la liste des messages qui sont signalés par le service Attestation d’intégrité de l’appareil à la solution de gestion des appareils mobiles : 

- **200** HTTP OK. Le certificat est renvoyé.
- **400** Requête incorrecte. Format de requête non valide, certificat d’intégrité non valide, la signature de certificat ne correspond pas, objet blob d’attestation d’intégrité non valide ou objet blob d’état d’intégrité non valide. La réponse contient également un message, comme décrit par le schéma de réponse, avec un code d’erreur et un message d’erreur utilisables à des fins de diagnostic.
- **500** Erreur de serveur interne. Cette erreur peut se produire s’il existe des problèmes qui empêchent le service d’émettre des certificats.
- **503** La limitation rejette des requêtes pour éviter une surcharge du serveur. 
