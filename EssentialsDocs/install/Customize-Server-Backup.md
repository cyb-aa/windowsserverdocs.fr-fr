---
title: Personnaliser la sauvegarde du serveur
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="customize-server-backup"></a>Personnaliser la sauvegarde du serveur

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

## <a name="turn-off-server-backup-by-default"></a>Désactiver la sauvegarde du serveur par défaut  
 Vous pouvez choisir de désactiver la sauvegarde du serveur par défaut. Vous devez définir la valeur de **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** sur 1 pour activer cette option.  
  
 Lorsque cette clé est définie, l’interface utilisateur de sauvegarde de serveur ne sera pas exposée par le biais du tableau de bord ou le Launchpad. Cela vous permet d’utiliser des applications tierces pour la sauvegarde du serveur.  
  
#### <a name="to-add-serverbackupproviderdisabled-registry-key-and-set-the-value-to-1"></a>Pour ajouter ServerBackup\ProviderDisabled? clé de Registre et définissez la valeur sur 1  
  
1.  Sur le serveur, cliquez sur **Démarrer**, cliquez sur **exécuter**, type **regedit** dans les **ouvrir** zone de texte, puis cliquez sur **OK**.  
  
2.  Dans le volet de navigation, développez **HKEY_LOCAL_MACHINE**, développez **logiciel**, développez **Microsoft**, développez **Windows Server**, puis développez **ServerBackup**.  
  
3.  Avec le bouton droit **ServerBackup**, cliquez sur **New**, puis cliquez sur **la valeur DWORD**.  
  
4.  Pour nom, entrez **ProviderDisabled**.  
  
5.  Cliquez sur le nom, sélectionnez **modifier**, entrez **1** pour les données de la valeur, puis cliquez sur **OK**.  
  
## <a name="turn-on-server-backup"></a>Activer la sauvegarde du serveur  
 Vous pouvez activer la sauvegarde du serveur s’il a été désactivé en créant **ProviderDisabled** clé de Registre (comme décrit précédemment dans ce document).  
  
 Vous devez supprimer la clé **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** afin d’activer la sauvegarde du serveur par défaut, modifier le type de démarrage de service du Service de sauvegarde de serveurs Windows Server et redémarrez le serveur.  
  
#### <a name="to-delete-serverbackupproviderdisabled-registry-key"></a>Pour supprimer ServerBackup\ProviderDisabled? clé de Registre  
  
1.  Sur le serveur, déplacez votre souris vers le coin supérieur droit de l’écran, puis cliquez sur **recherche**.  
  
2.  Dans la zone de recherche, tapez **regedit**, puis cliquez sur le **Regedit** application.  
  
3.  Dans le volet de navigation, développez **HKEY_LOCAL_MACHINE**, développez **logiciel**, développez **Microsoft**, développez **Windows Server**, puis développez **ServerBackup**.  
  
4.  Avec le bouton droit **ProviderDisabled**, puis cliquez sur **supprimer**.  
  
#### <a name="change-the-start-type-of-windows-server-server-backup-service"></a>Modifier le type de démarrage du Service de sauvegarde du serveur Windows Server  
  
1.  Sur le serveur, déplacez votre souris vers le coin supérieur droit de l’écran, puis cliquez sur **recherche**.  
  
2.  Dans la zone de recherche, tapez **services.msc**, puis cliquez sur **Services** application.  
  
3.  Dans le volet services, cliquez sur le **Service de sauvegarde du serveur Windows Server**, puis cliquez sur **propriétés**.  
  
4.  Dans **général** onglet, sélectionnez **automatique** pour **type de démarrage**.  
  
5.  Cliquez sur **OK** pour fermer la boîte de dialogue.  
  
#### <a name="restart-the-server"></a>Redémarrez le serveur  
  
1.  Sur le serveur, déplacez votre souris vers le coin supérieur droit de l’écran, cliquez sur **paramètres**, cliquez sur **alimentation**, puis cliquez sur Redémarrer.