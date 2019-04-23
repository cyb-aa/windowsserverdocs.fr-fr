---
title: Utiliser des expressions régulières dans NPS
description: Cette rubrique explique l’utilisation d’expressions régulières pour les critères spéciaux dans le serveur NPS dans Windows Server 2016. Vous pouvez utiliser cette syntaxe pour spécifier les conditions d’attributs de stratégie de réseau et de domaines RADIUS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a985df9fea31e5ee180caef4e69899ae8468ff71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865260"
---
# <a name="use-regular-expressions-in-nps"></a>Utiliser des expressions régulières dans NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique explique l’utilisation d’expressions régulières pour les critères spéciaux dans le serveur NPS dans Windows Server 2016. Vous pouvez utiliser cette syntaxe pour spécifier les conditions d’attributs de stratégie de réseau et de domaines RADIUS.

## <a name="pattern-matching-reference"></a>Référence de mise en correspondance de modèle

Vous pouvez utiliser le tableau suivant comme une source de référence lors de la création d’expressions régulières avec la syntaxe des critères spéciaux.

|Caractère|Description|Exemple|
|---------|-----------|-------|
|`\`  |Marque le caractère suivant comme un caractère à faire correspondre. |`/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`  |
|`^`  |Correspond au début de l’entrée ou de la ligne. | &nbsp; |
|`$`  |Correspond à la fin de l’entrée ou de la ligne. | &nbsp; |
|`*`  |Correspond au caractère précédent zéro ou plusieurs fois. |`/zo*/ matches either "z" or "zoo."` |
|`+`  |Correspond au caractère précédent une ou plusieurs fois. |`/zo+/ matches "zoo" but not "z."` |
|`?`  |Correspond à la précédente caractère zéro ou une fois. |`/a?ve?/ matches the "ve" in "never."` |
|`.`  |Correspond à n’importe quel caractère unique à l’exception d’un caractère de saut de ligne.  | &nbsp; |
|`(pattern)`  |Correspond à « modèle » et mémorise la correspondance.<br />Pour faire correspondre les caractères littéraux `(` et `)` (parenthèses), utilisez `\(` ou `\)`.   | &nbsp;  |
|`x|y `  |Correspond à x ou y.  |`/z|food?/ matches "zoo" or "food."` |
|`{n} `  |Correspond à exactement n occurrences \(n est un texte non\-entier négatif\).  |`/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`  |
|`{n,}`  |Correspond au moins n occurrences \(n est un texte non\-entier négatif\).  |`/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.`  |
|`{n,m}`  |Correspond au moins n et au maximum m fois \(m et n sont non-Windows\-entiers négatifs\).  |`/o{1,3}/ matches the first three instances of the letter o in "fooooood."`  |
|`[xyz]`  |Correspond à l’un des caractères entre \(un jeu de caractères\).  |`/[abc]/ matches the "a" in "plain."`  |
|`[^xyz]`  |Correspond à tous les caractères qui ne sont pas entourés \(un jeu de caractères négatif\).  |`/[^abc]/ matches the "p" in "plain."`  |
|`\b`  |Correspond à une limite de mot \(, par exemple, un espace\).  |`/ea*r\b/ matches the "er" in "never early."`  |
|`\B`  |Correspond à une limite non alphabétique.  |`/ea*r\B/ matches the "ear" in "never early."`  |
|`\d`  |Correspond à un chiffre \(correspondant aux chiffres de 0 à 9\).  | &nbsp; |
|`\D`  |Correspond à un caractère non numérique \(équivalent à `[^0-9]` \).  | &nbsp; |
|`\f`  |Correspond à un caractère de saut.  | &nbsp; |
|`\n`  |Correspond à un retour chariot.  | &nbsp; |
|`\r`  |Correspond à un caractère de retour chariot.  | &nbsp; |
|`\s`  |Correspond à n’importe quel caractère d’espace blanc, y compris d’espace, tabulation et saut de page \(équivalent à `[ \f\n\r\t\v]` \).  | &nbsp; |
|`\S`  |Correspond à n’importe quel caractère autre qu’un espace blanc \(équivalent à `[^ \f\n\r\t\v]` \).  | &nbsp; |
|`\t`  |Correspond à un caractère de tabulation.  | &nbsp; |
|`\v`  |Correspond à un caractère de tabulation verticale.  | &nbsp; |
|`\w`  |Correspond à n’importe quel caractère alphabétique, y compris un trait de soulignement \(équivalent à `[A-Za-z0-9_]` \).  | &nbsp; |
|`\W`  |Correspond à toute autre\-caractère alphabétique, à l’exclusion du caractère de soulignement \(équivalent à `[^A-Za-z0-9_]` \).  | &nbsp; |
|`\num`  |Fait référence à des correspondances mémorisées \( `?num`, où (pavé numérique) est un entier positif\).  Cette option peut être utilisée uniquement dans le **remplacer** zone de texte lors de la manipulation de l’attribut de configuration.| `\1` remplace ce qui est stocké dans la première correspondance mémorisée.  |
|`/n/ `  |Permet l’insertion de codes ASCII dans les expressions régulières \( `?n`, où n est une valeur d’échappement octal, hexadécimal ou décimal\).  | &nbsp; |

## <a name="examples-for-network-policy-attributes"></a>Exemples pour les attributs de stratégie réseau

Les exemples suivants décrivent l’utilisation de la syntaxe des critères spéciaux pour spécifier les attributs de la stratégie réseau :

- Pour spécifier tous les numéros de téléphone dans l’indicatif régional 899, la syntaxe est :

     `899.*`

- Pour spécifier une plage d’adresses IP qui commencent par 192.168.1, la syntaxe est :

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Exemples pour la manipulation du nom de domaine dans l’attribut de nom d’utilisateur

Les exemples suivants décrivent l’utilisation de la syntaxe des critères spéciaux pour manipuler des noms de domaine pour l’attribut de nom d’utilisateur, ce qui se trouve sur le **attribut** onglet dans les propriétés d’une stratégie de demande de connexion.

**Pour supprimer la partie domaine de l’attribut de nom d’utilisateur**

Dans un scénario d’accès à distance externalisé dans lequel les fournisseur de services Internet \(ISP\) achemine les demandes de connexion à une organisation NPS, le proxy RADIUS du fournisseur de services Internet peut nécessiter un nom de domaine pour acheminer la demande d’authentification. Toutefois, le serveur NPS ne reconnaît pas la partie du nom de domaine du nom d’utilisateur. Par conséquent, le nom de domaine doit être supprimé par le proxy RADIUS du fournisseur de services Internet avant qu’il est transféré à l’organisation NPS.

- Rechercher : @microsoft \.com

- Remplacez :

**Pour remplacer *user@example.microsoft.com* avec *example.microsoft.com\user***

- Rechercher :`(.*)@(.*)`

- Remplacez :`$2\$1`



**Pour remplacer *domaine\utilisateur* avec *specific_domain\user***

- Rechercher :`(.*)\\(.*)`

- Remplacement : *domaine_spécifique*`\$2`



**Pour remplacer *utilisateur* avec *user@specific_domain***

- Rechercher :`$`

- Remplacement : @*domaine_spécifique*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Exemple pour le transfert des messages RADIUS par un serveur proxy

Vous pouvez créer des règles de routage qui transfèrent les messages RADIUS avec un nom de domaine Kerberos spécifié à un ensemble de serveurs RADIUS lorsque NPS est utilisé en tant que proxy RADIUS. Voici une syntaxe recommandée pour les demandes de routage basé sur le nom de domaine.

- **Nom NetBIOS**: `WCOAST`
- **Modèle**:      `^wcoast\\`

Dans l’exemple suivant, wcoast.microsoft.com est un suffixe de nom principal (UPN) d’utilisateur unique pour le wcoast.microsoft.com de domaine Active Directory ou DNS. À l’aide du modèle fourni, le proxy de serveur NPS peut acheminer les messages sur le nom NetBIOS du domaine ou le suffixe UPN.

- **Nom NetBIOS**: `WCOAST`
- **Suffixe UPN**:   `wcoast.microsoft.com`
- **Modèle**:      `^wcoast\\|@wcoast\.microsoft\.com$`


Pour plus d’informations sur la gestion de serveur NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
