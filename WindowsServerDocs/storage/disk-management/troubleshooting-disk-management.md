---
title: Résolution des problèmes de la Gestion des disques
description: Cet article explique comment résoudre les problèmes liés à l’outil Gestion des disques
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d801b051918c090257a466ab58c200943487b2e8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402169"
---
# <a name="troubleshooting-disk-management"></a>Résolution des problèmes de la Gestion des disques

> **S’applique à :** Windows 10, Windows 8.1, Windows 7, Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique répertorie quelques-uns des problèmes courants que vous pouvez rencontrer lors de l’utilisation de l’outil Gestion des disques.

> [!TIP]
> Si vous obtenez une erreur ou si quelque chose ne fonctionne pas lorsque vous suivez ces procédures, ne vous affolez pas ! Il existe une multitude d’informations sur le site de la [communauté Microsoft](https://answers.microsoft.com/en-us/windows). Essayez de rechercher la section [Fichiers, dossiers et stockage](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) et si vous avez toujours besoin d’aide, publiez une question ici et Microsoft ou d’autres membres de la communauté essayeront de vous aider. Si vous avez des commentaires sur la façon d’améliorer ces rubriques, nous aimerions les connaître ! Répondez simplement à l’invite *Est-ce que cette page est utile ?* et laissez des commentaires ici ou dans la conversation des commentaires publics en bas de cette rubrique.

## <a name="a-disks-status-is-not-initialized-or-the-disk-is-missing"></a>L’état d’un disque est Non initialisé ou le disque est manquant

![Outil Gestion des disques montrant un disque inconnu qui doit être initialisé.](media/uninitialized-disk.PNG)

**Cause** : si vous avez un disque qui n’apparaît pas dans l’Explorateur de fichiers et qui est répertorié avec l’état *Non initialisé* dans l’outil Gestion des disques, cela peut signifier que le disque n’a pas de signature de disque valide. En fait, cela veut dire que le disque n’a jamais été initialisé ni formaté, ou que le formatage du disque a été endommagé. 

Il est également possible que le disque rencontre des problèmes matériels ou des problèmes de branchement, mais nous aborderons cela par la suite.

**Solution :**   si le lecteur est nouveau et doit simplement être initialisé, en effaçant toutes les données qu’il contient, la solution est facile ; consultez [Initialiser de nouveaux disques](initialize-new-disks.md). Toutefois, il est probable que vous ayez déjà essayé et que cela n’ait pas fonctionné. Ou vous disposez peut-être d’un disque contenant des fichiers importants, que vous ne souhaitez pas effacer du disque en l’initialisant.

De nombreuses raisons peuvent expliquer l’absence ou l’échec d’initialisation d’un disque, et l’une des plus courantes est une défaillance du disque. Le nombre d’opérations à effectuer pour réparer un disque défaillant est important, mais voici quelques étapes à essayer pour voir si nous pouvons le faire fonctionner de nouveau. Si le disque fonctionne après avoir suivi l’une des étapes ci-après, arrêtez-vous là. Détendez-vous, amusez-vous et mettez éventuellement à jour vos sauvegardes.

1. Observez le disque dans l’outil Gestion des disques. Si son état est défini sur *Hors connexion*, comme c’est le cas ici, cliquez avec le bouton droit dessus et sélectionnez **En ligne**.

    ![État du disque affiché comme étant hors connexion](media/offline-disk.png)
2. Si l’état du disque est défini sur *En ligne* dans l’outil Gestion des disques, et qu’il possède une partition principale répertoriée comme *Saine*, comme affiché ici, c’est bon signe.

    ![État du disque affiché comme étant hors connexion avec un volume sain](media/healthy-volume.png)
    - Si la partition a un système de fichiers, mais aucune lettre de lecteur (par exemple, E:), consultez [Change a drive letter](change-a-drive-letter.md) (Modifier une lettre de lecteur) pour ajouter une lettre de lecteur manuellement.
    - Si elle n’a pas de système de fichiers (NTFS, ReFS, FAT32 ou exFAT) et que vous savez que le disque est vide, cliquez avec le bouton droit sur la partition, puis sélectionnez **Formater**. Le formatage d’un disque efface toutes les données qu’il contient. Évitez donc de procéder à cette opération si vous tentez de récupérer des fichiers à partir du disque ; passez plutôt à l’étape suivante.
3. Si vous avez un disque externe, débranchez-le, rebranchez-le, puis sélectionnez **Action** > **Analyser les disques de nouveau**. 
4. Arrêtez votre PC, mettez hors tension votre disque dur externe (s’il s’agit d’un disque externe avec un cordon d’alimentation), puis rallumez votre PC et le disque.
    Pour arrêter votre PC dans Windows 10, sélectionnez le bouton Démarrer, le bouton Marche/Arrêt, puis **Arrêter**.
5. Branchez le disque sur un autre port USB qui se trouve directement sur votre PC (pas sur un concentrateur).
    Il peut arriver que les disques USB n’aient pas suffisamment de puissance sur certains ports ou qu’ils aient d’autres problèmes avec des ports spécifiques. Cela est particulièrement courant avec les concentrateurs USB, mais parfois, il existe des différences entre les ports d’un PC. Essayez donc plusieurs ports différents si possible.
6. Essayez d’utiliser un autre câble.
    Cela peut sembler étrange, mais les câbles sont souvent défaillants. Vous pouvez donc essayer d’utiliser un câble différent pour brancher le disque. Si vous avez un disque interne sur un PC de bureau, vous devrez probablement arrêter votre PC avant de changer les câbles. Consultez le manuel utilisateur de votre PC pour plus d’informations.
7. Consultez le Gestionnaire de périphériques pour voir les éventuels problèmes.
    Appuyez de façon prolongée (ou cliquez avec le bouton droit) sur le bouton Démarrer, puis sélectionnez le Gestionnaire de périphériques dans le menu contextuel. Recherchez tous les appareils avec un point d’exclamation en regard de leur nom ou d’autres problèmes, double-cliquez sur l’appareil et consultez son état.

    Voici une liste des [codes d’erreur dans le Gestionnaire de périphériques](https://support.microsoft.com/help/310123/error-codes-in-device-manager-in-windows), mais une approche qui est parfois efficace consiste à cliquer avec le bouton droit sur l’appareil qui pose problème, à sélectionner **Désinstaller l’appareil**, puis **Action** >  **Rechercher les modifications sur le matériel**.

    ![Gestionnaire de périphériques affichant un appareil USB inconnu](media/device-manager.PNG)
8. Branchez le disque sur un autre PC.
    
    Si le disque ne fonctionne pas sur un autre PC, cela veut dire que le problème vient du disque et non de votre PC, ce qui est plutôt bon signe, bien que tout soit relatif. Vous trouverez certaines étapes supplémentaires à essayer dans [Forum FAQ: External USB drive error "You must initialize the disk before Logical Disk Manager can access it"](https://social.technet.microsoft.com/Forums/windows/en-US/2b069948-82e9-49ef-bbb7-e44ec7bfebdb/forum-faq-external-usb-drive-error-you-must-initialize-the-disk-before-logical-disk-manager-can?forum=w7itprohardware) (FAQ forum : Erreur du lecteur USB externe « Vous devez initialiser le disque avant que le Gestionnaire de disque logique puisse y accéder »), mais il peut être temps de chercher et demander de l’aide sur le site de la [communauté Microsoft](https://answers.microsoft.com/en-us/windows) ou de contacter le fabricant de votre disque.

    Si le problème n’est toujours pas résolu, il existe également des applications qui peuvent essayer de récupérer des données à partir d’un disque défaillant, ou si les fichiers sont vraiment importants, vous pouvez payer un laboratoire de récupération des données pour tenter de les récupérer. Si vous trouvez quelque chose qui fonctionne pour vous, faites-le nous savoir dans la section des commentaires ci-dessous.

> [!IMPORTANT]
> Les disques échouent assez souvent, il est donc important de sauvegarder régulièrement les fichiers qui sont importants. Si vous avez un disque qui parfois ne s’affiche pas ou génère des erreurs, considérez cela comme un rappel pour vérifier vos méthodes de sauvegarde. Ce n’est pas grave si vous avez un peu de retard, nous sommes tous passés par là. La meilleure solution de sauvegarde est une solution que vous utilisez. Nous vous encourageons donc à trouver celle qui fonctionne pour vous et à la garder.
> 
> [!TIP]
> Pour plus d’informations sur l’utilisation des applications intégrées à Windows pour sauvegarder des fichiers sur un lecteur externe comme un lecteur USB, consultez [Sauvegarde et restauration dans Windows 10](https://support.microsoft.com/help/17143/windows-10-back-up-your-files). Vous pouvez également enregistrer des fichiers dans Microsoft OneDrive, qui synchronise les fichiers à partir de votre PC vers le cloud. Si votre disque dur tombe en panne, vous serez toujours en mesure de récupérer les fichiers que vous stockez dans OneDrive à partir de OneDrive.com. Pour plus d’informations, consultez [OneDrive sur votre PC](https://support.microsoft.com/help/17184/windows-10-onedrive).

## <a name="a-basic-or-dynamic-disks-status-is-unreadable"></a>L’état d’un disque de base ou dynamique est Illisible

**Cause :**   le disque de base ou dynamique n’est pas accessible ; il a peut-être été affecté par une défaillance matérielle, des erreurs d’E/S ou il a été endommagé. La copie du disque de la base de données de configuration du disque du système est peut-être endommagée. Une icône d’erreur s’affiche sur les disques dont l’état est **Illisible**.

Les disques peuvent également afficher l’état **Illisible** pendant qu’ils sont en rotation ou lorsque l’outil Gestion des disques effectue une nouvelle analyse de tous les disques du système. Dans certains cas, un disque illisible échoue et est irrécupérable. Pour les disques dynamiques, l’état **Illisible** est généralement dû à l’endommagement ou des erreurs d’E/S sur une partie du disque, plutôt qu’à l’échec de l’ensemble du disque.

**Solution :**   effectuez une nouvelle analyse des disques ou redémarrez l’ordinateur pour voir si l’état du disque change. Essayez également les étapes de résolution des problèmes décrites dans [L’état d’un disque est Non initialisé ou le disque est manquant](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="a-dynamic-disks-status-is-foreign"></a>L’état d’un disque dynamique est Étranger

**Cause :**   l’état **Étranger** est appliqué lorsque vous déplacez un disque dynamique sur l’ordinateur local depuis un autre PC. Une icône d’avertissement s’affiche sur les disques dont l’état est **Étranger**.

Dans certains cas, un disque qui était connecté au système auparavant peut afficher l’état **Étranger**. Les données de configuration des disques dynamiques sont stockées sur tous les disques dynamiques. Par conséquent, les informations sur les disques détenus par le système sont perdues en cas d’échec de tous les disques dynamiques.

**Solution :**   ajoutez le disque à la configuration système de votre ordinateur afin de pouvoir accéder aux données sur le disque. Pour ajouter un disque à la configuration système de votre ordinateur, importez le disque étranger (cliquez avec le bouton droit sur le disque, puis cliquez sur **Importer des disques étrangers**). Lorsque vous importez le disque, les volumes existants du disque étranger deviennent visibles et accessibles. 

## <a name="a-dynamic-disks-status-is-online-errors"></a>L’état d’un disque dynamique est En ligne (erreurs)

**Cause :**   le disque dynamique contient des erreurs d’E/S dans une région du disque. Une icône d’avertissement s’affiche sur le disque dynamique comportant des erreurs.

**Solution :**    si les erreurs d’E/S sont temporaires, réactivez le disque pour le remettre à l’état **En ligne**.

## <a name="a-dynamic-disks-status-is-offline-or-missing"></a>L’état d’un disque dynamique indique Hors connexion ou Manquant

**Cause :**   un disque dynamique **Hors connexion** peut être endommagé ou indisponible par intermittence. Une icône d’erreur s’affiche sur le disque dynamique hors connexion.

Si l’état du disque est **Hors connexion** et que le nom du disque devient **Manquant**, cela signifie que le disque était récemment disponible sur le système, mais qu’il ne peut plus être localisé ou identifié. Le disque manquant peut être endommagé, hors tension ou déconnecté.

**Solution :** pour remettre en ligne un disque qui est hors connexion et manquant :

1. Résolvez les problèmes éventuels liés au disque, au contrôleur ou aux câbles. 
2. Vérifiez que le disque physique est sous tension, branché et connecté à l’ordinateur. 
3. Ensuite, utilisez la commande **Réactiver le disque** pour remettre le disque en ligne.
4. Essayez les étapes de résolution des problèmes décrites dans [L’état d’un disque est Non initialisé ou le disque est manquant](#a-disks-status-is-not-initialized-or-the-disk-is-missing).
5. Si l’état du disque reste **Hors connexion**, que le nom du disque est toujours **Manquant** et que vous constatez qu’un problème affectant le disque ne peut pas être résolu, vous pouvez supprimer le disque du système en cliquant dessus avec le bouton droit, puis en cliquant sur **Supprimer le disque**. Toutefois, avant de pouvoir supprimer le disque, vous devez supprimer tous les volumes (ou miroirs) sur celui-ci. Vous pouvez enregistrer tous les volumes en miroir présents sur le disque en supprimant le miroir au lieu de l’intégralité du volume. La suppression d’un volume détruit les données du volume. Par conséquent, vous devez supprimer un disque uniquement si vous êtes absolument certain que le disque est définitivement endommagé et inutilisable.

**Pour remettre en ligne un disque qui est hors connexion et toujours appelé Disque \# (pas Manquant), essayez une ou plusieurs des procédures suivantes :**

1. Dans l’outil Gestion des disques, cliquez avec le bouton droit sur le disque, puis cliquez sur **Réactiver le disque** pour le remettre en ligne. Si l’état du disque reste **Hors connexion**, vérifiez les câbles et le contrôleur de disque et assurez-vous que le disque physique est intègre. Corrigez les problèmes éventuels et essayez à nouveau de réactiver le disque. Si la réactivation du disque réussit, tous les volumes sur le disque doivent automatiquement revenir à l’état **Sain**.
2. Dans l’observateur d’événements, vérifiez si les journaux des événements comportent des erreurs liées aux disques telles que « Aucune copie de configuration correcte ». Si les journaux des événements contiennent cette erreur, contactez les [services de support technique Microsoft](https://msdn.microsoft.com/library/aa263468(v=vs.60).aspx).

3. Essayez de déplacer le disque vers un autre ordinateur. Si vous parvenez à mettre le disque **En ligne** sur un autre ordinateur, le problème est probablement dû à la configuration de l’ordinateur sur lequel le disque ne passe pas à l’état **En ligne**.

4. Essayez de déplacer le disque vers un autre ordinateur doté de disques dynamiques. Importez le disque sur cet ordinateur, puis déplacez à nouveau le disque sur l’ordinateur sur lequel il ne peut pas être **En ligne**. 

## <a name="a-basic-or-dynamic-volumes-status-is-failed"></a>L’état d’un volume de base ou dynamique est Échec

**Cause :**    le volume de base ou dynamique ne peut pas être démarré automatiquement, le disque est endommagé ou le système de fichiers est endommagé. L’état **Échec** indique une perte de données, sauf si le disque ou le système de fichiers peut être réparé.

**Solution :**

si le volume est un volume de base dont l’état est **Échec** :

- Vérifiez que le disque physique sous-jacent est sous tension, branché et connecté à l’ordinateur.
- Essayez les étapes de résolution des problèmes décrites dans [L’état d’un disque est Non initialisé ou le disque est manquant](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

Si le volume est un volume dynamique dont l’état est **Échec** :

-   assurez-vous que les disques sous-jacents sont en ligne. Si ce n’est pas le cas, remettez les disques à l’état **En ligne**. Si l’opération réussit, le volume redémarre automatiquement et revient à l’état **Sain**. Si le disque dynamique revient à l’état **En ligne**, mais que le volume dynamique ne revient pas à l’état **Sain** vous pouvez réactiver le volume manuellement.
-   Si le volume dynamique est un volume en miroir ou RAID-5 contenant d’anciennes données, la remise en ligne du disque sous-jacent ne redémarre pas automatiquement le volume. Si les disques qui contiennent les données actuelles sont déconnectés, commencez par mettre ces disques en ligne (pour permettre la synchronisation des données). Sinon, redémarrez le volume en miroir ou RAID-5 manuellement, puis exécutez l’outil de Vérification des erreurs ou Chkdsk.exe.
- Essayez les étapes de résolution des problèmes décrites dans [L’état d’un disque est Non initialisé ou le disque est manquant](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="a-basic-or-dynamic-volumes-status-is-unknown"></a>L’état d’un volume de base ou dynamique est Inconnu

**Cause :**    l’état d’un volume peut être **Inconnu** si son secteur de démarrage est endommagé (peut-être en raison d’un virus) et si vous ne pouvez plus accéder aux données du volume. L’état peut également être **Inconnu** si, lorsque vous installez un nouveau disque, vous ne laissez pas l’Assistant créer la signature du disque.

**Solution :**    initialisez le disque. Pour obtenir des instructions, consultez [Initialiser de nouveaux disques](initialize-new-disks.md).

## <a name="a-dynamic-volumes-status-is-data-incomplete"></a>L’état d’un volume dynamique est Données incomplètes

**Cause** : vous avez déplacé une partie, mais pas la totalité des disques d’un volume comportant plusieurs disques. Pour éviter la destruction des données de ce volume, vous devez déplacer et importer les disques restants contenant ce volume.

**Solution :**

1. déplacez tous les disques du volume comportant plusieurs disques sur l’ordinateur.
2. Importez les disques. Pour obtenir des instructions sur la procédure à suivre pour déplacer et importer les disques, consultez [Move disks to another computer](move-disks-to-another-computer.md) (Déplacer des disques vers un autre ordinateur).

Si vous n’avez plus besoin du volume comportant plusieurs disques, vous pouvez importer le disque et créer de nouveaux volumes sur celui-ci. Pour ce faire :

1. Cliquez avec le bouton droit sur le volume dont l’état est **Échec** ou **Échec de la redondance**, puis cliquez sur **Supprimer le volume**.
2. Cliquez avec le bouton droit sur le disque, puis cliquez sur **Nouveau nom**.

## <a name="a-dynamic-volumes-status-is-healthy-at-risk"></a>L’état d’un volume dynamique est Sain (Courant un risque)

**Cause :**    indique que le volume dynamique est actuellement accessible, mais que des erreurs d’E/S ont été détectées sur le disque dynamique sous-jacent. Si une erreur d’E/S est détectée sur une zone d’un disque dynamique, tous les volumes du disque affichent l’état **Sain (Courant un risque)** et une icône d’avertissement s’affiche sur le volume.

Lorsque l’état du volume est **Sain (Courant un risque)** , l’état d’un disque sous-jacent est généralement **En ligne (erreurs)** .

**Solution :**  
1. Remettez le disque sous-jacent à l’état **En ligne**. Une fois que le disque est de nouveau **En ligne**, l’état du volume devrait de nouveau être **Sain**. Si l’état est toujours **Sain (Courant un risque)** , le disque est peut-être défectueux. 

2. Sauvegardez les données et remplacez le disque dès que possible. 

## <a name="cannot-manage-striped-volumes-using-disk-management-or-diskpart"></a>Impossible de gérer les volumes agrégés par bande à l’aide de l’outil Gestion des disques ou de DiskPart

**Cause :**    certains produits de gestion de disques tiers remplacent le Gestionnaire de disques logiques Microsoft pour la gestion avancée de disques, ce qui peut désactiver le Gestionnaire de disques logiques.

**Solution :**    si vous utilisez un logiciel de gestion de disques tiers qui a désactivé le Gestionnaire de disques logiques, vous devez contacter le fournisseur du logiciel de gestion de disques tiers pour obtenir de l’aide ou résoudre les problèmes liés à la configuration des disques.

## <a name="disk-management-cannot-start-the-virtual-disk-service"></a>L’outil Gestion des disques ne peut pas démarrer le service de disque virtuel

**Cause :**    vous pouvez recevoir cette erreur si un ordinateur distant ne prend pas en charge le service de disque virtuel, ou si vous ne pouvez pas établir de connexion avec l’ordinateur distant parce qu’il est bloqué par le pare-feu Windows.

**Solution :**

1. Si l’ordinateur distant prend en charge le service de disque virtuel, vous pouvez configurer le pare-feu Windows Defender afin qu’il autorise les connexions de service de disque virtuel. Si l’ordinateur distant ne prend pas en charge le service de disque virtuel, vous pouvez utiliser la Connexion Bureau à distance pour vous connecter à celui-ci, puis exécutez l’outil Gestion des disques directement sur l’ordinateur distant.
2. Pour gérer les disques sur des ordinateurs distants qui prennent en charge le service de disque virtuel, vous devez configurer le pare-feu Windows Defender sur l’ordinateur local (sur lequel vous exécutez l’outil Gestion des disques) et sur l’ordinateur distant.
3. Sur l’ordinateur local, configurez le pare-feu Windows Defender pour activer l’exception de gestion des volumes à distance.

> [!NOTE]
> L’exception de gestion des volumes à distance inclut les exceptions pour Vds.exe, Vdsldr.exe et le port TCP 135.

> [!NOTE]
> Les connexions à distance dans les groupes de travail ne sont pas prises en charge. L’ordinateur local et l’ordinateur distant doivent tous les deux être membres d’un domaine.