---
title: Utiliser des Expressions régulières dans NPS
description: Cette rubrique décrit l’utilisation d’expressions régulières pour la correspondance au modèle dans le serveur NPS dans Windows Server2016. Vous pouvez utiliser cette syntaxe pour spécifier les conditions d’attributs de la stratégie réseau et de domaines RADIUS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f84173f1f51be9fd44995dc41f759bbea4fb3539
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="use-regular-expressions-in-nps"></a>Utiliser des Expressions régulières dans NPS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique décrit l’utilisation d’expressions régulières pour la correspondance au modèle dans le serveur NPS dans Windows Server2016. Vous pouvez utiliser cette syntaxe pour spécifier les conditions d’attributs de la stratégie réseau et de domaines RADIUS.

## <a name="pattern-matching-reference"></a>Correspondance de référence

Vous pouvez utiliser le tableau suivant en tant que référence source lors de la création des expressions régulières avec la syntaxe de correspondance.

|Caractère|Description|Exemple|
|---------|-----------|-------|
|`\`  |Marque le caractère suivant en tant que caractère pour faire correspondre. |`/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`  |
|`^`  |Correspond au début de l’entrée ou la ligne. | &nbsp; |
|`$`  |Correspond à la fin de l’entrée ou la ligne. | &nbsp; |
|`*`  |Correspond au caractère précédent zéro ou plusieurs fois. |`/zo*/ matches either "z" or "zoo."` |
|`+`  |Correspond au caractère précédent une ou plusieurs fois. |`/zo+/ matches "zoo" but not "z."` |
|`?`  |Correspond aux fois de caractère zéro ou un précédent. |`/a?ve?/ matches the "ve" in "never."` |
|`.`  |Correspond à n’importe quel caractère unique à l’exception d’un caractère de saut de ligne.  | &nbsp; |
|`( pattern )`  |Correspond à «modèle» et mémorise la correspondance.   |`To match ( ) (parentheses), use "\(" or "\)".`  |
|«x | y»  |Correspond à x ou y.  |«/ z|cuisine? / correspond à «zoo» ou «nourriture». ` |
|`{ n } `  |Correspond à exactement n fois \ (n est un integer\ autre que celle négatif).  |`/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`  |
|`{ n ,}`  |Correspond au moins n fois \ (n est un integer\ autre que celle négatif).  |`/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.`  |
|`{ n , m }`  |Correspond au moins n et au plus m fois \ (m et n sont integers\ autre que celle négatif).  |`/o{1,3}/ matches the first three instances of the letter o in "fooooood."`  |
|`[ xyz ]`  |Correspond à n’importe quel caractère inclus \(a character set\).  |`/[abc]/ matches the "a" in "plain."`  |
|`[^ xyz ]`  |Correspond à tous les caractères qui ne sont pas placés \ (un set caractères négatif).  |`/[^abc]/ matches the "p" in "plain."`  |
|`\b`  |Correspond à une limite de word \ (par exemple, un espace disque\).  |`/ea*r\b/ matches the "er" in "never early."`  |
|`\B`  |Correspond à une limite non littérale.  |`/ea*r\B/ matches the "ear" in "never early."`  |
|`\d`  |Correspond à un chiffre \ (équivalent à chiffres de 0 à 9\).  | &nbsp; |
|`\D`  |Correspond à un caractère non numérique \ (équivalent à `[^0-9]`\).  | &nbsp; |
|`\f`  |Correspond à un caractère de saut.  | &nbsp; |
|`\n`  |Correspond à une saut de ligne.  | &nbsp; |
|`\r`  |Correspond à un retour chariot.  | &nbsp; |
|`\s`  |Correspond à n’importe quel espace blanc notamment espace, onglet et saut \ (équivalent à `[ \f\n\r\t\v]`\).  | &nbsp; |
|`\S`  |Correspond à n’importe quel espace blanc non-\ (équivalent à `[^ \f\n\r\t\v]`\).  | &nbsp; |
|`\t`  |Correspond à un caractère de tabulation.  | &nbsp; |
|`\v`  |Correspond à un caractère de tabulation verticale.  | &nbsp; |
|`\w`  |Correspond à tout caractère alphabétique, y compris le trait de soulignement \ (équivalent à `[A-Za-z0-9_]`\).  | &nbsp; |
|`\W`  |Correspond à n’importe quel caractère autre que celle-word, à l’exclusion de trait de soulignement \ (équivalent à `[^A-Za-z0-9_]`\).  | &nbsp; |
|`\ num`  |Fait référence à des correspondances mémorisées \ (`?num`, où num est un integer\ positif).  Cette option peut être utilisée uniquement dans les **remplacer** zone de texte lors de la configuration de manipulation d’attribut.| `\1` remplace ce qui est stocké dans la première correspondance à retenir.  |
|`/ n / `  |Permet l’insertion de codes ASCII dans des expressions régulières \ (`?n`, où n est un value\ d’échappement octale, hexadécimal ou décimal).  | &nbsp; |

## <a name="examples-for-network-policy-attributes"></a>Exemples d’attributs de la stratégie réseau

Les exemples suivants décrivent l’utilisation de la syntaxe des critères spéciaux pour spécifier les attributs de la stratégie réseau:

- Pour spécifier les numéros de téléphone dans l’indicatif régional 899, la syntaxe est:

     `899.*`

- Pour spécifier une plage d’adresses IP qui commencent par 192.168.1, la syntaxe est:

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Exemples de manipulation du nom de domaine dans l’attribut de nom d’utilisateur

Les exemples suivants décrivent l’utilisation de la syntaxe des critères spéciaux pour manipuler les noms de domaine pour l’attribut de nom d’utilisateur, qui se trouve sur le **attribut** onglet dans les propriétés d’une stratégie de demande de connexion.

**Pour supprimer la partie domaine de l’attribut de nom d’utilisateur**

Dans un scénario d’accès à distance externalisé dans laquelle une connexion itinéraires \(ISP\) de fournisseur de services Internet demande à un serveur NPS organisation, le proxy RADIUS du fournisseur de services Internet peut-être nécessiter un nom de domaine pour acheminer la demande d’authentification. Toutefois, le serveur NPS ne reconnaisse pas la partie du nom de domaine du nom d’utilisateur. Par conséquent, le nom de domaine doit être supprimé par le proxy RADIUS du fournisseur de services Internet avant qu’il soit transféré au serveur NPS de l’organisation.

- Rechercher:@microsoft\.com

- Remplacez:

**Pour remplacer * user@example.microsoft.com * avec *example.microsoft.com\user***

- Rechercher:`(.*)@(.*)`

- Remplacez:`$2\$1`



**Pour remplacer *domaine\utilisateur* avec *specific_domain\user***

- Rechercher:`(.*)\\(.*)`

- Remplacer: *domaine_spécifique*`\$2`



**Pour remplacer *utilisateur* avec*user@specific_domain***

- Rechercher:`$`

- Remplacer: @*domaine_spécifique*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Exemple de transfert des messages par un serveur proxy RADIUS

Vous pouvez créer des règles de routage qui transfèrent les messages RADIUS avec un nom de domaine Kerberos spécifié à un ensemble de serveurs RADIUS lorsque NPS est utilisé en tant que proxy RADIUS. Voici une syntaxe recommandée pour router les requêtes basées sur le nom de domaine.

- **Nom NetBIOS**: `WCOAST`
- **Modèle**:      `^wcoast\\`

Dans l’exemple suivant, wcoast.microsoft.com est un suffixe de nom principal (UPN) utilisateur unique pour le domaine ActiveDirectory ou DNS wcoast.microsoft.com. L’utilisation du modèle fourni, le proxy de serveur NPS de router les messages basés sur le nom de domaine NetBIOS ou le suffixe UPN.

- **Nom NetBIOS**: `WCOAST`
- **Suffixe UPN**:   `wcoast.microsoft.com`
- **Modèle**:      `^wcoast\\|@wcoast\.microsoft\.com$`


Pour plus d’informations sur la gestion du serveur NPS, voir [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).
