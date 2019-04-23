---
title: Résolution des problèmes du Service Guardian hôte
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 424b8090-0692-49a6-9dc4-3c0e77d74b80
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 2dc9a612fa9760a6ca5f05efe1c287fd0872a1d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861250"
---
# <a name="troubleshooting-the-host-guardian-service"></a>Résolution des problèmes du Service Guardian hôte

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit les solutions aux problèmes courants rencontrés lors du déploiement ou le fonctionnement d’un serveur de Service de Guardian hôte (SGH) dans une infrastructure protégée.
Si vous n’êtes pas sûr de la nature de votre problème, essayez d’exécuter première le [service Guardian diagnostics de fabric](guarded-fabric-troubleshoot-diagnostics.md) sur vos serveurs SGH et les hôtes Hyper-V afin de réduire le risque provoque.

## <a name="certificates"></a>Certificats

SGH nécessite plusieurs certificats fonctionnent, y compris le chiffrement administrateur configuré et signature de certificat, ainsi qu’un certificat d’attestation géré par SGH lui-même.
Si ces certificats sont configurés incorrectement, SGH ne pourront pas traiter les demandes des ordinateurs hôtes Hyper-V souhaitant attester ou déverrouiller les protecteurs de clé pour les machines virtuelles protégées.
Les sections suivantes abordent les problèmes courants liés aux certificats configurés sur SGH.

### <a name="certificate-permissions"></a>Autorisations de certificat

SGH doit être en mesure d’accéder aux clés publiques et privées du chiffrement et les certificats ajoutés à SGH par l’empreinte du certificat de signature.
Plus précisément, le groupe géré compte de service (gMSA) qui exécute le service SGH a besoin d’accéder aux clés.
Pour rechercher le compte gMSA utilisé par SGH, exécutez la commande suivante dans une invite de PowerShell avec élévation de privilèges sur votre serveur SGH :

```powershell
(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName
```

Comment vous accorder l’accès au compte de service administré de groupe pour utiliser la clé privée dépend d’où est stockée la clé : sur l’ordinateur dans un fichier de certificat local, sur un module de sécurité matériel (HSM), ou à l’aide d’un fournisseur de stockage de clés personnalisés tiers.

#### <a name="grant-access-to-software-backed-private-keys"></a>Accorder l’accès à reposant sur les logiciels des clés privées

Si vous utilisez un certificat auto-signé ou un certificat émis par une autorité de certification est **pas** stockées dans un module de sécurité matériel ou le fournisseur de stockage personnalisé, vous pouvez modifier les autorisations de clé privées en effectuant le étapes suivantes :

1. Gestionnaire de certificats local Open (certlm.msc)
2. Développez **personnel > certificats** et recherchez le certificat de signature ou de chiffrement que vous souhaitez mettre à jour.
3. Cliquez avec le bouton droit sur le certificat et sélectionnez **toutes les tâches > Gérer les clés privées**.
4. Cliquez sur **ajouter** pour accorder un nouvel utilisateur l’accès à la clé privée du certificat.
5. Dans le sélecteur d’objet, entrez le nom de compte de service administré de groupe pour SGH trouvé précédemment, puis cliquez sur **OK**.
6. Vérifiez que le compte gMSA dispose **en lecture** accès au certificat.
7. Cliquez sur **OK** pour fermer la fenêtre de l’autorisation.

Si vous exécutez SGH sur Server Core ou que vous gérez à distance le serveur, vous ne serez pas en mesure de gérer les clés privées à l’aide du Gestionnaire de certificats local.
Au lieu de cela, vous devrez télécharger le [module PowerShell des outils de Fabric service Guardian](https://www.powershellgallery.com/packages/GuardedFabricTools) qui vous permettra de gérer les autorisations dans PowerShell.

1. Ouvrez une console PowerShell avec élévation de privilèges sur l’ordinateur Server Core ou utiliser la communication à distance PowerShell avec un compte disposant des autorisations d’administrateur local sur le service SGH.
2. Exécutez les commandes suivantes pour installer le module PowerShell des outils de Fabric service Guardian et accorder l’accès au compte de service administré de groupe à la clé privée.

```powershell
$certificateThumbprint = '<ENTER CERTIFICATE THUMBPRINT HERE>'

# Install the Guarded Fabric Tools module, if necessary
Install-Module -Name GuardedFabricTools -Repository PSGallery

# Import the module into the current session
Import-Module -Name GuardedFabricTools

# Get the certificate object
$cert = Get-Item "Cert:\LocalMachine\My\$certificateThumbprint"

# Get the gMSA account name
$gMSA = (Get-IISAppPool -Name KeyProtection).ProcessModel.UserName

# Grant the gMSA read access to the certificate
$cert.Acl = $cert.Acl | Add-AccessRule $gMSA Read Allow
```

#### <a name="grant-access-to-hsm-or-custom-provider-backed-private-keys"></a>Accorder l’accès à HSM ou les clés privées de reposant sur un fournisseur personnalisés

Si les clés privées de votre certificat sont assorties d’un module de sécurité matériel (HSM) ou un fournisseur personnalisé de stockage de clés (KSP), le modèle d’autorisation varient selon votre fournisseur de logiciel spécifique.
Pour de meilleurs résultats, consultez la documentation de votre fournisseur, ou prendre en charge de site pour plus d’informations sur clé privée comment les autorisations sont gérées pour votre appareil/logiciel spécifique.

Certains modules de sécurité matériel ne prennent pas en charge le lui accordant l’accès de comptes d’utilisateur spécifique à une clé privée ; au lieu de cela, ils permettent l’accès de compte d’ordinateur à toutes les clés dans un jeu de clés spécifique.
Pour ces appareils, il est généralement suffisant donner l’accès à l’ordinateur à vos clés et SGH sera en mesure de tirer parti de cette connexion.

**Conseils pour les modules de sécurité matériels**

Voici les options de configuration suggérée pour vous aider à correctement clés HSM d’utilisation avec SGH basé sur Microsoft et l’expérience de ses partenaires.
Ces conseils sont fournis pour vous faciliter la tâche et ne sont pas garantis pour être correct au moment de la lecture, ni approuvées par les fabricants HSM.
Pour des informations précises se rapportant à votre appareil si vous avez d’autres questions, contactez le fabricant de votre HSM.

Série/marque HSM      | Suggestion
----------------------|-------------
Gemalto SafeNet       | Vérifiez que la propriété de l’utilisation de clé dans le fichier de demande de certificat est définie à 0xa0, ce qui permet le certificat à utiliser pour la signature et chiffrement. En outre, vous devez accorder au compte de service administré de groupe *lire* accès à la clé privée à l’aide de l’outil Gestionnaire de certificats local (voir les étapes ci-dessus).
NShield de Thales        | Vérifiez que chaque nœud SGH a accès au monde de sécurité contenant les clés de signature et le chiffrement. Vous n’avez pas besoin configurer les autorisations de service administré de groupe spécifique.
Utimaco CryptoServers | Vérifiez que la propriété de l’utilisation de clé dans le fichier de demande de certificat est définie à 0 x 13, autorisant le certificat à utiliser pour le chiffrement, le déchiffrement et la signature.

### <a name="certificate-requests"></a>Demandes de certificat

Si vous utilisez une autorité de certification pour émettre vos certificats dans un environnement d’infrastructure à clé publique (PKI), vous devez vérifier que votre demande de certificat inclut la configuration minimale requise pour l’utilisation du SHG de ces clés.

**Certificats de signature**

Propriété de la CSR | Valeur requise
-------------|---------------
Algorithm    | RSA
Taille de clé     | Au moins 2 048 bits.
Utilisation de la clé    | Signature/Sign/DigitalSignature

**Certificats de chiffrement**

Propriété de la CSR | Valeur requise
-------------|---------------
Algorithm    | RSA
Taille de clé     | Au moins 2 048 bits.
Utilisation de la clé    | Encryption/Encrypt/DataEncipherment

**Modèles de Services de certificats Active Directory**

Si vous utilisez des modèles de certificats des Services de certificats Active Directory (ADCS) pour créer les certificats, d' que nous vous recommandons utilisent un modèle avec les paramètres suivants :

Propriété de modèle AD CS | Valeur requise
-----------------------|---------------
Catégorie de fournisseur      | Key Storage Provider
Nom de l’algorithme         | RSA
Taille de clé minimale       | 2 048
Objectif                | Signature et chiffrement
Extension de l’utilisation de la clé    | Chiffrement des données numériques Signature, chiffrement de clé (« autoriser le chiffrement des données utilisateur »)


### <a name="time-drift"></a>Dérive de temps

Si votre temps de serveur a considérablement dérivé par rapport à celui d’autres nœuds de SGH ou les hôtes Hyper-V dans votre infrastructure protégée, vous pouvez rencontrer des problèmes avec la validité du certificat signataire d’attestation.
Le certificat de signataire d’attestation est créé et renouvelé les coulisses de SGH et est utilisé pour signer des certificats d’intégrité émis pour les hôtes service Guardian par le Service d’Attestation.

Pour actualiser le certificat de signataire d’attestation, exécutez la commande suivante dans une invite de PowerShell avec élévation de privilèges.

```powershell
Start-ScheduledTask -TaskPath \Microsoft\Windows\HGSServer -TaskName 
AttestationSignerCertRenewalTask
```

Ou bien, vous pouvez exécuter la tâche planifiée manuellement en ouvrant **Planificateur de tâches** (taskschd.msc), accédant à **bibliothèque du Planificateur de tâches > Microsoft > Windows > HGSServer** et en cours d’exécution le tâche nommée **AttestationSignerCertRenewalTask**.

## <a name="switching-attestation-modes"></a>Basculement entre les Modes d’Attestation

Si vous passez SGH du mode de module de plateforme sécurisée pour le mode Active Directory ou inversement à l’aide de la [Set-HgsServer](https://technet.microsoft.com/library/mt652180.aspx) applet de commande, il peut prendre jusqu'à 10 minutes pour chaque nœud dans votre cluster SGH pour démarrer l’application le nouveau mode d’attestation.
Ce comportement est normal.
Il est conseillé de ne pas supprimer toutes les stratégies permettant d’hôtes du mode d’attestation précédente jusqu'à ce que vous avez vérifié que tous les ordinateurs hôtes sont attestant avec succès à l’aide du nouveau mode d’attestation.

**Problème connu lors du passage du module de plateforme sécurisée en mode AD**

Si vous initialisé votre cluster SGH dans le mode de module de plateforme sécurisée et plus tard de passer en mode Active Directory, il est un problème connu qui empêche les autres nœuds dans votre cluster SGH de basculer vers le nouveau mode d’attestation.
Pour garantir que tous les serveurs SGH appliquez le mode d’attestation correct, exécutez `Set-HgsServer -TrustActiveDirectory` **sur chaque nœud** de votre cluster SGH.
Ce problème ne s’applique pas si vous passez du mode de module de plateforme sécurisée en mode AD *et* le cluster a été initialement configuré en mode d’AD.

Vous pouvez vérifier le mode d’attestation de votre serveur SGH en exécutant [Get-HgsServer](https://technet.microsoft.com/library/mt652162.aspx).

## <a name="memory-dump-encryption-policies"></a>Stratégies de chiffrement de vidage de mémoire

Si vous essayez de configurer des stratégies de chiffrement de vidage de mémoire et que vous ne voyez pas la valeur par défaut SGH vider les stratégies (Sgh\_NoDumps, Sgh\_DumpEncryption et Sgh\_DumpEncryptionKey) ou l’applet de commande de stratégie vidage () Add-HgsAttestationDumpPolicy), il est probable que vous n’avez pas la dernière mise à jour cumulative installée.
Pour y remédier, [mettre à jour votre serveur SGH](guarded-fabric-manage-hgs.md#patching-hgs) à la dernière version Windows mise à jour cumulative et [activer les nouvelles stratégies de l’attestation](guarded-fabric-manage-hgs.md#updates-requiring-policy-activation).
Vérifiez que vous mettez à jour vos ordinateurs hôtes Hyper-V pour la mise à jour cumulative même avant d’activer les nouvelles stratégies de l’attestation, comme ordinateurs hôtes qui n’ont pas de nouvelles fonctionnalités de chiffrement de vidage installées risquent de ne l’attestation une fois que la stratégie SGH est activée.

## <a name="endorsement-key-certificate-error-messages"></a>Messages d’erreur de certificat de clé de Endorsement

Lors de l’inscription d’un hôte en utilisant le [Add-HgsAttestationTpmHost](https://docs.microsoft.com/powershell/module/hgsattestation/add-hgsattestationtpmhost) applet de commande, deux identificateurs de module de plateforme sécurisée sont extraits à partir du fichier d’identificateur de plateforme fourni : le certificat de clé de type EK (EKcert) et la clé publique de type EK (EKpub).
Le EKcert identifie le fabricant du TPM, fournir des garanties que le module de plateforme sécurisée est authentique et fabriqués via la chaîne d’approvisionnement normal.
Identifie de façon unique le EKpub identifie ce module de plateforme sécurisée spécifique et est une des mesures que SGH utilise pour accorder à un ordinateur hôte d’un accès pour exécuter des machines virtuelles protégées.

Vous recevrez une erreur lors de l’inscription d’un hôte de module de plateforme sécurisée si une des deux conditions sont remplie :
1. Le fichier d’identificateur de plateforme **pas** contiennent un certificat de clé de type EK
2. Le fichier d’identificateur de plateforme contient un certificat de clé de type EK, mais ce certificat est **n’est ne pas approuvé** sur votre système

Certains fabricants de TPM n’incluent pas EKcerts dans leurs modules de plateforme sécurisée.
Si vous pensez que c’est le cas avec le module TPM, vérifiez avec votre fabricant OEM que votre module de plateforme sécurisée ne doit pas avoir un EKcert et utiliser le `-Force` indicateur inscrire manuellement l’ordinateur hôte avec SGH.
Si votre module de plateforme sécurisée doit avoir un EKcert, mais il n’en existe pas dans le fichier d’identificateur de plateforme, assurez-vous d’utiliser une console de PowerShell d’administrateur (élevé) lors de l’exécution [Get-PlatformIdentifier](https://docs.microsoft.com/powershell/module/platformidentifier/get-platformidentifier) sur l’ordinateur hôte.

Si vous avez reçu l’erreur que votre EKcert n’est pas approuvé, assurez-vous d’avoir [installé le package de certificats de confiance TPM racine](guarded-fabric-install-trusted-tpm-root-certificates.md) sur chaque serveur SGH et que le certificat racine pour votre fournisseur de module de plateforme sécurisée est présent dans l’ordinateur local  **TrustedTPM\_RootCA** stocker. Tous les certificats intermédiaires applicables doivent également être installés dans le **TrustedTPM\_IntermediateCA** stocker sur l’ordinateur local.
Après avoir installé les certificats racines et intermédiaires, vous devez être en mesure d’exécuter `Add-HgsAttestationTpmHost` avec succès.
