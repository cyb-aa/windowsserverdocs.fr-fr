---
title: Bitsadmin setclientcertificatebyid
description: La rubrique commandes Windows pour **Bitsadmin setclientcertificatebyid** spécifie l’identificateur du certificat client à utiliser pour l’authentification du client dans une requête HTTPS (SSL).
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 53b6fa4c65397cf710d0497fbae889afd31ec136
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380729"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>Bitsadmin setclientcertificatebyid



Spécifie l’identificateur du certificat client à utiliser pour l’authentification du client dans une requête HTTPs (SSL).

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> hexa-decimal_cert_id>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Store_location|Identifie l’emplacement d’un magasin système à utiliser pour la recherche du certificat. Les valeurs possibles incluent notamment :</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVICES)</br>5 (UTILISATEURS)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|Nom du magasin de certificats. Les valeurs possibles incluent notamment :</br>AUTORITÉ de certification (certificats d’autorité de certification)</br>MY (certificats personnels)</br>RACINE (certificats racines)</br>SPC (certificat de l’éditeur de logiciel)|
|Hexadecimal_cert_id|Nombre hexadécimal représentant le hachage du certificat|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant spécifie l’identificateur du certificat client à utiliser pour l’authentification du client dans une requête HTTPs (SSL) pour la tâche nommée *myJob*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByID myJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)