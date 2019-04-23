---
title: relog
description: Découvrez comment extraire des informations sur les compteurs de performances dans les fichiers de journaux d’un compteur de performances.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6804c25af04907edc8180b6a37be7efcc470f259
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869360"
---
# <a name="relog"></a>relog

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Extrait les compteurs de performance des journaux dans d’autres formats, tels que SQL, à l’emplacement du fichier binaire, texte-CSV (pour le texte délimité par des virgules) ou texte-TSV (pour le texte délimité par des tabulations).   

## <a name="syntax"></a>Syntaxe  
```  
relog [<FileName> [<FileName> ...]] [/a] [/c <path> [<path> ...]] [/cf <FileName>] [/f  {bin|csv|tsv|SQL}] [/t <Value>] [/o {OutputFile|DSN!CounterLog}] [/b <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/e <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/config {<FileName>|i}] [/q]  
```  

### <a name="parameters"></a>Paramètres  

|Paramètre|Description|
|--|--|
|*Nom de fichier* [*nom de fichier...* ]|Spécifie le chemin d’accès d’un journal de compteur de performances. Vous pouvez spécifier plusieurs fichiers d’entrée.|
|-a |Ajoute un fichier de sortie au lieu de remplacer. Cette option ne s’applique pas au format SQL où la valeur par défaut est toujours à ajouter.  |
|-c *chemin d’accès* [*chemin d’accès...* ]|Spécifie le chemin d’accès du compteur de performances pour vous connecter. Pour spécifier plusieurs chemins de compteurs, séparez-les par un espace et placez les chemins de compteur entre guillemets (par exemple, **« *** CheminCompteur1* * CheminCompteur2 ***»**)|  
|-cf *FileName*|Spécifie le chemin d’accès du fichier texte qui répertorie les compteurs de performances à inclure dans un fichier rejournaliser. Utilisez cette option pour les chemins de compteur de liste dans un fichier d’entrée, une par ligne. Paramètre par défaut est que tous les compteurs dans le fichier journal d’origine sont replacées dans le journal.|  
|-f {bin\| csv\|tsv\|SQL}|Spécifie le chemin d’accès du format de fichier de sortie. Le format par défaut est **bin**. Pour une base de données SQL, le fichier de sortie spécifie la *DSN ! JournalCompteur*. Vous pouvez spécifier l’emplacement de la base de données à l’aide du gestionnaire ODBC pour configurer le DSN (nom de système de base de données).  |
|-t *valeur*|Spécifie l’intervalle d’échantillon dans «*N*« enregistrements. Inclut chaque point de données nième dans le fichier rejournaliser. Valeur par défaut est chaque point de données.|  
|-o {*OutputFile* \| *« SQL:DSN ! Journal_compteur*} où DSN est un DSN ODMC défini sur le système.|Spécifie le chemin d’accès du fichier de sortie ou de la base de données SQL où les compteurs seront écrits. <br>Remarque: Pour les versions 64 bits et 32 bits de Relog.exe, vous devez définir une source de données dans la Source de données ODBC (64 bits et 32 bits respectivement)|
|-b \< *M*/*D*/*aaaa*> [[*HH*:]*MM*:]*SS*|Spécifie heure de début de la copie du premier enregistrement à partir du fichier d’entrée. date et l’heure doivent être au format exact *M***/*** D***/*** YYYYHH ***:*** MM ***:*** SS*.|  
|e - \< *M*/*D*/*aaaa*> [[*HH*:]*MM*:]*SS* |Spécifie l’heure de fin pour la copie du dernier enregistrement du fichier d’entrée. date et l’heure doivent être au format exact *M***/*** D***/*** YYYYHH ***:*** MM ***:*** SS*.|  
|-config {*FileName* \| *i*}|Spécifie le chemin d’accès du fichier de paramètres qui contient les paramètres de ligne de commande. Utilisez *-i* dans le fichier de configuration comme un espace réservé pour une liste des fichiers d’entrée qui peut être placé sur la ligne de commande. Sur la ligne de commande, toutefois, il est inutile d’utiliser *je*. Vous pouvez également utiliser des caractères génériques tels que *.blg pour spécifier plusieurs noms de fichier d’entrée.|  
|-q|Affiche les compteurs de performances et les périodes de fichiers journaux spécifiés dans le fichier d’entrée.|  
|-y|Contournements invite par si vous répondez « Oui » à toutes les questions.|  
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes  
Format du chemin d’accès de compteur :  
-   Le format général des chemins de compteur est le suivant : [\\\<ordinateur >] \\ \<objet > [\<Parent >\\< Instance #Index >] \\ \< Compteur >] où le parent, instance, index et les composants de compteur du format peuvent contenir un nom valide ou un caractère générique. L’ordinateur, parent, instance et les composants d’index ne sont pas nécessaires pour tous les compteurs.  
-   Vous déterminez les chemins de compteur à utiliser selon le compteur lui-même. Par exemple, l’objet disque logique a une instance <Index>, de sorte que vous devez fournir le < #index > ou un caractère générique. Par conséquent, vous pouvez utiliser le format suivant : **\LogicalDisk (\*/\*#\*)\\\***  
-   En comparaison, l’objet de processus ne nécessite pas une instance \<Index >. Par conséquent, vous pouvez utiliser le format suivant : **\Processus (\*) \ID processus**  
-   Si un caractère générique est spécifié dans le nom du parent, toutes les instances de l’objet spécifié qui correspondent aux champs de compteur et instance spécifiée seront affichera.  
-   Si un caractère générique est spécifié dans le nom d’instance, toutes les instances de l’objet spécifié et l’objet parent seront affichera si tous les noms d’instance correspondant à l’index spécifié correspondent au caractère générique.  
-   Si un caractère générique est spécifié dans le nom du compteur, tous les compteurs de l’objet spécifié sont retournés.  
-   Correspondances de chaîne de chemin d’accès de compteur partielle (par exemple, pro *) ne sont pas pris en charge.  

Fichiers de compteur :  
-   Les fichiers de compteurs sont des fichiers texte qui répertorient un ou plusieurs compteurs de performance dans le journal existant. Copiez le nom complet du compteur à partir du journal ou le **/q** de sortie dans \<ordinateur >\\\<objet >\\\<Instance >\\ \< Compteur > format. chemin d’accès de liste un compteur sur chaque ligne.  

Copie de compteurs :  
-   Lors de l’exécution, **rejournaliser** copie les compteurs spécifiés à partir de chaque enregistrement dans le fichier d’entrée, en convertissant le format si nécessaire. Chemins d’accès génériques sont autorisés dans le fichier de compteur.  
Enregistrement des sous-ensembles de fichier d’entrée :  
-   Utilisez le **/t** fichiers à des intervalles de sortie du paramètre pour spécifier que les fichiers d’entrée sont insérées dans chaque <n>enregistrement de th. Par défaut, data est replacées dans le journal à partir de chaque enregistrement.  
À l’aide de **/b** et **/e** paramètres avec les fichiers journaux  
-   Vous pouvez spécifier que vos journaux de sortie comprendre des enregistrements d’avant l’heure de début (autrement dit, **/b**) pour fournir des données pour les compteurs qui requièrent des valeurs de calcul de la valeur mise en forme. Le fichier de sortie auront les derniers enregistrements à partir des fichiers d’entrée avec un cachet temporel inférieure à la **/e** (autrement dit, heure de fin) paramètre.  
À l’aide de la **/config** option :  
-   Le contenu du fichier de paramètres utilisé avec la **/config** option doit être au format suivant :  
    -   \<OptionCommande >\\\<valeur >, où \<OptionCommande > est une option de ligne de commande et \<valeur > spécifie sa valeur.

Pour plus d’informations sur l’incorporation de **rejournaliser** dans vos scripts Windows Management Instrumentation (WMI), consultez « Écriture de scripts WMI » à la [site Web des Kits de ressources Microsoft Windows](https://go.microsoft.com/fwlink/?LinkId=4665).  

## <a name="BKMK_Examples"></a>Exemples  
Pour rééchantillonner les journaux de suivi existants à intervalles fixes de 30, liste des chemins de compteur, fichiers et les formats de sortie :  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.csv /t 30 /f csv  
```  
Pour rééchantillonner les journaux de suivi existants à intervalles fixes de 30, la liste des chemins de compteur et le fichier de sortie :  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.blg /t 30  
```
Pour rééchantillonner les journaux de suivi existants dans une base de données, utilisez :
```
relog "c:\perflogs\daily_trace_log.blg" -f sql -o "SQL:sql2016x64odbc!counter_log"
```

## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  
<!---
-   The following is a list of the possible formats:  
    -   \<computer>\\\<Object>(\<Parent>/\<Instance#Index>)\<Counter>  
    -   \<computer>\<Object>(<Parent>/<Instance>)\\<Counter>  
    -   \\\\<computer>\\<Object>(<Instance#Index>)\\<Counter>  
    -   \\\\<computer>\\<Object>(<Instance>)\\<Counter>  
    -   \\\\<computer>\\<Object>\\<Counter>  
    -   \\<Object>(<Parent>/<Instance#Index>)\\<Counter>  
    -   \\<Object>(<Parent>/<Instance>)<Counter>  
    -   \\<Object>(<Instance#Index>)\\<Counter>  
    -   \\<Object>(<Instance>)\\<Counter>  
    -   \\<Object>\\<Counter>  
--->