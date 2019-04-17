---
title: "Obtenir et configurer des certificats de déchiffrement de jeton et de signature de jetons pour ADFS"
description: "Ce document décrit comment obtenir et configurer le TS et TD certificats pour ADFS"
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0d7f8ba4efb74a377f9f9d748949a934398ebefc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>Obtenir et configurer TS et certificats TD pour ADFS

Cette rubrique décrit les tâches et procédures que vous pouvez effectuer pour vous assurer que vos certificats de déchiffrement de jeton et de signature de jetons ADFS sont à jour.

Jeton de certificats de signature sont X509 standard qui sont utilisées pour signer en toute sécurité tous les jetons que le serveur de fédération émet des certificats. Certificats de déchiffrement de jeton sont standard X509certificats qui sont utilisés pour déchiffrer les jetons entrants. Ils sont également publiés dans les métadonnées de fédération.

Pour plus d’informations, voir [configuration requise des certificats](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>Déterminer si ADFS renouvelle automatiquement les certificats
Par défaut, ADFS est configuré pour générer les certificats de déchiffrement de jeton et de signature de jetons automatiquement, lors de la configuration initiale et quand les certificats sont approche de leur date d’expiration.

Vous pouvez exécuter la commande Windows PowerShell suivante:`Get-AdfsProperties`.
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
La propriété AutoCertificateRollover décrit si ADFS est configuré pour renouveler le jeton de déchiffrement automatiquement des certificats et de signature de jetons.

Si AutoCertificateRollover est définie sur TRUE, les certificats ADFS seront être renouvelés et la configuration automatique dans ADFS. Une fois le nouveau certificat est configuré, afin d’éviter une panne, vous devez vous assurer que chaque partenaire de fédération (représenté par des approbations de partie de confiance ou approbations de fournisseur de revendications dans votre batterie ADFS) est mis à jour avec ce nouveau certificat.
    
Si ADFS n’est pas configuré pour renouveler la signature de jetons et jeton déchiffrement certificats automatiquement (si AutoCertificateRollover est défini sur False), ADFS ne seront pas automatiquement générer ou démarrer à l’aide de la nouvelle signature de jetons ou de certificats de déchiffrement de jeton. Vous devrez effectuer ces tâches manuellement.
    
Si ADFS est configuré pour renouveler le jeton de déchiffrement automatiquement des certificats et de signature de jetons (AutoCertificateRollover est défini sur TRUE), vous pouvez déterminer quand il seront renouvelés:

CertificateGenerationThreshold décrit le nombre de jours avant la date du certificat pas après un nouveau certificat sera généré.

CertificatePromotionThreshold détermine le nombre de jours après le nouveau certificat est généré qu’il sera promu pour être le certificat principal (en d’autres termes, ADFS démarrera l’utilisent pour se connecter qu’elle émet des jetons et déchiffrer les jetons à partir de fournisseurs d’identité).

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
Si ADFS est configuré pour renouveler le jeton de déchiffrement automatiquement des certificats et de signature de jetons (AutoCertificateRollover est défini sur TRUE), vous pouvez déterminer quand il seront renouvelés:

 - **CertificateGenerationThreshold** décrit le nombre de jours avant la date du certificat pas après un nouveau certificat sera généré.
 - **CertificatePromotionThreshold** détermine le nombre de jours après le nouveau certificat est généré qu’il sera promu pour être le certificat principal (en d’autres termes, ADFS démarrera l’utilisent pour se connecter qu’elle émet des jetons et déchiffrer les jetons d’identité fournisseurs).

## <a name="determine-when-the-current-certificates-expire"></a>Déterminer la date d’expiration des certificats actuels
Vous pouvez utiliser la procédure suivante pour identifier les certificats de déchiffrement de jeton et de signature de jetons principal et pour déterminer la date d’expiration des certificats actuels.

Vous pouvez exécuter la commande Windows PowerShell suivante: `Get-AdfsCertificate –CertificateType token-signing` (ou `Get-AdfsCertificate –CertificateType token-decrypting `). Ou vous pouvez examiner les certificats en cours dans la console MMC: Service-& gt; certificats.

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

Le certificat pour lequel le **IsPrimary** a la valeur **True** est le certificat ADFS utilise actuellement.

La date indiquée pour le **pas après** est la date à laquelle un nouveau jeton principal signature ou le déchiffrement du certificat doit être configuré.

Pour assurer la continuité de service, tous les partenaires de fédération (représentés par des approbations de partie de confiance ou approbations de fournisseur de revendications dans votre batterie ADFS) doivent utiliser la nouvelle signature de jetons et les certificats de déchiffrement de jeton avant cette expiration. Nous vous recommandons de commencer la planification de ce processus au moins 60jours à l’avance.

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>Création d’un nouveau certificat auto-signé manuellement avant la fin de la période de grâce
Vous pouvez utiliser les étapes suivantes pour générer un nouveau certificat auto-signé manuellement avant la fin de la période de grâce.

1. Assurez-vous que vous êtes connecté au serveur principal ADFS.
2. Ouvrez Windows PowerShell et exécutez la commande suivante: `Add-PSSnapin "microsoft.adfs.powershell"`
3. Si vous le souhaitez, vous pouvez vérifier la signature des certificats en cours dans ADFS. Pour ce faire, exécutez la commande suivante:`Get-ADFSCertificate –CertificateType token-signing`. Examinez la sortie de commande pour afficher les dates pas après de tous les certificats répertoriés.
4. Pour générer un nouveau certificat, exécutez la commande suivante pour renouveler et mettre à jour les certificats sur le serveur ADFS:`Update-ADFSCertificate –CertificateType token-signing`.
5. Vérifier la mise à jour en exécutant la commande suivante à nouveau: `Get-ADFSCertificate –CertificateType token-signing`
6. Deux certificats doivent être répertoriés maintenant, un d’eux a un **pas après** date d’environ un an et pour lesquels la **IsPrimary** valeur est **False**.

>[!IMPORTANT]
>Pour éviter une interruption de service, vous devez mettre à jour les informations de certificat sur Azure ActiveDirectory en exécutant les étapes dans la procédure de mise à jour Azure AD avec un certificat de signature de jetons valide.

## <a name="if-youre-not-using-self-signed-certificates"></a>Si vous n’utilisez pas les certificats auto-signés...
Si vous êtes ne pas à l’aide de la valeur par défaut généré automatiquement, signature de jetons auto-signé et les certificats de déchiffrement de jeton, vous devez renouveler et configurer ces certificats manuellement.

Tout d’abord, vous devez obtenir un nouveau certificat à partir de votre autorité de certification et importez-le dans le magasin de certificats personnel d’ordinateur local sur chaque serveur de fédération. Pour obtenir des instructions, voir la [importer un certificat](https://technet.microsoft.com/library/cc754489.aspx) article.

Vous devez configurer ce certificat en tant que le secondaire ADFS signature de jetons ou le certificat de déchiffrement. (Vous configurer un certificat secondaire pour permettre à vos partenaires de fédération suffisamment de temps pour utiliser ce nouveau certificat avant de promouvoir il pour le certificat principal).

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>Pour configurer un nouveau certificat en tant qu’un certificat secondaire
1. Ouvrez PowerShell et exécutez la commande suivante: `Set-ADFSProperties -AutoCertificateRollover $false`
2. Une fois, vous avez importé le certificat. Ouvrez le **gestion ADFS** console.
3. Développez **Service**, puis **certificats**.
4. Dans le volet Actions, cliquez sur **certificat de signature d’Ajouter jeton**.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. Sélectionnez le nouveau certificat à partir de la liste des certificats affichés, puis cliquez sur OK.
6.  Ouvrez PowerShell et exécutez la commande suivante: `Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>Vérifiez que le nouveau certificat a une clé privée associée et que le compte de service ADFS bénéficie d’autorisations de lecture sur la clé privée. Vérifier sur chaque serveur de fédération. Pour ce faire, dans le composant logiciel enfichable Certificats, cliquez sur le nouveau certificat, cliquez sur toutes les tâches, puis cliquez sur Gérer les clés privées.

Une fois que vous avez prévu assez de temps pour vos partenaires de fédération à utiliser votre nouveau certificat (ils extraient les métadonnées de fédération ou les envoyer la clé publique de votre nouveau certificat), vous devez promouvoir le certificat secondaire pour le certificat principal.

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>Pour promouvoir le nouveau certificat à partir de secondaire principal

1. Ouvrez le **gestion ADFS** console.
2. Développez **Service**, puis **certificats**.
3. Cliquez sur le certificat de signature de jetons secondaire.
4. Dans le **Actions** volet, cliquez sur **définir comme principale**. À l’invite de confirmation, cliquez sur Oui.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>Mise à jour des partenaires de fédération

### <a name="partners-who-can-consume-federation-metadata"></a>Partenaires qui peuvent utiliser des métadonnées de fédération
Si vous avez renouvelé et configurez une nouvelle signature de jetons ou un certificat de déchiffrement de jeton, vous devez vous assurer que tous vos partenaires de fédération (organisation ou le compte d’organisation partenaires de ressources qui sont représentées dans ADFS par partie de confiance tiers approbations et les nouveaux certificats ont récupéré les approbations de fournisseur de revendications).

### <a name="partners-who-can-not-consume-federation-metadata"></a>Partenaires qui ne peut pas consommer des métadonnées de fédération
Si vos partenaires de fédération ne peut pas consommer vos métadonnées de fédération, vous devez manuellement leur envoyer la clé publique de votre nouveau certificat de signature de jetons / déchiffrement de jeton. Envoyer votre nouvelle clé publique du certificat (.cer fichier ou .p7b si vous souhaitez inclure la totalité de la chaîne) à l’ensemble de votre ressource organisation ou compte d’organisation de partenaires (représenté par la partie de confiance dans ADFS tiers approbations et fournisseur de revendications approbations). Avoir les partenaires à implémenter des modifications sur leur côté d’approuver les nouveaux certificats.

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>Promouvoir à principal (si AutoCertificateRollover est False)
Si **AutoCertificateRollover** est défini sur **False**, ADFS ne génère pas automatiquement ou démarrage à l’aide de nouveau de signature de jeton ou de certificats de déchiffrement de jeton. Vous devrez effectuer ces tâches manuellement.
Après un délai suffisant pour l’ensemble de vos partenaires de fédération d’utiliser le nouveau certificat secondaire, promouvoir ce certificat secondaire principal (dans le composant logiciel enfichable MMC, cliquez sur la signature de jetons secondaire de certificat dans le volet Actions, cliquez sur **Définir comme principale**.)

## <a name="updating-azure-ad"></a>Mise à jour d’Azure AD
ADFS fournit un accès d’authentification unique aux services cloud de MicrosoftOffice365 par l’authentification des utilisateurs via leurs informations d’identification de domaine ActiveDirectory existantes.  Pour plus d’informations sur l’utilisation de certificats, voir [renouveler les certificats de fédération pour Office365 et Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-o365-certs).