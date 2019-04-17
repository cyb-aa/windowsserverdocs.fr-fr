---
title: "Restaurer un système complet à partir d’une sauvegarde d’ordinateur client existante"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47e498a6-1b71-47de-88f6-8c13c221d108
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cfc4d1ce461e1e1cbb9b99970355c4dc7241911b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="restore-a-full-system-from-an-existing-client-computer-backup"></a>Restaurer un système complet à partir d’une sauvegarde d’ordinateur client existante

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials 
  
 Échecs de système d’exploitation et de matériel sont rares, mais ils peuvent se produire. Un ventilateur pourrait surchauffe une carte mère et le rendre inutilisable. Le système d’exploitation peut être endommagé et refuse de démarrer. Un incendie ou l’eau peut endommager dans matériels irréparables. Un lecteur de disque dur peut échouer, ou vous pouvez décider de le remplacer par un lecteur de disque dur plus grand.  
  
 Ce document fournit des informations sur les rubriques suivantes:  
  
-   [Quelle est la restauration complète du système?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_WhatIs)  
  
-   [Comment fonctionne l’environnement de restauration du système?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_HowDoes)  
  
-   [Créer un lecteur flash USB pour restaurer un ordinateur client](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable)  
  
-   [À l’aide de l’Assistant Restauration complète du système](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using)  
  
-   [Où puis-je trouver les pilotes pour mon matériel?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers)  
  
##  <a name="BKMK_WhatIs"></a>Quelle est la restauration complète du système?  
 Dans le cas où vous remplacez un disque dur ou votre ordinateur tombe en panne au point où il ne peut pas être utilisé ou ne démarre pas, vous pouvez restaurer le système à partir d’une sauvegarde précédente de l’ordinateur. Une restauration complète du système remet le système à l’état où il se trouvait au moment de la sauvegarde.  
  
> [!IMPORTANT]
>  Vous ne pouvez pas effectuer une restauration complète du système sur le matériel de l’ordinateur, par exemple, une carte mère, qui n’est pas similaire pour le matériel de l’ordinateur qui est remplacé. Un système d’exploitation installé dépend étroitement du matériel sous-jacent de l’ordinateur. Toutefois, vous pouvez effectuer une restauration complète du système sur un disque dur qui est égale ou supérieure à celle qui est remplacé.  
  
 Lorsque vous effectuez une restauration complète du système, vous pouvez choisir une sauvegarde pour restaurer le système, avec toutes les applications, les configurations et paramètres familiers à l’utilisateur avant la défaillance, une catastrophe ou le vol. Vous pouvez également choisir les volumes que vous voulez restaurer.  
  
 Lorsque vous planifiez ou la préparation de la restauration complète du système à un ordinateur réseau, envisagez les éléments suivants:  
  
### <a name="windows-preinstallation-environment"></a>Environnement de préinstallation Windows  
 Environnement de préinstallation Windows (WindowsPE) est un système d’exploitation minimal conçu pour préparer un ordinateur pour l’installation de Windows. Pour les serveurs exécutant WindowsServerEssentials, WindowsPE est installé automatiquement lorsque vous insérez le support de restauration sur un ordinateur à restaurer. Pour les serveurs exécutant WindowsServerEssentials, WindowsPE est installé automatiquement lorsque vous démarrez l’ordinateur avec le service de restauration du client ou le lecteur flash USB.  
  
 WindowsPE ne prend pas en charge les connexions sans fil. Pour cette raison, l’ordinateur en cours de restauration doit être connecté physiquement au réseau de petite entreprise.  
  
### <a name="bitlocker"></a>BitLocker  
 Le chiffrement de lecteur BitLocker (BitLocker) est une fonctionnalité de protection des données disponible dans certaines versions de WindowsVista, Windows7 et Windows8. BitLocker protège contre le vol de données ou l’exposition sur les ordinateurs qui sont perdus ou volés et propose la suppression des données plus sécurisée lorsque les ordinateurs sont hors service.  
  
 **Pour WindowsServerEssentials:** si l’ordinateur dont vous avez besoin de restaurer a été chiffré à l’aide de BitLocker (qu’il se simplement le lecteur du système d’exploitation ou le lecteur du système d’exploitation et unique ou plusieurs autres disques fixes), vous pouvez toujours utiliser le support de restauration complète du système contenu sur le CD-ROM fourni avec votre serveur et l’Assistant Restauration complète du système pour réinstaller l’image de disque dur, y compris le système d’exploitation, à partir d’une sauvegarde et de restaurer les données sur l’ordinateur ou réparé.  
  
 **Pour WindowsServerEssentials:** si l’ordinateur dont vous avez besoin de restaurer a été chiffré à l’aide de BitLocker (qu’il se simplement le lecteur du système d’exploitation ou le lecteur du système d’exploitation et unique ou plusieurs autres disques fixes), vous pouvez toujours utiliser l’Assistant Restauration complète du système pour réinstaller l’image de disque dur, y compris le système d’exploitation, à partir d’une sauvegarde et de restaurer les données sur l’ordinateur réparé ou.  
  
 Lorsque le serveur de sauvegarde de fichiers, dossiers et lecteurs, une version non chiffrée est enregistrée sur le serveur. Au cours de restauration complète du système, cette version non chiffrée est copiée sur l’ordinateur.  
  
> [!NOTE]
>  Après une restauration complète du système réussi, vous devez réactiver BitLocker sur l’ordinateur.  
>   
>  Pour obtenir des instructions sur la façon d’activer BitLocker sur les ordinateurs qui exécutent Windows8, consultez [BitLocker: comment activer BitLocker](https://go.microsoft.com/fwlink/p/?LinkID=254918).  
>   
>  Pour obtenir des instructions sur la façon d’activer BitLocker sur les ordinateurs qui exécutent Windows7, voir [le chiffrement de lecteur BitLocker Step-by-Step Guide pour Windows7](https://go.microsoft.com/fwlink/p/?LinkId=140225).  
  
 Pour plus d’informations sur les notions de base de chiffrement de lecteur BitLocker, voir [BitLocker Forum aux Questions (FAQ)](https://go.microsoft.com/fwlink/p/?LinkID=254917).  
  
### <a name="encrypting-file-system-encrypted-files"></a>Le chiffrement des fichiers chiffrés au système de fichiers  
 La fonctionnalité de système de fichiers EFS (Encrypting File System) de Windows peut fournir supplémentaires utilisateur chiffrement des fichiers différents niveaux de sécurité entre plusieurs utilisateurs d’un même ordinateur. Il est important de noter que, contrairement aux lecteurs chiffrés par BitLocker, fichiers et dossiers chiffrés au format EFS continuent d’être cryptés dans la sauvegarde d’un ordinateur. EFS n’est pas disponible dans WindowsXPÉdition familiale, WindowsVistaStarter, WindowsVistaÉditionFamilialeBasique, WindowsVistaÉdition familiale Premium, Windows7 Édition Starter, Windows7 Édition Familiale Basique, Windows7 Édition familiale Premium ou Windows8. Il est uniquement disponible dans Windows8 Professionnel.  
  
> [!WARNING]
>  Contrairement à BitLocker, vous ne pouvez accéder des fichiers protégés par EFS à partir du système d’exploitation qui les a cryptés.  
  
### <a name="disk-partitions"></a>Partitions de disque  
 Si la taille de disque dur sur le nouvel ordinateur est identique ou supérieure à l’original, disque dur lecteur de disque est automatiquement reformaté et repartitionné. Reportez-vous au tableau ci-dessous pour connaître les spécificités:  
  
|Ordinateur d’origine|Ordinateur restauré ou nouvel|  
|-----------------------|------------------------------|  
|Disque unique avec plusieurs partitions|Disque unique avec plusieurs partitions et tout espace supplémentaire est alloué à la dernière partition|  
|Disque unique avec une seule partition|Disque unique avec une partition unique et tout l’espace disponible est utilisé pour la partition unique|  
  
> [!NOTE]
>  S’il existe des différences de disposition des taille et la partition de disque entre l’original et de l’ordinateur restauré ou nouvel, vous devez utiliser la gestion des disques pour créer les partitions appropriées sur l’ordinateur restauré ou nouvel. Vous pouvez faire cela dans l’Assistant Restauration complète du système.  
  
### <a name="raid-and-dynamic-disks"></a>Systèmes RAID et disques dynamiques  
 Sauvegarde de la baie redondante de disques indépendants (RAID) et les disques dynamiques n’est pas pris en charge.  
  
##  <a name="BKMK_HowDoes"></a>Comment fonctionne l’environnement de restauration du système?  
 Le support de restauration du système fourni avec Windows ServerÂ® 2012Essentials installe l’environnement de préinstallation Windows (WindowsPE) sur l’ordinateur. WindowsPE remplace l’environnement MS-DOS et contient les fichiers programmes principaux pour Windows. Dans WindowsServerEssentials, il existe deux méthodes prises en charge pour restaurer un système: à l’aide du service de restauration du client, qui utilise un réseau et ne repose pas sur le média, ou à l’aide de la souris USB, disque mémoire flash.  
  
> [!NOTE]
>  WindowsPE ne prend pas en charge les connexions sans fil. Pour cette raison, l’ordinateur en cours de restauration doit être connecté physiquement au réseau de petite entreprise.  
  
 L’environnement de restauration du système comprend des (x86) 32bits et 64bits (x64) fichiers programmes. Après avoir ajouté le système de support de restauration, choisissez la version appropriée des fichiers. (x86) 32bits est la valeur par défaut et est automatiquement sélectionnée si vous ne choisissez pas dans les 30secondes. S’il existe des mises à jour pour le système complet restaurer des fichiers de programme sur le serveur, les fichiers mis à jour sont automatiquement téléchargées sur l’ordinateur.  
  
 Après avoir configuré l’environnement de préinstallation, l’Assistant Restauration complète du système démarre. L’Assistant vous permet de restaurer votre ordinateur à partir d’une sauvegarde précédente. Vous pouvez également utiliser l’environnement de restauration système pour restaurer une sauvegarde vers un nouvel ordinateur avec un matériel similaire.  
  
 Dans la plupart des cas, les fichiers et les pilotes compris dans l’environnement de restauration système sont tout ce qui est nécessaire de redémarrer l’ordinateur restauré ou de nouveau. Selon que l’ordinateur ou restauré, l’environnement de restauration système ne peut pas contenir l’ensemble du stockage et pilotes qui sont requis lorsque vous redémarrez votre ordinateur restauré ou réseau. L’Assistant Restauration complète du système donne à installer des pilotes, si nécessaire. Pour plus d’informations sur vos pilotes matériels, consultez [où puis-je trouver les pilotes pour mon matériel?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers). Pour plus d’informations sur la façon d’utiliser le support de restauration système, voir [à l’aide de l’Assistant Restauration complète du système](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using).  
  
##  <a name="BKMK_CreateBootable"></a>Créer un lecteur flash USB pour restaurer un ordinateur client  
 Si vous devez restaurer un ordinateur client à partir d’une sauvegarde existante mais ne peut pas localiser le CD de restauration fourni avec votre serveur (dans WindowsServerEssentials) ou si vous ne souhaitez pas configurer le service de restauration de client sur votre serveur (dans WindowsServerEssentials), vous pouvez créer un lecteur flash USB. Vous pouvez ensuite utiliser le lecteur flash USB pour démarrer l’ordinateur client et de restaurer le système. Le lecteur flash USB que vous utilisez doit être au moins 1Go ou plus.  
  
#### <a name="to-create-a-bootable-usb-flash-drive"></a>Pour créer un lecteur flash USB  
  
1.  Ouvrez le **tableau de bord**.  
  
2.  Cliquez sur le **périphériques** onglet.  
  
3.  Dans **tâches** volet, cliquez sur **les paramètres de l’historique des fichiers et de personnaliser la sauvegarde ordinateur**.  
  
    > [!NOTE]
    >  Dans WindowsServerEssentials, cliquez sur **les tâches de sauvegarde des ordinateurs clients**.  
  
4.  Cliquez sur le **outils** onglet, puis cliquez sur **créer une clé** dans les **récupération d’ordinateur** section. L’Assistant Création d’une clé de récupération ordinateur s’ouvre.  
  
5.  Insérez un 1Go ou plus grande USB flash dans le serveur, puis suivez les instructions de l’Assistant.  
  
    > [!CAUTION]
    >  Toutes les données sur le lecteur flash USB seront supprimées.  
  
##  <a name="BKMK_Using"></a>À l’aide de l’Assistant Restauration complète du système  
 Après avoir correctement à l’aide de la restauration media, service de restauration de client ou lecteur flash USB pour démarrer votre ordinateur et vérifiez que le matériel de tous les pilotes chargés sur l’ordinateur restauré ou nouvel client, l’Assistant Restauration complète du système s’affiche. Cet Assistant permet d’accéder au serveur, la sauvegarde de l’ordinateur et les volumes source que vous souhaitez restaurer sur l’ordinateur et exécute le processus de restauration réelle.  
  
> [!NOTE]
>   WindowsServerEssentials ne prend pas en charge les scénarios de restauration suivants:  
>   
>  -   La restauration d’un disque de démarrage principal (MBR) à un Extensible Firmware Interface UEFI (Unified) basé ordinateur.  
> -   Restauration d’une sauvegarde UEFI/GPT à un système BIOS.  
>   
>  Si vous restaurez des données dans ces deux scénarios, vous ne pourrez pas démarrer le système. En outre, vous ne posez pas être en mesure d’utiliser des disques durs supérieurs à deux téraoctets.  
  
 **Configuration requise:**  
  
-   Avant de démarrer le système complet des processus de restauration, utilisez un câble réseau (une connexion câblée) pour connecter l’ordinateur au même réseau que le serveur. Assurez-vous que vous avez accès à tous les disques durs sur l’ordinateur client.  
  
    > [!WARNING]
    >  N’essayez pas d’effectuer une restauration complète du système sur un ordinateur qui utilise une connexion au réseau sans fil.  
  
-   Si vous savez que l’ordinateur est manquant réseau critiques ou des pilotes de périphériques de stockage, vous devrez localiser et copier les pilotes sur un lecteur flash USB avant de commencer le processus de restauration complète du système. Pour WindowsServerEssentials: Si vous utilisez le support de restauration complète du système fourni sur CD, CD doit rester dans le lecteur pendant la phase de démarrage du processus de restauration complète du système. Par conséquent, vous ne devez pas copier les pilotes manquants sur un CD ou DVD, sauf si vous disposez d’un second lecteur de CD/DVD. Au lieu de cela, copiez les pilotes manquants sur un lecteur flash USB.  
  
     Pour plus d’informations sur la façon de trouver les pilotes pour votre ordinateur, consultez [où puis-je trouver les pilotes pour mon matériel?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers)  
  
-   Pour WindowsServerEssentials: Si vous ne trouvez pas le CD de restauration de WindowsServerEssentials, vous pouvez créer un lecteur flash USB. Pour plus d’informations, voir [créer un lecteur flash USB pour restaurer un ordinateur client](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable).  
  
#### <a name="to-use-the-full-system-restore-wizard"></a>Pour utiliser l’Assistant Restauration complète du système  
  
1.  Effectuez l’une des opérations suivantes:  
  
    -   WindowsServerEssentials: Allumez l’ordinateur client que vous souhaitez restaurer, insérez le support de restauration, puis éteignez l’ordinateur.  
  
         Remettez l’ordinateur sous tension et au cours de Test POST (Power Self), appuyez sur la touche de fonction appropriée (touche F) pour accéder au Menu de périphérique de démarrage, puis sélectionnez le lecteur de CD/DVD. Le Gestionnaire de démarrage Windows démarre.  
  
    -   WindowsServerEssentials: Si vous utilisez le service de restauration du client, redémarrez l’ordinateur en utilisant le **démarrer à partir du réseau** option. Dans le cas contraire, démarrez l’ordinateur à l’aide de la clé USB.  
  
         Remettez l’ordinateur sous tension et au cours de Test POST (Power Self), appuyez sur la touche de fonction appropriée (touche F) pour accéder au Menu de périphérique de démarrage, puis sélectionnez **démarrer à partir du réseau** (ou vous pouvez choisir de démarrer à partir de la clé USB). Le Gestionnaire de démarrage Windows démarre.  
  
    > [!NOTE]
    >  Consultez la documentation du fabricant de votre ordinateur pour déterminer quelle touche de fonction accède au Menu démarrage du périphérique.  
  
2.  Le support de restauration d’ordinateur contient (x86) 32bits et les options de démarrage 64bits (x 64). Dans le Gestionnaire de démarrage Windows, choisissez **restauration complète du système (x86)** ou **restauration complète du système (x64)**. Si les pilotes de matériel d’ordinateur sont 32bits, choisissez x86; si elles sont 64bits, choisissez x64. Fichiers Windows sont alors chargés et l’Assistant Restauration complète du système effectue une vérification pour vous assurer que tous les pilotes matériels sont disponibles.  
  
3.  Dans le **Assistant Restauration complète du système** fenêtre, choisissez la langue de votre choix, puis cliquez sur la flèche.  
  
4.  Choisissez le **format horaire et monétaire**et le **clavier ou méthode d’entrée** pour cet ordinateur. Cliquez sur **continuer**.  
  
5.  Si les pilotes sont absents, le message que du processus de restauration ne peut pas vérifier les pilotes s’affiche. Cliquez sur **fermer**, puis dans la boîte de dialogue Bienvenue cliquez sur **charger les pilotes**.  
  
    1.  Sur le **détecter le matériel** boîte de dialogue, cliquez sur **installer des pilotes**.  
  
    2.  Insérez le lecteur flash USB qui contient les pilotes matériels, puis, dans le **installer les pilotes** boîte de dialogue, cliquez sur **analyse**.  
  
    3.  Sur le **installer les pilotes** boîte de dialogue, cliquez sur **OK** lorsque les pilotes localisés.  
  
    4.  Sur le **détecter le matériel** boîte de dialogue, cliquez sur **continuer**.  
  
6.  Si tous les pilotes ont été trouvées dans la vérification initiale ou lorsque tous les pilotes essentiels sont installés sur le **restauration complète du système** fenêtre, cliquez sur **continuer**.  
  
7.  Sur le **Bienvenue dans l’Assistant Restauration complète du système**, cliquez sur **suivant**.  
  
8.  L’Assistant recherche pour votre serveur.  
  
    1.  Si l’Assistant ne peut pas localiser votre serveur, vous pouvez soit relancer la recherche, ou d’entrer l’adresse IP du serveur.  
  
    2.  Si plusieurs serveurs ont été détectés, vous êtes invité à sélectionner.  
  
    3.  Si votre serveur se trouve, le **ouvrez une session sur < yourServerName\ >** page s’affiche.  
  
9. Sur le **ouvrez une session sur < yourServerName\ >**, tapez *< administratorAccountName\ >* dans les **nom d’utilisateur** zone de texte et le mot de passe du compte administrateur dans le **mot de passe** zone de texte, puis cliquez sur **suivant**.  
  
    > [!IMPORTANT]
    >  Vous devez utiliser un compte d’administrateur qui est créé en anglais. Si vous n’en avez pas, vous devez créer un nouveau compte d’administrateur. Pour ce faire, ouvrez d’abord le **utilisateurs** onglet du tableau de bord du serveur, définissez ensuite le format de langue du clavier en anglais, puis exécutez le **ajouter un compte d’utilisateur** tâche pour créer le compte d’administrateur. Ensuite, utilisez le nouveau compte d’administrateur pour continuer à restaurer l’ordinateur client.  
  
10. Sur le **sélectionner un ordinateur à restaurer** page, sélectionnez l’ordinateur que vous souhaitez restaurer, puis cliquez sur **suivant**. Vous pouvez choisir **< computerName\ >: (cet ordinateur)** ou un autre ordinateur sur le réseau à partir de la **un autre ordinateur** zone de liste déroulante.  
  
    > [!NOTE]
    >  S’il s’agit d’un ordinateur inconnu pour le serveur (par exemple, une nouvel ou un ordinateur reconditionné), le **cet ordinateur** option n’est pas affichée.  
  
11. Sur le **sélectionner une sauvegarde à restaurer** page, passez en revue la liste des sauvegardes disponibles et sélectionnez celle que vous souhaitez restaurer sur l’ordinateur.  
  
    > [!NOTE]
    >  Il est recommandé de sélectionner une sauvegarde réussie (cochée en vert). Cela permet de garantir que tous les fichiers système et les données seront restaurés avec succès.  
  
12. (*Facultatif*) Sélectionnez une sauvegarde, puis cliquez sur **détails** pour ouvrir le **détails de la sauvegarde** page et afficher plus d’informations sur cette sauvegarde. Utilisez les informations sur la **détails de la sauvegarde** page pour comparer plusieurs sauvegardes et déterminer quelle sauvegarde est le meilleur choix. Cliquez sur **fermer** sur le **détails de la sauvegarde** page pour revenir à la **sélectionner une sauvegarde à restaurer** page.  
  
13. Sur le **sélectionner une sauvegarde à restaurer** page, sélectionnez une sauvegarde, puis cliquez sur le **suivant** bouton.  
  
14. Sur le **sélectionner une option de restauration** page, cliquez sur une des opérations suivantes, puis cliquez sur **suivant**.  
  
    > [!NOTE]
    >  Cette page ne s’affiche pas si le partitionnement automatique n’est pas pris en charge.  
  
    1.  **Laisser l’Assistant Restauration complète de l’ordinateur (recommandé)**. Cette option permet de garantir que l’ordinateur est restauré à l’état où il était juste avant la date et heure de la sauvegarde que vous avez choisi. Si vous choisissez cette option, passez à l’étape15.  
  
    2.  **Je souhaite sélectionner les volumes à restaurer (Avancé)**. Cette option permet de choisir les volumes que vous voulez restaurer et où vous voulez les restaurer. Vous pouvez également créer des partitions sur le disque dur.  
  
15. Sur le **sélectionner les volumes à restaurer** page, vous pouvez choisir les volumes que vous voulez restaurer.  
  
    > [!NOTE]
    >  Cette page s’affiche s’il existe plusieurs disques durs sur l’ordinateur source, ou si le lecteur de destination de restauration a moins d’espace que le lecteur de la source de la sauvegarde.  
  
    1.  L’Assistant tente de faire correspondre les volumes source et de destination. Vous devez vérifier que le mappage par défaut est correct.  
  
        1.  Pour désélectionner un volume, cliquez sur la flèche du menu déroulant pour ce volume, puis cliquez sur **aucun**.  
  
        2.  Lorsque vous avez terminé votre sélection, cliquez sur **suivant**.  
  
    2.  Si le volume source et le volume de destination ont la même taille, ou si la taille de la source est inférieure à la destination, une flèche verte apparaît entre les deux. S’il existe un problème d’incompatibilité (où le volume source est plus grand que le volume de destination), un X rouge s’affiche entre la source et la destination.  
  
        > [!NOTE]
        >  Une croix rouge peut également s’afficher si:  
        >   
        >  -   La taille de secteur de disque du volume source ne correspond pas à la taille de secteur de disque du volume de destination. Cela peut se produire si vous remplacez le disque physique avec un disque ayant une taille de secteur différente, ou si vous configurez des espaces de stockage (ce qui peut avoir une taille de secteur différente que celle du disque physique).  
        > -   Vous atteignez la nombre limite de clusters. Pour restaurer le volume source sur le volume de destination, vous devez formater le volume de destination avec la même taille de cluster que le volume source. Si le volume de destination est trop importante, et si la taille de cluster est trop petite, vous pouvez atteindre le nombre limite de clusters.  
  
        1.  Cliquez sur **exécuter le Gestionnaire de disques (Avancé)**et créer un nouveau volume est la même taille que le volume réservé au système.  
  
            > [!NOTE]
            >  Si un ordinateur client est Extensible Firmware Interface UEFI (Unified) en fonction, vous devez utiliser le **diskpart** outil pour initialiser le disque système. Pour ce faire, ouvrez une fenêtre de commande (appuyez sur Ctrl + Alt + Maj pendant 5secondes dans l’environnement WinPE), exécutez **diskpart.exe**, puis exécutez les commandes diskpart suivantes:  
            >   
            >  1.  **DISKPART > disque de liste**  
            > 2.  **DISKPART > select disk #***< logique\ >*  
            > 3.  **DISKPART > nettoyer**  
            > 4.  **DISKPART > convertir gpt**  
            > 5.  **DISKPART > créer de taille de partition efi =***100* (où *100* est un exemple de taille de partition en Mo, doit être identique à la partition d’origine)  
            > 6.  **DISKPART > créer de taille de partition msr =***128* (où *128* est un exemple de taille de partition en Mo, doit être identique à la partition d’origine)  
            > 7.  **DISKPART > quitter**  
  
        2.  *(Facultatif) *Sélectionnez l’option **n’attribuez pas une lettre de lecteur ou un chemin d’accès de lecteur**.  
  
        3.  Formater le volume en tant que **NTFS**.  
  
        4.  Une fois le formatage terminé, cliquez sur le nouveau volume système, puis cliquez sur **marquer la Partition comme Active**.  
  
        5.  Si vous avez besoin de correspondre aux autres volumes de la sauvegarde des volumes supplémentaires, répétez les étapes *ii* via *iv* pour créer et activer les volumes, puis fermez **gestion des disques**.  
  
        6.  Sur le **sélectionner les volumes à restaurer** page, mappez le volume réservé à la source pour le volume de la même taille que vous avez créé à l’étape *v*.  
  
        7.  Mappez tous les autres volumes source aux volumes de destination correspondants.  
  
        8.  Cliquez sur **suivant** pour poursuivre la restauration.  
  
16. Sur le **confirmer les volumes à restaurer** page, passez en revue le mappage, puis cliquez sur **suivant**. Si vous avez besoin d’apporter des modifications, cliquez sur **précédent**, puis répétez l’étape14.  
  
17. Le **restauration < computerName\ > < date et heure de sauvegardes\ >** page indique la progression du processus de restauration.  
  
18. Sur le **la restauration a abouti** page, retirez le support de restauration, puis cliquez sur **Terminer**. L’ordinateur redémarre.  
  
    > [!IMPORTANT]
    >  Si le chiffrement de lecteur BitLocker était activé sur l’ordinateur avant la restauration, vous devez activer BitLocker manuellement après le redémarrage de l’ordinateur.  
  
##  <a name="BKMK_FindDrivers"></a>Où puis-je trouver les pilotes pour mon matériel?  
 Selon que l’ordinateur restauré ou, le support de restauration ne peut pas contenir l’ensemble du stockage et réseau des pilotes qui sont nécessaires lorsque vous redémarrez votre ordinateur restauré. Vous devez déterminer les pilotes manquent, rechercher ces pilotes sur le support existant ou sur le site Web de fabricant s, les copier sur un disque mémoire flash, puis les copier à partir de la clé USB sur le nouveau ou restauré l’ordinateur lorsque vous exécutez l’Assistant Restauration complète du système.  
  
 Lorsqu’un ordinateur est sauvegardé, les pilotes de l’ordinateur sont enregistrés dans la sauvegarde. Si votre support de restauration n’inclut pas tous les pilotes dont vous avez besoin, vous pouvez ouvrir une sauvegarde pour cet ordinateur et copier les pilotes sur un lecteur flash USB.  
  
#### <a name="to-copy-drivers-from-a-backup-to-a-usb-flash-drive"></a>Pour copier des pilotes à partir d’une sauvegarde sur un lecteur flash USB  
  
1.  Sur un autre ordinateur, ouvrez le tableau de bord.  
  
2.  Cliquez sur **périphériques**, puis cliquez sur l’ordinateur pour lequel vous avez besoin de pilotes.  
  
3.  Cliquez sur **restaurer des fichiers ou dossiers de l’ordinateur**. La restauration de fichiers ou dossiers Assistant s’ouvre.  
  
4.  Cliquez sur la dernière sauvegarde réussie, puis cliquez sur **suivant**.  
  
5.  Cliquez sur un volume pour l’ouvrir, puis cliquez sur **suivant**. Une fenêtre s’ouvre qui répertorie les fichiers et dossiers dans la sauvegarde.  
  
6.  Insérez votre lecteur flash USB dans un connecteur USB sur l’ordinateur, puis copiez les pilotes de restauration complète du système de dossier sur votre lecteur flash USB.  
  
    > [!NOTE]
    >  Vous devrez peut-être cliquer sur **un niveau plus haut** jusqu'à atteindre la racine du volume système.  
  
7.  Supprimer la clé USB, puis insérez-le dans l’ordinateur que vous restaurez.  
  
 Vous pouvez utiliser le lecteur flash USB pour installer les pilotes de votre ordinateur lors de sa restauration. La restauration de fichiers ou dossiers Assistant recherche de pilotes supplémentaires sur ce lecteur flash USB lors de l’utilisation de l’Assistant Restauration complète du système. Les pilotes que vous êtes probablement besoin sont le pilote de carte réseau et les pilotes de périphérique de stockage.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde et restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)
