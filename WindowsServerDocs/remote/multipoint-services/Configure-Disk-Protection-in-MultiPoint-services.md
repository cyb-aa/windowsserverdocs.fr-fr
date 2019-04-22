---
title: Configurer la Protection des disques dans MultiPoint Services
description: Découvrez comment configurer la protection de disque pour MultiPoint Services
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
ms.openlocfilehash: dc0a46b57753f08cc7d79fd05de7a9e81469cc86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820170"
---
# <a name="configure-disk-protection"></a>Configurer la Protection de disque
Vous pouvez utiliser la Protection des disques dans Multipoint Services pour protéger votre volume système à partir de mises à jour inattendues, pour planifier les mises à jour de Windows doivent être conservées pendant que la Protection des disques est active, pour désactiver temporairement la Protection de disque et désinstaller la Protection des disques.  
  
En activant la Protection des disques dans MultiPoint Services, vous pouvez protéger le volume système (le lecteur où Windows est installé, généralement c :)) des modifications indésirables. Lorsque la Protection des disques est activée, les modifications apportées au volume système sont stockées dans un emplacement temporaire afin que simplement le redémarrage de l’ordinateur les supprime et retourne automatiquement le système à l’état correct connu précédent.  
  
L’administrateur peut facilement installer des logiciels ou apporter des modifications de configuration en désactivant temporairement la protection des disques. Afin de conserver le système actuel avec les mises à jour Windows et les définitions de logiciels anti-programme malveillant, la Protection des disques planifie une fenêtre de maintenance pour télécharger et installer les mises à jour. L’administrateur peut également fournir un script personnalisé à exécuter pendant la fenêtre de maintenance pour prendre en compte les besoins de maintenance au-delà de mise à jour de Windows.  
  
## <a name="enable-disk-protection"></a>Activer la Protection de disque  
Avant d’activer la Protection des disques, vérifiez que toutes les applications et pilotes sont installés et à jour et déplacement vos profils utilisateur sur un volume qui ne sera pas protégé. Si vous avez besoin effectuer des mises à jour manuelles après avoir activé la Protection des disques, vous pouvez désactiver temporairement la Protection des disques. Toutefois, il est plus facile à exploiter le système dans un état idéal avant la Protection des disques est activée.  
  
 
1.  Ouvrez une session le serveur qui exécute MultiPoint Services en tant qu’administrateur.  
  
2.  Avant d’activer la Protection des disques :  
  
    -   Vérifiez que le système est exactement dans l’état dans lequel vous souhaitez qu’il reste MultiPoint Services. Par exemple, assurez-vous que les logiciels installés, les paramètres système et les mises à jour sont correctes.  
  
    -   Déplacer vers un volume qui n’est pas protégé ou configurer un emplacement de fichier partagé hors du volume système comme décrit dans les profils utilisateur [Activer partage de fichiers dans MultiPoint Services](Enable-file-sharing-in-MultiPoint-services.md).  
  
3.  À partir de la **Démarrer** écran, ouvrez **Gestionnaire MultiPoint**.  
  
4.  Cliquez sur le **accueil** , cliquez sur **activer la protection de disque**, puis cliquez sur **OK**.  
  
Lorsque la Protection de disque est activée pour la première fois, le système est préparé en installant un pilote et de création d’un fichier de cache sur le volume système. Le fichier cache stocke temporairement toutes les modifications apportées au volume système pendant que la Protection des disques est active. Étant donné que les mises à jour système sont stockés dans le fichier cache, elles ne modifient pas le contenu protégé du volume en dehors du fichier cache. Chaque fois que le système démarre, le fichier cache est réinitialisé à qui ignore toutes les modifications enregistrées depuis le démarrage du système précédent. Par conséquent, le système démarre toujours dans le même état que lorsque la Protection des disques a été activée.  
  
Windows doit mettre à jour quelques fichiers système – y compris le fichier d’échange système, emplacement de vidage sur incident et les journaux des événements. Ces fichiers ne sont pas ignorés lors de la Protection des disques est activée. Pour ce faire, un nouveau volume nommé DpReserved est créé lorsque la Protection des disques est activée pour la première fois, et ces fichiers sont déplacés vers ce volume. La partition DpReserved n’est pas protégée, les écritures à ces fichiers persistent redémarre, même lorsque la Protection des disques est activée.  
  
## <a name="schedule-software-updates"></a>Planifier les mises à jour logicielles  
Si Windows est configuré pour installer automatiquement les mises à jour de Windows, la Protection des disques permet à ces mises à jour sur la durée configurée et n’ignore pas les mises à jour. Par exemple, si les mises à jour Windows sont planifiées pour 3 h 00, la Protection des disques vérifie les mises à jour chaque jour à 3 h 00. Si les mises à jour sont trouvées, MultiPoint Services désactive temporairement la Protection des disques, applique les mises à jour et active de nouveau la Protection des disques.  
   
1.  Dans le gestionnaire MultiPoint, afficher le **accueil** onglet, puis cliquez sur **planifier les mises à jour logicielles**.  
  
2.  Dans la boîte de dialogue mises à jour logicielles de planification, cliquez sur **mise à jour à**et sélectionnez une heure pour les mises à jour - par exemple, **3 h 00**.  
  
3.  Sélectionnez le **exécuter la mise à jour Windows** case à cocher.  
  
4.  Si votre organisation exécute son propre script de mise à jour, sélectionnez le **exécuter le programme suivant** case à cocher et spécifiez l’emplacement du script de mise à jour de votre organisation.  
  
5.  Sélectionnez une durée maximale autorisée exécuter les mises à jour.  
  
6.  Sous **issue**, choisissez s’il faut que le système de revenir à son état d’alimentation ou arrêter après avoir appliqué les mises à jour.  
  
7.  Cliquez sur **OK**.  
  
## <a name="temporarily-disable-disk-protection"></a>Désactiver temporairement la Protection des disques  
Si un administrateur doit installer le logiciel, modifier les paramètres système ou effectuer d’autres tâches de maintenance qui impliquent des mises à jour système, ils peuvent désactiver temporairement la Protection des disques. Une fois que les modifications sont apportées, réactivez la Protection de disque. Lors d’un redémarrage du système, le système conserve son état lorsque la Protection de disque a été activée.  
    
1.  Dans le gestionnaire MultiPoint, cliquez sur le **accueil** onglet.  
  
2.  Sous l’onglet Accueil, cliquez sur **désactiver la protection de disque**, puis cliquez sur **OK**.  
  
> [!NOTE]  
> N’oubliez pas de réactiver la Protection de disque une fois la maintenance terminée. Le système ne sera pas protégé à nouveau jusqu'à ce que l’administrateur ré-Active explicitement la Protection des disques.  
  
## <a name="uninstall-disk-protection"></a>Désinstaller la Protection des disques  
Protection des disques en désinstallant le pilote et le fichier cache, donc vous devez uniquement le faire si vous souhaitez cesser d’utiliser la Protection des disques à long terme. Si vous souhaitez simplement effectuer une maintenance ou d’arrêter temporairement la protection, utilisez la tâche de protection de disque désactiver à la place.  
  
Vous pouvez désinstaller la Protection des disques s’il est activé ou désactivé.  
   
1.  Dans le gestionnaire MultiPoint, cliquez sur le **accueil** onglet.  
  
2.  Dans l’onglet Accueil, cliquez sur **désinstaller la protection des disques**, puis cliquez sur **OK**.  
  
    Après avoir cliqué sur **OK**, l’ordinateur redémarre. Le processus de désinstallation requiert plusieurs redémarrages, pendant laquelle le pilote et le fichier de cache sont supprimés. Le reste de la partition DpReserved et le fichier d’échange, emplacement de vidage sur incident et journal des événements fichiers restent configurés pour utiliser la partition DpReserved.