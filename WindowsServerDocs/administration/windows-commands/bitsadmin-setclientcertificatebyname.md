---
title: Bitsadmin setclientcertificatebyname
description: Rubrique de commandes de Windows pour **bitsadmin setclientcertificatebyname** -Spécifie le nom du sujet du certificat client à utiliser pour l’authentification du client dans une demande HTTPS (SSL).
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76150ccb34693eb692d27efbd6538f5363ba1c26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871310"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>Bitsadmin setclientcertificatebyname



Spécifie le nom du sujet du certificat client à utiliser pour l’authentification du client dans une demande HTTPS (SSL).

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> <subject_name>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Store_location|Identifie l’emplacement d’un magasin système à utiliser pour la recherche du certificat. Les valeurs possibles incluent notamment :</br>1 (CURRENT_USER)</br>2 (ORDINATEUR_LOCAL)</br>3 (CURRENT_SERVICE)</br>4 (SERVICES)</br>5 (UTILISATEURS)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|Le nom du magasin de certificats. Les valeurs possibles incluent notamment :</br>Autorité de certification (certificats d’autorité de Certification)</br>Mon (certificats personnels)</br>RACINE (certificats racine)</br>SPC (Software Publisher Certificate)|
|Subject_name|Nom du certificat|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant spécifie le nom du certificat client *myCertificate* à utiliser pour l’authentification du client dans une demande HTTPS (SSL) pour le travail nommé *myJob*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByName myJob 1 MY myCertificate 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)