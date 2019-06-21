---
title: Ajouter un enregistrement de ressource DNS
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 36773525187229e498b9addf4b1e6532fd413701
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282321"
---
# <a name="add-a-dns-resource-record"></a>Ajouter un enregistrement de ressource DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour ajouter un ou plusieurs enregistrements de ressource DNS nouvelle à l’aide de la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-add-a-dns-resource-record"></a>Pour ajouter un enregistrement de ressource DNS  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, dans **surveiller et gérer**, cliquez sur **Zones DNS**.  Le volet de navigation divise en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Dans le volet de navigation inférieur, cliquez sur **recherche directe**. Toutes les zones de recherche de DNS directes gérés par IPAM sont affichés dans les résultats de recherche du volet Affichage. Avec le bouton droit de la zone où vous souhaitez ajouter un enregistrement de ressource, puis cliquez sur **enregistrement de ressource DNS ajouter**.  
  
    ![Ajouter un enregistrement de ressource DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  Le **ajouter des enregistrements de ressource DNS** boîte de dialogue s’ouvre. Dans **propriétés enregistrements de ressource**, cliquez sur **serveur DNS** et sélectionnez le serveur DNS où vous souhaitez ajouter un ou plusieurs nouveaux enregistrements de ressources. Dans **les enregistrements de ressource de configurer des serveurs DNS**, cliquez sur **New**.  
  
    ![Configurer les enregistrements de ressource DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  La boîte de dialogue se développe pour afficher **nouvel enregistrement de ressource**. Cliquez sur **type d’enregistrement de ressource**.  
  
    ![Type d’enregistrement de ressource](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  La liste des types d’enregistrement de ressource s’affiche. Cliquez sur le type d’enregistrement de ressource que vous souhaitez ajouter.  
  
    ![Sélectionnez le type d’enregistrement à ajouter](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  Dans **nouvel enregistrement de ressource,** dans **nom**, tapez un nom d’enregistrement de ressource. Dans **adresse IP**, tapez une adresse IP, puis sélectionnez les propriétés d’enregistrement de ressource qui sont appropriées pour votre déploiement. Cliquez sur **ajouter l’enregistrement de ressource**.  
  
    ![Ajouter un enregistrement de ressource](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  Si vous ne souhaitez pas créer de nouveaux enregistrements de ressources supplémentaires, cliquez sur **OK**. Si vous souhaitez créer des enregistrements de ressources supplémentaires, cliquez sur **New**.  
  
    ![Cliquez sur OK ou nouvelles](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. La boîte de dialogue se développe pour afficher **nouvel enregistrement de ressource**. Cliquez sur **type d’enregistrement de ressource**. La liste des types d’enregistrement de ressource s’affiche. Cliquez sur le type d’enregistrement de ressource que vous souhaitez ajouter.  
  
10. Dans **nouvel enregistrement de ressource,** dans **nom**, tapez un nom d’enregistrement de ressource. Dans **adresse IP**, tapez une adresse IP, puis sélectionnez les propriétés d’enregistrement de ressource qui sont appropriées pour votre déploiement. Cliquez sur **ajouter l’enregistrement de ressource**.  
  
    ![Ajouter un enregistrement de ressource](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. Si vous souhaitez ajouter des enregistrements de ressources supplémentaires, répétez le processus de création d’enregistrements. Lorsque vous avez terminé la création de nouveaux enregistrements de ressources, cliquez sur **appliquer**.  
  
    ![Création d’enregistrements de ressources complet](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. Le **ajouter un enregistrement de ressource** boîte de dialogue affiche un résumé d’enregistrements de ressources pendant qu’IPAM crée les enregistrements de ressources sur le serveur DNS que vous avez spécifié. Lorsque les enregistrements sont créés avec succès, le **état** de l’enregistrement est **réussite**.  
  
    ![État de l’addition enregistrement](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. Cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des enregistrements ressource DNS](DNS-Resource-Record-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


