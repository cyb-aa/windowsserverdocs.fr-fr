---
title: tracerpt
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4d943da40793be0680d6ea6a4d9de0960f65c72
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861700"
---
# <a name="tracerpt"></a>tracerpt



Le **tracerpt** commande peut être utilisée pour analyser les journaux de suivi d’événements, les fichiers journaux générés par l’Analyseur de performances et les fournisseurs de suivi d’événements en temps réel. Il génère des fichiers de vidage, les fichiers de rapport et les schémas de rapport.

Pour obtenir des exemples montrant comment utiliser **tracerpt**, consultez [exemples](#BKMK_EXAMPLES).

## <a name="syntax"></a>Syntaxe

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>Options

|Indicateur d’option|Description|
|-----------|-----------|
|-?|Affiche sensible au contexte d’aide.|
|-config \<filename>|Charger un fichier de paramètres contenant les options de commande.|
|-y|Répondez Oui à toutes les questions sans demander confirmation.|
|f - \<XML | HTML>|Définir le format de rapport.|
|-de \<CSV | EVTX | XML>|Définir le format de vidage. Valeur par défaut est XML.|
|-df \<filename>|Créer un spécifique à Microsoft comptage/reporting du fichier de schéma.|
|-int \<filename >|Vider la structure des événements interprétées dans le fichier spécifié.|
|-rts|Horodatage brut de rapport dans l’en-tête de trace d’événements. Utilisable uniquement avec -o, - rapport ni ne-résumé.|
|-tmf \<filename>|Spécifiez un fichier de définition de Format de Message de Trace.|
|-tp \<value>|Spécifiez le chemin de recherche de fichiers TMF. Plusieurs chemins d’accès peuvent être utilisés, séparés par un point-virgule ( ;).|
|-i \<value>|Spécifiez le chemin d’image de fournisseur. Le fichier PDB correspondant sera situé dans le serveur de symboles. Plusieurs chemins d’accès peuvent être utilisés, séparés par un point-virgule ( ;).|
|-pdb \<value>|Spécifiez le chemin d’accès du serveur de symbole. Plusieurs chemins d’accès peuvent être utilisés, séparés par un point-virgule ( ;).|
|-gmt|Conversion des horodatages de charge utile WPP Greenwich Mean Time.|
|-rl \<value>|Définir le niveau de rapport système à partir de 1 à 5. Valeur par défaut est 1.|
|-Résumé [nom_fichier]|Générer un fichier texte de rapport de synthèse. Nom de fichier si non spécifié est summary.txt.|
|-o [nom_fichier]|Générer un fichier de sortie de texte. Nom de fichier si non spécifié est dumpfile.xml.|
|-rapports [nom_fichier]|Générer un fichier de rapport de sortie de texte. Nom de fichier si non spécifié est workload.xml.|
|-lr|Spécifiez « moins restrictif. » Cet exemple utilise meilleur effort pour les événements qui ne correspondent pas le schéma d’événements.|
|-Exporter [nom_fichier]|Générer un fichier d’exportation de schéma d’événement. Nom de fichier si non spécifié est : Schema.man.|
|[-l]. \<valeur [valeur [...]] >|Spécifiez le fichier journal de suivi d’événements à traiter.|
|-rt \<session_name [session_name [...]] >|Spécifier les sources de données de Session de Trace d’événements en temps réel.|

## <a name="BKMK_EXAMPLES"></a>Exemples

-   Cet exemple crée un rapport basé sur les deux journaux des événements **logfile1.etl** et **logfile2.etl** et crée le fichier de vidage **logdump.xml** au format XML.  
    ```
    tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
    ```  
-   Cet exemple crée un rapport basé sur le journal des événements **journal.etl**, crée le fichier de vidage **logdmp.xml** au format XML, utilise un meilleur effort pour identifier les événements pas dans le schéma, produit un fichier de rapport de synthèse **logdump.txt**et produit le fichier de rapport **logrpt.xml**.  
    ```
    tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
    ```  
-   Cet exemple utilise les deux journaux des événements **logfile1.etl** et **logfile2.etl** pour produire un fichier de vidage et le fichier de rapport avec les noms de fichiers par défaut.  
    ```
    tracerpt logfile1.etl logfile2.etl -o -report
    ```  
-   Cet exemple utilise le journal des événements **journal.etl** et le journal des performances **counterfile.blg** pour produire le fichier de rapport **logrpt.xml** et le schéma XML de spécifique à Microsoft fichier **schema.xml**.  
    ```
    tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
    ```  
-   Cet exemple lit la Session en temps réel de Trace d’événements « NT Kernel Logger » et génère le fichier de vidage **journal.csv** au format CSV.  
    ```
    tracerpt -rt "NT Kernel Logger" -o logfile.csv -of CSV
    ```