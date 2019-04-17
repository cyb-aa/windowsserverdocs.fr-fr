---
title: "Attestation d’intégrité de l’appareil"
H1: na
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology:
- techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e7b77a4-1c6a-4c21-8844-0df89b63f68d
author: brianlic-msft
ms.date: 10/12/2016
ms.openlocfilehash: d304ee3456f8db1e5b202c1d9221d1374a5251be
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="device-health-attestation"></a>Attestation d’intégrité de l’appareil

>S’applique à: Windows Server2016

Introduite dans Windows10, version1507 (attestation d’intégrité périphérique) inclut ce qui suit:

-   S’intègre à l’infrastructure de gestion de périphériques mobiles (GPM) de Windows10 conformément aux [normes Open Mobile Alliance (OMA)](http://openmobilealliance.org/).

-   Prend en charge les périphériques qui ont une plateforme de Module sécurisée (TPM) configuré dans un format discret ou le microprogramme.

-   Elle permet aux entreprises à élever la sécurité de leur organisation au matériel attestée et surveillée du sécurité, avec un impact minime ou aucun impact sur les coûts d’exploitation.

À partir de Windows Server2016, vous pouvez maintenant exécuter le service comme rôle serveur au sein de votre organisation. Utilisez cette rubrique pour savoir comment installer et configurer le rôle de serveur d’Attestation d’intégrité de l’appareil.

## <a name="overview"></a>Vue d’ensemble

Vous pouvez utiliser le service pour évaluer l’intégrité des appareils:
  
-   Appareils Windows10 et Windows10Mobile qui prennent en charge le module TPM 1.2 ou 2.0.  
-   Les périphériques qui sont gérés à l’aide d’ActiveDirectory avec accès à Internet, les périphériques qui sont gérés à l’aide d’ActiveDirectory sans accès à Internet, appareils gérés par Azure ActiveDirectory ou un déploiement hybride à l’aide d’ActiveDirectory et Azure ActiveDirectory sur site.


### <a name="dha-service"></a>Service de service

Le service valide les journaux TPM et PCR d’un périphérique et puis émet un rapport de service. Microsoft propose le service de trois façons:

- **Service cloud de service** service géré par Microsoft A un service qui est libre, géo-équilibrée et optimisé pour l’accès depuis différentes régions du monde.

- **Service local service** un nouveau rôle serveur introduit dans Windows Server2016. Il est disponible gratuitement pour les clients qui possèdent une licence Windows Server2016.

- **Service cloud Azure service** un hôte virtuel dans MicrosoftAzure. Pour ce faire, vous devez un hôte virtuel et les licences pour le service local.

Le service s’intègre aux solutions GPM et fournit les avantages suivants: 

-   Il combine les informations qu’ils reçoivent des appareils (par le biais des canaux de communication gestion appareil existant) avec l’état du service
-   Prendre une décision de sécurité plus sûre et fiable, fonction matériel données attestées et protégées

Voici un exemple qui montre comment vous pouvez utiliser le service pour aider à élever la protection de sécurité pour les ressources de votre organisation.

1. Vous créez une stratégie qui vérifie les configuration/les attributs de démarrage suivantes:
  - Démarrage sécurisé
  - BitLocker
  - ELAM
2. La solution MDM applique cette stratégie et déclenche une action corrective basée sur les données d’état de service.  Par exemple, elle peut vérifier les éléments suivants:
  - Le démarrage sécurisé a été activé, l’appareil a chargé le code de confiance qui est authentique et le chargeur de démarrage Windows n’a pas été falsifié.
  - Démarrage a correctement vérifié la signature numérique du noyau Windows et les composants qui ont été chargés pendant le démarrage de l’appareil de confiance.
  - Démarrage mesuré a créé une piste d’audit protégée par le module de plateforme sécurisée qui peut être vérifiée à distance.
  - Désactivation de BitLocker a été activé et a protégé les données lorsque le périphérique a été activé.
  - ELAM a été activée au dès les premières phases de démarrage et l’exécution de contrôle.
  
#### <a name="dha-cloud-service"></a>Service cloud de service

Le service cloud offre les avantages suivants:

-   Passe en revue les journaux de démarrage appareil TCG et PCR qu’il reçoit d’un appareil est inscrit avec une solution MDM. 
-   Crée un résistant aux falsifications et la falsification évidents (état Service) qui décrit la manière dont l’appareil démarrée sur la base de données qui sont collectées et protégées par la puce TPM d’un appareil. 
-   Fournit l’état de service pour le serveur GPM qui l’a demandé dans un canal de communication protégé.

#### <a name="dha-on-premises-service"></a>Service de service local

Le service local offre toutes les fonctionnalités offertes par le service de service cloud.  Il permet également aux clients:

 - Optimiser les performances en exécutant le service de service dans votre propre centre de données
 - Assurez-vous que l’état de service ne laisse pas votre réseau

#### <a name="dha-azure-cloud-service"></a>Service cloud Azure service

Ce service fournit les mêmes fonctionnalités que le service local, sauf que le service de cloud Azure service s’exécute comme un hôte virtuel dans MicrosoftAzure.

### <a name="dha-validation-modes"></a>Modes de validation de service

Vous pouvez configurer le service local pour s’exécuter en mode de validation EKCert ou AIKCert. Lorsque le service émet un rapport, il indique s’il a été émis en mode de validation AIKCert ou EKCert. Modes de validation EKCert et AIKCert offrent la même garantie de sécurité tant que la chaîne EKCert de confiance est tenue à jour.

#### <a name="ekcert-validation-mode"></a>Mode de validation EKCert

Mode de validation EKCert est optimisé pour les périphériques dans les organisations qui ne sont pas connectées à Internet. Effectuez des appareils qui se connectent à un service de service en cours d’exécution en mode de validation EKCert **pas** avoir un accès direct à Internet.

Lorsque le service s’exécute en mode de validation EKCert, il s’appuie sur une chaîne gérée par l’entreprise de confiance qui a besoin de mise à jour régulièrement (environ 5 et 10fois par an). 

Microsoft publie des packages agrégés des racines de confiance et intermédiaires l’autorité de certification pour les fabricants de TPM approuvés (dès qu’elles sont disponibles) dans une archive accessible publiquement dans l’archive .cab. Vous devez télécharger le flux, valider son intégrité et l’installer sur le serveur d’Attestation d’intégrité de l’appareil en cours d’exécution.

Exemple d’archive [https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab).

#### <a name="aikcert-validation-mode"></a>Mode de validation AIKCert

Mode de Validation AIKCert est optimisé pour les environnements d’exploitation qui n’ont pas accès à Internet. Périphériques se connectant à un service de service s’exécutant en mode de validation AIKCert doivent avoir un accès direct à Internet et sont en mesure d’obtenir un certificat AIK auprès de Microsoft. 

## <a name="install-and-configure-the-dha-service-on-windows-server-2016"></a>Installer et configurer le service sur Windows Server2016

Utilisez les sections suivantes pour obtenir le service installé et configuré sur Windows Server2016.

### <a name="prerequisites"></a>Conditions préalables

Pour configurer et vérifier un service local de service, vous devez:

- Un serveur exécutant Windows Server2016.
- Génération d’un (ou plusieurs) des périphériques client Windows10 avec un module de plateforme sécurisée (1.2 ou 2.0) qui se trouve dans un état transparent/prêt en cours d’exécution la plus récente de Windows Insider.
- Décider si vous souhaitez exécuter en mode de validation EKCert ou AIKCert.
- Les certificats suivants:
  - **Certificat SSL DHA** un certificat SSL x.509 lié à une racine approuvée d’entreprise avec une clé privée exportable. Ce certificat protège les communications de données de service en transit, notamment le serveur à serveur (service du service et serveur GPM) et serveur pour les communications client (service du service et un appareil Windows10).
  - **Certificat de signature du service** un certificat x.509 lié à une racine approuvée d’entreprise avec une clé privée exportable. Le service utilise ce certificat pour la signature numérique. 
  - **Certificat de chiffrement ADH** un certificat x.509 lié à une racine approuvée d’entreprise avec une clé privée exportable. Le service utilise également ce certificat pour le chiffrement. 


### <a name="install-windows-server-2016"></a>Installez Windows Server2016

Installez Windows Server2016 à l’aide de votre méthode d’installation par défaut, tels que les Services de déploiement Windows, ou en exécutant le programme d’installation à partir d’un média de démarrage, un lecteur USB ou le système de fichiers local. Si c’est la première fois que vous configurez le service local, vous devez installer Windows Server2016 à l’aide de la **expérience** option d’installation.

### <a name="add-the-device-health-attestation-server-role"></a>Ajouter le rôle de serveur d’Attestation d’intégrité de l’appareil

Vous pouvez installer le rôle de serveur d’Attestation d’intégrité de l’appareil et ses dépendances à l’aide du Gestionnaire de serveur. 

Une fois que vous avez installé Windows Server2016, l’appareil redémarre et ouvre le Gestionnaire de serveur. Si le Gestionnaire de serveur ne démarre pas automatiquement, cliquez sur **Démarrer**, puis cliquez sur **le Gestionnaire de serveur**.

1.  Cliquez sur **ajouter des rôles et fonctionnalités**.
2.  Sur le **avant de commencer** , cliquez sur **suivant**.
3.  Sur le **sélectionner le type d’installation**, cliquez sur **installation basée sur un rôle ou une fonctionnalité**, puis cliquez sur **suivant**.
4.  Sur le **serveur de destination sélectionnez**, cliquez sur **sélectionner un serveur du pool de serveurs**, sélectionnez le serveur, puis cliquez sur **suivant**.
5.  Sur le **sélectionner des rôles de serveur**, sélectionnez le **Attestation d’intégrité de l’appareil** case à cocher.
6.  Cliquez sur **ajouter des fonctionnalités** pour installer d’autres requis sur les services de rôle et fonctionnalités.
7.  Cliquez sur **suivant**.
8.  Sur le **sélectionner des fonctionnalités** , cliquez sur **suivant**.
9.  Sur le **rôle de serveur Web (IIS)**, cliquez sur **suivant**.
10. Sur le **sélectionner les services de rôle**, cliquez sur **suivant**.
11. Sur le **Service d’Attestation d’intégrité appareil**, cliquez sur **suivant**.
12. Sur le **confirmer les sélections d’installation** , cliquez sur **installer**.
13. Une fois l’installation terminée, cliquez sur **fermer**.

### <a name="install-the-signing-and-encryption-certificates"></a>Installer les certificats de signature et chiffrement

À l’aide du script Windows PowerShell suivant pour installer les certificats de signature et le chiffrement. Pour plus d’informations sur l’empreinte numérique, voir [Comment: récupérez l’empreinte numérique d’un certificat](https://msdn.microsoft.com/library/ms734695.aspx).

```
$key = Get-ChildItem Cert:\LocalMachine\My | Where-Object {$_.Thumbprint -like "<thumbprint>"}
$keyname = $key.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
$keypath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\" + $keyname
icacls $keypath /grant <username>`:R
  
#<thumbprint>: Certificate thumbprint for encryption certificate or signing certificate
#<username>: Username for web service app pool, by default IIS_IUSRS
```

### <a name="install-the-trusted-tpm-roots-certificate-package"></a>Installer le package certificat de racines de confiance TPM

Pour installer le package certificat de racines de confiance TPM, vous devez l’extraire, supprimer toutes les chaînes de confiance qui ne sont pas approuvées par votre organisation et puis exécuter setup.cmd.

#### <a name="download-the-trusted-tpm-roots-certificate-package"></a>Télécharger le package certificat de racines de confiance TPM

Avant d’installer le package de certification, vous pouvez télécharger la dernière liste de racines de module de plateforme sécurisée approuvés à partir de [https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab).

> **Important:** avant d’installer le package, vérifiez qu’elle est signée numériquement par Microsoft.

#### <a name="extract-the-trusted-certificate-package"></a>Extrayez le package de certification de confiance
Extrayez le package de certification de confiance en exécutant les commandes suivantes.
```
mkdir .\TrustedTpm
expand -F:* .\TrustedTpm.cab .\TrustedTpm
```

#### <a name="remove-the-trust-chains-for-tpm-vendors-that-are-not-trusted-by-your-organization-optional"></a>Supprimer les chaînes d’approbation pour les fournisseurs de module de plateforme sécurisée sont *pas* approuvés par votre organisation (facultatif)

Supprimer les dossiers pour les chaînes d’approbation de fournisseur de module de plateforme sécurisée qui ne sont pas approuvées par votre organisation.

> **Remarque:** si vous utilisez le mode de certificat AIK, le dossier Microsoft est nécessaire pour valider les certificats AIK émis de Microsoft.

#### <a name="install-the-trusted-certificate-package"></a>Installer le package de certification approuvé
Installez le package de certification de confiance en exécutant le script d’installation à partir du fichier .cab.

```
.\setup.cmd
```

### <a name="configure-the-device-health-attestation-service"></a>Configurer le service Attestation d’intégrité de l’appareil

Vous pouvez utiliser Windows PowerShell pour configurer le service local.

```
Install-DeviceHealthAttestation -EncryptionCertificateThumbprint <encryption> -SigningCertificateThumbprint <signing> -SslCertificateStoreName My -SslCertificateThumbprint <ssl> -SupportedAuthenticationSchema "<schema>"

#<encryption>: Thumbprint of the encryption certificate
#<signing>: Thumbprint of the signing certificate
#<ssl>: Thumbprint of the SSL certificate
#<schema>: Comma-delimited list of supported schemas including AikCertificate, EkCertificate, and AikPub
```

### <a name="configure-the-certificate-chain-policy"></a>Configurer la stratégie de chaîne de certificat

Configurer la stratégie de chaîne de certificat en exécutant le script Windows PowerShell suivant.

```
$policy = Get-DHASCertificateChainPolicy
$policy.RevocationMode = "NoCheck"
Set-DHASCertificateChainPolicy -CertificateChainPolicy $policy
```

## <a name="dha-management-commands"></a>Commandes de gestion de service

Voici quelques exemples de Windows PowerShell qui peuvent vous aider à gérer le service.

### <a name="configure-the-dha-service-for-the-first-time"></a>Configurer le service pour la première fois

```
Install-DeviceHealthAttestation -SigningCertificateThumbprint "<HEX>" -EncryptionCertificateThumbprint "<HEX>" -SslCertificateThumbprint "<HEX>" -Force
```

### <a name="remove-the-dha-service-configuration"></a>Supprimer la configuration du service service

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

> **Remarque:** ce certificat doit être déployé sur le serveur exécutant le service le **LocalMachine\My** magasin de certificats. Lorsque le certificat de signature actif est défini, le certificat de signature actif existant est déplacé vers la liste des certificats de signature inactifs.

### <a name="list-the-inactive-signing-certificates"></a>Liste de certificats de signature inactifs
```
Get-DHASInactiveSigningCertificates
```

### <a name="remove-any-inactive-signing-certificates"></a>Supprimez tous les certificats de signature inactifs
```
Remove-DHASInactiveSigningCertificates -Force
Remove-DHASInactiveSigningCertificates  -Thumbprint "<hex>" -Force
```

> **Remarque:** uniquement *un* certificat inactif (de n’importe quel type) peut exister dans le service à tout moment. Les certificats doivent être supprimés de la liste des certificats inactifs une fois qu’ils ne sont plus nécessaires.

### <a name="get-the-active-encryption-certificate"></a>Obtenir le certificat de chiffrement actif

```
Get-DHASActiveEncryptionCertificate
```

### <a name="set-the-active-encryption-certificate"></a>Définir le certificat de chiffrement actif

```
Set-DHASActiveEncryptionCertificate -Thumbprint "<hex>" -Force
```

Le certificat doit être déployé sur le périphérique dans le **LocalMachine\My** magasin de certificats. 

Lorsque le certificat de chiffrement actif est défini, le certificat de chiffrement actif existant est déplacé vers la liste des certificats de chiffrement inactifs.

### <a name="list-the-inactive-encryption-certificates"></a>Répertorier les certificats de chiffrement inactifs

```
Get-DHASInactiveEncryptionCertificates
```
### <a name="remove-any-inactive-encryption-certificates"></a>Supprimez tous les certificats de chiffrement inactifs

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

## <a name="dha-service-reporting"></a>Service service reporting

Voici une liste des messages qui sont signalés par le service à la solution MDM: 

- **200** OK HTTP. Le certificat est renvoyé.
- **400** demande incorrecte. Format de requête non valide, le certificat d’intégrité non valide, signature de certificat ne contient pas une correspondance, objet Blob d’Attestation d’intégrité non valide ou un Blob d’état d’intégrité non valide. La réponse contient également un message, comme décrit par le schéma de réponse, avec un code d’erreur et un message d’erreur qui peut être utilisé pour les tests de diagnostic.
- **500** erreur interne du serveur. Cela peut se produire s’il existe des problèmes qui empêchent le service d’émission de certificats.
- **503** la limitation rejette des requêtes pour éviter une surcharge du serveur. 
