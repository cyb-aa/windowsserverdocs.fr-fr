---
title: AuditPol resourceSACL
description: Rubrique de commandes de Windows pour **uditpol resourceSACL** -configure les listes de contrôle d’accès système (SACL) ressources globales.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 28771ba7-967a-45e9-9bf0-b2a2673070f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 375f37250404dd6740027cb18959697626c1ffc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837490"
---
# <a name="auditpol-resourcesacl"></a>AuditPol resourceSACL



Configure les listes de contrôle d’accès système (SACL) ressources globales.

> [!NOTE]
> S’applique uniquement à Windows 7 et Windows Server 2008 R2.

Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
auditpol /resourceSACL
[/set /type:<resource> [/success] [/failure] /user:<user> [/access:<access flags>]]
[/remove /type:<resource> /user:<user> [/type:<resource>]]
[/clear [/type:<resource>]]
[/view [/user:<user>] [/type:<resource>]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/set|Ajoute une nouvelle entrée à ou met à jour une entrée existante dans la liste SACL de ressource pour la ressource de type spécifié.|
|/remove|Supprime toutes les entrées pour l’utilisateur donné dans l’accès global aux objets liste d’audit.|
|/clear|Supprime toutes les entrées de l’accès global aux objets liste d’audit.|
|/View|Répertorie les entrées d’audit accès objet global dans une liste SACL de ressource. Les types d’utilisateur et des ressources sont facultatifs.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="arguments"></a>Arguments

|Argument|Description|
|--------|-----------|
|/type|La ressource pour l’objet auquel l’audit d’accès est en cours de configuration. Les valeurs d’argument prises en charge sont les fichier (pour les répertoires et fichiers) et la clé (pour les clés de Registre).</br>Remarque: Les valeurs du fichier et la clé respectent la casse.|
|/Success|Spécifie l’audit des réussites.|
|/Failure|Spécifie l’audit des échecs.|
|/User|Spécifie un utilisateur dans une des formes suivantes :</br>-DomainName\Account (par exemple, DOM\Administrators)</br>-StandaloneServer\Group compte (consultez [fonction LookupAccountName](https://msdn.microsoft.com/library/windows/desktop/aa379159(v=vs.85).aspx))</br>-{S-1-x-x-x-x} (x est exprimé au format décimal, et le SID entière doit être placé entre accolades) ; par exemple : {S-1-5-21-5624481-130208933-164394174-1001}</br>    Remarque:     Si la forme de SID est utilisée, aucune vérification n’est effectuée pour vérifier l’existence de ce compte.|
|/access|Spécifie un masque d’autorisation qui peut être spécifié dans une des deux formes :</br>-Une séquence de droits simples :</br>    -Droits d’accès générique :</br>        -GA - GÉNÉRIQUE TOUS LES</br>        -GÉNÉRIQUE GR - LIRE</br>        -GÉNÉRIQUE GW - ÉCRIRE</br>        -GX - GÉNÉRIQUE EXÉCUTER</br>    -Droits d’accès pour les fichiers :</br>        -FA - FICHIER TOUS LES ACCÈS</br>        -FR - FICHIER GÉNÉRIQUE EN LECTURE</br>        -FW - ÉCRITURE GÉNÉRIQUE</br>        EXÉCUTION - FX - FICHIER GÉNÉRIQUE</br>    -Droits d’accès pour les clés de Registre :</br>        -KA - CLÉ TOUS LES ACCÈS</br>        CLÉ DE - KR - LIRE</br>        ÉCRITURE DE LA CLÉ - KW-</br>        EXÉCUTER - KX - CLÉ</br>    Par exemple : ' / accès : FRFW' Active les événements d’audit pour la lecture et les opérations d’écriture</br>-Valeur hexadécimale qui représente le masque d’accès (tels que 0x1200a9)</br>    Cela est utile lorsque vous utilisez des masques de bits de propres aux ressources qui ne font pas partie de la norme de sécurité descripteur definition language (SDDL). Si omis, un accès complet est utilisé.|

## <a name="remarks"></a>Notes

Pour les opérations de resourceSACL, vous devez disposer d’autorisation d’écriture ou contrôle total sur cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de resourceSACL par possédant le **gérer le journal d’audit et de sécurité** droit d’utilisateur (SeSecurityPrivilege). Toutefois, ce droit permet un accès supplémentaire qui n’est pas nécessaire d’effectuer l’opération de suppression.

## <a name="BKMK_Examples"></a>Exemples

Pour définir une ressource globale SACL pour auditer les accès réussis tente par un utilisateur sur une clé de Registre :
```
auditpol /resourceSACL /set /type:Key /user:MYDOMAIN\myuser /success
```
Pour définir une ressource globale SACL pour auditer les tentatives ayant réussi et ayant échoués par un utilisateur pour générique lire et écrire des fonctions sur des fichiers ou dossiers :
```
auditpol /resourceSACL /set /type:File /user:MYDOMAIN\myuser /success /failure /access:FRFW
```
Pour supprimer toutes les entrées de liste SACL de ressources globales pour les fichiers ou dossiers :
```
auditpol /resourceSACL /type:File /clear
```
Pour supprimer toutes les entrées de liste SACL de ressources globales pour un utilisateur particulier à partir de fichiers ou dossiers :
```
auditpol /resourceSACL /remove /type:File /user:{S-1-5-21-56248481-1302087933-1644394174-1001}
```
Pour répertorier l’accès global aux objets défini sur fichiers ou dossiers des entrées d’audit :
```
auditpol /resourceSACL /type:File /view
```
Pour répertorier l’objet global, accéder aux entrées d’audit pour un utilisateur particulier qui sont définies sur des fichiers ou dossiers :
```
auditpol /resourceSACL /type:File /view /user:MYDOMAIN\myuser
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)