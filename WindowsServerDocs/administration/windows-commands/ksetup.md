---
title: ksetup
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f3fde0ada4ab8bcbe52eccf22b959f99f91319f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724542"
---
# <a name="ksetup"></a>ksetup



Effectue les tâches relatives à la configuration et à la maintenance du protocole Kerberos et du centre de distribution de clés (KDC) pour prendre en charge les domaines Kerberos, qui ne sont pas également des domaines Windows. Pour obtenir des exemples d’utilisation de cette commande, consultez la section exemples dans chacune des sous-rubriques connexes.

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

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[Ksetup:setrealm](ksetup-setrealm.md)|Fait de cet ordinateur un membre d’un domaine Kerberos.|
|[Ksetup:mapuser](ksetup-mapuser.md)|Mappe un principal Kerberos à un compte.|
|[Ksetup:addkdc](ksetup-addkdc.md)|Définit une entrée KDC pour le domaine donné.|
|[Ksetup:delkdc](ksetup-delkdc.md)|Supprime une entrée du KDC pour le domaine.|
|[Ksetup:addkpasswd](ksetup-addkpasswd.md)|Ajoute une adresse de serveur kpasswd pour un domaine.|
|[Ksetup:delkpasswd](ksetup-delkpasswd.md)|Supprime une adresse de serveur kpasswd pour un domaine.|
|[Ksetup:server](ksetup-server.md)|Vous permet de spécifier le nom d’un ordinateur Windows sur lequel appliquer les modifications.|
|[Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)|Définit le mot de passe du compte de domaine de l’ordinateur (ou du principal de l’hôte).|
|[Ksetup:removerealm](ksetup-removerealm.md)|Supprime du registre toutes les informations pour le domaine spécifié.|
|[Ksetup:domain](ksetup-domain.md)|Vous permet de spécifier un domaine (si \<DomainName> n’a pas été défini à l’aide de **/Domain**).|
|[Ksetup:changepassword](ksetup-changepassword.md)|Vous permet d’utiliser le kpasswd pour modifier le mot de passe de l’utilisateur connecté.|
|[Ksetup:listrealmflags](ksetup-listrealmflags.md)|Répertorie les indicateurs de domaine disponibles que **Ksetup** peut détecter.|
|[Ksetup:setrealmflags](ksetup-setrealmflags.md)|Définit des indicateurs de domaine pour un domaine spécifique.|
|[Ksetup:addrealmflags](ksetup-addrealmflags.md)|Ajoute des indicateurs de domaine supplémentaires à un domaine.|
|[Ksetup:delrealmflags](ksetup-delrealmflags.md)|Supprime les indicateurs de domaine d’un domaine.|
|[Ksetup:dumpstate](ksetup-dumpstate.md)|Analyse la configuration Kerberos sur l’ordinateur donné. Ajoute un hôte au mappage de domaine dans le registre.|
|[Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)|Ajoute une valeur de Registre pour mapper l’hôte au domaine Kerberos.|
|[Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)|Supprime la valeur de Registre qui a mappé l’ordinateur hôte au domaine Kerberos.|
|[Ksetup:setenctypeattr](ksetup-setenctypeattr.md)|Définit un ou plusieurs types de chiffrement des attributs d’approbation pour le domaine.|
|[Ksetup:getenctypeattr](ksetup-getenctypeattr.md)|Obtient l’attribut d’approbation des types de chiffrement pour le domaine.|
|[Ksetup:addenctypeattr](ksetup-addenctypeattr.md)|Ajoute des types de chiffrement à l’attribut d’approbation des types de chiffrement pour le domaine.|
|[Ksetup:delenctypeattr](ksetup-delenctypeattr.md)|Supprime l’attribut d’approbation des types de chiffrement pour le domaine.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

**Ksetup** est utilisé pour modifier les paramètres de l’ordinateur afin de localiser les domaines Kerberos. Dans les implémentations basées sur Kerberos non-Microsoft, ces informations sont généralement conservées dans le fichier krb5. conf. Dans les systèmes d’exploitation Windows Server, elle est conservée dans le registre. Vous pouvez utiliser cet outil pour modifier ces paramètres. Ces paramètres sont utilisés par les stations de travail pour localiser les domaines Kerberos et par les contrôleurs de domaine afin de localiser les domaines Kerberos pour les relations d’approbation entre domaines.

**Ksetup** initialise les clés de registre que le fournisseur SSP (Security Support Provider) Kerberos utilise pour localiser un KDC pour le domaine Kerberos si l’ordinateur exécute windows Server 2003, windows Server 2008 ou windows Server 2008 R2 et n’est pas membre d’un domaine Windows. Après la configuration, l’utilisateur d’un ordinateur client qui exécute le système d’exploitation Windows peut se connecter aux comptes dans le domaine Kerberos.

Le protocole Kerberos version 5 est la valeur par défaut pour l’authentification réseau sur les ordinateurs exécutant Windows XP professionnel, Windows Vista et Windows 7. Le SSP Kerberos recherche le nom de domaine du domaine de l’utilisateur dans le registre, puis résout le nom en adresse IP en interrogeant un serveur DNS. Le protocole Kerberos peut utiliser le DNS pour localiser les KDC en utilisant uniquement le nom de domaine, mais il doit être spécialement configuré pour ce faire.

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)