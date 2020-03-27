---
title: Ajouter un enregistrement de ressource DNS
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b89502a7ba5ceceea10e1f7cfae9e91f6a7ead59
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316907"
---
# <a name="add-a-dns-resource-record"></a>Ajouter un enregistrement de ressource DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour ajouter un ou plusieurs nouveaux enregistrements de ressource DNS à l’aide de la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-add-a-dns-resource-record"></a>Pour ajouter un enregistrement de ressource DNS  
  
1.  Dans Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, sous **surveiller et gérer**, cliquez sur **zones DNS**.  Le volet de navigation est divisé en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Dans le volet de navigation inférieur, cliquez sur **recherche directe**. Toutes les zones de recherche directe DNS gérées par IPAM s’affichent dans les résultats de la recherche du volet d’affichage. Cliquez avec le bouton droit sur la zone dans laquelle vous souhaitez ajouter un enregistrement de ressource, puis cliquez sur **Ajouter un enregistrement de ressource DNS**.  
  
    ![Ajouter un enregistrement de ressource DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  La boîte de dialogue **Ajouter des enregistrements de ressources DNS** s’ouvre. Dans **Propriétés**de l’enregistrement de ressource, cliquez sur **serveur DNS** et sélectionnez le serveur DNS dans lequel vous souhaitez ajouter un ou plusieurs nouveaux enregistrements de ressource. Dans **configurer les enregistrements de ressources DNS**, cliquez sur **nouveau**.  
  
    ![Configurer les enregistrements de ressources DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  La boîte de dialogue se développe pour afficher **un nouvel enregistrement de ressource**. Cliquez sur **type d’enregistrement de ressource**.  
  
    ![Type d’enregistrement de ressource](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  La liste des types d’enregistrements de ressources s’affiche. Cliquez sur le type d’enregistrement de ressource que vous souhaitez ajouter.  
  
    ![Sélectionner le type d’enregistrement à ajouter](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  Dans **nouvel enregistrement de ressource,** dans **nom**, tapez un nom d’enregistrement de ressource. Dans **adresse IP**, tapez une adresse IP, puis sélectionnez les propriétés d’enregistrement de ressource appropriées pour votre déploiement. Cliquez sur **Ajouter un enregistrement de ressource**.  
  
    ![Ajouter un enregistrement de ressource](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  Si vous ne souhaitez pas créer d’autres enregistrements de ressources, cliquez sur **OK**. Si vous souhaitez créer des enregistrements de ressources supplémentaires, cliquez sur **nouveau**.  
  
    ![Cliquez sur OK ou nouveau](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. La boîte de dialogue se développe pour afficher **un nouvel enregistrement de ressource**. Cliquez sur **type d’enregistrement de ressource**. La liste des types d’enregistrements de ressources s’affiche. Cliquez sur le type d’enregistrement de ressource que vous souhaitez ajouter.  
  
10. Dans **nouvel enregistrement de ressource,** dans **nom**, tapez un nom d’enregistrement de ressource. Dans **adresse IP**, tapez une adresse IP, puis sélectionnez les propriétés d’enregistrement de ressource appropriées pour votre déploiement. Cliquez sur **Ajouter un enregistrement de ressource**.  
  
    ![Ajouter un enregistrement de ressource](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. Si vous souhaitez ajouter d’autres enregistrements de ressources, répétez le processus de création d’enregistrements. Lorsque vous avez terminé de créer des enregistrements de ressources, cliquez sur **appliquer**.  
  
    ![Création d’un enregistrement de ressource complet](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. La boîte de dialogue **Ajouter un enregistrement de ressource** affiche un résumé des enregistrements de ressources pendant que IPAM crée les enregistrements de ressource sur le serveur DNS que vous avez spécifié. Lorsque les enregistrements sont correctement créés, l' **État** de l’enregistrement est **Success**.  
  
    ![État de l’ajout d’enregistrements](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. Cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des enregistrements de ressources DNS](DNS-Resource-Record-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


