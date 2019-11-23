---
title: Récupération de la forêt Active Directory-ajout du GC
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: f82033dd042847c7c735423c25756b936b137230
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369341"
---
# <a name="ad-forest-recovery---adding-the-gc"></a>Récupération de la forêt Active Directory-ajout du GC

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Utilisez la procédure suivante pour ajouter le catalogue global à un contrôleur de périphérique.  
  
## <a name="to-add-the-global-catalog"></a>Pour ajouter le catalogue global  
  
1. Cliquez sur **Démarrer**, pointez sur **tous les programmes**, sur **Outils d’administration**, puis cliquez sur **sites et services Active Directory**.  
2. Dans l’arborescence de la console, développez le conteneur **sites** , puis sélectionnez le site approprié qui contient le serveur cible.  
3. Développez le conteneur **serveurs** , puis développez l’objet serveur du contrôleur de périphérique auquel vous souhaitez ajouter le catalogue global.  
4. Cliquez avec le bouton droit sur **Paramètres NTDS**, puis cliquez sur **Propriétés**.  
5. Activez la case à cocher **catalogue global** .  
![ajouter un](media/AD-Forest-Recovery-Add-GC/addgc1.png) GC

## <a name="to-add-the-global-catalog-using-repadmin"></a>Pour ajouter le catalogue global à l’aide de repadmin  

- Ouvrez une invite de commandes avec élévation de privilèges, tapez la commande suivante et appuyez sur entrée :  

   ```  
   repadmin.exe /options DC_NAME +IS_GC  
   ```  

Voici les façons d’accélérer le processus d’ajout du catalogue global au contrôleur de domaine dans le domaine racine :  

- Dans l’idéal, le contrôleur de domaine dans le domaine racine doit être un partenaire de réplication des contrôleurs de domaine restaurés dans les domaines non racine. Si c’est le cas, vérifiez que le vérificateur de cohérence des connaissances (KCC) a créé l’objet **repsFrom** correspondant pour le DC source et la partition dans le DC racine. Vous pouvez le vérifier en exécutant la commande **repadmin/showreps/v** . 

- Si aucun objet **repsFrom** n’est créé, créez cet objet pour la partition de configuration. De cette façon, le contrôleur de domaine dans le domaine racine peut déterminer les contrôleurs de domaine du domaine non racine qui ont été supprimés. Pour ce faire, vous pouvez utiliser les commandes suivantes :  

   ```
   repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
   ```

   ```
   repadmin /options DSA -Disable_NTDSCONN_XLATE  
   ```

   Le format de *SourceDomainControllerCNAME* est le suivant :  

   ```
  
   sourceDCGuid._msdcs.root domain  
   ```

   Par exemple, la commande repadmin/Add pour la partition de configuration du domaine contoso.com peut être :  

   ```
   repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
   ```

- Si l’objet **repsFrom** est présent, essayez de synchroniser le contrôleur de domaine dans le domaine racine avec le contrôleur de domaine dans le domaine non racine comme suit :  

   ```
   Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
   ```

   Où *DestinationDomainController* est le contrôleur de domaine dans le domaine racine et *SOURCEDOMAINCONTROLLER* est le contrôleur de domaine restauré dans le domaine non racine. 

- Le serveur DNS du domaine racine doit avoir les enregistrements de ressource alias (CNAMe) pour le contrôleur de domaine source. Vérifiez que la zone DNS parent contient les enregistrements de ressource de délégation (serveur de noms (NS) et les enregistrements de ressource d’hôte (A)) pour les contrôleurs de domaine appropriés (les contrôleurs de domaine qui ont été restaurés à partir de la sauvegarde) dans la zone enfant. 
- Assurez-vous que le contrôleur de domaine dans le domaine racine contacte le centre de distribution de clés approprié (KDC) dans le domaine non racine. Pour tester cela, à l’invite de commandes, tapez la commande suivante, puis appuyez sur entrée :  

   ```
   nltest /dsgetdc:nonroot domain name /KDC /Force  
   ```

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)  
