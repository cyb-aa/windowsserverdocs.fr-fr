---
title: Résolution des problèmes du service Guardian hôte
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 424b8090-0692-49a6-9dc4-3c0e77d74b80
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: be817a2c06b13af254b80090b9a7488209d4df0a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403528"
---
# <a name="troubleshooting-the-host-guardian-service"></a>Résolution des problèmes du service Guardian hôte

> S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique décrit les solutions aux problèmes courants rencontrés lors du déploiement ou de l’utilisation d’un serveur de service Guardian hôte (SGH) dans une infrastructure protégée.
Si vous n’êtes pas sûr de la nature de votre problème, essayez d’abord d’exécuter les [Diagnostics de l’infrastructure protégée](guarded-fabric-troubleshoot-diagnostics.md) sur vos serveurs SGH et les hôtes Hyper-V pour réduire les causes potentielles.

## <a name="certificates"></a>Certificats

SGH requiert plusieurs certificats pour fonctionner, y compris le certificat de chiffrement et de signature configuré par l’administrateur, ainsi qu’un certificat d’attestation géré par SGH lui-même.
Si ces certificats ne sont pas correctement configurés, SGH ne pourra pas traiter les demandes des hôtes Hyper-V souhaitant attester ou déverrouiller les protecteurs de clé pour les machines virtuelles protégées.
Les sections suivantes couvrent les problèmes courants liés aux certificats configurés sur SGH.

### <a name="certificate-permissions"></a>Autorisations de certificat

SGH doit être en mesure d’accéder aux clés publiques et privées des certificats de chiffrement et de signature ajoutés à SGH par l’empreinte numérique du certificat.
Plus précisément, le compte de service administré de groupe (gMSA) qui exécute le service SGH a besoin d’accéder aux clés.
Pour trouver le gMSA utilisé par SGH, exécutez la commande suivante dans une invite PowerShell avec élévation de privilèges sur votre serveur SGH :

```powershell
(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName
```

La façon dont vous accordez l’accès au compte gMSA pour utiliser la clé privée dépend de l’emplacement de stockage de la clé : sur l’ordinateur en tant que fichier de certificat local, sur un module de sécurité matériel (HSM) ou à l’aide d’un fournisseur de stockage de clés tiers personnalisé.

#### <a name="grant-access-to-software-backed-private-keys"></a>Accorder l’accès à des clés privées logicielles

Si vous utilisez un certificat auto-signé ou un certificat émis par une autorité de certification qui n’est **pas** stockée dans un module de sécurité matériel ou un fournisseur de stockage de clés personnalisé, vous pouvez modifier les autorisations de la clé privée en effectuant les étapes suivantes :

1. Ouvrir le gestionnaire de certificat local (certlm. msc)
2. Développez **certificats > personnels** et recherchez le certificat de signature ou de chiffrement que vous souhaitez mettre à jour.
3. Cliquez avec le bouton droit sur le certificat et sélectionnez **toutes les tâches > gérer les clés privées**.
4. Cliquez sur **Ajouter** pour accorder à un nouvel utilisateur l’accès à la clé privée de certificat.
5. Dans le sélecteur d’objet, entrez le nom du compte gMSA pour SGH trouvé précédemment, puis cliquez sur **OK**.
6. Assurez-vous que le gMSA a accès **en lecture** au certificat.
7. Cliquez sur **OK** pour fermer la fenêtre d’autorisation.

Si vous exécutez SGH sur Server Core ou que vous gérez le serveur à distance, vous ne serez pas en mesure de gérer les clés privées à l’aide du gestionnaire de certificats local.
Au lieu de cela, vous devez télécharger le [module PowerShell outils d’infrastructure protégée](https://www.powershellgallery.com/packages/GuardedFabricTools) qui vous permettra de gérer les autorisations dans PowerShell.

1. Ouvrez une console PowerShell avec élévation de privilèges sur l’ordinateur Server Core ou utilisez la communication à distance PowerShell avec un compte disposant d’autorisations d’administrateur local sur SGH.
2. Exécutez les commandes suivantes pour installer le module PowerShell outils d’infrastructure protégée et accorder au compte gMSA l’accès à la clé privée.

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

#### <a name="grant-access-to-hsm-or-custom-provider-backed-private-keys"></a>Accorder l’accès à HSM ou à des clés privées protégées par un fournisseur personnalisé

Si les clés privées de votre certificat sont sauvegardées par un module de sécurité matériel (HSM) ou un fournisseur de stockage de clés (KSP) personnalisé, le modèle d’autorisation dépend de votre fournisseur de logiciels spécifique.
Pour obtenir des résultats optimaux, consultez la documentation de votre fournisseur ou le support technique pour plus d’informations sur la façon dont les autorisations de clé privée sont gérées pour votre périphérique/logiciel spécifique.

Certains modules de sécurité matériels ne prennent pas en charge l’octroi d’un accès à une clé privée à des comptes d’utilisateurs spécifiques. au lieu de cela, ils permettent au compte d’ordinateur d’accéder à toutes les clés d’un jeu de clés spécifique.
Pour ces appareils, il est généralement suffisant pour accorder à l’ordinateur l’accès à vos clés et SGH pourra tirer parti de cette connexion.

**Conseils pour les modules HSM**

Vous trouverez ci-dessous les options de configuration suggérées pour vous aider à utiliser avec succès des clés sauvegardées par HSM avec SGH basé sur les expériences de Microsoft et de ses partenaires.
Ces conseils sont fournis à des fins de commodité et ne sont pas nécessairement corrects au moment de la lecture, et ne sont pas approuvés par les fabricants de modules HSM.
Si vous avez d’autres questions, contactez le fabricant de votre module HSM pour obtenir des informations précises concernant votre appareil spécifique.

Gamme/série HSM      | Suggestion
----------------------|-------------
Gemalto SafeNet       | Vérifiez que la propriété utilisation de la clé dans le fichier de demande de certificat est définie sur 0xa0, ce qui permet d’utiliser le certificat pour la signature et le chiffrement. En outre, vous devez accorder au compte gMSA l’accès *en lecture* à la clé privée à l’aide de l’outil Gestionnaire de certificats local (voir les étapes ci-dessus).
nCipher nShield        | Vérifiez que chaque nœud SGH a accès au monde de sécurité contenant les clés de signature et de chiffrement. Vous n’avez pas besoin de configurer des autorisations spécifiques à gMSA.
Utimaco CryptoServers | Vérifiez que la propriété utilisation de la clé dans le fichier de demande de certificat est définie sur 0x13, ce qui permet d’utiliser le certificat pour le chiffrement, le déchiffrement et la signature.

### <a name="certificate-requests"></a>Demandes de certificat

Si vous utilisez une autorité de certification pour émettre vos certificats dans un environnement d’infrastructure à clé publique (PKI), vous devez vous assurer que votre demande de certificat comprend la configuration minimale requise pour l’utilisation de ces clés par SGH.

**Certificats de signature**

CSR, propriété | Valeur requise
-------------|---------------
Algorithm    | RSA
Taille de la clé     | Au moins 2048 bits
Utilisation de la clé    | Signature/Sign/DigitalSignature

**Certificats de chiffrement**

CSR, propriété | Valeur requise
-------------|---------------
Algorithm    | RSA
Taille de la clé     | Au moins 2048 bits
Utilisation de la clé    | Chiffrement/chiffrement/DataEncipherment

**Active Directory les modèles des services de certificats**

Si vous utilisez des modèles de certificat de services de certificats Active Directory (ADCS) pour créer les certificats, il est recommandé d’utiliser un modèle avec les paramètres suivants :

Propriété du modèle ADCS | Valeur requise
-----------------------|---------------
Catégorie de fournisseur      | Key Storage Provider
Nom de l’algorithme         | RSA
Taille de clé minimale       | 2 048
Objectif                | Signature et chiffrement
Extension d’utilisation de la clé    | Signature numérique, chiffrement de clé, chiffrement des données (« autoriser le chiffrement des données utilisateur »)


### <a name="time-drift"></a>Dérive temporelle

Si l’heure de votre serveur a été considérablement dépensée de celle des autres nœuds SGH ou des ordinateurs hôtes Hyper-V dans votre infrastructure protégée, vous risquez de rencontrer des problèmes avec la validité du certificat du signataire d’attestation.
Le certificat du signataire d’attestation est créé et renouvelé en arrière-plan sur SGH et est utilisé pour signer les certificats d’intégrité émis aux hôtes protégés par le service d’attestation.

Pour actualiser le certificat de signataire d’attestation, exécutez la commande suivante dans une invite PowerShell avec élévation de privilèges.

```powershell
Start-ScheduledTask -TaskPath \Microsoft\Windows\HGSServer -TaskName 
AttestationSignerCertRenewalTask
```

Vous pouvez également exécuter manuellement la tâche planifiée en ouvrant **Planificateur de tâches** (taskschd. msc), en accédant à **Planificateur de tâches bibliothèque > Microsoft > Windows > HGSServer** et en exécutant la tâche nommée  **AttestationSignerCertRenewalTask**.

## <a name="switching-attestation-modes"></a>Basculement des modes d’attestation

Si vous basculez SGH du mode TPM vers le mode Active Directory, ou vice versa à l’aide de l’applet de commande [Set-HgsServer](https://technet.microsoft.com/library/mt652180.aspx) , le démarrage de l’application du nouveau mode d’attestation peut prendre jusqu’à 10 minutes.
Il s’agit d’un comportement normal.
Il est recommandé de ne pas supprimer les stratégies autorisant les ordinateurs hôtes du mode d’attestation précédent tant que vous n’avez pas vérifié que tous les ordinateurs hôtes attestent correctement à l’aide du nouveau mode d’attestation.

**Problème connu lors du passage du mode TPM au mode AD**

Si vous avez initialisé votre cluster SGH en mode TPM et que vous passez ensuite en mode Active Directory, il existe un problème connu qui empêchera les autres nœuds de votre cluster SGH de basculer vers le nouveau mode d’attestation.
Pour vous assurer que tous les serveurs SGH appliquent le mode d’attestation correct, exécutez `Set-HgsServer -TrustActiveDirectory` **sur chaque nœud** de votre cluster SGH.
Ce problème ne s’applique pas si vous passez du mode TPM au mode AD *et* que le cluster a été initialement configuré en mode Active Directory.

Vous pouvez vérifier le mode d’attestation de votre serveur SGH en exécutant l’aide [de la récupération](https://technet.microsoft.com/library/mt652162.aspx)de l’ordinateur.

## <a name="memory-dump-encryption-policies"></a>Stratégies de chiffrement de l’image mémoire

Si vous essayez de configurer des stratégies de chiffrement de l’image mémoire et que vous ne voyez pas les stratégies de vidage SGH par défaut (SGH @ no__t-0NoDumps, SGH @ no__t-1DumpEncryption et SGH @ no__t-2DumpEncryptionKey) ou l’applet de commande de stratégie de vidage (Add-HgsAttestationDumpPolicy), il s’agit de vous n’avez probablement pas installé la dernière mise à jour cumulative.
Pour résoudre ce problème, [Mettez à jour votre serveur SGH](guarded-fabric-manage-hgs.md#patching-hgs) vers la dernière version cumulative de Windows Update et [activez les nouvelles stratégies d’attestation](guarded-fabric-manage-hgs.md#updates-requiring-policy-activation).
Veillez à mettre à jour vos ordinateurs hôtes Hyper-V vers la même mise à jour cumulative avant d’activer les nouvelles stratégies d’attestation, car les ordinateurs hôtes qui n’ont pas les nouvelles fonctionnalités de chiffrement de vidage installés risquent d’échouer à l’attestation une fois la stratégie SGH activée.

## <a name="endorsement-key-certificate-error-messages"></a>Message d’erreur de certificat de clé de type EK

Lors de l’inscription d’un ordinateur hôte à l’aide de l’applet de commande [Add-HgsAttestationTpmHost](https://docs.microsoft.com/powershell/module/hgsattestation/add-hgsattestationtpmhost) , deux identificateurs TPM sont extraits du fichier d’identificateur de plateforme fourni : le certificat de clé de type EK (EKcert) et la clé de type EK public (EKpub).
Le EKcert identifie le fabricant du module de plateforme sécurisée, ce qui garantit que le module de plateforme sécurisée est authentique et fabriqué via la chaîne d’approvisionnement normale.
Le EKpub identifie de façon unique ce module de plateforme sécurisée spécifique et est l’une des mesures que le SGH utilise pour accorder à un hôte l’accès pour exécuter des machines virtuelles dotées d’une protection maximale.

Vous recevrez une erreur lors de l’inscription d’un hôte TPM si l’une des deux conditions suivantes est vraie :
1. Le fichier d’identificateur de plateforme **ne contient pas** de certificat de clé de type EK
2. Le fichier d’identificateur de plateforme contient un certificat de clé de type EK, mais ce certificat n’est **pas approuvé** sur votre système

Certains fabricants de module de plateforme sécurisée n’incluent pas EKcerts dans leur plateforme sécurisée.
Si vous pensez que c’est le cas avec votre TPM, vérifiez auprès de votre fabricant d’ordinateurs OEM que votre plateforme sécurisée ne doit pas avoir de EKcert et utilisez l’indicateur `-Force` pour inscrire manuellement l’ordinateur hôte auprès de SGH.
Si votre module de plateforme sécurisée doit avoir un EKcert mais qu’aucun n’a été trouvé dans le fichier d’identificateur de plateforme, vérifiez que vous utilisez une console PowerShell administrateur (avec élévation de privilèges) lors de l’exécution de la fonction [obtenir-PlatformIdentifier](https://docs.microsoft.com/powershell/module/platformidentifier/get-platformidentifier) sur l’ordinateur hôte.

Si vous avez reçu l’erreur indiquant que votre EKcert n’est pas approuvé, vérifiez que vous avez [installé le package de certificats racines TPM approuvés](guarded-fabric-install-trusted-tpm-root-certificates.md) sur chaque serveur SGH et que le certificat racine de votre fournisseur de module de plateforme sécurisée est présent dans le **TrustedTPM @ no__ de l’ordinateur local stockage t-2RootCA** . Tous les certificats intermédiaires applicables doivent également être installés dans le magasin **TrustedTPM @ no__t-1IntermediateCA** sur l’ordinateur local.
Après avoir installé les certificats racine et intermédiaires, vous devez être en mesure d’exécuter `Add-HgsAttestationTpmHost` avec succès.
