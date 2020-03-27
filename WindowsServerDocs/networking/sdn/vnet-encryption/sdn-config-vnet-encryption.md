---
title: Configurer le chiffrement pour un réseau virtuel
description: Le chiffrement de réseau virtuel permet le chiffrement du trafic réseau virtuel entre les machines virtuelles qui communiquent entre elles au sein des sous-réseaux marqués comme « chiffrement activé ».
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: lizross
author: eross-msft
ms.date: 08/08/2018
ms.openlocfilehash: e68da9be84e9567458467c9ebd89155e7c405c5c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312827"
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>Configurer le chiffrement pour un sous-réseau virtuel

>S’applique à : Windows Server

Le chiffrement de réseau virtuel permet le chiffrement du trafic réseau virtuel entre les machines virtuelles qui communiquent entre elles dans des sous-réseaux marqués comme « chiffrement activé ». Il utilise aussi le protocole DTLS (Datagram Transport Layer Security) sur le sous-réseau virtuel pour chiffrer les paquets. DTLS protège contre les écoutes clandestines, l’altération et la falsification par toute personne ayant accès au réseau physique.

Le chiffrement de réseau virtuel requiert :
- Certificats de chiffrement installés sur chacun des hôtes Hyper-V compatibles SDN.
- Objet d’informations d’identification dans le contrôleur de réseau qui référence l’empreinte numérique de ce certificat.
- La configuration sur chaque réseau virtuel contient des sous-réseaux qui nécessitent un chiffrement.

Une fois que vous activez le chiffrement sur un sous-réseau, tout le trafic réseau au sein de ce sous-réseau est automatiquement chiffré, en plus du chiffrement au niveau de l’application qui peut également avoir lieu.  Le trafic qui traverse des sous-réseaux, même s’il est marqué comme étant chiffré, est envoyé automatiquement non chiffré. Tout trafic qui traverse la limite du réseau virtuel est également envoyé non chiffré.

>[!NOTE]
>Lors de la communication avec une autre machine virtuelle sur le même sous-réseau, qu’elle soit actuellement connectée ou connectée ultérieurement, le trafic est automatiquement chiffré.

>[!TIP]
>Si vous devez limiter les applications pour qu’elles communiquent uniquement sur le sous-réseau chiffré, vous pouvez utiliser des listes de Access Control (ACL) uniquement pour autoriser la communication au sein du sous-réseau actuel. Pour plus d’informations, consultez [utiliser des listes de Access Control (ACL) pour gérer le flux de trafic réseau du centre de](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)données.


## <a name="step-1-create-the-encryption-certificate"></a>Étape 1. Créer le certificat de chiffrement
Un certificat de chiffrement doit être installé sur chaque ordinateur hôte. Vous pouvez utiliser le même certificat pour tous les locataires ou en générer un unique pour chaque locataire. 

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

Une fois le script exécuté, un nouveau certificat s’affiche dans le magasin My :

    PS D:\> dir cert:\\localmachine\my


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

2. Exportez le certificat vers un fichier.<p>Vous avez besoin de deux copies du certificat, l’une avec la clé privée et l’autre sans.

```
   $subjectName = "EncryptedVirtualNetworks"
   $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"}
   [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret"))
   Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert
```

3. Installer les certificats sur chacun de vos ordinateurs hôtes Hyper-v 

   PS C :\> dir c :\$SubjectName. *


~~~
    Directory: C:\


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/22/2017   4:54 PM            543 EncryptedVirtualNetworks.cer
-a----        9/22/2017   4:54 PM           1706 EncryptedVirtualNetworks.pfx
~~~

4. Installation sur un hôte Hyper-V

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

5. Répétez cette opération pour chaque serveur de votre environnement.<p>Après la répétition de pour chaque serveur, vous devez disposer d’un certificat installé dans la racine et le magasin My de chaque hôte Hyper-V. 

6. Vérifiez l’installation du certificat.<p>Vérifiez les certificats en vérifiant le contenu des magasins de certificats My et root :

   PS C :\> Enter-PSSession server1

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

7. Prenez note de l’empreinte numérique.<p>Vous devez prendre note de l’empreinte numérique car vous en avez besoin pour créer l’objet d’informations d’identification de certificat dans le contrôleur de réseau.

## <a name="step-2-create-the-certificate-credential"></a>Étape 2. Créer les informations d’identification du certificat

Après avoir installé le certificat sur chacun des ordinateurs hôtes Hyper-V connectés au contrôleur de réseau, vous devez maintenant configurer le contrôleur de réseau pour l’utiliser.  Pour ce faire, vous devez créer un objet d’informations d’identification contenant l’empreinte numérique du certificat à partir de l’ordinateur sur lequel sont installés les modules PowerShell du contrôleur de réseau. 

```
    # Replace with thumbprint from your certificate
    $thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"  

    # Replace with your Network Controller URI
    $uri = "https://nc.contoso.com"

    Import-module networkcontroller

    $credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
    $credproperties.Type = "X509Certificate"
    $credproperties.Value = $thumbprint
    New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force
```
>[!TIP]
>Vous pouvez réutiliser ces informations d’identification pour chaque réseau virtuel chiffré, ou vous pouvez déployer et utiliser un certificat unique pour chaque locataire.


## <a name="step-3-configuring-a-virtual-network-for-encryption"></a>Étape 3. Configuration d’un réseau virtuel pour le chiffrement

Cette étape suppose que vous avez déjà créé un nom de réseau virtuel « mon réseau » et qu’il contient au moins un sous-réseau virtuel.  Pour plus d’informations sur la création de réseaux virtuels, consultez [créer, supprimer ou mettre à jour des réseaux virtuels locataires](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md).

>[!NOTE]
>Lors de la communication avec une autre machine virtuelle sur le même sous-réseau, qu’elle soit actuellement connectée ou connectée ultérieurement, le trafic est automatiquement chiffré.

1.  Récupérer le réseau virtuel et les objets d’informations d’identification à partir du contrôleur de réseau
```
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork"
    $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"
```
2.  Ajouter une référence aux informations d’identification de certificat et activer le chiffrement sur des sous-réseaux individuels
```
    $vnet.properties.EncryptionCredential = $certcred

    # Replace the Subnets index with the value corresponding to the subnet you want encrypted.  
    # Repeat for each subnet where encryption is needed
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true
```
3.  Placez l’objet réseau virtuel mis à jour dans le contrôleur de réseau.
```
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force
```

_**Félicitations!**_ Une fois ces étapes terminées, vous avez terminé. 


## <a name="next-steps"></a>Étapes suivantes :



