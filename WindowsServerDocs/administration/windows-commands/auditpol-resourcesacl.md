---
title: Auditpol resourceSACL
description: La rubrique commandes Windows pour **Auditpol resourceSACL**, qui configure les listes de contrôle d’accès (SACL) du système de ressources globales.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 28771ba7-967a-45e9-9bf0-b2a2673070f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b004be6f21cd076fe20e73c45268731c35d654e5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851152"
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

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /Set | Ajoute une nouvelle entrée à ou met à jour une entrée existante dans la liste SACL des ressources pour le type de ressource spécifié. |
| /remove | Supprime toutes les entrées pour l’utilisateur spécifié dans la liste d’audit de l’accès global aux objets. |
| /Clear | Supprime toutes les entrées de la liste d’audit de l’accès global aux objets.|
| /View | Répertorie les entrées d’audit de l’accès global aux objets dans une liste SACL de ressources. Les types d’utilisateurs et de ressources sont facultatifs. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="arguments"></a>Arguments

| Argument | Description |
| -------- | ----------- | 
| /type | Ressource pour laquelle l’audit d’accès aux objets est configuré. Les valeurs d’argument prises en charge, qui respectent la casse, sont fichier (pour les répertoires et les fichiers) et clé (pour les clés de registre). |
| /Success | Spécifie l’audit des réussites. |
| /Failure | Spécifie l’audit des échecs. |
| /User | Spécifie un utilisateur sous l’une des formes suivantes :<ul><li> DomainName\Account (par exemple, DOM\Administrators)</li><li>Compte StandaloneServer\Group (voir [fonction LookupAccountName](https://msdn.microsoft.com/library/windows/desktop/aa379159(v=vs.85).aspx))</li><li>{S-1-x-x-x-x} (x est exprimé en décimal et le SID entier doit être placé entre accolades). Par exemple : {S-1-5-21-5624481-130208933-164394174-1001}<p>**Remarque :** Si le formulaire SID est utilisé, aucune vérification n’est effectuée pour vérifier l’existence de ce compte.</li></ul> |
| /access | Spécifie un masque d’autorisation qui peut être spécifié par le biais de :<p>Droits d’accès génériques, y compris :<ul><li>GA-GÉNÉRIQUE TOUT</li><li>GR-LECTURE GÉNÉRIQUE</li><li>GW-ÉCRITURE GÉNÉRIQUE</li><li>GX-EXÉCUTION GÉNÉRIQUE</li></ul><p>Droits d’accès pour les fichiers, notamment :<ul><li>FA-FICHIER TOUT ACCÈS</li><li>FR-FICHIER GÉNÉRIQUE EN LECTURE</li><li>ÉCRITURE GÉNÉRIQUE DU FICHIER FW-FILE</li><li>FX-FICHIER GÉNÉRIQUE D’EXÉCUTION</li></ul><p>Droits d’accès pour les clés de Registre, notamment :<ul><li>CLÉ KA TOUT ACCÈS</li><li>KR-LECTURE DE LA CLÉ</li><li>KW-ÉCRITURE DE CLÉ</li><li>KX-EXÉCUTER LA CLÉ</li></ul><p>Par exemple : `/access:FRFW` active les événements d’audit pour les opérations de lecture et d’écriture.<p>Valeur hexadécimale représentant le masque d’accès (par exemple, ACE 0x1200a9)<p>    Cela est utile lorsque vous utilisez des masques binaires spécifiques aux ressources qui ne font pas partie de la norme SDDL (Security Descriptor Definition Language). En cas d’omission, l’accès complet est utilisé. |

## <a name="remarks"></a>Notes

Pour les opérations resourceSACL, vous devez disposer de l’autorisation d’écriture ou de contrôle total sur cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations resourceSACL en possédant le droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer l’opération de suppression.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

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

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)