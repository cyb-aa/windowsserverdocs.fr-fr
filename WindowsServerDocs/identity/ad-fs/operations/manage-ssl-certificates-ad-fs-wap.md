---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: Gestion des certificats SSL dans AD FS et WAP 2016 dans Windows Server 2016
description: Gestion des certificats SSL dans AD FS et WAP 2016 dans Windows Server 2016
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b832756e123bee0223738ee804ac3a4db2371e84
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855292"
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>Gestion des certificats SSL dans AD FS et WAP 2016 dans Windows Server 2016



Cet article explique comment déployer un nouveau certificat SSL sur vos serveurs AD FS et WAP.

>[!NOTE]
>La méthode recommandée pour remplacer le certificat SSL à l’avenir pour une batterie de serveurs AD FS consiste à utiliser Azure AD Connect.  Pour plus d’informations [, consultez mettre à jour le certificat SSL pour une batterie de services ADFS (AD FS)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>Obtention de vos certificats SSL
Pour les batteries de AD FS de production, il est recommandé d’avoir un certificat SSL approuvé publiquement. Cela est généralement obtenu en soumettant une demande de signature de certificat (CSR) à un fournisseur de certificats public tiers. Il existe plusieurs façons de générer la CSR, notamment à partir d’un PC Windows 7 ou supérieur. Votre fournisseur doit avoir une documentation à ce propos.

- Assurez-vous que le certificat est conforme aux [exigences de certificat SSL du proxy d’application Web et du AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>Nombre de certificats nécessaires
Il est recommandé d’utiliser un certificat SSL commun sur tous les serveurs de proxy d’application Web AD FS et. Pour plus d’informations sur la configuration requise, consultez le document [AD FS et les certificats SSL de proxy d’application Web](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>Exigences en matière de certificat SSL
Pour connaître les conditions requises, telles que l’affectation de noms, la racine de confiance et les extensions, consultez le document [AD FS et les certificats SSL de proxy d’application Web](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>Remplacement du certificat SSL pour AD FS
> [!NOTE]
> Le certificat SSL AD FS n’est pas identique au certificat de communication du service AD FS qui se trouve dans le composant logiciel enfichable Gestion AD FS. Pour modifier le certificat SSL AD FS, vous devez utiliser PowerShell.

Tout d’abord, déterminez le mode de liaison de certificat que vos serveurs AD FS exécutent : la liaison d’authentification par certificat par défaut ou le mode de liaison TLS du client secondaire.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>Remplacement du certificat SSL pour AD FS s’exécutant dans le mode de liaison d’authentification par certificat par défaut
AD FS par défaut effectue l’authentification par certificat d’appareil sur le port 443 et l’authentification par certificat utilisateur sur le port 49443 (ou sur un port configurable qui n’est pas 443).
Dans ce mode, utilisez l’applet de commande PowerShell Set-AdfsSslCertificate pour gérer le certificat SSL.

Suivez les étapes ci-dessous :

1. Tout d’abord, vous devez obtenir le nouveau certificat. Cela s’effectue généralement en soumettant une demande de signature de certificat (CSR) à un fournisseur de certificats public tiers. Il existe plusieurs façons de générer la CSR, notamment à partir d’un PC Windows 7 ou supérieur. Votre fournisseur doit avoir une documentation à ce propos.

    * Assurez-vous que le certificat est conforme aux [exigences de certificat SSL du proxy d’application Web et du AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Une fois que vous avez obtenu la réponse de votre fournisseur de certificats, importez-la dans le magasin de l’ordinateur local sur chaque AD FS et le serveur proxy d’application Web.

1. Sur le serveur de AD FS **principal** , utilisez l’applet de commande suivante pour installer le nouveau certificat SSL

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

L’empreinte numérique du certificat peut être trouvée en exécutant cette commande :

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Remarques supplémentaires

* L’applet de commande Set-AdfsSslCertificate est une applet de commande à plusieurs nœuds ; Cela signifie qu’il ne doit être exécuté qu’à partir du serveur principal et que tous les nœuds de la batterie de serveurs seront mis à jour. Il s’agit d’une nouveauté du serveur 2016. Sur le serveur 2012 R2, vous deviez exécuter Set-AdfsSslCertificate sur chaque serveur.
* L’applet de commande Set-AdfsSslCertificate doit être exécutée uniquement sur le serveur principal. Le serveur principal doit exécuter le serveur 2016 et le niveau de comportement de la batterie de serveurs doit être élevé à 2016.
* L’applet de commande Set-AdfsSslCertificate utilisera la communication à distance PowerShell pour configurer les autres serveurs AD FS, assurez-vous que le port 5985 (TCP) est ouvert sur les autres nœuds.
* L’applet de commande Set-AdfsSslCertificate accorde au principal adfssrv des autorisations de lecture sur les clés privées du certificat SSL. Ce principal représente le service AD FS. Il n’est pas nécessaire d’accorder au compte de service AD FS un accès en lecture aux clés privées du certificat SSL.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>Remplacement du certificat SSL pour AD FS s’exécutant en mode de liaison TLS alternatif
Lorsqu’il est configuré en mode de liaison TLS du client alternatif, AD FS effectue également l’authentification par certificat de l’appareil sur le port 443 et l’authentification par certificat utilisateur sur le port 443, sur un nom d’hôte différent. Le nom d’hôte du certificat de l’utilisateur est le AD FS nom d’hôte avant le préfixe « certauth », par exemple « certauth.fs.contoso.com ».
Dans ce mode, utilisez l’applet de commande PowerShell Set-AdfsAlternateTlsClientBinding pour gérer le certificat SSL. Cela permet de gérer non seulement la liaison TLS du client alternative, mais également toutes les autres liaisons sur lesquelles AD FS définit également le certificat SSL.

Suivez les étapes ci-dessous :

1. Tout d’abord, vous devez obtenir le nouveau certificat. Cela s’effectue généralement en soumettant une demande de signature de certificat (CSR) à un fournisseur de certificats public tiers. Il existe plusieurs façons de générer la CSR, notamment à partir d’un PC Windows 7 ou supérieur. Votre fournisseur doit avoir une documentation à ce propos.

    * Assurez-vous que le certificat est conforme aux [exigences de certificat SSL du proxy d’application Web et du AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Une fois que vous avez obtenu la réponse de votre fournisseur de certificats, importez-la dans le magasin de l’ordinateur local sur chaque AD FS et le serveur proxy d’application Web.

1. Sur le serveur de AD FS **principal** , utilisez l’applet de commande suivante pour installer le nouveau certificat SSL

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

L’empreinte numérique du certificat peut être trouvée en exécutant cette commande :

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Remarques supplémentaires

* L’applet de commande Set-AdfsAlternateTlsClientBinding est une applet de commande à plusieurs nœuds ; Cela signifie qu’il ne doit être exécuté qu’à partir du serveur principal et que tous les nœuds de la batterie de serveurs seront mis à jour.
* L’applet de commande Set-AdfsAlternateTlsClientBinding doit être exécutée uniquement sur le serveur principal. Le serveur principal doit exécuter le serveur 2016 et le niveau de comportement de la batterie de serveurs doit être élevé à 2016.
* L’applet de commande Set-AdfsAlternateTlsClientBinding utilisera la communication à distance PowerShell pour configurer les autres serveurs AD FS, assurez-vous que le port 5985 (TCP) est ouvert sur les autres nœuds.
* L’applet de commande Set-AdfsAlternateTlsClientBinding accorde au principal adfssrv des autorisations de lecture sur les clés privées du certificat SSL. Ce principal représente le service AD FS. Il n’est pas nécessaire d’accorder au compte de service AD FS un accès en lecture aux clés privées du certificat SSL.

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>Remplacement du certificat SSL pour le proxy d’application Web
Pour configurer la liaison d’authentification par certificat par défaut ou le mode de liaison TLS du client alternatif sur le WAP, nous pouvons utiliser l’applet de commande Set-WebApplicationProxySslCertificate.
Pour remplacer le certificat SSL du proxy d’application Web, sur **chaque** serveur proxy d’application Web, utilisez l’applet de commande suivante pour installer le nouveau certificat SSL :

```powershell
Set-WebApplicationProxySslCertificate -Thumbprint '<thumbprint of new cert>'
```

Si l’applet de commande ci-dessus échoue parce que l’ancien certificat a déjà expiré, reconfigurez le proxy à l’aide des applets de commande suivantes :

```powershell
$cred = Get-Credential
```

Entrez les informations d’identification d’un utilisateur de domaine qui est administrateur local sur le serveur AD FS

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>Références supplémentaires  
* [Prise en charge AD FS de la liaison de l’autre nom d’hôte pour l’authentification par certificat](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [AD FS et certificater les informations de propriété de KeySpec](../technical-reference/AD-FS-and-KeySpec-Property.md)
