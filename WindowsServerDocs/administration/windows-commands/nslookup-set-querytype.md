---
title: nslookup set querytype
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5af54ac5-fc1a-4af6-977b-f8e97c8eba90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc0eb19fd66e738b4bfc110a2bbc172153a12d98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372902"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifie le type d’enregistrement de ressource pour la requête.
## <a name="syntax"></a>Syntaxe
```
set querytype=<ResourceRecordtype>
```
## <a name="parameters"></a>Paramètres
<ResourceRecordtype> spécifie un type d’enregistrement de ressource DNS. Le type d’enregistrement de ressource par défaut est. Le tableau suivant répertorie les valeurs valides pour cette commande.

| Valeur |                                                   Description                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   A   |                                      Spécifie l'&#39;adresse IP de l’ordinateur                                      |
|  AUX  |                                     Spécifie l'&#39;adresse IP de l’ordinateur.                                      |
| CANONIQUE |                                    Spécifie un nom canonique pour un alias.                                     |
|  ID  |                                  Spécifie un identificateur de groupe d’un nom de groupe.                                  |
| HINFO |                          Spécifie le&#39;processeur et le type de système d’exploitation de l’ordinateur.                           |
|  Mo   |                                        Spécifie un nom de domaine de boîte aux lettres.                                         |
|  ML   |                                         Spécifie un membre du groupe de messagerie.                                          |
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
  <a href="command-line-syntax-key.md" data-raw-source="[Command-Line Syntax Key](command-line-syntax-key.md)">Clé de syntaxe de ligne de commande</a>
  <a href="nslookup-set-type.md" data-raw-source="[nslookup set type](nslookup-set-type.md)">type de jeu nslookup</a>
