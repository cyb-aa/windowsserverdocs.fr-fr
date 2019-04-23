---
title: Bitsadmin setsecurityflags
description: Rubrique de commandes de Windows pour **bitsadmin setsecurityflags** -jeux d’indicateurs pour le protocole HTTP qui déterminent si les BITS doivent vérifier la liste de révocation de certificats, ignorer certaines erreurs de certificat et définir la stratégie à utiliser lorsque un serveur redirige la requête HTTP.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a7f74146a26314ddb4fa92f85e5a40267d0f0d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858620"
---
# <a name="bitsadmin-setsecurityflags"></a>Bitsadmin setsecurityflags



Définit des indicateurs pour le protocole HTTP qui déterminent si les BITS doivent vérifier la liste de révocation de certificats, ignorer certaines erreurs de certificat et définir la stratégie à utiliser lorsqu’un serveur redirige la requête HTTP. La valeur est un entier non signé.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetSecurityFlags <Job> <Value>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Value|Consultez la section Notes|

## <a name="remarks"></a>Notes

Le **valeur** paramètre peut contenir un ou plusieurs des indicateurs de notification suivants.

|Action|Représentation binaire|
|------|---------------------|
|Activer la vérification de révocation de certificats|Définir le bit le moins significatif.|
|Ignorer le nom commun non valide dans le certificat de serveur|Définir le bit 2nd à partir de la droite|
|Ignorer une date non valide dans le certificat de serveur|Définir le bit 3 de la droite|
|Ignorer l’autorité de certification non valide dans le certificat de serveur|Définir le bit 4e à partir de la droite|
|Ignorer l’utilisation non valide du certificat|Définir le bit de 5 à partir de la droite|
|Stratégie de redirection|Contrôlé par les bits du 9 au 11 de la droite</br>0,0,0 - les redirections sont autorisées automatiquement.</br>0,0,1 - nom distant dans l’interface IBackgroundCopyFile mettra à jour si une redirection se produit.</br>0,1,0 - BITS échoue le travail si une redirection se produit.|
|Autoriser la redirection à partir de HTTPS vers HTTP|Définir le bit de 12 à partir de la droite|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant définit les indicateurs de sécurité pour activer la vérification de révocation de certificats pour le travail nommé *myJob*.
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)