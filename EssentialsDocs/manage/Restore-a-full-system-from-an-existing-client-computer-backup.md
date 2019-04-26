---
title: Restaurer un système complet à partir d’une sauvegarde d’ordinateur client existante
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869690"
---
# <a name="restore-a-full-system-from-an-existing-client-computer-backup"></a>Restaurer un système complet à partir d’une sauvegarde d’ordinateur client existante

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Les pannes du système d'exploitation et du matériel sont rares, mais pas impossibles. Un ventilateur défaillant peut provoquer une surchauffe de la carte mère et la rendre inutilisable. Le système d'exploitation pourrait alors être endommagé et refuser de démarrer. Un incendie ou une inondation sont susceptibles d'occasionner des dommages matériels irréparables. Vous pouvez également faire face à une panne du disque ou décider de le remplacer par un disque dur d'une capacité supérieure.  
  
 Ce document fournit des informations sur les sujets suivants :  
  
-   [Quelle est la restauration complète du système ?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_WhatIs)  
  
-   [Comment fonctionne l’environnement de restauration du système ?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_HowDoes)  
  
-   [Créer un lecteur flash USB pour restaurer un ordinateur client](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable)  
  
-   [À l’aide de l’Assistant de restauration complète du système](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using)  
  
-   [Où puis-je trouver les pilotes à mon matériel ?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers)  
  
##  <a name="BKMK_WhatIs"></a> Quelle est la restauration complète du système ?  
 Dans l'éventualité d'un changement de disque dur ou d'une défaillance de l'ordinateur qui l'empêcherait d'être opérationnel ou de redémarrer, vous pouvez envisager de restaurer le système à partir d'une sauvegarde précédente de l'ordinateur. Une restauration complète du système remet le système à l'état dans lequel il se trouvait au moment de la sauvegarde.  
  
> [!IMPORTANT]
>  Elle ne peut pas s'appliquer à un composant matériel (carte mère, par exemple) qui serait différent de celui de l'ordinateur remplacé. Le système d'exploitation installé dépend étroitement du matériel sous-jacent. Il est possible, cependant, d'effectuer une restauration complète du système sur un disque dur d'une capacité supérieure ou égale à celui que vous remplacez.  
  
 Lorsque vous lancez une restauration complète du système, vous pouvez choisir la sauvegarde qui permettra de restituer le système et l'ensemble des applications, configurations et paramètres que l'utilisateur avait l'habitude d'employer avant la panne, la catastrophe ou le vol. Vous êtes libre également de choisir les volumes à restaurer.  
  
 Quand vous planifiez ou préparez la restauration complète du système sur un ordinateur réseau, tenez compte des points suivants :  
  
### <a name="windows-preinstallation-environment"></a>Environnement de préinstallation Windows  
 Environnement de préinstallation Windows (Windows PE) est un système d'exploitation minimal conçu pour préparer un ordinateur à l'installation de Windows. Pour les serveurs exécutant Windows Server Essentials, Windows PE est installé automatiquement lorsque vous insérez le support de restauration sur un ordinateur à restaurer. Pour les serveurs exécutant Windows Server Essentials, Windows PE est installé automatiquement lorsque vous démarrez l’ordinateur avec le service de restauration du client ou le lecteur flash USB.  
  
 Windows PE ne prend pas en charge les connexions sans fil. Pour cette raison, l'ordinateur en cours de restauration doit être connecté physiquement au réseau de petite entreprise  
  
### <a name="bitlocker"></a>BitLocker  
 Le chiffrement de lecteur BitLocker (BitLocker) est une fonctionnalité de protection de données qui est disponible dans certaines versions de Windows Vista, Windows 7 et Windows 8. BitLocker répond aux menaces liées au vol ou à l'exposition de données sur des ordinateurs perdus ou volés et rend la suppression des données plus sûre lorsque les ordinateurs sont mis hors service.  
  
 **Pour Windows Server Essentials :** Si l'ordinateur à restaurer a été chiffré à l'aide de BitLocker (qu'il s'agisse du lecteur système seul ou combiné à un ou plusieurs autres disques fixes), vous pouvez toujours utiliser le support de restauration complète du système (figurant sur le CD-ROM fourni avec votre serveur) et l'Assistant Restauration complète du système pour réinstaller l'image du disque dur (avec le système d'exploitation) à partir d'une sauvegarde, puis restaurer les données sur le nouvel ordinateur ou l'ordinateur réparé.  
  
 **Pour Windows Server Essentials :** Si l'ordinateur à restaurer a été chiffré à l'aide de BitLocker (qu'il s'agisse du lecteur système seul ou combiné à un ou plusieurs autres disques fixes), vous pouvez toujours utiliser l'Assistant Restauration complète du système pour réinstaller l'image du disque dur (avec le système d'exploitation) à partir d'une sauvegarde, puis restaurer les données sur le nouvel ordinateur ou l'ordinateur réparé.  
  
 Lorsque le serveur sauvegarde des disques, des dossiers et des fichiers, une version non chiffrée est enregistrée sur le serveur. Au cours de la restauration complète du système, cette version non chiffrée est copiée sur l'ordinateur.  
  
> [!NOTE]
>  À l'issue de la procédure, vous devez réactiver BitLocker sur l'ordinateur.  
>   
>  Pour obtenir des instructions sur l’activation de BitLocker sur les ordinateurs qui exécutent Windows 8, consultez [BitLocker : Comment activer BitLocker](https://go.microsoft.com/fwlink/p/?LinkID=254918).  
>   
>  Pour obtenir des instructions sur l’activation de BitLocker sur les ordinateurs qui exécutent Windows 7, consultez [BitLocker Drive Encryption Step Guide pour Windows 7](https://go.microsoft.com/fwlink/p/?LinkId=140225).  
  
 Pour plus d’informations sur le chiffrement de base des lecteurs BitLocker, consultez [Forum Aux Questions sur BitLocker (FAQ)](https://go.microsoft.com/fwlink/p/?LinkID=254917).  
  
### <a name="encrypting-file-system-encrypted-files"></a>Fichiers chiffrés du système de fichiers EFS  
 La fonction EFS (Encrypting File System, système de fichiers de cryptage) de Windows assure un chiffrement des fichiers de niveau utilisateur, ce qui permet de proposer différents niveaux de sécurité sur un même ordinateur. Toutefois, à la différence des lecteurs chiffrés par BitLocker, les dossiers et fichiers cryptés avec EFS continuent d'être cryptés dans la sauvegarde d'un ordinateur. EFS n’est pas disponible dans Windows XP Édition familiale, Windows Vista Starter, Windows Vista Édition Familiale Basique, Windows Vista Édition familiale Premium, Windows 7 Édition Starter, Windows 7 Édition Familiale Basique, Windows 7 Édition familiale Premium ou Windows 8. Il est uniquement disponible dans Windows 8 Professionnel.  
  
> [!WARNING]
>  Contrairement à BitLocker, pour accéder aux fichiers protégés par EFS, il est indispensable d'utiliser le système d'exploitation ayant servi à crypter ces fichiers.  
  
### <a name="disk-partitions"></a>Partitions de disque  
 Si la taille du disque dur du nouvel ordinateur est supérieure ou égale à la taille d'origine, le disque dur est automatiquement reformaté et repartitionné. Pour connaître les spécificités de ces opérations, reportez-vous au tableau ci-dessous :  
  
|Ordinateur d'origine|Ordinateur restauré ou nouvel ordinateur|  
|-----------------------|------------------------------|  
|Disque unique avec plusieurs partitions|Disque unique avec plusieurs partitions et tout espace supplémentaire est alloué à la dernière partition|  
|Disque unique avec une seule partition|Disque unique avec une seule partition (tout l'espace disponible est réservé à cette partition)|  
  
> [!NOTE]
>  Si la capacité du disque dur et la configuration des partitions ne sont pas les mêmes que sur l'ordinateur d'origine, vous devez exécuter l'utilitaire Gestion des disques pour créer les partitions appropriées sur le nouvel ordinateur ou l'ordinateur restauré. Cet utilitaire est accessible à partir de l'Assistant Restauration complète du système.  
  
### <a name="raid-and-dynamic-disks"></a>Systèmes RAID et disques dynamiques  
 La sauvegarde des systèmes RAID (Redundant Array of Independent Disks, matrice redondante de disques indépendants) et des disques dynamiques n'est pas prise en charge.  
  
##  <a name="BKMK_HowDoes"></a> Comment fonctionne l’environnement de restauration du système ?  
 Le support de restauration système fourni avec Windows ServerÂ® 2012 Essentials installe l’environnement de préinstallation Windows (Windows PE) sur l’ordinateur. Windows PE remplace l'environnement MS-DOS et contient les fichiers programmes principaux pour Windows. Dans Windows Server Essentials, il existe deux méthodes prises en charge pour restaurer un système : l’utilisation du service de restauration de client, qui utilise un réseau et ne repose pas sur le média, ou à l’aide de la souris USB lecteur flash.  
  
> [!NOTE]
>  Windows PE ne prend pas en charge les connexions sans fil. Pour cette raison, l'ordinateur en cours de restauration doit être connecté physiquement au réseau de petite entreprise  
  
 L'environnement de restauration du système comprend des fichiers programmes 32 bits (x86) et 64 bits (x64). Une fois le support de restauration du système inséré, choisissez la version des fichiers appropriée. La version 32-bit (x86) est le réglage par défaut, et elle est sélectionnée automatiquement si vous n'en choisissez pas une autre dans les 30 secondes. S'il existe des mises à jour des fichiers programmes de la restauration complète du système sur le serveur, les fichiers mis à jour sont téléchargés automatiquement sur l'ordinateur.  
  
 Une fois l'environnement de préinstallation défini, l'Assistant Restauration complète du système démarre. Il vous aide à restaurer votre ordinateur à partir d'une sauvegarde antérieure. Vous pouvez également utiliser l'environnement de restauration du système pour restaurer une sauvegarde sur un autre ordinateur avec un matériel similaire.  
  
 Dans la plupart des instances, les fichiers programmes et les pilotes compris dans l'environnement de restauration du système sont tout ce dont vous avez besoin pour redémarrer le nouvel ordinateur ou l'ordinateur restauré. Selon que l'ordinateur de destination a déjà fait ou non l'objet d'une restauration, l'environnement de restauration du système risque de ne pas contenir l'ensemble des pilotes de stockage et de carte réseau nécessaires au redémarrage du nouvel ordinateur ou de l'ordinateur restauré. L'Assistant Restauration complète du système vous permet d'installer des pilotes le cas échéant. Pour plus d’informations sur vos pilotes matériels, consultez [Where can I find the drivers for my hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers). Pour plus d'informations sur la manière d'utiliser le support de restauration du système, voir [Utilisation de l'Assistant Restauration complète du système](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using).  
  
##  <a name="BKMK_CreateBootable"></a> Créer un lecteur flash USB pour restaurer un ordinateur client  
 Si vous devez restaurer un ordinateur client depuis une sauvegarde, mais ne peut pas localiser le CD de restauration fournie avec votre serveur (dans Windows Server Essentials) ou vous ne souhaitez pas configurer le service de restauration de client sur votre serveur (dans Windows Server Essentials) , vous pouvez créer un lecteur flash USB. Vous pouvez ensuite utiliser le lecteur flash USB pour démarrer l'ordinateur client et restaurer le système. Le lecteur flash USB que vous utilisez doit avoir 1 Go une taille supérieure.  
  
#### <a name="to-create-a-bootable-usb-flash-drive"></a>Pour créer un lecteur flash USB de démarrage  
  
1.  Ouvrez le **Tableau de bord**.  
  
2.  Cliquez sur l'onglet **Périphériques**.  
  
3.  Dans volet **Tâches**, cliquez sur **Personnaliser les paramètres de la sauvegarde de l'ordinateur et de l'historique des fichiers**.  
  
    > [!NOTE]
    >  Dans Windows Server Essentials, cliquez sur **les tâches de sauvegarde des ordinateurs clients**.  
  
4.  Cliquez sur l'onglet **Outils** , puis sur **Créer une clé** dans la section **Récupération d'ordinateur** . L'Assistant Création d’une clé de récupération d’ordinateur s'ouvre.  
  
5.  Insérez un lecteur flash USB de 1 Go ou plus dans le serveur, puis suivez les instructions de l'Assistant.  
  
    > [!CAUTION]
    >  Toutes les données sur le lecteur flash USB seront supprimées.  
  
##  <a name="BKMK_Using"></a> À l’aide de l’Assistant de restauration complète du système  
 L'Assistant Restauration complète du système s'affiche dès que vous démarrez votre ordinateur au moyen du support de restauration, du service de restauration de client ou du lecteur flash USB et que vous confirmez le chargement de tous les pilotes matériels sur le nouvel ordinateur client ou sur l'ordinateur restauré. Cet assistant vous permet d'accéder au serveur, à la sauvegarde de l'ordinateur et aux volumes source à restaurer sur l'ordinateur et d'effectuer l'opération de restauration proprement dite.  
  
> [!NOTE]
>   Windows Server Essentials ne prend pas en charge les scénarios de restauration suivants :  
>   
>  -   Restaurer un disque de démarrage principal (MBR) à un Extensible Firmware Interface UEFI (Unified) en fonction ordinateur.  
> -   Restauration d'une sauvegarde UEFI/GPT dans un système BIOS.  
>   
>  Si vous restaurez des données selon l'un de ces scénarios, vous ne serez pas en mesure de lancer le système. De plus, il se peut que vous ne puissiez pas utiliser des disques durs dont la taille dépasse deux téraoctets.  
  
 **Conditions préalables :**  
  
-   Avant de lancer le processus de restauration complète du système, utilisez un câble réseau (une connexion câblée) pour connecter l'ordinateur au même réseau que le serveur. Assurez-vous d'avoir accès à tous les disques durs de l'ordinateur client.  
  
    > [!WARNING]
    >  N'essayez pas de lancer une restauration complète du système sur un ordinateur relié au réseau par une connexion sans fil.  
  
-   Si vous constatez qu'il manque des pilotes de carte réseau et de périphériques de stockage essentiels sur l'ordinateur, recherchez ces pilotes, puis copiez-les sur un lecteur flash USB avant de procéder à la restauration complète du système. Pour Windows Server Essentials : Si vous utilisez le support de restauration complète du système fourni sur CD, le CD doit rester dans le lecteur pendant la phase de démarrage du processus de restauration complète du système. Pour cette raison, vous ne devez pas copier les lecteurs manquants sur un CD ou DVD à moins de disposer d'un second lecteur de CD/DVD. Au lieu de cela, copiez les pilotes manquants sur un lecteur flash USB.  
  
     Pour savoir où trouver les pilotes pour votre ordinateur, consultez [Where can I find the drivers for my hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers).  
  
-   Pour Windows Server Essentials : Si vous ne trouvez pas le CD de restauration de Windows Server Essentials, vous pouvez créer un lecteur flash USB. Pour plus d'informations, voir [Create a bootable USB flash drive to restore a client computer](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable).  
  
#### <a name="to-use-the-full-system-restore-wizard"></a>Pour utiliser l'Assistant Restauration complète du système  
  
1.  Faites une des actions suivantes :  
  
    -   Windows Server Essentials : Allumez l'ordinateur client à restaurer, insérez le support de restauration, puis éteignez l'ordinateur.  
  
         Remettez l'ordinateur sous tension et appuyez sur la touche de fonction appropriée (touche F) au cours du test POST (Power On Self Test, autotest de mise sous tension) pour accéder au menu du périphérique d'amorçage. Sélectionnez votre lecteur de CD/DVD-ROM dans ce menu. Cela a pour effet d'ouvrir le Gestionnaire de démarrage Windows.  
  
    -   Windows Server Essentials : Si vous utilisez le service de restauration de client, redémarrez l'ordinateur à l'aide de l'option **Démarrer à partir du réseau**. Sinon, démarrez l'ordinateur à l'aide de la clé USB.  
  
         Redémarrez à nouveau l'ordinateur, puis pendant l'autotest de mise sous tension (POST), appuyez sur la touche de fonction appropriée pour accéder au menu du périphérique de démarrage, puis sélectionnez **Démarrer à partir du réseau** (ou vous pouvez choisir de démarrer à partir de la clé USB). Cela a pour effet d'ouvrir le Gestionnaire de démarrage Windows.  
  
    > [!NOTE]
    >  Reportez-vous à la documentation fournie par le fabricant de votre ordinateur pour connaître la touche de fonction donnant accès au menu du périphérique d'amorçage.  
  
2.  Le support de restauration de l'ordinateur contient des options de démarrage 32 bits (x86) et 64 bits (x64). Le Gestionnaire de démarrage Windows offre, par conséquent, le choix entre deux options : **Restauration complète du système (x86)** ou **Restauration complète du système (x64)**. Si les pilotes de l'ordinateur sont au format 32 bits, choisissez la première option (x86) ; s'ils sont au format 64 bits, choisissez la deuxième option (x64). Les fichiers Windows sont alors chargés et l'Assistant Restauration complète du système lance une vérification pour s'assurer que tous les pilotes matériels sont disponibles.  
  
3.  Dans la fenêtre **Assistant Restauration complète du système**, choisissez votre langue par défaut, puis cliquez sur la flèche.  
  
4.  Choisissez le **Format d’heure et monétaire** et le **Clavier ou méthode d’entrée** pour cet ordinateur. Cliquez sur **Continuer**.  
  
5.  Si les pilotes sont absents, le message que le processus de restauration ne peut pas vérifier les pilotes s’affiche. Cliquez sur **Fermer**, puis cliquez sur **Charger les pilotes** dans la boîte de dialogue Bienvenue.  
  
    1.  Dans la boîte de dialogue **Détecter le matériel**, clquez sur **Installer les pilotes**.  
  
    2.  Insérez le lecteur flash USB contenant les pilotes matériels, puis cliquez sur **Analyser** dans la boîte de dialogue **Installer les pilotes**.  
  
    3.  Dans la boîte de dialogue **Installer les pilotes**, cliquez sur **OK** une fois les pilotes localisés.  
  
    4.  Dans la boîte de dialogue **Détecter le matériel** , cliquez sur **Continuer**.  
  
6.  Si tous les pilotes ont été identifiés à l'issue de l'analyse initiale ou si tous les pilotes essentiels sont installés, cliquez dans la fenêtre **Restauration complète du système** sur **Continuer**.  
  
7.  Dans la page **Bienvenue dans l'Assistant Restauration complète du système**, cliquez sur **Suivant**.  
  
8.  L'assistant recherche, à présent, votre serveur.  
  
    1.  S'il ne parvient pas à le localiser, vous pouvez soit relancer la recherche, soit entrer l'adresse IP du serveur.  
  
    2.  Si plusieurs serveurs ont été détectés, vous êtes invité à en sélectionner un.  
  
    3.  Si votre serveur se trouve, le **ouvrir une session sur < Nom_de_votre_serveur\>**  page s’affiche.  
  
9. Sur le **ouvrir une session sur < Nom_de_votre_serveur\>**  , tapez *< Compte_administrateur\>*  dans le **nom d’utilisateur** zone de texte et le mot de passe dans le **mot de passe** zone de texte, puis cliquez sur **suivant**.  
  
    > [!IMPORTANT]
    >  Vous devez utiliser un compte d'administrateur qui a été créé en anglais. Si vous ne disposez pas d'un tel compte, vous devez en créer un. Pour cela, commencez par ouvrir l'onglet **Utilisateurs** dans le tableau de bord du serveur, définissez ensuite le format de langue du clavier sur Anglais, puis exécutez la tâche **Ajouter un compte d'utilisateur** pour créer le compte d'administrateur. Ensuite, utilisez le nouveau compte d'administrateur pour continuer à restaurer l'ordinateur client.  
  
10. Faites votre choix dans la page **Sélectionner un ordinateur à restaurer**, puis cliquez sur **Suivant**. Vous pouvez choisir une **< nom_ordinateur\>: (Cet ordinateur)**  ou choisissez un autre ordinateur sur le réseau à partir de la **un autre ordinateur** liste déroulante.  
  
    > [!NOTE]
    >  Si l'ordinateur est inconnu du serveur (dans le cas d'un nouvel ordinateur ou d'un ordinateur reconditionné), l'option **Cet ordinateur** n'est pas proposée.  
  
11. Dans la page **Sélectionner une sauvegarde à restaurer** , passez en revue la liste des sauvegardes disponibles et sélectionnez celle qui vous permettra de réparer l'ordinateur dans les meilleures conditions.  
  
    > [!NOTE]
    >  Il est recommandé de sélectionner une sauvegarde réussie (cochée en vert). Vous aurez ainsi la garantie que l'ensemble des fichiers système et des fichiers de données seront restaurés avec succès.  
  
12. (*Facultatif*) Sélectionnez une sauvegarde, puis cliquez sur **Détails** pour ouvrir la page **Détails sur la sauvegarde** et obtenir des informations supplémentaires au sujet de cette sauvegarde. Servez-vous des données affichées dans la page **Détails sur la sauvegarde** pour comparer plusieurs sauvegardes et déterminer celle qui répond le mieux à la situation. Cliquez sur **Fermer** dans la page **Détails sur la sauvegarde** pour revenir à la page **Sélectionner une sauvegarde à restaurer**.  
  
13. Faites votre choix dans la page **Sélectionner une sauvegarde à restaurer** , puis cliquez sur le bouton **Suivant** .  
  
14. A la page **Sélectionner une option de restauration**, cliquez sur l'une des options ci-dessous, puis cliquez sur **Suivant**.  
  
    > [!NOTE]
    >  Cette page ne s'affiche pas si le partitionnement automatique n'est pas pris en charge.  
  
    1.  **Laisser l'Assistant effectuer une restauration complète de l'ordinateur (recommandé)**. Cette option rétablit l'ordinateur dans l'état où il était juste avant la date et l'heure de la sauvegarde que vous venez de choisir. Si vous choisissez cette option, passez directement à l'étape 15.  
  
    2.  **Je vais sélectionner les volumes à restaurer (avancé)**. Cette option permet de désigner les volumes que vous avez l'intention de restaurer et d'indiquer l'emplacement de destination. Vous pouvez également créer des partitions sur le disque dur.  
  
15. Faites votre choix dans la page **Sélectionner les volumes à restaurer** .  
  
    > [!NOTE]
    >  Cette page est affichée si l'ordinateur source de la restauration ou si le lecteur de destination de la restauration a un espace de stockage inférieur à celui du lecteur source de la sauvegarde.  
  
    1.  L'assistant tente de faire correspondre les volumes source et de destination. Vérifiez que le mappage par défaut est correct.  
  
        1.  Pour désélectionner un volume, cliquez sur la flèche du menu déroulant du volume en question, puis cliquez sur **Aucun**.  
  
        2.  Lorsque vous avez terminé votre sélection, cliquez sur **Suivant**.  
  
    2.  Si le volume source et le volume de destination ont la même capacité ou si le volume source est plus petit que le volume de destination, ils sont séparés par une flèche de couleur verte. La croix rouge signale, en revanche, un problème d'incompatibilité (le volume source a une capacité supérieure à celle du volume de destination).  
  
        > [!NOTE]
        >  Une croix rouge peut également apparaître dans les cas suivants :  
        >   
        >  -   La taille de secteurs de disque du volume source volume ne correspond pas à la taille de secteurs de disque du volume de destination. Cela peut se produire si vous remplacez le disque physique avec un disque possédant une taille de secteurs différente ou si vous configurez des espaces de stockage (pouvant avoir des tailles de secteur différentes de celles du disque physique).  
        > -   Vous avez atteint le nombre limite de clusters. Pour restaurer le volume source vers le volume de destination, vous devez formater le volume de destination avec la même taille du cluster que celle du volume source. Si la taille du volume de destination est trop importante, et si la taille du cluster est trop petite, il se peut que vous atteigniez le nombre limite de clusters.  
  
        1.  Cliquez sur **Exécuter le Gestionnaire de disques (avancé)** et créez un volume de la même taille que le volume réservé au système.  
  
            > [!NOTE]
            >  Si un ordinateur client est Extensible Firmware Interface UEFI (Unified) en fonction, vous devez utiliser le **diskpart** outil permettant d’initialiser le disque du système. Pour ce faire, ouvrez une fenêtre de commande (appuyez sur Ctrl+Alt+Maj pendant 5 secondes dans l'environnement WinPE), exécutez **diskpart.exe**, puis exécutez les commandes Diskpart suivantes :  
            >   
            >  1.  **DISKPART > disque de la liste**  
            > 2.  **DISKPART > select disk #** *< disque\>*  
            > 3.  **DISKPART > Nettoyer**  
            > 4.  **DISKPART > Convertir gpt**  
            > 5.  **DISKPART > créez une partition efi taille =** *100* (où *100* est un exemple de taille de partition en Mo, doit être le même que la partition d’origine)  
            > 6.  **DISKPART > créez une partition msr taille =** *128* (où *128* est un exemple de taille de partition en Mo, doit être le même que la partition d’origine)  
            > 7.  **DISKPART > Quitter**  
  
        2.  *(Facultatif)* Sélectionnez l'option **Ne pas attribuer de lettre de lecteur ni de chemin d'accès de lecteur**.  
  
        3.  Formatez le volume au format **NTFS**.  
  
        4.  Une fois le formatage terminé, cliquez avec le bouton droit sur le nouveau volume système, puis cliquez sur **Marquer la partition comme active**.  
  
        5.  Pour faire correspondre des volumes supplémentaires aux autres volumes de la sauvegarde, recommencez les étapes *ii* à *iv* pour créer et activer les volumes, puis fermez le **Gestion des disques**.  
  
        6.  A la page **Sélectionner les volumes à restaurer** , mappez le volume réservé à la source de la restauration au volume de même capacité créé à l'étape *v*.  
  
        7.  Mappez tous les autres volumes source aux volumes de destination correspondants.  
  
        8.  Cliquez sur **Suivant** pour poursuivre la restauration.  
  
16. A la page **Confirmer les volumes à restaurer** , examinez les détails des mappages, puis cliquez sur **Suivant**. Si vous avez besoin d'effectuer des modifications, cliquez sur **Retour**, puis répétez l'étape 14.  
  
17. Le **Restoring < ComputerName\> à partir de < Date et heure de sauvegarde\>**  page signale la progression du processus de restauration.  
  
18. Lorsque la page **La restauration s'est terminée correctement** s'affiche, retirez le support de restauration, puis cliquez sur **Terminer**. L'ordinateur redémarre.  
  
    > [!IMPORTANT]
    >  Si le chiffrement de lecteur BitLocker était activé sur l'ordinateur avant que ne débute la restauration, vous devez activer BitLocker manuellement après le redémarrage de l'ordinateur.  
  
##  <a name="BKMK_FindDrivers"></a> Où puis-je trouver les pilotes à mon matériel ?  
 Selon que l'ordinateur de destination a déjà fait ou non l'objet d'une restauration, le support de restauration risque de ne pas contenir l'ensemble des pilotes de stockage et de carte réseau nécessaires au redémarrage de votre ordinateur restauré. Vous devez déterminer quels pilotes sont manquants, rechercher ces pilotes sur un support existant ou sur le site Web de s fabricant, copiez-les sur un lecteur flash, puis les copier à partir du lecteur flash vers le nouveau ou ordinateur restauré lorsque vous exécutez l’Assistant Restauration complète du système.  
  
 Lors de la sauvegarde d'un ordinateur, les pilotes installés sur cet ordinateur sont intégrés à la sauvegarde. Si votre support de restauration ne contient pas tous les pilotes dont vous avez besoin, vous pouvez ouvrir une sauvegarde de cet ordinateur et copier les pilotes sur un lecteur flash USB.  
  
#### <a name="to-copy-drivers-from-a-backup-to-a-usb-flash-drive"></a>Pour copier des pilotes à partir d'une sauvegarde sur un lecteur flash USB  
  
1.  Sur un autre ordinateur, ouvrez le Tableau de bord.  
  
2.  Cliquez sur **Périphériques**, puis cliquez sur l'ordinateur pour lequel vous avez besoin de pilotes.  
  
3.  Cliquez sur **Restaurer des fichiers ou des dossiers de l'ordinateur**. L'Assistant Restauration de fichiers ou de dossiers s'ouvre.  
  
4.  Cliquez sur la dernière sauvegarde réussie, puis cliquez sur **Suivant**.  
  
5.  Cliquez sur un volume pour l'ouvrir, puis cliquez sur **Suivant**. Une fenêtre affiche la liste des fichiers et des dossiers contenus dans la sauvegarde.  
  
6.  Insérez votre lecteur flash USB dans un connecteur USB de l'ordinateur, puis copiez le dossier Pilotes pour la restauration complète du système sur votre lecteur flash USB.  
  
    > [!NOTE]
    >  Vous devrez éventuellement cliquer sur **Dossier parent** pour atteindre la racine du volume système.  
  
7.  Retirez le lecteur flash USB, puis insérez-le dans l'ordinateur à restaurer.  
  
 Vous pouvez vous servir du lecteur flash USB pour installer les pilotes de votre ordinateur lors de sa restauration. L'Assistant Restauration de fichiers ou de dossiers vérifie automatiquement la présence de pilotes supplémentaires sur ce lecteur flash USB lorsque vous exécutez l'Assistant Restauration complète du système. Le pilote de la carte réseau et les pilotes des périphériques de stockage sont généralement ceux dont vous aurez le plus besoin.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde et restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)
