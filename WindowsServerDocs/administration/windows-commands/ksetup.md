---
title: ksetup
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0194cf81d069d7a5c1223f0a514d593e4870d397
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868840"
---
# <a name="ksetup"></a>ksetup



Exécute les tâches liées à la configuration et gestion du protocole Kerberos et le centre de Distribution de clés (KDC) pour prendre en charge les domaines Kerberos, qui ne sont pas également les domaines Windows. Pour obtenir des exemples d’utilisation de cette commande, consultez la section exemples dans chacune des sous-sections associées.

## <a name="syntax"></a>Syntaxe

```
ksetup 
[/setrealm <DNSDomainName>] 
[/mapuser <Principal> <Account>] 
[/addkdc <RealmName> <KDCName>] 
[/delkdc <RealmName> <KDCName>]
[/addkpasswd <RealmName> <KDCPasswordName>] 
[/delkpasswd <RealmName> <KDCPasswordName>]
[/server <ServerName>] 
[/setcomputerpassword <Password>]
[/removerealm <RealmName>]  
[/domain <DomainName>] 
[/changepassword <OldPassword> <NewPassword>] 
[/listrealmflags] 
[/setrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/delrealmflags [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/dumpstate]
[/addhosttorealmmap] <HostName> <RealmName>]  
[/delhosttorealmmap] <HostName> <RealmName>]  
[/setenctypeattr] <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/getenctypeattr] <DomainName>
[/addenctypeattr] <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/delenctypeattr] <DomainName>

```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[Ksetup:setrealm](ksetup-setrealm.md)|Rend cet ordinateur un membre d’un domaine Kerberos.|
|[Ksetup:mapuser](ksetup-mapuser.md)|Mappe un principal Kerberos à un compte.|
|[Ksetup:addkdc](ksetup-addkdc.md)|Définit une entrée de contrôleur de domaine Kerberos pour le domaine donné.|
|[Ksetup:delkdc](ksetup-delkdc.md)|Supprime une entrée de contrôleur de domaine Kerberos pour le domaine.|
|[Ksetup:addkpasswd](ksetup-addkpasswd.md)|Ajoute une adresse de serveur Kpasswd pour un domaine.|
|[Ksetup:delkpasswd](ksetup-delkpasswd.md)|Supprime une adresse de serveur Kpasswd pour un domaine.|
|[Ksetup:server](ksetup-server.md)|Vous permet de spécifier le nom d’un ordinateur Windows sur lequel appliquer les modifications.|
|[Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)|Définit le mot de passe pour le domaine de l’ordinateur compte (ou principal de l’hôte).|
|[Ksetup:removerealm](ksetup-removerealm.md)|Supprime toutes les informations pour le domaine Kerberos spécifié à partir du Registre.|
|[Ksetup:domain](ksetup-domain.md)|Vous permet de spécifier un domaine (si \<DomainName > n’a pas été définie à l’aide de **/domain**).|
|[Ksetup:changepassword](ksetup-changepassword.md)|Vous permet d’utiliser le Kpasswd pour modifier le mot de passe de l’utilisateur connecté.|
|[Ksetup:listrealmflags](ksetup-listrealmflags.md)|Répertorie les indicateurs de domaine qui **ksetup** peut détecter.|
|[Ksetup:setrealmflags](ksetup-setrealmflags.md)|Définit les indicateurs de domaine pour un domaine spécifique.|
|[Ksetup:addrealmflags](ksetup-addrealmflags.md)|Ajoute des indicateurs de domaine supplémentaires à un domaine.|
|[Ksetup:delrealmflags](ksetup-delrealmflags.md)|Supprime les indicateurs de domaine à partir d’un domaine.|
|[Ksetup:dumpstate](ksetup-dumpstate.md)|Analyse la configuration de Kerberos sur l’ordinateur spécifié. Ajoute un ordinateur hôte au mappage de domaine dans le Registre.|
|[Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)|Ajoute une valeur de Registre pour mapper l’hôte au domaine Kerberos.|
|[Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)|Supprime la valeur de Registre qui mappée de l’ordinateur hôte au domaine Kerberos.|
|[Ksetup:setenctypeattr](ksetup-setenctypeattr.md)|Définit un ou plusieurs types de chiffrement des attributs de niveau de confiance pour le domaine.|
|[Ksetup:getenctypeattr](ksetup-getenctypeattr.md)|Obtient l’attribut de niveau de confiance de types de chiffrement pour le domaine.|
|[Ksetup:addenctypeattr](ksetup-addenctypeattr.md)|Ajoute des types de chiffrement à l’attribut de confiance des types de chiffrement pour le domaine.|
|[Ksetup:delenctypeattr](ksetup-delenctypeattr.md)|Supprime l’attribut de confiance des types de chiffrement pour le domaine.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

**Ksetup** est utilisée pour modifier les paramètres de l’ordinateur pour localiser les domaines Kerberos. Dans les implémentations de Microsoft Kerberos non-Windows, ces informations sont généralement conservées dans le fichier Krb5.conf. Dans les systèmes d’exploitation Windows Server, il est conservé dans le Registre. Vous pouvez utiliser cet outil pour modifier ces paramètres. Ces paramètres sont utilisés par les stations de travail pour localiser les domaines Kerberos et par les contrôleurs de domaine pour localiser les domaines Kerberos pour les relations d’approbation entre domaines.

**Ksetup** initialise les clés de Registre que le fournisseur SSP (Security Support Provider) Kerberos utilise pour rechercher un contrôleur de domaine Kerberos pour le domaine Kerberos si l’ordinateur exécute Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 et n’est pas un membre d’un Windows domaine. Après la configuration, comptes d’utilisateur d’un ordinateur client qui exécute le système d’exploitation peut ouvrir une session de Windows dans le domaine Kerberos.

Le protocole Kerberos version 5 est la valeur par défaut pour l’authentification réseau sur les ordinateurs exécutant Windows XP Professionnel, Windows Vista et Windows 7. Le SSP Kerberos recherche dans le Registre pour le nom de domaine du domaine de l’utilisateur et résout le nom en une adresse IP en interrogeant un serveur DNS. Le protocole Kerberos peut utiliser DNS pour localiser le KDC en utilisant uniquement le nom de domaine, mais il doit être configuré spécialement pour ce faire.

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)