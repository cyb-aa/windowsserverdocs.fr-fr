---
title: bitsadmin setcredentials
description: Rubrique relative aux commandes Windows pour Bitsadmin SetCredentials, qui ajoute des informations d’identification à un travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 918bda93407e029cedaaf5eab937d1bb23dc3c4c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849642"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

Ajoute des informations d’identification à un travail.

**BITS 1,2 et versions antérieures**: non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetCredentials <Job> <Target> <Scheme> <Username> <Password>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Target|SERVEUR ou PROXY|
|Mode|Une des causes suivantes :</br>-BASIC : schéma d’authentification dans lequel le nom d’utilisateur et le mot de passe sont envoyés en texte clair au serveur ou au proxy.</br>-DIGEST : schéma d’authentification par stimulation-réponse qui utilise une chaîne de données spécifiée par le serveur pour la stimulation.</br>-NTLM : schéma d’authentification par stimulation-réponse qui utilise les informations d’identification de l’utilisateur pour l’authentification dans un environnement réseau Windows.</br>-NEGOTIATE, également appelé Snego (simple and Protected Negotiation Protocol) est un schéma d’authentification par stimulation-réponse qui négocie avec le serveur ou le proxy pour déterminer le schéma à utiliser pour l’authentification. Le protocole Kerberos et NTLM en sont des exemples.</br>-PASSPORT : service d’authentification centralisé fourni par Microsoft qui propose une ouverture de session unique pour les sites membres.|
|Nom d'utilisateur|Nom des informations d’identification fournies|
|Password|Mot de passe associé au *nom d’utilisateur* fourni.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant ajoute des informations d’identification à la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC Edward Password20
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)