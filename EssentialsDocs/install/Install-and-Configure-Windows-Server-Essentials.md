---
title: Installer et configurer Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 06/17/2013
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e95cf219-46a4-4041-bd81-0c4c2a0622cf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 600aea223ebf48e1370f06070a4c3db7329c6f2f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311658"
---
# <a name="install-and-configure-windows-server-essentials"></a>Installer et configurer Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_InstallConfigure"></a>   

 Ce document fournit des instructions pas à pas pour l’installation et la configuration de Windows Server Essentials. Avant de commencer l’installation, passez en revue et effectuez les tâches décrites dans [avant d’installer Windows Server Essentials](Before-You-Install-Windows-Server-Essentials.md).  

 Ce document fournit des instructions pas à pas pour l’installation et la configuration de Windows Server Essentials. Avant de commencer l’installation, passez en revue et effectuez les tâches décrites dans [avant d’installer Windows Server Essentials](../install/Before-You-Install-Windows-Server-Essentials.md).  
  
 Vous installez et configurez Windows Server Essentials en deux étapes :  
  

1.  [Étape 1 : installer le système d’exploitation Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) Dans cette étape, vous installez le système d’exploitation sur votre serveur.  
  
2.  [Étape 2 : configurer le système d’exploitation Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) Dans cette étape, vous terminez l’installation en fournissant des informations sur votre société, les paramètres de domaine et l’administrateur réseau. L'information est utilisée pour préparer le serveur à être utilisé.  

1.  [Étape 1 : installer le système d’exploitation Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) Dans cette étape, vous installez le système d’exploitation sur votre serveur.  
  
2.  [Étape 2 : configurer le système d’exploitation Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) Dans cette étape, vous terminez l’installation en fournissant des informations sur votre société, les paramètres de domaine et l’administrateur réseau. L'information est utilisée pour préparer le serveur à être utilisé.  

  
###  <a name="step-1-install-the-windows-server-essentials-operating-system"></a><a name="BKMK_ManualInstallation"></a>Étape 1 : installer le système d’exploitation Windows Server Essentials  
  
> [!IMPORTANT]
>  Une fois le système d’exploitation installé, ne personnalisez pas votre serveur avant d’avoir terminé l ['étape 2 : configurer le système d’exploitation Windows Server Essentials](#BKMK_Step2Configure).  
  
 **Durée prévue :** environ 30 minutes.  
  
> [!NOTE]
>  Les temps d'achèvement estimés pour cette procédure sont basés sur les exigences matérielles minimum.  
  
##### <a name="to-install-the-operating-system"></a>Pour installer le système d'exploitation  
  
1. Connectez votre ordinateur à votre réseau avec un câble réseau.  
  
   > [!IMPORTANT]
   >  Ne déconnectez pas votre ordinateur du réseau pendant l'installation. Dans le cas contraire, l'installation pourrait échouer.  
  
2. Allumez votre ordinateur, puis insérez le DVD Windows Server Essentials dans le lecteur de DVD.  
  
    Si vous exécutez une installation sans assistance, connectez le support amovible (comme une disquette ou un disque mémoire flash USB) contenant vos fichiers de réponse. Selon les contenus de vos fichiers de réponse, il se peut que vous ne puissiez pas voir tous les écrans d'installation, voire même aucun.  
  
3. Redémarrez votre ordinateur. Appuyez sur une touche lorsque le message **Appuyez sur une touche pour lancer depuis le CD ou le DVD** s'affiche.  
  
   > [!NOTE]
   >  Si votre ordinateur ne démarre pas depuis le DVD, assurez-vous que le lecteur de CD-ROM apparaît en première place dans la séquence de démarrage du BIOS. Pour plus d'informations sur la séquence de démarrage du BIOS, voir la documentation du fabricant de l'ordinateur.  
  
4. Sélectionnez la **Langue** à installer, le **Format de l'heure et de la monnaie** et le **Clavier ou méthode d’entrée**, puis cliquez sur **Suivant**.  
  
5. Cliquez sur **Installer maintenant**.  
  
6. Entrez la clé de produit sous **Entrez votre clé de produit**.  
  
7. Lisez les **Termes du contrat de licence**. Pour les accepter, sélectionnez la case à cocher **J'accepte les termes du contrat de licence**, puis cliquez sur **Suivant**.  
  
   > [!NOTE]
   >  Si vous ne les acceptez pas, l'installation s'arrête.  
  
8. Sous **Quel type d'installation souhaitez-vous ?** , cliquez sur **Personnalisé : installer uniquement Windows (avancé)**  
  
9. Sous **Où souhaitez-vous installer Windows ?** , sélectionnez le disque dur sur lequel vous souhaitez installer le système d'exploitation Windows. Vérifiez que tous vos disques durs internes sont disponibles pour l'installation.  
  
    > [!IMPORTANT]
    >   Windows Server Essentials doit être installé en tant que volume C :, et la taille du volume doit être d’au moins 60 Go. Il est recommandé de créer deux partitions sur votre lecteur de système d’exploitation et de ne pas utiliser C: (partition système) pour stocker des données d’entreprise.  
  
    > [!NOTE]
    >  Si un disque dur n'est pas référencé (par exemple un disque dur SATA (Serial Advanced Technology Attachment), vous devez charger les pilotes de périphérique pour ce disque dur. Vous pouvez obtenir le pilote auprès du fabricant et le sauvegarder sur un support amovible (comme une disquette ou un disque mémoire flash USB). Reliez le support amovible à votre ordinateur, puis cliquez sur **Charger un pilote**.  
  
     Si vous devez supprimer et/ou créer des partitions, suivez les étapes suivantes :  
  
    1.  Pour supprimer une partition, sélectionnez la partition, cliquez sur **Options de lecteurs (avancées)** , puis cliquez sur **Supprimer**. Après la suppression de la partition système, créez une nouvelle partition en suivant les instructions de l'étape **b** ou de l'étape **c**.  
  
        > [!NOTE]
        >  Une fois que vous aurez cliqué sur **Options de lecteurs (avancées)** , cette option n'apparaîtra plus. Dans ce cas, ignorez la partie de l'étape concernant les options de lecteurs.  
  
    2.  Pour créer une partition depuis un espace non partitionné, cliquez sur le disque dur que vous souhaitez partitionner, cliquez sur **Options de lecteurs (avancées)** , puis sur **Nouveau**, et entrez la taille de la partition à créer dans la zone de texte **Taille**. Par exemple, si la taille de partition recommandée est de 120 Go, entrez **122880**, puis cliquez sur **Appliquer**. Une fois la partition créée, cliquez sur **Suivant**. La partition est formatée avant que l'installation ne continue.  
  
    3.  Pour créer une partition utilisant tout l'espace non partitionné, cliquez sur le disque dur que vous souhaitez partitionner, cliquez sur **Options de lecteurs (avancées)** , puis sur **Nouveau**, et enfin sur **Appliquer** pour accepter la taille de la partition par défaut. Une fois la partition créée, cliquez sur **Suivant**. La partition est formatée avant que l'installation ne continue.  
  
        > [!IMPORTANT]
        >  Vous ne pourrez plus déplacer le système d'exploitation vers une autre partition une fois cette étape terminée.  
  
   Pendant l'installation, des fichiers temporaires sont copiés vers un dossier d'installation sur votre ordinateur. Cette procédure dure environ 30 minutes. Une fois le système d’exploitation Windows Server Essentials installé, votre ordinateur redémarre. Vous êtes maintenant prêt à configurer le système d’exploitation Windows Server Essentials.  
  
###  <a name="step-2-configure-the-windows-server-essentials-operating-system"></a><a name="BKMK_Step2Configure"></a>Étape 2 : configurer le système d’exploitation Windows Server Essentials  
  
> [!IMPORTANT]
>  Si vous effectuez une migration à partir d’une version antérieure de Windows Small Business Server vers Windows Server Essentials, vous devez suivre un processus différent. Pour plus d'informations sur les installations de migration, consultez les rubriques suivantes :  
> 
> - [Migrer à partir de Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
>   -   [Migrer à partir de Windows SBS 2008](../migrate/Migrate-Windows-Small-Business-Server-2008-to-Windows-Server-Essentials.md)  
  
 Pendant cette phase de l'installation, vous êtes invité à répondre à quelques questions concernant votre organisation. Ces informations sont utilisées pour la configuration du système d'exploitation.  
  
> [!IMPORTANT]
>  Avant de commencer cette étape, assurez-vous que la carte réseau local est connectée à un routeur ou à un commutateur mis sous tension et fonctionne correctement.  
  
 **Durée prévue :** environ 30 minutes.  
  
##### <a name="to-configure-the-operating-system"></a>Pour configurer le système d'exploitation  
  
1.  À la page **Vérifiez les paramètres de date et d'heure**, cliquez sur **Modifier les paramètres de date et d’heure du système** pour sélectionner la date, l'heure et les paramètres de fuseau horaire pour votre serveur. Lorsque vous avez terminé, cliquez sur **Suivant**.  
  
    > [!IMPORTANT]
    >  Si vous installez Windows Server Essentials sur une machine virtuelle, assurez-vous de choisir les mêmes paramètres de fuseau horaire que ceux utilisés par le système d’exploitation hôte. Si les paramètres de fuseau horaire diffèrent, il se peut que l'installation du serveur ne puisse être effectuée.  
  
2.  À la page **Choix du mode d'installation du serveur** effectuez l'une des opérations suivantes :  
  
    1.  Choisissez nouvelle **installation** pour configurer une nouvelle installation complète du logiciel serveur Windows Server Essentials.  
  
    2.  Choisissez **migration du serveur** pour installer Windows Server Essentials et joindre ce serveur à un domaine Windows existant.  
  
3.  À la page **Personnalisez votre serveur**, saisissez le nom de votre organisation, un nom de domaine interne et le nom de serveur.  
  
    > [!IMPORTANT]
    >  Le nom de serveur doit être un nom unique dans votre réseau. Vous ne pourrez pas modifier le nom de serveur ou le nom de domaine interne une fois cette étape terminée.  
  
4.  Cliquez sur **Suivant**.  
  
5.  À la page **Saisissez les informations relatives au compte d'administrateur**, entrez les informations relatives à un nouveau compte d'administrateur.  
  
    > [!CAUTION]
    >  Ne nommez pas l’administrateur du compte d’administrateur réseau ou l’administrateur réseau. Ces noms de compte sont réservés au système.  
  
6.  À la page **Indiquez les informations de votre compte d'utilisateur standard**, saisissez les informations relatives à votre nouveau compte d'utilisateur standard, puis cliquez sur **Suivant**.  
  
7.  À la page **Maintenez votre serveur à jour automatiquement**, sélectionnez la manière dont vous souhaitez recevoir des mises à jour Windows pour votre serveur, puis cliquez sur **Suivant**.  
  
8.  La page **Mise à jour et préparation de votre serveur** affiche la progression du processus d'installation final. Cette opération prend du temps, et votre ordinateur sera redémarré plusieurs fois.  
  
9. Après le dernier redémarrage du serveur, la page **Votre serveur est maintenant prêt à l'emploi** s'affiche. Cliquez sur **Fermer**.  
  
10. Cliquez dans la mosaïque du Tableau de bord sur l'écran **Démarrer**, puis terminez les tâches **Configurer mon serveur** à la page d'**Accueil** du Tableau de bord. Vous devez effectuer ces tâches immédiatement après la fin de l’installation de Windows Server Essentials.  
  
> [!NOTE]
>  Une fois l'installation terminée, vous êtes automatiquement connecté au serveur avec le nouveau compte d'administrateur que vous avez ajouté pendant l'installation. Le mot de passe du compte d'administrateur intégré est défini sur le même mot de passe que le nouveau compte d'administrateur, et le compte d'administrateur intégré est ensuite désactivé.  
  
 Vous pouvez utiliser le serveur que vous venez d’installer pendant un laps de temps limité (appelé « période d’évaluation ») sans entrer de clé de produit. Après la période d'évaluation, vous devez entrer une clé de produit pour activer le serveur ou étendre la période d'évaluation. Vous pouvez étendre la période d'évaluation deux fois au maximum. Une fois le nombre de jours maximum autorisé pour la période d'évaluation atteint, vous devez activer votre serveur avec une clé de produit.  
  
### <a name="customize-windows-server-essentials"></a>Personnaliser Windows Server Essentials  
 La **page d’installation du tableau** de bord Windows Server Essentials contient des liens vers les tâches de **configuration** que vous devez effectuer immédiatement après l’installation de votre serveur. En effectuant ces tâches, vous pouvez protéger les informations stockées sur le serveur et activer les fonctionnalités disponibles dans Windows Server Essentials.  
  
 Si vous choisissez de ne pas exécuter ces tâches, il se peut que les utilisateurs n'aient pas accès à certaines fonctionnalités du réseau. Pour revenir à ces tâches ultérieurement, retournez à la page d' **espace** du tableau de bord Windows Server Essentials.  
  
 Le tableau suivant définit des éléments pouvant apparaître dans la liste des tâches de configuration.  
  
|Tâche|Description
|----------|-----------------|  
|Obtenir des mises à jour pour d'autres produits Microsoft|Cliquez sur cette tâche pour accéder à un lien qui exécute un outil qui vous permet de spécifier si vous souhaitez utiliser Microsoft Update pour obtenir automatiquement des mises à jour pour Windows Server Essentials et d’autres produits Microsoft tels qu’Office.  
|Ajouter des comptes d'utilisateur|Cliquez sur cette tâche pour afficher un bref résumé sur l'ajout de comptes d'utilisateur. Vous y trouverez un lien permettant d'exécuter l'**Assistant Ajouter un compte d'utilisateur**. Pour plus d'informations, voir [Ajouter un compte d'utilisateur](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  
|Ajouter des dossiers de serveur|Cliquez sur cette tâche pour afficher un bref résumé sur l'ajout de dossiers de serveur. Vous y trouverez un lien permettant d'exécuter l'**Assistant Ajouter un dossier**. Un lien vers une rubrique d'aide en ligne sur l'utilisation des dossiers de serveur est également proposé. Pour plus d'informations, voir [Ajouter ou déplacer un dossier de serveur](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5). 
|Configurer la sauvegarde du serveur|Cliquez sur cette tâche pour afficher un bref résumé sur l'utilisation de la sauvegarde de serveurs pour protéger vos données. Vous y trouverez un lien permettant d'exécuter l' **Assistant Configurer la sauvegarde du serveur** . Pour plus d'informations, voir [Configurer ou personnaliser la sauvegarde du serveur](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1). 
|Configurer l'accès à distance|Cliquez sur cette tâche pour afficher un bref résumé sur la fonctionnalité accès en tout lieu dans Windows Server Essentials. Vous y trouverez un lien vers la page **Paramètres de l'Accès en tout lieu**. Pour plus d’informations, consultez [gérer l’accès en tout lieu](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md). 
|Configurer la notification des alertes par courrier électronique|Cliquez sur cette tâche pour afficher un bref résumé sur la notification des alertes par courrier électronique. Vous y trouverez un lien permettant d'exécuter l'outil de **configuration de la notification des alertes par courrier électronique**. Pour plus d'informations, voir [Configurer des notifications par courrier électronique pour les alertes](../manage/Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
|Configurer le serveur multimédia|Cliquez sur cette tâche pour afficher un bref résumé sur l'utilisation du serveur multimédia pour partager des fichiers de musique, de vidéos et d'images. Vous y trouverez un lien vers la page  **Paramètres multimédias** . Un lien vers une rubrique d'aide en ligne permettant d'en savoir plus sur le serveur multimédia est également proposé. Pour plus d’informations, consultez [gérer des médias numériques](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md). 
|Connecter des ordinateurs|Cliquez sur cette tâche pour afficher un bref résumé sur la connexion d'un ordinateur réseau au serveur. Pour plus d'informations, voir [Connexion d'ordinateurs au serveur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).
  
## <a name="see-also"></a>Voir aussi  
  
-   [Installer Windows Server Essentials](Install-Windows-Server-Essentials.md)

