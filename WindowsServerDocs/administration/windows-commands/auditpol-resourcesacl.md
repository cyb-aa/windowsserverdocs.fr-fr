---
title: Auditpol resourceSACL
description: La rubrique commandes Windows pour **Uditpol resourceSACL** -configure les listes de contrôle d’accès (SACL) du système de ressources globales.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2acffd75298f0f36a9c15e0622816feaae57cb64
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382426"
---
# <a name="auditpol-resourcesacl"></a>Auditpol resourceSACL



Configure les listes de contrôle d’accès (SACL) du système de ressources globales.

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
|/Set|Ajoute une nouvelle entrée à ou met à jour une entrée existante dans la liste SACL des ressources pour le type de ressource spécifié.|
|/remove|Supprime toutes les entrées pour l’utilisateur spécifié dans la liste d’audit de l’accès global aux objets.|
|/Clear|Supprime toutes les entrées de la liste d’audit de l’accès global aux objets.|
|/View|Répertorie les entrées d’audit de l’accès global aux objets dans une liste SACL de ressources. Les types d’utilisateurs et de ressources sont facultatifs.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="arguments"></a>Arguments

|Argument|Description|
|--------|-----------|
|/type|Ressource pour laquelle l’audit d’accès aux objets est configuré. Les valeurs d’argument prises en charge sont file (pour les répertoires et les fichiers) et Key (pour les clés de registre).</br>Remarque : Les valeurs de fichier et de clé respectent la casse.|
|/Success|Spécifie l’audit des réussites.|
|/Failure|Spécifie l’audit des échecs.|
|/User|Spécifie un utilisateur sous l’une des formes suivantes :</br>-DomainName\Account (par exemple, DOM\Administrators)</br>-Compte StandaloneServer\Group (voir [fonction LookupAccountName](https://msdn.microsoft.com/library/windows/desktop/aa379159(v=vs.85).aspx))</br>-{S-1-x-x-x-x} (x est exprimé en décimal et le SID entier doit être placé entre accolades); par exemple : {S-1-5-21-5624481-130208933-164394174-1001}</br>    Remarque :     Si le formulaire SID est utilisé, aucune vérification n’est effectuée pour vérifier l’existence de ce compte.|
|/access|Spécifie un masque d’autorisation qui peut être spécifié sous l’une des deux formes suivantes :</br>-Une séquence de droits simples :</br>    -Droits d’accès génériques :</br>        -GA-GENERIC ALL</br>        -GR-LECTURE GÉNÉRIQUE</br>        -GW-ÉCRITURE GÉNÉRIQUE</br>        -GX-EXÉCUTION GÉNÉRIQUE</br>    -Droits d’accès pour les fichiers :</br>        -FA-FICHIER TOUS LES ACCÈS</br>        -FR-FICHIER GÉNÉRIQUE EN LECTURE</br>        -FW-FICHIER ÉCRITURE GÉNÉRIQUE</br>        -FX-FICHIER GÉNÉRIQUE D’EXÉCUTION</br>    -Droits d’accès pour les clés de Registre :</br>        -KA-CLÉ TOUS LES ACCÈS</br>        -KR-LECTURE DE LA CLÉ</br>        -KW-ÉCRITURE DE CLÉ</br>        -KX-CLÉ D’EXÉCUTION</br>    Par exemple : « /Access : FRFW » active les événements d’audit pour les opérations de lecture et d’écriture</br>-Valeur hexadécimale représentant le masque d’accès (par exemple, ACE 0x1200a9)</br>    Cela est utile lorsque vous utilisez des masques binaires spécifiques aux ressources qui ne font pas partie de la norme SDDL (Security Descriptor Definition Language). En cas d’omission, l’accès complet est utilisé.|

## <a name="remarks"></a>Notes

Pour les opérations resourceSACL, vous devez disposer de l’autorisation d’écriture ou de contrôle total sur cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations resourceSACL en possédant le droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer l’opération de suppression.

## <a name="BKMK_Examples"></a>Illustre

Pour définir une liste SACL des ressources globales pour auditer les tentatives d’accès réussies par un utilisateur sur une clé de Registre :
```
auditpol /resourceSACL /set /type:Key /user:MYDOMAIN\myuser /success
```
Pour définir une liste SACL des ressources globales pour l’audit des tentatives réussies et échouées par un utilisateur pour effectuer des fonctions de lecture et d’écriture génériques sur des fichiers ou des dossiers :
```
auditpol /resourceSACL /set /type:File /user:MYDOMAIN\myuser /success /failure /access:FRFW
```
Pour supprimer toutes les entrées SACL des ressources globales pour les fichiers ou les dossiers :
```
auditpol /resourceSACL /type:File /clear
```
Pour supprimer toutes les entrées SACL de ressources globales pour un utilisateur particulier à partir de fichiers ou de dossiers :
```
auditpol /resourceSACL /remove /type:File /user:{S-1-5-21-56248481-1302087933-1644394174-1001}
```
Pour répertorier les entrées d’audit de l’accès global aux objets définies sur des fichiers ou des dossiers :
```
auditpol /resourceSACL /type:File /view
```
Pour répertorier les entrées d’audit de l’accès global aux objets pour un utilisateur particulier qui sont définies sur des fichiers ou des dossiers :
```
auditpol /resourceSACL /type:File /view /user:MYDOMAIN\myuser
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)