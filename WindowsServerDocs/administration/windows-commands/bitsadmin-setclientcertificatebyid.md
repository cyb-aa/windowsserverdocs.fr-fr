---
title: Bitsadmin setclientcertificatebyid
description: Rubrique de commandes de Windows pour **bitsadmin setclientcertificatebyid** Spécifie l’identificateur du certificat client à utiliser pour l’authentification du client dans une demande HTTPS (SSL)
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2424de18ee8aaec73b086207e8ef56d85df862fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863930"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>Bitsadmin setclientcertificatebyid



Spécifie l’identificateur du certificat client à utiliser pour l’authentification du client dans une demande HTTPS (SSL).

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> hexa-decimal_cert_id>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Store_location|Identifie l’emplacement d’un magasin système à utiliser pour la recherche du certificat. Les valeurs possibles incluent notamment :</br>1 (CURRENT_USER)</br>2 (ORDINATEUR_LOCAL)</br>3 (CURRENT_SERVICE)</br>4 (SERVICES)</br>5 (UTILISATEURS)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|Le nom du magasin de certificats. Les valeurs possibles incluent notamment :</br>Autorité de certification (certificats d’autorité de Certification)</br>Mon (certificats personnels)</br>RACINE (certificats racine)</br>SPC (Software Publisher Certificate)|
|Hexadecimal_cert_id|Un nombre hexadécimal représentant le hachage du certificat|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant spécifie l’identificateur du certificat client à utiliser pour l’authentification du client dans une demande HTTPS (SSL) pour le travail nommé *myJob*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByID myJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)