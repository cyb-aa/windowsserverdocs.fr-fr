---
title: Filtrer l’affichage des enregistrements de ressources DNS
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 21e43751981b0b7b945c0c9404f6f93f36c48f16
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860662"
---
# <a name="filter-the-view-of-dns-resource-records"></a>Filtrer l’affichage des enregistrements de ressources DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour filtrer l’affichage des enregistrements de ressources DNS dans la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>Pour filtrer l’affichage des enregistrements de ressources DNS  
  
1.  Dans Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, sous **surveiller et gérer**, cliquez sur **zones DNS**.  Le volet de navigation est divisé en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Dans le volet de navigation inférieur, cliquez sur **recherche directe**. Toutes les zones de recherche directe DNS gérées par IPAM s’affichent dans les résultats de la recherche du volet d’affichage.  
  
4.  Cliquez sur la zone dont vous souhaitez afficher et filtrer les enregistrements.  
  
5.  Dans le volet d’informations, cliquez sur **affichage actuel**, puis cliquez sur **enregistrements de ressources**. Les enregistrements de ressources pour la zone s’affichent dans le volet d’informations.  
  
6.  Dans le volet d’affichage, cliquez sur **Ajouter des critères**.  
  
    ![Ajouter des critères](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  Sélectionnez un critère dans la liste déroulante. Par exemple, si vous souhaitez afficher un type d’enregistrement spécifique, cliquez sur **type d’enregistrement**.  
  
    ![Sélectionner un critère](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  Cliquez sur **Ajouter**.  
  
    ![Ajouter les critères](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. Le **type d’enregistrement** est ajouté en tant que paramètre de recherche. Entrez le texte du type d’enregistrement que vous souhaitez rechercher. Par exemple, si vous souhaitez afficher uniquement les enregistrements SRV, tapez **SRV**.  
  
    ![Spécifiez le type d’enregistrement que vous souhaitez rechercher.](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. Appuyez sur ENTRÉE. Les enregistrements de ressources DNS sont filtrés en fonction des critères et de l’expression de recherche que vous avez spécifiés.  
  
    ![Exécuter le filtre](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des enregistrements de ressources DNS](DNS-Resource-Record-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


