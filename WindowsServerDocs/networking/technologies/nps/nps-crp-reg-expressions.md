---
title: Utiliser des expressions régulières dans NPS
description: Cette rubrique explique l’utilisation d’expressions régulières pour les critères spéciaux dans NPS dans Windows Server. Vous pouvez utiliser cette syntaxe pour spécifier les conditions des attributs de stratégie réseau et des domaines RADIUS.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: jgerend
author: jasongerend
msdate: 08/16/2019
ms.openlocfilehash: 94bb9b54dba046c57c6f82e6a52a71adbcf4ce75
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396377"
---
# <a name="use-regular-expressions-in-nps"></a>Utiliser des expressions régulières dans NPS

> S’applique à :  Windows Server 2019, Windows Server 2016, Windows Server (Canal semi-annuel)

Cette rubrique explique l’utilisation d’expressions régulières pour les critères spéciaux dans NPS dans Windows Server. Vous pouvez utiliser cette syntaxe pour spécifier les conditions des attributs de stratégie réseau et des domaines RADIUS.

## <a name="pattern-matching-reference"></a>Référence de critères spéciaux

Vous pouvez utiliser le tableau suivant comme source de référence lors de la création d’expressions régulières avec une syntaxe de critères spéciaux. Notez que les modèles d’expressions régulières sont souvent entourés par des barres obliques (/).

|  symbole  |  Description  |   Exemple                                                                 |
| ----------- | ------------- | ------------------------------------------------------------------------  |
|     `\ `     | Indique que le caractère qui suit est un caractère spécial, ou qu’il doit être interprété littéralement.  | `/n/ matches the character "n" while the sequence /\n/ matches a line feed or newline character.`  |
|     `^`     |                                                                 Correspond au début de l’entrée ou de la ligne.                                                                  |                                                                 &nbsp;                                                                  |
|     `$`     |                                                                    Correspond à la fin de l’entrée ou de la ligne.                                                                     |                                                                 &nbsp;                                                                  |
|     `*`     |                                                             Met en correspondance le caractère précédent zéro ou plusieurs fois.                                                              |                                                  `/zo*/ matches either "z" or "zoo."`                                                   |
|     `+`     |                                                              Correspond au caractère précédent une ou plusieurs fois.                                                              |                                                   `/zo+/ matches "zoo" but not "z."`                                                    |
|     `?`     |                                                              Correspond au caractère précédent zéro ou une fois.                                                              |                                                 `/a?ve?/ matches the "ve" in "never."`                                                  |
|     `.`     |                                                           Représente n’importe quel caractère unique, à l’exception d’un caractère de saut de ligne.                                                           |                                                                 &nbsp;                                                                  |
| `(pattern)` |                         Correspond à « pattern » et mémorise la correspondance.<br />Pour faire correspondre les caractères littéraux `(` et `)` (parenthèses), utilisez `\(` ou `\)`.                         |                                                                 &nbsp;                                                                  |
|   `x | y `  |                                                                               Correspond à x ou y.                                                          |
|   `{n} `    |                                                          Correspond exactement n fois \(n est un entier non-no__t-1negative @ no__t-2.                                                           |               `/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`               |
|   `{n,}`    |                                                          Correspond au moins n fois \(n est un entier non-no__t-1negative @ no__t-2.                                                          | `/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.` |
|   `{n,m}`   |                                                Met en correspondance au moins n fois et au plus m fois \(m et n sont des entiers non @ no__t-1negative @ no__t-2.                                                |                               `/o{1,3}/ matches the first three instances of the letter o in "fooooood."`                               |
|   `[xyz]`   |                                                       Correspond à l’un des caractères entourés @no__t le jeu de caractères-0A @ no__t-1.                                                        |                                                  `/[abc]/ matches the "a" in "plain."`                                                  |
|  `[^xyz]`   |                                                  Correspond à tous les caractères qui ne sont pas entourés \(A jeu de caractères négatif @ no__t-1.                                                  |                                                 `/[^abc]/ matches the "p" in "plain."`                                                  |
|    `\b`     |                                                              Correspond à une limite de mot \(for exemple, un espace @ no__t-1.                                                               |                                              `/ea*r\b/ matches the "er" in "never early."`                                              |
|    `\B`     |                                                                         Correspond à une limite de mot.                                                                          |                                             `/ea*r\B/ matches the "ear" in "never early."`                                              |
|    `\d`     |                                                       Correspond à un caractère de chiffre \(equivalent à des chiffres compris entre 0 et 9 @ no__t-1.                                                        |                                                                 &nbsp;                                                                  |
|    `\D`     |                                                           Correspond à un caractère non numérique \(equivalent pour `[^0-9]` @ no__t-2.                                                           |                                                                 &nbsp;                                                                  |
|    `\f`     |                                                                        Correspond à un caractère de saut de forme.                                                                        |                                                                 &nbsp;                                                                  |
|    `\n`     |                                                                        Correspond à un caractère de saut de ligne.                                                                        |                                                                 &nbsp;                                                                  |
|    `\r`     |                                                                     Correspond à un caractère de retour chariot.                                                                     |                                                                 &nbsp;                                                                  |
|    `\s`     |                                   Correspond à n’importe quel caractère d’espace blanc, y compris l’espace, la tabulation et le flux de formulaire \(equivalent pour `[ \f\n\r\t\v]` @ no__t-2.                                   |                                                                 &nbsp;                                                                  |
|    `\S`     |                                                  Correspond à tout caractère autre qu’un espace blanc \(equivalent pour `[^ \f\n\r\t\v]` @ no__t-2.                                                   |                                                                 &nbsp;                                                                  |
|    `\t`     |                                                                           Correspond à un caractère de tabulation.                                                                           |                                                                 &nbsp;                                                                  |
|    `\v`     |                                                                      Correspond à un caractère de tabulation verticale.                                                                       |                                                                 &nbsp;                                                                  |
|    `\w`     |                                              Correspond à n’importe quel caractère de mot, y compris le trait de soulignement \(equivalent à `[A-Za-z0-9_]` @ no__t-2.                                              |                                                                 &nbsp;                                                                  |
|    `\W`     |                                           Correspond à n’importe quel caractère non @ no__t-0word, à l’exclusion du trait de soulignement \(equivalent à `[^A-Za-z0-9_]` @ no__t-3.                                           |                                                                 &nbsp;                                                                  |
|   `\num`    | Fait référence aux correspondances mémorisées \( @ no__t-1, où num est un entier positif @ no__t-2.  Cette option peut être utilisée uniquement dans la zone de texte **remplacer** lors de la configuration de la manipulation d’attributs. |                                       `\1` remplace ce qui est stocké dans la première correspondance mémorisée.                                       |
|   `/n/ `    |                      Autorise l’insertion de codes ASCII dans des expressions régulières \( @ no__t-1, où n est une valeur d’échappement octale, hexadécimale ou décimale @ no__t-2.                       |                                                                 &nbsp;                                                                  |

## <a name="examples-for-network-policy-attributes"></a>Exemples d’attributs de stratégie réseau

Les exemples suivants décrivent l’utilisation de la syntaxe de mise en correspondance de modèle pour spécifier des attributs de stratégie réseau :

- Pour spécifier tous les numéros de téléphone dans l’indicatif de zone 899, la syntaxe est la suivante :

     `899.*`

- Pour spécifier une plage d’adresses IP qui commencent par 192.168.1, la syntaxe est la suivante :

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Exemples de manipulation du nom de domaine dans l’attribut User Name

Les exemples suivants décrivent l’utilisation de la syntaxe de mise en correspondance de modèle pour manipuler des noms de domaine pour l’attribut User Name, qui se trouve sous l’onglet **attribut** dans les propriétés d’une stratégie de demande de connexion.

**Pour supprimer la partie domaine de l’attribut nom d’utilisateur**

Dans un scénario d’accès à distance externalisé dans lequel un fournisseur de services Internet \(ISP @ no__t-1 achemine les demandes de connexion à un serveur NPS de l’organisation, le proxy RADIUS de l’ISP peut nécessiter un nom de domaine pour acheminer la demande d’authentification. Toutefois, le serveur NPS peut ne pas reconnaître la partie nom de domaine du nom d’utilisateur. Par conséquent, le nom de domaine doit être supprimé par le proxy RADIUS du fournisseur de services Internet avant d’être transféré au serveur NPS de l’organisation.

- Rechercher : @microsoft @ no__t-1com

- Remplacez :

**Pour remplacer <em>user@example.microsoft.com</em> par _example. Microsoft. com\user_**

- Rechercher : `(.*)@(.*)`

- Remplacer : `$2\$1`



**Pour remplacer _domaine\utilisateur_ par _specific_domain\user_**

- Rechercher : `(.*)\\(.*)`

- Remplacer : *specific_domain*`\$2`



<strong>Pour remplacer l' *utilisateur* par *user@specific_domain</strong>*

- Rechercher : `$`

- Remplacer : @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Exemple de transfert de messages RADIUS par un serveur proxy

Vous pouvez créer des règles de routage qui transfèrent les messages RADIUS avec un nom de domaine spécifié vers un ensemble de serveurs RADIUS lorsque NPS est utilisé en tant que proxy RADIUS. Voici une syntaxe recommandée pour acheminer les demandes en fonction du nom de domaine.

- **Nom NetBIOS**: `WCOAST`
- **Modèle**: `^wcoast\\`

Dans l’exemple suivant, wcoast.microsoft.com est un suffixe UPN (nom d’utilisateur principal) unique pour le domaine DNS ou Active Directory wcoast.microsoft.com. À l’aide du modèle fourni, le proxy NPS peut acheminer des messages en fonction du nom NetBIOS du domaine ou du suffixe UPN.

- **Nom NetBIOS**: `WCOAST`
- **Suffixe UPN**: `wcoast.microsoft.com`
- **Modèle**: `^wcoast\\|@wcoast\.microsoft\.com$`


Pour plus d’informations sur la gestion de NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
