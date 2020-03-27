---
title: Créer un DVD de récupération de serveur pour les serveurs gérés à distance
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6141fa69-5952-4e3c-a868-40ef3f4badd2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a9b571c2d3e5d531d8c923741500c72675022adc
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312192"
---
# <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a>Créer un DVD de récupération de serveur pour les serveurs gérés à distance

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a><a name="BKMK_HeadlessRecovery"></a>Créer un DVD de récupération de serveur pour les serveurs gérés à distance  
 Il existe deux façons de procéder selon le matériel reçu par le client.  
  
 **Serveur administré à distance**  
  
 Cette option de récupération du serveur exige que la procédure soit exécutée à partir d’un ordinateur client appartenant au même réseau. La réinitialisation des paramètres d’usine nécessitant qu'une image spécifique au matériel soit livrée avec le serveur, le partenaire doit créer le DVD de récupération.  
  
 **Serveur géré localement**  
  
 Il s’agit de la procédure classique qui consiste à gérer le serveur à partir de la console serveur. Le support d’installation du serveur est utilisé pour exécuter une récupération. Le serveur doit être en mesure d’afficher la sortie vidéo et être équipé d’un lecteur de DVD. Le client démarre à partir de ce support d’installation du serveur, puis choisit la méthode de récupération appropriée. Il est inutile de créer un DVD de récupération de serveur pour les serveurs gérés localement.  
  
> [!NOTE]
>  Pour le modèle de serveur géré localement, le client peut éventuellement procéder à la réinitialisation des paramètres d’usine en effectuant une nouvelle installation. Cependant, les personnalisations effectuées par le partenaire seront perdues.  
  
 Il existe deux types de récupération du serveur.  
  
 **Réinitialisation aux paramètres d’usine**  
  
 Ce type de récupération rétablit l’état initial du serveur (tel qu’il était à la sortie d’usine). Suite à une réinitialisation aux paramètres d'usine, vous êtes invité à effectuer la configuration initiale du serveur comme la première fois que vous l'avez allumé, tous les réglages et les personnalisations étant perdues. On parle également de jour 0. La réinitialisation des paramètres d’usine nécessitant qu'une image spécifique au matériel soit livrée avec le serveur, le partenaire doit créer le DVD de récupération.  
  
 **Restauration complète**  
  
 Ce type de récupération suppose que vous avez configuré la sauvegarde du serveur et que celle-ci a réussi au moins une fois avant la défaillance du serveur. La restauration complète (BMR) prend en charge la récupération des lecteurs système et de démarrage uniquement à partir d’une sauvegarde antérieure du serveur.  
  
 Suite à une restauration complète (BMR), le serveur retourne à l'état où il se trouvait au moment de la sauvegarde utilisée pour la restauration. Il s’agit généralement de la sauvegarde la plus récente, mais dans certains cas, cela peut être une sauvegarde antérieure. La récupération de données se fait une fois le système restauré à l’aide de l’assistant de restauration des fichiers et des dossiers. La restauration complète (BMR) est la méthode de récupération de serveur préférée, car le serveur retrouve tous ses paramètres et sa configuration, alors qu'un paramétrage usine le ramène à l'état de serveur au Jour 0.  
  
### <a name="remotely-administered-server-recovery"></a>Récupération de serveur à distance  
 Cette section décrit les personnalisations requises que le partenaire doit effectuer et le support final qui doit être livré avec chaque serveur. Avant d’étudier cela en détail, intéressons-nous à l’expérience du client.  
  
 Dans ce scénario, le client «¢ s Server ne fonctionne plus. Cela peut être lié à un virus, à une panne de disque dur ou à toute autre chose. Le client insère le DVD de récupération du serveur dans un ordinateur client situé sur le même réseau que le serveur, puis suit les trois étapes proposées par le programme de récupération du serveur :  
  
1.  Création d’un lecteur flash USB amorçable pour démarrer le serveur en mode de récupération. Le lecteur flash USB doit avoir une capacité minimale de 8 Go.  
  
2.  Détection du serveur qui est maintenant en mode de récupération.  
  
3.  Choix de la méthode (réinitialisation des paramètres d’usine ou restauration complète) pour remettre le serveur en état de marche.  
  
### <a name="steps-for-creating-a-server-recovery-dvd-for-multiple-language-support"></a>Procédure de création d’un DVD de récupération pour la prise en charge de plusieurs langues  
 Six étapes sont nécessaires pour créer un DVD de récupération.  
  
1.  [Facultatif Mettre à jour WinPE](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Updating)  
  
2.  [Collecter les images de réinitialisation des paramètres d’usine et les fichiers XML](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Collecting)  
  
3.  [Créer le DVD de récupération du serveur](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Creating)  
  
4.  [Personnaliser l’Assistant](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Customizing)  
  
5.  [Créer le fichier ISO](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_CreatingISO)  
  
6.  [Test du DVD de récupération](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Testing)  
  
####  <a name="step-1-optional-update-winpe"></a><a name="BKMK_Updating"></a>Étape 1 : (facultatif) mettre à jour WinPE  
 Le Kit de déploiement et d’évaluation inclut une copie de Windows PE personnalisée. Le démarrage de cette image a pour effet de lancer automatiquement la balise utilisée par le programme de récupération du client pour se connecter à un serveur en mode de récupération.  
  
 Il est nécessaire de personnaliser davantage Windows PE en ajoutant des pilotes spécifiques au matériel tels que les pilotes du contrôleur de réseau ou de disque. Après le démarrage de WinPE, les disques durs sur le système doivent être reconnaissables et le réseau doit fonctionner correctement.  
  
####  <a name="step-2-collect-the-factory-reset-images-and-xml-files"></a><a name="BKMK_Collecting"></a>Étape 2 : collecter les images de réinitialisation des paramètres d’usine et les fichiers XML  
 Pour rétablir les paramètres par défaut d’un serveur, les deux images suivantes doivent être capturées :  
  
- L'image du lecteur système  
  
- La partition réservée du système  
  
  Un outil a été prévu spécialement pour capturer ces images : GenDiskXML.exe. GenDiskXML.exe collecte également un fichier nommé disk.xml, utilisé par le processus de récupération pour recréer la configuration du disque.  
  
1.  Après avoir exécuté Sysprep, redémarrez le système en utilisant une version 64 bits quelconque de Windows PE.  
  
2.  Pour placer les fichiers .xml et .wim sur une source externe, exécutez `GenDiskXML /outputdir:<path>` . Les fichiers sont ajoutés au DVD à l'étape suivante.  
  
    > [!NOTE]
    >  Le fichier .wim système sera fractionné pour respecter la limite de 4 Go par fichier imposée par le format FAT32. La cible réservée à la capture des fichiers .wim doit, cependant, être supérieure à 8 Go pour que le processus de fractionnement fonctionne.  
  
####  <a name="step-3-create-the-server-recovery-dvd"></a><a name="BKMK_Creating"></a>Étape 3 : créer le DVD de récupération du serveur  
 À la sortie de l’usine, chaque serveur doit être livré avec un DVD de récupération du serveur. Votre DVD d'outils du Kit de déploiement et d’évaluation inclut les fichiers nécessaires à la création du DVD.  
  
###### <a name="to-create-the-server-recovery-dvd"></a>Pour créer le DVD de récupération du serveur  
  
1.  Créez un dossier de travail réservé au stockage de l’image ISO finale.  
  
2.  À partir du CD du partenaire, copiez les contenus du dossier **Restauration du serveur** dans le dossier de travail que vous avez créé à l'étape 1.  
  
3.  Copiez le fichier disk.xml créé lors de l’exécution de GenDiskXML.exe dans le dossier **Reset**.  
  
4.  Copiez les fichiers d'images créés lorsque vous avez exécuté **GenDiskXML.exe** dans le dossier **Reset** . Il s’agit de fichiers .wim et .swm dont le nombre varie.  
  
5.  Supprimez GenDiskXML.exe du dossier. Cet outil, utilisé à des fins de paramétrage usine, ne doit pas figurer sur le DVD fourni au client final.  
  
####  <a name="step-4-customize-the-wizard"></a><a name="BKMK_Customizing"></a>Étape 4 : personnaliser l’Assistant  
 Il est nécessaire de personnaliser le programme de récupération du serveur en lui ajoutant une image du périphérique et une explication sur la façon de démarrer le périphérique en question en mode de récupération. Cette page de l’assistant de restauration de fichiers et de dossiers étant spécifique à chaque matériel, les étapes à suivre peuvent varier.  
  
> [!NOTE]
>  Les noms de fichiers énumérés doivent correspondre exactement.  
  
1. La page de l'assistant propose un lien pour une l'aide supplémentaire. Si ce fichier .chm existe, il remplacera le lien FWLink de l’aide en ligne. Le fichier d'aide est situé au :  
  
    <\>racine du DVD \\$OEM $ \Customization\\< nom de la culture\>\RestartHelp.chm  
  
2. Ce fichier contient le texte que le client voit sur la page de l'assistant. Il doit décrire comment démarrer le serveur en mode de récupération. Ce texte est placé dans une zone qu’il est possible de faire défiler.  
  
    Le fichier suivant, destiné à remplacer l’image de l’assistant, porte essentiellement sur la personnalisation. Il doit s’agir d’un fichier .png. Pour éviter qu’il soit recadré au moment de son affichage dans l’assistant, veillez à ce que sa taille ne dépasse pas 256 pixels x 256 pixels.  
  
    <\>racine du DVD \\$OEM $ \Customization\\< nom de la culture\>\RestartInstructions.rtf  
  
3. <\>racine du DVD \\$OEM $ \Customization\\< nom de la culture\>\ServerImage.png  
  
   Lors de la conversion du DVD de récupération du serveur en vue de prendre en charge plusieurs langues, prenez soin de respecter les consignes suivantes :  
  
4. Préservez toujours le dossier en-us. Si le programme de récupération du serveur ne trouve pas les fichiers spécifiques à la culture correspondant à l’ordinateur client sur lequel il est exécuté, il repasse automatiquement en mode en-US.  
  
5. Dans chaque dossier de culture que vous créez, ajoutez les trois fichiers de personnalisation (.png, .chm et .rtf).  
  
6. Copiez les deux dossiers de culture à partir des modules linguistiques\\< CultureName\>la récupération \Serveur à la racine du DVD de récupération du serveur. Par exemple : dossiers ES et ES-ES sont copiés à la racine du DVD pour prendre en charge l’espagnol.  
  
7. Finalisez le fichier ISO.  
  
   Les noms de la culture prise en charge incluent :  

|-|-|  
|-CS-CZ<br /><br /> -de<br /><br /> -fr-fr<br /><br /> -es-ES<br /><br /> -fr-FR<br /><br /> -HU-HU<br /><br /> -IT-IT<br /><br /> -ja-JP<br /><br /> -ko-KR<br /><br /> -NL-NL |-PL-PL<br /><br /> -PT-BR<br /><br /> -PT-PT<br /><br /> -ru-RU<br /><br /> -VP-SE<br /><br /> -TR-TR<br /><br /> -zh-CN<br /><br /> -zh-HK<br /><br /> -zh-TW
  
####  <a name="step-5-create-the-iso-file"></a><a name="BKMK_CreatingISO"></a>Étape 5 : créer le fichier ISO  
 Le dossier créé ainsi que la totalité de son contenu peuvent être gravés sur un DVD. Il s'agit du DVD qui sera fourni aux clients avec leur nouveau serveur.  
  
####  <a name="step-6-test-the-recovery-dvd"></a><a name="BKMK_Testing"></a>Étape 6 : tester le DVD de récupération  
 Une fois l’installation du serveur terminée, configurez la sauvegarde du serveur, exécutez une sauvegarde du serveur, puis testez le DVD de récupération.  
  
###### <a name="to-configure-and-run-a-server-backup"></a>Pour configurer et exécuter une sauvegarde du serveur  
  
1.  Ouvrez le tableau de bord, cliquez sur l’onglet **Périphériques**, puis cliquez sur **Configurer la sauvegarde pour le serveur** dans le volet **Tâches**.  
  
2.  Suivez les instructions de l’assistant pour configurer une sauvegarde du serveur. Vous avez besoin d'un disque dur externe pour la sauvegarde.  
  
3.  Cliquez sur **Démarrer une sauvegarde pour le serveur** dans le volet **Tâches**, puis suivez les instructions de l'assistant.  
  
4.  Lorsque la sauvegarde se termine, vérifier sa réussite.  
  
###### <a name="to-restore-a-server"></a>Pour restaurer un serveur  
  
1.  Insérez le DVD de récupération créé sur un ordinateur client connecté au même réseau que le serveur via un concentrateur ou un commutateur.  
  
2.  Double-cliquez sur **setup.exe**. Laissez-vous guider par l’assistant de récupération du serveur (le processus est le même que pour le client).  
  
3.  Cliquez sur **Restaurer le serveur à partir d'une sauvegarde**, puis suivez les instructions de l'assistant.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)