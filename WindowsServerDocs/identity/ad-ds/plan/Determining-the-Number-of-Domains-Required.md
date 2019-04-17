---
ms.assetid: 87bca912-b912-4bbe-9533-2c34a7abc52d
title: "Détermination du nombre de domaines nécessaires"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b65c792929a29c959244d1da83b4882f15fa2935
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="determining-the-number-of-domains-required"></a>Détermination du nombre de domaines nécessaires

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Chaque forêt démarre avec un seul domaine. Le nombre maximal d’utilisateurs dans une forêt à domaine unique pouvant contenir est basé sur le lien plus lent qui doit s’adapter à la réplication entre les contrôleurs de domaine et de la bande passante disponible que vous souhaitez allouer aux Services de domaine ActiveDirectory (ADDS). Le tableau suivant répertorie le nombre maximal recommandé d’utilisateurs contenant un domaine basé sur une forêt à domaine unique, la vitesse de la liaison lente et le pourcentage de bande passante que vous souhaitez réserver pour la réplication. Ces informations s’appliquent à des forêts contenant un maximum de 100000utilisateurs et qui ont une connectivité 28,8kilobits par seconde (Kbits/s) ou une version ultérieure. Pour obtenir des recommandations qui s’appliquent aux forêts qui contiennent plus de 100000utilisateurs ou de connectivité moins 28,8Kbits/s, consultez un concepteur expérimenté d’ActiveDirectory. Les valeurs dans le tableau suivant sont basées sur le trafic de réplication généré dans un environnement qui présente les caractéristiques suivantes:  
  
-   Nouveaux utilisateurs joindre la forêt à une vitesse de 20% par an.  
  
-   Utilisateurs les laissent la forêt à une fréquence de 15% par an.  
  
-   Chaque utilisateur est membre de cinq groupes globaux et de cinq groupes universels.  
  
-   Le rapport aux utilisateurs d’ordinateurs est 1:1.  
  
-   ActiveDirectory intégré système DNS (Domain Name) est utilisé.  
  
-   Le nettoyage DNS est utilisé.  
  
> [!NOTE]  
> Les chiffres figurant dans le tableau suivant sont approximations. La quantité de trafic de réplication dépend en grande partie le nombre de modifications apportées à l’annuaire dans un laps de temps donné. Vérifiez que votre réseau peut prendre en charge votre trafic de réplication en testant la quantité estimée et le taux de modifications sur votre conception dans un laboratoire avant de déployer vos domaines.  
  
|Plus lente liaison connectant un contrôleur de domaine (Kbits/s)|Nombre maximal d’utilisateurs si 1-% de bande passante est disponible|Nombre maximal d’utilisateurs si 5-% de bande passante est disponible|Nombre maximal d’utilisateurs si-10% de bande passante est disponible|  
|--------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------|  
|28.8|10,000|25,000|40,000|  
|32|10,000|25,000|50,000|  
|56|10,000|50,000|100,000|  
|64|10,000|50,000|100,000|  
|128|25,000|100,000|100,000|  
|256|50,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  
  
Pour utiliser le tableau suivant:  
  
1.  Dans le **liaison plus lente se connectant à un contrôleur de domaine** colonne, recherchez la valeur qui correspond à la vitesse de la liaison lente dans lequel les services ADDS sont répliquées dans votre domaine.  
  
2.  Dans la ligne correspondant à votre vitesse de liaison lente, recherchez la colonne qui représente la bande passante pourcentage que vous souhaitez allouer aux services ADDS. La valeur à cet emplacement est le nombre maximal d’utilisateurs du domaine dans une forêt à domaine unique peut contenir.  
  
Si vous déterminez que le nombre total d’utilisateurs dans votre forêt est inférieure au nombre maximal d’utilisateurs contenant votre domaine, vous pouvez utiliser un seul domaine. Veillez à prendre en charge de la croissance future planifiée lorsque vous apportez cette détermination. Si vous déterminez que le nombre total d’utilisateurs dans votre forêt est supérieur au nombre maximal d’utilisateurs contenant votre domaine, vous devez réserver un pourcentage plus élevé de la bande passante pour la réplication, augmenter votre vitesse de liaison ou diviser votre organisation dans les domaines régionaux.  
  
## <a name="dividing-the-organization-into-regional-domains"></a>Répartition de l’organisation dans les domaines régionaux  
Si vous ne pouvez pas prendre en compte tous les utilisateurs dans un domaine unique, vous devez sélectionner le modèle de domaine régional. Divisez votre organisation dans les zones d’une manière qui a du sens pour votre organisation et votre réseau existant. Par exemple, vous pouvez créer des zones, en fonction des limites continentales.  
  
Notez que dans la mesure où vous avez besoin créer un domaine ActiveDirectory pour chaque région que vous avez définis, nous recommandons que vous réduisez le nombre de zones que vous définissez pour les services ADDS. Bien qu’il soit possible d’inclure un nombre illimité de domaines dans une forêt, pour la gestion de nous recommandons d’une forêt inclure les domaines pas plus de 10. Vous devez établir le juste équilibre entre l’optimisation de la bande passante de réplication et en réduisant les votre la complexité d’administration durant la répartition de votre organisation dans les domaines régionaux.  
  
Tout d’abord, déterminez le nombre maximal d’utilisateurs capable d’héberger votre forêt. Cette base sur le lien dans la forêt plus lentes entre les contrôleurs de domaine effectue la réplication et la quantité moyenne de la bande passante que vous souhaitez allouer pour la réplication ActiveDirectory. Le tableau suivant répertorie le nombre maximal recommandé d’utilisateurs contenant une forêt. Cela est lié à la vitesse de la liaison lente et la pourcentage de bande passante que vous souhaitez réserver pour la réplication. Ces informations s’appliquent à des forêts contenant un maximum de 100000utilisateurs et qui ont une connectivité 28,8Kbits/s ou une version ultérieure. Les valeurs dans le tableau suivant sont basées sur les hypothèses suivantes:  
  
-   Tous les contrôleurs de domaine sont des serveurs de catalogue global.  
  
-   Nouveaux utilisateurs joindre la forêt à une vitesse de 20% par an.  
  
-   Utilisateurs les laissent la forêt à une fréquence de 15% par an.  
  
-   Les utilisateurs sont membres de cinq groupes globaux et de cinq groupes universels.  
  
-   Le rapport aux utilisateurs d’ordinateurs est 1:1.  
  
-   DNS intégré à ActiveDirectory est utilisé.  
  
-   Le nettoyage DNS est utilisé.  
  
> [!NOTE]  
> Les chiffres figurant dans le tableau suivant sont approximations. La quantité de trafic de réplication dépend en grande partie le nombre de modifications apportées à l’annuaire dans un laps de temps donné. Vérifiez que votre réseau peut prendre en charge votre trafic de réplication en testant la quantité estimée et le taux de modifications sur votre conception dans un laboratoire avant de déployer vos domaines.  
  
|Plus lente liaison connectant un contrôleur de domaine (Kbits/s)|Nombre maximal d’utilisateurs si 1-% de bande passante est disponible|Nombre maximal d’utilisateurs si 5-% de bande passante est disponible|Nombre maximal d’utilisateurs si-10% de bande passante est disponible|  
|--------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------|  
|28.8|10,000|50,000|75,000|  
|32|10,000|50,000|75,000|  
|56|10,000|75,000|100,000|  
|64|25,000|75,000|100,000|  
|128|50,000|100,000|100,000|  
|256|75,000|100,000|100,000|  
|512|100,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  
  
Pour utiliser le tableau suivant:  
  
1.  Dans le **liaison plus lente se connectant à un contrôleur de domaine** colonne, recherchez la valeur qui correspond à la vitesse de la liaison lente sur lesquels les services ADDS sont répliquées dans votre forêt.  
  
2.  Dans la ligne correspondant à votre vitesse de liaison lente, recherchez la colonne qui représente la pourcentage de bande passante que vous souhaitez allouer aux services ADDS. La valeur à cet emplacement est le nombre maximal d’utilisateurs capable d’héberger votre forêt.  
  
Si le nombre maximal d’utilisateurs que votre forêt peut héberger est supérieur au nombre d’utilisateurs dont vous avez besoin pour héberger, une seule forêt fonctionnera pour votre conception. Si vous avez besoin héberger plus d’utilisateurs que le nombre maximal que vous avez identifié, vous avez besoin pour augmenter la vitesse de liaison minimale, allouer un pourcentage de bande passante supérieur pour les services ADDS, ou déployez forêts supplémentaires.  
  
Si vous déterminez qu’une seule forêt prend en charge le nombre d’utilisateurs dont vous avez besoin pour héberger, l’étape suivante consiste à déterminer le nombre maximal d’utilisateurs chaque région pouvant prendre en charge en fonction de la liaison plus lente située dans cette région. Divisez votre forêt régions avoir un sens pour vous. Assurez-vous que vous baser votre décision sur un élément qui n’est pas susceptible de changer. Par exemple, utilisez continents au lieu de ventes régions. Ces régions seront la base de votre structure de domaine lorsque vous avez identifié le nombre maximal d’utilisateurs.  
  
Déterminer le nombre d’utilisateurs qui doivent être hébergée dans chaque région et vérifiez qu’ils ne dépassent pas la valeur maximale autorisée en fonction de la vitesse de liaison lente et la bande passante allouée aux services ADDS dans cette région. Le tableau suivant répertorie le nombre maximal recommandé d’un domaine régional pouvant contenir des utilisateurs. Il est basé sur la vitesse de la liaison plus lente et le pourcentage de bande passante que vous souhaitez réserver pour la réplication. Ces informations s’appliquent à des forêts contenant un maximum de 100000utilisateurs et qui ont une connectivité 28,8Kbits/s ou une version ultérieure. Les valeurs dans le tableau suivant sont basées sur les hypothèses suivantes:  
  
-   Tous les contrôleurs de domaine sont des serveurs de catalogue global.  
  
-   Nouveaux utilisateurs joindre la forêt à une vitesse de 20% par an.  
  
-   Utilisateurs les laissent la forêt à une fréquence de 15% par an.  
  
-   Les utilisateurs sont membres de cinq groupes globaux et de cinq groupes universels.  
  
-   Le rapport aux utilisateurs d’ordinateurs est 1:1.  
  
-   DNS intégré à ActiveDirectory est utilisé.  
  
-   Le nettoyage DNS est utilisé.  
  
> [!NOTE]  
> Les chiffres figurant dans le tableau suivant sont approximations. La quantité de trafic de réplication dépend en grande partie le nombre de modifications apportées à l’annuaire dans un laps de temps donné. Vérifiez que votre réseau peut prendre en charge votre trafic de réplication en testant la quantité estimée et le taux de modifications sur votre conception dans un laboratoire avant de déployer vos domaines.  
  
|Plus lente liaison connectant un contrôleur de domaine (Kbits/s)|Nombre maximal d’utilisateurs si 1-% de bande passante est disponible|Nombre maximal d’utilisateurs si 5-% de bande passante est disponible|Nombre maximal d’utilisateurs si-10% de bande passante est disponible|  
|--------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------|  
|28.8|10,000|18,000|40,000|  
|32|10,000|20,000|50,000|  
|56|10,000|40,000|100,000|  
|64|10,000|50,000|100,000|  
|128|15,000|100,000|100,000|  
|256|30,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  
  
Pour utiliser le tableau suivant:  
  
1.  Dans le **liaison plus lente se connectant à un contrôleur de domaine** colonne, recherchez la valeur qui correspond à la vitesse de la liaison lente sur lesquels les services ADDS sont répliquées dans votre région.  
  
2.  Dans la ligne correspondant à votre vitesse de liaison lente, recherchez la colonne qui représente la pourcentage de bande passante que vous souhaitez allouer aux services ADDS. Cette valeur représente le nombre maximal d’utilisateurs capable d’héberger la région.  
  
Évaluez chaque région proposée et déterminer si le nombre maximal d’utilisateurs dans chaque région est inférieur au nombre maximal recommandé d’utilisateurs un domaine peut contenir. Si vous déterminez que la région peut héberger le nombre d’utilisateurs dont vous avez besoin, vous pouvez créer un domaine pour cette région. Si vous déterminez que vous ne pouvez pas héberger que beaucoup d’utilisateurs, envisagez de diviser votre conception en régions de petite taille et recalculer le nombre maximal d’utilisateurs qui peuvent être hébergés dans chaque région. Les autres alternatives sont pour allouer davantage de bande passante ou augmenter la vitesse de votre lien.  
  
Bien que le nombre total d’utilisateurs que vous pouvez placer dans un domaine dans une forêt à plusieurs domaines est inférieur au nombre d’utilisateurs dans le domaine dans une forêt à domaine unique, le nombre total d’utilisateurs dans la forêt à plusieurs domaines peut être supérieur. Le plus petit nombre d’utilisateurs par domaine dans une forêt à plusieurs domaines prend en compte la charge de la réplication créée en conservant le catalogue global dans cet environnement. Pour obtenir des recommandations qui s’appliquent aux forêts qui contiennent plus de 100000utilisateurs ou de connectivité moins 28,8Kbits/s, consultez un concepteur expérimenté d’ActiveDirectory.  
  
## <a name="documenting-the-regions-identified"></a>Documenter les régions identifiées  
Une fois que votre organisation vous divisez de domaines régionaux, les régions que vous souhaitez représentées de document et le nombre d’utilisateurs qui existera dans chaque région. En outre, notez la vitesse des liens plus lentes dans chaque région que vous allez utiliser pour la réplication ActiveDirectory. Ces informations sont utilisées pour déterminer si d’autres domaines ou forêts sont nécessaires.  
  
Pour une feuille de calcul pour vous aider aux régions que vous avez identifié la documentation, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  


