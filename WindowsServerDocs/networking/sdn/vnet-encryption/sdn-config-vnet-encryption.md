---
title: Configurer le chiffrement pour un réseau virtuel
description: Cette rubrique fournit des informations sur le chiffrement de réseau virtuel pour le logiciel défini de mise en réseau dans Windows Server
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: pashort
author: grcusanz
ms.openlocfilehash: 7d1de535e7758793e5ddaa7eeefada0aa55d3328
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>Configurer le chiffrement pour un sous-réseau virtuel

>S’applique à: Windows Server

Cette rubrique contient les sections suivantes qui décrivent les étapes requises pour activer le chiffrement sur un réseau virtuel.

- [Création d’un certificat de chiffrement](#bkmk_Certificate)
- [Créer les informations d’identification du certificat](#bkmk_credential)
- [Configuration d’un réseau virtuel pour le chiffrement](#bkmk_vnet)

## <a name="bkmk_Certificate"></a>Création d’un certificat de chiffrement
Un certificat de chiffrement est requis pour être installé sur chaque ordinateur hôte sur lequel le chiffrement est utilisé.  Vous pouvez utiliser le même certificat pour tous les clients ou générer un certificat unique par locataire si nécessaire.

Étape1: Générer le certificat

    $subjectName = "EncryptedVirtualNetworks"
    $cryptographicProviderName = "Microsoft Base Cryptographic Provider v1.0";
    [int] $privateKeyLength = 1024;
    $sslServerOidString = "1.3.6.1.5.5.7.3.1";
    $sslClientOidString = "1.3.6.1.5.5.7.3.2";
    [int] $validityPeriodInYear = 5;

    $name = new-object -com "X509Enrollment.CX500DistinguishedName.1"
    $name.Encode("CN=" + $SubjectName, 0)

    #Generate Key
    $key = new-object -com "X509Enrollment.CX509PrivateKey.1"
    $key.ProviderName = $cryptographicProviderName
    $key.KeySpec = 1 #X509KeySpec.XCN_AT_KEYEXCHANGE
    $key.Length = $privateKeyLength
    $key.MachineContext = 1
    $key.ExportPolicy = 0x2 #X509PrivateKeyExportFlags.XCN_NCRYPT_ALLOW_EXPORT_FLAG 
    $key.Create()

    #Configure Eku
    $serverauthoid = new-object -com "X509Enrollment.CObjectId.1"
    $serverauthoid.InitializeFromValue($sslServerOidString)
    $clientauthoid = new-object -com "X509Enrollment.CObjectId.1"
    $clientauthoid.InitializeFromValue($sslClientOidString)
    $ekuoids = new-object -com "X509Enrollment.CObjectIds.1"
    $ekuoids.add($serverauthoid)
    $ekuoids.add($clientauthoid)
    $ekuext = new-object -com "X509Enrollment.CX509ExtensionEnhancedKeyUsage.1"
    $ekuext.InitializeEncode($ekuoids)

    # Set the hash algorithm to sha512 instead of the default sha1
    $hashAlgorithmObject = New-Object -ComObject X509Enrollment.CObjectId
    $hashAlgorithmObject.InitializeFromAlgorithmName( $ObjectIdGroupId.XCN_CRYPT_HASH_ALG_OID_GROUP_ID, $ObjectIdPublicKeyFlags.XCN_CRYPT_OID_INFO_PUBKEY_ANY, $AlgorithmFlags.AlgorithmFlagsNone, "SHA512")


    #Request Certificate
    $cert = new-object -com "X509Enrollment.CX509CertificateRequestCertificate.1"

    $cert.InitializeFromPrivateKey(2, $key, "")
    $cert.Subject = $name
    $cert.Issuer = $cert.Subject
    $cert.NotBefore = (get-date).ToUniversalTime()
    $cert.NotAfter = $cert.NotBefore.AddYears($validityPeriodInYear);
    $cert.X509Extensions.Add($ekuext)
    $cert.HashAlgorithm = $hashAlgorithmObject
    $cert.Encode()

    $enrollment = new-object -com "X509Enrollment.CX509Enrollment.1"
    $enrollment.InitializeFromRequest($cert)
    $certdata = $enrollment.CreateRequest(0)
    $enrollment.InstallResponse(2, $certdata, 0, "")

Après avoir exécuté la commande ci-dessus, vous verrez un nouveau certificat dans le magasin My de l’ordinateur sur lequel vous avez exécuté le script:

    PS D:\> dir cert:\\localmachine\my


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

Étape2: Exporter le certificat vers un fichier que vous aurez besoin de deux copies du certificat, l’autre avec la clé privée et l’autre sans.

    $subjectName = "EncryptedVirtualNetworks"
    $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"}
    [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret"))
    Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert

Après avoir exécuté la commande ci-dessus, vous avez maintenant deux fichiers de certificat.  Ils doivent être installés sur chacun de vos ordinateurs hôtes hyper-v.

    PS C:\> dir c:\$subjectname.*


        Directory: C:\


    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    -a----        9/22/2017   4:54 PM            543 EncryptedVirtualNetworks.cer
    -a----        9/22/2017   4:54 PM           1706 EncryptedVirtualNetworks.pfx

Étape3: Installer sur un ordinateur hôte Hyper-V

    $server = "Server01"

    $subjectname = "EncryptedVirtualNetworks"
    copy c:\$SubjectName.* \\$server\c$
    invoke-command -computername $server -ArgumentList $subjectname,"secret" {
        param (
            [string] $SubjectName,
            [string] $Secret
        )
        $certFullPath = "c:\$SubjectName.cer"

        # create a representation of the certificate file
        $certificate = new-object System.Security.Cryptography.X509Certificates.X509Certificate2
        $certificate.import($certFullPath){$_}

        # import into the store
        $store = new-object System.Security.Cryptography.X509Certificates.X509Store("Root", "LocalMachine")
        $store.open("MaxAllowed")
        $store.add($certificate)
        $store.close()

        $certFullPath = "c:\$SubjectName.pfx"
        $certificate = new-object System.Security.Cryptography.X509Certificates.X509Certificate2
        $certificate.import($certFullPath, $Secret, "MachineKeySet,PersistKeySet")

        # import into the store
        $store = new-object System.Security.Cryptography.X509Certificates.X509Store("My", "LocalMachine")
        $store.open("MaxAllowed")
        $store.add($certificate)
        $store.close()

        # Important: Remove the certficate files when finished
        remove-item C:\$SubjectName.cer
        remove-item C:\$SubjectName.pfx
    }    

Répétez pour chaque serveur dans votre environnement.  Vous devez maintenant avoir un certificat installé dans la racine et de mon magasin de chaque ordinateur hôte Hyper-V

Vous pouvez vérifier l’installation du certificat en vérifiant le contenu de la mon et racine des magasins de certificats:

    PS C:\> enter-pssession Server1

    [Server1]: PS C:\> get-childitem cert://localmachine/my,cert://localmachine/root | ? {$_.Subject -eq "CN=EncryptedVirtualNetworks"}

    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\root

    Thumbprint                                Subject
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

Prenez note de l’empreinte numérique que vous en aurez besoin pour la création de l’objet d’informations d’identification de certificat dans le contrôleur de réseau.

## <a name="bkmk_Certificate"></a>Créer les informations d’identification du certificat

Une fois le certificat est correctement installé sur chacun des ordinateurs hôtes Hyper-V connectés au contrôleur de réseau, vous pouvez configurer le contrôleur de réseau pour pouvoir l’utiliser.

Vous devez créer un objet d’informations d’identification qui contient l’empreinte numérique du certificat.  Vous devez le faire à partir d’un ordinateur où vous avez les modules powershell de contrôleur de réseau installés.

    # Replace with thumbprint from your certificate
    $thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"  
    
    # Replace with your Network Controller URI
    $uri = "https://nc.contoso.com"

    Import-module networkcontroller
    
    $credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
    $credproperties.Type = "X509Certificate"
    $credproperties.Value = $thumbprint
    New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force

Vous pouvez réutiliser ces informations d’identification pour chaque portant virtuel chiffré, ou vous pouvez déployer et utiliser un certificat unique pour chaque client.

## <a name="bkmk_Certificate"></a>Configuration d’un réseau virtuel pour le chiffrement

Cette étape suppose que vous avez déjà créé un nom de réseau virtuel «Mon réseau» et qu’il contient au moins un sous-réseau virtuel.  Pour plus d’informations sur la création de réseaux virtuels, voir [créer, supprimer ou mise à jour des réseaux virtuels locataires](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md).

Étape1: Récupérer les objets réseau virtuel et les informations d’identification à partir du contrôleur de réseau

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork"
    $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"

Étape2: Ajouter une référence pour les informations d’identification du certificat et activer le chiffrement sur différents sous-réseaux.

    $vnet.properties.EncryptionCredential = $certcred

    # Replace the Subnets index with the value corresponding to the subnet you want encrypted.  
    # Repeat for each subnet where encryption is needed
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true

Étape3: Placer l’objet réseau virtuel mis à jour dans le contrôleur de réseau

    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force

Une fois cette opération terminée, aucune étape supplémentaire est requise.  N’importe quel ordinateur virtuel qui est actuellement connecté et n’importe quel ordinateur virtuel qui est ensuite connecté au sous-réseau ci-dessus aura son trafic chiffré automatiquement lors de la communication avec un autre ordinateur virtuel sur le même sous-réseau.


