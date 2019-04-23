---
title: Configurer scwcmd
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6528b9dc-3d82-4228-b734-ed717458d74c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 31838ac7299cc30a7b7dde3beb47669df772487c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857500"
---
# <a name="scwcmd-configure"></a>Scwcmd: configure

> S'applique à : Windows Server 2012 R2, Windows Server 2012

Applique une stratégie de sécurité générés par l’Assistant Configuration de sécurité à un ordinateur. Cet outil de ligne de commande accepte également une liste de noms d’ordinateur en tant qu’entrée.

## <a name="syntax"></a>Syntaxe

```
scwcmd configure [[[/m:<ComputerName> | /ou:<OuName>] /p:<Policy>] | /i:<ComputerList>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ m:\<nom_ordinateur >|Spécifie le nom NetBIOS, le nom DNS ou l’adresse IP de l’ordinateur à configurer. Si le **/m** est précisé, le **/p** paramètre doit également être spécifié.|
|/ ou :\<OuName >|Spécifie le nom de domaine complet (FQDN) d’une unité d’organisation (UO) dans les Services de domaine Active Directory. Si le **/ou** est précisé, le **/p** paramètre doit également être spécifié. Tous les ordinateurs dans l’unité d’organisation seront analysés en fonction de la stratégie donnée.|
|/ p:\<stratégie >|Spécifie le chemin d’accès et le nom du fichier de stratégie .xml à utiliser pour effectuer la configuration.|
|/ i:\<ComputerList >|Spécifie le chemin d’accès et le nom d’un fichier .xml qui contient une liste des ordinateurs, ainsi que leurs fichiers de stratégie attendu. Tous les ordinateurs dans le fichier .xml seront configurés en fonction de leurs fichiers de stratégie correspondante. Un exemple de fichier .xml est % windir%\security\SampleMachineList.xml.|
|/ u:\<nom d’utilisateur >|Spécifie les informations d’identification utilisateur à utiliser lors de la configuration d’un ordinateur distant. La valeur par défaut est connecté sur l’utilisateur.|
|/ pw :\<mot de passe >|Spécifie les informations d’identification utilisateur à utiliser lors de la configuration d’un ordinateur distant. La valeur par défaut est le mot de passe de l’utilisateur connecté.|
|/ t:\<threads >|Spécifie le nombre d’opérations de configuration en attente simultanées qui doivent être conservées pendant le processus de configuration (DefaultValue = 40, MinValue = 1, MaxValue = 1000).|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Scwcmd.exe est uniquement disponible sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Exemples

Pour configurer une stratégie de sécurité sur le fichier webpolicy.xml, tapez :
```
scwcmd configure /p:webpolicy.xml
```
Pour configurer une stratégie de sécurité pour l’ordinateur à 172.16.0.0 contre le webpolicy.xml de fichier en utilisant les informations d’identification du compte webadmin, tapez :
```
scwcmd configure /m:172.16.0.0 /p:webpolicy.xml /u:webadmin
```
Pour configurer une stratégie de sécurité sur tous les ordinateurs sur le campusmachines.xml de liste avec un maximum de 100 threads, tapez :
```
scwcmd configure /i:campusmachines.xml /t:100
```
Pour configurer une stratégie de sécurité sur tous les ordinateurs dans l’OU WebServers sur le fichier webpolicy.xml en utilisant les informations d’identification du compte DomainAdmin, tapez :
```
scwcmd configure /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)