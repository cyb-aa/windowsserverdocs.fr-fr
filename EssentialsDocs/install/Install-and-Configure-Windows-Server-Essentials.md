---
title: Installer et configurer WindowsServerEssentials
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 06/17/2013
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e95cf219-46a4-4041-bd81-0c4c2a0622cf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cdad118c30fbf303b55ec7ea25bbe3e209c016db
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="install-and-configure-windows-server-essentials"></a>Installer et configurer WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

##  <a name="BKMK_InstallConfigure"></a>   

 Ce document fournit des instructions pas à pas pour installer et configurer WindowsServerEssentials. Avant de commencer l’installation, passez en revue et effectuez les tâches décrites dans [avant de vous installer WindowsServerEssentials](Before-You-Install-Windows-Server-Essentials.md).  

 Ce document fournit des instructions pas à pas pour installer et configurer WindowsServerEssentials. Avant de commencer l’installation, passez en revue et effectuez les tâches décrites dans [avant de vous installer WindowsServerEssentials](../install/Before-You-Install-Windows-Server-Essentials.md).  
  
 Vous installez et configurez WindowsServerEssentials en deux étapes:  
  

1.  [Étape1: Installer le système d’exploitation WindowsServerEssentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) dans cette étape, vous installez le système d’exploitation sur votre serveur.  
  
2.  [Étape2: Configurer le système d’exploitation WindowsServerEssentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) dans cette étape, vous terminez l’installation en fournissant des informations concernant votre société, les paramètres de domaine et administrateur réseau. Ces informations sont utilisées pour préparer le serveur à utiliser.  

1.  [Étape1: Installer le système d’exploitation WindowsServerEssentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) dans cette étape, vous installez le système d’exploitation sur votre serveur.  
  
2.  [Étape2: Configurer le système d’exploitation WindowsServerEssentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) dans cette étape, vous terminez l’installation en fournissant des informations concernant votre société, les paramètres de domaine et administrateur réseau. Ces informations sont utilisées pour préparer le serveur à utiliser.  

  
###  <a name="BKMK_ManualInstallation"></a>Étape1: Installer le système d’exploitation WindowsServerEssentials  
  
> [!IMPORTANT]
>  Une fois le système d’exploitation est installé, ne personnalisez pas votre serveur avant d’avoir terminé [étape2: configurer le système d’exploitation WindowsServerEssentials](#BKMK_Step2Configure).  
  
 **Durée prévue:** environ 30minutes.  
  
> [!NOTE]
>  Les temps d’achèvement estimés tout au long de cette procédure sont basés sur la configuration matérielle minimale requise.  
  
##### <a name="to-install-the-operating-system"></a>Pour installer le système d’exploitation  
  
1.  Connectez votre ordinateur à votre réseau avec un câble réseau.  
  
    > [!IMPORTANT]
    >  Ne déconnectez pas votre ordinateur à partir du réseau lors de l’installation. Cela peut entraîner l’échec de l’installation.  
  
2.  Allumez votre ordinateur, puis insérez le DVD WindowsServerEssentials dans le lecteur de DVD.  
  
     Si vous effectuez une installation sans assistance, connectez le support amovible (comme une disquette ou un lecteur flash USB) contenant vos fichiers de réponses. En fonction du contenu de vos fichiers de réponses, vous ne pouvez pas voir certaines ou un des écrans d’installation suivants.  
  
3.  Redémarrez votre ordinateur. Lorsque le message **appuyez sur n’importe quelle touche pour démarrer à partir de CD ou DVD** s’affiche, appuyez sur n’importe quelle touche.  
  
    > [!NOTE]
    >  Si votre ordinateur ne démarre pas à partir du DVD, assurez-vous que le lecteur de CD-ROM est répertorié en premier dans la séquence de démarrage du BIOS. Pour plus d’informations sur la séquence de démarrage du BIOS, consultez la documentation du fabricant de l’ordinateur.  
  
4.  Sélectionnez le **langue** que vous souhaitez installer, **format horaire et monétaire**, et **clavier ou méthode d’entrée**, puis cliquez sur **suivant**.  
  
5.  Cliquez sur **installer maintenant**.  
  
6.  Dans **entrer la clé de produit**, tapez la clé de produit.  
  
7.  Lire le **termes du contrat de licence**. Si vous les acceptez, sélectionnez le **J’accepte les termes du contrat de licence** case à cocher, puis cliquez sur **suivant**.  
  
    > [!NOTE]
    >  Si vous choisissez de ne pas accepter les termes du contrat de licence, l’installation ne continue pas.  
  
8.  Dans **quel type d’installation voulez-vous? **, cliquez sur **personnalisé: installer uniquement Windows (Avancé)**  
  
9. Dans **où souhaitez-vous installer Windows? **, sélectionnez le disque dur où vous souhaitez installer le système d’exploitation Windows. Vérifiez que tous vos disques durs internes sont disponibles pour l’installation.  
  
    > [!IMPORTANT]
    >   WindowsServerEssentials doit être installé en tant que volume C:, et la taille du volume doit être au moins 60Go. Il est recommandé de créer deux partitions sur votre disque de système d’exploitation et de pas utiliser C: (partition système) pour stocker des données d’entreprise.  
  
    > [!NOTE]
    >  Si un disque dur n’est pas répertorié (par exemple, le disque dur SATA Serial Advanced Technology Attachment ()), vous devez charger les pilotes de périphérique pour ce disque dur. Obtenir le pilote de périphérique auprès du fabricant et enregistrez-le sur un support amovible (comme une disquette ou un lecteur flash USB). Connectez le support amovible à votre ordinateur, puis cliquez sur **charger un pilote**.  
  
     Si vous devez supprimer et/ou créer des partitions, reportez-vous aux étapes suivantes:  
  
    1.  Pour supprimer une partition, sélectionnez la partition, cliquez sur **options de lecteurs (avancées)**, puis cliquez sur **supprimer**. Après la suppression de la partition système, créez une nouvelle partition en suivant les instructions de l’étape **b** ou étape **c**.  
  
        > [!NOTE]
        >  Après avoir cliqué sur **options de lecteurs (avancées)**, qu’option n’apparaît pas à nouveau. Dans ce cas, ignorez la partie de l’étape qui fait référence aux options de lecteurs.  
  
    2.  Pour créer une partition à partir d’un espace non partitionné, cliquez sur le disque dur que vous souhaitez partitionner, cliquez sur **options de lecteurs (avancées)**, cliquez sur **New**, puis, dans le **taille** texte, tapez la taille de partition à créer. Par exemple, si vous utilisez la taille de partition recommandée de 120gigaoctets (Go), tapez **122880**, puis cliquez sur **appliquer**. Une fois la partition est créée, cliquez sur **suivant**. La partition est formatée avant que l’installation se poursuit.  
  
    3.  Pour créer une partition utilisant tout l’espace non partitionné, cliquez sur le disque dur que vous souhaitez partitionner, cliquez sur **options de lecteurs (avancées)**, cliquez sur **New**, puis cliquez sur **appliquer** pour accepter la taille de partition par défaut. Une fois la partition est créée, cliquez sur **suivant**. La partition est formatée avant que l’installation se poursuit.  
  
        > [!IMPORTANT]
        >  Vous ne pouvez pas déplacer le système d’exploitation vers une autre partition après avoir terminé cette étape.  
  
 Pendant l’installation, les fichiers temporaires sont copiés dans un dossier d’installation sur votre ordinateur, ce qui prend environ 30minutes. Une fois le système d’exploitation WindowsServerEssentials est installé, votre ordinateur redémarre. Maintenant, vous êtes prêt à configurer le système d’exploitation WindowsServerEssentials.  
  
###  <a name="BKMK_Step2Configure"></a>Étape2: Configurer le système d’exploitation WindowsServerEssentials  
  
> [!IMPORTANT]
>  Si vous effectuez une migration depuis une version précédente de WindowsSmallBusinessServer vers WindowsServerEssentials, vous devez suivre un autre processus. Pour plus d’informations sur les installations de la migration, consultez les rubriques suivantes:  
>   
>  -   [Migrer à partir de Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
> -   [Migrer à partir de Windows SBS 2008](../migrate/Migrate-Windows-Small-Business-Server-2008-to-Windows-Server-Essentials.md)  
  
 Pendant cette phase de l’installation, vous êtes invité à répondre à quelques questions concernant votre organisation. Ces informations sont utilisées pour configurer le système d’exploitation.  
  
> [!IMPORTANT]
>  Avant de commencer cette étape, assurez-vous que la carte réseau local est connecté à un routeur ou un commutateur est activé et fonctionne correctement.  
  
 **Durée prévue:** environ 30minutes  
  
##### <a name="to-configure-the-operating-system"></a>Pour configurer le système d’exploitation  
  
1.  Sur le **vérifier la date et l’heure des paramètres**, cliquez sur **modifier la date système et paramètres de l’heure** pour sélectionner les paramètres de date et d’heure fuseau horaire pour votre serveur. Lorsque vous avez terminé, cliquez sur **suivant**.  
  
    > [!IMPORTANT]
    >  Si vous installez WindowsServerEssentials sur un ordinateur virtuel, assurez-vous que vous choisissez les mêmes paramètres de fuseau horaire qui sont utilisés par le système d’exploitation hôte. Si les paramètres de fuseau horaire diffèrent, l’installation du serveur ne peut pas réussir.  
  
2.  Sur le **choisir le mode d’installation du serveur** page, effectuez l’une des opérations suivantes:  
  
    1.  Choisissez **nouvelle installation** pour configurer une nouvelle installation complète du logiciel serveur WindowsServerEssentials.  
  
    2.  Choisissez **migration de serveur** pour installer WindowsServerEssentials et joindre ce serveur à un domaine Windows existant.  
  
3.  Sur le **personnaliser votre serveur** page, entrez le nom de votre organisation, un nom de domaine interne et le nom du serveur.  
  
    > [!IMPORTANT]
    >  Le nom du serveur doit être un nom unique de votre réseau. Après avoir terminé cette étape, vous ne pouvez pas modifier le nom du serveur ou le nom de domaine interne.  
  
4.  Cliquez sur **suivant**.  
  
5.  Sur le **fournir vos informations de compte d’administrateur**, tapez les informations pour un nouveau compte d’administrateur.  
  
    > [!CAUTION]
    >  Ne nommez pas le compte d’administrateur réseau administrateur ou administrateur réseau. Ces noms de compte sont réservés pour une utilisation par le système.  
  
6.  Sur le **fournir vos informations de compte d’utilisateur standard** page, tapez les informations pour un nouveau compte d’utilisateur standard, puis cliquez sur **suivant**.  
  
7.  Sur le **maintenir à jour automatiquement votre serveur**, sélectionnez la manière dont vous souhaitez recevoir des mises à jour Windows pour votre serveur, puis cliquez sur **suivant**.  
  
8.  Le **mise à jour et la préparation de votre serveur** page affiche la progression du processus d’installation final. Cela prend le temps, et votre ordinateur redémarre plusieurs fois.  
  
9. Après le dernier serveur redémarrer, le **votre serveur est prêt à être utilisé** page s’affiche. Cliquez sur **fermer **.  
  
10. Cliquez sur la vignette du tableau de bord dans le **Démarrer** de l’écran et sur le tableau de bord, exécutez le **configurer mon serveur** tâches sur le **accueil** page. Vous devez effectuer ces tâches immédiatement après la fin de votre installation de WindowsServerEssentials.  
  
> [!NOTE]
>  Une fois l’installation terminée, vous êtes automatiquement connecté au serveur avec le nouveau compte d’administrateur que vous avez ajouté pendant l’installation. Le mot de passe du compte administrateur intégré est défini sur le même mot de passe en tant que le nouveau compte d’administrateur, puis le compte administrateur intégré est désactivé.  
  
 Vous pouvez utiliser votre serveur nouvellement installé pour une durée limitée (appelée de la période d’évaluation) sans avoir à entrer une clé de produit. Après la période d’évaluation, vous devez entrer une clé de produit pour activer le serveur ou étendre la période d’évaluation. Vous pouvez étendre la période d’évaluation maximum deux fois. Lorsque vous atteignez le nombre maximal de jours autorisé pour la période d’évaluation, vous devez activer votre serveur avec une clé de produit.  
  
### <a name="customize-windows-server-essentials"></a>Personnaliser WindowsServerEssentials  
 Le **accueil** page du tableau de bord WindowsServerEssentials des liens vers **le programme d’installation** tâches que vous devez exécuter immédiatement après avoir installé votre serveur. En effectuant ces tâches, vous pouvez vous protéger les informations stockées sur le serveur et activer les fonctionnalités qui sont disponibles dans WindowsServerEssentials.  
  
 Si vous choisissez de ne pas effectuer les tâches, les utilisateurs peuvent disposer pas accès à certaines fonctionnalités de réseau. Pour revenir à ces tâches ultérieurement, retournez au tableau de bord de WindowsServerEssentials **accueil** page.  
  
 Le tableau suivant définit les éléments pouvant apparaître dans la liste des tâches de configuration.  
  
|Tâche|Description
|----------|-----------------|  
|Obtenir les mises à jour d’autres produits Microsoft|Cliquez sur cette tâche pour accéder à un lien qui exécute un outil qui vous permet de spécifier si vous souhaitez utiliser MicrosoftUpdate pour obtenir automatiquement les mises à jour pour WindowsServerEssentials et d’autres produits Microsoft tels qu’Office.  
|Ajouter des comptes d’utilisateur|Cliquez sur cette tâche pour afficher un bref résumé sur l’ajout de comptes d’utilisateur. Un lien permettant d’exécuter le **Assistant Ajout d’un utilisateur compte** est fourni. Pour plus d’informations, voir [ajouter un compte d’utilisateur](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  
|Ajouter des dossiers serveur|Cliquez sur cette tâche pour afficher un bref résumé sur l’ajout de dossiers du serveur. Un lien permettant d’exécuter le **Assistant Ajouter un dossier** est fourni. Également fourni est un lien vers une rubrique d’aide en ligne sur l’utilisation des dossiers du serveur. Pour plus d’informations, voir [ajouter ou déplacer un dossier serveur](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5). 
|Configurer la sauvegarde du serveur|Cliquez sur cette tâche pour afficher un bref résumé sur l’utilisation de la sauvegarde du serveur pour protéger vos données. Un lien permettant d’exécuter le **Assistant Configuration du serveur sauvegarde** est fourni. Pour plus d’informations, voir [définir configurer ou personnaliser la sauvegarde du serveur](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1). 
|Configurer l’accès en tout lieu|Cliquez sur cette tâche pour afficher un bref résumé sur la fonctionnalité accès en tout lieu dans WindowsServerEssentials. Un lien vers le **paramètres d’accès en tout lieu** page est fournie. Pour plus d’informations, voir [Manage Anywhere Access](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md). 
|Configurer la notification d’alerte par courrier électronique|Cliquez sur cette tâche pour afficher un bref résumé sur la notification d’alerte par courrier électronique. Un lien permettant d’exécuter le **configurer la notification par courrier électronique pour les alertes** outil est fourni. Pour plus d’informations, voir [configurer des notifications par courrier électronique pour les alertes](../manage/Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
|Configurer le serveur multimédia|Cliquez sur cette tâche pour afficher un bref résumé sur l’utilisation de serveur multimédia pour partager de musique, vidéo et les fichiers image. Un lien vers le **paramètres multimédias** page est fournie. Également fourni est un lien vers une rubrique d’aide en ligne pour en savoir plus sur le serveur multimédia. Pour plus d’informations, voir [Manage Digital Media](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md). 
|Connecter des ordinateurs|Cliquez sur cette tâche pour afficher un bref résumé sur la connexion d’un ordinateur réseau au serveur. Pour plus d’informations, voir [connecter des ordinateurs au serveur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).
  
## <a name="see-also"></a>Voir aussi  
  
-   [Installer WindowsServerEssentials](Install-Windows-Server-Essentials.md)

