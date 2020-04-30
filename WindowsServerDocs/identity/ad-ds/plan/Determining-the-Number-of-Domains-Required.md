---
ms.assetid: 87bca912-b912-4bbe-9533-2c34a7abc52d
title: Détermination du nombre de domaines nécessaires
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2ed2a6b601ee2cabd45fd5170764c812307b6ac1
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624267"
---
# <a name="determining-the-number-of-domains-required"></a>Détermination du nombre de domaines nécessaires

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Chaque forêt commence par un domaine unique. Le nombre maximal d’utilisateurs qu’une forêt de domaine unique peut contenir est basé sur le lien le plus lent qui doit prendre en charge la réplication entre les contrôleurs de domaine et la bande passante disponible que vous souhaitez allouer à Active Directory Domain Services (AD DS). Le tableau suivant répertorie le nombre maximal recommandé d’utilisateurs qu’un domaine peut contenir en fonction d’une forêt de domaine unique, de la vitesse du lien le plus lent et du pourcentage de bande passante que vous souhaitez réserver pour la réplication. Ces informations s’appliquent aux forêts qui contiennent un maximum de 100 000 utilisateurs et qui ont une connectivité de 28,8 kilobits par seconde (Kbits/s) ou plus. Pour obtenir des recommandations qui s’appliquent aux forêts qui contiennent plus de 100 000 utilisateurs ou une connectivité inférieure à 28,8 Kbits/s, consultez un Active Directory designer expérimenté. Les valeurs du tableau suivant sont basées sur le trafic de réplication généré dans un environnement qui présente les caractéristiques suivantes :

- Les nouveaux utilisateurs rejoignent la forêt à un débit de 20% par an.
- Les utilisateurs laissent la forêt à un débit de 15% par an.
- Chaque utilisateur est membre de cinq groupes globaux et de cinq groupes universels.
- Le ratio entre les utilisateurs et les ordinateurs est de 1:1.
- Le système de noms de domaine (DNS, Domain Name System) intégré à Active Directory est utilisé.
- Le nettoyage DNS est utilisé.

> [!NOTE]
> Les figures répertoriées dans le tableau suivant sont des approximations. La quantité de trafic de réplication dépend en grande partie du nombre de modifications apportées à l’annuaire dans un laps de temps donné. Vérifiez que votre réseau peut prendre en charge votre trafic de réplication en testant la quantité estimée et le taux de modifications de votre conception dans un laboratoire avant de déployer vos domaines.

|Liaison la plus lente connectant un contrôleur de domaine (kbps)|Nombre maximal d’utilisateurs si une bande passante de 1% est disponible|Nombre maximal d’utilisateurs si la bande passante de 5% est disponible|Nombre maximal d’utilisateurs si la bande passante de 10% est disponible|
| --- | --- | --- | --- |
|28.8|10 000|25 000|40 000|
|32|10 000|25 000|50 000|
|56|10 000|50 000|100 000|
|64|10 000|50 000|100 000|
|128|25 000|100 000|100 000|
|256|50 000|100 000|100 000|
|512|80 000|100 000|100 000|
|1 500|100 000|100 000|100 000|

Pour utiliser cette table :

1. Dans le **lien le plus lent connectant une colonne de contrôleur de domaine** , localisez la valeur qui correspond à la vitesse du lien le plus lent sur lequel AD DS sera répliqué dans votre domaine.

2. Dans la ligne qui correspond à la vitesse de liaison la plus lente, localisez la colonne qui représente le pourcentage de bande passante que vous souhaitez allouer à AD DS. La valeur à cet emplacement correspond au nombre maximal d’utilisateurs que le domaine d’une forêt de domaine unique peut contenir.

Si vous déterminez que le nombre total d’utilisateurs dans votre forêt est inférieur au nombre maximal d’utilisateurs que votre domaine peut contenir, vous pouvez utiliser un seul domaine. Veillez à prendre en compte la croissance future planifiée lorsque vous effectuez cette détermination. Si vous déterminez que le nombre total d’utilisateurs dans votre forêt est supérieur au nombre maximal d’utilisateurs que votre domaine peut contenir, vous devez réserver un pourcentage plus élevé de bande passante pour la réplication, augmenter votre vitesse de liaison ou diviser votre organisation en domaines régionaux.

## <a name="dividing-the-organization-into-regional-domains"></a>Division de l’organisation en domaines régionaux

Si vous ne pouvez pas prendre en charge tous vos utilisateurs dans un domaine unique, vous devez sélectionner le modèle de domaine régional. Divisez votre organisation en régions d’une manière adaptée à votre organisation et à votre réseau existant. Par exemple, vous pouvez créer des régions basées sur des limites continentales.

Notez que, étant donné que vous devez créer un domaine Active Directory pour chaque région que vous établissez, nous vous recommandons de réduire le nombre de régions que vous définissez pour AD DS. Bien qu’il soit possible d’inclure un nombre illimité de domaines dans une forêt, il est recommandé de ne pas inclure plus de 10 domaines dans une forêt. Vous devez établir l’équilibre approprié entre l’optimisation de la bande passante de réplication et la réduction de votre complexité administrative lors de la Division de votre organisation en domaines régionaux.

Tout d’abord, déterminez le nombre maximal d’utilisateurs que votre forêt peut héberger. Basez-le sur le lien le plus lent dans la forêt sur lequel les contrôleurs de domaine seront répliqués et la quantité moyenne de bande passante que vous souhaitez allouer à Active Directory réplication. Le tableau suivant répertorie le nombre maximal d’utilisateurs recommandés qu’une forêt peut contenir. Cela est basé sur la vitesse du lien le plus lent et le pourcentage de bande passante que vous souhaitez réserver pour la réplication. Ces informations s’appliquent aux forêts qui contiennent un maximum de 100 000 utilisateurs et qui ont une connectivité de 28,8 Kbits/s ou plus. Les valeurs du tableau suivant sont basées sur les hypothèses suivantes :

- Tous les contrôleurs de domaine sont des serveurs de catalogue global.
- Les nouveaux utilisateurs rejoignent la forêt à un débit de 20% par an.
- Les utilisateurs laissent la forêt à un débit de 15% par an.
- Les utilisateurs sont membres de cinq groupes globaux et de cinq groupes universels.
- Le ratio entre les utilisateurs et les ordinateurs est de 1:1.
- Le DNS intégré à l’Active Directory est utilisé.
- Le nettoyage DNS est utilisé.

> [!NOTE]
> Les figures répertoriées dans le tableau suivant sont des approximations. La quantité de trafic de réplication dépend en grande partie du nombre de modifications apportées à l’annuaire dans un laps de temps donné. Vérifiez que votre réseau peut prendre en charge votre trafic de réplication en testant la quantité estimée et le taux de modifications de votre conception dans un laboratoire avant de déployer vos domaines.

|Liaison la plus lente connectant un contrôleur de domaine (kbps)|Nombre maximal d’utilisateurs si une bande passante de 1% est disponible|Nombre maximal d’utilisateurs si la bande passante de 5% est disponible|Nombre maximal d’utilisateurs si la bande passante de 10% est disponible|
| --- | --- | --- | --- |
|28.8|10 000|50 000|75 000|
|32|10 000|50 000|75 000|
|56|10 000|75 000|100 000|
|64|25 000|75 000|100 000|
|128|50 000|100 000|100 000|
|256|75 000|100 000|100 000|
|512|100 000|100 000|100 000|
|1 500|100 000|100 000|100 000|

Pour utiliser cette table :

1. Dans le **lien le plus lent connectant une colonne de contrôleur de domaine** , localisez la valeur qui correspond à la vitesse du lien le plus lent sur lequel AD DS sera répliqué dans votre forêt.

2. Dans la ligne qui correspond à la vitesse de liaison la plus lente, localisez la colonne qui représente le pourcentage de bande passante que vous souhaitez allouer à AD DS. La valeur à cet emplacement correspond au nombre maximal d’utilisateurs que votre forêt peut héberger.

Si le nombre maximal d’utilisateurs que votre forêt peut héberger est supérieur au nombre d’utilisateurs que vous devez héberger, une seule forêt fonctionnera pour votre conception. Si vous devez héberger plus d’utilisateurs que le nombre maximal que vous avez identifié, vous devez augmenter la vitesse de liaison minimale, allouer un pourcentage plus élevé de bande passante pour AD DS ou déployer des forêts supplémentaires.

Si vous déterminez qu’une seule forêt prend en charge le nombre d’utilisateurs que vous devez héberger, l’étape suivante consiste à déterminer le nombre maximal d’utilisateurs que chaque région peut prendre en charge en fonction du lien le plus lent situé dans cette région. Divisez votre forêt en régions qui ont un sens pour vous. Veillez à baser votre décision sur un événement qui n’est pas susceptible de changer. Par exemple, utilisez des continents à la place des régions de vente. Ces régions sont la base de votre structure de domaine lorsque vous avez identifié le nombre maximal d’utilisateurs.

Déterminez le nombre d’utilisateurs qui doivent être hébergés dans chaque région, puis vérifiez qu’ils ne dépassent pas la valeur maximale autorisée en fonction de la vitesse de liaison la plus lente et de la bande passante allouée à AD DS dans cette région. Le tableau suivant répertorie le nombre maximal d’utilisateurs recommandés qu’un domaine régional peut contenir. Elle est basée sur la vitesse du lien le plus lent et le pourcentage de bande passante que vous souhaitez réserver pour la réplication. Ces informations s’appliquent aux forêts qui contiennent un maximum de 100 000 utilisateurs et qui ont une connectivité de 28,8 Kbits/s ou plus. Les valeurs du tableau suivant sont basées sur les hypothèses suivantes :

- Tous les contrôleurs de domaine sont des serveurs de catalogue global.
- Les nouveaux utilisateurs rejoignent la forêt à un débit de 20% par an.
- Les utilisateurs laissent la forêt à un débit de 15% par an.
- Les utilisateurs sont membres de cinq groupes globaux et de cinq groupes universels.
- Le ratio entre les utilisateurs et les ordinateurs est de 1:1.
- Le DNS intégré à l’Active Directory est utilisé.
- Le nettoyage DNS est utilisé.

> [!NOTE]
> Les figures répertoriées dans le tableau suivant sont des approximations. La quantité de trafic de réplication dépend en grande partie du nombre de modifications apportées à l’annuaire dans un laps de temps donné. Vérifiez que votre réseau peut prendre en charge votre trafic de réplication en testant la quantité estimée et le taux de modifications de votre conception dans un laboratoire avant de déployer vos domaines.

|Liaison la plus lente connectant un contrôleur de domaine (kbps)|Nombre maximal d’utilisateurs si une bande passante de 1% est disponible|Nombre maximal d’utilisateurs si la bande passante de 5% est disponible|Nombre maximal d’utilisateurs si la bande passante de 10% est disponible|
| --- | --- | --- | --- |
|28.8|10 000|18 000|40 000|
|32|10 000|20 000|50 000|
|56|10 000|40 000|100 000|
|64|10 000|50 000|100 000|
|128|15,000|100 000|100 000|
|256|30,000|100 000|100 000|
|512|80 000|100 000|100 000|
|1 500|100 000|100 000|100 000|

Pour utiliser cette table :

1. Dans le **lien le plus lent connectant une colonne de contrôleur de domaine** , localisez la valeur qui correspond à la vitesse du lien le plus lent sur lequel AD DS sera répliqué dans votre région.

2. Dans la ligne qui correspond à la vitesse de liaison la plus lente, localisez la colonne qui représente le pourcentage de bande passante que vous souhaitez allouer à AD DS. Cette valeur représente le nombre maximal d’utilisateurs que la région peut héberger.

Évaluez chaque région proposée et déterminez si le nombre maximal d’utilisateurs dans chaque région est inférieur au nombre maximal d’utilisateurs recommandé qu’un domaine peut contenir. Si vous déterminez que la région peut héberger le nombre d’utilisateurs dont vous avez besoin, vous pouvez créer un domaine pour cette région. Si vous déterminez que vous ne pouvez pas héberger un grand nombre d’utilisateurs, envisagez de diviser votre conception en plus petites régions et en recalculant le nombre maximal d’utilisateurs pouvant être hébergés dans chaque région. Les autres alternatives sont l’allocation de bande passante supplémentaire ou l’augmentation de la vitesse de liaison.

Même si le nombre total d’utilisateurs que vous pouvez placer dans un domaine dans une forêt à plusieurs domaines est plus petit que le nombre d’utilisateurs dans le domaine dans une forêt à domaine unique, le nombre total d’utilisateurs dans la forêt à plusieurs domaines peut être plus élevé. Le plus petit nombre d’utilisateurs par domaine dans une forêt à plusieurs domaines tient compte de la surcharge de réplication supplémentaire créée en conservant le catalogue global dans cet environnement. Pour obtenir des recommandations qui s’appliquent aux forêts qui contiennent plus de 100 000 utilisateurs ou une connectivité inférieure à 28,8 Kbits/s, consultez un Active Directory designer expérimenté.

## <a name="documenting-the-regions-identified"></a>Documentation des régions identifiées

Après avoir divisé votre organisation en domaines régionaux, documentez les régions que vous souhaitez représenter et le nombre d’utilisateurs qui se trouvent dans chaque région. En outre, notez la vitesse des liens les plus lents dans chaque région que vous allez utiliser pour la réplication Active Directory. Ces informations sont utilisées pour déterminer si des domaines ou des forêts supplémentaires sont requis.

Pour obtenir une feuille de calcul qui vous aide à documenter les régions que vous avez identifiées, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip à partir des [Outils d’aide à la tâche pour le kit de déploiement de Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608) et ouvrez « régions d’identification » (DSSLOGI_4. doc).
