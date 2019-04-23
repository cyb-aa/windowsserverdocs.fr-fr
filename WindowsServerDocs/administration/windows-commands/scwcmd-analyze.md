---
title: Scwcmd analyser
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0259271b-be5b-48d7-a51d-8b9b6786efb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cce3428b281ede582ed781afbdee9dea495b52ae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873910"
---
# <a name="scwcmd-analyze"></a>Scwcmd: analyze

> S'applique à : Windows Server 2012 R2, Windows Server 2012

Détermine si un ordinateur est conforme à une stratégie. Résultats sont retournés dans un fichier .xml. Accepte également une liste de noms d’ordinateur en tant qu’entrée. Pour afficher les résultats dans votre navigateur, utilisez **scwcmd vue** et spécifiez **%windir%\security\msscw\TransformFiles\scwanalysis.xsl** en tant que la transformation .xsl. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
scwcmd analyze [[[/m:<ComputerName> | /ou:<Ou>] /p:<Policy>] | /i:<ComputerList>] [/o:
<ResultDir>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>] [/l] [/e]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ m:\<nom_ordinateur >|Spécifie le nom NetBIOS, le nom DNS ou l’adresse IP de l’ordinateur à analyser. Si le **/m** est précisé, le **/p** paramètre doit également être spécifié.|
|/ ou :\<OuName >|Spécifie le nom de domaine complet (FQDN) d’une unité d’organisation (UO) dans les Services de domaine Active Directory. Si le **/ou** est précisé, le **/p** paramètre doit également être spécifié. Tous les ordinateurs dans l’unité d’organisation sont analysés par rapport à la stratégie donnée.|
|/ p:\<stratégie >|Spécifie le chemin d’accès et le nom du fichier de stratégie .xml à utiliser pour effectuer l’analyse.|
|/ i:\<ComputerList >|Spécifie le chemin d’accès et le nom d’un fichier .xml qui contient une liste des ordinateurs, ainsi que leurs fichiers de stratégie attendu. Tous les ordinateurs dans le fichier .xml seront analysés par rapport à leurs fichiers de stratégie correspondante. Un exemple de fichier .xml est % windir%\security\SampleMachineList.xml.|
|/ o:\<ResultDir >|Spécifie le chemin d’accès et le répertoire dans lequel les fichiers de résultats d’analyse doivent être enregistrés. L'emplacement par défaut est le répertoire actif.|
|/ u:\<nom d’utilisateur >|Spécifie les informations d’identification utilisateur à utiliser lors de l’exécution de l’analyse sur un ordinateur distant. La valeur par défaut est connecté sur l’utilisateur.|
|/ pw :\<mot de passe >|Spécifie les informations d’identification utilisateur à utiliser lors de l’exécution de l’analyse sur un ordinateur distant. La valeur par défaut est le mot de passe de l’utilisateur connecté.|
|/ t:\<threads >|Spécifie le nombre d’opérations d’analyse en attente simultanées qui doit être conservée lors de l’analyse (DefaultValue = 40, MinValue = 1, MaxValue = 1000).|
|/l|Provoque le processus d’analyse à enregistrer. Un fichier journal sera généré pour chaque ordinateur en cours d’analyse. Les fichiers journaux sont stockés dans le même répertoire que les fichiers de résultats. Utilisez le **/o** option pour spécifier le répertoire pour les fichiers de résultats.|
|/e|Enregistrer un événement au journal des événements de l’Application si une incompatibilité est trouvée.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Scwcmd.exe est uniquement disponible sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Exemples

Pour analyser une stratégie de sécurité sur le fichier webpolicy.xml, tapez :
```
scwcmd analyze /p:webpolicy.xml

```
Pour analyser une stratégie de sécurité sur l’ordinateur nommé le serveur Web sur le fichier webpolicy.xml en utilisant les informations d’identification du compte webadmin, tapez :
```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin

```
Pour analyser une stratégie de sécurité sur le fichier webpolicy.xml, avec un maximum de 100 threads et les résultats d’un fichier nommé entraîne le partage resultserver, tapez :
```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results

```
Pour analyser une stratégie de sécurité pour le WebServers OU sur le fichier webpolicy.xml en utilisant les informations d’identification DomainAdmin, tapez :
```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)