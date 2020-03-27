---
title: Installer Windows Server Essentials dans la migration MODE1
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fd7196ac-cfa6-46a5-ba77-6962b47a825e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: dbbd9f7303995e1547e48aa9701467b45e4bad34
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318996"
---
# <a name="install-windows-server-essentials-in-migration-mode1"></a>Installer Windows Server Essentials dans la migration MODE1

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous ne pouvez avoir qu’un seul serveur sur votre réseau qui exécute Windows Server Essentials, et ce serveur doit être un contrôleur de domaine pour le réseau.  
  
 Quand vous installez Windows Server Essentials en mode migration, l’Assistant installation effectue les tâches suivantes :  
  
1.  Installe et configure le logiciel serveur Windows Server Essentials sur le serveur de destination.  
  
2.  Met à jour le schéma de domaine vers la version la plus récente.  
  
3.  Joint le serveur de destination au domaine existant. Le serveur source et le serveur de destination peuvent tous deux être membres du même domaine jusqu'à ce que la migration soit terminée. Une fois la migration terminée, vous devez supprimer le serveur source du réseau dans un délai de 21 jours.  
  
    > [!WARNING]
    >  Un message d'erreur est ajouté au journal des événements tous les jours pendant la période de grâce de 21 jours jusqu'à la suppression du serveur source de votre réseau. Le message est le suivant : « La vérification du rôle FSMO a détecté dans votre environnement une condition non conforme à la stratégie de licence. Le Serveur d'administration doit contenir les rôles Active Directory Contrôleur de domaine principal et Maître d'opérations des noms de domaine. Placez les rôles Active Directory sur le Serveur d'administration. Ce serveur sera automatiquement arrêté si le problème n'est pas résolu dans les 21 jours à partir de la première détection de la condition. » Après la période de grâce de 21 jours, le serveur source s'arrête.  
  
4.  Transfère les rôles de maître des opérations (également appelées opérations à maître unique flottant ou FSMO) du serveur source vers le serveur de destination. Les rôles de maître des opérations sont des tâches spécialisées de contrôleur de domaine, utilisées quand les méthodes standard de mise à jour et de transfert de données sont inappropriées. Lorsque le serveur de destination devient un contrôleur de domaine, il doit tenir les rôles de maître d’opérations.  
  
5.  Configuration du serveur de destination en tant que serveur de catalogue global. Le serveur de catalogue global est un contrôleur de domaine qui gère un référentiel de données diffusées. Il contient une représentation partielle et consultable de chaque objet dans chaque domaine de la forêt Active Directory.  
  
6.  Configuration du serveur de destination en tant que serveur de licences de site.  
  
##  <a name="install-windows-server-essentials-on-the-destination-server"></a><a name="BKMK_Install"></a>Installer Windows Server Essentials sur le serveur de destination  
 Pour installer et configurer Windows Server Essentials sur le serveur de destination en mode migration, effectuez la procédure suivante.  
  
#### <a name="to-install-windows-server-essentials-on-the-destination-server"></a>Pour installer Windows Server Essentials sur le serveur de destination  
  
1. Activez le serveur de destination et insérez Windows Server Essentials le DVD1 dans le lecteur de DVD. Si un message s'affiche vous demandant si vous voulez démarrer à partir d'un CD ou DVD, appuyez sur une touche pour confirmer.  
  
   > [!NOTE]
   >  Si le serveur de destination prend en charge le démarrage à partir d’un disque mémoire flash USB, vous pouvez utiliser l' **outil de téléchargement USB/DVD de Windows 7** pour créer un lecteur flash USB de démarrage à partir du fichier ISO de Windows Server Essentials. L’utilisation d’un disque mémoire USB peut considérablement accélérer le processus d’installation, car les disques mémoire lisent les données beaucoup plus vite que les lecteurs de DVD-ROM. Après avoir créé un disque mémoire USB de démarrage, vous pouvez ajouter un fichier de réponses au disque mémoire. Vous pouvez [Télécharger l’outil de téléchargement USB/DVD de Windows 7](https://go.microsoft.com/fwlink/p/?LinkId=248282) gratuitement sur Microsoft Store site Web.  
  
   > [!NOTE]
   >  Si le serveur de destination ne démarre pas à partir du DVD, redémarrez l'ordinateur et vérifiez la configuration du BIOS pour vous assurer que **DVD-ROM** s'affiche en premier dans la séquence de démarrage. Pour plus d'informations sur la modification de la séquence de démarrage dans la configuration du BIOS, consultez la documentation du fabricant de votre matériel.  
  
2. Cliquez sur **Nouvelle installation**.  
  
3. Si un disque dur interne n'est pas affiché dans la liste, cliquez sur **Charger des pilotes** et installez le pilote nécessaire avant de continuer.  
  
4. Cochez la case qui vérifie que tous les fichiers et dossiers de votre disque dur principal seront supprimés, puis cliquez sur **Installer**.  
  
5. Dans la page **Choisir le mode d'installation du serveur**, cliquez sur **Migration du serveur**, puis fournissez les informations de migration requises.  
  
6. Quand le message **Votre serveur a correctement migré** s'affiche, cliquez sur **Fermer**.  
  
   Une fois l'installation terminée, vous êtes automatiquement connecté avec le compte d'administrateur et le mot de passe que vous avez fournis dans le fichier de réponses de migration.  
  
> [!NOTE]
>  Pour déverrouiller le bureau pendant l’installation de Windows Server Essentials, utilisez le compte d’administrateur intégré et laissez le mot de passe vide.  
  
##  <a name="verify-the-health-of-the-domain-controller"></a><a name="BKMK_VerifyTheHealthOfDC"></a>Vérifier l’intégrité du contrôleur de domaine  
 Avant de procéder à la migration, vous devez vous assurer que le contrôleur de domaine et le réseau Windows Server Essentials sont intègres.  
  
 Le tableau suivant répertorie les outils que vous pouvez utiliser pour diagnostiquer les problèmes sur votre serveur de destination et le réseau, ainsi que dans le domaine :  
  
|Outil|Description|  
|----------|-----------------|  
|Netdiag|Permet d’isoler les problèmes de réseau et de connectivité. Pour obtenir plus d’informations et effectuer le téléchargement, consultez [Netdiag](https://go.microsoft.com/fwlink/?LinkId=217388).|  
|Dcdiag.exe|Analyse l’état des contrôleurs de domaine dans une forêt ou entreprise, et signale les problèmes pour vous aider à les résoudre. Pour obtenir plus d’informations et effectuer le téléchargement, consultez [Dcdiag](https://go.microsoft.com/fwlink/?LinkId=217389).|  
|Repadmin.exe|Vous aide à diagnostiquer les problèmes de réplication entre les contrôleurs de domaine. Cet outil nécessite l’exécution de paramètres de ligne de commande. Pour obtenir plus d’informations et effectuer le téléchargement, consultez [Repadmin](https://go.microsoft.com/fwlink/?LinkId=217387).|  
  
 Vous devez corriger tous les problèmes signalés par ces outils avant de poursuivre la migration.  
  
> [!NOTE]
>  Si vous envisagez de migrer la messagerie électronique vers un autre serveur Exchange local, consultez [intégrer un serveur Exchange local à Windows Server Essentials](../manage/Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md) pour plus d’informations sur la configuration de votre serveur Exchange local.
