---
title: bitsadmin setclientcertificatebyid
description: Rubrique de référence pour la commande Bitsadmin setclientcertificatebyid, qui spécifie l’identificateur du certificat client à utiliser pour l’authentification du client dans une requête HTTPs (SSL)
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 24f5d0b9cda9fecc70611d8eaa21b0c8976c4c7c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719339"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>bitsadmin setclientcertificatebyid

Spécifie l’identificateur du certificat client à utiliser pour l’authentification du client dans une requête HTTPs (SSL).

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setclientcertificatebyid <job> <store_location> <store_name> <hexadecimal_cert_id>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |
| store_location | Identifie l’emplacement d’un magasin système à utiliser pour la recherche du certificat, y compris :<ul><li>CURRENT_USER</li><li>LOCAL_MACHINE</li><li>CURRENT_SERVICE</li><li>SYNCHRONISATION DES IDENTITÉS</li><li>UTILISATEURS</li><li>CURRENT_USER_GROUP_POLICY</li><li>LOCAL_MACHINE_GROUP_POLICY</li><li>LOCAL_MACHINE_ENTERPRISE.</li></ul> |
| store_name | Nom du magasin de certificats, y compris :<ul><li>AUTORITÉ de certification (certificats d’autorité de certification)</li><li>MY (certificats personnels)</li><li>RACINE (certificats racines)</li><li>SPC (certificat de l’éditeur de logiciel).</li></ul> |
| hexadecimal_cert_id | Nombre hexadécimal représentant le hachage du certificat. |

## <a name="examples"></a>Exemples

Pour spécifier l’identificateur du certificat client à utiliser pour l’authentification du client dans une requête HTTPs (SSL) pour le travail nommé *myDownloadJob*:

```
bitsadmin /setclientcertificatebyid myDownloadJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
