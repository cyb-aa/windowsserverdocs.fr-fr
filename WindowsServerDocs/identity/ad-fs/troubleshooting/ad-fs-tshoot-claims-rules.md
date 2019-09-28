---
title: Résolution des problèmes de AD FS-règles de revendication
description: Ce document décrit comment résoudre les problèmes de syntaxe de la règle de revendication avec AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d0146ba7cfc736f4d37ca66d58d624cc2f7f9a23
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366277"
---
# <a name="ad-fs-troubleshooting---claims-rules-syntax"></a>Résolution des problèmes de AD FS-syntaxe des règles de revendication
Une revendication est une déclaration qu’un sujet fait à propos de lui-même ou d’un autre objet.  Les revendications sont émises par une partie de confiance, et elles reçoivent une ou plusieurs valeurs, puis empaquetées dans des jetons de sécurité émis par le serveur AD FS.  Cet article traite de la syntaxe et de la création des revendications.  Pour plus d’informations sur l’émission de revendications [, consultez AD FS résolution des problèmes liés à l’émission de revendications](ad-fs-tshoot-claims-issuance.md).

>[!NOTE]  
>Vous pouvez utiliser [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) sur le site d' [aide ADFS](https://adfshelp.microsoft.com) pour vous aider à résoudre les problèmes liés aux revendications.   

## <a name="how-claim-rules-are-processed"></a>Mode de traitement des règles de revendication
Les règles de revendication sont traitées via le [pipeline](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md) de revendications à l’aide du [moteur de revendications](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md). Le moteur de revendications est un composant logique du service de fédération qui examine le jeu des revendications entrantes présenté par un utilisateur, puis, en fonction de la logique de chaque règle, produit un jeu de revendications de sortie.

## <a name="how-to-create-a-claim-rule"></a>Création d’une règle de revendication
Les règles de revendication sont créées séparément pour chaque relation d’approbation fédérée au sein du service de fédération et ne sont pas partagées par plusieurs approbations. Vous pouvez créer une règle à partir d’un [modèle de règle de revendication](../../ad-fs/technical-reference/determine-the-type-of-claim-rule-template-to-use.md), démarrer à partir de zéro en créant la règle à l’aide du langage de règle de [revendication](../../ad-fs/technical-reference/when-to-use-a-custom-claim-rule.md) ou utiliser Windows PowerShell pour personnaliser une règle.

## <a name="understanding-the-components-of-the-claim-rule-language"></a>Présentation des composants du langage de règle de revendication
Le langage de règle de revendication se compose des composants suivants, séparés par l’opérateur « = > » :

- Condition : utilisée pour vérifier les revendications d’entrée et déterminer si l’instruction d’émission de la règle doit être exécutée.  Il représente une expression logique qui doit être évaluée à true pour exécuter le corps de la règle.

- Une instruction d’émission

Exemple :

```c:[type == "Name", value == "domain user"] => issue(type = "Role", value = "employee");``` 

La revendication suivante présente les éléments suivants :
- condition-`c:[type == "Name", value == "domain user"] ` : évalue la revendication d’entrée indiquant si le nom du compte Windows est un utilisateur de domaine
- émission-`issue(type = "Role", value = "employee")`-si la condition est true, ajoute une nouvelle revendication à la revendication d’entrée avec le rôle Employee.

Pour plus d’informations sur les revendications et la syntaxe, consultez [le rôle du langage de règle de revendication](../../ad-fs/technical-reference/the-role-of-the-claim-rule-language.md).

## <a name="claims-rule-editor"></a>Éditeur de règles de revendication
La vérification de la syntaxe est effectuée par l’éditeur de règles de revendication une fois que vous avez terminé la revendication et cliqué sur **OK**.  Par conséquent, si vous avez une syntaxe incorrecte, l’éditeur vous en informera.

![claims](media/ad-fs-tshoot-claims/claims1.png)

## <a name="event-logs"></a>Journaux des événements
Lorsque vous tentez de dépanner une revendication à l’aide des journaux, la meilleure approche consiste à rechercher la sortie des revendications.  Vous pouvez rechercher les événements 1000 et 1001 dans le journal des événements.

![claims](media/ad-fs-tshoot-claims/claims2.png)

## <a name="creating-a-sample-application"></a>Création d’un exemple d’application
Vous pouvez également créer un exemple d’application qui signale vos revendications.  Par exemple, vous pouvez utiliser un exemple d’application et créer une partie de confiance qui a la même revendication que celle que vous tentez de dépanner et voir si l’application rencontre des problèmes avec cette revendication.

![claims](media/ad-fs-tshoot-claims/claim4.png)

Un bon exemple d’application Web est disponible ici.  Cette application est une application Web simple qui renvoie en arrière-plan les revendications qu’elle reçoit de la partie de confiance.  Pour pouvoir utiliser cela, vous devez modifier l’application Web. config en :
- en remplaçant https://app1.contoso.com/sampapp par l’URL que vous utiliserez pour héberger le sampapp
- modification de toutes les instances de sts.contoso.com pour pointer vers vous AD FS serveur de Fédération
- Remplacement de l’empreinte numérique par votre empreinte numérique

![claims](media/ad-fs-tshoot-claims/claims3.png)

L' [article de blog](https://blogs.technet.microsoft.com/tangent_thoughts/2015/02/20/install-and-configure-a-simple-net-4-5-sample-federated-application-samapp/) suivant contient des instructions remarquables et détaillées permettant de configurer cela.

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)