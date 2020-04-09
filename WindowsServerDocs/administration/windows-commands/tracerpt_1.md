---
title: tracerpt
description: La rubrique commandes Windows pour tracerpt, qui analyse les journaux de suivi des événements, les fichiers journaux générés par l’analyseur de performances et les fournisseurs de suivi d’événements en temps réel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17bed5b1cb084392ed3169ca963ce03c1ee2b0d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832682"
---
# <a name="tracerpt"></a>tracerpt

La commande **Tracerpt** peut être utilisée pour analyser les journaux de suivi d’événements, les fichiers journaux générés par l’analyseur de performances et les fournisseurs de suivi d’événements en temps réel. Il génère des fichiers de vidage, des fichiers de rapport et des schémas de rapport.

Pour obtenir des exemples d’utilisation de **Tracerpt**, consultez [exemples](#BKMK_EXAMPLES).

## <a name="syntax"></a>Syntaxe

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>Options

|              Indicateur d’option               |                                                                    Description                                                                    |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
|                   -?                   |                                                         Affiche l’aide contextuelle.                                                          |
|          -config \<nom du fichier >           |                                                 Charge un fichier de paramètres contenant les options de commande.                                                  |
|                   -y                   |                                                  Répondez oui à toutes les questions sans demander confirmation.                                                   |
|            -f \<XML\|HTML >             |                                                                  Format du rapport.                                                                   |
|         -of \<CSV\|EVTX\|XML >          |                                                         Format de vidage, la valeur par défaut est XML.                                                          |
|            -DF \<nom de fichier >             |                                            Créez un fichier de schéma de comptage/rapports spécifique à Microsoft.                                            |
|            -int \<filename >            |                                            Vide la structure d’événement interprétée dans le fichier spécifié.                                            |
|                  -RTS                  |                        Horodatage brut du rapport dans l’en-tête de suivi d’événement. Peut uniquement être utilisé avec-o, et non-Report ou-Summary.                         |
|            -TMF \<nom de fichier >            |                                                  Spécifiez un fichier de définition de format de message de trace.                                                  |
|              -valeur de TP \<>              |                            Spécifiez le chemin de recherche de fichiers TMF. Plusieurs chemins d’accès peuvent être utilisés, séparés par un point-virgule (;).                            |
|              -i \<valeur >               | Spécifiez le chemin d’accès de l’image du fournisseur. Le fichier PDB correspondant sera situé dans le serveur de symboles. Plusieurs chemins d’accès peuvent être utilisés, séparés par un point-virgule (;). |
|             valeur de \<PDB >              |                             Spécifiez le chemin d’accès du serveur de symboles. Plusieurs chemins d’accès peuvent être utilisés, séparés par un point-virgule (;).                             |
|                  -GMT                  |                                              Convertit les horodateurs de charge WPP en heure GMT.                                               |
|              valeur-RL \<>              |                                               Définissez le niveau de rapport système de 1 à 5. La valeur par défaut est 1.                                               |
|          -Summary [nom de fichier]           |                                  Générez un fichier texte de rapport de synthèse. Nom de fichier s’il n’est pas spécifié est Summary. txt.                                   |
|             -o [nom de fichier]              |                                      Générez un fichier de sortie de texte. Nom de fichier s’il n’est pas spécifié est dumpfile. Xml.                                      |
|           -rapport [nom de fichier]           |                                  Générez un fichier de rapport de sortie texte. Nom de fichier s’il n’est pas spécifié est Workload. Xml.                                   |
|                  -LR                   |                        Spécifiez moins restrictif. Cela utilise les meilleurs efforts pour les événements qui ne correspondent pas au schéma d’événements.                         |
|           -Export [nom de fichier]           |                                  Générez un fichier d’exportation du schéma d’événement. Nom de fichier s’il n’est pas spécifié est Schema. Man.                                   |
|       [-l] \<valeur [valeur [...]] >        |                                                   Spécifiez le fichier journal de suivi d’événements à traiter.                                                    |
| -RT \<session_name [session_name [...]] > |                                                Spécifiez les sources de données de session de suivi d’événements en temps réel.                                                |

## <a name="examples"></a><a name=BKMK_EXAMPLES></a>Illustre

- Cet exemple crée un rapport basé sur les deux journaux des événements **logfile1. etl** et **logfile2. etl** et crée le fichier de vidage **LOGDUMP. xml** au format XML.  
  ```
  tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
  ```  
- Cet exemple crée un rapport basé sur le journal des événements **logfile. etl**, crée le fichier de vidage **logdmp. XML** au format XML, utilise les meilleurs efforts pour identifier les événements qui ne se trouvent pas dans le schéma, génère un fichier de rapport de synthèse **logdump. txt**et génère le fichier de rapport **logrpt. xml**.  
  ```
  tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
  ```  
- Cet exemple utilise les deux journaux des événements **logfile1. etl** et **logfile2. etl** pour produire un fichier de vidage et un fichier de rapport avec les noms de fichiers par défaut.  
  ```
  tracerpt logfile1.etl logfile2.etl -o -report
  ```  
- Cet exemple utilise le journal des événements **logfile. etl** et le journal de performances **counterfile. BLG** pour générer le fichier de rapport **logrpt. xml** et le fichier de schéma XML spécifique à Microsoft **Schema. xml**.  
  ```
  tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
  ```  
- Cet exemple lit le journal de noyau NT de session de suivi d’événements en temps réel et génère le fichier de vidage **logfile. csv** au format CSV.  
  ```
  tracerpt -rt NT Kernel Logger -o logfile.csv -of CSV
  ```
