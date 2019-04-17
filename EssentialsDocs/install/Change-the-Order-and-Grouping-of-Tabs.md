---
title: "Modifier l’ordre et le regroupement des onglets"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79a417fd-1b3e-47ab-ae33-bb1faf95c86d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 578c5619cfdf076bb2735254494f393d56d35713
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="change-the-order-and-grouping-of-tabs"></a>Modifier l’ordre et le regroupement des onglets

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez modifier l’ordre des onglets dans le tableau de bord afin que votre onglet est le premier (à gauche) dans la rangée d’onglets. Pour ce faire, vous ajoutez une entrée au Registre. Vous pouvez également changer le regroupement des onglets en ajoutant des entrées dans le Registre. L’ordre des onglets peut être votre onglet principal suivi des onglets intégrés Microsoft, suivi par l’un de vos onglets supplémentaires, puis les onglets tiers.  
  
## <a name="change-the-order-of-the-tabs-in-the-dashboard"></a>Modifier l’ordre des onglets dans le tableau de bord  
 Vous devez ajouter l’identificateur du complément qui affiche votre onglet dans le Registre pour définir l’ordre.  
  
#### <a name="to-display-your-tab-first-in-the-list-of-tabs"></a>Pour afficher votre onglet en premier dans la liste des onglets  
  
1.  Sur l’ordinateur de référence, cliquez sur **Démarrer**, entrez **regedit**, puis appuyez sur **entrée**.  
  
2.  Dans le volet gauche, développez **HKEY_LOCAL_MACHINE**, développez **logiciel**, développez **Microsoft**, puis développez **Windows Server**. Si le **OEM** clé n’existe pas, vous devez effectuer les étapes suivantes pour la créer:  
  
    1.  Avec le bouton droit **Windows Server**, pointez sur **New**, puis cliquez sur **clé**.  
  
    2.  Type **OEM** pour le nom de la clé.  
  
3.  Avec le bouton droit **OEM**, pointez sur **New**, puis cliquez sur **valeur de chaîne**.  
  
4.  Type **DashboardMainTabID** comme nom de chaîne, puis appuyez sur **entrée**.  
  
5.  Avec le bouton droit de la nouvelle chaîne dans le volet droit, puis cliquez sur **modifier**.  
  
6.  Entrez le GUID qui a été défini pour l’onglet de niveau supérieur et appuyez sur **entrée**.  
  
     Pour plus d’informations sur la création et l’identification des onglets de niveau supérieur, voir [créer un onglet de niveau supérieur](https://msdn.microsoft.com/library/gg513957) dans le SDK WindowsServerSolutions.  
  
7.  Enregistrer les modifications au Registre.  
  
8.  Vous devez également inclure le GUID de votre onglet principal de niveau supérieur dans la liste des identificateurs pour le regroupement des onglets. Pour ce faire, effectuez les étapes décrites dans **modifier le regroupement des onglets dans le tableau de bord**.  
  
## <a name="change-the-grouping-of-tabs-in-the-dashboard"></a>Modifier le regroupement des onglets dans le tableau de bord  
 Vous pouvez vous assurer que vos onglets sont regroupés et inclus dans la liste des onglets intégrés Microsoft, en ajoutant les identificateurs dans le Registre.  
  
#### <a name="to-change-the-grouping-of-tabs"></a>Pour modifier le regroupement des onglets  
  
1.  Si regedit n’est pas ouvert, cliquez sur **Démarrer**, entrez **regedit**, puis appuyez sur **entrée**.  
  
2.  Dans le volet gauche, développez **HKEY_LOCAL_MACHINE**, développez **logiciel**, développez **Microsoft**, puis développez **Windows Server**.  
  
3.  Avec le bouton droit **OEM**, pointez sur **New**, puis cliquez sur **clé**.  
  
4.  Type **DashboardAddins** comme nom de la clé, puis appuyez sur **entrée**.  
  
5.  Avec le bouton droit **DashboardAddins**, pointez sur **New**, puis cliquez sur **valeur de chaîne**.  
  
6.  Tapez l’identificateur GUID de votre onglet comme nom de chaîne. Une valeur n’est pas nécessaire.  
  
7.  Répétez les étapes5 et 6 pour chacun des onglets et sous-onglets.  
  
8.  Enregistrer les modifications du Registre.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)