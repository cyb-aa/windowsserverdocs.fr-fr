---
title: Scwcmd analyser
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0259271b-be5b-48d7-a51d-8b9b6786efb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 017363ff2a60f9348290813c357560fe9fe3ba2a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722161"
---
# <a name="scwcmd-analyze"></a>Scwcmd: analyze

> S’applique à : Windows Server 2012 R2, Windows Server 2012

Détermine si un ordinateur est conforme à une stratégie. Les résultats sont retournés dans un fichier. Xml. Accepte également une liste de noms d’ordinateurs comme entrée. Pour afficher les résultats dans votre navigateur, utilisez la **vue scwcmd** et spécifiez **%windir%\security\msscw\TransformFiles\scwanalysis.xsl** en tant que transformation. Xsl.

## <a name="syntax"></a>Syntaxe

```
scwcmd analyze [[[/m:<ComputerName> | /ou:<Ou>] /p:<Policy>] | /i:<ComputerList>] [/o:
<ResultDir>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>] [/l] [/e]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/m :\<nom_ordinateur>|Spécifie le nom NetBIOS, le nom DNS ou l’adresse IP de l’ordinateur à analyser. Si le paramètre **/m** est spécifié, le paramètre **/p** doit également être spécifié.|
|/ou :\<OuName>|Spécifie le nom de domaine complet (FQDN) d’une unité d’organisation (UO) dans Active Directory Domain Services. Si le paramètre **/ou** est spécifié, le paramètre **/p** doit également être spécifié. Tous les ordinateurs de l’unité d’organisation seront analysés par rapport à la stratégie donnée.|
|/p :\<> de stratégie|Spécifie le chemin d’accès et le nom du fichier de stratégie. XML à utiliser pour effectuer l’analyse.|
|/i :\<ComputerList>|Spécifie le chemin d’accès et le nom d’un fichier. XML qui contient une liste d’ordinateurs ainsi que les fichiers de stratégie attendus. Tous les ordinateurs du fichier. xml seront analysés par rapport à leurs fichiers de stratégie correspondants. Un fichier Sample. xml est%windir%\security\SampleMachineList.xml.|
|/o :\<ResultDir>|Spécifie le chemin d’accès et le répertoire où les fichiers de résultats d’analyse doivent être enregistrés. L'emplacement par défaut est le répertoire actif.|
|/u :\<nom d’utilisateur>|Spécifie les autres informations d’identification de l’utilisateur à utiliser lors de l’analyse sur un ordinateur distant. La valeur par défaut est l’utilisateur connecté.|
|/PW :\<mot de passe>|Spécifie les autres informations d’identification de l’utilisateur à utiliser lors de l’analyse sur un ordinateur distant. La valeur par défaut est le mot de passe de l’utilisateur connecté.|
|/t :\<threads>|Spécifie le nombre d’opérations d’analyse en suspens simultanées qui doivent être maintenues pendant l’analyse (DefaultValue = 40, MinValue = 1, MaxValue = 1000).|
|/l|Entraîne la journalisation du processus d’analyse. Un fichier journal est généré pour chaque ordinateur en cours d’analyse. Les fichiers journaux sont stockés dans le même répertoire que les fichiers de résultats. Utilisez l’option **/o** pour spécifier le répertoire des fichiers de résultats.|
|/e|Consignez un événement dans le journal des événements de l’application si une incompatibilité est trouvée.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

Scwcmd. exe est disponible uniquement sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="examples"></a>Exemples

Pour analyser une stratégie de sécurité par rapport au fichier webpolicy. xml, tapez :
```
scwcmd analyze /p:webpolicy.xml

```
Pour analyser une stratégie de sécurité sur l’ordinateur nommé webserver par rapport au fichier webpolicy. XML à l’aide des informations d’identification du compte webadmin, tapez :
```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin

```
Pour analyser une stratégie de sécurité sur le fichier webpolicy. xml, avec un maximum de 100 threads, et générer les résultats dans un fichier nommé results dans le partage resultserver, tapez :
```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results

```
Pour analyser une stratégie de sécurité pour l’unité d’organisation de serveurs de fichiers par rapport au fichier webpolicy. XML à l’aide des informations d’identification DomainAdmin, tapez :
```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)