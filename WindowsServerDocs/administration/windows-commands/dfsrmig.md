---
title: dfsrmig
description: La rubrique de référence pour la commande dfsrmig, qui migre la réplication SYSvol de FRS vers réplication DFS, fournit des informations sur la progression de la migration et modifie les objets AD DS pour prendre en charge la migration.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a0329bd5829941595f9e1938f68eefb17036c132
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992718"
---
# <a name="dfsrmig"></a>dfsrmig

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

L’outil de migration pour le service réplication DFS, dfsrmig. exe, est installé avec le service réplication DFS. Cet outil migre la réplication SYSvol du service de réplication de fichiers (FRS) vers la réplication système de fichiers DFS (DFS). Il fournit également des informations sur la progression de la migration et modifie les objets Active Directory Domain Services (AD DS) pour prendre en charge la migration.

## <a name="syntax"></a>Syntaxe

```
dfsrmig [/setglobalstate <state> | /getglobalstate | /getmigrationstate | /createglobalobjects |
/deleterontfrsmember [<read_only_domain_controller_name>] | /deleterodfsrmember [<read_only_domain_controller_name>] | /?]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `/setglobalstate <state>` | Définit l’état de migration globale du domaine sur un qui correspond à la valeur spécifiée par *État*. Vous pouvez uniquement définir l’état de la migration globale sur un état stable. Les valeurs d' *État* sont les suivantes :<ul><li>**0** -état de démarrage</li><li>**1** -état préparé</li><li>**2** -État Redirigé</li><li>**3** -État éliminé</li></ul> |
| /getglobalstate | Récupère l’état actuel de la migration globale pour le domaine à partir de la copie locale de la base de données AD DS, lorsqu’elle est exécutée sur l’émulateur de contrôleur de domaine principal. Utilisez cette option pour confirmer que vous définissez l’état de la migration globale approprié.<p>**Important :** Vous ne devez exécuter cette commande que sur l’émulateur de contrôleur de domaine principal. |
| /getmigrationstate | Récupère l’état actuel de la migration locale pour tous les contrôleurs de domaine dans le domaine et détermine si ces États locaux correspondent à l’état de la migration globale actuelle. Utilisez cette option pour déterminer si tous les contrôleurs de domaine ont atteint l’état de la migration globale. |
| /createglobalobjects | Crée les objets et les paramètres globaux dans AD DS utilisé par réplication DFS utilise. Les seules situations dans lesquelles vous devez utiliser cette option pour créer manuellement des objets et des paramètres sont les suivantes :<ul><li>**Un nouveau contrôleur de domaine en lecture seule est promu durant la migration**. Si un nouveau contrôleur de domaine en lecture seule est promu dans le domaine après le passage à l’état **préparé** , mais avant la migration vers l’état **éliminé** , les objets qui correspondent au nouveau contrôleur de domaine ne sont pas créés, provoquant ainsi l’échec de la réplication et de la migration.</li><li>**Les paramètres globaux du service réplication DFS sont manquants ou ont été supprimés**. Si ces paramètres sont manquants pour un contrôleur de domaine, la migration de l’état de **démarrage** à l’état **préparé** est bloquée à l’état de **préparation** de la transition. **Remarque :** Étant donné que les paramètres de AD DS globaux pour le service réplication DFS pour un contrôleur de domaine en lecture seule sont créés sur l’émulateur de contrôleur de domaine principal, ces paramètres doivent être répliqués sur le contrôleur de domaine en lecture seule à partir de l’émulateur de contrôleur de domaine principal avant que le service réplication DFS sur le contrôleur de domaine en lecture seule puisse utiliser ces paramètres. En raison des latences de réplication Active Directory, cette réplication peut prendre un certain temps. |
| `/deleterontfrsmember [<read_only_domain_controller_name>]` | Supprime les paramètres de AD DS globaux pour la réplication FRS qui correspondent au contrôleur de domaine en lecture seule spécifié, ou supprime les paramètres de AD DS globaux pour la réplication FRS pour tous les contrôleurs de domaine `<read_only_domain_controller_name>`en lecture seule si aucune valeur n’est spécifiée pour.<p>Vous ne devez pas utiliser cette option pendant un processus de migration normal, car le service de réplication DFS supprime automatiquement ces AD DS paramètres lors de la migration de l’état **Redirigé** vers l’état **éliminé** . Utilisez cette option pour supprimer manuellement les paramètres de AD DS uniquement lorsque la suppression automatique échoue sur un contrôleur de domaine en lecture seule et bloque le contrôleur de domaine en lecture seule pour un IME long pendant la migration de l’état **Redirigé** vers l’état **éliminé** . |
| `/deleterodfsrmember [<read_only_domain_controller_name>]` | Supprime les paramètres de AD DS globaux pour les réplication DFS qui correspondent au contrôleur de domaine en lecture seule spécifié, ou supprime les paramètres de AD DS globaux pour réplication DFS pour tous les contrôleurs de domaine en lecture seule `<read_only_domain_controller_name>`si aucune valeur n’est spécifiée pour.<p>Utilisez cette option pour supprimer manuellement les paramètres de AD DS uniquement lorsque la suppression automatique échoue sur un contrôleur de domaine en lecture seule et bloque le contrôleur de domaine en lecture seule pendant une période prolongée lors de la restauration de la migration de l’état préparé à l’état de démarrage. |
| /? | Affiche l'aide à l'invite de commandes. |

#### <a name="remarks"></a>Notes 

- Utilisez la `/setglobalstate <state>` commande pour définir l’état de la migration globale dans AD DS sur l’émulateur de contrôleur de domaine principal pour initier et contrôler le processus de migration. Si l’émulateur de contrôleur de domaine principal n’est pas disponible, cette commande échoue.

- La migration vers l’état **éliminé** est irréversible et la restauration n’est pas possible. par conséquent, utilisez la valeur **3** pour l' *État* uniquement lorsque vous êtes entièrement engagé à utiliser réplication DFS pour la réplication SYSvol.

- Les États de migration globaux doivent être un état de migration stable.

- Active Directory réplication réplique l’état global vers d’autres contrôleurs de domaine dans le domaine, mais en raison des latences de réplication, vous pouvez `dfsrmig /getglobalstate` obtenir des incohérences si vous exécutez sur un contrôleur de domaine autre que l’émulateur de contrôleur de domaine principal.

- La sortie de `dsfrmig /getmigrationstate` indique si la migration vers l’état global actuel est terminée, en répertoriant l’état de migration local pour tous les contrôleurs de domaine qui n’ont pas encore atteint l’état de migration globale actuel. L’état de migration locale pour les contrôleurs de domaine peut également inclure des États de transition pour les contrôleurs de domaine qui n’ont pas atteint l’état de migration globale actuel.

- Les contrôleurs de domaine en lecture seule ne peuvent pas supprimer les paramètres de AD DS, l’émulateur de contrôleur de domaine principal effectue cette opération et les modifications finissent par être répliquées sur les contrôleurs de domaine en lecture seule après les latences applicables à la réplication Active Directory.

- La commande **dfsrmig** est prise en charge uniquement sur les contrôleurs de domaine qui s’exécutent au niveau fonctionnel du domaine Windows Server, car la migration SYSVOL de FRS vers réplication DFS est possible uniquement sur les contrôleurs de domaine qui fonctionnent à ce niveau.

- Vous pouvez exécuter la commande **dfsrmig** sur n’importe quel contrôleur de domaine, mais les opérations qui créent ou manipulent des objets AD DS ne sont autorisées que sur les contrôleurs de domaine en lecture-écriture (et non sur les contrôleurs de domaine en lecture seule).

## <a name="examples"></a>Exemples

Pour définir l’état de la migration globale sur préparé (**1**) et lancer la migration, ou pour effectuer une restauration à partir de l’état préparé, tapez :

```
dfsrmig /setglobalstate 1
```

Pour définir l’état de la migration globale sur Démarrer (**0**) et initialiser la restauration à l’état de démarrage, tapez :

```
dfsrmig /setglobalstate 0
```

Pour afficher l’état de la migration globale, tapez :

```
dfsrmig /getglobalstate
```

Sortie de la `dfsrmig /getglobalstate` commande :

```
Current DFSR global state: Prepared
Succeeded.
```

Pour afficher des informations indiquant si les États de migration locale sur tous les contrôleurs de domaine correspondent à l’état de migration globale et s’il existe des États de migration locale où l’état local ne correspond pas à l’état global, tapez :

```
dfsrmig /GetMigrationState
```

Sortie de la `dfsrmig /getmigrationstate` commande lorsque les États de migration locaux sur tous les contrôleurs de domaine correspondent à l’état de la migration globale :

```
All Domain Controllers have migrated successfully to Global state (Prepared).
Migration has reached a consistent state on all Domain Controllers.
Succeeded.
```

Sortie de la `dfsrmig /getmigrationstate` commande lorsque les États de migration locale sur certains contrôleurs de domaine ne correspondent pas à l’état de la migration globale.

```
The following Domain Controllers are not in sync with Global state (Prepared):
Domain Controller (Local Migration State) DC type
=========
CONTOSO-DC2 (start) ReadOnly DC
CONTOSO-DC3 (Preparing) Writable DC
Migration has not yet reached a consistent state on all domain controllers
State information might be stale due to AD latency.
```

Pour créer les objets et paramètres globaux utilisés par réplication DFS dans AD DS sur les contrôleurs de domaine où ces paramètres n’ont pas été créés automatiquement pendant la migration ou où ces paramètres sont manquants, tapez :

```
dfsrmig /createglobalobjects
```

Pour supprimer les paramètres de AD DS globaux pour la réplication FRS pour un contrôleur de domaine en lecture seule nommé Contoso-DC2 si ces paramètres n’ont pas été supprimés automatiquement par le processus de migration, tapez :

```
dfsrmig /deleterontfrsmember contoso-dc2
```

Pour supprimer les paramètres de AD DS globaux pour la réplication FRS pour tous les contrôleurs de domaine en lecture seule si ces paramètres n’ont pas été supprimés automatiquement par le processus de migration, tapez :

```
dfsrmig /deleterontfrsmember
```

Pour supprimer les paramètres de AD DS globaux pour réplication DFS pour un contrôleur de domaine en lecture seule nommé Contoso-DC2 si ces paramètres n’ont pas été supprimés automatiquement par le processus de migration, tapez :

```
dfsrmig /deleterodfsrmember contoso-dc2
```

Pour supprimer les paramètres de AD DS globaux pour réplication DFS pour tous les contrôleurs de domaine en lecture seule si ces paramètres n’ont pas été supprimés automatiquement par le processus de migration, tapez :

```
dfsrmig /deleterodfsrmember
```

Pour afficher l’aide à l’invite de commandes :

```
dfsrmig
```

```
dfsrmig /?
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](https://go.microsoft.com/fwlink/?LinkId=122056)

- [Série de migration SYSvol : partie 2 dfsrmig. exe : l’outil de migration SYSvol](https://techcommunity.microsoft.com/t5/storage-at-microsoft/sysvol-migration-series-part-2-8211-dfsrmig-exe-the-sysvol/ba-p/423470)

- [Active Directory Domain Services](../../identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview.md)
