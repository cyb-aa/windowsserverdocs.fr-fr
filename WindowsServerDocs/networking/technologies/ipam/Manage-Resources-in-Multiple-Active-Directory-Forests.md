---
title: Gérer des ressources dans plusieurs forêts Active Directory
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2bbd303df635af314cee2126a75f0569ede2f5de
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282190"
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>Gérer des ressources dans plusieurs forêts Active Directory

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à utiliser IPAM pour gérer les contrôleurs de domaine, les serveurs DHCP et les serveurs DNS dans plusieurs forêts Active Directory.  
  
Pour utiliser IPAM pour gérer les ressources dans des forêts Active Directory à distance, chaque forêt que vous souhaitez gérer doit avoir un deux d’approbation bidirectionnelle avec la forêt où IPAM est installé.  
  
Pour démarrer le processus de découverte de forêts Active Directory différentes, ouvrez le Gestionnaire de serveur, puis cliquez sur IPAM. Dans la console client IPAM, cliquez sur **configurer la découverte de serveur**, puis cliquez sur **obtenir forêts**. Cela lance une tâche en arrière-plan qui détecte les forêts approuvées et leurs domaines. Une fois le processus de découverte est terminée, cliquez sur **configurer la découverte de serveur**, qui ouvre la boîte de dialogue suivante.  
  
![Configurer la découverte de serveurs](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>Stratégie de groupe\-en fonction de l’approvisionnement pour un scénario entre forêts Active Directory, vérifiez que vous exécutez l’applet de commande Windows PowerShell suivante sur le serveur IPAM et non sur les contrôleurs de domaine du domaine d’approbation. Par exemple, si votre serveur IPAM est joint au corp.contoso.com de la forêt et la forêt d’approbation est fabrikam.com, vous pouvez exécuter l’applet de commande Windows PowerShell suivante sur le serveur IPAM dans corp.contoso.com pour la stratégie de groupe\-de configuration sur le forêt de Fabrikam.com. Pour exécuter cette applet de commande, vous devez être membre du groupe Admins du domaine dans la forêt fabrikam.com.

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

Dans le **configurer la découverte de serveur** boîte de dialogue, cliquez sur **sélectionnez la forêt**, puis choisissez la forêt que vous souhaitez gérer avec IPAM. Sélectionnez également les domaines que vous souhaitez gérer, puis cliquez sur **ajouter**.

Dans **sélectionner les rôles de serveur à découvrir**, pour chaque domaine que vous souhaitez gérer, spécifiez le type de serveurs à découvrir. Les options sont **contrôleur de domaine**, **serveur DHCP**, et **serveur DNS**.

Par défaut, les contrôleurs de domaine, les serveurs DHCP et les serveurs DNS sont découverts : par conséquent, si vous ne souhaitez pas détecter un de ces types de serveurs, assurez-vous que vous désactivez la case à cocher correspondant à cette option.

Dans l’illustration de l’exemple ci-dessus, le serveur IPAM est installé dans la forêt contoso.com, et le domaine racine de la forêt fabrikam.com est ajouté pour la gestion d’IPAM. Les rôles de serveur sélectionné qu’IPAM puisse découvrir et gérer les contrôleurs de domaine, les serveurs DHCP et les serveurs DNS dans le domaine racine de fabrikam.com et le domaine racine de contoso.com.

Une fois que vous avez spécifié des forêts, domaines et les rôles de serveur, cliquez sur **OK**. IPAM effectue la découverte, et lors de la détection est terminée, vous pouvez gérer les ressources de la forêt locale et distante.
