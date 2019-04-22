---
title: Obtenir et configurer des certificats de déchiffrement de jetons et de signature de jetons pour AD FS
description: Ce document décrit comment obtenir et configurer le TS et TD certificats pour AD FS
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d16a886c9fb8f88748ffe732a75f0fd6a32c3702
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820210"
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>Obtenir et configurer TS et les certificats TD pour AD FS

Cette rubrique décrit les tâches et les procédures que vous pouvez effectuer pour vous assurer que vos certificats de déchiffrement de jetons et de signature de jetons AD FS sont à jour.

Certificats de signature de jeton sont X509 standard des certificats qui sont utilisés pour connecter de façon sécurisée tous les jetons émis par le serveur de fédération. Certificats de déchiffrement de jetons sont standard X509 certificats qui sont utilisés pour déchiffrer les jetons entrants. Ils sont également publiés dans les métadonnées de fédération.

Pour plus d’informations, consultez [configuration requise des certificats](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>Déterminer si AD FS renouvelle les certificats automatiquement
Par défaut, AD FS est configuré pour générer des certificats de déchiffrement de jetons et de signature de jetons automatiquement, au moment de la configuration initiale et lorsque les certificats approchent de leur date d’expiration.

Vous pouvez exécuter la commande Windows PowerShell suivante : `Get-AdfsProperties`.
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
La propriété AutoCertificateRollover décrit si AD FS est configuré pour renouveler la signature de jetons et de déchiffrement automatiquement les certificats de jetons.

Si AutoCertificateRollover est définie sur TRUE, les certificats AD FS seront renouvelés et configurés automatiquement dans AD FS. Une fois que le nouveau certificat est configuré, afin d’éviter une panne, vous devez vous assurer que chaque partenaire de fédération (représenté par des approbations de partie de confiance ou approbations de fournisseur de revendications dans votre batterie de serveurs AD FS) est mis à jour avec ce nouveau certificat.
    
Si AD FS n’est pas configuré pour renouveler la signature de jetons et jeton déchiffrement certificats automatiquement (si AutoCertificateRollover est définie sur False), AD FS ne seront pas automatiquement générer ou commencer à utiliser la nouvelle signature de jetons ou certificats de déchiffrement de jetons. Vous devrez effectuer ces tâches manuellement.
    
Si AD FS est configuré pour renouveler la signature de jetons et de déchiffrement automatiquement les certificats de jetons (AutoCertificateRollover est définie sur TRUE), vous pouvez déterminer quand ils seront renouvelés :

CertificateGenerationThreshold décrit le nombre de jours avant la date du certificat pas après un nouveau certificat sera généré.

CertificatePromotionThreshold détermine le nombre de jours après le nouveau certificat est généré qu’il sera promue pour l’utiliser comme certificat principal (en d’autres termes, AD FS démarrera l’utiliser pour signer les jetons qu’il émet et déchiffrer les jetons provenant de fournisseurs d’identité).

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
Si AD FS est configuré pour renouveler la signature de jetons et de déchiffrement automatiquement les certificats de jetons (AutoCertificateRollover est définie sur TRUE), vous pouvez déterminer quand ils seront renouvelés :

 - **CertificateGenerationThreshold** décrit le nombre de jours avant la date du certificat pas après un nouveau certificat sera généré.
 - **CertificatePromotionThreshold** détermine le nombre de jours après le nouveau certificat est généré qu’il sera promue pour l’utiliser comme certificat principal (en d’autres termes, AD FS démarrera l’utiliser pour signer les jetons qu’il émet et déchiffrer les jetons d’identité fournisseurs).

## <a name="determine-when-the-current-certificates-expire"></a>Déterminer la date d’expiration des certificats en cours
Vous pouvez utiliser la procédure suivante pour identifier le principal de signature de jeton et les certificats de déchiffrement de jetons et pour déterminer la date d’expiration des certificats en cours.

Vous pouvez exécuter la commande Windows PowerShell suivante : `Get-AdfsCertificate –CertificateType token-signing` (ou `Get-AdfsCertificate –CertificateType token-decrypting `). Ou vous pouvez examiner les certificats en cours dans la console MMC : Service -> certificats.

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

Le certificat pour lequel le **IsPrimary** a la valeur **True** est le certificat AD FS utilise actuellement.

La date indiquée pour le **pas après** correspond à la date à laquelle un jeton principal nouvelle signature ou le certificat de déchiffrement doit être configuré.

Pour garantir la continuité de service, tous les partenaires de fédération (représentés par des approbations de partie de confiance ou approbations de fournisseur de revendications dans votre batterie de serveurs AD FS) doivent utiliser la nouvelle signature de jetons et les certificats de déchiffrement de jetons avant cette expiration. Nous vous recommandons de commencer ce processus de planification au moins 60 jours à l’avance.

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>Génération d’un nouveau certificat auto-signé manuellement avant la fin de la période de grâce
Vous pouvez utiliser les étapes suivantes pour générer un nouveau certificat auto-signé manuellement avant la fin de la période de grâce.

1. Assurez-vous que vous êtes connecté au serveur AD FS principal.
2. Ouvrez Windows PowerShell et exécutez la commande suivante : `Add-PSSnapin "microsoft.adfs.powershell"`
3. Si vous le souhaitez, vous pouvez vérifier les certificats de signatures actuels dans AD FS. Pour ce faire, exécutez la commande suivante : `Get-ADFSCertificate –CertificateType token-signing`. Examinez la sortie de commande pour afficher les dates non après des certificats répertoriés.
4. Pour générer un nouveau certificat, exécutez la commande suivante pour renouveler et mettre à jour les certificats sur le serveur AD FS : `Update-ADFSCertificate –CertificateType token-signing`.
5. Vérifiez la mise à jour en réexécutant la commande suivante : `Get-ADFSCertificate –CertificateType token-signing`
6. Deux certificats doivent apparaître maintenant, un d'entre eux possède un **pas après** date environ un an et pour lesquels le **IsPrimary** valeur est **False**.

>[!IMPORTANT]
>Pour éviter une interruption de service, vous devez mettre à jour les informations de certificat sur Azure AD en exécutant les étapes dans la procédure de mise à jour Azure AD avec un certificat de signature de jetons valide.

## <a name="if-youre-not-using-self-signed-certificates"></a>Si vous n’utilisez pas des certificats auto-signés...
Si vous êtes ne pas à l’aide de la valeur par défaut généré automatiquement, de signature de jetons auto-signé et les certificats de déchiffrement de jeton, vous devez renouveler et configurer ces certificats manuellement.

Tout d’abord, vous devez obtenir un nouveau certificat à partir de votre autorité de certification et importez-le dans le magasin de certificats personnel d’ordinateur local sur chaque serveur de fédération. Pour obtenir des instructions, consultez le [importer un certificat](https://technet.microsoft.com/library/cc754489.aspx) article.

Vous devez configurer ce certificat en tant que le secondaire AD FS signature de jetons ou un certificat de déchiffrement. (Vous configurer comme un certificat secondaire pour permettre à vos partenaires de fédération suffisamment de temps pour utiliser ce nouveau certificat avant de le passer au certificat principal).

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>Pour configurer un nouveau certificat comme certificat secondaire
1. Ouvrez PowerShell et exécutez la commande suivante : `Set-ADFSProperties -AutoCertificateRollover $false`
2. Une fois que vous avez importé le certificat. Ouvrez le **gestion AD FS** console.
3. Développez **Service** , puis sélectionnez **certificats**.
4. Dans le volet Actions, cliquez sur **Add Token-Signing Certificate**.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. Sélectionnez le nouveau certificat dans la liste des certificats affichés, puis cliquez sur OK.
6.  Ouvrez PowerShell et exécutez la commande suivante : `Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>Assurez-vous que le nouveau certificat a une clé privée associée et que le compte de service AD FS bénéficie d’autorisations de lecture sur la clé privée. Vérifiez cela sur chaque serveur de fédération. Pour ce faire, dans le composant logiciel enfichable Certificats, cliquez sur le nouveau certificat, cliquez sur toutes les tâches, puis cliquez sur Gérer les clés privées.

Une fois que vous avez prévu assez de temps pour vos partenaires de fédération consommer votre nouveau certificat (ils extraient les métadonnées de fédération ou les envoyer la clé publique de votre nouveau certificat), vous devez promouvoir le certificat secondaire pour le certificat principal.

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>Pour promouvoir le nouveau certificat secondaire à principal

1. Ouvrez le **gestion AD FS** console.
2. Développez **Service** , puis sélectionnez **certificats**.
3. Cliquez sur le certificat de signature de jetons secondaire.
4. Dans le **Actions** volet, cliquez sur **définir comme principal**. À l’invite de confirmation, cliquez sur Oui.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>La mise à jour des partenaires de fédération

### <a name="partners-who-can-consume-federation-metadata"></a>Partenaires qui peuvent consommer des métadonnées de fédération
Si vous avez renouvelé et que vous configurez une nouvelle signature de jetons ou un certificat de déchiffrement de jeton, vous devez vous assurer que tous vos partenaires de fédération (organisation ou le compte d’organisation partenaires de ressource qui sont représentées dans AD FS en vous appuyant tiers des approbations et les nouveaux certificats ont récupéré les approbations de fournisseur de revendications).

### <a name="partners-who-can-not-consume-federation-metadata"></a>Partenaires qui ne peuvent pas consommer les métadonnées de fédération
Si vos partenaires de fédération ne peut pas consommer vos métadonnées de fédération, vous devez manuellement leur envoyer la clé publique de votre nouveau certificat de signature de jetons / déchiffrement de jetons. Envoyer votre nouvelle clé publique certificat (fichier .cer ou .p7b si vous souhaitez inclure la chaîne entière) à l’ensemble de votre organisation de ressource ou partenaires d’organisation (représentés par des approbations de partie de confiance et approbations de fournisseur de revendications dans AD FS) de compte. Avoir les partenaires à implémenter les modifications sur son côté pour approuver les nouveaux certificats.

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>Promouvoir au rôle principal (si AutoCertificateRollover est False)
Si **AutoCertificateRollover** a la valeur **False**, AD FS ne génère pas automatiquement, ou commencer à utiliser de nouveau de signature de jetons ou certificats de déchiffrement de jeton. Vous devrez effectuer ces tâches manuellement.
Après avoir laissé un délai suffisant pour l’ensemble de vos partenaires de fédération pour consommer le nouveau certificat secondaire, promouvoir ce certificat secondaire vers le site principal (dans le composant logiciel enfichable MMC, cliquez sur la signature de jetons secondaire de certificat dans le volet Actions, cliquez sur **Définir comme principale**.)

## <a name="updating-azure-ad"></a>La mise à jour Azure AD
AD FS fournit l’accès l’authentification unique aux services de cloud de Microsoft comme Office 365 en authentifiant les utilisateurs via leurs informations d’identification AD DS existantes.  Pour plus d’informations sur l’utilisation de certificats, consultez [renouveler les certificats de fédération pour Office 365 et Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).