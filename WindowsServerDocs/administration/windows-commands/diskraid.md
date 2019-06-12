---
title: diskraid
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20aef1e5-7641-47cf-b4eb-cda117f65b6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ebd65fb56114bff9e6ae4b6a76376561c686dfa
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439566"
---
# <a name="diskraid"></a>diskraid



DiskRAID est un outil de ligne de commande qui vous permet de configurer et gérer une baie redondante de sous-systèmes de stockage de disques indépendants (ou peu coûteux) (RAID).

RAID est une méthode utilisée pour normaliser et classer les systèmes de disque à tolérance de pannes. Niveaux RAID offrent différentes combinaisons de performances, de fiabilité et de coût. RAID est généralement utilisé sur les serveurs. Certains serveurs fournissent trois niveaux RAID : Niveau 0 (entrelacement), niveau 1 (mise en miroir) et 5 (agrégation par bandes avec parité).

Un sous-système RAID matériel distingue les unités de stockage adressable physiquement eux à l’aide d’un numéro d’unité logique (LUN). Un objet de numéro d’unité logique doit avoir au moins un bre et peut avoir un nombre quelconque de plex supplémentaires. Chaque bre contient une copie des données sur l’objet de numéro d’unité logique. Plex peut être ajoutés à et supprimées à partir d’un objet de numéro d’unité logique.

La plupart des commandes DiskRAID fonctionnent sur un port de carte (HBA) de bus hôte spécifique, adaptateur de l’initiateur, portail de l’initiateur, fournisseur, sous-système, contrôleur, port, lecteur, numéro d’unité logique, un portail cible, cible ou groupe de portail cible. La commande SELECT vous permet de sélectionner un objet. L’objet sélectionné est dite avoir le focus. Le focus simplifie les tâches de configuration courantes, telles que la création de plusieurs numéros d’unités logiques dans le même sous-système.

> [!NOTE]
> L’outil de ligne de commande DiskRAID fonctionne uniquement avec des sous-systèmes de stockage qui prennent en charge le Service de disque virtuel (VDS).

## <a name="diskraid-commands"></a>Commandes DiskRAID

Pour afficher la syntaxe de commande, cliquez sur une commande :
-   [add](#BKMK_1)
-   [associate](#BKMK_2)
-   [automagic](#BKMK_3)
-   [break](#BKMK_4)
-   [chap](#BKMK_5)
-   [create](#BKMK_6)
-   [delete](#BKMK_7)
-   [detail](#BKMK_8)
-   [dissociate](#BKMK_9)
-   [exit](#BKMK_10)
-   [extend](#BKMK_11)
-   [flushcache](#BKMK_12)
-   [help](#BKMK_13)
-   [importtarget](#BKMK_14)
-   [initiator](#BKMK_15)
-   [invalidatecache](#BKMK_16)
-   [lbpolicy](#BKMK_18)
-   [list](#BKMK_19)
-   [login](#BKMK_20)
-   [logout](#BKMK_21)
-   [maintenance](#BKMK_22)
-   [name](#BKMK_23)
-   [offline](#BKMK_24)
-   [online](#BKMK_25)
-   [recover](#BKMK_26)
-   [reenumerate](#BKMK_27)
-   [refresh](#BKMK_28)
-   [rem](#BKMK_29)
-   [remove](#BKMK_30)
-   [replace](#BKMK_31)
-   [reset](#BKMK_32)
-   [select](#BKMK_33)
-   [setflag](#BKMK_34)
-   [shrink](#BKMK_shrink)
-   [standby](#BKMK_35)
-   [unmask](#BKMK_36)

### <a name="BKMK_1"></a>add

Ajoute un numéro d’unité logique existante à l’unité logique actuellement sélectionné, ou ajoute un portail de cible iSCSI pour le groupe de portail cible iSCSI actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

#### <a name="parameters"></a>Paramètres

**numéro d’unité logique bre**=*n*

Spécifie le numéro LUN à ajouter en tant qu’un bre à l’unité logique actuellement sélectionné.

> [!CAUTION]
> Toutes les données sur le LUN ajouté comme un bre seront supprimées.

**tpgroup tportal=** <em>n</em>

Spécifie le nombre de portail cible iSCSI à ajouter au groupe de portail cible iSCSI actuellement sélectionné.

**noerr**

Spécifie que les échecs qui se produisent lors de l’exécution de cette opération seront ignorées. Cela est utile en mode script.

### <a name="BKMK_2"></a>associate

Définit la liste spécifiée de contrôleur de ports comme actif pour le numéro d’unité logique sélectionné (autre contrôleur ports peuvent être désactivées), ou ajoute les ports du contrôleur spécifié à la liste des ports de contrôleur actif existant pour le numéro d’unité logique actuellement sélectionné ou associe le cible iSCSI spécifiés pour le numéro d’unité logique actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

#### <a name="parameters"></a>Paramètres

**Contrôleurs**

Pour une utilisation avec des fournisseurs VDS 1.0 uniquement. Ajoute ou remplace la liste des contrôleurs qui sont associés à des LUN actuellement sélectionné.

**ports**

Pour une utilisation avec uniquement les fournisseurs VDS 1.1. Ajoute ou remplace la liste des ports des contrôleurs qui sont associés à des LUN actuellement sélectionné.

**targets**

Pour une utilisation avec uniquement les fournisseurs VDS 1.1. Ajoute ou remplace la liste de cibles iSCSI qui sont associés à des LUN actuellement sélectionné.

**add**

Pour les fournisseurs VDS 1.0, ajoute les contrôleurs spécifiées à la liste de contrôleurs associés à l’unité logique existante. Si ce paramètre n’est pas spécifié, la liste des contrôleurs remplace la liste de contrôleurs associée à ce numéro d’unité logique existante.

Pour les fournisseurs VDS 1.1, ajoute les ports du contrôleur spécifié à la liste des ports de contrôleur associés à l’unité logique existante. Si ce paramètre n’est pas spécifié, la liste des ports de contrôleur remplace la liste de ports de contrôleur associés à ce numéro d’unité logique existante.
```
<n>[,<n> [, ...]]
```
Pour une utilisation avec le **contrôleurs** ou **cibles** paramètre. Spécifie les numéros des contrôleurs ou les cibles iSCSI pour la valeur active ou associer.
```
<n-m>[,<n-m>[,…]]
```
Pour une utilisation avec le **ports** paramètre. Spécifie les ports du contrôleur pour définir active à l’aide d’un nombre de contrôleur (*n*) et le numéro de port (*m*) paire.

#### <a name="example"></a>Exemple

L’exemple suivant montre comment associer et ajouter des ports à un numéro d’unité logique qui utilise un fournisseur VDS 1.1 :
```
DISKRAID> SEL LUN 5
LUN 5 is now the selected LUN.

DISKRAID> ASSOCIATE PORTS 0-0,0-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1)

DISKRAID> ASSOCIATE PORTS ADD 1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1, Ctlr 1 Port 1)
```

### <a name="BKMK_3"></a>automagic

Définit ou efface les indicateurs qui donnent des conseils aux fournisseurs sur la façon de configurer un numéro d’unité logique. Utilisé sans paramètres, le **automagic** opération affiche une liste d’indicateurs.

#### <a name="syntax"></a>Syntaxe

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

#### <a name="parameters"></a>Paramètres

**set**

Définit les indicateurs spécifiés sur les valeurs spécifiées.

**clear**

Efface les indicateurs spécifiés. Le **tous les** mot clé efface tous les indicateurs d’automagic.

**apply**

Applique les indicateurs actuels à l’unité logique sélectionné.

\<flag>

Les indicateurs sont identifiés par des acronymes de trois lettres.

|Indicateur|Description|
|----|-----------|
|FCR|Récupération sur incident rapide n’est requise|
|FTL|Une tolérance de panne|
|MSR|Principalement des lectures|
|MXD|Nombre maximum de disques|
|MXS|Taille maximale attendue|
|ORA|Alignement de lecture optimale|
|ORS|Taille optimale de lecture|
|OSR|Optimiser pour les lectures séquentielles|
|OSW|Optimiser pour les écritures séquentielles|
|OWA|Alignement de l’écriture optimale|
|OWS|Taille d’écriture optimal|
|RBP|Reconstruire la priorité|
|RBV|Lecture Revenez vérifier activé|
|TPM|Remappage activé|
|STS|Taille d’entrelacement|
|WTC|Écriture via la mise en cache activée|
|YNK|Amovible|

### <a name="BKMK_4"></a>break

Supprime le bre le LUN sélectionné. Le bre et les données qu’il contenue ne sont pas conservées, et les étendues de disque peuvent être réutilisés.

#### <a name="syntax"></a>Syntaxe

```
break plex=<plex_number> [noerr]
```

#### <a name="parameters"></a>Paramètres

**plex**

Spécifie le nombre de la bre à supprimer. Le bre et les données qu’il contenue ne sont pas conservées, et les ressources utilisées par cette bre seront récupérées. Les données contenues dans le numéro d’unité logique ne sont pas garanties pour être cohérent. Si vous souhaitez conserver cette bre, utilisez Volume Shadow Copy Service (VSS).

**noerr**

Spécifie que les échecs qui se produisent lors de l’exécution de cette opération seront ignorées. Cela est utile en mode script.

#### <a name="remarks"></a>Notes

> [!NOTE]
> Vous devez d’abord sélectionner un numéro d’unité logique de mise en miroir avant d’utiliser le **saut** commande.

> [!CAUTION]
> Toutes les données sur le bre seront supprimées.

> [!CAUTION]
> Toutes les données contenues sur le numéro d’unité logique d’origine n’est pas garanti pour être cohérent.

### <a name="BKMK_5"></a>chap

Définit le CHAP Challenge Handshake Authentication Protocol () shared secret afin que les initiateurs iSCSI et les cibles iSCSI peuvent communiquer entre eux.

#### <a name="syntax"></a>Syntaxe

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

#### <a name="parameters"></a>Paramètres

**ensemble de l’initiateur**

Définit le secret partagé dans le service initiateur iSCSI local utilisé pour l’authentification CHAP mutuelle lors de l’initiateur authentifie la cible.

**n’oubliez pas de l’initiateur**

Communique le secret CHAP d’une cible iSCSI au service initiateur iSCSI local afin que le service initiateur peut utiliser la clé secrète pour s’authentifier auprès de la cible lors de l’authentification CHAP.

**jeu de cibles**

Définit le secret partagé dans la cible iSCSI actuellement sélectionné utilisée pour l’authentification CHAP lorsque la cible authentifie l’initiateur.

**n’oubliez pas de cible**

Communique le secret CHAP de l’initiateur iSCSI à la cible iSCSI de focus actuel afin que la cible peut utiliser la clé secrète pour s’authentifier auprès de l’initiateur au cours de l’authentification CHAP mutuelle.

**secret**

Spécifie la clé secrète à utiliser. Si elle est vide, le secret sera effacé.

**target**

Spécifie une cible dans le sous-système actuellement sélectionné à associer à la clé secrète. Cela est facultatif lors de la définition d’une clé secrète sur l’initiateur et en laissant indique que la clé secrète sera utilisée pour toutes les cibles qui n’est pas déjà une clé secrète associée.

**initiatorname**

Spécifie un nom d’initiateur iSCSI à associer à la clé secrète. Cela est facultatif lors de la définition d’une clé secrète du serveur cible et en laissant indique que la clé secrète sera utilisée pour tous les initiateurs qui n’est pas déjà une clé secrète associée.

### <a name="BKMK_6"></a>create

Crée un nouveau numéro d’unité logique ou d’une cible iSCSI sur le sous-système actuellement sélectionné, ou crée un groupe de portail cible sur la cible actuellement sélectionnée. Vous pouvez afficher la liaison réelle à l’aide de la **DiskRAID liste** commande.

#### <a name="syntax"></a>Syntaxe

```
create lun simple [size=<n>] [drives=<n>] [noerr]
create lun stripe [size=<n>] [drives=<n, n> [,...]]  [stripesize=<n>] [noerr]
create lun raid [size=<n>] [drives=<n, n> [,...]] [stripesize=<n>] [noerr]
create lun mirror [size=<n>] [drives=<n, n> [,...]] [stripesize=<n>] [noerr]
create lun automagic size=<n> [noerr]
create target name=<name> [iscsiname=<iscsiname>] [noerr]
create tpgroup [noerr]
```

#### <a name="parameter"></a>Paramètre

**simple**

Crée un numéro d’unité logique simple.

**stripe**

Crée un numéro d’unité logique agrégé par bandes.

**RAID**

Crée un numéro d’unité logique agrégé par bandes avec parité.

**mirror**

Crée un numéro d’unité logique de mise en miroir.

**automagic**

Crée un numéro d’unité logique à l’aide de la *automagic* indicateurs actuellement en vigueur. Consultez le **automagic** sous-composant commande pour plus d’informations.

**size**=

Spécifie la taille totale de numéro d’unité logique en mégaoctets. Si le **taille =** paramètre n’est pas spécifié, le numéro d’unité logique créé sera la taille maximale autorisée par les lecteurs spécifiés.

Un fournisseur crée généralement un numéro d’unité logique au moins aussi grand que la taille demandée, mais le fournisseur peut avoir à arrondir par excès à la plus grande taille dans certains cas. Par exemple, si la taille est spécifiée comme.99 Go et le fournisseur peuvent seulement allouer des étendues de disque Go, le numéro d’unité logique résultant serait 1 Go.

Pour spécifier la taille à l’aide d’autres unités, utilisez un des suffixes reconnus suivants immédiatement après la taille :
-   **B** pour octets.
-   **Base de connaissances** pour exprimer en kilo-octets.
-   **Mo** pour mégaoctet.
-   **Go** pour gigaoctet.
-   **TO** téraoctet.
-   **PO** pour se mesure en pétaoctets.

**drives**=

Spécifie le *drive_number* pour les lecteurs à utiliser pour créer un numéro d’unité logique. Si le **taille =** paramètre n’est pas spécifié, le numéro d’unité logique créé est la taille maximale autorisée par les lecteurs spécifiés. Si le **taille =** est précisé, fournisseurs sélectionnera les lecteurs à partir de la liste de lecteur spécifiée pour créer le numéro d’unité logique. Fournisseurs tentera d’utiliser les lecteurs dans l’ordre spécifié lorsque cela est possible.

**stripesize**=

Spécifie la taille en mégaoctets pour un *stripe* ou *RAID* numéro d’unité logique. Impossible de modifier le stripesize après que le numéro d’unité logique est créé.

Pour spécifier la taille à l’aide d’autres unités, utilisez un des suffixes reconnus suivants immédiatement après la taille :
-   **B** pour octets.
-   **Base de connaissances** pour exprimer en kilo-octets.
-   **Mo** pour mégaoctet.
-   **Go** pour gigaoctet.
-   **TO** téraoctet.
-   **PO** pour se mesure en pétaoctets.

**target**

Crée une nouvelle cible iSCSI sur le sous-système actuellement sélectionné.

**name**

Fournit le nom convivial pour la cible.

**iscsiname**

Fournit l’iSCSI de noms pour la cible et peut être omis pour que le fournisseur de générer un nom.

**tpgroup**

Crée un nouveau groupe de portail de cible iSCSI sur la cible actuellement sélectionnée.

**noerr**

Spécifie que les échecs qui se produisent lors de l’exécution de cette opération seront ignorées. Cela est utile en mode script.

#### <a name="remarks"></a>Notes

-   Soit le **taille**= ou **lecteurs**= paramètre doit être spécifié. Ils peuvent également être utilisés ensemble.
-   La taille d’entrelacement pour un numéro d’unité logique ne peut pas être modifiée après la création.

### <a name="BKMK_7"></a>delete

Supprime le LUN sélectionné, la cible iSCSI (à condition qu’il sont a pas de numéros d’unités logiques associées à la cible iSCSI) ou le groupe de portail cible iSCSI.

#### <a name="syntax"></a>Syntaxe

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

#### <a name="parameters"></a>Paramètres

**lun**

Supprime le LUN actuellement sélectionné et toutes les données qu’il contient.

**uninstall**

Spécifie que le disque sur le système local associé au LUN sera nettoyé avant que le numéro d’unité logique est supprimé.

**target**

Supprime la cible iSCSI actuellement sélectionnée si aucune LUN n’est associées à la cible.

**tpgroup**

Supprime le groupe de portail cible iSCSI actuellement sélectionné.

**noerr**

Spécifie que les échecs qui se produisent lors de l’exécution de cette opération seront ignorées. Cela est utile en mode script.

### <a name="BKMK_8"></a>detail

Affiche des informations détaillées sur l’objet actuellement sélectionné du type spécifié.

#### <a name="syntax"></a>Syntaxe

```
Detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

#### <a name="parameters"></a>Paramètres

**hbaport**

Affiche des informations détaillées sur le port de carte (HBA) de bus hôte actuellement sélectionné.

**iadapter**

Affiche des informations détaillées sur l’adaptateur d’initiateur iSCSI actuellement sélectionné.

**iportal**

Affiche des informations détaillées sur le portail d’initiateur iSCSI actuellement sélectionné.

**provider**

Affiche des informations détaillées sur le fournisseur actuellement sélectionné.

**subsystem**

Affiche des informations détaillées sur le sous-système actuellement sélectionné.

**controller**

Affiche des informations détaillées sur le contrôleur actuellement sélectionné.

**port**

Affiche des informations détaillées sur le port de contrôleur actuellement sélectionné.

**drive**

Affiche des informations détaillées sur le lecteur sélectionné, y compris les LUN occupée.

**lun**

Affiche des informations détaillées sur le LUN sélectionné, y compris la contribution des lecteurs. La sortie diffère légèrement selon que le numéro d’unité logique fait partie d’un sous-système iSCSI ou Fibre Channel. Si la liste des hôtes non masquées contient uniquement un astérisque, cela signifie que le numéro d’unité logique est démasqué sur tous les ordinateurs hôtes.

**tportal**

Affiche des informations détaillées sur le portail cible iSCSI actuellement sélectionné.

**target**

Affiche des informations détaillées sur la cible iSCSI actuellement sélectionné.

**tpgroup**

Affiche des informations détaillées sur le groupe de portail cible iSCSI actuellement sélectionné.

**verbose**

Pour une utilisation uniquement avec le paramètre de numéro d’unité logique. Répertorie des informations supplémentaires, y compris son plex.

### <a name="BKMK_9"></a>dissociate

Jeux spécifié la liste des ports de contrôleur comme inactifs pour le LUN sélectionné (autre contrôleur ports ne sont pas affectés), ou dissocie la liste spécifiée de cibles iSCSI pour le numéro d’unité logique actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

#### <a name="parameter"></a>Paramètre

**Contrôleurs**

Pour une utilisation avec des fournisseurs VDS 1.0 uniquement. Supprime les contrôleurs dans la liste des contrôleurs qui sont associés à des LUN actuellement sélectionné.

**ports**

Pour une utilisation avec uniquement les fournisseurs VDS 1.1. Supprime les ports du contrôleur dans la liste des ports des contrôleurs qui sont associés à des LUN actuellement sélectionné.

**targets**

Pour une utilisation avec uniquement les fournisseurs VDS 1.1. Supprime les cibles de la liste de cibles iSCSI qui sont associés à des LUN actuellement sélectionné.
```
<n> [,<n> [,…]]
```
Pour une utilisation avec le **contrôleurs** ou **cibles** paramètre. Spécifie les numéros des contrôleurs ou les cibles iSCSI pour le définir comme étant inactive ou l’en dissocier.
```
<n-m>[,<n-m>[,…]]
```
Pour une utilisation avec le **ports** paramètre. Spécifie les ports du contrôleur pour définir comme inactive à l’aide d’un nombre de contrôleur (*n*) et le numéro de port (*m*) paire.

#### <a name="example"></a>Exemple

```
DISKRAID> SEL LUN 5
LUN 5 is now the selected LUN.

DISKRAID> ASSOCIATE PORTS 0-0,0-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1)

DISKRAID> ASSOCIATE PORTS ADD 1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1, Ctlr 1 Port 1)

DISKRAID> DISSOCIATE PORTS 0-0,1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 1)
```

### <a name="BKMK_10"></a>exit

Quitte DiskRAID.

#### <a name="syntax"></a>Syntaxe

```
exit
```

### <a name="BKMK_11"></a>extend

Étend le LUN sélectionné en ajoutant des secteurs à la fin de l’unité logique. Pas tous les fournisseurs prennent en charge l’extension des numéros d’unités logiques. N’étend pas les volumes ou des systèmes de fichiers contenus sur le LUN. Une fois que vous étendez le numéro d’unité logique, vous devez étendre les structures sur disque associés à l’aide de la **DiskPart étendre** commande.

#### <a name="syntax"></a>Syntaxe

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

#### <a name="parameters"></a>Paramètres

**size=**

Spécifie la taille en mégaoctets pour étendre le numéro d’unité logique. Si le **taille =** paramètre n’est pas spécifié, le numéro d’unité logique est étendu par la taille maximale autorisée par les lecteurs spécifiés. Si le **taille =** paramètre est spécifié, les fournisseurs sélectionnez les lecteurs dans la liste spécifiée par le **lecteurs =** paramètre pour créer le numéro d’unité logique.

Pour spécifier la taille à l’aide d’autres unités, utilisez un des suffixes reconnus suivants immédiatement après la taille :
-   **B** pour octets.
-   **Base de connaissances** pour exprimer en kilo-octets.
-   **Mo** pour mégaoctet.
-   **Go** pour gigaoctet.
-   **TO** téraoctet
-   **PO** pour se mesure en pétaoctets

**drives=**

Spécifie le \<drive_number > pour les lecteurs à utiliser lors de la création d’un numéro d’unité logique. Si le **taille =** paramètre n’est pas spécifié, le numéro d’unité logique créé est la taille maximale autorisée par les lecteurs spécifiés. Fournisseurs utilisent les lecteurs dans l’ordre spécifié lorsque cela est possible.

**noerr**

Spécifie que les échecs qui se produisent lors de l’exécution de cette opération doivent être ignorées. Cela est utile en mode script.

#### <a name="remarks"></a>Notes

Soit le *taille* ou \<lecteur > paramètre doit être spécifié. Ils peuvent également être utilisés ensemble.

### <a name="BKMK_12"></a>flushcache

Efface le cache sur le contrôleur actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
flushcache controller
```

### <a name="BKMK_13"></a>help

Affiche une liste de toutes les commandes DiskRAID.

#### <a name="syntax"></a>Syntaxe

```
help
```

### <a name="BKMK_14"></a>importtarget

Récupère ou définit la cible d’importation Volume Shadow Copy Service (VSS) actuel qui est définie pour le sous-système actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
importtarget subsystem [set target]
```

#### <a name="parameter"></a>Paramètre

**cible de jeu**

Si spécifié, définit la cible actuellement sélectionnée à la cible d’importation VSS pour le sous-système actuellement sélectionné. Si non spécifié, la commande récupère la cible d’importation VSS actuelle qui est définie pour le sous-système actuellement sélectionné.

### <a name="BKMK_15"></a>initiator

Récupère des informations sur l’initiateur iSCSI local.

#### <a name="syntax"></a>Syntaxe

```
initiator
```

### <a name="BKMK_16"></a>invalidatecache

Invalide le cache sur le contrôleur actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
invalidatecache controller
```

### <a name="BKMK_18"></a>lbpolicy

Définit la stratégie d’équilibrage de charge sur le LUN sélectionné.

#### <a name="syntax"></a>Syntaxe

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

#### <a name="parameters"></a>Paramètres

**type**

Spécifie la stratégie d’équilibrage de charge. Si le type n’est pas spécifié, puis le **chemin d’accès** paramètre doit être spécifié. Type peut être une des opérations suivantes :

**BASCULEMENT**: Utilise un chemin d’accès primaire avec les autres chemins d’accès en cours de chemins de sauvegarde.

**ROUNDROBIN**: Utilise tous les chemins de tourniquet, qui tente de façon séquentielle chaque chemin d’accès.

**SUBSETROUNDROBIN**: Utilise tous les chemins d’accès primaires de manière alternée ; chemins de sauvegarde sont utilisées uniquement si tous les chemins d’accès principaux échouent.

**DYNLQD**: Utilise le chemin d’accès avec le plus petit nombre de demandes actives.

**PONDÉRÉE**: Utilise le chemin d’accès avec le poids minimum (chaque chemin d’accès doit être assigné un poids).

**LEASTBLOCKS**: Utilise le chemin d’accès avec les plus petits blocs.

**VENDORSPECIFIC**: Utilise une stratégie spécifique au fournisseur.

**paths**

Spécifie si un chemin d’accès est **principal** ou a un particulier \<poids >. Les chemins d’accès sauf indication contraire sont implicitement définis en tant que sauvegarde. Les chemins d’accès répertoriés doivent être un des chemins d’accès de l’unité logique actuellement sélectionné.

### <a name="BKMK_19"></a>list

Affiche une liste d’objets du type spécifié.

#### <a name="syntax"></a>Syntaxe

```
List {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

#### <a name="parameters"></a>Paramètres

**hbaports**

Répertorie des informations récapitulatives sur tous les ports HBA connu VDS. Actuellement sélectionné est marqué par un astérisque (*).

**iadapters**

Répertorie des informations résumées concernant tous les adaptateurs d’initiateur iSCSI VDS connu. L’adaptateur de l’initiateur actuellement sélectionné est marqué par un astérisque (*).

**iportals**

Répertorie des informations résumées concernant tous les portails d’initiateur iSCSI dans l’adaptateur de l’initiateur actuellement sélectionné. Le portail initiateur actuellement sélectionné est marqué par un astérisque (*).

**providers**

Répertorie des informations résumées concernant chaque fournisseur VDS connu. Le fournisseur actuellement sélectionné est marqué par un astérisque (*).

**subsystems**

Répertorie des informations résumées concernant chaque sous-système dans le système. Le sous-système actuellement sélectionné est marqué par un astérisque (*).

**Contrôleurs**

Affiche des informations récapitulatives sur chaque contrôleur dans le sous-système actuellement sélectionné. Le contrôleur actuellement sélectionné est marqué par un astérisque (*).

**ports**

Répertorie des informations résumées concernant chaque port de contrôleur dans le contrôleur actuellement sélectionné. Le port actuellement sélectionné est marqué par un astérisque (*).

**drives**

Affiche des informations récapitulatives sur chaque lecteur dans le sous-système actuellement sélectionné. Le lecteur sélectionné est marqué par un astérisque (*).

**luns**

Répertorie des informations résumées concernant chaque numéro d’unité logique dans le sous-système actuellement sélectionné. Le numéro d’unité logique actuellement sélectionnée est marquée par un astérisque (*).

**tportals**

Répertorie des informations résumées concernant tous les portails de cibles iSCSI dans le sous-système actuellement sélectionné. Le portail cible sélectionné est marqué par un astérisque (*).

**targets**

Répertorie des informations résumées concernant toutes les cibles iSCSI dans le sous-système actuellement sélectionné. La cible actuellement sélectionnée est marquée par un astérisque (*).

**tpgroups**

Répertorie des informations résumées concernant tous les groupes de portails de cible iSCSI dans la cible actuellement sélectionnée. Le groupe de portails actuellement sélectionné est marqué par un astérisque (*).

### <a name="BKMK_20"></a>login

Se connecte à l’adaptateur d’initiateur iSCSI spécifiés à la cible iSCSI actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

#### <a name="parameters"></a>Paramètres

**type**

Spécifie le type de connexion à effectuer : **manuelle**, **persistant**, ou **démarrage**. Si non spécifié, une connexion manuelle sera effectuée.

**manuel** -connexion manuellement.

**persistant** - automatiquement utilisent la même connexion lorsque l’ordinateur est redémarré.

**démarrage** -(cette option est pour le développement futur et n’est pas utilisée actuellement<em>.</em>)

**chap**

Spécifie le type d’authentification CHAP à utiliser : **aucun**, **oneway** CHAP, ou **mutuelle** CHAP ; si non spécifié, aucune authentification n’est utilisée.

**tportal**

Spécifie un portail cible facultatif dans le sous-système actuellement sélectionné à utiliser pour la connexion.

**iportal**

Spécifie un portail initiateur facultatif dans l’adaptateur d’initiateur spécifié à utiliser pour la connexion.

\<flag>

Identifié par trois lettres :

**ADRESSES IP**: Requiert IPsec

**EMP**: Activer Multipath i/o

**EHD**: Activer le résumé de l’en-tête

**EDD**: Activer le résumé des données

### <a name="BKMK_21"></a>logout

Enregistre l’adaptateur d’initiateur iSCSI spécifié hors de la cible iSCSI actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
logout target iadapter= <iadapter>
```

#### <a name="parameters"></a>Paramètres

**iadapter**

Spécifie la carte de l’initiateur avec une session de connexion à une déconnexion de.

### <a name="BKMK_22"></a>Maintenance

Effectue des opérations de maintenance sur l’objet actuellement sélectionné du type spécifié.

#### <a name="syntax"></a>Syntaxe

```
maintenance <object operation> [count=<iteration>]
```

#### <a name="parameters"></a>Paramètres

\<object>

Spécifie le type d’objet sur lequel effectuer l’opération. Le *objet* type peut être un **sous-système**, **contrôleur**, **le port, lecteur** ou **LUN**.

\<operation>

Spécifie l’opération de maintenance à effectuer. Le *opération* type peut être **spinup**, **spindown**, **faire clignoter**, **signal sonore** ou **ping** . Un *opération* doit être spécifié.

**count=**

Spécifie le nombre de répétitions de la *opération*. Cela est généralement utilisé avec **faire clignoter**, **signal sonore**, ou **ping**.

### <a name="BKMK_23"></a>name

Définit le nom convivial de la cible de sous-système, numéro d’unité logique ou iSCSI actuellement sélectionnée pour le nom spécifié.

#### <a name="syntax"></a>Syntaxe

```
name {subsystem | lun | target} [<name>]
```

#### <a name="parameter"></a>Paramètre

\<name>

Spécifie un nom pour le sous-système, numéro d’unité logique ou cible. Le nom doit être inférieure à 64 caractères. Si aucun nom n’est fourni, le nom existant, le cas échéant, est supprimé.

### <a name="BKMK_24"></a>offline

Définit l’état de l’objet actuellement sélectionné du type spécifié à **hors connexion**.

#### <a name="syntax"></a>Syntaxe

```
offline <object>
```

#### <a name="parameter"></a>Paramètre

\<object>

Spécifie le type d’objet sur lequel effectuer cette opération. Le \<objet >

type peut être **sous-système**, **contrôleur**, **lecteur**, **LUN**, ou **portail**.

### <a name="BKMK_25"></a>En ligne

Définit l’état de l’objet sélectionné du type spécifié à **online**. Si l’objet est **hbaport**, modifie le statut de chemins d’accès au port HBA actuellement sélectionné pour **online**.

#### <a name="syntax"></a>Syntaxe

```
online <object> 
```

#### <a name="parameter"></a>Paramètre

\<object>

Spécifie le type d’objet sur lequel effectuer cette opération. Le \<objet >

type peut être **hbaport**, **sous-système**, **contrôleur**, **lecteur**, **LUN**, ou  **portail**.

### <a name="BKMK_26"></a>recover

Effectue les opérations nécessaires, telles que la resynchronisation ou de secours, pour réparer le LUN à tolérance de pannes actuellement sélectionné. Par exemple, récupérer peut-être provoquer un échange à chaud d’être lié à un ensemble RAID qui a un disque défectueux ou autres réallocation d’étendue de disque.

#### <a name="syntax"></a>Syntaxe

```
recover <lun>
```

### <a name="BKMK_27"></a>reenumerate

Énumère à nouveau les objets du type spécifié. Si vous utilisez la commande de numéro d’unité logique de l’étendre, vous devez utiliser la commande Actualiser pour mettre à jour la taille du disque avant d’utiliser la commande reenumerate.

#### <a name="syntax"></a>Syntaxe

```
reenumerate {subsystems | drives}
```

#### <a name="parameters"></a>Paramètres

**subsystems**

Interroge le fournisseur pour découvrir d’éventuels nouveaux sous-systèmes qui ont été ajoutés dans le fournisseur actuellement sélectionné.

**drives**

Interroge les bus d’e/s internes pour découvrir de nouveaux lecteurs qui ont été ajoutés dans le sous-système actuellement sélectionné.

### <a name="BKMK_28"></a>refresh

Actualise les données internes pour le fournisseur sélectionné.

#### <a name="syntax"></a>Syntaxe

```
refresh provider
```

### <a name="BKMK_29"></a>rem

Utilisé pour les scripts de commentaire.

#### <a name="syntax"></a>Syntaxe

```
Rem <comment>
```

### <a name="BKMK_30"></a>remove

Supprime le portail cible iSCSI spécifiés à partir du groupe de portail cible sélectionné.

#### <a name="syntax"></a>Syntaxe

```
remove tpgroup tportal=<tportal> [noerr]
```

#### <a name="parameter"></a>Paramètre

**tpgroup tportal=** \<tportal>

Spécifie le portail cible iSCSI à supprimer.

**noerr**

Spécifie que les échecs qui se produisent lors de l’exécution de cette opération doivent être ignorées. Cela est utile en mode script.

### <a name="BKMK_31"></a>replace

Remplace le lecteur spécifié par le lecteur actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
replace drive=<drive_number>
```

#### <a name="parameter"></a>Paramètre

**drive=**

Spécifie le \<drive_number > pour le lecteur à remplacer.

#### <a name="remarks"></a>Notes

-   Le lecteur spécifié n’est peut-être pas le lecteur actuellement sélectionné.

### <a name="BKMK_32"></a>reset

Réinitialise le contrôleur actuellement sélectionné ou le port.

#### <a name="syntax"></a>Syntaxe

```
Reset {controller | port}
```

#### <a name="parameters"></a>Paramètres

**controller**

Réinitialise le contrôleur.

**port**

Réinitialise le port.

### <a name="BKMK_33"></a>select

Affiche ou modifie l’objet actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
Select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

#### <a name="parameters"></a>Paramètres

**object**

Spécifie le type d’objet à sélectionner. Le \<objet > type peut être **fournisseur**, **sous-système**, **contrôleur**, **lecteur**, ou **LUN**.

**hbaport** [\<n>]

Définit le focus sur le port HBA local spécifié. Si aucun port HBA n’est spécifié, la commande affiche actuellement sélectionné (le cas échéant). Spécification d’un index de port HBA non valide entraîne aucun port HBA de focus. Sélection d’un port HBA désélectionne tout initiateur sélectionné adaptateurs et les portails de l’initiateur.

**iadapter** [\<n>]

Définit le focus à l’adaptateur d’initiateur iSCSI local spécifié. Si aucune carte de l’initiateur n’est spécifié, la commande affiche l’adaptateur initiateur actuellement sélectionné (le cas échéant). Spécification d’un index de carte initiateur non valide entraîne aucune carte de l’initiateur de focus. Sélection d’un adaptateur initiateur désélectionne les ports HBA sélectionnés et les portails de l’initiateur.

**iportal** [\<n>]

Définit le focus sur le portail d’initiateur iSCSI local spécifié dans l’adaptateur d’initiateur iSCSI sélectionné. Si aucun portail de l’initiateur n’est spécifié, la commande affiche le portail initiateur actuellement sélectionné (le cas échéant). Spécification d’un index de portail initiateur non valide entraîne aucun portail initiateur sélectionné.

**provider** [\<n>]

Définit le focus sur le fournisseur spécifié. Si aucun fournisseur n’est spécifié, la commande affiche le fournisseur actuellement sélectionné (le cas échéant). Spécification d’un index de fournisseur non valide entraîne aucun fournisseur de focus.

**subsystem** [\<n>]

Définit le focus sur le sous-système spécifié. Si aucun sous-système n’est spécifié, la commande affiche le sous-système qui a le focus (le cas échéant). Spécification d’un index de sous-système non valide entraîne aucun sous-système de focus. Un sous-système implicitement en cochant son fournisseur associé.

**controller** [\<n>]

Définit le focus sur le contrôleur spécifié dans le sous-système actuellement sélectionné. Si aucun contrôleur n’est spécifié, la commande affiche le contrôleur actuellement sélectionné (le cas échéant). Spécification d’un index de contrôleur non valide entraîne aucun contrôleur actif. Sélection d’un contrôleur désélectionne n’importe quel contrôleur sélectionné ports, lecteurs, LUN, portails cibles, cibles et groupes de portail cible.

**port** [\<n>]

Définit le focus sur le port de contrôleur spécifié au sein du contrôleur sélectionné. Si aucun port n’est spécifié, la commande affiche le port actuellement sélectionné (le cas échéant). Spécification d’un index de port non valide entraîne aucun port sélectionné.

**drive** [\<n>]

Définit le focus sur le lecteur spécifié, ou la pile physique, dans le sous-système actuellement sélectionné. Si aucun lecteur n’est spécifié, la commande affiche le lecteur actuellement sélectionné (le cas échéant). Spécification d’un index de lecteur non valide entraîne aucun lecteur actif. En sélectionnant un lecteur désélectionne n’importe quel contrôleurs sélectionnés, ports des contrôleurs, numéros d’unités logiques, portails cibles, cibles et les groupes de portails cible.

**lun** [\<n>]

Définit le focus sur le LUN spécifié dans le sous-système actuellement sélectionné. Si aucun numéro d’unité logique n’est spécifié, la commande affiche le numéro d’unité logique sélectionné (le cas échéant). Spécification d’un index de numéro d’unité logique non valide entraîne aucun numéro d’unité logique sélectionné. En sélectionnant un numéro d’unité logique désélectionne n’importe quel contrôleurs sélectionnés ports des contrôleurs, lecteurs, portails cibles, cibles et les groupes de portails cible.

**tportal** [\<n>]

Définit le focus sur le portail cible iSCSI spécifiés dans le sous-système actuellement sélectionné. Si aucun portail cible n’est spécifié, la commande affiche le portail cible actuellement sélectionné (le cas échéant). Spécification d’un index de portail cible non valide entraîne aucun portail cible sélectionné. En sélectionnant un portail cible désélectionne n’importe quel contrôleurs, ports des contrôleurs, lecteurs, LUN, cibles et groupes de portail cible.

**target** [\<n>]

Définit le focus à la cible iSCSI spécifiés dans le sous-système actuellement sélectionné. Si aucune cible n’est spécifié, la commande affiche la cible actuellement sélectionnée (le cas échéant). Spécification d’un index cible non valide entraîne aucune cible sélectionné. Sélection d’une cible désélectionne n’importe quel contrôleurs, ports des contrôleurs, lecteurs, LUN, portails cibles et les groupes de portails cible.

**tpgroup** [\<n>]

Définit le focus sur le groupe de portail cible iSCSI spécifiés au sein de la cible iSCSI actuellement sélectionné. Si aucun groupe de portail cible est spécifié, la commande affiche le groupe de portail cible sélectionné (le cas échéant). Spécification d’un index de groupe du portail cible non valide entraîne dans aucun groupe de portail cible active.

[\<n>]

Spécifie le \<objet nombre > pour sélectionner. Si le <object number> spécifié n’est pas valide, toutes les sélections existantes pour les objets du type spécifié sont effacées. Si aucun <object number> est spécifié, l’objet en cours s’affiche.

### <a name="BKMK_34"></a>setflag

Définit le lecteur sélectionné actuellement comme un échange à chaud.

#### <a name="syntax"></a>Syntaxe

```
setflag drive hotspare={true | false}
```

#### <a name="parameters"></a>Paramètres

**true**

Sélectionne le lecteur sélectionné actuellement comme un échange à chaud.

**false**

Désélectionne le lecteur sélectionné actuellement comme un échange à chaud.

#### <a name="remarks"></a>Notes

À chaud ne peut pas être utilisé pour les opérations de liaison ordinaires numéro d’unité logique. Ils sont réservés pour la gestion des erreurs uniquement. Le lecteur ne doit pas être actuellement lié à n’importe quel numéro d’unité logique existante.

### <a name="BKMK_shrink"></a>réduction

Réduit la taille de la LUN sélectionnée.

#### <a name="syntax"></a>Syntaxe

```
shrink lun size=<n> [noerr]
```

#### <a name="parameters"></a>Paramètres

**size=**

Spécifie la quantité d’espace en mégaoctets (Mo) pour réduire la taille de la LUN souhaitée par. Pour spécifier la taille à l’aide d’autres unités, utilisez un des suffixes reconnus (B, Ko, Mo, Go, To et PB) immédiatement après la taille.

**noerr**

Spécifie que les échecs qui se produisent lors de l’exécution de cette opération seront ignorées. Cela est utile en mode script.

### <a name="BKMK_35"></a>standby

Modifie le statut de chemins d’accès pour le port de carte (HBA) de bus hôte actuellement sélectionné en mode de veille.

#### <a name="syntax"></a>Syntaxe

```
standby hbaport
```

#### <a name="parameters"></a>Paramètres

**hbaport**

Modifie le statut de chemins d’accès pour le port de carte (HBA) de bus hôte actuellement sélectionné en mode de veille.

### <a name="BKMK_36"></a>unmask

Rend les LUN actuellement sélectionnés soit accessible depuis les ordinateurs hôtes indiqués.

#### <a name="syntax"></a>Syntaxe

```
unmask LUN {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

#### <a name="parameters"></a>Paramètres

**all**

Spécifie que le numéro d’unité logique doit être effectuée accessible à partir de tous les ordinateurs hôtes. Toutefois, vous ne pouvez pas démasquer le numéro d’unité logique pour toutes les cibles dans un sous-système iSCSI.

> [!IMPORTANT]
> Vous devez la déconnexion de la cible avant d’exécuter la commande Afficher tout.

**None**

Spécifie que le numéro d’unité logique ne doit pas être accessible à n’importe quel hôte.

> [!IMPORTANT]
> Vous devez la déconnexion de la cible avant d’exécuter la commande Annuler le masquage LUN NONE.

**add**

Spécifie que les ordinateurs hôtes spécifiés doivent être ajoutés à la liste existante d’hôtes ce LUN est accessible à partir de. Si ce paramètre n’est pas spécifié, la liste d’hôtes fournie remplace la liste existante d’hôtes ce LUN est accessible à partir de.

**WWN=**

Spécifie une liste de nombres hexadécimaux représentant les noms à l’échelle mondiale à partir de laquelle le numéro d’unité logique ou les hôtes doivent être accessibles. Pour le masquage et à un ensemble spécifique d’ordinateurs hôtes dans un sous-système de Fibre Channel, vous pouvez taper une liste délimitée par des points-virgules du WWN pour les ports sur les ordinateurs hôtes d’intérêt.

**initiator=**

Spécifie une liste des initiateurs iSCSI à laquelle le numéro d’unité logique sélectionné doit être accessibles. Pour le masquage et à un ensemble spécifique d’ordinateurs hôtes dans un sous-système iSCSI, vous pouvez taper une liste délimitée par des points-virgules des noms d’initiateur iSCSI pour les initiateurs sur les ordinateurs hôtes d’intérêt.

**uninstall**

Si spécifié, désinstalle le disque associé avec le numéro d’unité logique sur le système local avant le LUN est masqué.

## <a name="scripting-diskraid"></a>Écriture de scripts DiskRAID

DiskRAID peut être scriptée sur n’importe quel ordinateur exécutant Windows Server 2008 ou Windows Server 2003 avec un fournisseur de matériel VDS associé. Pour appeler un script de DiskRAID, à l’invite de commandes, tapez :
```
diskraid /s <script.txt>
```
Par défaut, DiskRAID arrête le traitement des commandes et retourne un code d’erreur s’il existe un problème dans le script. Pour continuer à exécuter le script et ignorer les erreurs, incluez le paramètre NOERR sur la commande. Cela vous permet d’en tant qu’à l’aide d’un seul script pour supprimer tous les numéros d’unités logiques dans un sous-système, quel que soit le nombre total de numéros d’unités logiques. Pas toutes les commandes prennent en charge le paramètre NOERR. Les erreurs sont toujours retournées sur les erreurs de syntaxe de commande, indépendamment de si vous avez inclus le paramètre NOERR,

### <a name="diskraid-error-codes"></a>Codes d’erreur DiskRAID

|Code d'erreur|Description de l'erreur|
|----------|-----------------|
|0|Aucune erreur ne s’est produite. La totalité du script s’est exécuté sans échec.|
|1|Une exception irrécupérable s’est produite.|
|2|Les arguments spécifiés sur une ligne de commande DiskRAID sont incorrectes.|
|3|DiskRAID n’a pas pu ouvrir le script spécifié ou le fichier de sortie.|
|4|Un des services que Diskraid utilise a retourné une erreur.|
|5|Une erreur de syntaxe de commande s’est produite. Le script a échoué car un objet a été sélectionné de façon incorrecte ou n’est pas valide pour une utilisation avec cette commande.|

## <a name="example-interactively-view-status-of-subsystem"></a>Exemple : Afficher l’état du sous-système de manière interactive

Si vous souhaitez afficher l’état du sous-système 0 sur votre ordinateur, tapez la commande suivante à la ligne de commande :
```
diskraid
```
Appuyez sur ENTRÉE. Affiche les informations suivantes :
```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```
Pour sélectionner le sous-système de 0, tapez la commande suivante à l’invite de DiskRAID :
```
select subsystem 0
```
Appuyez sur ENTRÉE. Sortie semblable au suivant s’affiche :
```
Subsystem 0 is now the selected subsystem.

DISKRAID> list drives

  Drive ###  Status      Health          Size      Free    Bus  Slot  Flags
  ---------  ----------  ------------  --------  --------  ---  ----  -----
  Drive 0    Online      Healthy         107 GB    107 GB    0     1
  Drive 1    Offline     Healthy          29 GB     29 GB    1     0
  Drive 2    Online      Healthy         107 GB    107 GB    0     2
  Drive 3    Not Ready   Healthy          19 GB     19 GB    1     1
```
Pour quitter DiskRAID, tapez la commande suivante à l’invite de DiskRAID :
```
exit
```