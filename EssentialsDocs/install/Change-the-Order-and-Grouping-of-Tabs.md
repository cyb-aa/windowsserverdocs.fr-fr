---
title: Modification de l’ordre et du regroupement des onglets
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 79a417fd-1b3e-47ab-ae33-bb1faf95c86d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a07cfbef374ff86a8c7845917f0fbae85e7a2235
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817232"
---
# <a name="change-the-order-and-grouping-of-tabs"></a>Modification de l’ordre et du regroupement des onglets

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Il est possible de changer l’ordre des onglets dans le Tableau de bord de manière à afficher le vôtre en premier (à gauche) dans la rangée d’onglets. Pour ce faire, il convient d’ajouter une entrée au Registre. Vous pouvez également changer le mode de regroupement des onglets en ajoutant des entrées au Registre. Pourquoi pas, par exemple, présenter votre onglet principal suivi des onglets intégrés Microsoft, puis de vos onglets supplémentaires et enfin des onglets provenant de tierces parties ?  
  
## <a name="change-the-order-of-the-tabs-in-the-dashboard"></a>Modification de l’ordre des onglets dans le Tableau de bord  
 Pour définir l’ordre qui vous intéresse, commencez par ajouter au Registre l’identificateur du complément prévu pour afficher votre onglet.  
  
#### <a name="to-display-your-tab-first-in-the-list-of-tabs"></a>Pour afficher votre onglet en premier dans la liste des onglets  
  
1.  Sur l'ordinateur de référence, cliquez sur **Démarrer**, entrez **regedit**, puis appuyez sur **Entrée**.  
  
2.  Dans le volet gauche, développez successivement les entrées **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft** et **Windows Server**. Si la clé **OEM** n’existe pas, procédez de la façon suivante pour la créer :  
  
    1.  Cliquez avec le bouton droit sur **Windows Server**, pointez sur **Nouveau**, puis cliquez sur **Clé**.  
  
    2.  Donnez le nom **OEM** à la clé.  
  
3.  Cliquez avec le bouton droit sur **OEM**, pointez sur **Nouveau**, puis cliquez sur **Nom de la valeur**.  
  
4.  Entrez **DashboardMainTabID** comme nom de valeur, puis appuyez sur **Entrée**.  
  
5.  Cliquez avec le bouton droit sur la nouvelle chaîne dans le volet droit, puis cliquez sur **Modifier**.  
  
6.  Entrez le GUID défini pour l’onglet de niveau supérieur, puis appuyez sur **Entrée**.  
  
     Pour plus d’informations sur la création et l’identification des onglets de niveau supérieur, voir [Création d’un onglet de niveau supérieur](https://msdn.microsoft.com/library/gg513957) dans le Kit de développement logiciel (SDK) des solutions Windows Server.  
  
7.  Enregistrez les modifications dans le Registre.  
  
8.  Vous devez également inclure le GUID de votre onglet principal de niveau supérieur dans la liste des identificateurs en vue de procéder au regroupement des onglets. Pour ce faire, effectuez les étapes décrites à la rubrique **Modification du regroupement des onglets dans le Tableau de bord**.  
  
## <a name="change-the-grouping-of-tabs-in-the-dashboard"></a>Modification du regroupement des onglets dans le Tableau de bord  
 Pour vous assurer que vos onglets sont regroupés et qu’ils figurent dans la liste des onglets intégrés Microsoft, ajoutez les identificateurs requis au Registre.  
  
#### <a name="to-change-the-grouping-of-tabs"></a>Pour modifier le regroupement des onglets  
  
1.  Si regedit n'est pas ouvert, cliquez sur **Démarrer**, entrez **regedit**, puis appuyez sur **Entrée**.  
  
2.  Dans le volet gauche, développez successivement les entrées **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft** et **Windows Server**.  
  
3.  Cliquez avec le bouton droit sur **OEM**, pointez sur **Nouveau**, puis cliquez sur **Clé**.  
  
4.  Entrez **DashboardAddins** en guise de nom de clé, puis appuyez sur **Entrée**.  
  
5.  Cliquez avec le bouton droit sur **DashboardAddins**, pointez sur **Nouveau**, puis cliquez sur **Nom de la valeur**.  
  
6.  Entrez l’identificateur GUID de votre onglet comme nom de valeur. Il est inutile de spécifier une valeur.  
  
7.  Répétez les étapes 5 et 6 pour chacun des onglets et sous-onglets.  
  
8.  Enregistrez les modifications apportées au Registre.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)