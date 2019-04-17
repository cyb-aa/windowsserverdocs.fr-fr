---
title: "Créer un DVD de récupération pour les serveurs gérés à distance"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6141fa69-5952-4e3c-a868-40ef3f4badd2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7bfe1686ac84962cdb4ab1cde8d6ca5226cb9d44
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a>Créer un DVD de récupération pour les serveurs gérés à distance

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

##  <a name="BKMK_HeadlessRecovery"></a>Créer un DVD de récupération pour les serveurs gérés à distance  
 Il existe deux modèles pour la récupération de serveur et réinitialisation en usine, et elles diffèrent selon le matériel reçu par le client.  
  
 **Serveur administré à distance**  
  
 Cette option de récupération du serveur nécessite que le processus est exécuté à partir d’un ordinateur client sur le même réseau. Réinitialisation d’usine nécessitant qu’une image spécifique au matériel soit livrée avec le serveur, le partenaire doit créer le DVD de récupération.  
  
 **Serveur géré localement**  
  
 Il s’agit de la procédure classique dans lequel le serveur est administré à la console du serveur. Le support d’installation de serveur est utilisé pour exécuter une récupération. Cela nécessite que le serveur est fourni avec la possibilité d’afficher la sortie vidéo en plus, y compris un lecteur de DVD. Le client démarre à partir du support d’installation de ce serveur, puis choisit la méthode de récupération appropriée. Vous n’avez pas besoin créer un DVD de récupération pour les serveurs gérés localement.  
  
> [!NOTE]
>  Pour le modèle de serveur géré localement, le client peut atteindre d’usine en effectuant une nouvelle installation. Toutefois, ils sont perdues le partenaire de personnalisations.  
  
 Il existe deux types de récupération du serveur.  
  
 **Réinitialisation d’usine**  
  
 Cette récupération renvoie le serveur à l’état d’origine qui existaient lorsque le serveur a été expédié d’usine. Après une réinitialisation d’usine, vous êtes invité à effectuer la configuration initiale du serveur, tout comme vous ont été la première fois que vous l’avez allumé, et toutes les personnalisations et paramètres sont perdues. Cela est également appelé jour 0.? Réinitialisation d’usine nécessitant qu’une image spécifique au matériel soit livrée avec le serveur, le partenaire doit créer le DVD de récupération.  
  
 **La restauration complète**  
  
 Cette récupération suppose que vous avez configuré une sauvegarde du serveur et que la sauvegarde du serveur terminée avec succès au moins une fois avant la défaillance du serveur. La restauration complète (BMR) prend en charge la récupération des lecteurs système et de démarrage uniquement à partir d’une sauvegarde antérieure du serveur.  
  
 Après un récupération complète du système, le serveur est renvoyé à l’état où se trouvait au moment de la sauvegarde qui est utilisée pour la restauration. Il s’agit généralement de la dernière sauvegarde, mais dans certains cas, il peut être une sauvegarde antérieure. Récupération de données est effectuée après que le système est restauré à l’aide de l’Assistant de dossiers et de restauration de fichiers. Récupération complète du système est la méthode de récupération de serveur par défaut, car tous les paramètres et configuration sont retournées, alors qu’une réinitialisation d’usine renvoie le serveur à un état jour 0.  
  
### <a name="remotely-administered-server-recovery"></a>Récupération de serveur à distance  
 Cette section décrit les personnalisations requises que le partenaire doit effectuer et le support final qui doit être livré avec chaque serveur. Avant d’aborder les détails, examinons l’expérience utilisateur.  
  
 Dans ce scénario, le client «la s serveur ne fonctionne plus. Cela peut être dû à un virus, une défaillance de disque dur ou toute autre chose. Le client insère le DVD de récupération dans un ordinateur client qui se trouve sur le même réseau que le serveur. L’application de récupération de serveur guide tout au long les trois étapes pour récupérer leur serveur:  
  
1.  Crée un lecteur flash USB qui est utilisé pour redémarrer le serveur en mode de récupération. Le lecteur flash USB doit être de 8Go ou plus.  
  
2.  Détecte le serveur est maintenant en mode de récupération.  
  
3.  Permet au client choisir une réinitialisation d’usine ou une restauration complète, puis retourne leur serveur à un état opérationnel.  
  
### <a name="steps-for-creating-a-server-recovery-dvd-for-multiple-language-support"></a>Prise en charge des étapes de création d’un DVD de récupération pour plusieurs langues  
 Il existe six étapes principales pour créer un DVD de récupération  
  
1.  [(Facultatif) Mise à jour WinPE](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Updating)  
  
2.  [Collecter les images de réinitialisation d’usine et les fichiers XML](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Collecting)  
  
3.  [Créer le DVD de récupération](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Creating)  
  
4.  [Personnaliser l’Assistant](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Customizing)  
  
5.  [Créer le fichier ISO](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_CreatingISO)  
  
6.  [Tester le DVD de récupération](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Testing)  
  
####  <a name="BKMK_Updating"></a>Étape1: (Facultatif) mettre à jour WinPE  
 Le kit ADK inclut une copie de WindowsPE personnalisée. Lorsque cette image est démarrée, il lance automatiquement la balise qui est utilisée par l’application de récupération du client pour se connecter à un serveur en mode de récupération.  
  
 WindowsPE doit être personnalisés en ajoutant des pilotes spécifiques au matériel, par exemple, réseau ou les pilotes de contrôleur de disque. Après le démarrage de WinPE, les disques durs sur le système doivent être reconnaissables et le réseau doit fonctionner correctement.  
  
####  <a name="BKMK_Collecting"></a>Étape2: Recueillir les images de réinitialisation d’usine et les fichiers XML  
 Pour réinitialiser un serveur par défaut, les deux images suivantes doivent être capturées:  
  
-   L’image du lecteur système  
  
-   La partition réservée du système  
  
 Pour capturer ces images, l’outil GenDiskXML.exe est fourni. GenDiskXML.exe collecte également un fichier nommé disk.xml qui est utilisé par le processus de récupération pour recréer la configuration du disque.  
  
1.  Après avoir exécuté Sysprep, redémarrez le système à l’aide de n’importe quelle version64bits de WindowsPE.  
  
2.  Pour exporter les fichiers .xml et .wim dans une source externe, exécutez `GenDiskXML /outputdir:<path>`pour les fichiers .xml et .wim dans n’importe quelle source externe. Les fichiers sont ajoutés au DVD à l’étape suivante.  
  
    > [!NOTE]
    >  Le fichier .wim système sera fractionné pour respecter le format FAT32 d’aucun fichier de plus de 4Go. Pendant le processus, la capacité requise de la cible qui est utilisée pour capturer les fichiers .wim doit être supérieure à 8Go pour le processus de fractionnement fonctionne.  
  
####  <a name="BKMK_Creating"></a>Étape3: Créer le DVD de récupération  
 À partir de l’usine, chaque serveur doit être accompagné d’un DVD de récupération. Votre DVD d’outils du kit ADK comprend les fichiers nécessaires à la création du DVD.  
  
###### <a name="to-create-the-server-recovery-dvd"></a>Pour créer le DVD de récupération  
  
1.  Créer un dossier de travail à utiliser comme emplacement pour stocker l’image ISO finale.  
  
2.  À partir du CD du partenaire, copiez le contenu de la **de récupération de serveur** dossier dans le dossier de travail que vous avez créé à l’étape1.  
  
3.  Copiez le fichier disk.xml créé lors de l’exécution de GenDiskXML.exe dans le **réinitialiser** dossier.  
  
4.  Copiez les fichiers d’image qui ont été créés lorsque vous avez exécuté **GenDiskXML.exe** à la **réinitialiser** dossier. Les fichiers sont les fichiers .wim et .swm, et le nombre de fichiers peut-être varier.  
  
5.  Supprimez GenDiskXML.exe du dossier. Il est utilisé uniquement à des fins de paramétrage usine, et il ne doit pas être inclus sur le DVD fourni au client.  
  
####  <a name="BKMK_Customizing"></a>Étape4: Personnaliser l’Assistant  
 L’application de récupération de serveur doit être personnalisée avec une image de l’appareil et le texte qui décrit comment démarrer l’appareil spécifique en mode de récupération. Cette page de l’Assistant de dossiers et de restauration de fichiers étant spécifiques au matériel, les étapes pour démarrer le serveur en mode de récupération varient.  
  
> [!NOTE]
>  Les noms de fichiers qui sont répertoriés doivent correspondre exactement.  
  
1.  La page de l’Assistant propose un lien pour obtenir une aide supplémentaire. Si ce fichier .chm existe, il remplace le FWLink de l’aide en ligne. Le fichier d’aide est situé:  
  
     < root DVD > \\$OEM$\Customization\\ < culture nom\ > \RestartHelp.chm  
  
2.  Ce fichier contient le texte que le client voit sur la page de l’Assistant. Le texte doit expliquer comment démarrer le serveur en mode de récupération. Le contrôle est interactif, ce qui place une limite pratique sur la quantité de texte qui peut être ajouté.  
  
     Le fichier suivant est utilisé pour remplacer l’image de l’Assistant, et il est principalement sur la personnalisation. Il doit être un fichier .png. La taille du fichier doit être x 256 x 256pixels ou éviter lorsqu’elle est affichée dans l’Assistant.  
  
     < root DVD > \\$OEM$\Customization\\ < culture nom\ > \RestartInstructions.rtf  
  
3.  < root DVD > \\$OEM$\Customization\\ < culture nom\ > \ServerImage.png  
  
 Lorsque vous convertissez votre DVD pour prendre en charge plusieurs langues de récupération, assurez-vous que vous procédez comme suit:  
  
1.  Vous devez toujours avoir l’en-us dossier. Si l’application de récupération du serveur ne trouve pas les fichiers spécifiques à la culture qui correspondent à l’ordinateur client, il est en cours d’exécution sur, il revient à en-us.  
  
2.  Dans chaque dossier de culture que vous créez, ajoutez les trois fichiers de personnalisation (.png, .chm et .rtf).  
  
3.  Copiez les deux dossiers d’à partir de la langue Packs\\ < cultureName\ > \Server récupération à la racine du DVD de récupération. Par exemple: les dossiers ES et ES-ES sont copiés à la racine du DVD pour prendre en charge espagnol.  
  
4.  Finalisez le fichier ISO.  
  
 Les noms de culture prise en charge sont les suivantes:  

|-|-|  
|-Cs-CZ<br /><br /> -De-DE<br /><br /> -En-US<br /><br /> -Es-ES<br /><br /> -Fr-FR<br /><br /> -Hu-HU<br /><br /> -It-IT<br /><br /> -Ja-JP<br /><br /> -Ko-KR<br /><br /> -Nl-NL |-pl-PL<br /><br /> -Pt-BR<br /><br /> -Pt-PT<br /><br /> -Ru-RU<br /><br /> -Sv-SE<br /><br /> -Tr-TR<br /><br /> -Zh-CN<br /><br /> -Zh-HK<br /><br /> -Zh-TW
  
####  <a name="BKMK_CreatingISO"></a>Étape5: Créer le fichier ISO  
 Le dossier qui a été créé et tout le contenu peut être gravé sur un DVD. Il s’agit du DVD qui sera fourni aux clients avec leur nouveau serveur.  
  
####  <a name="BKMK_Testing"></a>Étape6: Tester le DVD de récupération  
 Après avoir effectué l’installation du serveur, configurer la sauvegarde du serveur, exécuter une sauvegarde du serveur, puis testez le DVD de récupération.  
  
###### <a name="to-configure-and-run-a-server-backup"></a>Pour configurer et exécuter une sauvegarde du serveur  
  
1.  Ouvrez le tableau de bord, cliquez sur le **périphériques** onglet, puis cliquez sur **configurer la sauvegarde pour le serveur** dans les **tâches** volet.  
  
2.  Suivez les instructions de l’Assistant pour configurer une sauvegarde du serveur de sauvegarde. Vous avez besoin d’un disque dur externe pour la sauvegarde.  
  
3.  Cliquez sur **démarrer une sauvegarde pour le serveur** dans les **tâches** volet, puis suivez les instructions de l’Assistant.  
  
4.  Une fois la sauvegarde terminée, vérifiez si elle a réussi.  
  
###### <a name="to-restore-a-server"></a>Pour restaurer un serveur  
  
1.  Insérez le DVD de récupération que vous avez créé dans un ordinateur client qui est connecté au même réseau que le serveur via un concentrateur ou un commutateur.  
  
2.  Double-cliquez sur **setup.exe**. L’Assistant Récupération de serveur vous invite par le biais du même processus que le client.  
  
3.  Cliquez sur **restaurer le serveur à partir d’une sauvegarde**, puis suivez les instructions de l’Assistant.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)