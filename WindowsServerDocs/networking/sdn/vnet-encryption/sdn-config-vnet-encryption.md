---
title: Configurer le chiffrement pour un réseau virtuel
description: Chiffrement de réseau virtuel, le chiffrement du trafic de réseau virtuel entre des machines virtuelles qui communiquent entre eux au sein de sous-réseaux marqués comme « Chiffrement Enabled ».
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: d2c09c83a227c5a75ff5b1b39b2ef6d1286bbfc8
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501561"
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>Configurer le chiffrement pour un sous-réseau virtuel

>S’applique à : Windows Server

Permet de cryptage de réseau virtuel pour le chiffrement du trafic de réseau virtuel entre les machines virtuelles qui communiquent entre eux au sein de sous-réseaux marqués comme « Chiffrement Enabled ». Il utilise aussi le protocole DTLS (Datagram Transport Layer Security) sur le sous-réseau virtuel pour chiffrer les paquets. DTLS protège contre les écoutes clandestines, l'altération et la falsification par toute personne ayant accès au réseau physique.

Le chiffrement du réseau virtuel requiert :
- Certificats de chiffrement installés sur chacun des hôtes Hyper-V activé de SDN.
- Un objet d’informations d’identification dans le contrôleur de réseau faisant référence à l’empreinte numérique du certificat.
- Configuration sur chacun des réseaux virtuels contiennent des sous-réseaux qui exigent un chiffrement.

Une fois que vous activez le chiffrement sur un sous-réseau, tout le trafic réseau au sein de ce sous-réseau est chiffré automatiquement, en plus d’un chiffrement au niveau de l’application peut également avoir lieu.  Le trafic entre sous-réseaux, même si marquée sous forme chiffrée, est automatiquement envoyé sans être chiffré. Tout le trafic qui traverse la limite de réseau virtuel obtient envoyé sans être chiffré.

>[!NOTE]
>Lors de la communication avec une autre machine virtuelle sur le même sous-réseau, si son actuellement connecté ou connecté à une date ultérieure, le trafic chiffré obtient automatiquement.

>[!TIP]
>Si vous devez limiter les applications de communiquer uniquement sur le sous-réseau chiffré, vous pouvez utiliser la listes de contrôle d’accès (ACL) uniquement pour permettre la communication au sein du sous-réseau en cours. Pour plus d’informations, consultez [utiliser Access Control Lists (ACL) pour le flux du trafic réseau centre de données gérer](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).


## <a name="step-1-create-the-encryption-certificate"></a>Étape 1. Créer le certificat de chiffrement
Chaque hôte doit avoir installé un certificat de chiffrement. Vous pouvez utiliser le même certificat pour tous les locataires ou générer un unique pour chaque client. 

1.  Générer le certificat  

```
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
```

Après avoir exécuté le script, un nouveau certificat s’affiche dans le magasin My :

    PS D:\> dir cert:\\localmachine\my


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

2. Exporter le certificat vers un fichier.<p>Vous avez besoin de deux copies du certificat, l’autre avec la clé privée et l’autre sans.

```
   $subjectName = "EncryptedVirtualNetworks"
   $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"}
   [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret"))
   Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert
```

3. Installez les certificats sur chacun de vos hôtes hyper-v 

   PS c :\> dir c:\$subjectname.*


~~~
    Directory: C:\


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/22/2017   4:54 PM            543 EncryptedVirtualNetworks.cer
-a----        9/22/2017   4:54 PM           1706 EncryptedVirtualNetworks.pfx
~~~

4. L’installation sur un ordinateur hôte Hyper-V

```
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
       $certificate.import($certFullPath)

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

       # Important: Remove the certificate files when finished
       remove-item C:\$SubjectName.cer
       remove-item C:\$SubjectName.pfx
   }
```

5. Répétez pour chaque serveur dans votre environnement.<p>Après avoir répété pour chaque serveur, vous devez avoir un certificat installé dans la racine et le magasin my de chaque hôte Hyper-V. 

6. Vérifiez l’installation du certificat.<p>Vérifier les certificats en vérifiant le contenu de la mon et racine des magasins de certificats :

   PS C :\> Entrez-pssession Server1

~~~
[Server1]: PS C:\> get-childitem cert://localmachine/my,cert://localmachine/root | ? {$_.Subject -eq "CN=EncryptedVirtualNetworks"}

PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

Thumbprint                                Subject
----------                                -------
5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks


PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\root

Thumbprint                                Subject
----------                                -------
5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks
~~~

7. Prenez note de l’empreinte numérique.<p>Vous devez prenez note de l’empreinte numérique, car vous en avez besoin pour créer l’objet d’informations d’identification de certificat dans le contrôleur de réseau.

## <a name="step-2-create-the-certificate-credential"></a>Étape 2. Créer les informations d’identification de certificat

Après avoir installé le certificat sur chacun des hôtes Hyper-V connectés au contrôleur de réseau, vous devez maintenant configurer le contrôleur de réseau pour l’utiliser.  Pour ce faire, vous devez créer un objet d’informations d’identification contenant l’empreinte de certificat à partir de l’ordinateur avec les modules PowerShell de contrôleur de réseau installés. 


    # Replace with thumbprint from your certificate
    $thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"  

    # Replace with your Network Controller URI
    $uri = "https://nc.contoso.com"

    Import-module networkcontroller

    $credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
    $credproperties.Type = "X509Certificate"
    $credproperties.Value = $thumbprint
    New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force

>[!TIP]
>Vous pouvez réutiliser ces informations d’identification pour chaque réseau virtuel chiffrée, ou vous pouvez déployer et utiliser un certificat unique pour chaque client.


## <a name="step-3-configuring-a-virtual-network-for-encryption"></a>Étape 3. Configuration d’un réseau virtuel pour le chiffrement

Cette étape suppose que vous avez déjà créé un nom de réseau virtuel « Mon réseau » et qu’il contient au moins un sous-réseau virtuel.  Pour plus d’informations sur la création de réseaux virtuels, consultez [Create, Delete ou réseaux virtuels des clients de mise à jour](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md).

>[!NOTE]
>Lors de la communication avec une autre machine virtuelle sur le même sous-réseau, si son actuellement connecté ou connecté à une date ultérieure, le trafic chiffré obtient automatiquement.

1.  Récupérer les objets de réseau virtuel et les informations d’identification à partir du contrôleur de réseau

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork" $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"

2.  Ajoutez une référence à l’information d’identification de certificat et d’activer le chiffrement sur des sous-réseaux individuels

    $vnet.properties.EncryptionCredential = $certcred

    # <a name="replace-the-subnets-index-with-the-value-corresponding-to-the-subnet-you-want-encrypted"></a>Remplacez l’index de sous-réseaux avec la valeur correspondant au sous-réseau que vous souhaitez chiffré.  
    # <a name="repeat-for-each-subnet-where-encryption-is-needed"></a>Répétez pour chaque sous-réseau dans lequel le chiffrement est nécessaire
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true

3.  Placer l’objet de réseau virtuel mis à jour dans le contrôleur de réseau

    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force


_**Félicitations !** _ Vous avez terminé une fois que vous effectuez ces étapes. 


## <a name="next-steps"></a>Étapes suivantes



