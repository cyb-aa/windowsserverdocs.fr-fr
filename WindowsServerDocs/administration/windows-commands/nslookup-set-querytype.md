---
title: nslookup set querytype
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5af54ac5-fc1a-4af6-977b-f8e97c8eba90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c066277a22325e5db8383f58cda31038aa656e0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838432"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifie le type d’enregistrement de ressource pour la requête.
## <a name="syntax"></a>Syntaxe
```
set querytype=<ResourceRecordtype>
```
### <a name="parameters"></a>Paramètres
<ResourceRecordtype> spécifie un type d’enregistrement de ressource DNS. Le type d’enregistrement de ressource par défaut est. Le tableau suivant répertorie les valeurs valides pour cette commande.

| Valeur |                                                   Description                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   A   |                                      Spécifie l'&#39;adresse IP de l’ordinateur                                      |
|  AUX  |                                     Spécifie l'&#39;adresse IP de l’ordinateur.                                      |
| CNAME |                                    Spécifie un nom canonique pour un alias.                                     |
|  ID  |                                  Spécifie un identificateur de groupe d’un nom de groupe.                                  |
| HINFO |                          Spécifie le&#39;processeur et le type de système d’exploitation de l’ordinateur.                           |
|  Mo   |                                        Spécifie un nom de domaine de boîte aux lettres.                                         |
|  MG   |                                         Spécifie un membre du groupe de messagerie.                                          |
| MINFO |                                   Spécifie les informations de boîte aux lettres ou de messagerie.                                   |
|  MR   |                                     Spécifie le nom de domaine du changement de nom de messagerie.                                      |
|  MX   |                                          Spécifie l’échange de messagerie.                                          |
|  NS   |                                 Spécifie un serveur de noms DNS pour la zone nommée.                                 |
|  EFFECTUÉS  | Spécifie un nom d’ordinateur si la requête est une adresse IP ; Sinon, spécifie le pointeur vers d’autres informations. |
|  Start  |                                Spécifie le début de l’autorité d’une zone DNS.                                 |
|  TXT  |                                         Spécifie les informations de texte.                                         |
|  CODÉ  |                                         Spécifie l’identificateur de l’utilisateur.                                          |
| UINFO |                                         Spécifie les informations sur l’utilisateur.                                         |
|  CONNU  |                                         Décrit un service bien connu.                                         |
| {aide |                                                       ?}                                                        |

Affiche un bref résumé des sous-commandes <strong>nslookup</strong>
## <a name="remarks"></a>Notes
- La commande <strong>Set type</strong> effectue la même fonction que la commande <strong>Set QueryType</strong> .
- Pour plus d’informations sur les types d’enregistrements de ressources, consultez Request for Comment (RFC) 1035.
  ## <a name="additional-references"></a>Références supplémentaires
  < a href = syntaxe de ligne de commande-key.md Data-RAW-source =- [clé de](command-line-syntax-key.md)syntaxe de ligne de commande > clé de syntaxe de ligne de commande</a> < a href = nslookup-set-type.MD Data-RAW-source =[nslookup Set type](nslookup-set-type.md)> nslookup Set type</a>
