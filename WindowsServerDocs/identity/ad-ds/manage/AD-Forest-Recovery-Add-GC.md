---
title: Récupération de forêt AD - ajouter le catalogue global
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 156a4a64d9c3bb8261bd603b72ae11b81ff1d152
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825630"
---
# <a name="ad-forest-recovery---adding-the-gc"></a>Récupération de forêt AD - ajouter le catalogue global

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Utilisez la procédure suivante pour ajouter le catalogue global à un contrôleur de domaine.  
  
## <a name="to-add-the-global-catalog"></a>Pour ajouter le catalogue global  
  
1. Cliquez sur **Démarrer**, pointez sur **tous les programmes**, pointez sur **outils d’administration**, puis cliquez sur **Sites Active Directory et Services**.  
2. Dans l’arborescence de la console, développez le **Sites** conteneur, puis sélectionnez le site approprié qui contient le serveur cible.  
3. Développez le **serveurs** conteneur, puis développez l’objet serveur pour le contrôleur de domaine auquel vous souhaitez ajouter le catalogue global.  
4. Avec le bouton droit **Paramètres NTDS**, puis cliquez sur **propriétés**.  
5. Sélectionnez le **catalogue Global** case à cocher.  
![Ajouter le catalogue global](media/AD-Forest-Recovery-Add-GC/addgc1.png)

## <a name="to-add-the-global-catalog-using-repadmin"></a>Pour ajouter le catalogue global à l’aide de Repadmin  

- Ouvrez une invite de commandes avec élévation de privilèges, tapez la commande suivante et appuyez sur ENTRÉE :  

   ```  
   repadmin.exe /options DC_NAME +IS_GC  
   ```  

Voici les façons d’accélérer le processus d’ajout du catalogue global au contrôleur de domaine dans le domaine racine :  

- Dans l’idéal, le contrôleur de domaine dans le domaine racine doit être un partenaire de réplication des contrôleurs de domaine restaurés dans les domaines non racine. Dans ce cas, vérifiez que le vérificateur de cohérence des connaissances (KCC) a créé le correspondantes **repsFrom** objet pour le contrôleur de domaine source et de la partition à la racine du contrôleur de domaine. Vous pouvez le vérifier en exécutant la **repadmin /showreps /v** commande. 

- S’il existe aucune **repsFrom** objet créé, créer cet objet pour la partition de configuration. De cette façon, le contrôleur de domaine dans le domaine racine peut déterminer quels contrôleurs de domaine dans le domaine non racine ont été supprimés. Vous pouvez le faire avec les commandes suivantes :  

   ```
   repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
   ```

   ```
   repadmin /options DSA -Disable_NTDSCONN_XLATE  
   ```

   Le format pour le *SourceDomainControllerCNAME* est :  

   ```
  
   sourceDCGuid._msdcs.root domain  
   ```

   Par exemple, le repadmin / ajouter des commandes pour la partition de configuration du domaine contoso.com peut être :  

   ```
   repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
   ```

- Si le **repsFrom** objet n’est présent, essayez de synchroniser le contrôleur de domaine dans le domaine racine avec le contrôleur de domaine dans le domaine non racine comme suit :  

   ```
   Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
   ```

   Où *DestinationDomainController* est le contrôleur de domaine dans le domaine racine et *SourceDomainController* est le DC restauré dans le domaine non racine. 

- Le serveur DNS du domaine racine doit avoir les enregistrements de ressource alias (CNAME) pour le contrôleur de domaine source. Assurez-vous que la zone DNS parente contienne les enregistrements de ressource de délégation (serveur de noms (NS) et les enregistrements de ressource hôte (A)) pour les contrôleurs de domaine corrects (les contrôleurs de domaine qui ont été restaurés à partir de la sauvegarde) dans la zone enfant. 
- Assurez-vous que le contrôleur de domaine dans le domaine racine contacte le centre de Distribution clé correcte (KDC) dans le domaine non racine. Pour tester cela, à l’invite de commandes, tapez la commande suivante, puis appuyez sur ENTRÉE :  

   ```
   nltest /dsgetdc:nonroot domain name /KDC /Force  
   ```

## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)  
