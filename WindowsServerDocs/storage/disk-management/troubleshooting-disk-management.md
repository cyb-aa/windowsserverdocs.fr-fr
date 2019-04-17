---
title: "Résolution des problèmes de Gestion des disques"
description: "Cet article explique comment résoudre les problèmes liés à l’outil Gestion des disques"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e4b361631799dccbc2b77fb5aa909052532a057d
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="troubleshooting-disk-management"></a>Résolution des problèmes de Gestion des disques

> **S’applique à:** Windows10, Windows8.1, WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012

Cette rubrique répertorie quelques uns des problèmes courants que vous pouvez rencontrer lors de l’utilisation de l’outil Gestion des disques.

<a id="BKMK_1"></a>

## <a name="partitions-on-basic-disks-added-to-the-system-do-not-appear-in-the-disk-management-volume-list-view"></a>Les partitions des disques de base ajoutés au système ne s’affichent pas dans la Liste des volumes de Gestion des disques.

**Cause:** par défaut, les volumes des disques de base ajoutés au système ne sont pas automatiquement montés et aucune lettre de lecteur ne leur affectée.
Lors de l’ajout au système, l’état des disques dynamiques est **Étranger**. Pour utiliser les volumes, vous devez importer les disques **étrangers**, puis les mettre **en ligne**.
Les médias amovibles (lecteurs Zip ou Jaz, par exemple) et les disques optiques (CD-ROM ou DVD-RAM, par exemple) sont toujours automatiquement montés par le système.

**Solution:** montez manuellement les volumes de base en leur affectant des lettres de lecteur ou en créant des points de montage à l’aide de Gestion des disques ou des commandes [DiskPart](http://go.microsoft.com/fwlink/?LinkId=64110) ou [mountvol](http://go.microsoft.com/fwlink/?LinkId=64111).

<a id="BKMK_2"></a>

## <a name="a-basic-disks-status-is-not-initialized"></a>L’état d’un disque de base est Non initialisé.

**Cause:** le disque ne contient pas de signature valide. Après l’installation d’un nouveau disque, le système d’exploitation doit écrire la signature du disque, le marqueur de fin de secteur (également appelé mot signature), ainsi qu’un enregistrement de démarrage principal (MBR) ou une table de partition GUID pour vous permettre de créer des partitions sur le disque. La première fois que vous démarrez l’outil Gestion des disques après l’installation d’un nouveau disque, un Assistant s’affiche et indique la liste des nouveaux disques détectés par le système d’exploitation. Si vous annulez l’Assistant avant l’écriture de la signature du disque, l’état du disque reste **Non initialisé**.

**Solution:** initialisez le disque. L’état du disque passe brièvement à **Initialisation** puis à **En ligne**. Pour obtenir des instructions décrivant comment initialiser un disque, voir [Initialiser les nouveaux disques](initialize-new-disks.md). 

<a id="BKMK_3"></a>

## <a name="a-basic-or-dynamic-disks-status-is-unreadable"></a>L’état d’un disque de base ou dynamique est Illisible.

**Cause:** le disque de base ou dynamique n’est pas accessible; il a peut-être été affecté par une défaillance matérielle, des erreurs d’E/S ou il a été endommagé. La copie du disque de la base de données de configuration du disque du système est peut-être endommagée. Une icône d’erreur s’affiche sur les disques dont l’état est **Illisible**.

Les disques peuvent également afficher l’état **Illisible** pendant qu’ils sont en rotation ou lorsque Gestion des disques effectue une nouvelle analyse de tous les disques du système. Dans certains cas, un disque illisible échoue et est irrécupérable. Pour les disques dynamiques, l’état **Illisible** est généralement dû à l’endommagement ou des erreurs d’E/S sur une partie du disque, plutôt qu’à l’échec de l’ensemble du disque.

**Solution:** effectuez une nouvelle analyse des disques ou redémarrez l’ordinateur pour voir si l’état du disque change.

<a id="BKMK_4"></a>

## <a name="a-dynamic-disks-status-is-foreign"></a>L’état d’un disque dynamique est Étranger.

**Cause:** l’état **Étranger** est généré lorsque vous déplacez un disque dynamique vers l’ordinateur local à partir d’un autre ordinateur qui exécute les systèmes d’exploitation Windows2000, WindowsXPProfessionnel, WindowsP édition 64bits ou WindowsServer2003. Une icône d’avertissement s’affiche sur les disques dont l’état est **Étranger**.

Dans certains cas, un disque qui était connecté au système auparavant peut afficher l’état **Étranger**. Les données de configuration des disques dynamiques sont stockées sur tous les disques dynamiques. Par conséquent, les informations sur les disques détenus par le système sont perdues en cas d’échec de tous les disques dynamiques.

**Solution:** ajoutez le disque à la configuration système de votre ordinateur afin de pouvoir accéder aux données sur le disque. Pour ajouter un disque à la configuration système de votre ordinateur, importez le disque étranger (cliquez avec le bouton droit sur le disque, puis cliquez sur **Importer des disques étrangers**). Lorsque vous importez le disque, les volumes existants du disque étranger deviennent visibles et accessibles. 

<a id="BKMK_5"></a>

## <a name="a-dynamic-disks-status-is-online-errors"></a>L’état d’un disque dynamique est En ligne (erreurs).

**Cause:** le disque dynamique contient des erreurs d’E/S dans une région du disque. Une icône d’avertissement s’affiche sur le disque dynamique comportant des erreurs.

**Solution:** si les erreurs d’E/S sont temporaires, réactivez le disque pour le remettre à l’état **En ligne**.

<a id="BKMK_6"></a>

## <a name="a-dynamic-disks-status-is-offline-or-missing"></a>L’état d’un disque dynamique indique Hors connexion ou Manquant.

**Cause:** un disque dynamique **Hors connexion** peut être endommagé ou indisponible par intermittence. Une icône d’erreur s’affiche sur le disque dynamique hors connexion.

Si l’état du disque est **Hors connexion** et que le nom du disque devient **Manquant**, cela signifie que le disque était récemment disponible sur le système, mais qu’il ne peut plus être localisé ou identifié. Le disque manquant peut être endommagé, hors tension ou déconnecté.

**Solution:** pour remettre en ligne un disque qui est hors connexion et manquant:
1. Résolvez les problèmes éventuels liés au disque, au contrôleur ou aux câbles. 
2. Vérifiez que le disque physique est sous tension, branché et connecté à l’ordinateur. 
3. Ensuite, utilisez la commande **Réactiver le disque** pour remettre le disque en ligne.

4. Si l’état du disque reste **Hors connexion**, que le nom du disque est toujours **Manquant** et que vous constatez qu’un problème affectant le disque ne peut pas être résolu, vous pouvez supprimer le disque du système en cliquant dessus avec le bouton droit, puis en cliquant sur **Supprimer le disque**). Toutefois, avant de pouvoir supprimer le disque, vous devez supprimer tous les volumes (ou miroirs) sur celui-ci. Vous pouvez enregistrer tous les volumes en miroir présents sur le disque en supprimant le miroir au lieu de l’intégralité du volume. La suppression d’un volume détruit les données du volume. Par conséquent, vous devez supprimer un disque uniquement si vous êtes absolument certain que le disque est définitivement endommagé et inutilisable.

**Pour remettre en ligne un disque qui est hors connexion et toujours appelé Disque \ # (pas Manquant), essayez une ou plusieurs des procédures suivantes:**

1. Dans Gestion des disques, cliquez avec le bouton droit sur le disque, puis cliquez sur **Réactiver le disque** pour le remettre en ligne. Si l’état du disque reste **Hors connexion**, vérifiez les câbles et le contrôleur de disque et assurez-vous que le disque physique est sain. Corrigez les problèmes éventuels et essayez à nouveau de réactiver le disque. Si la réactivation du disque réussit, tous les volumes sur le disque doivent automatiquement revenir à l’état **Sain**.
2. Dans l’observateur d’événements, vérifiez si les journaux des événements comportent des erreurs liées aux disques telles que «Aucune copie de configuration correcte». Si les journaux des événements contiennent cette erreur, contactez les [services de support technique Microsoft](https://msdn.microsoft.com/library/aa263468(v=vs.60).aspx).

3. Essayez de déplacer le disque vers un autre ordinateur. Si vous parvenez à mettre le disque **En ligne** sur un autre ordinateur, le problème est probablement dû à la configuration de l’ordinateur sur lequel le disque ne passe pas à l’état **En ligne**.

4. Essayez de déplacer le disque vers un autre ordinateur doté de disques dynamiques. Importez le disque sur cet ordinateur, puis déplacez à nouveau le disque sur l’ordinateur sur lequel il ne peut pas être **En ligne**. 

<a id="BKMK_7"></a>

## <a name="a-basic-or-dynamic-volumes-status-is-failed"></a>L’état d’un volume de base ou dynamique est Échec.

**Cause:** le volume de base ou dynamique ne peut pas être démarré automatiquement, le disque est endommagé ou le système de fichiers est endommagé. L’état **Échec** indique une perte de données, sauf si le disque ou le système de fichiers peut être réparé.

**Solution:** si le volume est un volume de base dont l’état est **Échec**:

-   Vérifiez que le disque physique sous-jacent est sous tension, branché et connecté à l’ordinateur. Aucune autre action utilisateur n’est possible pour les volumes de base.

    Si le volume est un volume dynamique dont l’état est **Échec**: 
-   Assurez-vous que les disques sous-jacents sont en ligne. Si ce n’est pas le cas, remettez les disques à l’état **En ligne**. Si l’opération réussit, le volume redémarre automatiquement et revient à l’état **Sain**. Si le disque dynamique revient à l’état **En ligne**, mais que le volume dynamique ne revient pas à l’état **Sain** vous pouvez réactiver le volume manuellement. 
    
-   Si le volume dynamique est un volume en miroir ou RAID-5 contenant d’anciennes données, la remise en ligne du disque sous-jacent ne redémarre pas automatiquement le volume. Si les disques qui contiennent des données actuelles sont déconnectés, commencez par mettre ces disques en ligne (pour permettre la synchronisation des données). Sinon, redémarrez le volume en miroir ou RAID-5 manuellement, puis exécutez l’outil de Vérification des erreurs ou Chkdsk.exe.

<a id="BKMK_8"></a>

## <a name="a-basic-or-dynamic-volumes-status-is-unknown"></a>L’état d’un volume de base ou dynamique est Inconnu.

**Cause:** l’état d’un volume peut être **Inconnu** si son secteur de démarrage est endommagé (peut-être en raison d’un virus) et si vous ne pouvez plus accéder aux données du volume. L’état peut également être **Inconnu** si, lorsque vous installez un nouveau disque, vous ne laissez pas l’Assistant créer la signature du disque.

**Solution:** initialisez le disque. Pour obtenir des instructions, voir [Initialiser les nouveaux disques](initialize-new-disks.md). 

<a id="BKMK_9"></a>

## <a name="a-dynamic-volumes-status-is-data-incomplete"></a>L’état d’un volume dynamique est Données incomplètes.

**Cause:** vous avez déplacé une partie, mais pas la totalité des disques d’un volume comportant plusieurs disques. Pour éviter la destruction des données de ce volume, vous devez déplacer et importer les disques restants contenant ce volume.

**Solution:**  
1. Déplacez tous les disques du volume comportant plusieurs disques sur l’ordinateur.

2. Importez les disques. Pour obtenir des instructions sur la procédure à suivre pour déplacer et importer les disques, voir [Déplacer des disques vers un autre ordinateur](move-disks-to-another-computer.md).

Si vous n’avez plus besoin du volume comportant plusieurs disques, vous pouvez importer le disque et créer de nouveaux volumes sur celui-ci. Pour ce faire : 

1. Cliquez avec le bouton droit sur le volume dont l’état est **Échec** ou **Échec de la redondance**, puis cliquez sur **Supprimer le volume**. 

2. Cliquez avec le bouton droit sur le disque, puis cliquez sur **Nouveau volume**.

<a id="BKMK_10"></a>

## <a name="a-dynamic-volumes-status-is-healthy-at-risk"></a>L’état d’un volume dynamique est Sain (Courant un risque).

**Cause:** indique que le volume dynamique est actuellement accessible, mais que des erreurs d’E/S ont été détectées sur le disque dynamique sous-jacent. Si une erreur d’E/S est détectée sur une zone d’un disque dynamique, tous les volumes du disque affichent l’état **Sain (Courant un risque)** et une icône d’avertissement s’affiche sur le volume.

Lorsque l’état du volume est **Sain (courant un risque)**, l’état d’un disque sous-jacent est généralement **En ligne (erreurs)**.

**Solution:**  
1. Remettez le disque sous-jacent à l’état **En ligne**. Une fois que le disque est de nouveau **En ligne**, l’état du volume devrait de nouveau être **Sain**. Si l’état est toujours **Sain (Courant un risque)**, le disque est peut-être défectueux. 

2. Sauvegardez les données et remplacez le disque dès que possible. 

<a id="BKMK_11"></a>

## <a name="cannot-manage-striped-volumes-using-disk-management-or-diskpart"></a>Impossible de gérer les volumes agrégés par bande à l’aide de Gestion des disques ou de DiskPart.

**Cause:** certains produits de gestion de disques tiers remplacent le Gestionnaire de disques logiques (LDM) Microsoft pour la gestion avancée de disques, ce qui peut désactiver le Gestionnaire de disques logiques.

**Solution:** si vous utilisez un logiciel de gestion de disques tiers qui a désactivé LDM, vous devez contacter le fournisseur du logiciel de gestion de disques tiers pour obtenir de l’aide ou résoudre les problèmes liés à la configuration des disques.

<a id="BKMK_virtdisk"></a>

## <a name="disk-management-cannot-start-the-virtual-disk-service"></a>Gestion des disques ne peut pas démarrer le service de disque virtuel.

**Cause:** vous pouvez recevoir cette erreur si un ordinateur distant ne prend pas en charge le service de disque virtuel (VDS), ou si vous ne pouvez pas établir de connexion avec l’ordinateur distant parce qu’il est bloqué par le pare-feu Windows

**Solution:**  
1. Si l’ordinateur distant prend en charge le service VDS, vous pouvez configurer le pare-feu WindowsDefender afin qu’il autorise les connexions VDS. Si l’ordinateur distant ne prend pas en charge VDS, vous pouvez utiliser la Connexion Bureau à distance pour vous connecter à celui-ci, puis exécutez Gestion des disques directement sur l’ordinateur distant.

2. Pour gérer les disques sur des ordinateurs distants qui prennent pas en charge le service VDS, vous devez configurer le pare-feu WindowsDefender sur l’ordinateur local (sur lequel vous exécutez Gestion des disques) et sur l’ordinateur distant.

3. Sur l’ordinateur local, configurez le pare-feu WindowsDefender pour activer l’exception de gestion des volumes à distance.

<br />

> [!NOTE]
> L’exception de gestion des volumes à distance inclut les exceptions pour Vds.exe, Vdsldr.exe et le port TCP135.

<br />

 > [!NOTE]
 > Les connexions à distance dans les groupes de travail ne sont pas prises en charge. L’ordinateur local et l’ordinateur distant doivent tous les deux être membres d’un domaine.