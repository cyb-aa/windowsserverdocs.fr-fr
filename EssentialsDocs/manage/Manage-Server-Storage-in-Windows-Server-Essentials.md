---
title: Gérer le stockage serveur dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
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
ms.openlocfilehash: 1637dc6844a0152249fc406a66ef583b9e3a5242
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914653"
---
# <a name="manage-server-storage-in-windows-server-essentials"></a>Gérer le stockage serveur dans Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
   
 Windows Server Essentials vous permet de gérer facilement l’accès au stockage serveur (y compris les disques durs et les espaces de stockage) depuis la page **Disques durs** de l’onglet **Stockage** du tableau de bord .  
  
 Les rubriques suivantes fournissent des informations qui vous aideront à augmenter le stockage serveur, à comprendre et à utiliser les espaces de stockage et à gérer vos disques durs :  
  
-   [Gérer les disques durs à l’aide du tableau de bord](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Augmenter le stockage sur le serveur](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Effectuer des vérifications et des réparations sur les disques durs](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_Check)  
  
-   [Formater les disques durs](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Ajouter un nouveau disque dur](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Vue d’ensemble des espaces de stockage](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Créer un espace de stockage](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_6)  
  
##  <a name="BKMK_1"></a>Gérer les disques durs à l’aide du tableau de bord  
 Windows Server Essentials vous permet de gérer tous les disques durs connectés au serveur via le tableau de bord. Sous l’onglet **Stockage** du tableau de bord, **Disques durs** répertorie tous les disques durs disponibles sur le serveur pour le stockage des données et les sauvegardes du serveur. Le serveur contrôle l’espace disponible sur chaque disque dur et affiche une alerte si l’espace disponible sur le disque est bas. L’onglet **Disques durs** affiche les informations suivantes :  
  
- Le nom de chaque disque dur  
  
- La capacité de chaque disque dur  
  
- La quantité d’espace utilisé sur chaque disque dur  
  
- La quantité d’espace disponible sur chaque disque dur  
  
- L’état de chaque disque dur : si cet état est vide, le disque dur fonctionne correctement.  
  
- Le volet de détails, qui affiche toutes les informations de la pile de stockage (pool de stockage, espace de stockage et disque dur) si le disque dur sélectionné se trouve sur un espace de stockage (au lieu d’un disque physique)  
  
  Le tableau suivant répertorie les tâches de gestion de disque dur qui sont disponibles dans le tableau de bord, ainsi que leur description. Certaines tâches ne sont affichées que lorsqu’un disque dur est sélectionné.  
  
### <a name="available-hard-drive-management-tasks"></a>Tâches de gestion de disque dur disponibles  
  
|Nom de tâche|Description|  
|---------------|-----------------|  
|**Afficher les propriétés du disque dur**|Ouvre la page _Propriétés_ **NomDisqueDur** . Cette tâche est affichée lorsque le disque dur est sélectionné. L’onglet **Général** de la page de propriétés de *NomDisqueDur* comprend les tâches suivantes :<br /><br /> -   **Nettoyage du lecteur**:  Vous permet de nettoyer les fichiers inutilisés sur le disque dur (cette tâche n’est disponible que dans Windows Server Essentials).<br />-   **Vérifier et réparer**:  Vérifie le disque dur à la recherche d’erreurs dans le système de fichiers et tente de réparer automatiquement les erreurs détectées.<br /><br /> L’onglet **Clichés instantanés** de la page de propriétés de _Propriétés_ **NomDisqueDur** vous permet de réaliser des clichés instantanés. Cet onglet indique aussi l’heure de la prochaine exécution automatique des clichés instantanés.|  
|**Gérer les espaces de stockage**|**Remarque :** Pour Windows Server Essentials, cette tâche s’affiche uniquement lorsqu’il existe un espace de stockage existant.<br /><br /> Ouvre le panneau de configuration **Espaces de stockage**, dans lequel vous pouvez créer et gérer des pools de stockage et des espaces de stockage.|  
|**Créer un espace de stockage**|Ouvre l’Assistant Créer un espace de stockage, qui vous permet d’utiliser un ou plusieurs disques durs pour augmenter la capacité de votre pool de stockage.|  
|**Augmenter la capacité du pool de stockage**|**Remarque :** Cette tâche n’est visible que si le disque dur sélectionné se trouve sur un espace de stockage.<br /><br /> Ouvre l’Assistant Augmenter la capacité d’un pool de stockage, qui vous permet d’utiliser un ou plusieurs disques durs pour augmenter la capacité de votre pool de stockage.|  
  
##  <a name="BKMK_2"></a>Augmenter le stockage sur le serveur  
 Pour augmenter le stockage sur le serveur, vous pouvez ajouter un disque dur interne supplémentaire au serveur. Pour ce faire, vous devez arrêter le serveur, ajouter le disque dur, puis redémarrer le serveur. Vous n’avez pas besoin d’arrêter le serveur si le disque dur est attaché au contrôleur SCSI. Dans ce cas, le disque dur peut être branché pendant que le serveur fonctionne.  
  
 Selon si le disque dur à ajouter est formaté ou non, effectuez l’une des opérations suivantes :  
  
-   **Formaté** Si le disque dur interne est formaté en NTFS ou ReFS, le serveur lui affecte une lettre de lecteur, et le disque dur apparaît sous l’onglet **Disques durs** . Vous pouvez maintenant créer ou déplacer des dossiers du serveur vers le nouveau disque dur.  
  
-   **Non formaté** Si le disque dur interne n’est pas formaté, l’alerte suivante s’affiche: Un ou plusieurs disques durs non formatés sont connectés au serveur. Utilisez la procédure suivante pour formater le disque dur.  
  
#### <a name="to-format-the-hard-disk"></a>Pour formater le disque dur  
  
1.  Ouvrez le tableau de bord.  
  
2.  Dans le volet de navigation, cliquez sur l’icône d’alerte pour lancer l' **Afficheur des alertes** dans Windows Server Essentials, ou sous l’onglet analyse de l' **intégrité** de Windows Server Essentials.  
  
3.  Dans l’**Afficheur des alertes** ou dans l’onglet **Analyse du fonctionnement**, cliquez sur l’alerte, puis dans le volet des tâches, cliquez sur **Résoudre ce problème**.  
  
4.  Suivez les instructions pour compléter les étapes de l’Assistant Ajouter un nouveau disque dur.  
  
###  <a name="BKMK_Clean"></a>Pour nettoyer le disque dur  
  
1.  Ouvrez le tableau de bord.  
  
2.  Dans le volet de navigation, cliquez sur **Stockage**, puis sur **Disques durs**.  
  
3.  Dans la section **Disques durs** , sélectionnez la lettre de lecteur qui a été affectée au disque dur qui vient d’être ajouté, puis dans le volet des tâches, cliquez sur **Afficher les propriétés du disque dur**.  
  
4.  Dans **< propriétés\> DriveLetter**, sous l’onglet **général** , cliquez sur **nettoyage de lecteur**.  
  
##  <a name="BKMK_Check"></a>Effectuer des vérifications et des réparations sur les disques durs  
 Le processus de vérification et de réparation des disques durs vérifie l’intégrité du système de fichiers stocké sur les disques durs. Il lance un processus **chkdsk** sur le volume sur lequel les fichiers de sauvegarde sont stockés. L’alerte peut être résolue en lançant une vérification et une réparation sur les disques durs :  
  
-   Un ou plusieurs des disques durs de sauvegarde du serveur doivent être vérifiés.  
  
#### <a name="to-check-and-repair-hard-drives"></a>Pour vérifier et réparer les disques durs  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur **Dossiers du serveur et disques durs**, puis sur **Disques durs**.  
  
3.  Sélectionnez le disque dur qui affiche l’erreur, puis sélectionnez **Afficher les propriétés du disque dur**.  
  
4.  Sous l’onglet **Vérifier et réparer**, cliquez sur **Vérifier et réparer**.  
  
##  <a name="BKMK_3"></a>Formater les disques durs  
 Lorsqu’un disque dur interne non formaté est détecté sur le serveur, une alerte d’intégrité guide l’utilisateur dans le processus de formatage. L’Assistant Ajouter un nouveau disque dur vous guide tout au long du formatage du disque dur et vous permet de configurer le disque dur comme suit :  
  
1.  Formater le disque dur et créer automatiquement un lecteur dans ce disque. Si vous choisissez cette option, à la fin de l’Assistant, un disque dur logique formaté avec le système de fichiers NTFS est créé.  
  
2.  Formater le disque dur et le configurer pour la sauvegarde du serveur. Si vous choisissez cette option, l’Assistant Configurer la sauvegarde du serveur se lance, et il vous guide dans la configuration de la sauvegarde du serveur.  
  
3.  Si un espace de stockage n’existe pas, utilisez le nouveau disque dur pour créer un espace de stockage. Vous devez disposer d’au moins deux disques durs pour créer un espace de stockage.  
  
4.  Si l’espace de stockage existe déjà, utilisez le nouveau disque dur pour augmenter la capacité du pool de stockage. Cette option n’est affichée que si un espace de stockage a été créé sur le serveur. Si vous choisissez cette option, l’Assistant ajoute ce nouveau disque dur au pool de stockage.  
  
##  <a name="BKMK_4"></a>Ajouter un nouveau disque dur  
 Lorsque vous branchez un nouveau disque dur sur un serveur exécutant Windows Server Essentials, vous pouvez :  
  
-   [Utiliser le nouveau disque dur pour stocker les dossiers du serveur](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4a)  
  
-   [Utiliser le nouveau disque dur pour stocker les sauvegardes du serveur](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4b)  
  
-   [Utiliser le nouveau disque dur pour augmenter la capacité du pool de stockage](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)  
  
###  <a name="BKMK_4a"></a>Utiliser le nouveau disque dur pour stocker les dossiers du serveur  
 Pour utiliser le nouveau disque dur pour stocker les dossiers serveur, vous pouvez ajouter un nouveau dossier de serveur sur le disque dur ou déplacer un dossier de serveur existant sur le disque dur.  
  
##### <a name="to-store-server-folders"></a>Pour stocker des dossiers du serveur  
  
1. Ouvrez le tableau de bord.  
  
2. Cliquez sur l’onglet **STOCKAGE** , puis sur **Dossiers de serveur**.  
  
3. Dans le volet **Tâches de dossiers de serveur** , effectuez l’une des opérations suivantes :  
  
   1.  Pour ajouter un dossier serveur, cliquez sur **Ajouter un dossier**.  
  
   2.  Pour déplacer un dossier de serveur, sélectionnez le dossier que vous voulez déplacer vers le nouveau disque dur, puis cliquez sur **Déplacer un dossier**.  
  
   > [!NOTE]
   >  Si vous accédez au disque dur et que vous le sélectionnez comme emplacement de dossier de serveur sans créer de dossiers, le message d’erreur suivant s’affiche : **Un répertoire racine (tel que C:\\, D:\\) ne peut pas être ajouté en tant que dossier serveur. Créez un dossier ou sélectionnez un dossier existant sous le répertoire racine, puis réessayez**. Pour résoudre cette erreur, créez un dossier sur le disque dur que vous venez d’ajouter, puis sélectionnez le nouveau dossier comme emplacement de stockage des dossiers du serveur.  
  
4. Suivez les instructions pour terminer l’Assistant.  
  
   Pour plus d’informations sur le déplacement des dossiers du serveur, consultez [Add or move a server folder](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
###  <a name="BKMK_4b"></a>Utiliser le nouveau disque dur pour stocker les sauvegardes du serveur  
 Vous pouvez utiliser le disque dur nouvellement ajouté pour stocker les sauvegardes du serveur.  
  
##### <a name="to-store-server-backups"></a>Pour stocker les sauvegardes du serveur  
  
1. Ouvrez le tableau de bord.  
  
2. Cliquez sur l’onglet **Périphériques** , sélectionnez le serveur dans la liste, puis dans le volet des tâches, effectuez l’une des opérations suivantes :  
  
   1. Si la sauvegarde du serveur n’est pas configurée sur le serveur, cliquez sur **Configurer la sauvegarde du serveur**.  
  
   2. Si la sauvegarde du serveur est configurée sur le serveur, cliquez sur **Personnaliser la sauvegarde du serveur**.  
  
      L’Assistant Configurer la sauvegarde du serveur s’affiche.  
  
3. Dans la page **Sélectionner la destination de sauvegarde**, sélectionnez le nouveau disque dur comme destination de sauvegarde.  
  
4. Suivez les instructions pour terminer l’Assistant.  
  
###  <a name="BKMK_4c"></a>Utiliser le nouveau disque dur pour augmenter la capacité du pool de stockage  
 Si la capacité de votre pool de stockage est insuffisante, vous recevez une alerte indiquant que vous pouvez augmenter la capacité du pool de stockage en ajoutant un nouveau disque à l’aide de l’Assistant Augmenter la capacité d’un pool de stockage.  
  
> [!NOTE]
>  Vous ne pouvez effectuer cette procédure que si vous avez créé un pool de stockage sur le serveur.  
  
##### <a name="to-increase-storage-pool-capacity"></a>Pour augmenter la capacité du pool de stockage  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur l’onglet **Stockage** , puis sur **Disques durs**.  
  
3.  Sélectionnez le lecteur qui présente une capacité faible.  
  
4.  Dans le volet des tâches, sélectionnez **Augmenter la capacité du pool de stockage**. L’assistant Augmenter la capacité d’un pool de stockage s’affiche.  
  
5.  Suivez les instructions pour terminer l’Assistant.  
  
##  <a name="BKMK_5"></a>Vue d’ensemble des espaces de stockage  
 Les espaces de stockage vous permettent de regrouper des disques pour constituer un espace de stockage. Vous pouvez ensuite utiliser la capacité du pool pour créer des espaces de stockage. Les espaces de stockage sont des lecteurs virtuels qui s’affichent sous l’onglet **Disques durs** du tableau de bord. Vous pouvez utiliser des espaces de stockage comme n’importe quel autre lecteur. il est donc facile de travailler avec des fichiers. Lorsque la capacité du pool diminue, vous pouvez créer de plus grands espaces de stockage et ajouter des disques au pool de stockage. Si vous avez deux disques ou plus dans le pool de stockage, vous pouvez créer des espaces de stockage avec un miroir bidirectionnel qui ne seront pas affectés par une défaillance de disque ou même la défaillance de deux disques. Si vous créez un espace de stockage en miroir triple.  
  
 Pour créer un espace de stockage, il vous suffit d’un ou plusieurs disques supplémentaires sur le lecteur sur lequel Windows est installé. Ces lecteurs peuvent être des disques durs internes ou externes, ou encore des disques SSD. Vous pouvez utiliser différents types de lecteurs avec les espaces de stockage, par exemple USB, SATA et SAS.  
  
> [!NOTE]
>  Si vous configurez des espaces de stockage sur un serveur qui exécute Windows Server Essentials, vous ne pouvez pas effectuer une réinitialisation aux paramètres d’usine avec l’option **nettoyer les données** . La solution de contournement est de d’abord supprimer les espaces de stockage, puis d’effectuer une réinitialisation des paramètres d’usine avec l’option **Nettoyer les données**.  
  
 Pour plus d’informations sur les espaces de stockage, consultez [Forum aux questions sur les espaces de stockage](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).  
  
##  <a name="BKMK_6"></a>Créer un espace de stockage  
 Pour commencer à travailler avec les espaces de stockage sur le serveur, vous devez respecter les exigences minimales suivantes :  
  
-   Le serveur exécutant Windows Server Essentials doit être attaché aux lecteurs physiques supplémentaires (et pas seulement au lecteur de démarrage), les lecteurs ne doivent héberger aucun volume et ils doivent avoir une capacité minimale de 10 Go. Un disque physique est nécessaire pour créer un pool de stockage. Un minimum de deux disques physiques est nécessaire pour créer un espace de stockage en miroir.  
  
-   Un minimum de deux disques physiques est nécessaire pour créer un espace de stockage résilient par parité ou par mise en miroir.  
  
> [!NOTE]
>  Si vous configurez des espaces de stockage sur un serveur qui exécute Windows Server Essentials, vous ne pouvez pas effectuer une réinitialisation aux paramètres d’usine avec l’option **nettoyer les données** . La solution de contournement est de d’abord supprimer les espaces de stockage, puis d’effectuer une réinitialisation des paramètres d’usine avec l’option **Nettoyer les données**.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Pour créer un espace de stockage dans Windows Server Essentials  
  
1. Ajoutez ou connectez tous les lecteurs que vous voulez regrouper dans les espaces de stockage au serveur Windows Server Essentials.  
  
2. Dans le tableau de bord **, cliquez sur avancé: Gérer les espaces**de stockage.  
  
3. Cliquez sur **Créer un nouveau pool et un nouvel espace de stockage**.  
  
4. Sélectionnez les disques que vous souhaitez ajouter au nouvel espace de stockage, puis cliquez sur **Créer le pool**.  
  
5. Affectez un nom et une lettre au lecteur, puis choisissez une disposition. Les options **Miroir double**, **Miroir triple** et **Parité** peuvent aider à protéger les fichiers de l’espace de stockage d’une panne de disque.  
  
6. Entrez la taille maximale que l’espace de stockage peut atteindre, puis cliquez sur **Créer un espace de stockage**.  
  
   Dans Windows Server Essentials, vous pouvez créer un espace de stockage en miroir bidirectionnel à l’aide de l’Assistant Création d’un espace de stockage à partir du tableau de bord.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Pour créer un espace de stockage dans Windows Server Essentials  
  
1. Ajoutez ou connectez tous les lecteurs que vous voulez regrouper dans les espaces de stockage au serveur Windows Server Essentials.  
  
2. Dans le tableau de bord, cliquez sur l’onglet **Gérer les espaces de stockage**. L’Assistant Créer un espace de stockage s’affiche.  
  
3. Suivez les instructions pour exécuter l'Assistant.  
  
   Pour plus d’informations sur l’augmentation de capacité du pool de stockage, consultez [utiliser le nouveau disque dur pour augmenter la capacité du pool de stockage](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c).  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer les dossiers du serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)
