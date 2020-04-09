---
title: Scwcmd configurer
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6528b9dc-3d82-4228-b734-ed717458d74c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac4333628c33b60daabbb6cff55575d6ec8cd5f6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835182"
---
# <a name="scwcmd-configure"></a>Scwcmd: configure

> S’applique à : Windows Server 2012 R2, Windows Server 2012

Applique une stratégie de sécurité générée par l’Assistant Configuration de la sécurité (SCW) à un ordinateur. Cet outil en ligne de commande accepte également une liste de noms d’ordinateurs comme entrée.

## <a name="syntax"></a>Syntaxe

```
scwcmd configure [[[/m:<ComputerName> | /ou:<OuName>] /p:<Policy>] | /i:<ComputerList>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/m :\<ComputerName >|Spécifie le nom NetBIOS, le nom DNS ou l’adresse IP de l’ordinateur à configurer. Si le paramètre **/m** est spécifié, le paramètre **/p** doit également être spécifié.|
|/ou :\<OuName >|Spécifie le nom de domaine complet (FQDN) d’une unité d’organisation (UO) dans Active Directory Domain Services. Si le paramètre **/ou** est spécifié, le paramètre **/p** doit également être spécifié. Tous les ordinateurs de l’unité d’organisation seront analysés en fonction de la stratégie donnée.|
|/p :\<> de stratégie|Spécifie le chemin d’accès et le nom de fichier du fichier de stratégie. XML à utiliser pour effectuer la configuration.|
|/i :\<ComputerList >|Spécifie le chemin d’accès et le nom d’un fichier. XML qui contient une liste d’ordinateurs ainsi que les fichiers de stratégie attendus. Tous les ordinateurs du fichier. xml seront configurés en fonction de leurs fichiers de stratégie correspondants. Un fichier Sample. xml est%windir%\security\SampleMachineList.xml.|
|/u :\<nom d’utilisateur >|Spécifie les autres informations d’identification de l’utilisateur à utiliser lors de la configuration d’un ordinateur distant. La valeur par défaut est l’utilisateur connecté.|
|/PW :\<> mot de passe|Spécifie les autres informations d’identification de l’utilisateur à utiliser lors de la configuration d’un ordinateur distant. La valeur par défaut est le mot de passe de l’utilisateur connecté.|
|/t :\<threads >|Spécifie le nombre d’opérations de configuration en suspens simultanées qui doivent être maintenues pendant le processus de configuration (DefaultValue = 40, MinValue = 1, MaxValue = 1000).|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Scwcmd. exe est disponible uniquement sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Pour configurer une stratégie de sécurité par rapport au fichier webpolicy. xml, tapez :
```
scwcmd configure /p:webpolicy.xml
```
Pour configurer une stratégie de sécurité pour l’ordinateur 172.16.0.0 sur le fichier webpolicy. XML à l’aide des informations d’identification du compte webadmin, tapez :
```
scwcmd configure /m:172.16.0.0 /p:webpolicy.xml /u:webadmin
```
Pour configurer une stratégie de sécurité sur tous les ordinateurs de la liste campusmachines. XML avec un maximum de 100 threads, tapez :
```
scwcmd configure /i:campusmachines.xml /t:100
```
Pour configurer une stratégie de sécurité sur tous les ordinateurs de l’unité d’organisation de serveurs de fichiers par rapport au fichier webpolicy. XML à l’aide des informations d’identification du compte DomainAdmin, tapez :
```
scwcmd configure /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)