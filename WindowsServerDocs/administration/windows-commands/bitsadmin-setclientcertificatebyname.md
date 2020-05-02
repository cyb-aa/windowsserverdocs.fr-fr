---
title: bitsadmin setclientcertificatebyname
description: Rubrique de référence pour la commande Bitsadmin setclientcertificatebyname, qui spécifie le nom du sujet du certificat client à utiliser pour l’authentification du client dans une requête HTTPs (SSL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef7111d462cd0509e8959855e9bc950dc15f3bfe
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719346"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>bitsadmin setclientcertificatebyname

Spécifie le nom du sujet du certificat client à utiliser pour l’authentification du client dans une requête HTTPs (SSL).

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setclientcertificatebyname <job> <store_location> <store_name> <subject_name>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |
| store_location | Identifie l’emplacement d’un magasin système à utiliser pour la recherche du certificat. Les valeurs possibles incluent :<ul><li>1 (CURRENT_USER)</li><li>2 (LOCAL_MACHINE)</li><li>3 (CURRENT_SERVICE)</li><li>4 (SERVICES)</li><li>5 (UTILISATEURS)</li><li>6 (CURRENT_USER_GROUP_POLICY)</li><li>7 (LOCAL_MACHINE_GROUP_POLICY)</li><li>8 (LOCAL_MACHINE_ENTERPRISE)</li></ul> |
| store_name | Nom du magasin de certificats. Les valeurs possibles incluent :<ul><li>AUTORITÉ de certification (certificats d’autorité de certification)</li><li>MY (certificats personnels)</li><li>RACINE (certificats racines)</li><li>SPC (certificat de l’éditeur de logiciel)</li></ul> |
| subject_name | Nom du certificat. |

## <a name="examples"></a>Exemples

Pour spécifier le nom du certificat client *myCertificate* à utiliser pour l’authentification du client dans une requête HTTPS (SSL) pour la tâche nommée *myDownloadJob*:

```
bitsadmin /setclientcertificatebyname myDownloadJob 1 MY myCertificate
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
