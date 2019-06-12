---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: Gestion des certificats SSL dans AD FS et WAP 2016 dans Windows Server 2016
description: Gestion des certificats SSL dans AD FS et WAP 2016 dans Windows Server 2016
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9acdbe2be56b990876fe365c1f535aaa411009c5
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501633"
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>Gestion des certificats SSL dans AD FS et WAP 2016 dans Windows Server 2016



Cet article décrit comment déployer un nouveau certificat SSL sur vos serveurs AD FS et WAP.

>[!NOTE]
>La méthode recommandée pour remplacer le certificat SSL à l’avenir pour une batterie de serveurs AD FS consiste à utiliser Azure AD Connect.  Pour plus d’informations, consultez [mettre à jour le certificat SSL pour une batterie de serveurs Active Directory Federation Services (AD FS)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>Obtention de vos certificats SSL
Pour les batteries de serveurs de production AD FS un certificat SSL publiquement approuvé est recommandé. Cela est généralement obtenue en soumettant une demande de signature de certificat (CSR) à un tiers, le fournisseur de certificat public. Il existe plusieurs façons pour générer la CSR, y compris à partir d’un Windows 7 ou un PC supérieure. Votre fournisseur doit avoir la documentation pour cela.

- Vérifiez que le certificat répond à la [AD FS et SSL de Proxy d’Application Web des spécifications relatives aux certificats](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>Nombre de certificats est nécessaires.
Il est recommandé d’utiliser un certificat SSL commun entre les serveurs de tous les services AD FS et Proxy d’Application Web. Pour connaître la configuration requise, consultez le document [AD FS et SSL de Proxy d’Application Web des spécifications relatives aux certificats](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>Exigences en matière de certificat SSL
Pour cette configuration, y compris d’affectation de noms, racine de confiance et d’extensions voir le document [AD FS et SSL de Proxy d’Application Web des spécifications relatives aux certificats](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>Remplacement du certificat SSL pour AD FS
> [!NOTE]
> Le certificat SSL AD FS n’est pas le même que le certificat de communications de Service AD FS trouvé dans le composant logiciel enfichable Gestion AD FS. Pour modifier le certificat SSL AD FS, vous devrez utiliser PowerShell.

Tout d’abord, déterminez quel mode de liaison de certificat vos serveurs AD FS sont en cours d’exécution : liaison de l’authentification de certificat par défaut, ou mode de liaison TLS autre client.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>Remplacement du certificat SSL pour AD FS en cours d’exécution par défaut mode de liaison de certificat d’authentification
AD FS par défaut effectue l’authentification par certificat de périphérique sur le port 443 et authentification des certificats utilisateur sur le port 49443 (ou un port configurable qui n’est pas 443).
Dans ce mode, utilisez l’applet de commande powershell Set-AdfsSslCertificate pour gérer le certificat SSL.

Suivez les étapes ci-dessous :

1. Tout d’abord, vous devez obtenir le nouveau certificat. Cela est généralement effectuée en envoyant une demande de signature de certificat (CSR) à un tiers, le fournisseur de certificat public. Il existe plusieurs façons pour générer la CSR, y compris à partir d’un Windows 7 ou un PC supérieure. Votre fournisseur doit avoir la documentation pour cela.

    * Vérifiez que le certificat répond à la [AD FS et SSL de Proxy d’Application Web des spécifications relatives aux certificats](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Une fois que vous obtenez la réponse à partir de votre fournisseur de certificats, importez-le dans le magasin ordinateur Local sur chaque serveur AD FS et Proxy d’Application Web.

1. Sur le **principal** serveur AD FS, utilisez l’applet de commande suivante pour installer le nouveau certificat SSL

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

Vous trouverez l’empreinte de certificat en exécutant cette commande :

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Remarques supplémentaires

* L’applet de commande Set-AdfsSslCertificate est une applet de commande à plusieurs nœuds ; Cela signifie qu’il doit uniquement s’exécuter depuis le serveur principal et tous les nœuds dans la batterie de serveurs seront actualisés. Ceci est une nouveauté dans Server 2016. Sur Server 2012 R2, vous deviez exécuter Set-AdfsSslCertificate sur chaque serveur.
* L’applet de commande Set-AdfsSslCertificate doit uniquement être exécuté sur le serveur principal. Le serveur principal doit être en cours d’exécution Server 2016 et le niveau de comportement de batterie de serveurs doit être déclenché à 2016.
* L’applet de commande Set-AdfsSslCertificate utilisera la communication à distance PowerShell pour configurer les autres serveurs AD FS, vérifiez que le port 5985 (TCP) est ouvert sur les autres nœuds.
* L’applet de commande Set-AdfsSslCertificate accordera des autorisations de lecture du principal d’adfssrv aux clés privées du certificat SSL. Cette entité représente le service AD FS. Il n’est pas nécessaire d’accorder au compte de service AD FS en lecture des accès aux clés privées du certificat SSL.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>Remplacement du certificat SSL pour AD FS en cours d’exécution dans un autre mode de liaison TLS
Lorsque configuré dans un autre client le mode de liaison TLS, AD FS effectue l’authentification par certificat de périphérique sur le port 443 et authentification des certificats utilisateur sur le port 443, sur un nom d’hôte différent. Le nom d’hôte du certificat utilisateur est l’AD FS hostname précédés de « certauth », par exemple « certauth.fs.contoso.com ».
Dans ce mode, utilisez l’applet de commande powershell Set-AdfsAlternateTlsClientBinding pour gérer le certificat SSL. Cela sera gérer non seulement la liaison TLS client alternatif, mais toutes les autres liaisons sur lequel AD FS définit également le certificat SSL.

Suivez les étapes ci-dessous :

1. Tout d’abord, vous devez obtenir le nouveau certificat. Cela est généralement effectuée en envoyant une demande de signature de certificat (CSR) à un tiers, le fournisseur de certificat public. Il existe plusieurs façons pour générer la CSR, y compris à partir d’un Windows 7 ou un PC supérieure. Votre fournisseur doit avoir la documentation pour cela.

    * Vérifiez que le certificat répond à la [AD FS et SSL de Proxy d’Application Web des spécifications relatives aux certificats](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Une fois que vous obtenez la réponse à partir de votre fournisseur de certificats, importez-le dans le magasin ordinateur Local sur chaque serveur AD FS et Proxy d’Application Web.

1. Sur le **principal** serveur AD FS, utilisez l’applet de commande suivante pour installer le nouveau certificat SSL

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

Vous trouverez l’empreinte de certificat en exécutant cette commande :

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Remarques supplémentaires

* L’applet de commande Set-AdfsAlternateTlsClientBinding est une applet de commande à plusieurs nœuds ; Cela signifie qu’il doit uniquement s’exécuter depuis le serveur principal et tous les nœuds dans la batterie de serveurs seront actualisés.
* L’applet de commande Set-AdfsAlternateTlsClientBinding doit uniquement être exécuté sur le serveur principal. Le serveur principal doit être en cours d’exécution Server 2016 et le niveau de comportement de batterie de serveurs doit être déclenché à 2016.
* L’applet de commande Set-AdfsAlternateTlsClientBinding utilisera la communication à distance PowerShell pour configurer les autres serveurs AD FS, vérifiez que le port 5985 (TCP) est ouvert sur les autres nœuds.
* L’applet de commande Set-AdfsAlternateTlsClientBinding accordera des autorisations de lecture du principal d’adfssrv aux clés privées du certificat SSL. Cette entité représente le service AD FS. Il n’est pas nécessaire d’accorder au compte de service AD FS en lecture des accès aux clés privées du certificat SSL.

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>Remplacement du certificat SSL pour le Proxy d’Application Web
Pour configurer la liaison de l’authentification de certificat par défaut ou le mode de liaison TLS autre client sur le proxy d’application Web, nous pouvons utiliser l’applet de commande Set-WebApplicationProxySslCertificate.
Pour remplacer le certificat SSL de Proxy d’Application Web, sur **chaque** serveur Proxy d’Application Web utiliser l’applet de commande suivante pour installer le nouveau certificat SSL :

```powershell
Set-WebApplicationProxySslCertificate -Thumbprint '<thumbprint of new cert>'
```

Si l’applet de commande ci-dessus échoue, car l’ancien certificat a déjà expiré, reconfigurez le proxy à l’aide des applets de commande suivantes :

```powershell
$cred = Get-Credential
```

Entrez les informations d’identification d’un utilisateur de domaine qui est administrateur local sur le serveur AD FS

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>Références supplémentaires  
* [Prise en charge AD FS de la liaison de l’autre nom d’hôte pour l’authentification par certificat](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [AD FS et la propriété KeySpec informations de certificat](../technical-reference/AD-FS-and-KeySpec-Property.md)
