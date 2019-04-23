---
title: Installer Windows Server Essentials en mode de migration 1
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847690"
---
# <a name="install-windows-server-essentials-in-migration-mode1"></a>Installer Windows Server Essentials en mode de migration 1

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez avoir qu’un seul serveur sur votre réseau qui exécute Windows Server Essentials, et ce serveur doit être un contrôleur de domaine pour le réseau.  
  
 Lorsque vous installez Windows Server Essentials en mode migration, l’Assistant installation effectue les tâches suivantes :  
  
1.  Installe et configure le logiciel de serveur Windows Server Essentials sur le serveur de Destination.  
  
2.  Met à jour le schéma de domaine vers la version la plus récente.  
  
3.  Joint le serveur de destination au domaine existant. Le serveur source et le serveur de destination peuvent tous deux être membres du même domaine jusqu'à ce que la migration soit terminée. Une fois la migration terminée, vous devez supprimer le serveur source du réseau dans un délai de 21 jours.  
  
    > [!WARNING]
    >  Un message d'erreur est ajouté au journal des événements tous les jours pendant la période de grâce de 21 jours jusqu'à la suppression du serveur source de votre réseau. Le message est le suivant : « La vérification du rôle FSMO a détecté dans votre environnement une condition non conforme à la stratégie de licence. Le Serveur d'administration doit contenir les rôles Active Directory Contrôleur de domaine principal et Maître d’opérations des noms de domaine. Placez les rôles Active Directory sur le Serveur d'administration. Ce serveur sera automatiquement arrêté si le problème n'est pas résolu dans les 21 jours à partir de la première détection de la condition. » Après la période de grâce de 21 jours, le serveur source s'arrête.  
  
4.  Transfère les rôles de maître des opérations (également appelées opérations à maître unique flottant ou FSMO) du serveur source vers le serveur de destination. Les rôles de maître des opérations sont des tâches spécialisées de contrôleur de domaine, utilisées quand les méthodes standard de mise à jour et de transfert de données sont inappropriées. Lorsque le serveur de destination devient un contrôleur de domaine, il doit tenir les rôles de maître d’opérations.  
  
5.  Configuration du serveur de destination en tant que serveur de catalogue global. Le serveur de catalogue global est un contrôleur de domaine qui gère un référentiel de données diffusées. Il contient une représentation partielle et consultable de chaque objet dans chaque domaine de la forêt Active Directory.  
  
6.  Configuration du serveur de destination en tant que serveur de licences de site.  
  
##  <a name="BKMK_Install"></a> Installer Windows Server Essentials sur le serveur de Destination  
 Pour installer et configurer Windows Server Essentials sur le serveur de Destination en mode migration, effectuez la procédure suivante.  
  
#### <a name="to-install-windows-server-essentials-on-the-destination-server"></a>Pour installer Windows Server Essentials sur le serveur de Destination  
  
1.  Activer le serveur de Destination et insérez le DVD1 de Windows Server Essentials dans le lecteur de DVD. Si un message s'affiche vous demandant si vous voulez démarrer à partir d'un CD ou DVD, appuyez sur une touche pour confirmer.  
  
    > [!NOTE]
    >  Si le serveur de Destination prend en charge le démarrage à partir d’un lecteur flash USB, vous pouvez utiliser la **outil de téléchargement de Windows 7 USB/DVD** pour créer un lecteur Flash USB démarrable à partir du fichier ISO Windows Server Essentials. L’utilisation d’un disque mémoire USB peut considérablement accélérer le processus d’installation, car les disques mémoire lisent les données beaucoup plus vite que les lecteurs de DVD-ROM. Après avoir créé un disque mémoire USB de démarrage, vous pouvez ajouter un fichier de réponses au disque mémoire. Vous pouvez [télécharger l’outil de téléchargement USB/DVD de Windows 7](https://go.microsoft.com/fwlink/p/?LinkId=248282) disponible gratuitement sur le site Web Microsoft Store.  
  
    > [!NOTE]
    >  Si le serveur de destination ne démarre pas à partir du DVD, redémarrez l'ordinateur et vérifiez la configuration du BIOS pour vous assurer que **DVD-ROM** s'affiche en premier dans la séquence de démarrage. Pour plus d'informations sur la modification de la séquence de démarrage dans la configuration du BIOS, consultez la documentation du fabricant de votre matériel.  
  
2.  Cliquez sur **Nouvelle installation**.  
  
3.  Si un disque dur interne n'est pas affiché dans la liste, cliquez sur **Charger des pilotes** et installez le pilote nécessaire avant de continuer.  
  
4.  Cochez la case qui vérifie que tous les fichiers et dossiers de votre disque dur principal seront supprimés, puis cliquez sur **Installer**.  
  
5.  Dans la page **Choisir le mode d'installation du serveur**, cliquez sur **Migration du serveur**, puis fournissez les informations de migration requises.  
  
6.  Quand le message **Votre serveur a correctement migré** s'affiche, cliquez sur **Fermer**.  
  
 Une fois l'installation terminée, vous êtes automatiquement connecté avec le compte d'administrateur et le mot de passe que vous avez fournis dans le fichier de réponses de migration.  
  
> [!NOTE]
>  Pour déverrouiller le bureau pendant l’installation de Windows Server Essentials, utilisez le compte administrateur intégré et laissez le mot de passe vide.  
  
##  <a name="BKMK_VerifyTheHealthOfDC"></a> Vérifier l’intégrité du contrôleur de domaine  
 Avant de procéder à la migration, vous devez vous assurer que le contrôleur de domaine et le réseau de Windows Server Essentials sont intègres.  
  
 Le tableau suivant répertorie les outils que vous pouvez utiliser pour diagnostiquer les problèmes sur votre serveur de destination et le réseau, ainsi que dans le domaine :  
  
|Tool|Description|  
|----------|-----------------|  
|Netdiag|Permet d’isoler les problèmes de réseau et de connectivité. Pour obtenir plus d’informations et effectuer le téléchargement, consultez [Netdiag](https://go.microsoft.com/fwlink/?LinkId=217388).|  
|Dcdiag.exe|Analyse l’état des contrôleurs de domaine dans une forêt ou entreprise, et signale les problèmes pour vous aider à les résoudre. Pour obtenir plus d’informations et effectuer le téléchargement, consultez [Dcdiag](https://go.microsoft.com/fwlink/?LinkId=217389).|  
|Repadmin.exe|Vous aide à diagnostiquer les problèmes de réplication entre les contrôleurs de domaine. Cet outil nécessite l’exécution de paramètres de ligne de commande. Pour obtenir plus d’informations et effectuer le téléchargement, consultez [Repadmin](https://go.microsoft.com/fwlink/?LinkId=217387).|  
  
 Vous devez corriger tous les problèmes signalés par ces outils avant de poursuivre la migration.  
  
> [!NOTE]
>  Si vous envisagez de migrer la messagerie électronique vers un autre serveur d’Exchange sur site, consultez [intégrer un On-Premises Exchange Server avec Windows Server Essentials](../manage/Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md) pour plus d’informations sur la façon de configurer votre serveur d’Exchange sur site.
