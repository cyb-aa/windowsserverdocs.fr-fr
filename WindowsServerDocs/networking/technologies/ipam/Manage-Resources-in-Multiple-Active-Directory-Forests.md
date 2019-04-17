---
title: Gérer les ressources dans plusieurs forêts ActiveDirectory
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b01680d4b35461ff85965781ebc60e7a613d1cb8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>Gérer les ressources dans plusieurs forêts ActiveDirectory

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment utiliser IPAM pour gérer les contrôleurs de domaine, les serveurs DHCP et les serveurs DNS dans plusieurs forêts ActiveDirectory.  
  
Pour utiliser IPAM pour gérer les ressources dans des forêts ActiveDirectory à distance, chaque forêt que vous souhaitez gérer doit avoir un deux façon approbation avec la forêt où IPAM est installé.  
  
Pour démarrer le processus de découverte pour différentes forêts ActiveDirectory, ouvrez le Gestionnaire de serveur et cliquez sur IPAM. Dans la console client IPAM, cliquez sur **configurer la découverte de serveur**, puis cliquez sur **obtenir forêts**. Cette commande lance une tâche en arrière-plan qui détecte les forêts approuvées et leurs domaines. Une fois le processus de découverte est terminée, cliquez sur **configurer la découverte de serveur**, qui ouvre la boîte de dialogue.  
  
![Configurer la découverte de serveur](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>Pour la configuration basée sur le compte de groupe pour un scénario entre forêts ActiveDirectory, assurez-vous que vous exécutez l’applet de commande Windows PowerShell suivante sur le serveur IPAM et non sur les contrôleurs de domaine du domaine d’approbation. Par exemple, si votre serveur IPAM est associé à la forêt corp.contoso.com et de la forêt d’approbation est fabrikam.com, vous pouvez exécuter l’applet de commande Windows PowerShell suivante sur le serveur IPAM dans corp.contoso.com pour la configuration basée sur le compte de groupe sur la forêt fabrikam.com. Pour exécuter cette applet de commande, vous devez être membre du groupe Admins du domaine dans la forêt fabrikam.com.

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

Dans le **configurer la découverte de serveur** boîte de dialogue, cliquez sur **sélectionnez la forêt**, puis choisissez la forêt que vous souhaitez gérer avec IPAM. Sélectionnez également les domaines que vous souhaitez gérer, puis cliquez sur **ajouter**.

Dans **sélectionner les rôles de serveur pour découvrir**, pour chaque domaine que vous souhaitez gérer, spécifiez le type de serveurs à découvrir. Les options sont **contrôleur de domaine**, **serveur DHCP**, et **serveur DNS**.

Par défaut, les contrôleurs de domaine, les serveurs DHCP et les serveurs DNS sont découverts: Si vous ne souhaitez pas détecter un de ces types de serveurs, assurez-vous que vous désactivez la case à cocher pour cette option.

Dans l’illustration exemple ci-dessus, le serveur IPAM est installé dans la forêt contoso.com, et le domaine racine de la forêt fabrikam.com est ajouté pour la gestion IPAM. Les rôles de serveur sélectionné permettent à IPAM de détecter et gérer les contrôleurs de domaine, les serveurs DHCP et les serveurs DNS dans le domaine racine de fabrikam.com et le domaine racine de contoso.com.

Après avoir spécifié les forêts, domaines et les rôles de serveur, cliquez sur **OK**. IPAM effectue la découverte, et lors de la détection est terminée, vous pouvez gérer les ressources dans la forêt locale et distante.
