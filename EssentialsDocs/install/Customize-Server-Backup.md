---
title: Personnaliser la sauvegarde du serveur
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19b2559c-6090-45af-9a08-2eefc28473c8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d18dca276bccdf672664a5a3c2bd28e0221fff94
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838540"
---
# <a name="customize-server-backup"></a>Personnaliser la sauvegarde du serveur

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="turn-off-server-backup-by-default"></a>Désactiver la sauvegarde du serveur par défaut.  
 Vous pouvez choisir de désactiver la sauvegarde du serveur par défaut. Vous devez définir la valeur de **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** sur 1 pour activer cette option.  
  
 Lorsque cette clé est définie, l’interface utilisateur de la sauvegarde du serveur ne sera pas exposée via le Tableau de bord ou le Launchpad. Cela vous permet d'utiliser des applications tierces pour la sauvegarde du serveur.  
  
#### <a name="to-add-serverbackupproviderdisabled-registry-key-and-set-the-value-to-1"></a>Pour ajouter ServerBackup\ProviderDisabled ? clé de Registre et définissez la valeur sur 1  
  
1.  Sur le serveur, cliquez successivement sur **Démarrer**et **Exécuter**, tapez **regedit** dans le champ **Ouvrir** , puis cliquez sur **OK**.  
  
2.  Dans le volet de navigation, développez successivement **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, **Windows Server**, et enfin **ServerBackup**.  
  
3.  Cliquez avec le bouton droit sur **ServerBackup**, cliquez sur **Nouveau**, puis cliquez sur **Valeur DWARD**.  
  
4.  Comme nom, entrez **ProviderDisabled**.  
  
5.  Cliquez avec le bouton droit sur le nom, sélectionnez **Modifier**, entrez **1** comme valeur, puis cliquez sur **OK**.  
  
## <a name="turn-on-server-backup"></a>Activer la sauvegarde du serveur  
 Vous pouvez activer la sauvegarde du serveur si elle était désactivée en créant la clé de registre **ProviderDisabled** (comme décrit auparavant dans le présent document).  
  
 Vous devez supprimer la clé **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** afin d’activer la sauvegarde du serveur par défaut, de modifier le type de démarrage du Service de sauvegarde de serveurs Windows Server et de redémarrer le serveur.  
  
#### <a name="to-delete-serverbackupproviderdisabled-registry-key"></a>Pour supprimer ServerBackup\ProviderDisabled ? clé de Registre  
  
1.  Sur le serveur, déplacez votre souris sur le coin supérieur droit de l’écran, puis cliquez sur **Rechercher**.  
  
2.  Dans la zone de recherche, tapez **regedit**, puis cliquez sur l’application **Regedit** .  
  
3.  Dans le volet de navigation, développez successivement **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, **Windows Server**, et enfin **ServerBackup**.  
  
4.  Cliquez avec le bouton droit de la souris sur **ProviderDisabled**, puis cliquez sur **Supprimer**.  
  
#### <a name="change-the-start-type-of-windows-server-server-backup-service"></a>Modifier le type de démarrage du Service de sauvegarde de serveurs Windows Server  
  
1.  Sur le serveur, déplacez votre souris sur le coin supérieur droit de l’écran, puis cliquez sur **Rechercher**.  
  
2.  Dans la boîte de dialogue Rechercher, entrez **services.msc**, puis cliquez sur l'application **Services** application.  
  
3.  Dans le volet Services, cliquez avec le bouton droit sur **Service de sauvegarde de serveurs Windows Server**, puis cliquez sur **Propriétés**.  
  
4.  Dans l'onglet **Général**, sélectionnez **Automatique** comme **Type de démarrage**.  
  
5.  Cliquez sur **OK** pour fermer la boîte de dialogue.  
  
#### <a name="restart-the-server"></a>Redémarrer le serveur  
  
1.  Sur le serveur, déplacez votre souris sur le coin supérieur droit de l’écran, cliquez sur **Paramètres**, sur **Alimentation**, puis sur Redémarrer.