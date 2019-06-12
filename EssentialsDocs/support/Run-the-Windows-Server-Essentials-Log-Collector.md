---
title: Exécuter Windows Server Essentials Log Collector
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0d340223-fa24-4c75-ba8e-b654feb120ab
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5654f28aeda3c231376ed888a8aa04bc0cf3d000
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432488"
---
# <a name="run-the-windows-server-essentials-log-collector"></a>Exécuter Windows Server Essentials Log Collector
Vous pouvez exécuter Windows Server Essentials Log Collector à partir du serveur ou sur un ordinateur sur le réseau. Si vous exécutez Log Collector à partir du serveur, vous pouvez uniquement collecter les journaux du serveur. Si vous exécutez Log Collector à partir d'un ordinateur réseau, vous pouvez choisir de collecter les journaux du serveur en plus de ceux de cet ordinateur.  
  
 Vous devez disposer de privilèges d'administration appropriés pour exécuter Log Collector. Si vous collectez les fichiers journaux d'un serveur, vous devez être un administrateur de serveur ; si vous collectez les fichiers journaux sur un ordinateur réseau, vous devez être un administrateur de client pour cet ordinateur.  
  
#### <a name="to-run-the-log-collector-on-the-server-by-using-the-wizard"></a>Pour exécuter Log Collector sur le serveur à l'aide de l'Assistant  
  
1. Sur le **Démarrer** page du serveur, cliquez sur **Windows Server Essentials Log Collector**.  
  
   > [!NOTE]
   > - Si le programme Log Collector n’apparaît pas sur le **Démarrer** page, accédez à **%system%\Program Files (x86) \Windows Server Essentials Log Collector**, puis double-cliquez sur **LogCollector** .  
   >   -   Si vous n'êtes pas connecté au serveur avec des privilèges d'administration, Log Collector vous invite à entrer vos informations d'identification.  
  
2. Quand vous êtes invité à un emplacement enregistrer les fichiers journaux seront collectés, vous pouvez choisir l’emplacement par défaut,  **\\ \\< nom_serveur\>\logs**, ou spécifiez un autre emplacement. Pour accepter l'emplacement par défaut, cliquez sur **Suivant**. Pour modifier l'emplacement, cliquez sur **Parcourir**, accédez au dossier dans lequel vous souhaitez enregistrer les fichiers journaux, puis cliquez sur **Enregistrer**.  
  
   > [!NOTE]
   >  Vous n'avez pas besoin de fournir les noms des fichiers journaux. Log Collector désigne la collection de fichiers zip en concaténant le nom d’ordinateur et l’horodatage du fichier.  
  
3. Une barre de progression s'affiche pendant la collecte des journaux.  
  
4. Pour afficher le contenu du fichier de collecte des journaux, cochez la case **Ouvrir l'emplacement du fichier dans lequel les journaux ont été enregistrés**, puis cliquez sur **Fermer** pour fermer l'Assistant et ouvrir le fichier de collecte des journaux.  
  
#### <a name="to-run-the-log-collector-on-a-network-computer-by-using-the-wizard"></a>Pour exécuter Log Collector sur un ordinateur réseau à l'aide de l'Assistant  
  
1.  Accédez à **%system%\Program Files (x86) \Windows Server Essentials Log Collector**, puis double-cliquez sur le fichier **LogCollector.exe**.  
  
    > [!NOTE]
    >  Si vous n'êtes pas connecté à l'ordinateur réseau avec des privilèges d'administration, entrez votre nom d'utilisateur et votre mot de passe quand vous y êtes invité, puis cliquez sur **Suivant**.  
  
2.  Sélectionnez les journaux à collecter, comme suit :  
  
    1.  Cochez la case **Fichiers journaux du serveur** pour collecter les fichiers journaux sur le serveur.  
  
    2.  La case **Fichiers journaux de l'ordinateur client (cet ordinateur)** est cochée par défaut, ce qui signifie que Log Collector collecte les journaux de l'ordinateur réseau. Si vous souhaitez uniquement collecter des journaux du serveur, décochez la case **Fichiers journaux de l'ordinateur client (cet ordinateur)** .  
  
    3.  Cliquez sur **Suivant**.  
  
3.  Quand vous y êtes invité, tapez le nom d'utilisateur et le mot de passe d'un administrateur de serveur, puis cliquez sur **Suivant**.  
  
4.  Tapez ou recherchez l'emplacement dans lequel vous souhaitez enregistrer les fichiers journaux, puis cliquez sur **Suivant**.  
  
    > [!NOTE]
    >  Vous n'avez pas besoin de fournir les noms des fichiers journaux. Log Collector désigne la collection de fichiers zip en concaténant le nom d’ordinateur et l’horodatage du fichier.  
  
5.  Une barre de progression s'affiche pendant la collecte des journaux.  
  
6.  Pour afficher le contenu du fichier de collecte des journaux, cochez la case **Ouvrir l'emplacement du fichier dans lequel les journaux ont été enregistrés**, puis cliquez sur **Fermer** pour fermer l'Assistant et ouvrir le fichier de collecte des journaux.  
  
### <a name="running-the-log-collector-manually"></a>Exécution manuelle de Log Collector  
 Une fois Log Collector installé, une tâche planifiée est créée pour exécuter l'outil. Vous pouvez ensuite exécuter Log Collector à partir du **Gestionnaire de tâches planifiées** sans passer par l'Assistant si le démarrage de celui-ci pose problème.  
  
##### <a name="to-manually-run-the-log-collector-on-the-server"></a>Pour exécuter manuellement Log Collector sur le serveur  
  
1.  Connectez-vous directement ou à distance au serveur.  
  
2.  Ouvrez le **Planificateur de tâches**.  
  
3.  À la racine de la **Bibliothèque du Planificateur de tâches**, accédez à la tâche planifiée nommée **LogCollector**.  
  
4.  Cliquez avec le bouton droit sur **LogCollector**, puis cliquez sur **Exécuter**. Log Collector enregistre les journaux dans le dossier par défaut sur le serveur,  **\\ \\< nom_serveur\>\Logs**. Si vous n’avez pas l’autorisation d’écriture pour le dossier ou le dossier n’existe pas, les journaux sont placés dans le **< temp\>**  sous-répertoire.  
  
##### <a name="to-manually-run-the-log-collector-on-a-network-computer"></a>Pour exécuter manuellement Log Collector sur un ordinateur réseau  
  
1.  Connectez-vous directement ou à distance à l'ordinateur réseau.  
  
2.  Ouvrez le **Planificateur de tâches**.  
  
3.  À la racine de la **Bibliothèque du Planificateur de tâches**, accédez à la tâche planifiée nommée **LogCollector**.  
  
4.  Cliquez avec le bouton droit sur **LogCollector**, puis cliquez sur **Exécuter**. Log Collector enregistre les journaux dans le **< temp\>**  dossier sur l’ordinateur réseau.
