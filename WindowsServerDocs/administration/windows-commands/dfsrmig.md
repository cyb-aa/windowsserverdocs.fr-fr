---
title: dfsrmig
description: Rubrique de référence pour dfsrmig, qui migre la réplication SYSvol à partir du service de réplication de fichiers (FRS) vers la réplication système de fichiers DFS (DFS), fournit des informations sur la progression de la migration et modifie les objets des services de domaine Active Directory (AD DS) pour prendre en charge la migration.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a5cac54a1ad96994059f3131b81702136b26aad
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719535"
---
# <a name="dfsrmig"></a>dfsrmig

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Migre la réplication SYSvol du service de réplication de fichiers (FRS) vers la réplication système de fichiers DFS (DFS), fournit des informations sur la progression de la migration et modifie les objets des services de domaine Active Directory (AD DS) pour prendre en charge la migration.



## <a name="syntax"></a>Syntaxe

```
dfsrmig [/SetGlobalState <state> | /GetGlobalState | /GetMigrationState | /createGlobalObjects | 
/deleteRoNtfrsMember [<read_only_domain_controller_name>] | /deleteRoDfsrMember [<read_only_domain_controller_name>] | /?]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description | | -----_ | ------ | | /SetGlobalState <state> | Définit l’état de la migration globale souhaitée pour le domaine à l’État qui correspond à la valeur spécifiée par *État*.<p>Pour effectuer les processus de migration ou de restauration, utilisez cette commande pour parcourir les États valides. Cette option vous permet de lancer et de contrôler le processus de migration en définissant l’état de la migration globale dans AD DS sur l’émulateur de contrôleur de domaine principal. Si l’émulateur de contrôleur de domaine principal n’est pas disponible, cette commande échoue.<p>Vous pouvez uniquement définir l’état de la migration globale sur un état stable. Les valeurs valides *pour l’État,* par conséquent, sont **0** pour l’état de démarrage, **1** pour l’état préparé, **2** pour l’État Redirigé et **3** pour l’État éliminé.<p>La migration vers l’État éliminé est irréversible et la restauration à partir de cet État n’est pas possible. Utilisez donc la valeur **3** pour l' *État* uniquement lorsque vous êtes entièrement validé pour utiliser réplication DFS pour la réplication SYSvol. | | /GetGlobalState | Récupère l’état actuel de la migration globale pour le domaine à partir de la copie locale de la base de données AD DS, lorsqu’elle est exécutée sur l’émulateur de contrôleur de domaine principal.<p>Utilisez cette option pour confirmer que vous définissez l’état de la migration globale approprié. Seuls les États de migration stables peuvent être des États de migration globaux. par conséquent, les résultats que la commande **dfsrmig** signale avec l’option **/GetGlobalState** correspondent aux États que vous pouvez définir avec l’option **/SetGlobalState** .<p>Vous devez exécuter la commande **dfsrmig** avec l’option **/GetGlobalState** uniquement sur l’émulateur de contrôleur de domaine principal. la réplication Active Directory réplique l’état global vers les autres contrôleurs de domaine du domaine, mais les latences de réplication peuvent entraîner des incohérences si vous exécutez la commande **dfsrmig** avec l’option **/GetGlobalState** sur un contrôleur de domaine autre que l’émulateur PDC. Pour vérifier l’état de migration locale d’un contrôleur de domaine autre que l’émulateur de contrôleur de domaine principal, utilisez l’option **/GetMigrationState** à la place. | | /GetMigrationState | Récupère l’état actuel de la migration locale pour tous les contrôleurs de domaine dans le domaine et détermine si ces États locaux correspondent à l’état de la migration globale actuelle.<p>Utilisez cette option pour déterminer si tous les contrôleurs de domaine ont atteint l’état de la migration globale. La sortie de la commande **dsfrmig** quand vous utilisez l’option **/GetMigrationState** indique si la migration vers l’état global actuel est terminée, et elle répertorie l’état de migration local pour tous les contrôleurs de domaine qui n’ont pas atteint l’état de migration globale actuel. L’état de la migration locale pour les contrôleurs de domaine peut inclure des États de transition pour les contrôleurs de domaine qui n’ont pas atteint l’état de migration globale actuel. | | /createGlobalObjects | Crée les objets et paramètres globaux dans AD DS que réplication DFS utilise.<p>Vous n’avez pas besoin d’utiliser cette option lors d’un processus de migration normal, car le service réplication DFS crée automatiquement ces AD DS objets et paramètres lors de la migration de l’état de démarrage à l’état préparé. Utilisez cette option pour créer manuellement ces objets et paramètres dans les cas suivants :<p>-  **Un nouveau contrôleur de domaine en lecture seule est promu durant la migration**. Le service réplication DFS crée automatiquement les objets et les paramètres AD DS pour réplication DFS pendant la migration de l’état de démarrage à l’état préparé. Si un nouveau contrôleur de domaine en lecture seule est promu dans le domaine après cette transition, mais avant la migration vers l’État éliminé, les objets qui correspondent au contrôleur de domaine en lecture seule qui vient d’être activé ne sont pas créés dans AD DS provoquant l’échec de la réplication et de la migration.<br />-Dans ce cas, vous pouvez exécuter la commande **dfsrmig** wth l’option **/createGlobalObjects** pour créer manuellement les objets sur tous les contrôleurs de domaine en lecture seule qui ne les ont pas déjà. L’exécution de cette commande n’affecte pas les contrôleurs de domaine qui ont déjà les objets et les paramètres pour le service réplication DFS.<p>- **Les paramètres globaux du service réplication DFS sont manquants ou ont été supprimés**. Si ces paramètres sont absents pour un contrôleur de domaine particulier, la migration de l’état de démarrage à l’état préparé se bloque à l’état de transition de préparation pour le contrôleur de domaine. Dans ce cas, vous pouvez utiliser la commande **dfsrmig** avec l’option **/createGlobalObjects** pour créer manuellement les paramètres. **Remarque :** Étant donné que les paramètres de AD DS globaux pour le service réplication DFS pour un contrôleur de domaine en lecture seule sont créés sur l’émulateur de contrôleur de domaine principal, ces paramètres doivent être répliqués sur le contrôleur de domaine en lecture seule à partir de l’émulateur de contrôleur de domaine principal avant que le service réplication DFS sur le contrôleur de domaine en lecture seule puisse utiliser ces paramètres. En raison des latences de réplication Active IRE, cette réplication peut prendre un certain temps. | | /deleteRoNtfrsMember [<read_only_domain_controller_name>] | Supprime les paramètres de AD DS globaux pour la réplication FRS qui correspondent au contrôleur de domaine en lecture seule spécifié, ou supprime les paramètres de AD DS globaux pour la réplication FRS pour tous les contrôleurs de domaine en lecture seule si aucune valeur n’est spécifiée pour *read_only_domain_controller_name*.<p>Vous n’avez pas besoin d’utiliser cette option pendant un processus de migration normal, car le service de réplication DFS supprime automatiquement ces AD DS paramètres lors de la migration de l’État redirigé vers l’État éliminé. Étant donné que les contrôleurs de domaine en lecture seule ne peuvent pas supprimer ces paramètres de AD DS, l’émulateur de contrôleur de domaine principal effectue cette opération et les modifications finissent par être répliquées sur les contrôleurs de domaine en lecture seule après les latences applicables à la réplication Active Directory.<p>Utilisez cette option pour supprimer manuellement les paramètres de AD DS uniquement lorsque la suppression automatique échoue sur un contrôleur de domaine en lecture seule et bloque le contrôleur de domaine en lecture seule pour un IME long pendant la migration de l’État redirigé vers l’État éliminé. | | /deleteRoDfsrMember [<read_only_domain_controller_name>] | Supprime les paramètres de AD DS globaux pour les réplication DFS qui correspondent au contrôleur de domaine en lecture seule spécifié, ou supprime les paramètres de AD DS globaux pour réplication DFS pour tous les contrôleurs de domaine en lecture seule si aucune valeur n’est spécifiée pour *read_only_domain_controller_name*.<p>Utilisez cette option pour supprimer manuellement les paramètres de AD DS uniquement lorsque la suppression automatique échoue sur un contrôleur de domaine en lecture seule et bloque le contrôleur de domaine en lecture seule pendant une période prolongée lors de la restauration de la migration de l’état préparé à l’état de démarrage. | | /? | Affiche l’aide à l’invite de commandes. Équivalent à l’exécution d' **dfsrmig** sans aucune option. |

## <a name="remarks"></a>Notes 
- dfsrmig. exe, l’outil de migration pour le service réplication DFS, est installé avec le service réplication DFS.
 pour un nouveau serveur Windows Server 2008, Dcpromo. exe s’installe et démarre le service réplication DFS lorsque vous promouvez l’ordinateur sur un contrôleur de domaine. Lorsque vous mettez à niveau un serveur de Windows Server 2003 vers Windows Server 2008, le processus de mise à niveau installe et démarre le service réplication DFS. Vous n’avez pas besoin d’installer le service de rôle réplication DFS pour installer et démarrer le service réplication DFS.
- L’outil **dfsrmig** est pris en charge uniquement sur les contrôleurs de domaine qui s’exécutent au niveau fonctionnel du domaine windows Server 2008, car la migration SYSVOL de FRS vers réplication DFS est possible uniquement sur les contrôleurs de domaine qui fonctionnent au niveau fonctionnel du domaine windows Server 2008.
- Vous pouvez exécuter la commande **dfsrmig** sur n’importe quel contrôleur de domaine, mais les opérations qui créent ou manipulent des objets AD DS ne sont autorisées que sur les contrôleurs de domaine en lecture-écriture (et non sur les contrôleurs de domaine en lecture seule).
- L’exécution d' **dfsrmig** sans aucune option affiche l’aide à l’invite de commandes.

## <a name="examples"></a>Exemples
Pour définir l’état de la migration globale sur préparé (**1**) et lancer la migration vers ou la restauration à partir de l’état préparé, tapez :
 ```
 dfsrmig /SetGlobalState 1
 ```
 Pour définir l’état de la migration globale sur Start (**0**) et lancer la restauration à l’État Start, tapez :
 ```
 dfsrmig /SetGlobalState 0
 ```
 Pour afficher l’état de la migration globale, tapez :
 ```
 dfsrmig /GetGlobalState
 ```
 Cet exemple illustre la sortie standard de la commande **dfsrmig/GetGlobalState** .
 ```
 Current DFSR global state: Prepared 
 Succeeded.
 ```
 Pour afficher les informations indiquant si les États de migration locale sur tous les contrôleurs de domaine correspondent à l’état de migration globale et aux États de migration locale pour tous les contrôleurs de domaine dont l’état local ne correspond pas à l’état global, tapez :
 ```
 dfsrmig /GetMigrationState
 ```
 Cet exemple illustre la sortie standard de la commande **dfsrmig/GetMigrationState** lorsque les États de migration locaux sur tous les contrôleurs de domaine correspondent à l’état de migration globale.
 ```
 All Domain Controllers have migrated successfully to Global state ( Prepared ).
 Migration has reached a consistent state on all Domain Controllers.
 Succeeded.
 ```
 Cet exemple illustre la sortie standard de la commande **dfsrmig/GetMigrationState** lorsque les États de migration locaux de certains contrôleurs de domaine ne correspondent pas à l’état de la migration globale.
 ```
  The following Domain Controllers are not in sync with Global state ( Prepared ):
  Domain Controller (Local Migration State)  DC type
  =========
  CONTOSO-DC2 ( start )  ReadOnly DC
  CONTOSO-DC3 ( Preparing )  Writable DC
  Migration has not yet reached a consistent state on all domain controllers
  State information might be stale due to AD latency.
 ```
Pour créer les objets et paramètres globaux utilisés par réplication DFS dans AD DS sur les contrôleurs de domaine où ces paramètres n’ont pas été créés automatiquement pendant la migration ou où ces paramètres sont manquants, tapez :
```
dfsrmig /createGlobalObjects
```
Pour supprimer les paramètres de AD DS globaux pour la réplication FRS pour un contrôleur de domaine en lecture seule nommé Contoso-DC2 si ces paramètres n’ont pas été supprimés automatiquement par le processus de migration, tapez :
```
dfsrmig /deleteRoNtfrsMember contoso-dc2
```
Pour supprimer les paramètres de AD DS globaux pour la réplication FRS pour tous les contrôleurs de domaine en lecture seule si ces paramètres n’ont pas été supprimés automatiquement par le processus de migration, tapez :
```
dfsrmig /deleteRoNtfrsMember
```
Pour supprimer les paramètres de AD DS globaux pour réplication DFS pour un contrôleur de domaine en lecture seule nommé Contoso-DC2 si ces paramètres n’ont pas été supprimés automatiquement par le processus de migration, tapez :
```
dfsrmig /deleteRoDfsrMember contoso-dc2
```
Pour supprimer les paramètres de AD DS globaux pour réplication DFS pour tous les contrôleurs de domaine en lecture seule si ces paramètres n’ont pas été supprimés automatiquement par le processus de migration, tapez :
```
dfsrmig /deleteRoDfsrMember
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](https://go.microsoft.com/fwlink/?LinkId=122056)

- [Série de migration SYSvol : partie 2 dfsrmig. exe : l’outil de migration SYSvol](https://go.microsoft.com/fwlink/?LinkID=121757)
