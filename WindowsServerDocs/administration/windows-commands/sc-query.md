---
title: Requête SC
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac365f89-4b20-4de6-a582-b204c5e7d0eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ed9ee813e3ea41f24ce5c012eecd278ef051138
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879290"
---
# <a name="sc-query"></a>Requête SC



Obtient et affiche des informations sur le service spécifié, le pilote, le type de service ou le type de pilote.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
sc [<ServerName>] query [<ServiceName>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <BufferSize>] [ri= <ResumeIndex>] [group= <GroupName>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ServerName>|Spécifie le nom du serveur distant sur lequel se trouve le service. Le nom doit utiliser le format UNC Universal Naming Convention () (par exemple, \\ \\myserver). Pour exécuter SC.exe localement, omettez ce paramètre.|
|\<ServiceName>|Spécifie le nom de service retourné par la **getkeyname** opération. Cela **requête** paramètre n’est pas utilisé conjointement avec d’autres **requête** paramètres (autre que *nom_serveur*).|
|type= {driver | service | all}|Spécifie les éléments à énumérer. La valeur par défaut pour le premier type est **service**.</br>-pilote : Spécifie que seuls les pilotes sont énumérés.</br>-service : Spécifie que seuls les services sont énumérés.</br>-toutes les : Spécifie que les pilotes et les services sont énumérés.|
|type= {own | partager | interagir | kernel | filesys | rec | adapt}|Spécifie le type de services ou de pilote à énumérer. La valeur par défaut pour le deuxième type est **propre**.</br>-propre : Spécifie que le service s’exécute dans son propre processus. Il ne partage pas un fichier exécutable avec d’autres services.</br>-partager : Spécifie que le service s’exécute comme un processus partagé. Il partage un fichier exécutable avec d’autres services.</br>-interagir : Spécifie que le service peut interagir avec le bureau, en recevant des données des utilisateurs. Services interactifs doivent être exécutés sous le compte LocalSystem.</br>-noyau : Spécifie un pilote.</br>-filesys : Spécifie un pilote de système de fichiers.|
|état = {active | inactif | all}|Spécifie l’état de prise en main du service à énumérer. L’état par défaut est **active**.</br>-active : Spécifie tous les services actifs.</br>-inactive : Spécifie tous les services interrompus ou arrêtés.</br>-toutes les : Spécifie tous les services.|
|BufSize = \<BufferSize >|Spécifie la taille (en octets) de la mémoire tampon d’énumération. La taille de mémoire tampon par défaut est 1 024 octets. Vous devez augmenter la taille de la mémoire tampon d’énumération lorsque le résultat d’une requête est supérieur à 1 024 octets.|
|RI = \<ResumeIndex >|Spécifie le numéro d’index à laquelle énumération doit commencer ou reprendre. La valeur par défaut est **0** (zéro). Utilisez ce paramètre conjointement avec la **bufsize =** paramètre lorsque plus d’informations sont retournés par une requête de la mémoire tampon par défaut peut afficher.|
|group= \<GroupName>|Spécifie le groupe de service à énumérer. Par défaut, tous les groupes sont énumérées (**groupe = « «**).|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Sans espace entre un paramètre et sa valeur (autrement dit, **type = propre**, et non **type = propre**), l’opération échoue.
-   Le **requête** opération affiche les informations suivantes sur un service : (Nom du service Registre sous-clé), TYPE, l’état (ainsi que les États qui ne sont pas disponibles), WIN32_EXIT_B, SERVICE_EXIT_B, point de contrôle et d’indication d’attente.
-   Le **type =** paramètre peut être utilisé deux fois dans certains cas. La première apparition de la **type =** paramètre spécifie s’il faut interroger les services, les pilotes ou les deux (**tous les**). Le deuxième aspect de la **type =** paramètre spécifie un type à partir de la **créer** opération pour réduire davantage la portée d’une requête.
-   Lors de l’affichage résultant d’une **requête** commande dépasse la taille de la mémoire tampon d’énumération, un message semblable au suivant s’affiche :  
    ```
    Enum: more data, need 1822 bytes start resume at index 79
    ```  
    Pour afficher les autres **requête** plus d’informations, exécutez à nouveau **requête**, la définition **bufsize =** correspondre au nombre d’octets et le paramètre **ri =** à la index spécifié. Par exemple, la sortie restante s’affichera en tapant ce qui suit à l’invite de commandes :  
    ```
    sc query bufsize= 1822 ri= 79
    ```

## <a name="BKMK_examples"></a>Exemples

Pour afficher des informations pour les services active uniquement, tapez une des commandes suivantes :
```
sc query
sc query type= service
```
Pour afficher des informations pour les services actifs et pour spécifier une taille de mémoire tampon de 2 000 octets, tapez :
```
sc query type= all bufsize= 2000
```
Pour afficher des informations pour le service WUAUSERV, tapez :
```
sc query wuauserv
```
Pour afficher des informations pour tous les services (actives et inactives), tapez :
```
sc query state= all
```
Pour afficher des informations pour tous les services (actives et inactives), commençant à la ligne de 56, tapez :
```
sc query state= all ri= 56
```
Pour afficher des informations pour les services interactifs, tapez :
```
sc query type= service type= interact
```
Pour afficher des informations pour les pilotes uniquement, tapez :
```
sc query type= driver
```
Pour afficher des informations pour les pilotes dans le groupe pilote spécification NDIS (Network Interface), tapez :
```
sc query type= driver group= ndis
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)