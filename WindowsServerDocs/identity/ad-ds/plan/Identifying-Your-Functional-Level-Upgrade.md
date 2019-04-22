---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: Identification de la mise à niveau de votre niveau fonctionnel
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8aee6a5560edfae656582b3c2812ca69c6b90f57
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812040"
---
# <a name="identifying-your-functional-level-upgrade"></a>Identification de la mise à niveau de votre niveau fonctionnel

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avant d’augmenter les niveaux fonctionnels de domaine et de forêt, vous devez évaluer votre environnement actuel et identifier l’exigence de niveau fonctionnel que répond le mieux aux besoins de votre organisation. Évaluez votre environnement actuel en identifiant les domaines dans votre forêt, les contrôleurs de domaine qui sont trouvent dans chaque domaine, le système d’exploitation, service packs que chaque contrôleur de domaine est en cours d’exécution et la date à laquelle vous souhaitez mettre à niveau le domaine contrôleurs. Si vous envisagez de mettre hors service un contrôleur de domaine, assurez-vous que vous comprenez l’impact que cela aura sur votre environnement.  
  
Les circonstances suivantes peuvent vous empêcher de mise à niveau d’une version antérieure du système d’exploitation Windows Server vers le niveau fonctionnel de Windows Server 2008 ou Windows Server 2008 R2 :  
  
-   Configuration matérielle insuffisante  
  
-   Un contrôleur de domaine exécutant un programme antivirus n’est pas compatible avec Windows Server 2008 ou Windows Server 2008 R2   
  
-   Utilisation d’un programme spécifique à la version qui n’exécute pas sur Windows Server 2008 ou Windows Server 2008 R2   
  
-   La nécessité de mettre à niveau d’un programme avec le dernier service pack  
  
Documenter ces informations permettre vous aider à identifier les étapes à suivre pour vous assurer que vous avez un environnement entièrement fonctionnel de Windows Server 2008 ou Windows Server 2008 R2.  
  
Une fois que vous évaluez votre environnement actuel, vous devez identifier la mise à niveau au niveau fonctionnel qui s’applique à votre organisation. Ces options sont disponibles :  
  
-   Environnement en mode natif de Windows 2000 à Windows Server 2008 ou Windows Server 2008 R2   
  
-   Forêt Windows Server 2003 à Windows Server 2008 ou Windows Server 2008 R2   
  
-   Nouvelle forêt Windows Server 2008  
  
-   Nouvelle forêt Windows Server 2008 R2  
  
## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>La mise à niveau des niveaux fonctionnels dans une forêt Active Directory de Windows 2000 native  
Dans un environnement natif de Windows 2000 qui se compose uniquement de contrôleurs de domaine Windows 2000, les niveaux fonctionnels sont définis par défaut pour les niveaux suivants, et elles restent à ces niveaux, jusqu'à ce que vous les générez manuellement :  
  
-   Niveau fonctionnel de domaine en mode natif de Windows 2000  
  
-   Niveau fonctionnel de forêt Windows 2000  
  
Pour utiliser toutes les fonctionnalités de niveau de la forêt et au niveau du domaine dans Windows Server 2008 ou Windows Server 2008 R2, vous devez mettre à niveau de cet environnement Windows 2000 vers Windows Server 2008 ou Windows Server 2008 R2. Vous pouvez effectuer cette mise à niveau dans une des manières suivantes :  
  
-   Introduire nouvellement installé Windows Server 2008-base ou Windows Server 2008 R2-basé sur des contrôleurs de domaine à la forêt, puis mettre hors service tous les contrôleurs de domaine exécutant Windows 2000.  
  
-   Effectuer une mise à niveau sur place de tous les contrôleurs de domaine existant exécutant Windows 2000 dans la forêt pour les contrôleurs de domaine exécutant Windows Server 2003. Ensuite, effectuez une mise à niveau sur place de ces contrôleurs de domaine vers Windows Server 2008 ou Windows Server 2008 R2. Pour plus d’informations, consultez [la mise à niveau de domaines Active Directory pour Windows Server 2008 domaines AD DS \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
    > [!IMPORTANT]  
    >  Windows Server 2008 R2 est un système d’exploitation x64 64. Si votre serveur exécute une version x64 64 de Windows Server 2003, vous pouvez effectuer avec succès une mise à niveau sur place de ce système d’exploitation vers Windows Server 2008 R2. Si votre serveur exécute une version x86 de Windows Server 2003, vous ne pouvez pas mettre à niveau cet ordinateur vers Windows Server 2008 R2.  
  
Pour utiliser les fonctionnalités au niveau du domaine Windows Server 2008 ou Windows Server 2008 R2 sans mise à niveau de votre ensemble de la forêt Windows 2000 vers Windows Server 2008 ou Windows Server 2008 R2, déclencher uniquement le niveau fonctionnel du domaine à Windows Server 2008 ou Windows Server 2008 R2.  
  
> [!NOTE]  
> Avant d’augmenter le niveau fonctionnel du domaine, vous devez mettre à niveau tous les contrôleurs de domaine Windows 2000 dans ce domaine pour Windows Server 2008 ou Windows Server 2008 R2.  
  
Une fois que vous remplacez tous les contrôleurs de domaine Windows 2000 dans la forêt avec des contrôleurs de domaine qui exécutent Windows Server 2008 ou Windows Server 2008 R2, vous pouvez déclencher le niveau fonctionnel de forêt à Windows Server 2008 ou Windows Server 2008 R2. Alors automatiquement déclenche le niveau fonctionnel de tous les domaines dans la forêt qui sont définies sur Windows 2000 natif ou ultérieur pour Windows Server 2008 ou Windows Server 2008 R2.  
  
Pour plus d’informations sur l’augmentation des niveaux fonctionnels de forêt et du domaine et pour connaître les procédures effectuer ces tâches, consultez [déploiement d’un domaine racine de forêt Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>La mise à niveau des niveaux fonctionnels dans une forêt Windows Server 2003 Active Directory  
Dans un environnement Windows Server 2003 qui se compose de contrôleurs de domaine Windows Server 2003 uniquement, les niveaux fonctionnels sont définis par défaut pour les niveaux suivants, et elles restent à ces niveaux, jusqu'à ce que vous les générez manuellement :  
  
-   Niveau fonctionnel de domaine en mode natif de Windows 2000  
  
-   Niveau fonctionnel de forêt Windows 2000  
  
Pour utiliser toutes les fonctionnalités de niveau de la forêt et au niveau du domaine dans Windows Server 2008 ou Windows Server 2008 R2, vous devez mettre à niveau de cet environnement de Windows Server 2003 vers Windows Server 2008 ou Windows Server 2008 R2. Vous pouvez effectuer cette mise à niveau dans une des manières suivantes :  
  
-   Introduire un nouvellement installé Windows Server 2008-base ou Windows Server 2008 R2-en fonction de contrôleur de domaine dans la forêt, puis mettre hors service tous les contrôleurs de domaine exécutant Windows Server 2003 ou les mettre à niveau vers Windows Server 2008 ou Windows Server 2008 R2.  
  
-   Effectuer une mise à niveau sur place de tous les contrôleurs de domaine existant exécutant Windows Server 2003 pour les contrôleurs de domaine exécutant Windows Server 2008 ou Windows Server 2008 R2. Pour plus d’informations, consultez [la mise à niveau de domaines Active Directory pour Windows Server 2008 domaines AD DS \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
> [!IMPORTANT]  
>  Windows Server 2008 R2 est un système d’exploitation x64 64. Si votre serveur exécute une version x64 64 de Windows Server 2003, vous pouvez effectuer avec succès une mise à niveau sur place de ce système d’exploitation vers Windows Server 2008 R2. Si votre serveur exécute une version x86 de Windows Server 2003, vous ne pouvez pas mettre à niveau cet ordinateur pour exécuter Windows Server 2008 R2.  
  
Pour utiliser les fonctionnalités au niveau du domaine tous les Windows Server 2008 ou Windows Server 2008 R2 sans mise à niveau de votre ensemble de la forêt Windows Server 2003 vers Windows Server 2008 ou Windows Server 2008 R2, déclencher uniquement le niveau fonctionnel du domaine à Windows Server 2008 ou Windows S erver 2008 R2.  
  
> [!NOTE]  
> Avant d’augmenter le niveau fonctionnel du domaine, vous devez mettre à niveau tous les contrôleurs de domaine Windows Server 2003 dans ce domaine pour Windows Server 2008 ou Windows Server 2008 R2.  
  
Une fois que vous mettez à niveau tous les contrôleurs de domaine Windows Server 2003 dans la forêt à Windows Server 2008 ou Windows Server 2008 R2, vous pouvez augmenter le niveau fonctionnel de forêt à Windows Server 2008 ou Windows Server 2008 R2. Alors automatiquement déclenche le niveau fonctionnel de tous les domaines dans la forêt qui sont définies sur Windows Server 2003 vers Windows Server 2008 ou Windows Server 2008 R2.  
  
Pour plus d’informations sur l’augmentation des niveaux fonctionnels de forêt et du domaine et pour connaître les procédures effectuer ces tâches, consultez [déploiement d’un domaine racine de forêt Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>La mise à niveau des niveaux fonctionnels dans une nouvelle forêt Windows Server 2008  
Lorsque vous installez le premier contrôleur de domaine dans une nouvelle forêt Windows Server 2008, les niveaux fonctionnels sont définis par défaut pour les niveaux suivants et elles restent à ces niveaux, jusqu'à ce que vous les générez manuellement :  
  
-   Niveau fonctionnel de domaine en mode natif de Windows 2000  
  
-   Niveau fonctionnel de forêt Windows 2000  
  
Niveaux fonctionnels sont définis à ces niveaux par défaut pour vous donner la possibilité d’ajouter des contrôleurs de domaine Windows Server 2003 ou de Windows 2000 vers votre nouvelle forêt Windows Server 2008. Après avoir créé un domaine racine de forêt, le niveau fonctionnel du domaine pour chaque domaine que vous ajoutez à la forêt de Windows Server 2008 a Windows 2000 natif. Toutefois, si vous souhaitez que tous les contrôleurs de domaine dans votre nouvel environnement Windows Server 2008 pour exécuter Windows Server 2008, définir le niveau fonctionnel de forêt, puis le niveau fonctionnel du domaine vers Windows Server 2008 lorsque vous installez le premier contrôleur de domaine dans votre fores t. Cette opération fait gagner du temps et Active toutes les fonctions de niveau de la forêt et au niveau du domaine dans Windows Server 2008.  
  
> [!IMPORTANT]  
> Si la forêt fonctionne au niveau de Windows Server 2008 fonctionnel niveau et que vous tentez d’installer Active Directory sur un serveur membre Windows Server 2003 ou un serveur membre basé sur Windows 2000, l’installation échoue.  
  
Pour plus d’informations sur l’augmentation des niveaux fonctionnels de forêt et du domaine et pour connaître les procédures effectuer ces tâches, consultez [déploiement d’un domaine racine de forêt Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>La mise à niveau des niveaux fonctionnels dans une nouvelle forêt Windows Server 2008 R2  
Lorsque vous installez le premier contrôleur de domaine dans une nouvelle forêt Windows Server 2008 R2, les niveaux fonctionnels sont définis par défaut pour les niveaux suivants et elles restent à ces niveaux, jusqu'à ce que vous les générez manuellement :  
  
-   Niveau fonctionnel du domaine Windows Server 2003  
  
-   Niveau fonctionnel de forêt Windows Server 2003  
  
Niveaux fonctionnels sont définis à ces niveaux par défaut pour vous donner la possibilité d’ajouter des contrôleurs de domaine Windows Server 2003 pour votre nouvelle forêt Windows Server 2008 R2. Après avoir créé un domaine racine de forêt, le niveau fonctionnel du domaine pour chaque domaine que vous ajoutez à la forêt de Windows Server 2008 R2 est défini à Windows Server 2003. Toutefois, si vous souhaitez que tous les contrôleurs de domaine dans votre nouvel environnement Windows Server 2008 R2 pour exécuter Windows Server 2008 R2, définir le niveau fonctionnel de forêt, puis le niveau fonctionnel du domaine vers Windows Server 2008 R2 lorsque vous installez le premier contrôleur de domaine dans yo votre forêt. Cette opération fait gagner du temps et Active toutes les fonctionnalités de niveau de la forêt et au niveau du domaine dans Windows Server 2008 R2.  
  
> [!IMPORTANT]  
> Si la forêt fonctionne au niveau fonctionnel Windows Server 2008 R2 et que vous essayez d’installer Active Directory sur un serveur Windows Server 2008-serveur membre ou Windows Server 2003, ou sur un serveur membre Windows 2000, l’installation échoue.  
  
Pour plus d’informations sur l’augmentation des niveaux fonctionnels de forêt et du domaine et pour connaître les procédures effectuer ces tâches, consultez [déploiement d’un domaine racine de forêt Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
> [!NOTE]  
> Bien que ADMT v3.1 doit être installé sur Windows Server 2008, vous pouvez utiliser ADMT 3.1 pour migrer des objets à un domaine qui est hébergé par un ou plusieurs contrôleurs de domaine Windows Server 2008 R2. Pour plus d’informations, consultez [article 976659](https://go.microsoft.com/fwlink/?LinkId=180398) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=180398).  
  


