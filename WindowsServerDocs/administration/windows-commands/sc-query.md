---
title: Requête SC
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ac365f89-4b20-4de6-a582-b204c5e7d0eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a6b4555790a8f9bb1149531251e724189177975
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722184"
---
# <a name="sc-query"></a>Requête SC



Obtient et affiche des informations sur le service, le pilote, le type de service ou le type de pilote spécifié.



## <a name="syntax"></a>Syntaxe

```
sc [<ServerName>] query [<ServiceName>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <BufferSize>] [ri= <ResumeIndex>] [group= <GroupName>]
```

### <a name="parameters"></a>Paramètres

|       Paramètre        |                                                                                                                          Description                                                                                                                          |
|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     \<Nom du serveur>      |                       Spécifie le nom du serveur distant sur lequel se trouve le service. Le nom doit utiliser le format UNC (Universal Naming Convention) (par exemple, \\ \\monserveur). Pour exécuter SC. exe localement, omettez ce paramètre.                        |
|     \<> ServiceName     |                                      Spécifie le nom du service retourné par l’opération **getkeyname** . Ce paramètre de **requête** n’est pas utilisé conjointement avec d’autres paramètres de **requête** (autres que *ServerName*).                                      |
|     type = {Driver      |                                                                                                                            service                                                                                                                            |
|       type = {Own       |                                                                                                                             partager                                                                                                                             |
|     État = {actif     |                                                                                                                           inactive                                                                                                                            |
| bufsize = \<bufferSize> |                     Spécifie la taille (en octets) de la mémoire tampon d’énumération. La taille de la mémoire tampon par défaut est de 1 024 octets. Vous devez augmenter la taille de la mémoire tampon d’énumération lorsque l’affichage résultant d’une requête dépasse 1 024 octets.                      |
|   RI = \<ResumeIndex>   | Spécifie le numéro d’index au niveau duquel l’énumération doit commencer ou reprendre. La valeur par défaut est **0** (zéro). Utilisez ce paramètre avec le paramètre **bufsize =** lorsque des informations supplémentaires sont retournées par une requête que le tampon par défaut peut afficher. |
|  groupe = \<GroupName>   |                                                                             Spécifie le groupe de services à énumérer. Par défaut, tous les groupes sont énumérés (* * groupe = * *).                                                                              |
|           /?           |                                                                                                             Affiche l'aide à l'invite de commandes.                                                                                                              |

## <a name="remarks"></a>Notes 

- Si vous ne disposez pas d’un espace entre un paramètre et sa valeur (autrement dit, **type = Own**, et non **type = Own**), l’opération échoue.
- L’opération de **requête** affiche les informations suivantes sur un service : service_name (nom de la sous-clé de Registre du service), type, État (ainsi que les États qui ne sont pas disponibles), WIN32_EXIT_B, SERVICE_EXIT_B, point de contrôle et WAIT_HINT.
- Le paramètre **type =** peut être utilisé deux fois dans certains cas. La première apparence du paramètre **type =** spécifie s’il faut interroger les services, les pilotes ou les deux (**tout**). La deuxième apparence du paramètre **type =** spécifie un type de l’opération **Create** pour affiner davantage la portée d’une requête.
- Lorsque l’affichage résultant d’une commande de **requête** dépasse la taille de la mémoire tampon d’énumération, un message semblable au suivant s’affiche :  
  ```
  Enum: more data, need 1822 bytes start resume at index 79
  ```  
  Pour afficher les informations de **requête** restantes, réexécutez la **requête**, en définissant **bufsize =** comme nombre d’octets et en définissant **RI =** à l’index spécifié. Par exemple, la sortie restante s’affiche en tapant ce qui suit à l’invite de commandes :  
  ```
  sc query bufsize= 1822 ri= 79
  ```

## <a name="examples"></a>Exemples

Pour afficher uniquement les informations relatives aux services actifs, tapez l’une des commandes suivantes :
```
sc query
sc query type= service
```
Pour afficher des informations sur les services actifs et spécifier une taille de mémoire tampon de 2 000 octets, tapez :
```
sc query type= all bufsize= 2000
```
Pour afficher des informations pour le service WUAUSERV, tapez :
```
sc query wuauserv
```
Pour afficher des informations sur tous les services (actifs et inactifs), tapez :
```
sc query state= all
```
Pour afficher des informations sur tous les services (actifs et inactifs), en commençant à la ligne 56, tapez :
```
sc query state= all ri= 56
```
Pour afficher des informations sur les services interactifs, tapez :
```
sc query type= service type= interact
```
Pour afficher uniquement les informations relatives aux pilotes, tapez :
```
sc query type= driver
```
Pour afficher les informations relatives aux pilotes dans le groupe NDIS (Network Driver Interface Specification), tapez :
```
sc query type= driver group= ndis
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)