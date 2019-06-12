---
title: Résolution des problèmes de la Gestion des disques
description: Cet article explique comment résoudre les problèmes liés à l’outil Gestion des disques
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4d9448cc642ef522fa129dcfe97e2286f16bad1b
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812533"
---
# <a name="troubleshooting-disk-management"></a>Résolution des problèmes de la Gestion des disques

> **S’applique à :** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique répertorie quelques uns des problèmes courants que vous pouvez rencontrer lors de l’utilisation de l’outil Gestion des disques.

> [!TIP]
> Si vous obtenez une erreur ou quelque chose ne fonctionne pas lorsque vous suivez ces procédures, ne vous affolez pas ! Il existe une multitude d’informations sur la [Communauté Microsoft](https://answers.microsoft.com/en-us/windows) site - essayez de rechercher le [fichiers, dossiers et stockage](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) section et si vous avez besoin d’aide, publiez une question et de Microsoft ou d’autres membres de la Communauté va tenter de vous aider à. Si vous avez des commentaires sur la façon d’améliorer ces rubriques, nous aimerions connaître votre opinion ! Répondez simplement le *sert cette page ?* invite et laisser des commentaires de cet emplacement ou dans le thread de commentaires publics en bas de cette rubrique.

## <a name="a-disks-status-is-not-initialized-or-the-disk-is-missing"></a>État d’un disque n’est pas initialisé ou le disque est manquant

![Gestion des disques montrant un disque inconnu qui doit être initialisé.](media/uninitialized-disk.PNG)

**Cause :** Si vous avez un disque qui n’apparaît pas dans l’Explorateur de fichiers et est répertorié dans la gestion des disques en tant que *pas initialisé*, cela peut signifier que le disque n’a pas une signature de disque valide. En fait, cela signifie que le disque n’a jamais été initialisé et formaté, ou la mise en forme du lecteur a été endommagée d’une certaine manière. 

Il est également possible que le disque rencontre des problèmes de matériel ou branché, mais nous le verrons dans quelques paragraphes.

**Solution :**   si le lecteur est toute nouvel et doit simplement être initialisé, effacer toutes les données, la solution est facile, consultez [initialiser de nouveaux disques](initialize-new-disks.md). Toutefois, il est probable que vous avez déjà essayé et il n’a pas fonctionné. Ou vous disposez peut-être d’un disque plein de fichiers importants, et vous ne souhaitez pas effacer le disque par son initialisation.

Il existe un ensemble de raisons pour un disque peut être manquant ou ne parviennent pas à initialiser, avec un motif courantes étant, car le disque est défectueux. Uniquement pour plus vous pouvez faire pour résoudre un disque défaillant, mais voici quelques étapes à essayer de voir si nous pouvons obtenir cela fonctionne à nouveau. Si le disque fonctionne après une de ces étapes, ne s’embêter avec les étapes suivantes, simplement brille, Célébrez et peut-être mettre à jour de vos sauvegardes.

1. Examinez le disque dans Disk Management. S’il apparaît *hors connexion* comme indiqué ici, essayez dessus et en sélectionnant **Online**.

    ![Disque indiqué comme étant hors connexion](media/offline-disk.png)
2. Si le disque s’affiche dans la gestion des disques en tant que *Online*, et a une partition principale est répertoriée en tant que *sain*, comme illustré ici, qui est un bon signe.

    ![Disque indiqué comme en ligne avec un volume sain](media/healthy-volume.png)
    - Si la partition a un système de fichiers, mais aucune lettre de lecteur (par exemple, E:), consultez [modifier une lettre de lecteur](change-a-drive-letter.md) pour ajouter une lettre de lecteur manuellement.
    - Si elle n’a pas un système de fichiers (NTFS, ReFS, FAT32 ou exFAT) et que vous savez que le disque est vide, cliquez sur la partition, puis sélectionnez **Format**. Mise en forme d’un disque efface toutes les données qu’il contient, donc vous ne le faites pas cela si vous tentez de récupérer des fichiers à partir du disque - au lieu de cela, passez à l’étape suivante.
3. Si vous avez un externe du disque, débranchez le disque, branchez-le dans et puis **Action** > **relancer l’analyse des disques**. 
4. Arrêter votre PC, mettre hors tension de votre disque dur externe (s’il est un disque externe avec un cordon d’alimentation), puis rallumez votre PC et le disque.
    Pour désactiver votre PC dans Windows 10, sélectionnez le bouton Démarrer, sélectionnez le bouton d’alimentation, puis **arrêter**.
5. Branchez le disque sur un autre port USB qui se trouve directement sur votre PC (pas sur un concentrateur).
    Parfois les disques USB n’obtenir suffisamment de puissance à partir de certains ports ou d’autres problèmes avec des ports particuliers. Cela est particulièrement courant avec les concentrateurs USB, mais parfois, il existe des différences entre les ports sur un PC, donc essayer quelques ports différents si vous les avez.
6. Essayez un câble différent.
    Il peut sembler étrange, mais échouer beaucoup les câbles, par conséquent, essayez d’utiliser un câble différent pour brancher le disque. Si vous avez un disque interne dans un PC de bureau, vous devrez probablement arrêter votre ordinateur avant de passer des câbles - consultez le manuel de votre PC pour plus d’informations.
7. Vérifiez le Gestionnaire de périphériques pour les problèmes.
    Appuyez et maintenez (ou avec le bouton droit) le bouton Démarrer, puis sélectionnez le Gestionnaire de périphériques dans le menu contextuel. Recherchez tous les appareils avec un point d’exclamation à côté ou d’autres problèmes, double-cliquez sur l’appareil et lisez son état.

    Voici une liste de [codes d’erreur dans le Gestionnaire de périphériques](https://support.microsoft.com/help/310123/error-codes-in-device-manager-in-windows), mais une approche est parfois fonctionne avec le bouton droit de l’appareil problématique, sélectionnez **désinstallation appareil**, puis **Action**  >  **Rechercher les modifications de matériel**.

    ![Gestionnaire de périphériques affiche un périphérique USB inconnu](media/device-manager.PNG)
8. Branchez le disque sur un autre PC.
    
    Si le disque ne fonctionne pas sur un autre PC, il est un bon signe qu’il y a quelque chose de mauvais passe sur le disque et pas sur votre PC. Aucun amusants, que nous savons. Certaines étapes supplémentaires que vous pouvez essayer dans [erreur du lecteur USB externe « Vous devez initialiser le disque avant que le Gestionnaire de disque logique peut y accéder »](https://social.technet.microsoft.com/Forums/windows/en-US/2b069948-82e9-49ef-bbb7-e44ec7bfebdb/forum-faq-external-usb-drive-error-you-must-initialize-the-disk-before-logical-disk-manager-can?forum=w7itprohardware), mais il peut être le temps à rechercher et demander de l’aide à la [delaCommunautéMicrosoft](https://answers.microsoft.com/en-us/windows) de site, ou contactez le fabricant de votre disque.

    Si vous ne pouvez pas simplement obtenez son fonctionnement, il existe également des applications qui peuvent essayer de récupérer des données à partir d’un disque défectueux, ou si les fichiers sont très importantes, vous pouvez payer un laboratoire de récupération de données pour essayer de les récupérer. Si vous trouvez quelque chose qui fonctionne pour vous, faites-le nous savoir dans la section commentaires ci-dessous.

> [!IMPORTANT]
> Disques échouent assez souvent, il est donc important de sauvegarder régulièrement les fichiers qui que vous intéressent. Si vous avez un disque parfois n’apparaît pas, ou génère des erreurs, considérer ceci comme un rappel pour vérifier vos méthodes de sauvegarde. Il est OK si vous êtes un peu en retard, nous avons tous été là. La meilleure solution de sauvegarde étant celui que vous utilisez, nous vous encourageons à en trouver un qui fonctionne pour vous et gardez-le.
> 
> [!TIP]
> Pour plus d’informations sur l’utilisation des applications intégrées à Windows pour les fichiers de sauvegarde sur un lecteur externe tel qu’un lecteur USB, consultez [sauvegarder et restaurer vos fichiers](https://support.microsoft.com/help/17143/windows-10-back-up-your-files). Vous pouvez également enregistrer des fichiers dans OneDrive de Microsoft, qui synchronise les fichiers à partir de votre PC vers le cloud. Si votre disque dur tombe en panne, vous serez toujours en mesure d’obtenir les fichiers que vous stockez dans OneDrive à partir de OneDrive.com. Pour plus d’informations, consultez [OneDrive sur votre PC](https://support.microsoft.com/help/17184/windows-10-onedrive).

## <a name="a-basic-or-dynamic-disks-status-is-unreadable"></a>L’état d’un disque de base ou dynamique est illisible

**Cause :**  le disque de base ou dynamique n’est pas accessible et peut avoir subi défaillance matérielle, de corruption ou d’erreurs d’e/s. La copie du disque de la base de données de configuration du disque du système est peut-être endommagée. Une icône d’erreur s’affiche sur les disques dont l’état est **Illisible**.

Les disques peuvent également afficher l’état **Illisible** pendant qu’ils sont en rotation ou lorsque Gestion des disques effectue une nouvelle analyse de tous les disques du système. Dans certains cas, un disque illisible échoue et est irrécupérable. Pour les disques dynamiques, l’état **Illisible** est généralement dû à l’endommagement ou des erreurs d’E/S sur une partie du disque, plutôt qu’à l’échec de l’ensemble du disque.

**Solution :**  réanalyser les disques ou redémarrez l’ordinateur pour voir si l’état du disque change. Essayez également les étapes de dépannage décrites dans [l’état d’un disque n’est pas initialisé ou le disque est manquant entièrement](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="a-dynamic-disks-status-is-foreign"></a>L’état d’un disque dynamique est étranger

**Cause :**  le **étrangères** état se produit lorsque vous déplacez un disque dynamique sur l’ordinateur local à partir d’un autre ordinateur PC. Une icône d’avertissement s’affiche sur les disques dont l’état est **Étranger**.

Dans certains cas, un disque qui était connecté au système auparavant peut afficher l’état **Étranger**. Les données de configuration des disques dynamiques sont stockées sur tous les disques dynamiques. Par conséquent, les informations sur les disques détenus par le système sont perdues en cas d’échec de tous les disques dynamiques.

**Solution :**  ajouter le disque à la configuration du système de votre ordinateur afin que vous pouvez accéder aux données sur le disque. Pour ajouter un disque à la configuration système de votre ordinateur, importez le disque étranger (cliquez avec le bouton droit sur le disque, puis cliquez sur **Importer des disques étrangers**). Lorsque vous importez le disque, les volumes existants du disque étranger deviennent visibles et accessibles. 

## <a name="a-dynamic-disks-status-is-online-errors"></a>État d’un disque dynamique est en ligne (erreurs)

**Cause :**  disque dynamique contient des erreurs d’e/s sur une zone du disque. Une icône d’avertissement s’affiche sur le disque dynamique comportant des erreurs.

**Solution :**   si les erreurs d’e/s sont temporaires, réactivez le disque pour rétablir les **Online** état.

## <a name="a-dynamic-disks-status-is-offline-or-missing"></a>État d’un disque dynamique est hors connexion ou manquant

**Cause :**  un **hors connexion** disque dynamique peut être endommagé ou temporairement indisponible. Une icône d’erreur s’affiche sur le disque dynamique hors connexion.

Si l’état du disque est **Hors connexion** et que le nom du disque devient **Manquant**, cela signifie que le disque était récemment disponible sur le système, mais qu’il ne peut plus être localisé ou identifié. Le disque manquant peut être endommagé, hors tension ou déconnecté.

**Solution :** Pour réactiver un disque qui est hors connexion et n’atteint pas en ligne :

1. Résolvez les problèmes éventuels liés au disque, au contrôleur ou aux câbles. 
2. Vérifiez que le disque physique est sous tension, branché et connecté à l’ordinateur. 
3. Ensuite, utilisez la commande **Réactiver le disque** pour remettre le disque en ligne.
4. Essayez les étapes de dépannage décrites dans [l’état d’un disque n’est pas initialisé ou le disque est manquant entièrement](#a-disks-status-is-not-initialized-or-the-disk-is-missing).
5. Si l’état du disque reste **Hors connexion**, que le nom du disque est toujours **Manquant** et que vous constatez qu’un problème affectant le disque ne peut pas être résolu, vous pouvez supprimer le disque du système en cliquant dessus avec le bouton droit, puis en cliquant sur **Supprimer le disque**). Toutefois, avant de pouvoir supprimer le disque, vous devez supprimer tous les volumes (ou miroirs) sur celui-ci. Vous pouvez enregistrer tous les volumes en miroir présents sur le disque en supprimant le miroir au lieu de l’intégralité du volume. La suppression d’un volume détruit les données du volume. Par conséquent, vous devez supprimer un disque uniquement si vous êtes absolument certain que le disque est définitivement endommagé et inutilisable.

**Pour réactiver un disque hors connexion et est également appelé disque \# (non manquant), essayez une ou plusieurs des procédures suivantes :**

1. Dans Gestion des disques, cliquez avec le bouton droit sur le disque, puis cliquez sur **Réactiver le disque** pour le remettre en ligne. Si l’état du disque reste **Hors connexion**, vérifiez les câbles et le contrôleur de disque et assurez-vous que le disque physique est sain. Corrigez les problèmes éventuels et essayez à nouveau de réactiver le disque. Si la réactivation du disque réussit, tous les volumes sur le disque doivent automatiquement revenir à l’état **Sain**.
2. Dans l’observateur d’événements, vérifiez si les journaux des événements comportent des erreurs liées aux disques telles que « Aucune copie de configuration correcte ». Si les journaux des événements contiennent cette erreur, contactez les [services de support technique Microsoft](https://msdn.microsoft.com/library/aa263468(v=vs.60).aspx).

3. Essayez de déplacer le disque vers un autre ordinateur. Si vous parvenez à mettre le disque **En ligne** sur un autre ordinateur, le problème est probablement dû à la configuration de l’ordinateur sur lequel le disque ne passe pas à l’état **En ligne**.

4. Essayez de déplacer le disque vers un autre ordinateur doté de disques dynamiques. Importez le disque sur cet ordinateur, puis déplacez à nouveau le disque sur l’ordinateur sur lequel il ne peut pas être **En ligne**. 

## <a name="a-basic-or-dynamic-volumes-status-is-failed"></a>Échec de l’état d’un volume de base ou dynamique

**Cause :**   le volume de base ou dynamique ne peut pas être démarré automatiquement, le disque est endommagé, ou le système de fichiers est endommagé. L’état **Échec** indique une perte de données, sauf si le disque ou le système de fichiers peut être réparé.

**Solution :**

Si le volume est un volume de base avec **échec** état :

- Vérifiez que le disque physique sous-jacent est sous tension, branché et connecté à l’ordinateur.
- Essayez les étapes de dépannage décrites dans [l’état d’un disque n’est pas initialisé ou le disque est manquant entièrement](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

Si le volume est un volume dynamique dont l’état est **Échec** :

-   Assurez-vous que les disques sous-jacents sont en ligne. Si ce n’est pas le cas, remettez les disques à l’état **En ligne**. Si l’opération réussit, le volume redémarre automatiquement et revient à l’état **Sain**. Si le disque dynamique revient à l’état **En ligne**, mais que le volume dynamique ne revient pas à l’état **Sain** vous pouvez réactiver le volume manuellement.
-   Si le volume dynamique est un volume en miroir ou RAID-5 contenant d’anciennes données, la remise en ligne du disque sous-jacent ne redémarre pas automatiquement le volume. Si les disques qui contiennent des données actuelles sont déconnectés, commencez par mettre ces disques en ligne (pour permettre la synchronisation des données). Sinon, redémarrez le volume en miroir ou RAID-5 manuellement, puis exécutez l’outil de Vérification des erreurs ou Chkdsk.exe.
- Essayez les étapes de dépannage décrites dans [l’état d’un disque n’est pas initialisé ou le disque est manquant entièrement](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="a-basic-or-dynamic-volumes-status-is-unknown"></a>L’état d’un volume de base ou dynamique est inconnu

**Cause :**   le **inconnu** état se produit lorsque le secteur d’amorçage pour le volume est endommagé (probablement en raison d’un virus) et vous pouvez ne plus accéder aux données sur le volume. L’état peut également être **Inconnu** si, lorsque vous installez un nouveau disque, vous ne laissez pas l’Assistant créer la signature du disque.

**Solution**  initialiser le disque. Pour obtenir des instructions, voir [Initialiser les nouveaux disques](initialize-new-disks.md).

## <a name="a-dynamic-volumes-status-is-data-incomplete"></a>État d’un volume dynamique est données incomplètes

**Cause :** Vous avez déplacé certains, mais pas tous les disques dans un volume de disque multiples. Pour éviter la destruction des données de ce volume, vous devez déplacer et importer les disques restants contenant ce volume.

**Solution :**

1. Déplacez tous les disques du volume comportant plusieurs disques sur l’ordinateur.
2. Importez les disques. Pour obtenir des instructions sur la procédure à suivre pour déplacer et importer les disques, voir [Déplacer des disques vers un autre ordinateur](move-disks-to-another-computer.md).

Si vous n’avez plus besoin du volume comportant plusieurs disques, vous pouvez importer le disque et créer de nouveaux volumes sur celui-ci. Pour ce faire :

1. Cliquez avec le bouton droit sur le volume dont l’état est **Échec** ou **Échec de la redondance**, puis cliquez sur **Supprimer le volume**.
2. Cliquez avec le bouton droit sur le disque, puis cliquez sur **Nouveau nom**.

## <a name="a-dynamic-volumes-status-is-healthy-at-risk"></a>État d’un volume dynamique est sain (courant un risque)

**Cause :**   indique que le volume dynamique est actuellement accessible, mais les erreurs d’e/s ont été détectées sur le disque dynamique sous-jacent. Si une erreur d’E/S est détectée sur une zone d’un disque dynamique, tous les volumes du disque affichent l’état **Sain (Courant un risque)** et une icône d’avertissement s’affiche sur le volume.

Lorsque l’état du volume est **Sain (courant un risque)** , l’état d’un disque sous-jacent est généralement **En ligne (erreurs)** .

**Solution :**  
1. Remettez le disque sous-jacent à l’état **En ligne**. Une fois que le disque est de nouveau **En ligne**, l’état du volume devrait de nouveau être **Sain**. Si l’état est toujours **Sain (Courant un risque)** , le disque est peut-être défectueux. 

2. Sauvegardez les données et remplacez le disque dès que possible. 

## <a name="cannot-manage-striped-volumes-using-disk-management-or-diskpart"></a>Ne peut pas gérer les volumes agrégés par bandes à l’aide de la gestion des disques ou DiskPart

**Cause :**   certains produits de gestion de disque non-Microsoft remplacement le Gestionnaire de disque logique (LDM) Microsoft pour la gestion de disque avancée, qui peut désactiver le Gestionnaire de disque logique.

**Solution :**   si vous utilisez un logiciel de gestion de disque non-Microsoft qui a désactivé LDM, vous devez contacter le fournisseur sur le logiciel de gestion de disque non-Microsoft pour obtenir une assistance ou à résoudre des problèmes avec le disque configuration.

## <a name="disk-management-cannot-start-the-virtual-disk-service"></a>Gestion des disques ne peut pas démarrer le Service de disque virtuel

**Cause :**   si un ordinateur distant ne prend pas en charge le Service de disque virtuel (VDS) ou si vous ne pouvez pas établir une connexion à l’ordinateur distant parce qu’il est bloqué par le pare-feu Windows, vous pouvez recevoir cette erreur.

**Solution :**

1. Si l’ordinateur distant prend en charge le service VDS, vous pouvez configurer le pare-feu Windows Defender afin qu’il autorise les connexions VDS. Si l’ordinateur distant ne prend pas en charge VDS, vous pouvez utiliser la Connexion Bureau à distance pour vous connecter à celui-ci, puis exécutez Gestion des disques directement sur l’ordinateur distant.
2. Pour gérer les disques sur des ordinateurs distants qui prennent pas en charge le service VDS, vous devez configurer le pare-feu Windows Defender sur l’ordinateur local (sur lequel vous exécutez Gestion des disques) et sur l’ordinateur distant.
3. Sur l’ordinateur local, configurez le pare-feu Windows Defender pour activer l’exception de gestion des volumes à distance.

> [!NOTE]
> L’exception de gestion des volumes à distance inclut les exceptions pour Vds.exe, Vdsldr.exe et le port TCP 135.

> [!NOTE]
> Les connexions à distance dans les groupes de travail ne sont pas prises en charge. L’ordinateur local et l’ordinateur distant doivent tous les deux être membres d’un domaine.