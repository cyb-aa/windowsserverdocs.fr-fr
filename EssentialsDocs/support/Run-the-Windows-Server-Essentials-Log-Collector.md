---
title: "Exécuter WindowsServerEssentialsLogCollector"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.openlocfilehash: 6b49fee7ca4a19d5a501cf96c1ce356f8242c81f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="run-the-windows-server-essentials-log-collector"></a>Exécuter WindowsServerEssentialsLogCollector
Vous pouvez exécuter WindowsServerEssentialsLogCollector à partir du serveur ou un ordinateur sur le réseau. Si vous exécutez Log Collector à partir du serveur, vous pouvez uniquement collecter les journaux à partir du serveur. Si vous exécutez Log Collector à partir d’un ordinateur réseau, vous pouvez choisir de collecter les journaux à partir du serveur, outre les journaux pour cet ordinateur.  
  
 Vous devez disposer des privilèges d’administration appropriés pour exécuter Log Collector. Si vous collectez les fichiers journaux pour un serveur, vous devez être un administrateur de serveur; si vous collectez les fichiers journaux sur un ordinateur réseau, vous devez être un administrateur Client pour cet ordinateur.  
  
#### <a name="to-run-the-log-collector-on-the-server-by-using-the-wizard"></a>Pour exécuter Log Collector sur le serveur à l’aide de l’Assistant  
  
1.  Sur le **Démarrer** page du serveur, cliquez sur **WindowsServerEssentialsLogCollector**.  
  
    > [!NOTE]
    >  -   Si le programme Log Collector n’apparaît pas sur le **Démarrer** page, accédez à **%system%\Program Files (x86) \WindowsServerEssentialsLogCollector**, puis double-cliquez sur **LogCollector**.  
    > -   Si vous n’êtes pas connecté au serveur avec des privilèges d’administration, Log Collector vous invite à entrer vos informations d’identification.  
  
2.  Lorsque vous êtes invité à un emplacement enregistrer les fichiers journaux seront collectés, vous pouvez choisir l’emplacement par défaut, **\\\ < serverName\ > \logs**, ou spécifier un autre emplacement. Pour accepter l’emplacement par défaut, cliquez sur **suivant**. Pour modifier l’emplacement, cliquez sur **Parcourir**, accédez au dossier où vous voulez enregistrer les fichiers journaux, puis cliquez sur **enregistrer**.  
  
    > [!NOTE]
    >  Vous n’avez pas besoin de fournir des noms de fichiers pour les fichiers journaux. Log Collector désigne la collection de fichiers zip en concaténant le nom d’ordinateur et l’horodatage du fichier.  
  
3.  Une barre de progression s’affiche pendant les journaux sont collectés.  
  
4.  Pour afficher le contenu du fichier journal regroupement, sélectionnez le **ouvrir l’emplacement du fichier dans lequel les journaux ont été enregistrés** case à cocher, puis cliquez sur **fermer** pour fermer l’Assistant et ouvrir le fichier de collecte de journaux.  
  
#### <a name="to-run-the-log-collector-on-a-network-computer-by-using-the-wizard"></a>Pour exécuter Log Collector sur un ordinateur réseau à l’aide de l’Assistant  
  
1.  Accédez à **%system%\Program Files (x86) \WindowsServerEssentialsLogCollector**, puis double-cliquez sur le fichier **LogCollector.exe**.  
  
    > [!NOTE]
    >  Si vous n’êtes pas connecté à l’ordinateur réseau avec des privilèges d’administration, entrez votre nom d’utilisateur et un mot de passe lorsque vous y êtes invité, puis cliquez sur **suivant**.  
  
2.  Sélectionnez les journaux que vous souhaitez collecter, comme suit:  
  
    1.  Sélectionnez le **fichiers journaux du serveur** case à cocher pour collecter les fichiers journaux sur le serveur.  
  
    2.  Le **fichiers journaux de l’ordinateur Client (cet ordinateur)** case à cocher est activée par défaut, indiquant que le collecteur de journaux collecte les journaux de l’ordinateur réseau exécutant. Si vous souhaitez uniquement collecter les journaux du serveur, désactivez le **fichiers journaux de l’ordinateur Client (cet ordinateur)** case à cocher.  
  
    3.  Cliquez sur **suivant**.  
  
3.  Lorsque vous y êtes invité, tapez le nom d’utilisateur et mot de passe pour un administrateur de serveur, puis cliquez sur **suivant**.  
  
4.  Tapez ou recherchez l’emplacement où vous souhaitez enregistrer les fichiers journaux, puis cliquez sur **suivant**.  
  
    > [!NOTE]
    >  Vous n’avez pas besoin de fournir des noms de fichiers pour les fichiers journaux. Log Collector désigne la collection de fichiers zip en concaténant le nom d’ordinateur et l’horodatage du fichier.  
  
5.  Une barre de progression s’affiche pendant les journaux sont collectés.  
  
6.  Pour afficher le contenu du fichier journal regroupement, sélectionnez le **ouvrir l’emplacement du fichier dans lequel les journaux ont été enregistrés** case à cocher, puis cliquez sur **fermer** pour fermer l’Assistant et ouvrir le fichier de collecte de journaux.  
  
### <a name="running-the-log-collector-manually"></a>Exécution manuelle de Log Collector  
 Une fois le collecteur de journaux est installé, une tâche planifiée est créée pour exécuter l’outil. Vous pouvez ensuite exécuter Log Collector à partir de la **Gestionnaire des tâches planifiées** sans l’aide de l’Assistant, s’il existe des problèmes de démarrage de l’Assistant.  
  
##### <a name="to-manually-run-the-log-collector-on-the-server"></a>Pour exécuter manuellement Log Collector sur le serveur  
  
1.  Connectez-vous directement ou à distance au serveur.  
  
2.  Ouvrez le **Planificateur de tâches**.  
  
3.  Dans la racine de la **bibliothèque du Planificateur de tâches**, accédez à la tâche planifiée nommée **LogCollector**.  
  
4.  Avec le bouton droit **LogCollector**, puis cliquez sur **exécuter**. Log Collector enregistre les journaux dans le dossier par défaut sur le serveur, **\\\ < serverName\ > \Logs**. Si vous n’avez pas accès en écriture pour le dossier ou le dossier n’existe pas, les journaux sont placés dans le **< temp\ >** sous-répertoire.  
  
##### <a name="to-manually-run-the-log-collector-on-a-network-computer"></a>Pour exécuter manuellement Log Collector sur un ordinateur réseau  
  
1.  Connectez-vous directement ou à distance à l’ordinateur réseau.  
  
2.  Ouvrez le **Planificateur de tâches**.  
  
3.  Dans la racine de la **bibliothèque du Planificateur de tâches**, accédez à la tâche planifiée nommée **LogCollector**.  
  
4.  Avec le bouton droit **LogCollector**, puis cliquez sur **exécuter**. Log Collector enregistre les journaux dans le **< temp\ >** dossier sur l’ordinateur réseau.
