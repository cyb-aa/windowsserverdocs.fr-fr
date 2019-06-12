---
title: gpfixup
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b145410-fc75-4526-932d-f16b7ee3aaef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: efb30e243d9c165fdcf13943225eb90d38235070
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438213"
---
# <a name="gpfixup"></a>gpfixup



Résoudre les dépendances de nom de domaine dans les liens des objets de stratégie de groupe et de stratégie de groupe après le changement de nom un domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
Gpfixup [/v] 
[/olddns:<OLDDNSNAME> /newdns:<NEWDNSNAME>] 
[/oldnb:<OLDFLATNAME> /newnb:<NEWFLATNAME>] 
[/dc:<DCNAME>] [/sionly] 
[/user:<USERNAME> [/pwd:{<PASSWORD>|*}]] [/?]
```

### <a name="parameters"></a>Paramètres

|       Paramètre       |                                                                                                                                                                                                                               Description                                                                                                                                                                                                                               |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /v           |                                                                                                                                                      Affiche des messages de statut détaillés.</br>Si ce paramètre n’est pas utilisé, seuls les messages d’erreur ou un message d’état récapitulatif de **réussite** ou **échec** s’affiche.                                                                                                                                                       |
| /olddns :\<OLDDNSNAME > |                                                                                                           Spécifie l’ancien nom DNS du domaine renommé en tant que  *\<OLDDNSNAME >* lorsque l’opération de changement de nom de domaine change le nom DNS d’un domaine. Vous pouvez utiliser ce paramètre uniquement si vous utilisez également le **/newdns** paramètre pour spécifier un nouveau nom DNS de domaine.                                                                                                            |
| /newdns :\<NEWDNSNAME > |                                                                                                          Spécifie le nouveau nom DNS du domaine renommé en tant que  *\<NEWDNSNAME >* lorsque l’opération de changement de nom de domaine change le nom DNS d’un domaine. Vous pouvez utiliser ce paramètre uniquement si vous utilisez également le **/olddns** paramètre pour spécifier l’ancien nom DNS de domaine.                                                                                                           |
| /oldnb:\<OLDFLATNAME> |                                                                                                        Spécifie l’ancien nom NetBIOS du domaine renommé en tant que  *\<OLDFLATNAME >* lorsque l’opération de changement de nom de domaine change le nom NetBIOS d’un domaine. Vous pouvez utiliser ce paramètre uniquement si vous utilisez le **/newnb** paramètre pour spécifier un nouveau nom de domaine NetBIOS.                                                                                                        |
| /newnb :\<NEWFLATNAME > |                                                                                                       Spécifie le nouveau nom NetBIOS du domaine renommé en tant que  *\<NEWFLATNAME >* lorsque l’opération de changement de nom de domaine change le nom NetBIOS d’un domaine. Vous pouvez utiliser ce paramètre uniquement si vous utilisez le **/oldnb** paramètre pour spécifier l’ancien nom de domaine NetBIOS.                                                                                                       |
|     / DC :\<DCNAME >     | Se connecter au contrôleur de domaine nommé  *\<DCNAME >* (un nom DNS ou un nom NetBIOS). *\<DCNAME >* doit héberger un réplica accessible en écriture de la partition d’annuaire de domaine comme indiqué par une des opérations suivantes :</br>-Le nom DNS  *\<NEWDNSNAME >* à l’aide de **/newdns**</br>-Le nom NetBIOS  *\<NEWFLATNAME >* à l’aide de **/newnb**</br>Si ce paramètre n’est pas utilisé, vous connecter à n’importe quel contrôleur de domaine du domaine renommé indiqué par  *\<NEWDNSNAME >* ou  *\<NEWFLATNAME >* . |
|        /sionly        |                                                                                                                           Effectue uniquement le correctif de stratégie de groupe qui est lié à l’installation de logiciels gérés (l’extension de l’Installation du logiciel pour la stratégie de groupe). Ignorer les actions permettant de résoudre les liens de stratégie de groupe et les chemins d’accès SYSVOL dans Stratégie de groupe.                                                                                                                           |
|   / User :\<nom d’utilisateur >   |                                                                                                                                   Exécute cette commande dans le contexte de sécurité de l’utilisateur  *\<nom d’utilisateur >* , où  *\<nom d’utilisateur >* est au format domaine\utilisateur.</br>Si ce paramètre n’est pas utilisé, exécute cette commande en tant que l’utilisateur connecté.                                                                                                                                    |
|   / PWD : {\<mot de passe >   |                                                                                                                                                                                                                                   \*}                                                                                                                                                                                                                                   |
|          /?           |                                                                                                                                                                                                                  Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                   |

## <a name="remarks"></a>Notes

-   Le **gpfixup** commande est disponible dans Windows Server 2008 R2 et Windows Server 2008, sauf sur les installations Server Core.
-   Bien que la Console de gestion des stratégies de groupe (GPMC) est distribué avec Windows Server 2008 R2 et Windows Server 2008, vous devez installer la gestion des stratégies de groupe en tant que fonctionnalité via le Gestionnaire de serveur.

## <a name="BKMK_Examples"></a>Exemples

Cet exemple suppose que vous avez déjà effectué une opération de changement de nom de domaine dans lequel vous avez modifié le nom DNS à partir de **MyOldDnsName** à **MyNewDnsName**et le nom NetBIOS dans  **MyOldNetBIOSName** à **MyNewNetBIOSName**. Dans cet exemple, vous utilisez le **gpfixup** commande pour vous connecter au contrôleur de domaine nommé **MyDcDnsName** et réparer les objets stratégie de groupe Stratégie de groupe et des liens en mettant à jour l’ancien nom de domaine incorporé dans les objets stratégie de groupe et les liens. Sortie d’erreur et d’état est enregistré dans un fichier nommé **gpfixup.log**.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```
Cet exemple est identique à celui précédent, sauf qu’elle suppose que le nom NetBIOS du domaine n’a pas été modifié pendant le domaine de changement de nom.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Administration de changement de nom de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=198385)
-   [TechCenter stratégie de groupe](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)