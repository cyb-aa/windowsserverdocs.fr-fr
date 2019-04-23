---
title: Résolution des problèmes d’AD FS - les règles de revendications
description: Ce document décrit comment résoudre les problèmes de syntaxe de règle de revendications avec AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 027b2afc9e580253ec820e7e5be14419387ddd44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837920"
---
# <a name="ad-fs-troubleshooting---claims-rules-syntax"></a>Résolution des problèmes d’AD FS - syntaxe de règles de revendications
Une revendication est une instruction qu’un sujet fait à propos de lui-même ou un autre objet.  Revendications sont émises par une partie de confiance, ils sont et un ou plusieurs des valeurs empaquetées dans des jetons de sécurité émis par le serveur AD FS.  Cet article porte sur la création et la syntaxe de revendications.  Pour plus d’informations sur les revendications émission consultez [AD FS résolution des problèmes - émission de revendications](ad-fs-tshoot-claims-issuance.md).

>[!NOTE]  
>Vous pouvez utiliser [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) sur le [ADFS aide](https://adfshelp.microsoft.com) site pour aider à résoudre les problèmes de revendications.   

## <a name="how-claim-rules-are-processed"></a>Mode de traitement des règles de revendication
Règles de revendication sont traitées par le biais du [pipeline de revendications](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md) à l’aide de la [moteur de revendications](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md). Le moteur de revendications est un composant logique du service de fédération qui examine le jeu des revendications entrantes présenté par un utilisateur, puis, en fonction de la logique de chaque règle, produit un jeu de revendications de sortie.

## <a name="how-to-create-a-claim-rule"></a>Création d’une règle de revendication
Les règles de revendication sont créées séparément pour chaque relation d’approbation fédérée au sein du service de fédération et ne sont pas partagées par plusieurs approbations. Vous pouvez créer une règle à partir d’un [modèle de règle de revendication](../../ad-fs/technical-reference/determine-the-type-of-claim-rule-template-to-use.md), démarrer à partir de rien en créant la règle à l’aide de la [langage de règle de revendication](../../ad-fs/technical-reference/when-to-use-a-custom-claim-rule.md) ou utiliser Windows PowerShell pour personnaliser une règle.

## <a name="understanding-the-components-of-the-claim-rule-language"></a>Présentation des composants du langage de règle de revendication
Le langage de règle de revendication se compose des éléments suivants, séparés par le « = > « opérateur :

- Une condition - utilisée pour vérifier les revendications d’entrée et de déterminer si l’instruction d’émission de la règle doit être exécutée.  Il représente une expression logique qui doit être évaluée comme « true » pour exécuter la partie du corps de la règle.

- Une instruction d’émission

Exemple :

```c:[type == "Name", value == "domain user"] => issue(type = "Role", value = "employee");``` 

La revendication suivante présente les éléments suivants :
- condition - `c:[type == "Name", value == "domain user"] ` -évalue la revendication d’entrée si le nom du compte windows est un utilisateur de domaine
- émission - `issue(type = "Role", value = "employee")` : si la condition est true, ajoute une nouvelle revendication à la revendication d’entrée avec le rôle d’employé.

Pour plus d’informations sur les revendications et la syntaxe, consultez [The Role of le langage de règle de revendication](../../ad-fs/technical-reference/the-role-of-the-claim-rule-language.md).

## <a name="claims-rule-editor"></a>Éditeur de règles de revendications
Vérification de la syntaxe est effectuée par l’éditeur de règles de revendications une fois que vous avez terminé la revendication et cliquez sur **OK**.  Par conséquent, si vous avez une syntaxe incorrecte puis l’éditeur vous indiquera.

![revendications](media/ad-fs-tshoot-claims/claims1.png)

## <a name="event-logs"></a>Journaux des événements
Lors de la recherche tente de résoudre une revendication en utilisant les journaux la meilleure approche consiste à rechercher pour la sortie de revendications.  Vous pouvez rechercher des événements 1000 et 1001 dans le journal des événements.

![revendications](media/ad-fs-tshoot-claims/claims2.png)

## <a name="creating-a-sample-application"></a>Création d’un exemple d’application
Vous pouvez également créer un exemple d’application les répercute vos revendications.  Par exemple, vous pouvez utiliser un exemple d’application et créer une partie de confiance qui a la même revendication que vous tentez de résoudre les problèmes et de voir si l’application a des problèmes avec celui de revendication.

![revendications](media/ad-fs-tshoot-claims/claim4.png)

Un exemple de bon d’application web est disponible ici.  Cette application est une application web simple qui renvoie les revendications qu’il reçoit de la partie de confiance.  Pour l’utiliser, vous devez modifier l’application web.config par :
- modification https://app1.contoso.com/sampapp à l’URL vous l’utiliserez pour l’hébergement de la sampapp
- modification de toutes les instances de sts.contoso.com pour vous pointer le serveur de fédération AD FS
- En remplaçant l’empreinte numérique avec votre empreinte numérique

![revendications](media/ad-fs-tshoot-claims/claims3.png)

Ce qui suit [l’article de blog](https://blogs.technet.microsoft.com/tangent_thoughts/2015/02/20/install-and-configure-a-simple-net-4-5-sample-federated-application-samapp/) a excellente instructions détaillées, pour cette configuration.

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes de AD FS](ad-fs-tshoot-overview.md)