---
title: Bitsadmin setclientcertificatebyid
description: Rubrique relative aux commandes Windows pour Bitsadmin setclientcertificatebyid, qui spécifie l’identificateur du certificat client à utiliser pour l’authentification du client dans une requête HTTPs (SSL)
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80c97b21194c773d1b21aab2ee31794624da671c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849662"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>Bitsadmin setclientcertificatebyid

Spécifie l’identificateur du certificat client à utiliser pour l’authentification du client dans une requête HTTPs (SSL).

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> hexa-decimal_cert_id>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Store_location|Identifie l’emplacement d’un magasin système à utiliser pour la recherche du certificat. Les valeurs possibles incluent notamment :</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVICES)</br>5 (UTILISATEURS)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|Nom du magasin de certificats. Les valeurs possibles incluent notamment :</br>AUTORITÉ de certification (certificats d’autorité de certification)</br>MY (certificats personnels)</br>RACINE (certificats racines)</br>SPC (certificat de l’éditeur de logiciel)|
|Hexadecimal_cert_id|Nombre hexadécimal représentant le hachage du certificat|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant spécifie l’identificateur du certificat client à utiliser pour l’authentification du client dans une requête HTTPs (SSL) pour la tâche nommée *myJob*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByID myJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)