---
title: "Récupération de la forêt ActiveDirectory - ajouter le catalogue global"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 3749fd99737f61c55871f613b9feaa21090d3bae
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---adding-the-gc"></a>Récupération de la forêt ActiveDirectory - ajouter le catalogue global 

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

 Utilisez la procédure suivante pour ajouter le catalogue global à un contrôleur de domaine.  
  
## <a name="to-add-the-global-catalog"></a>Pour ajouter le catalogue global  
  
1.  Cliquez sur **Démarrer**, pointez sur **tous les programmes**, pointez sur **outils d’administration**, puis cliquez sur **Services et Sites ActiveDirectory**.  
2.  Dans l’arborescence de la console, développez le **Sites** conteneur, puis sélectionnez le site approprié qui contient le serveur cible.  
3.  Développez le **serveurs** conteneur, puis développez l’objet serveur pour le contrôleur de domaine auquel vous souhaitez ajouter le catalogue global.  
4.  Avec le bouton droit **Paramètres NTDS**, puis cliquez sur **propriétés**.  
5.  Sélectionnez le **catalogue Global** case à cocher.  
![Ajouter le catalogue global](media/AD-Forest-Recovery-Add-GC/addgc1.png)
  
## <a name="to-add-the-global-catalog-using-repadmin"></a>Pour ajouter le catalogue global à l’aide de Repadmin  
  
1.  Ouvrez une invite de commandes avec élévation de privilèges, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    repadmin.exe /options DC_NAME +IS_GC  
    ```  
  
 Voici comment accélérer le processus d’ajout du catalogue global au contrôleur de domaine dans le domaine racine:  
  
-   Dans l’idéal, le contrôleur de domaine dans le domaine racine doit être un partenaire de réplication des contrôleurs de domaine restaurés dans les domaines non racine. Dans ce cas, vérifiez que le vérificateur de cohérence des connaissances (KCC) a créé le correspondant **repsFrom** objet pour le contrôleur de domaine source et de la partition à la racine du contrôleur de domaine. Vous pouvez confirmer ceci en exécutant la **repadmin /showreps /v** commande.  
  
-   S’il existe aucun **repsFrom** objet créé, créer cet objet pour la partition de configuration. De cette façon, le contrôleur de domaine dans le domaine racine peut déterminer quels contrôleurs de domaine dans le domaine non racine ont été supprimés. Vous pouvez effectuer cette opération avec les commandes suivantes:  
  
    ```  
    repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
    ```  
  
    ```  
    repadmin /options DSA -Disable_NTDSCONN_XLATE  
    ```  
  
     Le format de la *SourceDomainControllerCNAME* est:  
  
    ```  
  
    sourceDCGuid._msdcs.root domain  
    ```  
  
     Par exemple, la commande repadmin /add pour la partition de configuration du domaine contoso.com peut être:  
  
    ```  
    repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
    ```  
  
-   Si le **repsFrom** objet n’est présent, essayez de synchroniser le contrôleur de domaine dans le domaine racine avec le contrôleur de domaine dans le domaine non racine comme suit:  
  
    ```  
    Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
    ```  
  
     Où *DestinationDomainController* est le contrôleur de domaine dans le domaine racine et *SourceDomainController* est le contrôleur de domaine restauré dans le domaine non racine.  
  
-   Le serveur DNS du domaine racine doit avoir les enregistrements de ressource alias (CNAME) pour le contrôleur de domaine source. Assurez-vous que la zone DNS parente contienne les enregistrements de ressource de délégation (serveur de noms (NS) et les enregistrements de ressource hôte (A)) pour les contrôleurs de domaine corrects (les contrôleurs de domaine qui ont été restaurés à partir de la sauvegarde) dans la zone enfant.  
  
-   Assurez-vous que le contrôleur de domaine dans le domaine racine contacte le correcte du centre KDC (Key Distribution) dans le domaine non racine. Pour tester cela, à l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    nltest /dsgetdc:nonroot domain name /KDC /Force  
    ```  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)  
