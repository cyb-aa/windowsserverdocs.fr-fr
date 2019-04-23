---
ms.assetid: 87bca912-b912-4bbe-9533-2c34a7abc52d
title: Détermination du nombre de domaines nécessaires
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e5d6369ca91c1d48375c22238d1e706576df7fbf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843520"
---
# <a name="determining-the-number-of-domains-required"></a>Détermination du nombre de domaines nécessaires

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Chaque forêt démarre avec un seul domaine. Le nombre maximal d’utilisateurs une forêt à domaine unique peut contenir est basé sur le lien plus lent qui doit de prendre en charge la réplication entre contrôleurs de domaine et de la bande passante disponible que vous souhaitez allouer aux Services de domaine Active Directory (AD DS). Le tableau suivant répertorie le nombre maximal recommandé d’utilisateurs contenant un domaine sur une forêt de domaine unique, la vitesse de la liaison la plus lente et le pourcentage de bande passante que vous souhaitez réserver pour la réplication. Ces informations s’appliquent aux forêts qui contiennent un maximum de 100 000 utilisateurs et qui ont une connectivité 28,8 kilobits par seconde (Kbits/s) ou version ultérieure. Pour obtenir des recommandations qui s’appliquent aux forêts qui contiennent plus de 100 000 utilisateurs ou de connectivité d’inférieur à. à 28,8 Kbits/s, consultez un concepteur expérimenté d’Active Directory. Les valeurs dans le tableau suivant sont basées sur le trafic de réplication généré dans un environnement qui présente les caractéristiques suivantes :  
  
- Nouveaux utilisateurs joindre la forêt à une fréquence de 20 % par an.  
- Les utilisateurs laissent la forêt à une fréquence de 15 pourcent par an.  
- Chaque utilisateur est membre de cinq groupes globaux et de cinq groupes universels.  
- Le rapport entre les utilisateurs sur des ordinateurs est 1:1.  
- Active Directory-intégré système DNS (Domain Name) est utilisé.  
- Le nettoyage DNS est utilisé.  

> [!NOTE]  
> Les valeurs répertoriées dans le tableau suivant sont des approximations. La quantité de trafic de réplication dépend en grande partie le nombre de modifications apportées à l’annuaire dans un laps de temps donné. Vérifiez que votre réseau peut gérer votre trafic de réplication en testant la quantité estimée et le taux de modifications sur votre conception dans un laboratoire avant de déployer vos domaines.  
  
|Plus lente liaison connectant un contrôleur de domaine (Kbits/s)|Nombre maximal d’utilisateurs si la bande passante de 1 % est disponible|Nombre maximal d’utilisateurs si la bande passante de 5 % est disponible|Nombre maximal d’utilisateurs si la bande passante de 10 % est disponible|  
| --- | --- | --- | --- |  
|28.8|10,000|25,000|40,000|  
|32|10,000|25,000|50,000|  
|56|10,000|50,000|100,000|  
|64|10,000|50,000|100,000|  
|128|25,000|100,000|100,000|  
|256|50,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  

Pour utiliser cette table :  

1. Dans le **liaison la plus lente se connectant à un contrôleur de domaine** colonne, recherchez la valeur qui correspond à la vitesse de la liaison les plus lentes sur lesquelles les services AD DS répliquera dans votre domaine.  

2. Dans la ligne qui correspond à votre vitesse de liaison lente, localisez la colonne qui représente la bande passante pourcentage que vous souhaitez allouer aux services AD DS. La valeur à cet emplacement est le nombre maximal d’utilisateurs que le domaine dans une forêt à domaine unique peut contenir.  

Si vous déterminez que le nombre total d’utilisateurs dans votre forêt est inférieur au nombre maximal d’utilisateurs contenant votre domaine, vous pouvez utiliser un seul domaine. Veillez à prendre en charge une croissance future planifiée lorsque vous apportez cette détermination. Si vous déterminez que le nombre total d’utilisateurs dans votre forêt est supérieur au nombre maximal d’utilisateurs que votre domaine peut contenir, vous avez besoin de réserver un pourcentage plus élevé de la bande passante pour la réplication, augmentez votre vitesse de liaison, ou diviser votre organisation vers domaines régionaux.  
  
## <a name="dividing-the-organization-into-regional-domains"></a>Division de l’organisation dans les domaines régionaux

Si vous ne pouvez pas répondre aux besoins de vos utilisateurs dans un domaine unique, vous devez sélectionner le modèle de domaine régional. Diviser votre organisation en régions de manière appropriée pour votre organisation et votre réseau existant. Par exemple, vous pouvez créer des régions en fonction des limites continentales.  
  
Notez que, car vous devrez créer un domaine Active Directory pour chaque région que vous établissez, nous vous recommandons de réduire le nombre de régions que vous définissez pour les services AD DS. Bien qu’il soit possible d’inclure un nombre illimité de domaines dans une forêt, pour la gestion de nous recommandons une forêt inclure pas plus de 10 domaines. Vous devez établir le juste équilibre entre l’optimisation de votre bande passante de réplication et en réduisant votre complexité administrative lors d’une division de votre organisation dans les domaines régionaux.  
  
Tout d’abord, déterminez le nombre maximal d’utilisateurs que votre forêt peut héberger. Cette base sur le lien les plus lentes dans la forêt entre les contrôleurs de domaine effectue la réplication et la quantité moyenne de la bande passante que vous souhaitez allouer pour la réplication Active Directory. Le tableau suivant répertorie le nombre maximal recommandé d’utilisateurs contenant une forêt. Elle repose sur la vitesse de la liaison la plus lente et la pourcentage de bande passante que vous souhaitez réserver pour la réplication. Ces informations s’appliquent aux forêts qui contiennent un maximum de 100 000 utilisateurs et qui ont une connectivité de. à 28,8 Kbits/s ou version ultérieure. Les valeurs dans le tableau suivant sont basées sur les hypothèses suivantes :  

- Tous les contrôleurs de domaine sont des serveurs de catalogue global.  
- Nouveaux utilisateurs joindre la forêt à une fréquence de 20 % par an.  
- Les utilisateurs laissent la forêt à une fréquence de 15 pourcent par an.  
- Les utilisateurs sont membres de cinq groupes globaux et de cinq groupes universels.  
- Le rapport entre les utilisateurs sur des ordinateurs est 1:1.  
- DNS d’Active Directory-intégré est utilisé.  
- Le nettoyage DNS est utilisé.  

> [!NOTE]  
> Les valeurs répertoriées dans le tableau suivant sont des approximations. La quantité de trafic de réplication dépend en grande partie le nombre de modifications apportées à l’annuaire dans un laps de temps donné. Vérifiez que votre réseau peut gérer votre trafic de réplication en testant la quantité estimée et le taux de modifications sur votre conception dans un laboratoire avant de déployer vos domaines.  
  
|Plus lente liaison connectant un contrôleur de domaine (Kbits/s)|Nombre maximal d’utilisateurs si la bande passante de 1 % est disponible|Nombre maximal d’utilisateurs si la bande passante de 5 % est disponible|Nombre maximal d’utilisateurs si la bande passante de 10 % est disponible|  
| --- | --- | --- | --- |  
|28.8|10,000|50,000|75,000|  
|32|10,000|50,000|75,000|  
|56|10,000|75,000|100,000|  
|64|25,000|75,000|100,000|  
|128|50,000|100,000|100,000|  
|256|75,000|100,000|100,000|  
|512|100,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  

Pour utiliser cette table :  

1. Dans le **liaison la plus lente se connectant à un contrôleur de domaine** colonne, recherchez la valeur qui correspond à la vitesse de la liaison les plus lentes sur lesquelles les services AD DS répliquera dans votre forêt.  

2. Dans la ligne qui correspond à votre vitesse de liaison lente, localisez la colonne qui représente la pourcentage de bande passante que vous souhaitez allouer aux services AD DS. La valeur à cet emplacement est le nombre maximal d’utilisateurs que votre forêt peut héberger.  

Si le nombre maximal d’utilisateurs que votre forêt peut héberger est supérieur au nombre d’utilisateurs dont vous avez besoin pour héberger, une seule forêt fonctionne pour votre conception. Si vous avez besoin pour héberger davantage d’utilisateurs que le nombre maximal que vous avez identifié, vous avez besoin pour augmenter la vitesse de liaison minimale, allouer un pourcentage supérieur de la bande passante pour les services AD DS, ou déployez forêts supplémentaires.  

Si vous déterminez qu’une seule forêt inclura le nombre d’utilisateurs dont vous avez besoin pour héberger, l’étape suivante consiste à déterminer le nombre maximal d’utilisateurs que chaque région peut prendre en charge en fonction de la liaison la plus lente située dans cette région. Diviser votre forêt dans des régions très parlant pour vous. Assurez-vous que vous basez votre décision sur quelque chose qui n’est pas susceptible de changer. Par exemple, utiliser continents au lieu de régions de ventes. Ces régions sera la base de votre structure de domaine lorsque vous avez identifié le nombre maximal d’utilisateurs.  

Déterminer le nombre d’utilisateurs qui doivent être hébergés dans chaque région et vérifiez qu’ils ne dépassent pas la valeur maximale autorisée en fonction de la vitesse de liaison lente et la bande passante allouée dans les services AD DS dans cette région. Le tableau suivant répertorie le nombre maximal recommandé d’utilisateurs contenant un domaine régional. Il est basé sur la vitesse de la liaison la plus lente et le pourcentage de bande passante que vous souhaitez réserver pour la réplication. Ces informations s’appliquent aux forêts qui contiennent un maximum de 100 000 utilisateurs et qui ont une connectivité de. à 28,8 Kbits/s ou version ultérieure. Les valeurs dans le tableau suivant sont basées sur les hypothèses suivantes :  

- Tous les contrôleurs de domaine sont des serveurs de catalogue global.  
- Nouveaux utilisateurs joindre la forêt à une fréquence de 20 % par an.  
- Les utilisateurs laissent la forêt à une fréquence de 15 pourcent par an.  
- Les utilisateurs sont membres de cinq groupes globaux et de cinq groupes universels.  
- Le rapport entre les utilisateurs sur des ordinateurs est 1:1.  
- DNS d’Active Directory-intégré est utilisé.  
- Le nettoyage DNS est utilisé.  
  
> [!NOTE]  
> Les valeurs répertoriées dans le tableau suivant sont des approximations. La quantité de trafic de réplication dépend en grande partie le nombre de modifications apportées à l’annuaire dans un laps de temps donné. Vérifiez que votre réseau peut gérer votre trafic de réplication en testant la quantité estimée et le taux de modifications sur votre conception dans un laboratoire avant de déployer vos domaines.  
  
|Plus lente liaison connectant un contrôleur de domaine (Kbits/s)|Nombre maximal d’utilisateurs si la bande passante de 1 % est disponible|Nombre maximal d’utilisateurs si la bande passante de 5 % est disponible|Nombre maximal d’utilisateurs si la bande passante de 10 % est disponible|  
| --- | --- | --- | --- |  
|28.8|10,000|18,000|40,000|  
|32|10,000|20,000|50,000|  
|56|10,000|40,000|100,000|  
|64|10,000|50,000|100,000|  
|128|15,000|100,000|100,000|  
|256|30,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  

Pour utiliser cette table :  

1. Dans le **liaison la plus lente se connectant à un contrôleur de domaine** colonne, recherchez la valeur qui correspond à la vitesse de la liaison les plus lentes sur lesquelles les services AD DS répliquera dans votre région.  

2. Dans la ligne qui correspond à votre vitesse de liaison lente, localisez la colonne qui représente la pourcentage de bande passante que vous souhaitez allouer aux services AD DS. Cette valeur représente le nombre maximal d’utilisateurs pouvant héberger de la région.  

Évaluez chaque région proposée et déterminer si le nombre maximal d’utilisateurs dans chaque région est inférieur au nombre maximal recommandé d’utilisateurs contenant un domaine. Si vous déterminez que la région peut héberger le nombre d’utilisateurs dont vous avez besoin, vous pouvez créer un domaine pour cette région. Si vous déterminez que vous ne pouvez pas héberger que beaucoup d’utilisateurs, envisagez de diviser votre conception en petites régions et recalculer le nombre maximal d’utilisateurs qui peuvent être hébergés dans chaque région. Les autres alternatives sont à allouer davantage de bande passante ou à augmenter votre vitesse de liaison.  

Bien que le nombre total d’utilisateurs que vous pouvez placer dans un domaine dans une forêt multidomaine est inférieur au nombre d’utilisateurs dans le domaine dans une forêt à domaine unique, le nombre total d’utilisateurs dans la forêt multidomaine peut être supérieur. Le plus petit nombre d’utilisateurs par domaine dans une forêt multidomaine prend en compte la surcharge de réplication supplémentaire créée en conservant le catalogue global dans cet environnement. Pour obtenir des recommandations qui s’appliquent aux forêts qui contiennent plus de 100 000 utilisateurs ou de connectivité d’inférieur à. à 28,8 Kbits/s, consultez un concepteur expérimenté d’Active Directory.  
  
## <a name="documenting-the-regions-identified"></a>Documenter les régions identifiées

Une fois que vous divisez votre organisation en domaines régionaux, le document représentées les régions que vous le souhaitez et le nombre d’utilisateurs qui existera dans chaque région. En outre, notez la vitesse des liens les plus lentes dans chaque région que vous allez utiliser pour la réplication Active Directory. Cette information sert à déterminer si des domaines ou forêts sont nécessaires.  

Pour une feuille de calcul pour vous aider à documenter les régions que vous avez identifié, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip à partir de [travail aides pour Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) et ouvrez » Identification des régions » (DSSLOGI_4.doc).  
