---
title: Bitsadmin setclientcertificatebyname
description: La rubrique commandes Windows pour Bitsadmin setclientcertificatebyname, qui spécifie le nom du sujet du certificat client à utiliser pour l’authentification du client dans une requête HTTPs (SSL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08ec6fd8c941234de36f14cd71ffa51c3b428acb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849652"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>Bitsadmin setclientcertificatebyname

Spécifie le nom du sujet du certificat client à utiliser pour l’authentification du client dans une requête HTTPs (SSL).

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> <subject_name>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Store_location|Identifie l’emplacement d’un magasin système à utiliser pour la recherche du certificat. Les valeurs possibles incluent notamment :</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVICES)</br>5 (UTILISATEURS)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|Nom du magasin de certificats. Les valeurs possibles incluent notamment :</br>AUTORITÉ de certification (certificats d’autorité de certification)</br>MY (certificats personnels)</br>RACINE (certificats racines)</br>SPC (certificat de l’éditeur de logiciel)|
|Subject_name|Nom du certificat|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant spécifie le nom du certificat client *myCertificate* à utiliser pour l’authentification du client dans une requête HTTPS (SSL) pour la tâche nommée *myJob*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByName myJob 1 MY myCertificate 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)