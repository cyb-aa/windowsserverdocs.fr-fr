---
title: Gérer les dossiers serveur dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 090cf1b8-7b9b-48b9-ae85-b98477b8d7cc
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 0c46d19f4f172786e0ffe7f3b9dd7ac1d8f4fcf0
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322231"
---
# <a name="manage-server-folders-in-windows-server-essentials"></a>Gérer les dossiers serveur dans Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 En tant qu’administrateur de serveur, vous pouvez gérer l’accès aux dossiers de serveur (appelés dossiers partagés quand vous y accédez à partir du Launchpad, du Accès web distant, de l’application my Server pour Windows Phone ou de l’application my Server pour Windows 8) sur le serveur à l’aide des tâches de l’onglet **dossiers du serveur** du tableau de bord, ce qui permet aux utilisateurs de différents niveaux d’accès à  
  
 Les rubriques suivantes fournissent des informations qui vous aideront à comprendre, créer et gérer des dossiers serveur :  
  
-   [Gérer les dossiers du serveur à l’aide du tableau de bord](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Gérer l’accès aux dossiers du serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Ajouter ou déplacer un dossier de serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Ajouter un dossier serveur manquant](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Comprendre les dossiers partagés](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Comprendre les clichés instantanés](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Shadow)  
  
##  <a name="BKMK_2"></a>Gérer les dossiers du serveur à l’aide du tableau de bord  
 Windows Server Essentials vous permet d'effectuer des tâches d'administration courantes à l'aide du tableau de bord. La page **Dossiers du serveur** du tableau de bord fournit les éléments suivants :  
  
- Liste de dossiers serveur, qui indique les éléments suivants :  
  
  -   nom du dossier ;  
  
  -   description du dossier ;  
  
  -   emplacement du dossier ;  
  
  -   quantité d'espace libre disponible à l'emplacement du dossier ;  
  
  -   brèves informations d'état sur les tâches en cours d'exécution pour le dossier. Le champ **État** est vide si le dossier est sain, et si aucune tâche n'est en cours d'exécution.  
  
- Volet d'informations qui fournit parfois des indications supplémentaires sur un dossier sélectionné.  
  
- Volet des tâches qui inclut un ensemble de tâches d'administration relatives aux dossiers.  
  
  Le tableau suivant décrit les différentes tâches de dossier serveur disponibles dans le tableau de bord Windows Server Essentials. La plupart des tâches sont spécifiques aux dossiers. Elles ne sont visibles que si vous sélectionnez un dossier dans la liste.  
  
### <a name="server-folder-tasks-on-the-dashboard"></a>Tâches de dossier serveur dans le tableau de bord  
  
|Nom de la tâche|Description|  
|---------------|-----------------|  
|Ouvrir le dossier|Affiche le contenu du dossier sélectionné dans l'Explorateur de fichiers (appelé Explorateur Windows dans les versions précédentes de Windows).|  
|Supprimer le dossier|Vous permet de supprimer un dossier créé par un utilisateur. Cette tâche n'est pas disponible pour les dossiers par défaut créés durant l'installation du serveur.|  
|Déplacer le dossier|Ouvre un Assistant qui permet de déplacer un dossier serveur vers un nouvel emplacement.|  
|Interrompre le partage du dossier|Interrompt le partage du dossier sélectionné, mais ne le supprime pas. Quand le dossier n'est plus partagé, il n'apparaît pas dans le tableau de bord. Cette tâche n'est pas disponible pour les dossiers par défaut créés durant l'installation du serveur.|  
|Afficher les propriétés du dossier|Affiche les propriétés du dossier sélectionné et vous permet d'effectuer les actions suivantes :<br /><br /> -Modifier le nom des dossiers créés par l’utilisateur.<br /><br /> -Modifier la description d’un dossier sélectionné.<br /><br /> -Afficher la taille du dossier.<br /><br /> -Ouvrir le dossier sélectionné dans l’Explorateur de fichiers.<br /><br /> -Spécifiez les autorisations d’accès au compte d’utilisateur pour un dossier sélectionné.<br /><br /> -Masquer un dossier sélectionné à partir de la Accès web distante et des applications de service Web.<br /><br /> -Spécifiez le quota du dossier.|  
|Ajouter un dossier|Vous permet de créer un dossier serveur et d'affecter le niveau d'accès autorisé pour chaque compte d'utilisateur.|  
|Présentation des dossiers serveur|Ouvre une rubrique d'aide sur Internet qui décrit l'utilisation et les fonctionnalités des dossiers serveur.|  
  
##  <a name="BKMK_1"></a>Gérer l’accès aux dossiers du serveur  
 Windows Server Essentials vous permet de stocker les fichiers qui se trouvent sur vos ordinateurs clients dans un emplacement central à l'aide de dossiers serveur. En stockant les fichiers dans des dossiers serveur, vous avez la certitude que ces fichiers se trouvent dans un emplacement constamment accessible, de manière sécurisée, depuis chaque client.  
  
 L'utilisation de dossiers serveur pour stocker vos fichiers vous permet d'effectuer les tâches suivantes :  
  
- sauvegarder le dossier serveur à l'aide de la fonctionnalité de sauvegarde et restauration du serveur en prévision d'une défaillance totale du serveur ;  
  
- accéder aux fichiers stockés sur le dossier serveur depuis n'importe quel emplacement à l'aide d'un navigateur Internet via l'accès web à distance, ou via les applications Mon serveur pour Windows Phone et Windows 8 ;  
  
- accéder au nouveau dossier serveur à partir de n'importe quel ordinateur client.  
  
  Vous pouvez gérer l'accès aux dossiers serveur sur le serveur à l'aide des tâches situées sous l'onglet **Dossiers du serveur** du tableau de bord. Le tableau suivant répertorie les dossiers serveur créés par défaut quand vous installez Windows Server Essentials ou que vous activez la diffusion multimédia en continu sur votre serveur.  
  
|Nom du dossier serveur|Description|  
|------------------------|-----------------|  
|Sauvegardes d'ordinateurs client|Par défaut, Windows Server Essentials crée des sauvegardes d'ordinateurs clients qui sont stockées dans ce dossier. Les paramètres relatifs aux sauvegardes d'ordinateurs clients peuvent être modifiés par l'administrateur réseau.|  
|Société|Permet aux utilisateurs réseau de stocker des documents liés à votre organisation et d'y accéder.|  
|Sauvegardes de l'Historique des fichiers|Par défaut, Windows Server Essentials utilise l'historique des fichiers pour créer des sauvegardes de fichiers qui sont stockées dans ce dossier. Ces paramètres de l'historique des fichiers peuvent être modifiés par les administrateurs réseau.|  
|Redirection de dossiers|Permet aux utilisateurs réseau de stocker des dossiers configurés pour la redirection de dossiers et d'y accéder.|  
|Utilisateurs|Permet aux utilisateurs réseau de stocker des fichiers et d'y accéder. Un dossier propre à l'utilisateur est généré automatiquement dans le dossier serveur **Utilisateurs** pour chaque compte d'utilisateur réseau que vous créez.|  
|Musique|Permet aux utilisateurs réseau de stocker des fichiers de musique et d'y accéder. Ce dossier est disponible quand vous activez le partage de fichiers multimédias.|  
|Images|Permet aux utilisateurs réseau de stocker des fichiers image et d'y accéder. Ce dossier est disponible quand vous activez le partage de fichiers multimédias.|  
|TV enregistrée|Permet aux utilisateurs réseau de stocker des programmes TV enregistrés et d'y accéder. Ce dossier est disponible quand vous activez le partage de fichiers multimédias.|  
|Vidéos|Permet aux utilisateurs réseau de stocker des fichiers vidéo et d'y accéder. Ce dossier est disponible quand vous activez le partage de fichiers multimédias.|  
  
 Pour masquer ou définir les autorisations des dossiers serveur, ou pour changer les propriétés des dossiers serveur, consultez les procédures suivantes :  
  
-   [Masquer les dossiers du serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Hide)  
  
-   [Définir des autorisations sur les dossiers du serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Perms)  
  
-   [Afficher ou modifier les propriétés du dossier du serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_10)  
  
###  <a name="BKMK_Hide"></a>Masquer les dossiers du serveur  
 En tant qu'administrateur réseau, vous pouvez choisir de masquer ces dossiers serveur et les empêcher de s'afficher sur le site de l'accès web à distance ou dans les applications de services web (par exemple Mon serveur).  
  
> [!NOTE]
>  Vous devez être un administrateur réseau pour effectuer cette procédure.  
  
##### <a name="to-hide-server-folders-from-being-displayed-in-remote-web-access"></a>Pour masquer l'affichage des dossiers serveur dans l'accès web à distance  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Cliquez sur **STOCKAGE**, puis sur **Dossiers du serveur**.  
  
3.  Dans la liste, sélectionnez le dossier serveur dont vous souhaitez afficher ou modifier les propriétés.  
  
4.  Dans le volet **tâches < dossierserveur\>** , cliquez sur **afficher les propriétés du dossier**.  
  
5.  Dans **< propriétés nomdossier\>** , cliquez sur **partage**, sélectionnez **masquer ce dossier dans les applications de service Web et d’accès Web à distance**, puis cliquez sur **appliquer**.  
  
###  <a name="BKMK_Perms"></a>Définir des autorisations sur les dossiers du serveur  
 Pour chaque dossier serveur supplémentaires que vous ajoutez au serveur à l'aide du tableau de bord, vous pouvez choisir trois paramètres d'accès différents :  
  
-   **Lecture/écriture**  
  
     Choisissez ce paramètre si vous souhaitez autoriser une personne à créer, changer et supprimer des fichiers dans le dossier serveur.  
  
-   **Lecture seule**  
  
     Choisissez ce paramètre si vous souhaitez autoriser une personne à lire uniquement les fichiers du dossier serveur. Les utilisateurs avec accès en lecture seule ne peuvent pas créer, changer ou supprimer de fichiers dans le dossier serveur.  
  
-   **Aucun accès**  
  
     Choisissez ce paramètre si vous ne souhaitez pas qu'une personne ait accès aux fichiers du dossier serveur.  
  
> [!IMPORTANT]
>  Les autorisations affichées dans les propriétés du dossier correspondent uniquement aux utilisateurs gérés par le tableau de bord. Elles n'incluent pas les autorisations d'utilisateurs, par exemple celles des comptes de service ou de groupe. Par ailleurs, elles n'incluent pas les autorisations qui peuvent être définies pour le dossier à l'aide d'autres outils natifs, et n'incluent pas non plus celles des utilisateurs qui n'ont pas été ajoutés via le tableau de bord.  
  
> [!NOTE]
>  Vous devez être un administrateur réseau pour effectuer cette procédure.  
  
##### <a name="to-set-permissions-to-server-folders-on-the-server"></a>Pour définir les autorisations des dossiers serveur sur le serveur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Cliquez sur **STOCKAGE**, puis sur **Dossiers du serveur**.  
  
3.  Dans la liste, sélectionnez le dossier serveur dont vous souhaitez afficher ou modifier les propriétés.  
  
4.  Dans le volet **tâches < dossierserveur\>** , cliquez sur **afficher les propriétés du dossier**.  
  
5.  Dans **< propriétés nom_dossier\>** , cliquez sur **partage**, sélectionnez le niveau d’accès utilisateur approprié pour les comptes d’utilisateurs listés, puis cliquez sur **appliquer**.  
  
> [!NOTE]
>  Par défaut, quand vous ajoutez un compte d'utilisateur à votre réseau, un sous-dossier est créé pour l'utilisateur sous le dossier **Utilisateurs** sur le serveur. Le sous-dossier est accessible depuis un ordinateur réseau uniquement par l'utilisateur ou l'administrateur. Les autorisations sont définies pour chaque sous-dossier situé sous **Utilisateurs**. Ainsi, il n'existe aucune autorisation d'accès générale pour le dossier de premier niveau **Utilisateurs**.  
  
> [!NOTE]
>  Vous ne pouvez pas changer les autorisations de partage des dossiers serveur **Sauvegardes de l'Historique des fichiers**, **Redirection de dossiers** et **Utilisateurs**. Il n'y a donc pas d'onglet **Partage** dans les propriétés de dossier de ces dossiers serveur.  
  
###  <a name="BKMK_10"></a>Afficher ou modifier les propriétés du dossier du serveur  
 Vous pouvez changer le nom du dossier serveur et sa description. En outre, vous pouvez définir les comptes d'utilisateur qui ont accès à un dossier serveur via la tâche **Afficher les propriétés du dossier** sous l'onglet **Dossiers du serveur** du tableau de bord.  
  
> [!NOTE]
>  Dans Windows Server Essentials et Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, vous pouvez également modifier le quota de dossiers.  
  
##### <a name="to-view-or-modify-folder-properties"></a>Pour afficher ou changer les propriétés de dossier  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Cliquez sur **STOCKAGE**, puis sur **Dossiers du serveur**.  
  
3.  Dans la liste, sélectionnez le dossier serveur dont vous souhaitez afficher ou modifier les propriétés.  
  
4.  Dans le volet **tâches < dossierserveur\>** , cliquez sur **afficher les propriétés du dossier**.  
  
5.  Dans **< propriétés nomdossier\>** , sous l’onglet **général** , affichez ou modifiez le nom et la description du dossier serveur.  
  
    > [!NOTE]
    >  Dans Windows Server Essentials et Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, vous pouvez également modifier le quota de dossiers qui émet un message d’avertissement quand un dossier serveur atteint la taille spécifiée.  
  
##  <a name="BKMK_5"></a>Ajouter ou déplacer un dossier de serveur  
 Vous pouvez **ajouter d'autres dossiers serveur** pour stocker vos fichiers sur le serveur, en plus des dossiers serveur par défaut créés pendant l'installation. Vous pouvez ajouter des dossiers serveur sur le serveur principal ou sur un serveur membre exécutant Windows Server Essentials.  
  
 Vous pouvez **déplacer un dossier serveur** qui se trouve sur le serveur principal exécutant Windows Server Essentials, et qui s'affiche sous l'onglet **Dossiers du serveur** du tableau de bord vers un autre disque dur, en cas de besoin, à l'aide de l'Assistant Déplacement d'un dossier. Vous pouvez déplacer un dossier serveur vers un autre disque dur dans les situations suivantes :  
  
- Le disque dur de données n'a plus suffisamment d'espace pour stocker les données.  
  
- Vous souhaitez changer l'emplacement de stockage par défaut. Pour aller plus vite, déplacez le dossier serveur tant qu'il ne contient pas de données.  
  
- Vous souhaitez supprimer le disque dur existant sans perdre les dossiers serveur situés sur ce dernier.  
  
  Avant de déplacer le dossier, vérifiez les points suivants :  
  
- Vérifiez que vous avez sauvegardé votre serveur.  
  
- Assurez-vous que toutes les sauvegardes de clients sont à l'arrêt, si vous prévoyez de déplacer le dossier Sauvegardes d'ordinateurs client. Durant le déplacement du dossier Sauvegardes d'ordinateurs client, le serveur ne peut pas sauvegarder les ordinateurs clients tant que le déplacement du dossier n'est pas fini.  
  
- Assurez-vous que le serveur n'effectue pas d'opérations système critiques. Il est recommandé d'achever les mises à jour ou les sauvegardes en cours d'exécution avant de démarrer un déplacement de dossier. Sinon, le processus risque de durer plus longtemps.  
  
- Aucun des fichiers du dossier à déplacer n'est en cours d'utilisation. Vous ne pouvez pas accéder au dossier serveur pendant son déplacement.  
  
  Le déplacement d'un dossier du système de fichiers NTFS vers ReFS n'est pas pris en charge, si les fichiers des dossiers serveur implémentent les technologies suivantes :  
  
- autres flux de données ;  
  
- ID d’objet  
  
- noms courts (noms au format 8.3) ;  
  
- Compression  
  
- chiffrement EFS ;  
  
- Transactional NTFS, TxF (introduit avec Windows Vista) ;  
  
- Fichiers partiellement alloués  
  
- Liens physiques  
  
- attributs étendus ;  
  
- quotas.  
  
###  <a name="BKMK_6"></a>Où ajouter ou déplacer un dossier de serveur  
 En règle générale, ajoutez ou déplacez les dossiers serveur sur des disques durs disposant de la quantité maximale d'espace libre. Si possible, évitez d'ajouter ou de déplacer un dossier partagé sur le lecteur système (par exemple C:), car il risque d'empiéter sur l'espace disque nécessaire au système d'exploitation et à ses mises à jour. En outre, évitez d'ajouter ou de déplacer les dossiers serveur sur un disque dur externe, car ces derniers peuvent être facilement déconnectés, ce qui vous empêcherait d'accéder à vos fichiers. À la place, nous vous recommandons de créer le dossier sur un lecteur interne.  
  
 Un dossier serveur ne peut pas être ajouté ou déplacé dans certains emplacements. Une erreur est générée si l'un des emplacements suivants est sélectionné pour des ajouts ou des déplacements :  
  
-   disque dur non formaté avec le système de fichiers NTFS ou ReFS ;  
  
-   dossier %windir% ;  
  
-   lecteur réseau mappé ;  
  
-   dossier contenant un dossier partagé ;  
  
-   disque dur situé sous un **Périphérique avec stockage amovible** ;  
  
-   Un répertoire racine d’un disque dur (par exemple, C :\\, D :\\, E :\\)  
  
-   sous-dossier d'un dossier partagé existant ;  
  
-   Un serveur membre exécutant Windows Server Essentials ou Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé  
  
### <a name="steps-to-add-or-move-a-server-folder"></a>Étapes à suivre pour ajouter ou déplacer un dossier serveur  
  
> [!NOTE]
>  Vous devez être un administrateur du serveur pour effectuer ces procédures.  
  
##### <a name="to-add-a-server-folder"></a>Pour ajouter un dossier serveur  
  
1. Ouvrez le Tableau de bord.  
  
2. Cliquez sur **STOCKAGE**, puis sur **Dossiers du serveur**.  
  
3. Dans **Tâches de dossier serveur**, cliquez sur **Ajouter un dossier**. Cela permet de lancer l'Assistant Ajout d'un dossier.  
  
4. Suivez les instructions pour exécuter l'Assistant.  
  
   > [!NOTE]
   > - Si vous recherchez un dossier particulier à l'aide du bouton Parcourir pour spécifier l'emplacement du dossier serveur, le dossier auquel vous avez accédé est ajouté en tant que dossier serveur.  
   >   -   Vous pouvez définir les dossiers serveur accessibles via l'accès web à distance. Pour plus d'informations, voir [Gérer l'accès aux dossiers serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-move-a-server-folder"></a>Pour déplacer un dossier serveur  
  
1.  Ouvrez le Tableau de bord.  
  
2.  Cliquez sur **STOCKAGE**, puis sur **Dossiers du serveur**.  
  
3.  Dans la liste des dossiers serveur, sélectionnez le dossier à déplacer.  
  
4.  Dans le volet des tâches, cliquez sur **Déplacer le dossier**.  
  
5.  Suivez les instructions pour exécuter l'Assistant.  
  
##  <a name="BKMK_9"></a>Ajouter un dossier serveur manquant  
 Quand le serveur détecte qu’un dossier serveur prédéfini ? Société, utilisateurs, sauvegardes d’ordinateurs clients, sauvegardes de l’historique des fichiers ou redirection de dossiers ? n’est plus partagé (pour une raison ou une autre), une alerte est générée pour aider l’utilisateur à résoudre ce problème. Il est recommandé d'essayer de restaurer le dossier à partir de la sauvegarde du serveur. Toutefois, si le serveur n'a pas été sauvegardé, sélectionnez le dossier manquant, puis cliquez sur **Recréez le dossier manquant** pour reconfigurer l'emplacement du dossier serveur.  
  
> [!NOTE]
>  Seuls les dossiers prédéfinis ? La société, les utilisateurs, les sauvegardes d’ordinateurs clients, la sauvegarde de l’historique des fichiers ou la redirection de dossiers peuvent être recréés. Les dossiers serveur créés par l'utilisateur et les dossiers serveur multimédias ne peuvent pas être recréés.  
  
 Une fois le dossier manquant restauré ou recréé, il ne doit plus être répertorié comme étant **Manquant**.  
  
 Pour plus d’informations sur la restauration de fichiers à partir de sauvegardes du serveur, consultez la section en savoir plus sur la restauration de fichiers et de dossiers dans la rubrique [gérer la sauvegarde et la restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_11"></a>Comprendre les dossiers partagés  
 Il existe plusieurs manières différentes d'accéder à vos dossiers partagés sur Windows Server Essentials à partir d'un appareil connecté au serveur. Pour plus d’informations, consultez la rubrique [utiliser des dossiers partagés](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_Shadow"></a>Comprendre les clichés instantanés  
 À l'aide des clichés instantanés du serveur, les utilisateurs peuvent afficher les fichiers et les dossiers partagés tels qu'ils existaient à une date et heure spécifiques. L'accès aux versions précédentes des fichiers, ou clichés instantanés, est utile, car cela permet aux utilisateurs d'effectuer les tâches suivantes :  
  
1. **Récupérer des fichiers accidentellement supprimés**. Si vous supprimez accidentellement un fichier, vous pouvez ouvrir une version précédente de ce dernier, et la copier dans un emplacement sûr.  
  
2. **Récupérer un fichier accidentellement remplacé**. Si vous remplacez accidentellement un fichier, vous pouvez récupérer une version précédente de ce dernier. (Le nombre de versions dépend du nombre d'instantanés que vous avez créés.)  
  
3. **Comparer différentes versions d'un fichier en cours de travail**. Vous pouvez utiliser les versions précédentes d'un fichier pour vérifier les changements qui se sont produits entre les versions.  
  
   Pour utiliser les clichés instantanés, sur un ordinateur client, cliquez sur un dossier serveur partagé, puis sélectionnez **Restaurer les versions précédentes**.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer le stockage du serveur](Manage-Server-Storage-in-Windows-Server-Essentials.md)  
  
-   [Utiliser des dossiers partagés](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)
