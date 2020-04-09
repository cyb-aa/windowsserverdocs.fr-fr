---
title: Récupération de forêt Active Directory faisant autorité pour la synchronisation de SYSVOL
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adds
ms.openlocfilehash: 4c797c98fc8d41621954077ebf470f1f52604cf7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824302"
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Récupération de la forêt Active Directory-exécution d’une synchronisation faisant autorité d’un SYSVOL répliqué par DFSR  

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Il existe différentes façons d’effectuer une restauration faisant autorité de SYSVOL. Vous pouvez modifier l’attribut **msDFSR-options** ou effectuer une restauration de l’état du système à l’aide de Wbadmin – authsysvol. Si vous avez la possibilité de restaurer une sauvegarde de l’état du système (c’est-à-dire que vous restaurez AD DS sur le même matériel et la même instance du système d’exploitation), l’utilisation de Wbadmin – authsysvol est plus simple. Toutefois, si vous devez effectuer une restauration complète, vous devez modifier l’attribut **msDFSR-options** .  

Procédez comme suit pour effectuer une synchronisation de SYSVOL faisant autorité (si elle est répliquée à l’aide de DFSR) en modifiant l’attribut **msDFSR-options** . Si SYSVOL est répliqué à l’aide de FRS, consultez l' [article 290762](https://go.microsoft.com/fwlink/?LinkId=148443).  

## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Pour effectuer une synchronisation faisant autorité d’un SYSVOL répliqué par DFSR  

1. Ouvrez Utilisateurs et ordinateurs Active Directory.  
2. Cliquez sur **affichage**, puis sélectionnez **utilisateurs, contacts, groupes et ordinateurs en tant que conteneurs** et **fonctionnalités avancées**. 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 

3. Dans l’arborescence, cliquez sur **contrôleurs de domaine**, sur le nom du contrôleur de domaine que vous avez restauré, sur **DFSR-LocalSettings**, puis sur **volume système de domaine**. 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  

4. Dans le volet d’informations, cliquez avec le bouton droit sur **abonnement SYSVOL**, cliquez sur **Propriétés**, puis sur **éditeur d’attributs**.  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 

5. Cliquez sur **msDFSR-options**, sur **modifier**, tapez **1**, puis cliquez sur **OK** .  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 

6. Cliquez sur **OK** pour fermer l’éditeur d’attributs.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
