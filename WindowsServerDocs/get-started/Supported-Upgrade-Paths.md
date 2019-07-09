---
title: Options de mise à niveau et de conversion pour Windows Server 2016
description: Présente tous les chemins de mise à niveau pris en charge pour Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 01/18/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 74aa1da3-7076-4a1f-ad5b-9e17bd46dba2
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 299cf420b44e4a15985d00489edf84784316540d
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810575"
---
# <a name="upgrade-and-conversion-options-for-windows-server-2016"></a>Options de mise à niveau et de conversion pour Windows Server 2016

>S'applique à : Windows Server 2019, Windows Server 2016

Cette rubrique comprend des informations sur la mise à niveau vers Windows Server2016 depuis plusieurs systèmes d’exploitation antérieurs à l’aide de différentes méthodes.

En fonction du système d’exploitation de départ et de la méthode choisie, le processus d’installation de Windows Server 2016 peut varier grandement. Nous utilisons les termes suivants pour différencier les actions qui font partie d’un nouveau déploiement de Windows Server 2016.

- **Installation** représente le concept de base d’obtention d’un nouveau système d’exploitation pour votre matériel. Une **nouvelle installation** exige la suppression du système d’exploitation précédent. Pour plus d’informations sur l’installation de Windows Server 2016, voir [Configuration requise et informations d’installation pour Windows Server 2016](https://technet.microsoft.com/windows-server-docs/get-started/system-requirements--and-installation). Pour plus d’informations sur l’installation d’autres versions de Windows Server, voir [Windows Server Installation and Upgrade](https://technet.microsoft.com//windowsserver/dn527667) (Installation et mise à niveau de Windows Server).

- Une **migration** consiste à passer d’un système d’exploitation existant à Windows Server 2016 en effectuant un transfert sur une autre configuration matérielle ou machine virtuelle. La migration, qui dépend des rôles serveur installés, est décrite en détail dans l’article [Windows Server Installation, Upgrade, and Migration](https://technet.microsoft.com/windowsserver/dn458795) (Installation, mise à niveau et migration de Windows Server).

- La **mise à niveau propagée de système d’exploitation de cluster** est une nouvelle fonctionnalité de Windows Server 2016 qui permet à un administrateur de mettre à niveau le système d’exploitation des nœuds de cluster Windows Server 2016 R2 vers Windows Server 2012, sans arrêter les charges de travail du serveur de fichiers avec montée en puissance parallèle ou Hyper-V. Cette fonctionnalité élimine les temps d’arrêt susceptibles d’affecter les contrats de niveau de service. Cette nouvelle fonctionnalité est décrite plus en détail dans l’article [Mise à niveau propagée de système d’exploitation de cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

- **Conversion de licence** : dans certaines versions de systèmes d’exploitation, vous pouvez convertir une édition particulière de la version vers une autre édition de la même version en une seule étape à l’aide d’une simple commande et de la clé de licence appropriée. Cette opération est appelée « conversion de licence ». Par exemple, si vous exécutez Windows Server 2016 Standard, vous pouvez effectuer une conversion vers Windows Server 2016 Datacenter.

- Une **mise à niveau** correspond au passage d’une version de système d’exploitation existante à une version plus récente, sur le même matériel. (Cette opération est parfois appelée mise à niveau « sur place ».) Par exemple, si votre serveur exécute Windows Server 2012 ou Windows Server 2012 R2, vous pouvez le mettre à niveau vers Windows Server 2016. Vous pouvez effectuer une mise à niveau à partir d’une version d’évaluation du système d’exploitation vers une version commercialisée, d’une version commercialisée plus ancienne vers une version plus récente ou, dans certains cas, d’une édition de licence en volume du système d’exploitation vers une version commercialisée ordinaire.

> [!IMPORTANT]  
> La mise à niveau convient le mieux aux machines virtuelles qui ne nécessitent pas de pilotes de matériel OEM spécifiques pour que l’opération réussisse.  

> [!IMPORTANT]  
> Pour les versions de Windows Server 2016 antérieures à la version 14393.0.161119-1705.RS1_REFRESH, **vous pouvez uniquement effectuer cette conversion d’une version d’évaluation vers une version commerciale** si Windows Server 2016 a été installé à l’aide de l’option Serveur avec Expérience utilisateur (et non l’option d’installation minimale). Avec la version 14393.0.161119-1705. RS1_REFRESH et les versions ultérieures, vous pouvez convertir les versions d’évaluation vers une version commerciale, quelle que soit l’option d’installation utilisée.

> [!IMPORTANT]  
> Si votre serveur utilise l’association de cartes réseau, désactivez cette fonctionnalité avant de le mettre à niveau, puis réactivez-la une fois la mise à niveau terminée. Pour plus d’informations, consultez [Vue d’ensemble de l’association de cartes réseau](https://technet.microsoft.com/library/hh831648(v=ws.11).aspx).

## <a name="upgrading-previous-retail-versions-of-windows-server-to-windows-server-2016"></a>Mise à niveau de versions commerciales antérieures de WindowsServer vers WindowsServer2016

Le tableau ci-dessous récapitule brièvement les mises à niveau possibles de systèmes d’exploitation Windows **déjà sous licence** (c’est-à-dire qui ne sont pas des versions d’évaluation) vers des éditions spécifiques de Windows Server 2016.

Notez les directives générales suivantes pour les chemins d’accès pris en charge :

- Les mises à niveau d’architectures 32 bits vers des architectures 64 bits ne sont pas prises en charge. Toutes les éditions de Windows Server 2016 sont des éditions 64 bits uniquement.
- Les mises à niveau d’une langue vers une autre ne sont pas prises en charge.
- Si le serveur est un contrôleur de domaine, voir [Mettre à niveau des contrôleurs de domaine vers Windows Server 2012 R2 et Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx) pour obtenir des informations importantes.
- Les mises à niveau depuis des versions préliminaires de Windows Server 2016 ne sont pas prises en charge. Effectuez une nouvelle installation de Windows Server 2016.
- Les mises à niveau qui passent d’une installation minimale à une installation Serveur avec Expérience utilisateur (et vice versa) ne sont pas prises en charge.
- La mise à niveau d’une installation précédente de Windows Server vers une version d’évaluation de Windows Server n’est pas prise en charge. Les versions d’évaluation doivent faire l’objet d’une nouvelle installation.

Si vous ne voyez pas votre version actuelle dans la colonne de gauche, cela signifie que la mise à niveau vers cette version de Windows Server2016 n’est pas prise en charge.

Si vous voyez plusieurs éditions dans la colonne de droite, la mise à niveau vers **toutes** ces éditions est prise en charge depuis la même version de départ.

|Si vous exécutez cette édition :|Vous pouvez effectuer une mise à niveau vers ces éditions :|  
|-------------------|----------|  
|Windows Server 2012 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|


## <a name="per-server-role-considerations-for-upgrading"></a>Éléments à prendre en compte par rôle serveur pour la mise à niveau

Même dans les chemins de mise à niveau de versions commerciales antérieures vers Windows Server 2016 pris en charge, certains rôles serveur déjà installés peuvent nécessiter une préparation ou des actions supplémentaires pour continuer à fonctionner après la mise à niveau. Pour obtenir plus de détails sur les étapes supplémentaires qui peuvent être requises, consultez les rubriques de la bibliothèque TechNet pour chaque rôle serveur que vous envisagez de mettre à niveau.

## <a name="converting-a-current-evaluation-version-to-a-current-retail-version"></a>Conversion d’une version d’évaluation actuelle vers une version commercialisée actuelle

Vous pouvez convertir la version d’évaluation de Windows Server 2016 Standard vers Windows Server 2016 Standard (version commerciale) ou Datacenter (version commerciale). De même, vous pouvez convertir la version d’évaluation de Windows Server2016 Datacenter vers la version commerciale.

> [!IMPORTANT]  
> Pour les versions de Windows Server 2016 antérieures à la version 14393.0.161119-1705.RS1_REFRESH, vous pouvez uniquement effectuer cette conversion d’une version d’évaluation vers une version commerciale si Windows Server 2016 a été installé à l’aide de l’option Serveur avec Expérience de bureau (et non l’option d’installation minimale). Avec la version 14393.0.161119-1705. RS1_REFRESH et les versions ultérieures, vous pouvez convertir les versions d’évaluation vers une version commerciale, quelle que soit l’option d’installation utilisée.

Avant de tenter d’effectuer une conversion d’une version d’évaluation vers une version commercialisée, vérifiez que votre serveur exécute réellement une version d’évaluation. Pour ce faire, effectuez l’une ou l’autre des opérations suivantes :

- À partir d’une invite de commandes avec élévation de privilèges, exécutez **slmgr.vbs /dlv**; les versions d’évaluation incluent « EVAL » dans la sortie.

- Dans l’écran d’accueil, ouvrez le **Panneau de configuration**. Ouvrez **Système et sécurité**, puis **Système**. Affichez l’état d’activation de Windows dans la zone Activation de Windows de la page **Système**. Pour plus d’informations sur l’état d’activation de votre installation de Windows, cliquez sur **Afficher les détails** dans Activation de Windows.

Si vous avez déjà activé Windows, le Bureau affiche le temps restant de la période d’évaluation.

Si le serveur exécute une version commerciale au lieu d’une version d’évaluation, consultez la section « Mise à niveau de versions commerciales antérieures de Windows Server vers Windows Server 2016 » de cette rubrique pour connaître la procédure de mise à niveau vers Windows Server 2016.

Pour **Windows Server 2016 Essentials** : vous pouvez passer à la version commercialisée complète en entrant une clé de version de licence en volume, commercialisée ou OEM dans la commande **slmgr.vbs**.

Si le serveur exécute une version d’évaluation de Windows Server 2016 Standard ou de Windows Server 2016 Datacenter, vous pouvez la convertir en version commerciale en procédant comme suit :

1.  Si le serveur est un **contrôleur de domaine**, vous ne pouvez pas le convertir en version commerciale. Dans ce cas, installez un contrôleur de domaine supplémentaire sur un serveur qui exécute une version commerciale, puis supprimez les services AD DS du contrôleur de domaine qui exécute la version d’évaluation. Pour plus d’informations, consultez [Mettre à niveau des contrôleurs de domaine vers Windows Server 2012 R2 et Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx).
2.  Lisez les termes du contrat de licence.
3.  À partir d’une invite de commandes avec élévation de privilèges, déterminez le nom de l’édition actuelle à l’aide de la commande **DISM /online /Get-CurrentEdition**. Relevez l’ID de l’édition, qui est une forme abrégée du nom de l’édition. Ensuite, exécutez **DISM /online /Set-Edition:\<ID de l’édition\> /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula**, en indiquant l’ID de l’édition et une clé de produit commercialisé. Le serveur redémarre deux fois.

Pour la version d’évaluation de Windows Server2016 Standard, vous pouvez également passer à la version commercialisée de Windows Server2016 Datacenter en une étape à l’aide de cette même commande et de la clé de produit appropriée.

> [!TIP] 
> Pour plus d’informations sur Dism.exe, consultez [DISM Command-line options](https://go.microsoft.com/fwlink/?LinkId=192466) (Options de ligne de commande DISM).

## <a name="converting-a-current-retail-edition-to-a-different-current-retail-edition"></a>Conversion d’une version commercialisée actuelle en une autre version commercialisée actuelle

À tout moment après l’installation de Windows Server 2016, vous pouvez exécuter le programme d’installation pour réparer l’installation (opération parfois appelée « réparation sur place ») ou, dans certains cas, effectuer une conversion en une autre édition.
Vous pouvez exécuter le programme d’installation pour effectuer une «réparation sur place» sur n’importe quelle édition de Windows Server2016; vous obtiendrez la même édition que celle avec laquelle vous avez commencé.

Pour Windows Server 2016 Standard, vous pouvez effectuer une conversion vers Windows Server 2016 Datacenter comme suit : À partir d’une invite de commandes avec élévation de privilèges, déterminez le nom de l’édition actuelle à l’aide de la commande **DISM /online /Get-CurrentEdition**. Pour Windows Server 2016 Standard, il s’agira de `ServerStandard`. Exécutez la commande **DISM /online /Get-TargetEditions** pour obtenir l’ID de l’édition vers laquelle vous pouvez effectuer une mise à niveau. Relevez l’ID de cette édition, qui est une forme abrégée du nom de l’édition. Ensuite, exécutez **DISM /online /Set-Edition:\<ID_édition\> /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula**, en indiquant l’ID d’édition de votre cible et sa clé de produit commercialisé. Le serveur redémarre deux fois.

## <a name="converting-a-current-retail-version-to-a-current-volume-licensed-version"></a>Conversion d’une version commercialisée actuelle en une version de licence en volume actuelle

À tout moment après l’installation de Windows Server 2016, vous pouvez convertir gratuitement votre version en une version commercialisée, une version de licence en volume ou une version OEM. L’édition reste la même pendant cette conversion. Si vous débutez avec une version d’évaluation, convertissez-la d’abord en version commercialisée, puis en une autre version en suivant la procédure décrite ici.

Pour ce faire, à partir d’une invite de commandes avec élévation de privilèges, exécutez : **slmgr /ipk \<clé\>**

(Remplacez \<clé\> par la clé de produit de licence en volume, commercialisé ou OEM appropriée.)
