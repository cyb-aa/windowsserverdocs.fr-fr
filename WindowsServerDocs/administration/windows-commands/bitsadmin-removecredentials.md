---
title: bitsadmin removecredentials
description: Rubrique relative aux commandes Windows pour **Bitsadmin removecredentials** -supprime les informations d’identification d’un travail.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 34a5bae9304a9db9f47f437276270ca06b1ebeee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380821"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

Supprime les informations d’identification d’un travail.

**BITS 1,2 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /RemoveCredentials <Job> <Target> <Scheme>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Cible|SERVEUR ou PROXY|
|mode|l’un des suivants :</br>-BASIC : schéma d’authentification dans lequel le nom d’utilisateur et le mot de passe sont envoyés en texte clair au serveur ou au proxy.</br>-DIGEST : schéma d’authentification par stimulation-réponse qui utilise une chaîne de données spécifiée par le serveur pour la stimulation.</br>-NTLM : schéma d’authentification par stimulation-réponse qui utilise les informations d’identification de l’utilisateur pour l’authentification dans un environnement réseau Windows.</br>-NEGOTIATE, également appelé Snego (simple and Protected Negotiation Protocol) est un schéma d’authentification par stimulation-réponse qui négocie avec le serveur ou le proxy pour déterminer le schéma à utiliser pour l’authentification. Le protocole Kerberos et NTLM en sont des exemples.</br>-PASSPORT : service d’authentification centralisé fourni par Microsoft qui propose une ouverture de session unique pour les sites membres.|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant supprime les informations d’identification de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)