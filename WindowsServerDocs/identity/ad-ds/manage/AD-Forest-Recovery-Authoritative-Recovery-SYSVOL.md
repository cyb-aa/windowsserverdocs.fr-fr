---
title: Récupération de forêt AD - synchronisation faisant autoritée de SYSVOL
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adds
ms.openlocfilehash: 246a2ea589ee05110362cff99d50e93a0a18c2b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827000"
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Récupération de forêt AD - effectuer une synchronisation faisant autoritée de SYSVOL de réplication DFSR  

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Il existe différentes façons d’effectuer une restauration faisant autorité de SYSVOL. Vous pouvez modifier le **msDFSR-Options** attribut ou effectuer une restauration d’état système à l’aide de wbadmin – authsysvol. Si vous avez la possibilité de restaurer une sauvegarde d’état système (autrement dit, vous restaurez les services AD DS à la même instance de matériel et de système d’exploitation) à l’aide de wbadmin – authsysvol est plus simple. Mais si vous avez besoin effectuer une restauration à froid, vous devez modifier le **msDFSR-Options** attribut.  

Procédez comme suit pour effectuer une synchronisation faisant autoritée de SYSVOL (si elle est répliquée à l’aide de DFSR) en modifiant le **msDFSR-Options** attribut. Si SYSVOL est répliqué à l’aide de FRS, consultez [article 290762](https://go.microsoft.com/fwlink/?LinkId=148443).  

## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Pour effectuer une synchronisation faisant autoritée de SYSVOL de réplication DFSR  

1. Ouvrez le composant Utilisateurs et ordinateurs Active Directory.  
2. Cliquez sur **vue**, puis sélectionnez **utilisateurs, Contacts, groupes et ordinateurs en tant que conteneurs** et **fonctionnalités avancées**. 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 

3. Dans l’arborescence, cliquez sur **contrôleurs de domaine**, le nom du contrôleur de domaine que vous avez restauré, **DFSR-LocalSettings**, puis **Domain System Volume**. 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  

4. Dans le volet détails, cliquez sur **SYSVOL abonnement**, cliquez sur **propriétés**, puis cliquez sur **Éditeur d’attributs**.  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 

5. Cliquez sur **msDFSR-Options**, cliquez sur **modifier**, type **1**, puis cliquez sur **OK**  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 

6. Cliquez sur **OK** pour fermer l’éditeur d’attributs.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)
