---
title: Gérer des ressources dans plusieurs forêts Active Directory
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9a3e58a3ca8ca5334996776644bfb2c3b19cbc2e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309598"
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>Gérer des ressources dans plusieurs forêts Active Directory

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à utiliser IPAM pour gérer les contrôleurs de domaine, les serveurs DHCP et les serveurs DNS dans plusieurs forêts Active Directory.  
  
Pour utiliser IPAM afin de gérer les ressources dans les forêts Active Directoryes distantes, chaque forêt que vous souhaitez gérer doit avoir une relation d’approbation bidirectionnelle avec la forêt dans laquelle IPAM est installé.  
  
Pour démarrer le processus de découverte pour différentes forêts de Active Directory, ouvrez Gestionnaire de serveur et cliquez sur IPAM. Dans la console client IPAM, cliquez sur **configurer la découverte de serveurs**, puis sur récupérer les **forêts**. Cela lance une tâche en arrière-plan qui Découvre les forêts approuvées et leurs domaines. Une fois le processus de découverte terminé, cliquez sur **configurer la découverte de serveurs**pour ouvrir la boîte de dialogue suivante.  
  
![Configurer la découverte de serveurs](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>Pour stratégie de groupe l’approvisionnement\-pour un scénario inter-forêts Active Directory, veillez à exécuter l’applet de commande Windows PowerShell suivante sur le serveur IPAM et non sur les contrôleurs de domaine de confiance. Par exemple, si votre serveur IPAM est joint à la forêt corp.contoso.com et que la forêt d’approbation est fabrikam.com, vous pouvez exécuter l’applet de commande Windows PowerShell suivante sur le serveur IPAM dans corp.contoso.com pour stratégie de groupe\-la configuration basée sur la forêt fabrikam.com. Pour exécuter cette applet de commande, vous devez être membre du groupe Admins du domaine dans la forêt fabrikam.com.

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

Dans la boîte de dialogue **configurer la découverte de serveurs** , cliquez sur **Sélectionner la forêt**, puis sélectionnez la forêt que vous souhaitez gérer avec IPAM. Sélectionnez également les domaines que vous souhaitez gérer, puis cliquez sur **Ajouter**.

Dans **Sélectionner les rôles de serveur à découvrir**, pour chaque domaine que vous souhaitez gérer, spécifiez le type de serveurs à découvrir. Les options sont **contrôleur de domaine**, **serveur DHCP**et **serveur DNS**.

Par défaut, les contrôleurs de domaine, les serveurs DHCP et les serveurs DNS sont découverts. par conséquent, si vous ne souhaitez pas découvrir l’un de ces types de serveurs, veillez à désélectionner la case à cocher de cette option.

Dans l’exemple ci-dessus, le serveur IPAM est installé dans la forêt contoso.com, et le domaine racine de la forêt fabrikam.com est ajouté pour la gestion IPAM. Les rôles de serveur sélectionnés permettent à IPAM de détecter et de gérer les contrôleurs de domaine, les serveurs DHCP et les serveurs DNS dans le domaine racine fabrikam.com et le domaine racine contoso.com.

Une fois que vous avez spécifié les forêts, les domaines et les rôles de serveur, cliquez sur **OK**. IPAM effectue la détection et, lorsque la découverte est terminée, vous pouvez gérer les ressources à la fois dans la forêt locale et la forêt distante.
