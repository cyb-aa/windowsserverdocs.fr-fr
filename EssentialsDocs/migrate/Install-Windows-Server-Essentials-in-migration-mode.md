---
title: Installer WindowsServerEssentials en mode de migration 1
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fd7196ac-cfa6-46a5-ba77-6962b47a825e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 808a4b1e120fa559d603b34ad006b18de6b94378
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-windows-server-essentials-in-migration-mode1"></a>Installer WindowsServerEssentials en mode de migration 1

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez avoir qu’un seul serveur sur votre réseau qui exécute WindowsServerEssentials, et ce serveur doit être un contrôleur de domaine pour le réseau.  
  
 Lorsque vous installez WindowsServerEssentials en mode migration, l’Assistant installation effectue les tâches suivantes:  
  
1.  Installe et configure le logiciel du serveur WindowsServerEssentials sur le serveur de Destination.  
  
2.  Mises à jour le schéma de domaine à la version la plus récente.  
  
3.  Joint le serveur de Destination au domaine existant. Le serveur Source et le serveur de Destination peuvent tous deux être membres du même domaine jusqu'à ce que le processus de migration est terminé. Une fois la migration terminée, vous devez supprimer le serveur Source à partir du réseau de 21jours.  
  
    > [!WARNING]
    >  Un message d’erreur est ajouté au journal des événements chaque jour pendant la période de grâce de 21jours jusqu'à la suppression du serveur Source à partir de votre réseau. Le texte du message est le suivant, «la vérification du rôle FSMO a détecté une condition dans votre environnement qui est conformes à la stratégie de licence. Le serveur d’administration doit contenir le contrôleur de domaine principal et les rôles de maître d’ActiveDirectory de noms de domaine. Déplacez les rôles ActiveDirectory vers le serveur d’administration maintenant. Ce serveur sera automatiquement arrêté si le problème n’est pas résolu dans les 21jours à partir de la que première détection de la condition.» Après la période de grâce de 21jours, le serveur Source s’arrête.  
  
4.  Transfère les rôles de maître d’opérations (également appelé opérations à maître uniques flottant ou FSMO) opérations à partir du serveur Source vers le serveur de Destination. Rôles de maître d’opérations sont des tâches de contrôleur de domaine spécialisées, qui sont utilisées lorsque des méthodes standard de mise à jour et de transfert de données sont inappropriées. Lorsque le serveur de Destination devient un contrôleur de domaine, il doit contenir les rôles de maître des opérations.  
  
5.  Configure le serveur de Destination en tant que serveur de catalogue global. Le serveur de catalogue global est un contrôleur de domaine qui gère un référentiel de données distribuée. Il contient une représentation partielle et consultable de chaque objet dans chaque domaine dans la forêt ActiveDirectory.  
  
6.  Configure le serveur de Destination en tant que le serveur de licences de site.  
  
##  <a name="BKMK_Install"></a>Installer WindowsServerEssentials sur le serveur de Destination  
 Pour installer et configurer WindowsServerEssentials sur le serveur de Destination en mode migration, effectuez la procédure suivante.  
  
#### <a name="to-install-windows-server-essentials-on-the-destination-server"></a>Pour installer WindowsServerEssentials sur le serveur de Destination  
  
1.  Activez le serveur de Destination et insérez le DVD1 de WindowsServerEssentials dans le lecteur de DVD. Si vous voyez un message vous demandant si vous voulez démarrer à partir d’un CD ou DVD, appuyez sur n’importe quelle touche à le faire.  
  
    > [!NOTE]
    >  Si le serveur de Destination prend en charge le démarrage à partir d’un lecteur flash USB, vous pouvez utiliser le **outil de téléchargement USB/DVD Windows7** pour créer un lecteur Flash USB de démarrage à partir du fichier ISO de WindowsServerEssentials. À l’aide d’un lecteur flash USB peut considérablement accélérer le processus d’installation, car les disques mémoire flash lisent les données beaucoup plus vite que les lecteurs de DVD-ROM. Après avoir créé un lecteur flash USB, vous pouvez ajouter un fichier de réponses sur le lecteur flash USB. Vous pouvez [télécharger l’outil de téléchargement USB/DVD de Windows7](https://go.microsoft.com/fwlink/p/?LinkId=248282) disponible gratuitement sur le site Web MicrosoftStore.  
  
    > [!NOTE]
    >  Si le serveur de Destination ne démarre pas à partir du DVD, redémarrez l’ordinateur et vérifier la configuration du BIOS pour vous assurer que **DVD-ROM** est répertoriée en premier dans la séquence de démarrage. Pour plus d’informations sur la modification de la séquence de démarrage de configuration du BIOS, consultez la documentation du fabricant de votre matériel.  
  
2.  Cliquez sur **nouvelle Installation**.  
  
3.  Si vous avez un disque dur interne qui n’est pas affiché dans la liste, cliquez sur **charger des pilotes** et installez le pilote nécessaire avant de continuer.  
  
4.  Cochez la case qui vérifie tous les fichiers et dossiers sur votre disque dur principal seront supprimés, puis cliquez sur **installer**.  
  
5.  Sur le **choisir le mode d’installation du serveur**, cliquez sur **migration de serveur**, puis fournissez les informations de migration requises.  
  
6.  Lorsque le **votre serveur est migré** message s’affiche, cliquez sur **fermer**.  
  
 Une fois l’installation terminée, vous êtes automatiquement connecté avec le compte d’utilisateur administrateur et le mot de passe que vous avez fournie dans le fichier de réponses de migration.  
  
> [!NOTE]
>  Pour déverrouiller le bureau pendant l’installation de WindowsServerEssentials, utilisez le compte administrateur intégré et laissez le mot de passe vide.  
  
##  <a name="BKMK_VerifyTheHealthOfDC"></a>Vérifier l’intégrité du contrôleur de domaine  
 Avant de procéder à la migration, vous devez vous assurer que le contrôleur de domaine et d’un réseau WindowsServerEssentials sont sains.  
  
 Le tableau suivant répertorie les outils que vous pouvez utiliser pour diagnostiquer les problèmes sur votre serveur de Destination et le réseau et dans le domaine:  
  
|Outil|Description|  
|----------|-----------------|  
|Netdiag|Permet d’isoler les problèmes de connectivité et de mise en réseau. Pour plus d’informations et pour télécharger, voir [Netdiag](https://go.microsoft.com/fwlink/?LinkId=217388).|  
|Dcdiag.exe|Analyse l’état des contrôleurs de domaine dans une forêt ou d’entreprise et signale les problèmes pour vous aider à résoudre. Pour plus d’informations et pour télécharger, voir [Dcdiag](https://go.microsoft.com/fwlink/?LinkId=217389).|  
|Repadmin.exe|Vous aide à diagnostiquer les problèmes de réplication entre les contrôleurs de domaine. Cet outil nécessite des paramètres de ligne de commande à exécuter. Pour plus d’informations et pour télécharger, voir [Repadmin](https://go.microsoft.com/fwlink/?LinkId=217387).|  
  
 Vous devez corriger tous les problèmes de rapport de ces outils avant de procéder à la migration.  
  
> [!NOTE]
>  Si vous envisagez de migrer la messagerie électronique vers un autre serveur d’Exchange sur site, voir [intégrer un serveur On-Premises Exchange Server avec WindowsServerEssentials](../manage/Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md) pour plus d’informations sur la façon de configurer le serveur Exchange local.
