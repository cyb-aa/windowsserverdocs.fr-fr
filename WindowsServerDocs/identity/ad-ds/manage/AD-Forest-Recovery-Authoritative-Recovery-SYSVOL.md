---
title: "Récupération de la forêt ActiveDirectory - synchronisation faisant autoritée de SYSVOL"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adfs
ms.openlocfilehash: 5bdc619ddf9f28fb074e90dfbf22a7be076a05d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Récupération de la forêt ActiveDirectory - effectuer une synchronisation faisant autoritée de SYSVOL répliqué DFSR  

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

 Il existe différentes façons d’effectuer une restauration faisant autoritée de SYSVOL. Vous pouvez modifier le **msDFSR-Options** d’attribut ou d’effectuer une restauration d’état du système à l’aide de wbadmin – authsysvol. Si vous avez la possibilité de restaurer une sauvegarde de l’état système (autrement dit, vous restaurez les services ADDS à la même instance de matériels et du système d’exploitation) à l’aide de wbadmin – authsysvol est plus simple. Mais si vous devez effectuer une restauration à froid, vous devez modifier le **msDFSR-Options** attribut.  
  
 Utilisez les étapes suivantes pour effectuer une synchronisation faisant autoritée de SYSVOL (si elle est répliquée à l’aide de DFSR) en modifiant le **msDFSR-Options** attribut. Si SYSVOL est répliqué à l’aide de FRS, voir [article 290762](https://go.microsoft.com/fwlink/?LinkId=148443).  
  
## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Pour effectuer une synchronisation faisant autoritée de SYSVOL répliqué DFSR  
  
1.  Ouvrez Utilisateurs et ordinateurs ActiveDirectory.  
2.  Cliquez sur **affichage**, puis sélectionnez **les utilisateurs, Contacts, groupes et ordinateurs en tant que conteneurs** et **fonctionnalités avancées**. 
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 
3.  Dans l’arborescence, cliquez sur **contrôleurs de domaine**, le nom du contrôleur de domaine que vous avez restauré, **DFSR-LocalSettings**, puis **Domain System Volume**. 
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  
4.  Dans le volet d’informations, cliquez sur **abonnement à SYSVOL**, cliquez sur **propriétés**, puis cliquez sur **Éditeur d’attribut**.  
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 
5.  Cliquez sur **msDFSR-Options**, cliquez sur **modifier**, type **1**, puis cliquez sur **OK**  
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 
6.  Cliquez sur **OK** pour fermer l’éditeur d’attribut.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
