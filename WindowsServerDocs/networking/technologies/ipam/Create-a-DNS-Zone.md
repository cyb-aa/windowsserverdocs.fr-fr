---
title: Créer une zone DNS
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0ae9869b95cfa1da04e0103b5a824ff1fc21568f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814752"
---
# <a name="create-a-dns-zone"></a>Créer une zone DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour créer une zone DNS à l’aide de la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-create-a-dns-zone"></a>Pour créer une zone DNS  
  
1.  Dans Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, sous **surveiller et gérer**, cliquez sur **serveurs DNS et DHCP**. Dans le volet d’affichage, cliquez sur **type de serveur**, puis sur **DNS**. Tous les serveurs DNS gérés par IPAM sont répertoriés dans les résultats de la recherche.  
  
3.  Localisez le serveur sur lequel vous souhaitez ajouter une zone, puis cliquez avec le bouton droit sur le serveur.  Cliquez sur **créer une zone DNS**.  
  
    ![Créer une zone DNS](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  La boîte de dialogue **créer une zone DNS** s’ouvre. Dans **Propriétés générales**, sélectionnez une catégorie de zone, un type de zone, puis entrez un nom dans la **zone Nom**. Sélectionnez également les valeurs appropriées pour votre déploiement dans **Propriétés avancées**, puis cliquez sur **OK**.  
  
    ![Propriétés avancées](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des zones DNS](DNS-Zone-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


