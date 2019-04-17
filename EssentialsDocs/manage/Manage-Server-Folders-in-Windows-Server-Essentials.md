---
title: "Gérer les dossiers serveur dans Windows Server Essentials"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.openlocfilehash: e1f3640f21b95acafa850b2204cd52f9c0f324e5
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-server-folders-in-windows-server-essentials"></a>Gérer les dossiers serveur dans Windows Server Essentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
  
 En tant qu’un administrateur de serveur, vous pouvez gérer l’accès aux dossiers serveur (appelés des dossiers partagés à partir du Launchpad, accès Web à distance, application mon serveur pour Windows Phone ou application mon serveur pour Windows 8) sur le serveur à l’aide des tâches sur le **dossiers du serveur** onglet du tableau de bord, qui permet aux utilisateurs de différents niveaux d’accès à une variété de fichiers.  
  
 Les rubriques suivantes fournissent des informations qui vous aideront à comprendre, créer et gérer des dossiers du serveur:  
  
-   [Gérer les dossiers de serveur à l’aide du tableau de bord](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Gérer l’accès aux dossiers du serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Ajouter ou déplacer un dossier serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Ajouter un dossier serveur manquant](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Comprendre les dossiers partagés](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Comprendre les clichés instantanés](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Shadow)  
  
##  <a name="BKMK_2"></a>Gérer les dossiers de serveur à l’aide du tableau de bord  
 Windows Server Essentials permet d’effectuer des tâches d’administration courantes à l’aide du tableau de bord. Le **dossiers du serveur** page du tableau de bord fournit les éléments suivants:  
  
-   Liste des dossiers du serveur, qui affiche:  
  
    -   Le nom du dossier  
  
    -   Une description du dossier  
  
    -   L’emplacement du dossier  
  
    -   La quantité d’espace libre est disponible à l’emplacement du dossier  
  
    -   Brèves informations d’état sur les tâches en cours d’exécution sur le dossier; le **état** champ est vide si le dossier est sain, et si aucune tâche n’est en cours d’exécution  
  
-   Un volet d’informations qui peut-être fournir des informations supplémentaires sur un dossier sélectionné  
  
-   Un volet des tâches qui inclut un ensemble de tâches d’administration relatives aux dossiers.  
  
 Le tableau suivant décrit les différentes tâches de dossier de serveur sont disponibles sur le tableau de bord Windows Server Essentials. La plupart des tâches sont spécifiques aux dossiers, et elles ne sont visibles que lorsque vous sélectionnez un dossier dans la liste.  
  
### <a name="server-folder-tasks-on-the-dashboard"></a>Tâches de dossier de serveur sur le tableau de bord  
  
|Nom de la tâche|Description|  
|---------------|-----------------|  
|Ouvrez le dossier|Affiche le contenu du dossier sélectionné dans l’Explorateur de fichiers (appelé Explorateur Windows dans les versions précédentes de Windows).|  
|Supprimez le dossier|Vous permet de supprimer un dossier créé par l’utilisateur. Cette tâche n’est pas disponible pour les dossiers par défaut créés durant l’installation du serveur.|  
|Déplacer le dossier|Ouvre un Assistant qui vous permet de déplacer un dossier serveur vers un nouvel emplacement.|  
|Arrêter de partager le dossier|Arrête le partage du dossier sélectionné, mais ne le supprime pas. Lorsque le dossier n’est plus partagé, il n’apparaît pas dans le tableau de bord. Cette tâche n’est pas disponible pour les dossiers par défaut créés durant l’installation du serveur.|  
|Afficher les propriétés du dossier|Affiche les propriétés d’un dossier sélectionné et vous permet de:<br /><br /> -Modifier le nom des dossiers créés par l’utilisateur.<br /><br /> -Modifier la description d’un dossier sélectionné.<br /><br /> -Permet d’afficher la taille du dossier.<br /><br /> -Ouvrir le dossier sélectionné dans l’Explorateur de fichiers.<br /><br /> -Spécifier des autorisations d’accès de compte d’utilisateur pour un dossier sélectionné.<br /><br /> -Masquer un dossier sélectionné à partir d’applications accès Web à distance et le Service Web.<br /><br /> -Spécifiez le quota du dossier.|  
|Ajouter un dossier|Vous permet de créer un nouveau dossier de serveur et affecter le niveau d’accès autorisé pour chaque compte d’utilisateur.|  
|Présentation des dossiers serveur|Ouvre une rubrique d’aide sur Internet qui décrit l’utilisation et la fonctionnalité dossiers de serveur.|  
  
##  <a name="BKMK_1"></a>Gérer l’accès aux dossiers du serveur  
 Windows Server Essentials vous permet de stocker les fichiers qui sont trouvent sur vos ordinateurs clients à un emplacement central à l’aide des dossiers du serveur. En stockant les fichiers dans les dossiers du serveur garantit que vos fichiers sont dans un emplacement qui est toujours accessible de manière sécurisée à partir de chaque client.  
  
 À l’aide des dossiers du serveur pour stocker vos fichiers vous permet de:  
  
-   Sauvegarder le dossier de serveur à l’aide de la sauvegarde du serveur et restauration pour vous protéger contre une défaillance totale du serveur.  
  
-   Accès aux fichiers qui sont stockés sur le dossier de serveur à partir de n’importe quel emplacement à l’aide d’un navigateur Internet via accès Web à distance, ou via l’application My Server pour Windows Phone et Windows 8.  
  
-   Le dossier du serveur d’accès à partir de n’importe quel ordinateur client.  
  
 Vous pouvez gérer l’accès aux dossiers serveur sur le serveur à l’aide des tâches sur le **dossiers du serveur** onglet du tableau de bord. Le tableau suivant répertorie les dossiers du serveur qui sont créés par défaut lorsque vous installez Windows Server Essentials ou désactiver la diffusion multimédia en continu sur votre serveur.  
  
|Nom du dossier serveur|Description|  
|------------------------|-----------------|  
|Sauvegardes de l’ordinateur client|Par défaut, Windows Server Essentials crée des sauvegardes de l’ordinateur qui sont stockés dans ce dossier client. Les paramètres pour les sauvegardes de l’ordinateur Client peuvent être modifiés par l’administrateur réseau.|  
|Entreprise|Utilisé pour stocker et accéder aux documents liés à votre organisation par les utilisateurs du réseau.|  
|Sauvegardes de l’historique des fichiers|Par défaut, Windows Server Essentials utilise l’historique des fichiers pour créer des sauvegardes de fichiers qui sont stockés dans ce dossier. Ces paramètres de l’historique des fichiers peuvent être modifiés par les administrateurs réseau.|  
|Redirection de dossiers|Permet de stocker et d’accéder aux dossiers qui sont configurés pour la redirection de dossiers par les utilisateurs du réseau.|  
|Utilisateurs|Utilisé pour stocker les fichiers et y accéder aux utilisateurs du réseau. Un dossier spécifique à l’utilisateur est généré automatiquement dans le **utilisateurs** dossier serveur pour chaque compte d’utilisateur réseau que vous créez.|  
|Musique|Utilisé pour stocker et accéder aux fichiers de musique aux utilisateurs du réseau. Ce dossier est disponible lorsque vous activez le partage de médias.|  
|Images|Utilisé pour stocker et accéder aux fichiers d’image par les utilisateurs du réseau. Ce dossier est disponible lorsque vous activez le partage de médias.|  
|TV enregistrée|Utilisé pour stocker et accéder à des programmes TV enregistrés par les utilisateurs du réseau. Ce dossier est disponible lorsque vous activez le partage de médias.|  
|Vidéos|Utilisé pour stocker et accéder à des fichiers vidéo par les utilisateurs du réseau. Ce dossier est disponible lorsque vous activez le partage de médias.|  
  
 Pour masquer ou définir des autorisations pour les dossiers du serveur, ou pour modifier les propriétés du dossier serveur, consultez les procédures suivantes:  
  
-   [Masquer des dossiers du serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Hide)  
  
-   [Définir des autorisations aux dossiers du serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Perms)  
  
-   [Afficher ou modifier les propriétés du dossier serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_10)  
  
###  <a name="BKMK_Hide"></a>Masquer des dossiers du serveur  
 En tant qu’un administrateur réseau, vous pouvez choisir de masquer un de ces dossiers serveur et les empêcher de s’afficher sur le site Web accès Web à distance ou les applications de Services Web (par exemple, mon serveur).  
  
> [!NOTE]
>  Vous devez être un administrateur réseau pour effectuer cette procédure.  
  
##### <a name="to-hide-server-folders-from-being-displayed-in-remote-web-access"></a>Pour masquer les dossiers du serveur s’affiche dans l’accès Web à distance  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Cliquez sur **stockage**, puis cliquez sur **dossiers du serveur**.  
  
3.  Dans la liste, sélectionnez le dossier de serveur dont vous souhaitez afficher ou modifier les propriétés.  
  
4.  Dans le **< ServerFolder\ > tâches** volet, cliquez sur **afficher les propriétés du dossier**.  
  
5.  Dans **< FolderName\ > Propriétés**, cliquez sur **partage**, sélectionnez **masquez ce dossier à partir d’applications accès Web à distance et le Service Web**, puis cliquez sur **appliquer**.  
  
###  <a name="BKMK_Perms"></a>Définir des autorisations aux dossiers du serveur  
 Pour chaque dossier serveur supplémentaires que vous ajoutez sur le serveur à l’aide du tableau de bord, vous pouvez choisir trois paramètres différents accès pour qu’il:  
  
-   **En lecture/écriture**  
  
     Choisissez ce paramètre si vous souhaitez autoriser une personne à créer, modifier et supprimer des fichiers dans le dossier du serveur.  
  
-   **En lecture seule**  
  
     Choisissez ce paramètre si vous souhaitez autoriser une personne à lire uniquement les fichiers dans le dossier de serveur. Les utilisateurs avec accès en lecture seule ne peut pas créer, modifier ou supprimer des fichiers dans le dossier du serveur.  
  
-   **Aucun accès**  
  
     Choisissez ce paramètre si vous ne souhaitez pas que cette personne ait accès aux fichiers dans le dossier de serveur.  
  
> [!IMPORTANT]
>  Les autorisations qui sont affichées dans les propriétés du dossier représentent uniquement les utilisateurs qui sont gérés par le tableau de bord. Ils ne pas inclure les autorisations utilisateur tels que les groupes ou comptes de service ou inclure toutes les autorisations qui peuvent être définies pour le dossier à l’aide d’autres outils natifs ou incluent les utilisateurs qui n’ont pas été ajoutés par le biais du tableau de bord.  
  
> [!NOTE]
>  Vous devez être un administrateur réseau pour effectuer cette procédure.  
  
##### <a name="to-set-permissions-to-server-folders-on-the-server"></a>Pour définir des autorisations aux dossiers du serveur sur le serveur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Cliquez sur **stockage**, puis cliquez sur **dossiers du serveur**.  
  
3.  Dans la liste, sélectionnez le dossier de serveur dont vous souhaitez afficher ou modifier les propriétés.  
  
4.  Dans le **< ServerFolder\ > tâches** volet, cliquez sur **afficher les propriétés du dossier**.  
  
5.  Dans **< FolderName\ > Propriétés**, cliquez sur **partage**et sélectionnez le niveau d’accès utilisateur approprié pour les comptes d’utilisateur de la liste, puis cliquez sur **appliquer**.  
  
> [!NOTE]
>  Par défaut, lorsque vous ajoutez un compte d’utilisateur à votre réseau, un sous-dossier est créé pour l’utilisateur sous la **utilisateurs** dossier sur le serveur. Le sous-dossier est accessible à partir d’un ordinateur réseau par uniquement l’utilisateur ou l’administrateur. Les autorisations sont définies pour chaque sous-dossier situé sous **utilisateurs**, il y a aucune autorisation d’accès générale pour le niveau supérieur **utilisateurs** dossier.  
  
> [!NOTE]
>  Vous ne pouvez pas modifier les autorisations de partage pour le **sauvegardes de l’historique des fichiers**, **la Redirection de dossiers**, et **utilisateurs** dossiers du serveur. Par conséquent, les propriétés du dossier de ces dossiers serveur n’incluent pas un **partage** onglet.  
  
###  <a name="BKMK_10"></a>Afficher ou modifier les propriétés du dossier serveur  
 Vous pouvez modifier le nom du dossier serveur, sa description et définir les comptes d’utilisateurs ont accès à un dossier serveur via le **permet d’afficher les propriétés du dossier** de tâches sur le **dossiers du serveur** onglet du tableau de bord.  
  
> [!NOTE]
>  Dans Windows Server Essentials et Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, vous pouvez également modifier le quota du dossier.  
  
##### <a name="to-view-or-modify-folder-properties"></a>Pour afficher ou modifier les propriétés du dossier  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Cliquez sur **stockage**, puis cliquez sur **dossiers du serveur**.  
  
3.  Dans la liste, sélectionnez le dossier de serveur dont vous souhaitez afficher ou modifier les propriétés.  
  
4.  Dans le **< ServerFolder\ > tâches** volet, cliquez sur **afficher les propriétés du dossier**.  
  
5.  Dans **< Foldername\ > Propriétés**, dans le **général** , affichez ou modifiez le nom et la description du dossier serveur.  
  
    > [!NOTE]
    >  Dans Windows Server Essentials et Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, vous pouvez également modifier le quota du dossier qui donne un message d’avertissement quand un dossier serveur atteint la taille spécifiée.  
  
##  <a name="BKMK_5"></a>Ajouter ou déplacer un dossier serveur  
 Vous pouvez **ajouter d’autres dossiers serveur** pour stocker vos fichiers sur le serveur en plus des dossiers du serveur par défaut qui sont créés pendant l’installation. Vous pouvez ajouter des dossiers du serveur sur le serveur principal ou un serveur membre exécutant Windows Server Essentials.  
  
 Vous pouvez **déplacer un dossier serveur** qui se trouve sur le serveur principal exécutant Windows Server Essentials et s’affiche sur la **dossiers du serveur** onglet du tableau de bord vers un autre disque dur si nécessaire, en utilisant le déplacement un Assistant de dossier. Vous pouvez déplacer un dossier serveur vers une autre adresse d’emplacement de disque dur si:  
  
-   Le disque dur de données n’a plus suffisamment d’espace pour stocker les données.  
  
-   Vous souhaitez modifier l’emplacement de stockage par défaut. Pour un déplacement plus rapide, envisagez de déplacer le dossier du serveur si elle n’inclut pas toutes les données.  
  
-   Vous souhaitez supprimer le disque dur existant sans perdre les dossiers du serveur qui sont trouvent sur ce dernier.  
  
 Avant de déplacer le dossier, vérifiez les points suivants:  
  
-   Assurez-vous que vous avez sauvegardé votre serveur.  
  
-   Assurez-vous que toutes les sauvegardes de client sont arrêtés et pas en cours d’exécution si vous prévoyez de déplacer le dossier de sauvegarde de l’ordinateur Client. Lors du déplacement du dossier de sauvegarde de l’ordinateur Client, le serveur sera impossible de sauvegarder tous les ordinateurs clients jusqu'à ce que le déplacement du dossier est terminé.  
  
-   Assurez-vous que le serveur n’effectue pas d’opérations système critiques. Il est recommandé que vous effectuez des mises à jour ou déplacement les sauvegardes sont en cours d’exécution avant de démarrer un dossier ou le processus peut prendre plu de temps.  
  
-   Aucun des fichiers dans le dossier à déplacer n’est en cours d’utilisation. Vous ne pourrez pas accéder au dossier serveur pendant son déplacement.  
  
 Déplacement d’un dossier NTFS vers ReFS n’est pas prise en charge si les fichiers dans les dossiers serveur implémentent les technologies suivantes:  
  
-   Autres flux de données  
  
-   ID d’objet  
  
-   Noms courts (noms au format 8.3)  
  
-   Compression  
  
-   Chiffrement EFS  
  
-   Transactional NTFS, TxF (introduit avec Windows Vista)  
  
-   Fichiers partiellement alloués  
  
-   Liens physiques  
  
-   Les attributs étendus  
  
-   Quotas  
  
###  <a name="BKMK_6"></a>Où ajouter ou déplacer un dossier serveur  
 En règle générale, vous ajoutez ou déplacez les dossiers du serveur sur des disques durs disposant de la quantité maximale d’espace libre. Si possible, évitez d’ajouter ou de déplacer un dossier partagé sur le lecteur système (par exemple, C:) car il risque d’empiéter l’espace disque nécessaire est requis pour le système d’exploitation et de ses mises à jour. En outre, évitez d’ajouter ou déplacer les dossiers serveur sur un disque dur externe, car ils peuvent être facilement déconnectés, et par conséquent, vous ne pourrez pas accéder à vos fichiers. Au lieu de cela, nous vous recommandons de créer le dossier sur un lecteur interne.  
  
 Un dossier de serveur ne peut pas être ajouté ou déplacé vers les emplacements suivants et entraîne une erreur si un de ces emplacements est activée pour les ajouts ou des déplacements:  
  
-   Un disque dur qui n’est pas formaté avec le système de fichiers NTFS ou ReFS  
  
-   Le dossier % Windir%  
  
-   Un lecteur réseau mappé  
  
-   Un dossier qui contient un dossier partagé  
  
-   Un disque dur qui se trouve sous **périphérique avec stockage amovible**  
  
-   Répertoire racine d’un disque dur (par exemple, E:\\ C:\\, D:\\)  
  
-   Un sous-dossier d’un dossier partagé existant  
  
-   Un serveur membre exécutant Windows Server Essentials ou Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials  
  
### <a name="steps-to-add-or-move-a-server-folder"></a>Procédure pour ajouter ou déplacer un dossier serveur  
  
> [!NOTE]
>  Vous devez être un administrateur de serveur pour effectuer ces procédures.  
  
##### <a name="to-add-a-server-folder"></a>Pour ajouter un dossier serveur  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur **stockage**, puis cliquez sur **dossiers du serveur**.  
  
3.  Dans **tâches de dossier serveur**, cliquez sur **ajouter un dossier**. Cette opération lance l’Assistant Ajouter un dossier.  
  
4.  Suivez les instructions pour terminer l’Assistant.  
  
    > [!NOTE]
    >  -   Si vous recherchez un dossier spécifique à l’aide du bouton Parcourir pour spécifier l’emplacement du dossier serveur, le dossier que vous avez accédé est ajouté en tant qu’un dossier serveur.  
    > -   Vous pouvez définir les dossiers du serveur est accessible via l’accès Web à distance. Pour plus d’informations, voir [gérer l’accès aux dossiers du serveur](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-move-a-server-folder"></a>Pour déplacer un dossier serveur  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur **stockage**, puis cliquez sur **dossiers du serveur**.  
  
3.  Dans la liste des dossiers du serveur, sélectionnez le dossier que vous souhaitez déplacer.  
  
4.  Dans le volet tâches, cliquez sur **déplacer le dossier**.  
  
5.  Suivez les instructions pour terminer l’Assistant.  
  
##  <a name="BKMK_9"></a>Ajouter un dossier serveur manquant  
 Lorsque le serveur détecte qu’un dossier serveur prédéfini? Entreprise, les utilisateurs, sauvegardes de l’ordinateur Client, la sauvegarde de l’historique des fichiers ou Redirection de dossiers? n’est plus partagé (pour une raison quelconque ou un autre), une alerte est générée pour inviter l’utilisateur pour résoudre ce problème. Il est recommandé d’essayer de restaurer le dossier de sauvegarde du serveur. Toutefois, si le serveur n’a pas été sauvegardé, sélectionnez le dossier manquant, puis **recréer le dossier manquant** pour reconfigurer l’emplacement du dossier serveur.  
  
> [!NOTE]
>  Seuls les dossiers prédéfinis? Entreprise, les utilisateurs, sauvegardes de l’ordinateur Client, la sauvegarde de l’historique des fichiers ou Redirection de dossiers? peuvent être recréés. Dossiers serveur créés par l’utilisateur et les dossiers serveur multimédias ne peuvent pas être recréés.  
  
 Une fois que vous restaurez ou recréez le dossier manquant, il doit être répertorié n’est plus en tant que **manquant**.  
  
 Pour plus d’informations sur la restauration de fichiers à partir de sauvegardes de serveur, voir la section en savoir plus sur la restauration de fichiers et dossiers dans la rubrique [gérer la sauvegarde et restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_11"></a>Comprendre les dossiers partagés  
 Il existe différentes méthodes que vous pouvez accéder à vos dossiers partagés sur Windows Server Essentials à partir d’un appareil est connecté au serveur. Pour plus d’informations, voir la rubrique [Use Shared Folders](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_Shadow"></a>Comprendre les clichés instantanés  
 Avec les clichés instantanés du serveur, les utilisateurs peuvent afficher fichiers et dossiers partagés tels qu’ils existaient à des moments dans le passé. Accès aux versions précédentes des fichiers, ou clichés instantanés, est utile, car les utilisateurs peuvent:  
  
1.  **Récupérer des fichiers accidentellement supprimés**. Si vous supprimez accidentellement un fichier, vous pouvez ouvrir une version précédente et copiez-le dans un emplacement sûr.  
  
2.  **Récupérer à partir d’un fichier accidentellement remplacé**. Si vous remplacez accidentellement un fichier, vous pouvez récupérer une version précédente du fichier. (Le nombre de versions dépend sur le nombre de captures instantanées, vous avez créé.)  
  
3.  **Comparer différentes versions d’un fichier tout en travaillant**. Vous pouvez utiliser les versions précédentes lorsque vous souhaitez vérifier ce qui a changé entre les versions d’un fichier.  
  
 Pour utiliser des clichés instantanés, à partir d’un ordinateur client, cliquez sur un dossier partagé du serveur et sélectionnez **restaurer les versions précédentes**.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer le stockage serveur](Manage-Server-Storage-in-Windows-Server-Essentials.md)  
  
-   [Utiliser des dossiers partagés](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)
