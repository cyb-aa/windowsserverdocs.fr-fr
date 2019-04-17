---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: La gestion des certificats SSL dans AD FS et WAP dans Windows Server 2016
description: La gestion des certificats SSL dans AD FS et WAP dans Windows Server 2016
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5156b3ad357ab8edb5e08a89a459beaf9b8c9b1a
ms.sourcegitcommit: ca7dc3d56a33924ae5fe0e9ffb1274da6dc4e54d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2017
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>La gestion des certificats SSL dans AD FS et WAP dans Windows Server 2016

>S’applique à: Windows Server2016

Cet article décrit comment déployer un nouveau certificat SSL sur vos serveurs AD FS et WAP.

>[!NOTE]
>La méthode recommandée pour remplacer le certificat SSL à l’avenir pour une batterie ADFS consiste à utiliser Azure AD Connect.  Pour plus d’informations, voir [mettre à jour le certificat SSL pour une batterie de serveurs ActiveDirectory Federation Services (ADFS)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>Obtention de vos certificats SSL
Un certificat SSL publiquement approuvé est recommandé pour les batteries de serveurs de production AD FS. Cela est généralement obtenue en envoyant une demande de signature de certificat (DSC) à un tiers, le fournisseur de certificats publics. Il existe plusieurs façons pour générer la DSC, y compris à partir d’un PC plus élevée ou de Windows 7. Votre fournisseur doit avoir la documentation pour ce faire.

- Assurez-vous que le certificat remplit les [configuration requise des certificats AD FS et SSL de Proxy d’Application Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>Nombre de certificats est nécessaires.
Il est recommandé d’utiliser un certificat SSL courantes sur les serveurs de tous les services AD FS et le Proxy d’Application Web. Pour les exigences détaillées, consultez le document [configuration requise des certificats AD FS et SSL de Proxy d’Application Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>Configuration requise des certificats SSL
Pour cette configuration, y compris d’attribution de noms, racine de confiance et des extensions de voir le document [configuration requise des certificats AD FS et SSL de Proxy d’Application Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>Remplacement du certificat SSL pour AD FS
> [!NOTE]
> Le certificat SSL ADFS n’est pas le même que le certificat de communications de Service ADFS trouvé dans le composant logiciel enfichable Gestion ADFS. Pour modifier le certificat SSL AD FS, vous devrez utiliser PowerShell.

Tout d’abord, déterminez quel mode de liaison de certificat vos serveurs AD FS sont en cours d’exécution: liaison de l’authentification de certificat par défaut ou le mode de liaison TLS autre client.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>Remplacement du certificat SSL pour AD FS en cours d’exécution par défaut mode de liaison de certificat d’authentification
AD FS par défaut effectue l’authentification par certificat de périphérique sur le port 443 et authentification des certificats utilisateur sur le port 49443 (ou un port configurable qui n’est pas 443).
Dans ce mode, utilisez l’applet de commande powershell Set-AdfsSslCertificate pour gérer le certificat SSL.

Suivez les étapes ci-dessous:

1. Tout d’abord, vous devez obtenir le nouveau certificat. Cela est généralement effectuée par envoi d’une demande de signature de certificat (DSC) à un tiers, le fournisseur de certificats publics. Il existe plusieurs façons pour générer la DSC, y compris à partir d’un PC plus élevée ou de Windows 7. Votre fournisseur doit avoir la documentation pour ce faire.

    * Assurez-vous que le certificat remplit les [configuration requise des certificats AD FS et SSL de Proxy d’Application Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Une fois que vous obtenez la réponse à partir de votre fournisseur de certificats, l’importer dans le magasin de l’ordinateur Local sur chaque serveur AD FS et le Proxy d’Application Web.

1. Sur le **principal** serveur AD FS, utilisez l’applet de commande suivante pour installer le nouveau certificat SSL

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

L’empreinte de certificat sont accessibles par l’exécution de cette commande:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Remarques supplémentaires

* L’applet de commande Set-AdfsSslCertificate est une applet de commande à plusieurs nœuds; Cela signifie qu’il ne doit pas exécuter le principal et tous les nœuds de la batterie de serveurs seront mis à jour. C’est nouveau dans Server 2016. Sur Server 2012 R2, vous deviez exécuter Set-AdfsSslCertificate sur chaque serveur.
* L’applet de commande Set-AdfsSslCertificate doit être exécuté uniquement sur le serveur principal. Le serveur principal doit être en cours d’exécution Server 2016 et le niveau de comportement de la batterie de serveurs doit être déclenché pour 2016.
* L’applet de commande Set-AdfsSslCertificate utilisera la communication à distance PowerShell pour configurer les autres serveurs AD FS, assurez-vous que le port 5985 (TCP) est ouvert sur les autres nœuds.
* L’applet de commande Set-AdfsSslCertificate accorde des autorisations de lecture principal d’adfssrv aux clés privées du certificat SSL. Cette entité de sécurité représente le service AD FS. Il n’est pas nécessaire d’accorder au compte de service AD FS en lecture des accès aux clés privées du certificat SSL.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>Remplacement du certificat SSL pour AD FS en cours d’exécution dans un autre mode de liaison TLS
Lorsque vous configuré dans un autre client le mode de liaison TLS, AD FS effectue l’authentification par certificat de périphérique sur le port 443 et l’authentification des certificats utilisateur sur le port 443, sur un autre nom d’hôte. Le nom d’hôte du certificat utilisateur est la AD FS nom d’hôte ajouté avec «certauth», par exemple «certauth.fs.contoso.com».
Dans ce mode, utilisez l’applet de commande powershell Set-AdfsAlternateTlsClientBinding pour gérer le certificat SSL. Cela va gérer non seulement la liaison TLS autre client mais toutes les autres liaisons sur lequel AD FS définit également le certificat SSL.

Suivez les étapes ci-dessous:

1. Tout d’abord, vous devez obtenir le nouveau certificat. Cela est généralement effectuée par envoi d’une demande de signature de certificat (DSC) à un tiers, le fournisseur de certificats publics. Il existe plusieurs façons pour générer la DSC, y compris à partir d’un PC plus élevée ou de Windows 7. Votre fournisseur doit avoir la documentation pour ce faire.

    * Assurez-vous que le certificat remplit les [configuration requise des certificats AD FS et SSL de Proxy d’Application Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Une fois que vous obtenez la réponse à partir de votre fournisseur de certificats, l’importer dans le magasin de l’ordinateur Local sur chaque serveur AD FS et le Proxy d’Application Web.

1. Sur le **principal** serveur AD FS, utilisez l’applet de commande suivante pour installer le nouveau certificat SSL

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

L’empreinte de certificat sont accessibles par l’exécution de cette commande:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Remarques supplémentaires

* L’applet de commande Set-AdfsAlternateTlsClientBinding est une applet de commande à plusieurs nœuds; Cela signifie qu’il ne doit pas exécuter le principal et tous les nœuds de la batterie de serveurs seront mis à jour.
* L’applet de commande Set-AdfsAlternateTlsClientBinding doit être exécuté uniquement sur le serveur principal. Le serveur principal doit être en cours d’exécution Server 2016 et le niveau de comportement de la batterie de serveurs doit être déclenché pour 2016.
* L’applet de commande Set-AdfsAlternateTlsClientBinding utilisera la communication à distance PowerShell pour configurer les autres serveurs AD FS, assurez-vous que le port 5985 (TCP) est ouvert sur les autres nœuds.
* L’applet de commande Set-AdfsAlternateTlsClientBinding accorde des autorisations de lecture principal d’adfssrv aux clés privées du certificat SSL. Cette entité de sécurité représente le service AD FS. Il n’est pas nécessaire d’accorder au compte de service AD FS en lecture des accès aux clés privées du certificat SSL.

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>Remplacement du certificat SSL pour le Proxy d’Application Web
Pour configurer la liaison de l’authentification de certificat par défaut ou le mode de liaison TLS autre client sur le protocole WAP, nous pouvons utiliser l’applet de commande Set-WebApplicationProxySslCertificate.
Pour remplacer le certificat SSL de Proxy d’Application Web, sur **chaque** serveur Proxy d’Application Web utiliser l’applet de commande suivante pour installer le nouveau certificat SSL:

```powershell
Set-WebApplicationProxySslCertificate '<thumbprint of new cert>'
```

Si l’applet de commande ci-dessus échoue parce que l’ancien certificat a expiré, reconfigurez le serveur proxy à l’aide des applets de commande suivantes:

```powershell
$cred = Get-Credential
```

Entrez les informations d’identification d’un utilisateur de domaine qui est administrateur local sur le serveur AD FS

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>Références supplémentaires  
* [ADFS prend en charge pour la liaison de l’autre nom d’hôte pour l’authentification par certificat](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [ADFS et propriété KeySpec informations de certificat](../technical-reference/AD-FS-and-KeySpec-Property.md)
