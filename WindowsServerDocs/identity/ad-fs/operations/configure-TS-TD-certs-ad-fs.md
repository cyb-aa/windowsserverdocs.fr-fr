---
title: Obtenir et configurer des certificats de signature de jetons et de déchiffrement de jetons pour AD FS
description: Ce document explique comment obtenir et configurer les certificats TS et TD pour AD FS
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 608f78ed03bc102cbdffb8abcc52244450b97b3c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865625"
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>Obtenir et configurer des certificats TS et TD pour AD FS

Cette rubrique décrit les tâches et les procédures que vous pouvez effectuer pour vous assurer que vos certificats de signature de jetons et de déchiffrement de jetons AD FS sont à jour.

Les certificats de signature de jetons sont des certificats X509 standard qui sont utilisés pour signer de façon sécurisée tous les jetons émis par le serveur de Fédération. Les certificats de déchiffrement de jeton sont des certificats X509 standard qui sont utilisés pour déchiffrer les jetons entrants. Ils sont également publiés dans les métadonnées de Fédération.

Pour plus d’informations, consultez [certificats requis](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>Déterminer si AD FS renouvelle automatiquement les certificats
Par défaut, AD FS est configuré pour générer automatiquement des certificats de signature de jetons et de déchiffrement de jeton, à la fois au moment de la configuration initiale et lorsque les certificats approchent de leur date d’expiration.

Vous pouvez exécuter la commande Windows PowerShell suivante : `Get-AdfsProperties`.
  
  ![ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
La propriété AutoCertificateRollover indique si AD FS est configuré pour renouveler automatiquement les certificats de signature de jetons et de déchiffrement de jeton.

Si AutoCertificateRollover a la valeur TRUE, les certificats AD FS sont renouvelés et configurés automatiquement dans AD FS. Une fois le nouveau certificat configuré, afin d’éviter une panne, vous devez vous assurer que chaque partenaire de Fédération (représenté dans votre batterie AD FS par les approbations de partie de confiance ou les approbations de fournisseur de revendications) est mis à jour avec ce nouveau certificat.
    
Si AD FS n’est pas configuré pour renouveler automatiquement les certificats de signature de jetons et de déchiffrement de jeton (si AutoCertificateRollover est défini sur false), AD FS ne génère pas ou ne commence pas automatiquement à utiliser les nouveaux certificats de signature de jetons ou de déchiffrement de jetons. Vous devez effectuer ces tâches manuellement.
    
Si AD FS est configuré pour renouveler automatiquement les certificats de signature de jetons et de déchiffrement de jeton (AutoCertificateRollover est défini sur TRUE), vous pouvez déterminer à quel moment ils seront renouvelés :

CertificateGenerationThreshold décrit le nombre de jours à l’avance du certificat qui n’est pas après la date de génération d’un nouveau certificat.

CertificatePromotionThreshold détermine le nombre de jours après la génération du nouveau certificat qui sera promu en tant que certificat principal (en d’autres termes, AD FS commencera à l’utiliser pour signer les jetons qu’il émet et déchiffre les jetons des fournisseurs d’identité).

![ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
Si AD FS est configuré pour renouveler automatiquement les certificats de signature de jetons et de déchiffrement de jeton (AutoCertificateRollover est défini sur TRUE), vous pouvez déterminer à quel moment ils seront renouvelés :

 - **CertificateGenerationThreshold** décrit le nombre de jours à l’avance du certificat qui n’est pas après la date de génération d’un nouveau certificat.
 - **CertificatePromotionThreshold** détermine le nombre de jours après la génération du nouveau certificat qui sera promu en tant que certificat principal (en d’autres termes, AD FS commencera à l’utiliser pour signer les jetons qu’il émet et déchiffrer les jetons de l’identité fournisseurs).

## <a name="determine-when-the-current-certificates-expire"></a>Déterminer à quel moment les certificats actuels expirent
Vous pouvez utiliser la procédure suivante pour identifier les certificats de signature de jetons principaux et de déchiffrement de jetons et pour déterminer à quel moment les certificats actuels expirent.

Vous pouvez exécuter la commande Windows PowerShell suivante : `Get-AdfsCertificate –CertificateType token-signing` (ou `Get-AdfsCertificate –CertificateType token-decrypting `). Ou vous pouvez examiner les certificats actuels dans la console MMC : Certificats de > de service.

![ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

Le certificat pour lequel la valeur **IsPrimary** est définie sur **true** est le certificat actuellement utilisé par AD FS.

La date affichée pour le **pas après** est la date à laquelle un nouveau certificat principal de signature ou de déchiffrement de jetons doit être configuré.

Pour garantir la continuité de service, tous les partenaires de Fédération (représentés dans votre batterie de AD FS par les approbations de partie de confiance ou les approbations de fournisseur de revendications) doivent utiliser les nouveaux certificats de signature de jetons et de déchiffrement de jetons avant cet expiration. Nous vous recommandons de commencer à planifier ce processus au moins 60 jours à l’avance.

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>Génération manuelle d’un nouveau certificat auto-signé avant la fin de la période de grâce
Vous pouvez utiliser les étapes suivantes pour générer manuellement un nouveau certificat auto-signé avant la fin de la période de grâce.

1. Vérifiez que vous êtes connecté au serveur de AD FS principal.
2. Ouvrez Windows PowerShell et exécutez la commande suivante :`Add-PSSnapin "microsoft.adfs.powershell"`
3. Si vous le souhaitez, vous pouvez vérifier les certificats de signature actuels dans AD FS. Pour ce faire, exécutez la commande suivante : `Get-ADFSCertificate –CertificateType token-signing`. Examinez la sortie de la commande pour voir les dates pas après des certificats listés.
4. Pour générer un nouveau certificat, exécutez la commande suivante pour renouveler et mettre à jour les certificats sur le serveur `Update-ADFSCertificate –CertificateType token-signing`de AD FS :.
5. Vérifiez la mise à jour en exécutant à nouveau la commande suivante :`Get-ADFSCertificate –CertificateType token-signing`
6. Deux certificats doivent être listés, dont l’un n’a **pas** une date ultérieure d’environ un an et pour lesquels la valeur **IsPrimary** est **false**.

>[!IMPORTANT]
>Pour éviter une interruption de service, mettez à jour les informations de certificat sur Azure AD en exécutant les étapes de la procédure de mise à jour de Azure AD avec un certificat de signature de jetons valide.

## <a name="if-youre-not-using-self-signed-certificates"></a>Si vous n’utilisez pas de certificats auto-signés...
Si vous n’utilisez pas les certificats de signature de jeton et de déchiffrement de jetons auto-signés générés automatiquement par défaut, vous devez renouveler et configurer ces certificats manuellement.

Tout d’abord, vous devez obtenir un nouveau certificat auprès de votre autorité de certification et l’importer dans le magasin de certificats personnels de l’ordinateur local sur chaque serveur de Fédération. Pour obtenir des instructions, consultez l’article [Importer un certificat](https://technet.microsoft.com/library/cc754489.aspx) .

Vous devez ensuite configurer ce certificat en tant que certificat secondaire AD FS la signature ou le déchiffrement de jetons. (Vous le configurez en tant que certificat secondaire pour permettre à vos partenaires de la Fédération de consommer suffisamment de temps pour utiliser ce nouveau certificat avant de le promouvoir au certificat principal).

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>Pour configurer un nouveau certificat en tant que certificat secondaire
1. Ouvrez PowerShell et exécutez la commande suivante :`Set-ADFSProperties -AutoCertificateRollover $false`
2. Une fois que vous avez importé le certificat. Ouvrez la console de **gestion AD FS** .
3. Développez **service** , puis sélectionnez **certificats**.
4. Dans le volet Actions, cliquez sur **Ajouter un certificat de signature de jetons**.
![ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. Sélectionnez le nouveau certificat dans la liste des certificats affichés, puis cliquez sur OK.
6.  Ouvrez PowerShell et exécutez la commande suivante :`Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>Vérifiez que le nouveau certificat est associé à une clé privée et que le compte de service AD FS dispose des autorisations de lecture sur la clé privée. Vérifiez cela sur chaque serveur de Fédération. Pour ce faire, dans le composant logiciel enfichable Certificats, cliquez avec le bouton droit sur le nouveau certificat, cliquez sur toutes les tâches, puis sur gérer les clés privées.

Une fois que vous avez accordé suffisamment de temps à vos partenaires de Fédération pour utiliser votre nouveau certificat (soit ils extraient vos métadonnées de Fédération, soit vous leur envoyez la clé publique de votre nouveau certificat), vous devez promouvoir le certificat secondaire en certificat principal.

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>Pour promouvoir le nouveau certificat secondaire en principal

1. Ouvrez la console de **gestion AD FS** .
2. Développez **service** , puis sélectionnez **certificats**.
3. Cliquez sur le certificat de signature de jetons secondaire.
4. Dans le volet **actions** , cliquez sur **définir comme principal**. Cliquez sur Oui à l’invite de confirmation.
![ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>Mise à jour des partenaires de Fédération

### <a name="partners-who-can-consume-federation-metadata"></a>Partenaires qui peuvent consommer les métadonnées de Fédération
Si vous avez renouvelé et configuré un nouveau certificat de signature de jetons ou de déchiffrement de jetons, vous devez vous assurer que tous les partenaires de votre Fédération (Organisation de ressources ou partenaire de l’organisation de compte sont représentés dans votre AD FS par des approbations de partie de confiance et les approbations de fournisseur de revendications) ont choisi les nouveaux certificats.

### <a name="partners-who-can-not-consume-federation-metadata"></a>Partenaires qui ne peuvent pas consommer les métadonnées de Fédération
Si vos partenaires de Fédération ne peuvent pas consommer vos métadonnées de Fédération, vous devez leur envoyer manuellement la clé publique de votre nouveau certificat de signature de jetons/déchiffrement de jetons. Envoyez votre nouvelle clé publique de certificat (fichier. cer ou. p7b si vous souhaitez inclure la chaîne entière) à l’ensemble de vos partenaires d’organisation de ressources ou d’organisation de compte (représentés dans votre AD FS par des approbations de partie de confiance et des approbations de fournisseur de revendications). Demander aux partenaires d’implémenter les modifications de leur côté pour approuver les nouveaux certificats.

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>Promouvoir en principal (si AutoCertificateRollover a la valeur false)
Si **AutoCertificateRollover** a la valeur **false**, AD FS ne génère pas ou ne commence pas automatiquement à utiliser les nouveaux certificats de signature de jetons ou de déchiffrement de jetons. Vous devez effectuer ces tâches manuellement.
Après avoir donné suffisamment de temps pour que tous vos partenaires de Fédération consomment le nouveau certificat secondaire, promouvez ce certificat secondaire sur le serveur principal (dans le composant logiciel enfichable MMC, cliquez sur le certificat de signature de jetons secondaire et dans le volet Actions, cliquez sur **Définir comme principal**.)

## <a name="updating-azure-ad"></a>Mise à jour de Azure AD
AD FS fournit un accès d’authentification unique aux services Cloud Microsoft tels qu’Office 365 en authentifiant les utilisateurs à l’aide de leurs informations d’identification AD DS existantes.  Pour plus d’informations sur l’utilisation des certificats [, consultez renouveler des certificats de Fédération pour Office 365 et Azure ad](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).