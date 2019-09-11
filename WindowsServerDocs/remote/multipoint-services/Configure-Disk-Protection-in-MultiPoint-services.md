---
title: Configurer la Protection des disques dans MultiPoint Services
description: Découvrez comment configurer la protection des disques pour MultiPoint services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bd9bf5b9-e481-499b-9c15-7ee5a4f470c4
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 867848b65b02b6a7436fc5c86ba796a1b42aec42
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871737"
---
# <a name="configure-disk-protection"></a>Configurer la protection des disques
Vous pouvez utiliser la protection des disques dans multipoint services pour protéger votre volume système contre les mises à jour involontaires, pour planifier la conservation des mises à jour Windows lorsque la protection des disques est active, pour désactiver temporairement la protection des disques et désinstaller la protection des disques.  
  
En activant la protection des disques dans MultiPoint services, vous pouvez protéger le volume système (le lecteur sur lequel Windows est installé, généralement C :) contre les modifications indésirables. Lorsque la protection des disques est activée, les modifications apportées au volume système sont stockées dans un emplacement temporaire afin que le simple redémarrage de l’ordinateur les ignore et retourne automatiquement le système au précédent état connu.  
  
L’administrateur peut facilement installer des logiciels ou apporter des modifications à la configuration en désactivant temporairement la protection des disques. Pour que le système reste à jour avec les définitions de logiciels anti-programmes malveillants et des mises à jour Windows, la protection des disques planifie une fenêtre de maintenance pour télécharger et installer les mises à jour. L’administrateur peut également fournir un script personnalisé à exécuter pendant la fenêtre de maintenance pour répondre à tous les besoins de maintenance au-delà de Windows Update.  
  
## <a name="enable-disk-protection"></a>Activer la protection des disques  
Avant d’activer la protection des disques, assurez-vous que toutes les applications et tous les pilotes sont installés et à jour, et déplacez vos profils utilisateur vers un volume qui ne sera pas protégé. Si vous devez effectuer des mises à jour manuelles après avoir activé la protection des disques, vous pouvez désactiver temporairement la protection des disques. Toutefois, il est plus facile de faire passer le système dans un état idéal avant l’activation de la protection des disques.  
  
 
1.  Connectez-vous au serveur exécutant MultiPoint services en tant qu’administrateur.  
  
2.  Avant d’activer la protection des disques :  
  
    -   Assurez-vous que le système MultiPoint services est exactement dans l’État dans lequel vous souhaitez qu’il reste. Par exemple, assurez-vous que les logiciels installés, les paramètres système et les mises à jour sont corrects.  
  
    -   Déplacez les profils utilisateur vers un volume qui n’est pas protégé ou configurez un emplacement de fichier partagé à partir du volume système, comme décrit dans [activer le partage de fichiers dans multipoint services](Enable-file-sharing-in-MultiPoint-services.md).  
  
3.  À partir de l’écran d' **Accueil** , ouvrez le **Gestionnaire multipoint**.  
  
4.  Cliquez sur l’onglet dossier de **démarrage** , cliquez sur **activer la protection des disques**, puis cliquez sur **OK**.  
  
Lorsque la protection des disques est activée pour la première fois, le système est préparé en installant un pilote et en créant un fichier cache sur le volume système. Le fichier cache stocke temporairement toute modification apportée au volume système alors que la protection des disques est active. Étant donné que les mises à jour système sont stockées dans le fichier cache, elles ne modifient pas le contenu protégé du volume en dehors du fichier cache. À chaque démarrage du système, le fichier cache est réinitialisé, ce qui a pour effet de supprimer toutes les modifications qui y sont stockées depuis le démarrage du système précédent. Ainsi, le système démarre toujours dans le même État que lors de l’activation de la protection des disques.  
  
Windows doit mettre à jour quelques fichiers système, y compris le fichier d’échange système, l’emplacement de vidage sur incident et les journaux des événements. Ces fichiers ne sont pas supprimés lorsque la protection des disques est activée. Pour ce faire, un nouveau volume nommé DpReserved est créé lorsque la protection de disque est activée pour la première fois, et ces fichiers sont déplacés vers ce volume. La partition DpReserved n’étant pas protégée, les écritures dans ces fichiers sont conservées par le biais de redémarrages, même lorsque la protection des disques est activée.  
  
## <a name="schedule-software-updates"></a>Planifier les mises à jour logicielles  
Si Windows est configuré pour installer automatiquement les mises à jour Windows, la protection des disques autorise ces mises à jour à l’heure configurée et n’ignore pas les mises à jour. Par exemple, si les mises à jour Windows sont planifiées pour 3:00 a.m., la protection des disques vérifie les mises à jour chaque jour à 3:00 h 00. Si des mises à jour sont trouvées, MultiPoint services désactive temporairement la protection des disques, applique les mises à jour, puis réactive la protection des disques.  
   
1.  Dans le gestionnaire MultiPoint, affichez l’onglet dossier de **démarrage** , puis cliquez sur **planifier les mises à jour logicielles**.  
  
2.  Dans la boîte de dialogue planifier les mises à jour logicielles, cliquez sur **mettre à jour**dans, puis sélectionnez une heure pour les mises à jour. par exemple, **3:00 AM**.  
  
3.  Activez la case à cocher **exécuter Windows Update** .  
  
4.  Si votre organisation exécute son propre script de mise à jour, activez la case à cocher **exécuter le programme suivant** et spécifiez l’emplacement du script de mise à jour de votre organisation.  
  
5.  Sélectionnez une durée maximale pour permettre l’exécution des mises à jour.  
  
6.  Sous **lorsque**vous avez terminé, indiquez si le système doit revenir à son état d’alimentation précédent ou s’arrêter après l’application des mises à jour.  
  
7.  Cliquez sur **OK**.  
  
## <a name="temporarily-disable-disk-protection"></a>Désactiver temporairement la protection des disques  
Si un administrateur doit installer des logiciels, modifier les paramètres système ou effectuer d’autres tâches de maintenance qui impliquent des mises à jour système, ils peuvent désactiver temporairement la protection des disques. Une fois les modifications effectuées, réactivez la protection des disques. Lors du redémarrage du système, le système conserve son état lors de l’activation de la protection des disques.  
    
1.  Dans le gestionnaire MultiPoint, cliquez sur l’onglet dossier de **démarrage** .  
  
2.  Dans l’onglet dossier de démarrage, cliquez sur **désactiver la protection des disques**, puis cliquez sur **OK**.  
  
> [!NOTE]  
> N’oubliez pas de réactiver la protection des disques une fois la maintenance terminée. Le système ne sera pas protégé à nouveau tant que l’administrateur n’aura pas réactivé explicitement la protection des disques.  
  
## <a name="uninstall-disk-protection"></a>Désinstaller la protection des disques  
La désinstallation de la protection des disques supprime le pilote et le fichier cache. vous ne devez donc le faire que si vous souhaitez cesser d’utiliser la protection des disques à long terme. Si vous souhaitez simplement effectuer une maintenance ou arrêter temporairement la protection, utilisez plutôt la tâche désactiver la protection de disque.  
  
Vous pouvez désinstaller la protection sur disque, qu’elle soit activée ou désactivée.  
   
1.  Dans le gestionnaire MultiPoint, cliquez sur l’onglet dossier de **démarrage** .  
  
2.  Dans l’onglet dossier de démarrage, cliquez sur **désinstaller la protection des disques**, puis cliquez sur **OK**.  
  
    Une fois que vous avez cliqué sur **OK**, l’ordinateur redémarre. Le processus de désinstallation nécessite plusieurs redémarrages, au cours desquels le pilote et le fichier cache sont supprimés. La partition DpReserved est conservée, et le fichier d’échange, l’emplacement de vidage sur incident et les fichiers journaux des événements restent configurés pour utiliser la partition DpReserved.