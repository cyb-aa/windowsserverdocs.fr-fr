---
title: gpfixup
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e32427369f1664476c81a81353ae8869ec0c2ff3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375672"
---
# <a name="gpfixup"></a>gpfixup



Corrigez les dépendances de nom de domaine dans stratégie de groupe objets et stratégie de groupe liens après une opération de modification de nom de domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

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
|          /v           |                                                                                                                                                      Affiche des messages d’État détaillés.</br>Si ce paramètre n’est pas utilisé, seuls les messages d’erreur ou un message d’état récapitulatif de **réussite** ou d' **échec** s’affichent.                                                                                                                                                       |
| /olddns : \<OLDDNSNAME > |                                                                                                           Spécifie l’ancien nom DNS du domaine renommé en tant que *\<OLDDNSNAME >* lorsque l’opération de changement de nom de domaine modifie le nom DNS d’un domaine. Vous pouvez utiliser ce paramètre uniquement si vous utilisez également le paramètre **/newdns** pour spécifier un nouveau nom DNS de domaine.                                                                                                            |
| /newdns : \<NEWDNSNAME > |                                                                                                          Spécifie le nouveau nom DNS du domaine renommé en tant que *\<NEWDNSNAME >* lorsque l’opération de changement de nom de domaine modifie le nom DNS d’un domaine. Vous pouvez utiliser ce paramètre uniquement si vous utilisez également le paramètre **/olddns** pour spécifier l’ancien nom DNS du domaine.                                                                                                           |
| /oldnb : \<OLDFLATNAME > |                                                                                                        Spécifie l’ancien nom NetBIOS du domaine renommé en tant que *\<OLDFLATNAME >* lorsque l’opération de changement de nom de domaine modifie le nom NetBIOS d’un domaine. Vous pouvez utiliser ce paramètre uniquement si vous utilisez le paramètre **/newnb** pour spécifier un nouveau nom NetBIOS de domaine.                                                                                                        |
| /newnb : \<NEWFLATNAME > |                                                                                                       Spécifie le nouveau nom NetBIOS du domaine renommé en tant que *\<NEWFLATNAME >* lorsque l’opération de changement de nom de domaine modifie le nom NetBIOS d’un domaine. Vous pouvez utiliser ce paramètre uniquement si vous utilisez le paramètre **/oldnb** pour spécifier l’ancien nom NetBIOS du domaine.                                                                                                       |
|     /DC : \<DCNAME >     | Connectez-vous au contrôleur de domaine nommé *\<DCNAME >* (nom DNS ou nom NetBIOS). *\<DCNAME >* devez héberger un réplica accessible en écriture de la partition d’annuaire du domaine comme indiqué par l’une des opérations suivantes :</br>-Le nom DNS *\<NEWDNSNAME >* à l’aide de **/newdns**</br>-Le nom NetBIOS *\<NEWFLATNAME >* à l’aide de **/newnb**</br>Si ce paramètre n’est pas utilisé, connectez-vous à n’importe quel contrôleur de domaine dans le domaine renommé indiqué par *\<NEWDNSNAME >* ou *\<NEWFLATNAME >* . |
|        /sionly        |                                                                                                                           Exécute uniquement le correctif stratégie de groupe qui concerne l’installation du logiciel géré (l’extension d’installation de logiciel pour stratégie de groupe). Ignorez les actions qui corrigent stratégie de groupe liens et les chemins d’accès SYSVOL dans les objets de stratégie de groupe.                                                                                                                           |
|   /User : \<USERNAME >   |                                                                                                                                   Exécute cette commande dans le contexte de sécurité de l’utilisateur *\<USERNAME >* , où *\<USERNAME >* est au format domaine\utilisateur.</br>Si ce paramètre n’est pas utilisé, exécute cette commande en tant qu’utilisateur connecté.                                                                                                                                    |
|   /PWD : {@no__t 0PASSWORD >   |                                                                                                                                                                                                                                   \*}                                                                                                                                                                                                                                   |
|          /?           |                                                                                                                                                                                                                  Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                   |

## <a name="remarks"></a>Notes

-   La commande **gpfixup** est disponible dans windows Server 2008 R2 et windows Server 2008, sauf sur les installations Server Core.
-   Bien que la Console de gestion des stratégies de groupe (GPMC) soit distribuée avec Windows Server 2008 R2 et Windows Server 2008, vous devez installer la gestion des stratégie de groupe en tant que fonctionnalité via Gestionnaire de serveur.

## <a name="BKMK_Examples"></a>Illustre

Cet exemple suppose que vous avez déjà effectué une opération de changement de nom de domaine dans laquelle vous avez modifié le nom DNS de **MyOldDnsName** en **MyNewDnsName**, et le nom NetBIOS de **MyOldNetBIOSName** à **MyNewNetBIOSName**. Dans cet exemple, vous utilisez la commande **gpfixup** pour vous connecter au contrôleur de domaine nommé **MyDcDnsName** et réparer les objets de stratégie de groupe et les liens de stratégie de groupe en mettant à jour l’ancien nom de domaine incorporé dans les objets de stratégie de groupe et les liens. L’État et la sortie d’erreur sont enregistrés dans un fichier nommé **gpfixup. log**.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```
Cet exemple est identique au précédent, à ceci près qu’il suppose que le nom NetBIOS du domaine n’a pas été modifié lors de l’opération de changement de nom de domaine.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Administration domaine Active Directory renommer](https://go.microsoft.com/fwlink/?LinkId=198385)
-   [TechCenter stratégie de groupe](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)