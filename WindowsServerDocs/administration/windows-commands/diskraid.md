---
title: diskraid
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f72e91f856da3b24e7450381b293f4b365d914f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377797"
---
# <a name="diskraid"></a>diskraid



DiskRAID est un outil de ligne de commande qui vous permet de configurer et de gérer un tableau redondant de sous-systèmes de stockage (RAID) indépendants (ou peu onéreux).

RAID est une méthode utilisée pour normaliser et classer les systèmes de disques à tolérance de pannes. Les niveaux RAID offrent différents mixages de performances, de fiabilité et de coût. RAID est généralement utilisé sur les serveurs. Certains serveurs fournissent trois des niveaux RAID : le niveau 0 (entrelacement), le niveau 1 (mise en miroir) et le niveau 5 (entrelacement avec parité).

Un sous-système RAID matériel distingue les unités de stockage physiquement adressables les unes des autres à l’aide d’un numéro d’unité logique (LUN). Un objet LUN doit avoir au moins un plex et peut comporter un nombre quelconque de plex supplémentaires. Chaque Plex contient une copie des données sur l’objet LUN. Les plex peuvent être ajoutés et supprimés d’un objet LUN.

La plupart des commandes DiskRAID fonctionnent sur un port d’adaptateur de bus hôte (HBA) spécifique, un adaptateur d’initiateur, un portail initiateur, un fournisseur, un sous-système, un contrôleur, un port, un lecteur, un numéro d’unité logique, un portail cible, une cible ou un groupe de portails cible. Vous utilisez la commande Sélectionner pour sélectionner un objet. On dit que l’objet sélectionné a le focus. Le focus simplifie les tâches de configuration courantes, telles que la création de plusieurs numéros d’unités logiques dans le même sous-système.

> [!NOTE]
> L’outil en ligne de commande DiskRAID fonctionne uniquement avec les sous-systèmes de stockage qui prennent en charge le service de disque virtuel (VDS).

## <a name="diskraid-commands"></a>Commandes DiskRAID

Pour afficher la syntaxe de la commande, cliquez sur une commande :
-   [add](#BKMK_1)
-   [Association](#BKMK_2)
-   [magie](#BKMK_3)
-   [break](#BKMK_4)
-   [chap](#BKMK_5)
-   [create](#BKMK_6)
-   [delete](#BKMK_7)
-   [détail](#BKMK_8)
-   [dissocier](#BKMK_9)
-   [exit](#BKMK_10)
-   [extend](#BKMK_11)
-   [flushcache](#BKMK_12)
-   [help](#BKMK_13)
-   [importtarget](#BKMK_14)
-   [initiator](#BKMK_15)
-   [invalidatecache](#BKMK_16)
-   [lbpolicy](#BKMK_18)
-   [tarifs](#BKMK_19)
-   [login](#BKMK_20)
-   [logout](#BKMK_21)
-   [jour](#BKMK_22)
-   [name](#BKMK_23)
-   [hors connexion](#BKMK_24)
-   [service](#BKMK_25)
-   [recover](#BKMK_26)
-   [réénumérer](#BKMK_27)
-   [refresh](#BKMK_28)
-   [rem](#BKMK_29)
-   [Installez](#BKMK_30)
-   [replace](#BKMK_31)
-   [initialisation](#BKMK_32)
-   [sélectionné](#BKMK_33)
-   [setflag](#BKMK_34)
-   [shrink](#BKMK_shrink)
-   [secours](#BKMK_35)
-   [Démasquez](#BKMK_36)

### <a name="BKMK_1"></a>complémentaires

Ajoute un numéro d’unité logique (LUN) existant au LUN actuellement sélectionné ou ajoute un portail cible iSCSI au groupe de portail cible iSCSI actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

#### <a name="parameters"></a>Paramètres

**numéro d’unité logique de plex**=*n*

Spécifie le numéro de LUN à ajouter en tant que Plex au LUN actuellement sélectionné.

> [!CAUTION]
> Toutes les données sur le LUN en cours d’ajout en tant que Plex seront supprimées.

**TPGROUP TPORTAL =** <em>n</em>

Spécifie le numéro du portail cible iSCSI à ajouter au groupe de portails cibles iSCSI actuellement sélectionné.

**noerr**

Spécifie que les échecs qui se produisent pendant l’exécution de cette opération seront ignorés. Cela est utile en mode script.

### <a name="BKMK_2"></a>Association

Définit la liste spécifiée de ports de contrôleur comme actifs pour le numéro d’unité logique actuellement sélectionné (les autres ports de contrôleur sont rendus inactifs), ou ajoute les ports de contrôleur spécifiés à la liste des ports de contrôleur actifs existants pour le numéro d’unité logique actuellement sélectionné, ou associe le cible iSCSI spécifiée pour le numéro d’unité logique actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

#### <a name="parameters"></a>Paramètres

**secondaires**

À utiliser avec les fournisseurs VDS 1,0 uniquement. Ajoute ou remplace la liste des contrôleurs associés au LUN actuellement sélectionné.

**utilis**

À utiliser avec les fournisseurs VDS 1,1 uniquement. Ajoute ou remplace la liste des ports de contrôleur associés au LUN actuellement sélectionné.

**compilé**

À utiliser avec les fournisseurs VDS 1,1 uniquement. Ajoute ou remplace la liste des cibles iSCSI associées au LUN actuellement sélectionné.

**add**

Pour les fournisseurs VDS 1,0, ajoute les contrôleurs spécifiés à la liste existante de contrôleurs associés au numéro d’unité logique. Si ce paramètre n’est pas spécifié, la liste des contrôleurs remplace la liste existante de contrôleurs associés à ce numéro d’unité logique.

Pour les fournisseurs VDS 1,1, ajoute les ports de contrôleur spécifiés à la liste existante de ports de contrôleur associés au numéro d’unité logique. Si ce paramètre n’est pas spécifié, la liste des ports de contrôleur remplace la liste existante de ports de contrôleur associés à ce numéro d’unité logique.
```
<n>[,<n> [, ...]]
```
À utiliser avec le paramètre **Controllers** ou **Targets** . Spécifie le nombre de contrôleurs ou de cibles iSCSI à définir comme actifs ou associés.
```
<n-m>[,<n-m>[,…]]
```
À utiliser avec le paramètre **ports** . Spécifie les ports de contrôleur à définir comme actifs à l’aide d’une paire numéro de contrôleur (*n*) et numéro de port (*m*).

#### <a name="example"></a>Exemple

L’exemple suivant montre comment associer et ajouter des ports à un numéro d’unité logique (LUN) qui utilise un fournisseur VDS 1,1 :
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

### <a name="BKMK_3"></a>magie

Définit ou efface des indicateurs qui donnent aux fournisseurs des indications sur la configuration d’un numéro d’unité logique (LUN). Utilisée sans paramètre, l’opération **automagic** affiche une liste d’indicateurs.

#### <a name="syntax"></a>Syntaxe

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

#### <a name="parameters"></a>Paramètres

**set**

Affecte les valeurs spécifiées aux indicateurs spécifiés.

**clear**

Efface les indicateurs spécifiés. Le mot clé **All** efface tous les indicateurs automagic.

**appliqu**

Applique les indicateurs actuels au numéro d’unité logique sélectionné.

indicateur de \<>

Les indicateurs sont identifiés par des acronymes à trois lettres.

|Flag|Description|
|----|-----------|
|FCR|Récupération rapide après incident requise|
|FTL|À tolérance de panne|
|Caisse|Lire principalement|
|MXD|Nombre maximal de lecteurs|
|MXS|Taille maximale attendue|
|RA|Alignement optimal de la lecture|
|ORS|Taille de lecture optimale|
|OSR|Optimiser les lectures séquentielles|
|OSW|Optimiser pour les écritures séquentielles|
|OWA|Alignement optimal des écritures|
|OWS|Taille d’écriture optimale|
|RBP|Reconstruire la priorité|
|RBV|Vérification de lecture activée|
|RMP|Remappage activé|
|Team|Taille d’entrelacement|
|WTC|Mise en cache accessible en écriture activée|
|YNK|Bande|

### <a name="BKMK_4"></a>saut

Supprime le plex de l’unité logique actuellement sélectionnée. Le plex et les données qu’il contient ne sont pas conservés, et les étendues de lecteur peuvent être récupérées.

#### <a name="syntax"></a>Syntaxe

```
break plex=<plex_number> [noerr]
```

#### <a name="parameters"></a>Paramètres

**duplex**

Spécifie le numéro du Plex à supprimer. Le plex et les données qu’il contient ne sont pas conservés, et les ressources utilisées par ce Plex sont récupérées. Il n’est pas garanti que les données contenues sur le numéro d’unité logique soient cohérentes. Si vous souhaitez conserver ce Plex, utilisez le Service VSS (VSS).

**noerr**

Spécifie que les échecs qui se produisent pendant l’exécution de cette opération seront ignorés. Cela est utile en mode script.

#### <a name="remarks"></a>Notes

> [!NOTE]
> Vous devez d’abord sélectionner un numéro d’unité logique mis en miroir avant d’utiliser la commande **break** .

> [!CAUTION]
> Toutes les données sur le plex seront supprimées.

> [!CAUTION]
> Il n’est pas garanti que toutes les données contenues sur le numéro d’unité logique d’origine soient cohérentes.

### <a name="BKMK_5"></a>chap

Définit le secret partagé CHAP (Challenge Handshake Authentication Protocol) afin que les initiateurs iSCSI et les cibles iSCSI puissent communiquer les uns avec les autres.

#### <a name="syntax"></a>Syntaxe

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

#### <a name="parameters"></a>Paramètres

**initiateur, ensemble**

Définit le secret partagé dans le service initiateur iSCSI local utilisé pour l’authentification CHAP mutuelle lorsque l’initiateur authentifie la cible.

**rappel de l’initiateur**

Communique le secret CHAP d’une cible iSCSI au service de l’initiateur iSCSI local afin que le service initiateur puisse utiliser le secret pour s’authentifier auprès de la cible pendant l’authentification CHAP.

**jeu de cibles**

Définit le secret partagé dans la cible iSCSI actuellement sélectionnée utilisée pour l’authentification CHAP lorsque la cible authentifie l’initiateur.

**mémoriser la cible**

Communique le secret CHAP d’un initiateur iSCSI à la cible iSCSI actuellement active afin que la cible puisse utiliser le secret pour s’authentifier auprès de l’initiateur pendant l’authentification CHAP mutuelle.

**confidentialité**

Spécifie le secret à utiliser. S’il est vide, le secret sera effacé.

**Indicatif**

Spécifie une cible dans le sous-système actuellement sélectionné à associer à la clé secrète. Cette option est facultative lors de la définition d’un secret sur l’initiateur et sa sortie indique que la clé secrète sera utilisée pour toutes les cibles qui n’ont pas encore de secret associé.

**initiatorname**

Spécifie un nom iSCSI d’initiateur à associer à la clé secrète. Cette option est facultative lors de la définition d’un secret sur une cible et sa sortie indique que le secret sera utilisé pour tous les initiateurs qui n’ont pas encore de secret associé.

### <a name="BKMK_6"></a>créés

Crée un numéro d’unité logique (LUN) ou iSCSI cible sur le sous-système actuellement sélectionné, ou crée un groupe de portails cible sur la cible actuellement sélectionnée. Vous pouvez afficher la liaison réelle à l’aide de la commande **DiskRAID List** .

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

**Stripe**

Crée un LUN agrégé par bandes.

**SOLUTION**

Crée un LUN agrégé par bandes avec parité.

**mirror**

Crée un LUN mis en miroir.

**magie**

Crée un numéro d’unité logique à l’aide des indicateurs *automagic* actuellement en vigueur. Pour plus d’informations, consultez la sous-commande **automagic** .

**taille**=

Spécifie la taille totale des LUN en mégaoctets. Si le paramètre **Size =** n’est pas spécifié, le numéro d’unité logique (LUN) créé sera la plus grande taille possible autorisée par tous les lecteurs spécifiés.

Un fournisseur crée généralement un numéro d’unité logique au moins aussi grand que la taille demandée, mais le fournisseur peut être amené à arrondir à la taille suivante dans certains cas. Par exemple, si la taille est spécifiée comme. 99 Go et que le fournisseur peut uniquement allouer des étendues de disque Go, le numéro d’unité logique résultant est de 1 Go.

Pour spécifier la taille à l’aide d’autres unités, utilisez l’un des suffixes reconnus suivants immédiatement après la taille :
-   **B** pour Byte.
-   **Ko** pour kilo-octets.
-   **Mo** pour mégaoctets.
-   **Go** pour gigaoctets.
-   **To** pour téraoctet.
-   **PB** pour pétaoctet.

**lecteurs**=

Spécifie le *drive_number* pour les lecteurs à utiliser pour créer un numéro d’unité logique. Si le paramètre **Size =** n’est pas spécifié, le numéro d’unité logique (LUN) créé est la plus grande taille possible autorisée par tous les lecteurs spécifiés. Si le paramètre **Size =** est spécifié, les fournisseurs sélectionnent les lecteurs dans la liste de lecteurs spécifiée pour créer le numéro d’unité logique. Les fournisseurs tentent d’utiliser les lecteurs dans l’ordre spécifié lorsque cela est possible.

**stripesize**=

Spécifie la taille, en mégaoctets, d’une *bande* ou d’un numéro d’unité logique *RAID* . Le STRIPESIZE ne peut pas être modifié après la création du numéro d’unité logique.

Pour spécifier la taille à l’aide d’autres unités, utilisez l’un des suffixes reconnus suivants immédiatement après la taille :
-   **B** pour Byte.
-   **Ko** pour kilo-octets.
-   **Mo** pour mégaoctets.
-   **Go** pour gigaoctets.
-   **To** pour téraoctet.
-   **PB** pour pétaoctet.

**Indicatif**

Crée une nouvelle cible iSCSI sur le sous-système actuellement sélectionné.

**name**

Fournit le nom convivial de la cible.

**iscsiname**

Fournit le nom iSCSI pour la cible et peut être omis pour que le fournisseur génère un nom.

**tpgroup**

Crée un nouveau groupe de portails cibles iSCSI sur la cible actuellement sélectionnée.

**noerr**

Spécifie que les échecs qui se produisent pendant l’exécution de cette opération seront ignorés. Cela est utile en mode script.

#### <a name="remarks"></a>Notes

-   Le paramètre **Size**= ou **Drives**= doit être spécifié. Elles peuvent également être utilisées ensemble.
-   La taille d’entrelacement d’un numéro d’unité logique ne peut pas être modifiée après la création.

### <a name="BKMK_7"></a>supprimer

Supprime le numéro d’unité logique (LUN) actuellement sélectionné, la cible iSCSI (tant qu’il n’y a pas de LUN associées à la cible iSCSI) ou le groupe de portails cibles iSCSI.

#### <a name="syntax"></a>Syntaxe

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

#### <a name="parameters"></a>Paramètres

**Lun**

Supprime l’unité logique actuellement sélectionnée et toutes les données qu’elle contient.

**supprimer**

Spécifie que le disque sur le système local associé au numéro d’unité logique est nettoyé avant la suppression de l’unité logique.

**Indicatif**

Supprime la cible iSCSI actuellement sélectionnée si aucun numéro d’unité logique n’est associé à la cible.

**tpgroup**

Supprime le groupe de portails cibles iSCSI actuellement sélectionné.

**noerr**

Spécifie que les échecs qui se produisent pendant l’exécution de cette opération seront ignorés. Cela est utile en mode script.

### <a name="BKMK_8"></a>détail

Affiche des informations détaillées sur l’objet actuellement sélectionné du type spécifié.

#### <a name="syntax"></a>Syntaxe

```
Detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

#### <a name="parameters"></a>Paramètres

**HBAPORT**

Répertorie des informations détaillées sur le port de l’adaptateur de bus hôte (HBA) actuellement sélectionné.

**iadapter**

Répertorie des informations détaillées sur la carte de l’initiateur iSCSI actuellement sélectionnée.

**iportal**

Répertorie des informations détaillées sur le portail de l’initiateur iSCSI actuellement sélectionné.

**provider**

Répertorie des informations détaillées sur le fournisseur actuellement sélectionné.

**subsystem**

Répertorie des informations détaillées sur le sous-système actuellement sélectionné.

**SideWinder**

Répertorie des informations détaillées sur le contrôleur actuellement sélectionné.

**importer**

Répertorie des informations détaillées sur le port de contrôleur actuellement sélectionné.

**drive**

Répertorie des informations détaillées sur le lecteur actuellement sélectionné, y compris sur les numéros d’unités logiques.

**Lun**

Répertorie des informations détaillées sur le numéro d’unité logique actuellement sélectionné, y compris les lecteurs contributeurs. La sortie diffère légèrement selon que le numéro d’unité logique fait partie d’un Fibre Channel ou d’un sous-système iSCSI. Si la liste hôtes non masqués contient uniquement un astérisque, cela signifie que le numéro d’unité logique est démasqué pour tous les ordinateurs hôtes.

**TPORTAL**

Répertorie des informations détaillées sur le portail cible iSCSI actuellement sélectionné.

**Indicatif**

Répertorie des informations détaillées sur la cible iSCSI actuellement sélectionnée.

**tpgroup**

Répertorie des informations détaillées sur le groupe de portail cible iSCSI actuellement sélectionné.

**verbose**

À utiliser uniquement avec le paramètre LUN. Répertorie des informations supplémentaires, y compris ses Plex.

### <a name="BKMK_9"></a>dissocier

Définit la liste spécifiée de ports de contrôleur comme inactifs pour le numéro d’unité logique actuellement sélectionné (les autres ports de contrôleur ne sont pas affectés) ou dissocie la liste spécifiée de cibles iSCSI pour le numéro d’unité logique actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

#### <a name="parameter"></a>Paramètre

**secondaires**

À utiliser avec les fournisseurs VDS 1,0 uniquement. Supprime les contrôleurs de la liste des contrôleurs associés au LUN actuellement sélectionné.

**utilis**

À utiliser avec les fournisseurs VDS 1,1 uniquement. Supprime les ports du contrôleur de la liste des ports de contrôleur associés à l’unité logique actuellement sélectionnée.

**compilé**

À utiliser avec les fournisseurs VDS 1,1 uniquement. Supprime les cibles de la liste des cibles iSCSI associées au LUN actuellement sélectionné.
```
<n> [,<n> [,…]]
```
À utiliser avec le paramètre **Controllers** ou **Targets** . Spécifie le nombre de contrôleurs ou de cibles iSCSI à définir comme inactifs ou dissociés.
```
<n-m>[,<n-m>[,…]]
```
À utiliser avec le paramètre **ports** . Spécifie les ports du contrôleur à définir comme inactifs à l’aide d’une paire numéro de contrôleur (*n*) et numéro de port (*m*).

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

### <a name="BKMK_10"></a>terminer

Quitte DiskRAID.

#### <a name="syntax"></a>Syntaxe

```
exit
```

### <a name="BKMK_11"></a>étendre

Étend le numéro d’unité logique actuellement sélectionné en ajoutant des secteurs à la fin du numéro d’unité logique. Tous les fournisseurs ne prennent pas en charge l’extension des numéros d’unité logique. N’étend aucun volume ou système de fichiers contenu sur le numéro d’unité logique. Après avoir étendu le numéro d’unité logique (LUN), vous devez étendre les structures sur disque associées à l’aide de la commande **diskpart Extend** .

#### <a name="syntax"></a>Syntaxe

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

#### <a name="parameters"></a>Paramètres

**taille =**

Spécifie la taille en mégaoctets pour étendre le numéro d’unité logique. Si le paramètre **Size =** n’est pas spécifié, le numéro d’unité logique (LUN) est étendu par la plus grande taille possible autorisée par tous les lecteurs spécifiés. Si le paramètre **Size =** est spécifié, les fournisseurs sélectionnent les lecteurs dans la liste spécifiée par le paramètre **Drives =** pour créer le numéro d’unité logique.

Pour spécifier la taille à l’aide d’autres unités, utilisez l’un des suffixes reconnus suivants immédiatement après la taille :
-   **B** pour Byte.
-   **Ko** pour kilo-octets.
-   **Mo** pour mégaoctets.
-   **Go** pour gigaoctets.
-   **To** pour téraoctets
-   **PB** pour pétaoctet

**lecteurs =**

Spécifie le > de drive_number \<pour les lecteurs à utiliser lors de la création d’un numéro d’unité logique. Si le paramètre **Size =** n’est pas spécifié, le numéro d’unité logique (LUN) créé est la plus grande taille possible autorisée par tous les lecteurs spécifiés. Les fournisseurs utilisent les lecteurs dans l’ordre spécifié dans la mesure du possible.

**noerr**

Spécifie que les échecs qui se produisent lors de l’exécution de cette opération doivent être ignorés. Cela est utile en mode script.

#### <a name="remarks"></a>Notes

Vous devez spécifier la *taille* ou le paramètre de > du lecteur \<. Elles peuvent également être utilisées ensemble.

### <a name="BKMK_12"></a>flushcache

Efface le cache sur le contrôleur actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
flushcache controller
```

### <a name="BKMK_13"></a>Aide

Affiche la liste de toutes les commandes DiskRAID.

#### <a name="syntax"></a>Syntaxe

```
help
```

### <a name="BKMK_14"></a>importtarget

Récupère ou définit la cible d’importation Service VSS (VSS) actuelle qui est définie pour le sous-système actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
importtarget subsystem [set target]
```

#### <a name="parameter"></a>Paramètre

**définir la cible**

Si ce paramètre est spécifié, définit la cible actuellement sélectionnée sur la cible d’importation VSS pour le sous-système actuellement sélectionné. S’il n’est pas spécifié, la commande récupère la cible d’importation VSS actuelle définie pour le sous-système actuellement sélectionné.

### <a name="BKMK_15"></a>initiateur

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

Définit la stratégie d’équilibrage de charge sur le numéro d’unité logique actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

#### <a name="parameters"></a>Paramètres

**type**

Spécifie la stratégie d’équilibrage de charge. Si le type n’est pas spécifié, le paramètre **path** doit être spécifié. Le type peut être l’un des suivants :

**Basculement**: utilise un chemin d’accès principal avec d’autres chemins d’accès de sauvegarde.

**RoundRobin**: utilise tous les chemins d’accès en mode tourniquet (Round Robin), qui essaie chaque chemin d’accès séquentiellement.

**SUBSETROUNDROBIN**: utilise tous les chemins d’accès principaux en mode tourniquet (Round Robin). les chemins de sauvegarde sont utilisés uniquement en cas d’échec de tous les chemins principaux.

**DYNLQD**: utilise le chemin d’accès avec le moins de demandes actives.

**Weighted**: utilise le chemin d’accès avec le poids le plus faible (un poids doit être affecté à chaque chemin d’accès).

**LEASTBLOCKS**: utilise le chemin d’accès avec les blocs les plus faibles.

**VENDORSPECIFIC**: utilise une stratégie spécifique au fournisseur.

**trajet**

Spécifie si un chemin d’accès est **principal** ou a un > de poids \<particulier. Les chemins d’accès non spécifiés sont implicitement définis comme sauvegarde. Tous les chemins d’accès figurant dans la liste doivent être l’un des chemins d’accès de l’unité logique actuellement sélectionnée.

### <a name="BKMK_19"></a>tarifs

Affiche la liste des objets du type spécifié.

#### <a name="syntax"></a>Syntaxe

```
List {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

#### <a name="parameters"></a>Paramètres

**hbaports**

Répertorie des informations récapitulatives sur tous les ports HBA connus du VDS. Le port HBA actuellement sélectionné est marqué d’un astérisque (*).

**iadapters**

Répertorie des informations récapitulatives sur tous les adaptateurs d’initiateur iSCSI connus du VDS. L’adaptateur de l’initiateur actuellement sélectionné est marqué d’un astérisque (*).

**iportals**

Répertorie des informations récapitulatives sur tous les portails de l’initiateur iSCSI dans la carte initiatrice actuellement sélectionnée. Le portail initiateur actuellement sélectionné est marqué d’un astérisque (*).

**éditeurs**

Répertorie des informations récapitulatives sur chaque fournisseur connu de VDS. Le fournisseur actuellement sélectionné est marqué d’un astérisque (*).

**sous-systèmes**

Répertorie des informations récapitulatives sur chaque sous-système du système. Le sous-système actuellement sélectionné est marqué par un astérisque (*).

**secondaires**

Répertorie des informations récapitulatives sur chaque contrôleur dans le sous-système actuellement sélectionné. Le contrôleur actuellement sélectionné est marqué d’un astérisque (*).

**utilis**

Répertorie des informations récapitulatives sur chaque port de contrôleur dans le contrôleur actuellement sélectionné. Le port actuellement sélectionné est marqué d’un astérisque (*).

**durs**

Répertorie des informations récapitulatives sur chaque lecteur dans le sous-système actuellement sélectionné. Le lecteur actuellement sélectionné est marqué d’un astérisque (*).

**d**

Répertorie des informations récapitulatives sur chaque numéro d’unité logique dans le sous-système actuellement sélectionné. Le numéro d’unité logique actuellement sélectionné est marqué d’un astérisque (*).

**tportals**

Répertorie des informations récapitulatives sur tous les portails cibles iSCSI dans le sous-système actuellement sélectionné. Le portail cible actuellement sélectionné est marqué d’un astérisque (*).

**compilé**

Répertorie des informations récapitulatives sur toutes les cibles iSCSI dans le sous-système actuellement sélectionné. La cible actuellement sélectionnée est marquée par un astérisque (*).

**tpgroups**

Répertorie des informations récapitulatives sur tous les groupes de portails cibles iSCSI dans la cible actuellement sélectionnée. Le groupe de portails actuellement sélectionné est marqué d’un astérisque (*).

### <a name="BKMK_20"></a>connexion

Journalise l’adaptateur d’initiateur iSCSI spécifié dans la cible iSCSI actuellement sélectionnée.

#### <a name="syntax"></a>Syntaxe

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

#### <a name="parameters"></a>Paramètres

**type**

Spécifie le type de connexion à effectuer : **Manuel**, **persistant**ou de **démarrage**. S’il n’est pas spécifié, une connexion manuelle est effectuée.

**Manuel** : Connectez-vous manuellement.

**persistent** -utiliser automatiquement la même connexion lorsque l’ordinateur est redémarré.

**Boot** -(cette option est destinée au développement futur et n’est pas utilisée actuellement<em>.</em>)

**chap**

Spécifie le type d’authentification CHAP à utiliser : **aucun**, **OneWay** chap ou CHAP **mutuel** . s’il n’est pas spécifié, aucune authentification n’est utilisée.

**TPORTAL**

Spécifie un portail cible facultatif dans le sous-système actuellement sélectionné à utiliser pour la connexion.

**iportal**

Spécifie un portail initiateur facultatif dans la carte initiatrice spécifiée à utiliser pour la connexion.

indicateur de \<>

Identifié par trois caractères :

**Adresses IP**: exiger IPSec

**EMP**: activer Multipath

**EHD**: activer le résumé d’en-tête

**EDD**: activer le résumé des données

### <a name="BKMK_21"></a>déconnexion

Journalise la carte d’initiateur iSCSI spécifiée à partir de la cible iSCSI actuellement sélectionnée.

#### <a name="syntax"></a>Syntaxe

```
logout target iadapter= <iadapter>
```

#### <a name="parameters"></a>Paramètres

**iadapter**

Spécifie l’adaptateur de l’initiateur avec une session de connexion à partir de laquelle se déconnecter.

### <a name="BKMK_22"></a>jour

Effectue des opérations de maintenance sur l’objet actuellement sélectionné du type spécifié.

#### <a name="syntax"></a>Syntaxe

```
maintenance <object operation> [count=<iteration>]
```

#### <a name="parameters"></a>Paramètres

objet \<>

Spécifie le type d’objet sur lequel effectuer l’opération. Le type d' *objet* peut être un **sous-système**, un **contrôleur**, un **port, un lecteur** ou un numéro d' **unité logique**.

opération de \<>

Spécifie l’opération de maintenance à effectuer. Le type d' *opération* peut être **spinup**, **SpinDown**, **Blink**, **Beep** ou **ping**. Une *opération* doit être spécifiée.

**nombre =**

Spécifie le nombre de répétitions de l' *opération*. Cette fonction est généralement utilisée avec **Blink**, **Beep**ou **ping**.

### <a name="BKMK_23"></a>nomme

Définit le nom convivial du sous-système, de l’unité logique ou de la cible iSCSI actuellement sélectionné (e) sur le nom spécifié.

#### <a name="syntax"></a>Syntaxe

```
name {subsystem | lun | target} [<name>]
```

#### <a name="parameter"></a>Paramètre

\<name>

Spécifie un nom pour le sous-système, le numéro d’unité logique ou la cible. La longueur du nom doit être inférieure à 64 caractères. Si aucun nom n’est fourni, le nom existant, le cas échéant, est supprimé.

### <a name="BKMK_24"></a>hors connexion

Définit l’état de l’objet actuellement sélectionné du type spécifié sur **hors connexion**.

#### <a name="syntax"></a>Syntaxe

```
offline <object>
```

#### <a name="parameter"></a>Paramètre

objet \<>

Spécifie le type d’objet sur lequel effectuer cette opération. Objet \<>

le type peut être **sous-système**, **contrôleur**, **lecteur**, **numéro d’unité logique**ou **TPORTAL**.

### <a name="BKMK_25"></a>service

Affecte **en ligne**l’état de l’objet sélectionné du type spécifié. Si l’objet est **HBAPORT**, modifie l’état des chemins d’accès au port HBA actuellement sélectionné **en ligne**.

#### <a name="syntax"></a>Syntaxe

```
online <object> 
```

#### <a name="parameter"></a>Paramètre

objet \<>

Spécifie le type d’objet sur lequel effectuer cette opération. Objet \<>

le type peut être **HBAPORT**, **Subsystem**, **Controller**, **Drive**, **lun**ou **TPORTAL**.

### <a name="BKMK_26"></a>récupérer

Effectue les opérations nécessaires, telles que la resynchronisation ou le remplacement à chaud, pour réparer le numéro d’unité logique tolérante aux pannes actuellement sélectionné. Par exemple, la récupération peut entraîner la liaison d’un disque de rechange à chaud à un ensemble RAID dont l’allocation de disque ou d’extension de disque a échoué.

#### <a name="syntax"></a>Syntaxe

```
recover <lun>
```

### <a name="BKMK_27"></a>réénumérer

Réénumère les objets du type spécifié. Si vous utilisez la commande extend LUN, vous devez utiliser la commande Actualiser pour mettre à jour la taille du disque avant d’utiliser la commande réénumérer.

#### <a name="syntax"></a>Syntaxe

```
reenumerate {subsystems | drives}
```

#### <a name="parameters"></a>Paramètres

**sous-systèmes**

Interroge le fournisseur pour découvrir les nouveaux sous-systèmes qui ont été ajoutés dans le fournisseur actuellement sélectionné.

**durs**

Interroge les bus d’e/s internes pour détecter les nouveaux lecteurs qui ont été ajoutés dans le sous-système actuellement sélectionné.

### <a name="BKMK_28"></a>générer

Actualise les données internes du fournisseur actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
refresh provider
```

### <a name="BKMK_29"></a>livr

Utilisé pour commenter des scripts.

#### <a name="syntax"></a>Syntaxe

```
Rem <comment>
```

### <a name="BKMK_30"></a>Installez

Supprime le portail cible iSCSI spécifié du groupe de portails cible actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
remove tpgroup tportal=<tportal> [noerr]
```

#### <a name="parameter"></a>Paramètre

**TPGROUP TPORTAL =** \<TPORTAL >

Spécifie le portail cible iSCSI à supprimer.

**noerr**

Spécifie que les échecs qui se produisent lors de l’exécution de cette opération doivent être ignorés. Cela est utile en mode script.

### <a name="BKMK_31"></a>lieu

Remplace le lecteur spécifié par le lecteur actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
replace drive=<drive_number>
```

#### <a name="parameter"></a>Paramètre

**lecteur =**

Spécifie le > de drive_number \<pour le lecteur à remplacer.

#### <a name="remarks"></a>Notes

-   Le lecteur spécifié n’est peut-être pas le lecteur actuellement sélectionné.

### <a name="BKMK_32"></a>initialisation

Réinitialise le contrôleur ou le port actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
Reset {controller | port}
```

#### <a name="parameters"></a>Paramètres

**SideWinder**

Réinitialise le contrôleur.

**importer**

Réinitialise le port.

### <a name="BKMK_33"></a>sélectionné

Affiche ou modifie l’objet actuellement sélectionné.

#### <a name="syntax"></a>Syntaxe

```
Select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

#### <a name="parameters"></a>Paramètres

**dessin**

Spécifie le type d’objet à sélectionner. Le type de > objet \<peut être **Provider**, **Subsystem**, **Controller**, **Drive**ou **lun**.

**HBAPORT** [\<n >]

Définit le focus sur le port de l’adaptateur de bus hôte local spécifié. Si aucun port HBA n’est spécifié, la commande affiche le port HBA actuellement sélectionné (le cas échéant). Si vous spécifiez un index de port HBA non valide, le port HBA est inactif. La sélection d’un port HBA désélectionne les adaptateurs initiateurs et les portails de l’initiateur sélectionnés.

**IADAPTER** [\<n >]

Définit le focus sur l’adaptateur d’initiateur iSCSI local spécifié. Si aucun adaptateur d’initiateur n’est spécifié, la commande affiche la carte initiatrice actuellement sélectionnée (le cas échéant). La spécification d’un index d’adaptateur initiateur non valide entraîne l’absence d’un adaptateur initiateur en cours. La sélection d’un adaptateur d’initiateur désélectionne les ports HBA et les portails de l’initiateur sélectionnés.

**IPORTAL** [\<n >]

Définit le focus sur le portail de l’initiateur iSCSI local spécifié au sein de la carte de l’initiateur iSCSI sélectionnée. Si aucun portail initiateur n’est spécifié, la commande affiche le portail initiateur actuellement sélectionné (le cas échéant). Si vous spécifiez un index du portail de l’initiateur non valide, aucun portail initiateur n’est sélectionné.

**fournisseur** [\<n >]

Définit le focus sur le fournisseur spécifié. Si aucun fournisseur n’est spécifié, la commande affiche le fournisseur actuellement sélectionné (le cas échéant). La spécification d’un index de fournisseur non valide entraîne l’absence de fournisseur en cours.

**sous-système** [\<n >]

Définit le focus sur le sous-système spécifié. Si aucun sous-système n’est spécifié, la commande affiche le sous-système avec le focus (le cas échéant). La spécification d’un index de sous-système non valide n’entraîne pas de sous-système in-focus. La sélection d’un sous-système sélectionne implicitement son fournisseur associé.

**contrôleur** [\<n >]

Définit le focus sur le contrôleur spécifié dans le sous-système actuellement sélectionné. Si aucun contrôleur n’est spécifié, la commande affiche le contrôleur actuellement sélectionné (le cas échéant). Si vous spécifiez un index de contrôleur non valide, aucun contrôleur n’est actif. La sélection d’un contrôleur désélectionne les ports de contrôleur, les lecteurs, les numéros d’unités logiques, les portails cibles, les cibles et les groupes de portails cibles sélectionnés.

**port** [\<n >]

Définit le focus sur le port de contrôleur spécifié au sein du contrôleur actuellement sélectionné. Si aucun port n’est spécifié, la commande affiche le port actuellement sélectionné (le cas échéant). Si vous spécifiez un index de port non valide, aucun port n’est sélectionné.

**lecteur** [\<n >]

Définit le focus sur le lecteur spécifié, ou sur l’axe physique, dans le sous-système actuellement sélectionné. Si aucun lecteur n’est spécifié, la commande affiche le lecteur actuellement sélectionné (le cas échéant). Si vous spécifiez un index de lecteur non valide, aucun lecteur n’est actif. La sélection d’un lecteur désélectionne les contrôleurs, les ports de contrôleur, les numéros d’unités logiques, les portails cibles, les cibles et les groupes de portails cibles sélectionnés.

**lun** [\<n >]

Définit le focus sur le numéro d’unité logique spécifié dans le sous-système actuellement sélectionné. Si aucun numéro d’unité logique n’est spécifié, la commande affiche le numéro d’unité logique actuellement sélectionné (le cas échéant). Si vous spécifiez un index de LUN non valide, aucun numéro d’unité logique n’est sélectionné. La sélection d’un numéro d’unité logique désélectionne les contrôleurs, les ports de contrôleur, les lecteurs, les portails cibles, les cibles et les groupes de portails cibles sélectionnés.

**TPORTAL** [\<n >]

Définit le focus sur le portail cible iSCSI spécifié dans le sous-système actuellement sélectionné. Si aucun portail cible n’est spécifié, la commande affiche le portail cible actuellement sélectionné (le cas échéant). Si vous spécifiez un index de portail cible non valide, aucun portail cible n’est sélectionné. La sélection d’un portail cible désélectionne les contrôleurs, les ports de contrôleur, les lecteurs, les numéros d’unités logiques, les cibles et les groupes de portails cibles.

**cible** [\<n >]

Définit le focus sur la cible iSCSI spécifiée dans le sous-système actuellement sélectionné. Si aucune cible n’est spécifiée, la commande affiche la cible actuellement sélectionnée (le cas échéant). La spécification d’un index cible non valide n’entraîne aucune cible sélectionnée. La sélection d’une cible désélectionne les contrôleurs, les ports de contrôleur, les lecteurs, les numéros d’unités logiques, les portails cibles et les groupes de portails cibles.

**TPGROUP** [\<n >]

Définit le focus sur le groupe de portails cibles iSCSI spécifié au sein de la cible iSCSI actuellement sélectionnée. Si aucun groupe de portails cible n’est spécifié, la commande affiche le groupe de portails cibles actuellement sélectionné (le cas échéant). Si vous spécifiez un index de groupe de portails cible non valide, aucun groupe de portails cibles n’est actif.

[\<n >]

Spécifie le numéro d’objet \<> à sélectionner. Si le <object number> spécifié n’est pas valide, toutes les sélections existantes pour les objets du type spécifié sont effacées. Si aucun <object number> n’est spécifié, l’objet actuel est affiché.

### <a name="BKMK_34"></a>setflag

Définit le lecteur actuellement sélectionné en tant que disque de secours à chaud.

#### <a name="syntax"></a>Syntaxe

```
setflag drive hotspare={true | false}
```

#### <a name="parameters"></a>Paramètres

**true**

Sélectionne le lecteur actuellement sélectionné en tant que disque de secours à chaud.

**false**

Désélectionne le lecteur actuellement sélectionné en tant que disque de secours à chaud.

#### <a name="remarks"></a>Notes

Les disques d’échange à chaud ne peuvent pas être utilisés pour les opérations de liaison de LUN ordinaires. Elles sont réservées à la gestion des erreurs uniquement. Le lecteur ne doit pas être actuellement lié à un numéro d’unité logique existante.

### <a name="BKMK_shrink"></a>réduire

Réduit la taille du numéro d’unité logique sélectionné.

#### <a name="syntax"></a>Syntaxe

```
shrink lun size=<n> [noerr]
```

#### <a name="parameters"></a>Paramètres

**taille =**

Spécifie la quantité d’espace souhaitée en mégaoctets (Mo) pour réduire la taille du numéro d’unité logique. Pour spécifier la taille à l’aide d’autres unités, utilisez l’un des suffixes reconnus (B, Ko, Mo, Go, to et PB) juste après la taille.

**noerr**

Spécifie que les échecs qui se produisent pendant l’exécution de cette opération seront ignorés. Cela est utile en mode script.

### <a name="BKMK_35"></a>secours

Modifie l’état des chemins d’accès au port de l’adaptateur de bus hôte (HBA) actuellement sélectionné en veille.

#### <a name="syntax"></a>Syntaxe

```
standby hbaport
```

#### <a name="parameters"></a>Paramètres

**HBAPORT**

Modifie l’état des chemins d’accès au port de l’adaptateur de bus hôte (HBA) actuellement sélectionné en veille.

### <a name="BKMK_36"></a>Démasquez

Rend les LUN actuellement sélectionnés accessibles à partir des ordinateurs hôtes spécifiés.

#### <a name="syntax"></a>Syntaxe

```
unmask LUN {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

#### <a name="parameters"></a>Paramètres

**all**

Spécifie que le numéro d’unité logique doit être rendu accessible à partir de tous les ordinateurs hôtes. Toutefois, vous ne pouvez pas démasquer le numéro d’unité logique pour toutes les cibles dans un sous-système iSCSI.

> [!IMPORTANT]
> Vous devez déconnecter la cible avant d’exécuter la commande démasquer tout.

**None**

Spécifie que le numéro d’unité logique ne doit pas être accessible à un hôte.

> [!IMPORTANT]
> Vous devez vous déconnecter de la cible avant d’exécuter la commande démasquez le numéro d’unité logique (LUN NONE).

**add**

Spécifie que les ordinateurs hôtes spécifiés doivent être ajoutés à la liste existante des hôtes à partir desquels ce numéro d’unité logique est accessible. Si ce paramètre n’est pas spécifié, la liste des hôtes fournis remplace la liste existante d’hôtes à partir desquels ce numéro d’unité logique est accessible.

**NOM WWN =**

Spécifie une liste de nombres hexadécimaux représentant des noms au niveau du monde à partir desquels le numéro d’unité logique ou les hôtes doivent être rendus accessibles. Pour masquer/démasquer un ensemble spécifique d’ordinateurs hôtes dans un sous-système Fibre Channel, vous pouvez taper une liste séparée par des points-virgules des WWN des ports sur les ordinateurs hôtes qui vous intéressent.

**initiateur =**

Spécifie la liste des initiateurs iSCSI auxquels le numéro d’unité logique actuellement sélectionné doit être rendu accessible. Pour masquer/démasquer un ensemble spécifique d’ordinateurs hôtes dans un sous-système iSCSI, vous pouvez taper une liste de noms d’initiateurs iSCSI séparés par des points-virgules pour les initiateurs sur les ordinateurs hôtes qui vous intéressent.

**supprimer**

Si ce paramètre est spécifié, désinstalle le disque associé au numéro d’unité logique sur le système local avant le masquage du numéro d’unité logique.

## <a name="scripting-diskraid"></a>Scripts DiskRAID

DiskRAID peut être exécuté sur n’importe quel ordinateur exécutant Windows Server 2008 ou Windows Server 2003 avec un fournisseur de matériel VDS associé. Pour appeler un script DiskRAID, à l’invite de commandes, tapez :
```
diskraid /s <script.txt>
```
Par défaut, DiskRAID arrête le traitement des commandes et retourne un code d’erreur en cas de problème dans le script. Pour continuer à exécuter le script et ignorer les erreurs, incluez le paramètre NOERR sur la commande. Cela permet d’utiliser un script unique pour supprimer tous les numéros d’unités logiques d’un sous-système, quel que soit le nombre total de numéros d’unités logiques. Toutes les commandes ne prennent pas en charge le paramètre NOERR. Les erreurs sont toujours renvoyées dans les erreurs de syntaxe de commande, que vous ayez inclus le paramètre NOERR,

### <a name="diskraid-error-codes"></a>Codes d’erreur DiskRAID

|Code d'erreur|Description de l'erreur|
|----------|-----------------|
|0|Aucune erreur ne s’est produite. L’ensemble du script s’est exécuté sans échec.|
|1|Une exception irrécupérable s’est produite.|
|2|Les arguments spécifiés sur une ligne de commande DiskRAID sont incorrects.|
|3|DiskRAID n’a pas pu ouvrir le script ou le fichier de sortie spécifié.|
|4|L’un des services que DiskRAID utilise a renvoyé une erreur.|
|5|Une erreur de syntaxe de commande s’est produite. Le script a échoué, car un objet n’a pas été correctement sélectionné ou n’était pas valide pour une utilisation avec cette commande.|

## <a name="example-interactively-view-status-of-subsystem"></a>Exemple : affichage interactif de l’état du sous-système

Si vous souhaitez afficher l’état du sous-système 0 sur votre ordinateur, tapez la commande suivante à partir de la ligne de commande :
```
diskraid
```
Appuyez sur ENTRÉE. Les éléments suivants s’affichent :
```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```
Pour sélectionner le sous-système 0, tapez la commande suivante à l’invite DiskRAID :
```
select subsystem 0
```
Appuyez sur ENTRÉE. Une sortie similaire à ce qui suit s’affiche :
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
Pour quitter DiskRAID, tapez la commande suivante à l’invite DiskRAID :
```
exit
```