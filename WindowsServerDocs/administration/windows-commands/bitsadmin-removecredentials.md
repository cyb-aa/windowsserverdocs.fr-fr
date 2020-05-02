---
title: bitsadmin removecredentials
description: Rubrique de référence pour la commande Bitsadmin removecredentials, qui supprime les informations d’identification d’un travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4a78ce9a-1feb-4811-a000-cce81287b22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4dcfaa55847e531871c6a7ad9fd84c3861c4cd9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717047"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

Supprime les informations d’identification d’un travail.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 1,2 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /removecredentials <job> <target> <scheme>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |
| target | Utilisez un **serveur** ou un **proxy**. |
| scheme | Utilisez l’une des valeurs suivantes :<ul><li>**Bases.** Schéma d’authentification dans lequel le nom d’utilisateur et le mot de passe sont envoyés en texte clair au serveur ou au proxy.</li><li>**Voyage.** Schéma d’authentification par stimulation-réponse qui utilise une chaîne de données spécifiée par le serveur pour la stimulation.</li><li>**NTLM.** Schéma d’authentification par stimulation-réponse qui utilise les informations d’identification de l’utilisateur pour l’authentification dans un environnement réseau Windows.</li><li>**NEGOTIATE (également appelé protocole de négociation simple et protégé).** Schéma d’authentification par stimulation-réponse qui négocie avec le serveur ou le proxy pour déterminer le schéma à utiliser pour l’authentification. Le protocole Kerberos et NTLM en sont des exemples.</li><li>**.Net.** Service d’authentification centralisé fourni par Microsoft qui propose une ouverture de session unique pour les sites membres.</li></ul> |

## <a name="examples"></a>Exemples

Pour supprimer les informations d’identification de la tâche nommée *myDownloadJob*:

```
bitsadmin /removecredentials myDownloadJob SERVER BASIC
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
