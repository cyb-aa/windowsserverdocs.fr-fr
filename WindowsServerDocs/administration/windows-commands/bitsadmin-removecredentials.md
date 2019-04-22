---
title: bitsadmin removecredentials
description: Rubrique de commandes de Windows pour **bitsadmin removecredentials** -supprime les informations d’identification d’un travail.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a78ce9a-1feb-4811-a000-cce81287b22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cbd65442ff0d74ec1179a49df5d4a94785f3dd25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822290"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

Supprime les informations d’identification d’un travail.

**BITS 1.2 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /RemoveCredentials <Job> <Target> <Scheme>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Cible|SERVEUR ou PROXY|
|Schéma|l’un des suivants :</br>-De base : schéma d’authentification dans lequel le nom d’utilisateur et le mot de passe sont envoyés en texte clair au serveur ou au proxy.</br>-DIGEST, un schéma d’authentification stimulation / réponse qui utilise une chaîne de données spécifiée par le serveur pour la stimulation.</br>-NTLM, un schéma d’authentification stimulation / réponse qui utilise les informations d’identification de l’utilisateur pour l’authentification dans un environnement de réseau Windows.</br>-Par négociation, également connu sous le protocole Simple et protégés de négociation (Snego) est un schéma d’authentification stimulation / réponse qui négocie avec le serveur ou le proxy pour déterminer le modèle à utiliser pour l’authentification. Le protocole Kerberos et NTLM en sont des exemples.</br>-PASSPORT, un service d’authentification centralisé fourni par Microsoft qui propose une ouverture de session unique pour les sites membres.|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant supprime les informations d’identification de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)