---
title: relog
description: Découvrez comment extraire des informations de compteur de performances à partir des fichiers journaux coutner de performances.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7480f6c0-9953-4d70-9b1c-b27e09d8db13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: daedd85f1557c191a690e7eb750559cfd268d3a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371624"
---
# <a name="relog"></a>relog

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Extrait les compteurs de performance des journaux des compteurs de performance dans d’autres formats, tels que Text-TSV (texte délimité par des tabulations), text-CSV (texte délimité par des virgules), binaire-BIN ou SQL.   

## <a name="syntax"></a>Syntaxe  
```  
relog [<FileName> [<FileName> ...]] [/a] [/c <path> [<path> ...]] [/cf <FileName>] [/f  {bin|csv|tsv|SQL}] [/t <Value>] [/o {OutputFile|DSN!CounterLog}] [/b <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/e <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/config {<FileName>|i}] [/q]  
```  

### <a name="parameters"></a>Paramètres  

|                                         Paramètre                                          |                                                                                                                                                                  Description                                                                                                                                                                   |
|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                *Nom de fichier* [*nom de fichier...* ]                                 |                                                                                                                      Spécifie le chemin d’accès d’un journal des compteurs de performances existant. Vous pouvez spécifier plusieurs fichiers d’entrée.                                                                                                                      |
|                                             -a                                             |                                                                                                          Ajoute le fichier de sortie au lieu de le remplacer. Cette option ne s’applique pas au format SQL où la valeur par défaut est toujours Append.                                                                                                           |
|                                   -c *chemin d'* accès [*chemin...* ]                                   |                                                       Spécifie le chemin d’accès du compteur de performance à enregistrer. Pour spécifier plusieurs chemins d’accès aux compteurs, séparez-les par un espace et placez les chemins d’accès des compteurs entre guillemets (par exemple, **«** <em>Counterpath1</em> <em>Counterpath2</em> **»** )                                                       |
|                                       -CF *nom du fichier*                                       |                                            Spécifie le chemin d’accès du fichier texte qui répertorie les compteurs de performance à inclure dans un fichier relog. Utilisez cette option pour répertorier les chemins de compteur dans un fichier d’entrée, un par ligne. Le paramètre par défaut est tous les compteurs du fichier journal d’origine sont reconnectés.                                            |
|                                  -f {bin\| CSV\|TSV\|SQL}                                  |                                       Spécifie le chemin d’accès au format du fichier de sortie. Le format par défaut est **bin**. Pour une base de données SQL, le fichier de sortie spécifie le nom de source de données *! CounterLog*. Vous pouvez spécifier l’emplacement de la base de données à l’aide du gestionnaire ODBC pour configurer le nom de source de données (nom du système de base de données).                                        |
|                                         *valeur* -t                                         |                                                                                                           Spécifie des intervalles d’échantillonnage dans les enregistrements «*N*». Comprend tous les nième points de données dans le fichier relog. La valeur par défaut est tous les points de données.                                                                                                           |
| -o {*fichiersortie* \| *"SQL : DSN ! Counter_Log*}, où DSN est un DSN odmc défini sur le système. |                                                   Spécifie le chemin d’accès du fichier de sortie ou de la base de données SQL dans lequel les compteurs seront écrits. <br>Remarque : pour les versions 64 bits et 32 bits de relog. exe, vous devez définir un nom de source de données dans la source de données ODBC (respectivement 64 bits et 32 bits).                                                   |
|                          -b \<*M*/*D*/*aaaa*> [[*hh*:]*mm*:]*SS*                           |                                                                          Spécifie l’heure de début de la copie du premier enregistrement du fichier d’entrée. la date et l’heure doivent être au format exact <em>M</em> **/** <em>D</em> **/** <em>YYYYHH</em> **:** <em>mm</em> **:** <em>SS</em>.                                                                          |
|                          -e \<*M*/*D*/*aaaa*> [[*hh*:]*mm*:]*SS*                           |                                                                           Spécifie l’heure de fin de la copie du dernier enregistrement à partir du fichier d’entrée. la date et l’heure doivent être au format exact <em>M</em> **/** <em>D</em> **/** <em>YYYYHH</em> **:** <em>mm</em> **:** <em>SS</em>.                                                                            |
|                                -config {*FileName* \| *i*}                                 | Spécifie le chemin d’accès du fichier de paramètres qui contient les paramètres de ligne de commande. Utilisez *-i* dans le fichier de configuration en tant qu’espace réservé pour une liste de fichiers d’entrée qui peuvent être placés sur la ligne de commande. Sur la ligne de commande, toutefois, vous n’avez pas besoin d’utiliser *i*. Vous pouvez également utiliser des caractères génériques tels que \*. BLG pour spécifier de nombreux noms de fichiers d’entrée. |
|                                             -q                                             |                                                                                                                          Affiche les compteurs de performances et les plages de temps des fichiers journaux spécifiés dans le fichier d’entrée.                                                                                                                           |
|                                             -y                                             |                                                                                                                                            Ignore les invites en répondant « Oui » à toutes les questions.                                                                                                                                             |
|                                             /?                                             |                                                                                                                                                      Affiche l'aide à l'invite de commandes.                                                                                                                                                      |

## <a name="remarks"></a>Notes  
Format du chemin du compteur :  
- Le format général pour les chemins de compteur est le suivant : [\\\<ordinateur >] \\\<objet > [\<parent >\\< instance # index >] \\\<compteur >] où les composants parent, instance, index et compteur du format peuvent contenir un nom valide ou un caractère générique. Les composants de l’ordinateur, du parent, de l’instance et de l’index ne sont pas nécessaires pour tous les compteurs.  
- Vous déterminez les chemins de compteur à utiliser en fonction du compteur lui-même. Par exemple, l’objet LogicalDisk a une instance <Index>. vous devez donc fournir le < #index > ou un caractère générique. Par conséquent, vous pouvez utiliser le format suivant : **\LogicalDisk (\*/\*#\*)\\** \\*  
- En comparaison, l’objet processus ne nécessite pas d’instance \<index >. Par conséquent, vous pouvez utiliser le format suivant : **\Processus (\*) \ID process**  
- Si un caractère générique est spécifié dans le nom du parent, toutes les instances de l’objet spécifié qui correspondent à l’instance et aux champs de compteur spécifiés sont retournées.  
- Si un caractère générique est spécifié dans le nom de l’instance, toutes les instances de l’objet et de l’objet parent spécifiées sont retournées si tous les noms d’instance correspondant à l’index spécifié correspondent au caractère générique.  
- Si un caractère générique est spécifié dans le nom du compteur, tous les compteurs de l’objet spécifié sont retournés.  
- Les correspondances de chaîne de chemin de compteur partielles (par exemple, Pro *) ne sont pas prises en charge.  

Fichiers de compteur :  
-   Les fichiers de compteur sont des fichiers texte qui répertorient un ou plusieurs compteurs de performance dans le journal existant. Copiez le nom complet du compteur à partir du journal ou de la sortie **/q** dans \<ordinateur >\\\<objet >\\\<>\\\<> compteur. Répertoriez un chemin de compteur sur chaque ligne.  

Copie des compteurs :  
-   Une fois exécuté, **relog** copie les compteurs spécifiés à partir de chaque enregistrement du fichier d’entrée, en convertissant le format si nécessaire. Les chemins d’accès génériques sont autorisés dans le fichier de compteur.  
Enregistrement des sous-ensembles de fichiers d’entrée :  
-   Utilisez le paramètre **/t** pour spécifier que les fichiers d’entrée sont insérés dans les fichiers de sortie à des intervalles de chaque <n>ième enregistrement. Par défaut, les données sont reconnectées à partir de chaque enregistrement.  
Utilisation des paramètres **/b** et **/e** avec les fichiers journaux  
-   Vous pouvez spécifier que vos journaux de sortie incluent les enregistrements avant l’heure de début (c’est-à-dire, **/b**) pour fournir des données pour les compteurs qui nécessitent des valeurs de calcul de la valeur mise en forme. Le fichier de sortie contient les derniers enregistrements des fichiers d’entrée dont les horodateurs sont inférieurs au paramètre **/e** (c’est-à-dire, heure de fin).  
Utilisation de l’option **/config** :  
-   Le contenu du fichier de paramètres utilisé avec l’option **/config** doit avoir le format suivant :  
    -   \<CommandOption >\\\<valeur >, où \<CommandOption > est une option de ligne de commande et \<valeur > spécifie sa valeur.

Pour plus d’informations sur l’incorporation de **relog** dans vos scripts Windows Management Instrumentation (WMI), consultez « Scripting WMI » (en anglais) sur le [site Web des kits de ressources Microsoft Windows](https://go.microsoft.com/fwlink/?LinkId=4665).  

## <a name="BKMK_Examples"></a>Illustre  
Pour rééchantillonner les journaux de suivi existants à intervalles fixes de 30, répertoriez les chemins d’accès des compteurs, les fichiers de sortie et les formats :  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.csv /t 30 /f csv  
```  
Pour rééchantillonner les journaux de suivi existants à intervalles fixes de 30, répertoriez les chemins d’accès des compteurs et le fichier de sortie :  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.blg /t 30  
```
Pour rééchantillonner des journaux de suivi existants dans une base de données, utilisez :
```
relog "c:\perflogs\daily_trace_log.blg" -f sql -o "SQL:sql2016x64odbc!counter_log"
```

## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
