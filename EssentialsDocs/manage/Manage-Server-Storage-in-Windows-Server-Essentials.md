---
title: "Gérer le stockage de serveur dans Windows Server Essentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1836682e-c7bb-4dd5-a2b5-6ff032693574
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e0f65dfd25afbd584764d33904ba82e4da4c5443
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-server-storage-in-windows-server-essentials"></a>Gérer le stockage de serveur dans Windows Server Essentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
   
 Windows Server Essentials vous permet de gérer tout votre stockage serveur (y compris les disques durs et les espaces de stockage) à partir de la **disques durs** pages sur le **stockage** onglet du tableau de bord.  
  
 Les sections suivantes fournissent des informations qui vous aideront à augmenter le stockage serveur, comprendre et utiliser des espaces de stockage et gérer vos disques durs:  
  
-   [Gérer les disques durs à l’aide du tableau de bord](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Augmenter le stockage sur le serveur](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Effectuer les vérifications et réparations sur les disques durs](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_Check)  
  
-   [Disques durs au format](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Ajouter un nouveau disque dur](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Vue d’ensemble des espaces de stockage](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Créer un espace de stockage](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_6)  
  
##  <a name="BKMK_1"></a>Gérer les disques durs à l’aide du tableau de bord  
 Windows Server Essentials vous permet de gérer tous les disques durs connectés au serveur via le tableau de bord. Sur le tableau de bord **stockage** onglet, **disques durs** affiche tous les disques durs qui sont disponibles sur le serveur pour le stockage des sauvegardes de données et le serveur. Le serveur analyse l’espace disponible sur chaque disque dur et affiche une alerte si un espace disque est faible. Le **les disques durs** onglet affiche les informations suivantes:  
  
-   Le nom de chaque disque dur  
  
-   La capacité de chaque disque dur  
  
-   La quantité d’espace utilisé sur chaque disque dur  
  
-   La quantité d’espace libre sur chaque disque dur  
  
-   L’état de chaque disque dur; un état vierge signifie que le disque dur fonctionne correctement  
  
-   Le volet d’informations, qui affiche toutes les informations de pile de stockage (pour le pool de stockage, l’espace de stockage et disque dur) si le disque dur sélectionné se trouve sur un espace de stockage (au lieu d’un disque physique)  
  
 Le tableau suivant répertorie les tâches de gestion de disque dur qui sont disponibles dans le tableau de bord et leurs descriptions. Certaines tâches sont affichés uniquement lorsqu’un disque dur est sélectionné.  
  
### <a name="available-hard-drive-management-tasks"></a>Tâches de gestion de disque dur disponible  
  
|Nom de la tâche|Description|  
|---------------|-----------------|  
|**Afficher les propriétés du disque dur**|Ouvre le *Nomdisquedur***propriétés** page. Cette tâche s’affiche lorsque le disque dur est sélectionné. Le **général** onglet de la *Nomdisquedur* page de propriétés comprend les tâches supplémentaires suivantes:<br /><br /> -   **Nettoyage de disque**: vous permet de nettoyer les fichiers inutilisés du disque dur (cette tâche est uniquement disponible dans Windows Server Essentials).<br />-   **Vérifier et réparer**: vérifie le disque dur pour les erreurs de système de fichiers et tente de réparer automatiquement les erreurs détectées.<br /><br /> Le **clichés instantanés** onglet de la *Nomdisquedur***propriétés** page vous permet d’activer les clichés instantanés. Cet onglet affiche également la prochaine fois que les clichés instantanés est planifiée pour être exécutée.|  
|**Gérer les espaces de stockage**|**Remarque:** pour Windows Server Essentials, cette tâche ne s’affiche lorsqu’il existe un espace de stockage.<br /><br /> Ouvre le **espaces de stockage** le panneau de configuration, à partir de laquelle vous pouvez créer et gérer les pools de stockage et les espaces de stockage.|  
|**Créer un espace de stockage**|Ouvre l’Assistant Création d’un stockage espace, ce qui vous permet d’utiliser un ou plusieurs disques durs pour augmenter la capacité du pool de stockage.|  
|**Augmenter la capacité du pool de stockage**|**Remarque:** cette tâche est visible uniquement si le disque dur sélectionné se trouve sur un espace de stockage.<br /><br /> Ouvre l’augmenter la capacité d’un Assistant de Pool de stockage, ce qui vous permet d’utiliser un ou plusieurs disques durs pour augmenter la capacité du pool de stockage.|  
  
##  <a name="BKMK_2"></a>Augmenter le stockage sur le serveur  
 Pour augmenter le stockage sur le serveur, vous pouvez ajouter un disque dur interne supplémentaire au serveur. Pour ajouter le disque dur interne supplémentaire, vous devez arrêter le serveur, ajouter le disque dur interne et puis redémarrez le serveur. Vous n’avez pas besoin d’arrêter le serveur si le disque dur est attaché au contrôleur SCSI. Dans ce cas, le disque dur peut être branché pendant que le serveur est en cours d’exécution.  
  
 Selon que le disque dur à ajouter est formaté, effectuez l’une des opérations suivantes:  
  
-   **Mise en forme** si le disque dur interne est formaté en NTFS ou ReFS, le serveur lui affecte une lettre de lecteur et le disque dur apparaît sous la **les disques durs** onglet. Vous pouvez maintenant créer ou déplacer des dossiers du serveur vers le nouveau disque dur.  
  
-   **Ne pas au format** si le disque dur interne n’est pas formaté, l’alerte suivante s’affiche: un ou plusieurs disques durs non formatés sont connectés au serveur. Utilisez la procédure suivante pour formater le disque dur.  
  
#### <a name="to-format-the-hard-disk"></a>Pour formater le disque dur  
  
1.  Ouvrez le tableau de bord.  
  
2.  Dans le volet de navigation, cliquez sur l’icône d’alerte pour lancer le **Afficheur des alertes** dans Windows Server Essentials, ou le **contrôle d’intégrité** onglet dans Windows Server Essentials.  
  
3.  Dans le **Afficheur des alertes** ou **contrôle d’intégrité** onglet et cliquez sur l’alerte, puis dans le volet tâches, cliquez sur **résoudre ce problème**.  
  
4.  Suivez les instructions pour terminer l’Assistant Ajouter un nouveau disque dur lecteur.  
  
###  <a name="BKMK_Clean"></a>Pour nettoyer le disque dur  
  
1.  Ouvrez le tableau de bord.  
  
2.  Dans le volet de navigation, cliquez sur **stockage**, puis cliquez sur **les disques durs**.  
  
3.  Dans le **disques durs** , sélectionnez la lettre de lecteur qui a été attribuée au disque dur nouvellement ajouté et dans le volet des tâches, cliquez sur **afficher les propriétés du disque dur**.  
  
4.  Dans **< Driveletter\ > Propriétés**, dans le **général** , cliquez sur **nettoyage de disque**.  
  
##  <a name="BKMK_Check"></a>Effectuer les vérifications et réparations sur les disques durs  
 Les disques durs vérifier et réparer processus vérifie l’intégrité du système de fichiers stocké sur les disques durs. Il exécute une **chkdsk** processus sur le volume que les fichiers de sauvegarde sont stockés dans. Le problème suivant alerte peut être résolu en une vérification en cours d’exécution et réparer les disques durs:  
  
-   Un ou plusieurs disques durs de sauvegarde du serveur doivent être vérifiés.  
  
#### <a name="to-check-and-repair-hard-drives"></a>Pour vérifier et réparer les disques durs  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur **dossiers du serveur et les disques durs**, puis cliquez sur **les disques durs**.  
  
3.  Sélectionnez le disque dur qui affiche l’erreur, puis **afficher les propriétés du disque dur**.  
  
4.  Sur le **vérifier et réparer** , cliquez sur **vérifier et réparer**.  
  
##  <a name="BKMK_3"></a>Disques durs au format  
 Lorsqu’un disque dur interne non formaté est détecté sur le serveur, une alerte d’intégrité guide l’utilisateur dans le processus de mise en forme. L’ajout d’un nouvel Assistant de disque dur vous guide à travers le formatage du disque dur et vous permet de configurer le disque dur de l’une des manières suivantes:  
  
1.  Formater le disque dur et créer automatiquement un lecteur sur ce dernier. Si vous choisissez cette option, lorsque l’Assistant est terminé, un disque dur logique formaté avec le système de fichiers NTFS est créé.  
  
2.  Formater le disque dur et le configurer pour la sauvegarde du serveur. Si vous choisissez cette option, l’Assistant Configuration du serveur sauvegarde est lancé, et il vous guide dans la configuration de sauvegarde du serveur.  
  
3.  Si le stockage espace ne t existe, utilisez le nouveau disque dur pour créer un espace de stockage. Vous devez disposer d’au moins deux disques durs pour créer un espace de stockage.  
  
4.  Si un espace de stockage existe déjà, utilisez le nouveau disque dur pour augmenter la capacité du pool de stockage. Cette option s’affiche uniquement s’il existe un espace de stockage créé sur le serveur. Si vous choisissez cette option, l’Assistant ajoute ce nouveau disque dur au pool de stockage.  
  
##  <a name="BKMK_4"></a>Ajouter un nouveau disque dur  
 Lorsque vous branchez un lecteur de disque dur sur un serveur exécutant Windows Server Essentials, vous pouvez:  
  
-   [Utiliser le nouveau disque dur pour stocker les dossiers du serveur](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4a)  
  
-   [Utiliser le nouveau disque dur pour stocker des sauvegardes de serveur](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4b)  
  
-   [Utiliser le nouveau disque dur pour augmenter la capacité du pool de stockage](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)  
  
###  <a name="BKMK_4a"></a>Utiliser le nouveau disque dur pour stocker les dossiers du serveur  
 Pour utiliser le nouveau disque dur pour stocker les dossiers du serveur, vous pouvez ajouter un nouveau dossier de serveur sur le disque dur ou déplacer un dossier serveur existant sur le disque dur.  
  
##### <a name="to-store-server-folders"></a>Pour stocker les dossiers du serveur  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur le **stockage** onglet, puis cliquez sur **dossiers du serveur**.  
  
3.  Dans le **tâches de dossiers de serveur** volet, effectuez l’une des opérations suivantes:  
  
    1.  Pour ajouter un dossier de serveur, cliquez sur **ajouter un dossier**.  
  
    2.  Pour déplacer un dossier de serveur, sélectionnez le dossier que vous souhaitez déplacer vers le nouveau disque dur, puis cliquez sur **déplacer un dossier**.  
  
    > [!NOTE]
    >  Si vous naviguez sur le disque dur et que vous sélectionnez en tant que l’emplacement du dossier serveur sans avoir à créer un dossier, le message d’erreur suivant s’affiche: **un répertoire racine (par exemple, C:\\, D:\\) ne peut pas être ajouté en tant qu’un dossier serveur. Créer un nouveau dossier ou sélectionnez-en un existant sous le répertoire racine, puis essayez à nouveau**. Pour résoudre cette erreur, créez un dossier sur le disque dur nouvellement ajouté, puis sélectionnez le nouveau dossier comme emplacement pour stocker les dossiers du serveur.  
  
4.  Suivez les instructions pour terminer l’Assistant.  
  
 Pour plus d’informations sur le déplacement des dossiers du serveur, voir [ajouter ou déplacer un dossier serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
###  <a name="BKMK_4b"></a>Utiliser le nouveau disque dur pour stocker des sauvegardes de serveur  
 Vous pouvez utiliser le disque dur nouvellement ajouté pour stocker des sauvegardes de serveur.  
  
##### <a name="to-store-server-backups"></a>Pour stocker des sauvegardes de serveur  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur le **périphériques** , sélectionnez le serveur à partir du volet de la liste, et puis dans le volet des tâches, effectuez l’une des opérations suivantes:  
  
    1.  Si la sauvegarde du serveur n’est pas configurée sur le serveur, cliquez sur **configurer la sauvegarde du serveur**.  
  
    2.  Si la sauvegarde du serveur est configurée sur le serveur, cliquez sur **personnaliser la sauvegarde du serveur**.  
  
     L’Assistant Configurer la sauvegarde serveur s’affiche.  
  
3.  Dans le **sélectionner la Destination de sauvegarde** , sélectionnez le nouveau disque dur comme destination de sauvegarde.  
  
4.  Suivez les instructions pour terminer l’Assistant.  
  
###  <a name="BKMK_4c"></a>Utiliser le nouveau disque dur pour augmenter la capacité du pool de stockage  
 Lorsque la capacité de votre pool de stockage est faible, vous recevez une alerte indiquant que vous pouvez augmenter la capacité du pool de stockage en ajoutant un nouveau disque dur au pool de stockage à l’aide de l’augmenter la capacité d’un Assistant de Pool de stockage.  
  
> [!NOTE]
>  Vous pouvez effectuer cette procédure uniquement si vous avez créé un pool de stockage sur le serveur.  
  
##### <a name="to-increase-storage-pool-capacity"></a>Pour augmenter la capacité du pool de stockage  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur le **stockage** onglet, puis cliquez sur **les disques durs**.  
  
3.  Sélectionnez le lecteur qui présente une capacité faible.  
  
4.  Dans le volet des tâches, sélectionnez **augmenter la capacité du pool de stockage**. L’augmenter la capacité d’un Assistant de Pool de stockage s’affiche.  
  
5.  Suivez les instructions pour terminer l’Assistant.  
  
##  <a name="BKMK_5"></a>Vue d’ensemble des espaces de stockage  
 Stockage espaces vous permet de regrouper des disques pour constituer un pool de stockage. Vous pouvez ensuite utiliser la capacité du pool pour créer des espaces de stockage. Espaces de stockage sont des disques virtuels qui s’affichent sur la **disques durs** onglet du tableau de bord. Vous pouvez utiliser des espaces de stockage comme tout autre lecteur, par conséquent, il s facile à utiliser vos fichiers. Lorsque vous exécutez faible sur la capacité du pool, vous pouvez créer des espaces de stockage volumineux et ajouter des disques au pool de stockage. Si vous avez deux ou plusieurs disques dans le pool de stockage, vous pouvez créer des espaces de stockage avec un miroir bidirectionnel ne soient pas affecté par une défaillance du lecteur? ou même de deux lecteurs? si vous créez un espace de stockage en miroir triple.  
  
 Pour créer un espace de stockage, vous avez uniquement besoin est un ou plusieurs disques supplémentaires en outre sur le lecteur sur lequel Windows est installé. Ces lecteurs peuvent être des disques durs internes ou externes ou des disques SSD. Vous pouvez utiliser différents types de lecteurs aux espaces de stockage, y compris les lecteurs USB, SATA et SAS.  
  
> [!NOTE]
>  Si vous configurez des espaces de stockage sur un serveur exécutant Windows Server Essentials, vous ne pouvez pas effectuer une réinitialisation d’usine avec le **nettoyer les données** option. La solution de contournement pour résoudre ce problème est tout d’abord supprimer les espaces de stockage, puis effectuer une réinitialisation d’usine avec le **nettoyer les données** option.  
  
 Pour plus d’informations sur les espaces de stockage, voir [stockage espaces Forum aux Questions (FAQ)](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).  
  
##  <a name="BKMK_6"></a>Créer un espace de stockage  
 Pour commencer à travailler avec les espaces de stockage sur le serveur, les exigences minimales suivantes doivent être remplies:  
  
-   Le serveur exécutant Windows Server Essentials doit être attaché aux lecteurs physiques supplémentaires (et pas seulement le lecteur de démarrage), les lecteurs ne doivent héberger aucun volume et doivent avoir une capacité minimale de 10 Go. Un disque physique est nécessaire pour créer un pool de stockage; un minimum de deux disques physiques est nécessaire pour créer un espace de stockage miroir.  
  
-   Un minimum de deux disques physiques sont requis pour créer un espace de stockage avec une résilience par le biais de parité ou la mise en miroir bidirectionnelle.  
  
> [!NOTE]
>  Si vous configurez des espaces de stockage sur un serveur exécutant Windows Server Essentials, vous ne pouvez pas effectuer une réinitialisation d’usine avec le **nettoyer les données** option. La solution de contournement pour résoudre ce problème est tout d’abord supprimer les espaces de stockage, puis effectuer une réinitialisation d’usine avec le **nettoyer les données** option.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Pour créer un espace de stockage dans Windows Server Essentials  
  
1.  Ajoutez ou connectez tous les lecteurs que vous souhaitez regrouper des espaces de stockage sur le serveur exécutant Windows Server Essentials.  
  
2.  Dans le tableau de bord, cliquez sur **avancé: gérer les espaces de stockage**.  
  
3.  Cliquez sur **créer un nouvel espace de stockage et de pool**.  
  
4.  Sélectionnez les lecteurs que vous souhaitez ajouter au nouvel espace de stockage, puis cliquez sur **créer le pool**.  
  
5.  Attribuez le lecteur un nom et une lettre, puis choisissez une disposition. **Miroir**, **trois voies**, et **parité** peut aider à protéger les fichiers de l’espace de stockage à partir de la défaillance du lecteur.  
  
6.  Entrez la taille maximale de l’espace de stockage peut atteindre, puis cliquez sur **créer l’espace de stockage**.  
  
 Dans Windows Server Essentials, vous pouvez créer un espace de stockage en miroir double à l’aide de la création d’un Assistant d’espace de stockage à l’à partir du tableau de bord.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Pour créer un espace de stockage dans Windows Server Essentials  
  
1.  Ajoutez ou connectez tous les lecteurs que vous souhaitez regrouper des espaces de stockage sur le serveur exécutant Windows Server Essentials.  
  
2.  Dans le tableau de bord, cliquez sur **gérer les espaces de stockage**. La création de qu'un Assistant d’espace de stockage s’affiche.  
  
3.  Suivez les instructions pour terminer l’Assistant.  
  
 Pour plus d’informations sur l’augmentation de capacité du pool de stockage, consultez [utiliser le nouveau disque dur pour augmenter la capacité du pool de stockage](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c).  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer les dossiers serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)
