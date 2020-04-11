---
title: Bitsadmin setsecurityflags
description: La rubrique commandes Windows pour **Bitsadmin setsecurityflags**, qui définit les indicateurs de sécurité pour http pour déterminer si bits doit vérifier la liste de révocation de certificats, ignorer certaines erreurs de certificat et définir la stratégie à utiliser lorsqu’un serveur redirige la requête http.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8d73361bceda8c0eb24992bdee176b47bf82a878
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122725"
---
# <a name="bitsadmin-setsecurityflags"></a>Bitsadmin setsecurityflags

Définit des indicateurs de sécurité pour le protocole HTTP pour déterminer si BITS doit vérifier la liste de révocation de certificats, ignorer certaines erreurs de certificat et définir la stratégie à utiliser lorsqu’un serveur redirige la requête HTTP. La valeur est un entier non signé.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setsecurityflags <job> <value>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |
| valeur | Peut inclure un ou plusieurs des indicateurs de notification suivants, notamment :<ul><li>Définissez le bit le moins significatif pour activer la vérification de la liste de révocation de certificats.</li><li>Définissez le 2e bit à partir de la droite pour ignorer les noms communs incorrects dans le certificat de serveur.</li><li>Définissez le troisième bit de droite pour ignorer les dates incorrectes dans le certificat de serveur.</li><li>Définissez le quatrième bit à partir de la droite pour ignorer les autorités de certification incorrectes dans le certificat de serveur.</li><li>Définissez le 5e bit à partir de la droite pour ignorer l’utilisation incorrecte du certificat de serveur.</li><li>Définissez le neuvième du 11 bits à partir de la droite pour implémenter votre stratégie de redirection spécifiée, notamment :<ul><li>**0, 0, 0.** Les redirections sont automatiquement autorisées.</li><li>**0, 0, 1.** Le nom distant dans l’interface **IBackgroundCopyFile** est mis à jour si une redirection se produit.</li><li>**0, 1, 0.** BITS fait échouer la tâche si une redirection se produit.</li></ul></li><li>Définissez le douzième bit à partir de la droite pour autoriser la redirection de HTTPs vers HTTP.</li></ul> |

## <a name="examples"></a>Exemples

L’exemple suivant définit les indicateurs de sécurité pour activer la vérification de la liste de révocation de certificats pour la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /setsecurityflags myDownloadJob 0x0001
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)