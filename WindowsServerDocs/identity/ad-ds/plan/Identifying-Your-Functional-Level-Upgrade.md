---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: Identification de la mise à niveau de votre niveau fonctionnel
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e43bec8a8d61cd0f6fd82982d5e3a0f01984fc65
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408781"
---
# <a name="identifying-your-functional-level-upgrade"></a>Identification de la mise à niveau de votre niveau fonctionnel

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avant de pouvoir augmenter les niveaux fonctionnels des domaines et des forêts, vous devez évaluer votre environnement actuel et identifier l’exigence de niveau fonctionnel qui répond le mieux aux besoins de votre organisation. Évaluez votre environnement actuel en identifiant les domaines de votre forêt, les contrôleurs de domaine qui se trouvent dans chaque domaine, le système d’exploitation et les service packs exécutés par chaque contrôleur de domaine, ainsi que la date à laquelle vous envisagez de mettre à niveau le domaine. secondaires. Si vous envisagez de mettre hors service un contrôleur de domaine, assurez-vous que vous comprenez l’impact total de cette opération sur votre environnement.  
  
Les circonstances suivantes peuvent vous empêcher de mettre à niveau une version antérieure du système d’exploitation Windows Server vers le niveau fonctionnel Windows Server 2008 ou Windows Server 2008 R2 :  
  
-   Matériel insuffisant  
  
-   Un contrôleur de domaine exécutant un logiciel antivirus qui n’est pas compatible avec Windows Server 2008 ou Windows Server 2008 R2   
  
-   Utilisation d’un programme spécifique à la version qui ne s’exécute pas sur Windows Server 2008 ou Windows Server 2008 R2   
  
-   La nécessité de mettre à niveau un programme avec la dernière Service Pack  
  
La documentation de ces informations peut vous aider à identifier les étapes à suivre pour vous assurer que vous disposez d’un environnement Windows Server 2008 ou Windows Server 2008 R2 entièrement fonctionnel.  
  
Une fois que vous avez évalué votre environnement actuel, vous devez identifier la mise à niveau fonctionnelle qui s’applique à votre organisation. Les options disponibles sont les suivantes :  
  
-   Environnement en mode natif Windows 2000 vers Windows Server 2008 ou Windows Server 2008 R2   
  
-   Forêt Windows Server 2003 vers Windows Server 2008 ou Windows Server 2008 R2   
  
-   Nouvelle forêt Windows Server 2008  
  
-   Nouvelle forêt Windows Server 2008 R2  
  
## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>Mise à niveau des niveaux fonctionnels dans une forêt Windows 2000 Active Directory Native  
Dans un environnement natif Windows 2000 qui se compose uniquement de contrôleurs de domaine basés sur Windows 2000, les niveaux fonctionnels sont définis par défaut aux niveaux suivants, et ils restent à ces niveaux jusqu’à ce que vous les montiez manuellement :  
  
-   Niveau fonctionnel du domaine natif Windows 2000  
  
-   Niveau fonctionnel de la forêt Windows 2000  
  
Pour utiliser toutes les fonctionnalités au niveau de la forêt et du domaine dans Windows Server 2008 ou Windows Server 2008 R2, vous devez mettre à niveau cet environnement Windows 2000 vers Windows Server 2008 ou Windows Server 2008 R2. Vous pouvez effectuer cette mise à niveau de l’une des manières suivantes :  
  
-   Introduisez dans la forêt les contrôleurs de domaine Windows Server 2008 ou Windows Server 2008 R2 qui viennent d’être installés, puis mettez hors service tous les contrôleurs de domaine exécutant Windows 2000.  
  
-   Effectuez une mise à niveau sur place de tous les contrôleurs de domaine existants exécutant Windows 2000 dans la forêt vers les contrôleurs de domaine exécutant Windows Server 2003. Ensuite, effectuez une mise à niveau sur place de ces contrôleurs de domaine vers Windows Server 2008 ou Windows Server 2008 R2. Pour plus d’informations, consultez [mise à niveau des domaines de Active Directory vers Windows Server 2008 AD DS domaines \[LH @ no__t-2](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
    > [!IMPORTANT]  
    >  Windows Server 2008 R2 est un système d’exploitation basé sur x64. Si votre serveur exécute une version x64 de Windows Server 2003, vous pouvez effectuer une mise à niveau sur place du système d’exploitation de cet ordinateur vers Windows Server 2008 R2. Si votre serveur exécute une version x86 de Windows Server 2003, vous ne pouvez pas mettre à niveau cet ordinateur vers Windows Server 2008 R2.  
  
Pour utiliser les fonctionnalités au niveau du domaine Windows Server 2008 ou Windows Server 2008 R2 sans mettre à niveau la totalité de votre forêt Windows 2000 vers Windows Server 2008 ou Windows Server 2008 R2, augmentez uniquement le niveau fonctionnel du domaine à Windows Server 2008 ou Windows Server 2008 2.  
  
> [!NOTE]  
> Avant d’augmenter le niveau fonctionnel du domaine, vous devez mettre à niveau tous les contrôleurs de domaine basés sur Windows 2000 de ce domaine vers Windows Server 2008 ou Windows Server 2008 R2.  
  
Après avoir remplacé tous les contrôleurs de domaine basés sur Windows 2000 dans la forêt par des contrôleurs de domaine qui exécutent Windows Server 2008 ou Windows Server 2008 R2, vous pouvez augmenter le niveau fonctionnel de la forêt à Windows Server 2008 ou à Windows Server 2008 R2. Cela déclenche automatiquement le niveau fonctionnel de tous les domaines de la forêt qui sont définis sur Windows 2000 natif ou version ultérieure sur Windows Server 2008 ou Windows Server 2008 R2.  
  
Pour plus d’informations sur le déclenchement des niveaux fonctionnels de forêt et de domaine, et sur les procédures d’exécution de ces tâches, voir [déploiement d’un domaine racine de forêt Windows Server 2008 \[LH @ no__t-2](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>Mise à niveau des niveaux fonctionnels dans une forêt Windows Server 2003 Active Directory  
Dans un environnement Windows Server 2003 qui se compose uniquement de contrôleurs de domaine basés sur Windows Server 2003, les niveaux fonctionnels sont définis par défaut aux niveaux suivants, et ils restent à ces niveaux jusqu’à ce que vous les montiez manuellement :  
  
-   Niveau fonctionnel du domaine natif Windows 2000  
  
-   Niveau fonctionnel de la forêt Windows 2000  
  
Pour utiliser toutes les fonctionnalités au niveau de la forêt et du domaine dans Windows Server 2008 ou Windows Server 2008 R2, vous devez mettre à niveau cet environnement Windows Server 2003 vers Windows Server 2008 ou Windows Server 2008 R2. Vous pouvez effectuer cette mise à niveau de l’une des manières suivantes :  
  
-   Introduisez un contrôleur de domaine Windows Server 2008 ou Windows Server 2008 R2 récemment installé dans la forêt, puis retirez tous les contrôleurs de domaine exécutant Windows Server 2003 ou mettez-les à niveau vers Windows Server 2008 ou Windows Server 2008 R2.  
  
-   Effectuez une mise à niveau sur place de tous les contrôleurs de domaine existants exécutant Windows Server 2003 vers des contrôleurs de domaine exécutant Windows Server 2008 ou Windows Server 2008 R2. Pour plus d’informations, consultez [mise à niveau des domaines de Active Directory vers Windows Server 2008 AD DS domaines \[LH @ no__t-2](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
> [!IMPORTANT]  
>  Windows Server 2008 R2 est un système d’exploitation basé sur x64. Si votre serveur exécute une version x64 de Windows Server 2003, vous pouvez effectuer une mise à niveau sur place du système d’exploitation de cet ordinateur vers Windows Server 2008 R2. Si votre serveur exécute une version x86 de Windows Server 2003, vous ne pouvez pas mettre à niveau cet ordinateur pour exécuter Windows Server 2008 R2.  
  
Pour utiliser toutes les fonctionnalités au niveau du domaine Windows Server 2008 ou Windows Server 2008 R2 sans mettre à niveau la totalité de votre forêt Windows Server 2003 vers Windows Server 2008 ou Windows Server 2008 R2, n’augmentez que le niveau fonctionnel du domaine à Windows Server 2008 ou Windows S erveur 2008 R2.  
  
> [!NOTE]  
> Avant d’augmenter le niveau fonctionnel du domaine, vous devez mettre à niveau tous les contrôleurs de domaine Windows Server 2003 de ce domaine vers Windows Server 2008 ou Windows Server 2008 R2.  
  
Après avoir mis à niveau tous les contrôleurs de domaine Windows Server 2003 de la forêt vers Windows Server 2008 ou Windows Server 2008 R2, vous pouvez augmenter le niveau fonctionnel de la forêt à Windows Server 2008 ou Windows Server 2008 R2. Cela déclenche automatiquement le niveau fonctionnel de tous les domaines de la forêt qui sont définis sur Windows Server 2003 sur Windows Server 2008 ou Windows Server 2008 R2.  
  
Pour plus d’informations sur le déclenchement des niveaux fonctionnels de forêt et de domaine, et sur les procédures d’exécution de ces tâches, voir [déploiement d’un domaine racine de forêt Windows Server 2008 \[LH @ no__t-2](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>Mise à niveau des niveaux fonctionnels dans une nouvelle forêt Windows Server 2008  
Lorsque vous installez le premier contrôleur de domaine dans une nouvelle forêt Windows Server 2008, les niveaux fonctionnels sont définis par défaut aux niveaux suivants, et ils restent à ces niveaux jusqu’à ce que vous les montiez manuellement :  
  
-   Niveau fonctionnel du domaine natif Windows 2000  
  
-   Niveau fonctionnel de la forêt Windows 2000  
  
Les niveaux fonctionnels sont définis à ces niveaux par défaut pour vous permettre d’ajouter des contrôleurs de domaine Windows 2000 ou Windows Server 2003 à votre nouvelle forêt Windows Server 2008. Une fois que vous avez créé un domaine racine de forêt, le niveau fonctionnel de domaine pour chaque domaine que vous ajoutez à la forêt Windows Server 2008 est défini sur Windows 2000 natif. Toutefois, si vous souhaitez que tous les contrôleurs de domaine dans votre nouvel environnement Windows Server 2008 exécutent Windows Server 2008, définissez le niveau fonctionnel de la forêt, puis le niveau fonctionnel du domaine, sur Windows Server 2008 quand vous installez le premier contrôleur de domaine dans vos t. Cela permet de gagner du temps et d’activer toutes les fonctionnalités au niveau de la forêt et du domaine dans Windows Server 2008.  
  
> [!IMPORTANT]  
> Si la forêt fonctionne au niveau fonctionnel de Windows Server 2008 et que vous tentez d’installer Active Directory sur un serveur membre basé sur Windows Server 2003 ou un serveur membre Windows 2000, l’installation échoue.  
  
Pour plus d’informations sur le déclenchement des niveaux fonctionnels de forêt et de domaine, et sur les procédures d’exécution de ces tâches, voir [déploiement d’un domaine racine de forêt Windows Server 2008 \[LH @ no__t-2](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>Mise à niveau des niveaux fonctionnels dans une nouvelle forêt Windows Server 2008 R2  
Lorsque vous installez le premier contrôleur de domaine dans une nouvelle forêt Windows Server 2008 R2, les niveaux fonctionnels sont définis par défaut aux niveaux suivants, et ils restent à ces niveaux jusqu’à ce que vous les montiez manuellement :  
  
-   Niveau fonctionnel du domaine Windows Server 2003  
  
-   Niveau fonctionnel de la forêt Windows Server 2003  
  
Les niveaux fonctionnels sont définis à ces niveaux par défaut pour vous permettre d’ajouter des contrôleurs de domaine basés sur Windows Server 2003 à votre nouvelle forêt Windows Server 2008 R2. Après avoir créé un domaine racine de forêt, le niveau fonctionnel de domaine pour chaque domaine que vous ajoutez à la forêt Windows Server 2008 R2 est défini sur Windows Server 2003. Toutefois, si vous souhaitez que tous les contrôleurs de domaine dans votre nouvel environnement Windows Server 2008 R2 exécutent Windows Server 2008 R2, définissez le niveau fonctionnel de la forêt, puis le niveau fonctionnel du domaine, sur Windows Server 2008 R2 quand vous installez le premier contrôleur de domaine dans Yo. votre forêt. Cela permet de gagner du temps et d’activer toutes les fonctionnalités au niveau de la forêt et du domaine dans Windows Server 2008 R2.  
  
> [!IMPORTANT]  
> Si la forêt fonctionne au niveau fonctionnel de Windows Server 2008 R2 et que vous tentez d’installer Active Directory sur un serveur membre Windows Server 2008 ou Windows Server 2003, ou sur un serveur membre basé sur Windows 2000, l’installation échoue.  
  
Pour plus d’informations sur le déclenchement des niveaux fonctionnels de forêt et de domaine, et sur les procédures d’exécution de ces tâches, voir [déploiement d’un domaine racine de forêt Windows Server 2008 \[LH @ no__t-2](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
> [!NOTE]  
> Même si ADMT v 3.1 doit être installé sur Windows Server 2008, vous pouvez utiliser ADMT v 3.1 pour migrer des objets vers un domaine hébergé par un ou plusieurs contrôleurs de domaine Windows Server 2008 R2. Pour plus d’informations, consultez l' [article 976659](https://go.microsoft.com/fwlink/?LinkId=180398) de la base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=180398).  
  


