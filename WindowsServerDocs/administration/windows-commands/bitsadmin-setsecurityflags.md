---
title: Bitsadmin setsecurityflags
description: La rubrique commandes Windows pour **Bitsadmin setsecurityflags** -définit des indicateurs pour http qui déterminent si bits doit vérifier la liste de révocation de certificats, ignorer certaines erreurs de certificat et définir la stratégie à utiliser lorsqu’un serveur redirige la requête http.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: acc5a64ef7c82b14e6815b6d51dda5ea4700dcad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380412"
---
# <a name="bitsadmin-setsecurityflags"></a>Bitsadmin setsecurityflags



Définit des indicateurs pour HTTP qui déterminent si BITS doit vérifier la liste de révocation de certificats, ignorer certaines erreurs de certificat et définir la stratégie à utiliser lorsqu’un serveur redirige la requête HTTP. La valeur est un entier non signé.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetSecurityFlags <Job> <Value>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Value|Voir les notes|

## <a name="remarks"></a>Notes

Le paramètre de **valeur** peut contenir un ou plusieurs des indicateurs de notification suivants.

|Action|Représentation binaire|
|------|---------------------|
|Activer la vérification de la liste de révocation|Définir le bit le moins significatif|
|Ignorer le nom commun non valide dans le certificat de serveur|Définir le 2e bit à partir de la droite|
|Ignorer la date non valide dans le certificat de serveur|Définir le troisième bit à partir de la droite|
|Ignorer l’autorité de certification non valide dans le certificat de serveur|Définir le quatrième bit à partir de la droite|
|Ignorer l’utilisation non valide du certificat|Définir le 5e bit à partir de la droite|
|Stratégie de redirection|Contrôlé par les 9 à 11 bits à partir de la droite</br>0, 0, 0-les redirections sont automatiquement autorisées.</br>0, 0, 1-le nom distant dans l’interface IBackgroundCopyFile sera mis à jour si une redirection se produit.</br>0, 1, 0-BITS échouera la tâche si une redirection se produit.|
|Autoriser la redirection de HTTPs vers HTTP|Définir le douzième bit à partir de la droite|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant définit les indicateurs de sécurité pour activer la vérification de la liste de révocation de certificats pour la tâche nommée *myJob*.
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)