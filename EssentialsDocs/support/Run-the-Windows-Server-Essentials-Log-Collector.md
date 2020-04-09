---
title: Exécuter Windows Server Essentials Log Collector
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 0d340223-fa24-4c75-ba8e-b654feb120ab
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6aac2ed382321349d39874c7db7617f6da845919
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852272"
---
# <a name="run-the-windows-server-essentials-log-collector"></a>Exécuter Windows Server Essentials Log Collector
Vous pouvez exécuter le collecteur de journaux Windows Server Essentials à partir du serveur ou d’un ordinateur sur le réseau. Si vous exécutez Log Collector à partir du serveur, vous pouvez uniquement collecter les journaux du serveur. Si vous exécutez Log Collector à partir d'un ordinateur réseau, vous pouvez choisir de collecter les journaux du serveur en plus de ceux de cet ordinateur.  
  
 Vous devez disposer de privilèges d'administration appropriés pour exécuter Log Collector. Si vous collectez les fichiers journaux d'un serveur, vous devez être un administrateur de serveur ; si vous collectez les fichiers journaux sur un ordinateur réseau, vous devez être un administrateur de client pour cet ordinateur.  
  
#### <a name="to-run-the-log-collector-on-the-server-by-using-the-wizard"></a>Pour exécuter Log Collector sur le serveur à l'aide de l'Assistant  
  
1. Sur la page de **démarrage** du serveur, cliquez sur **collecteur de journaux Windows Server Essentials**.  
  
   > [!NOTE]
   > - Si le programme collecteur de journaux n’apparaît pas dans la page de **démarrage** , accédez à **%System%\Program Files Files (x86) \Windows Server Essentials log Collector**, puis double-cliquez sur **LogCollector**.  
   >   -   Si vous n'êtes pas connecté au serveur avec des privilèges d'administration, Log Collector vous invite à entrer vos informations d'identification.  
  
2. Lorsque vous êtes invité à entrer un emplacement dans lequel enregistrer les fichiers journaux collectés, vous pouvez choisir l’emplacement par défaut, **\\\\< ServerName\>\Logs**ou spécifier un autre emplacement. Pour accepter l'emplacement par défaut, cliquez sur **Suivant**. Pour modifier l'emplacement, cliquez sur **Parcourir**, accédez au dossier dans lequel vous souhaitez enregistrer les fichiers journaux, puis cliquez sur **Enregistrer**.  
  
   > [!NOTE]
   >  Vous n'avez pas besoin de fournir les noms des fichiers journaux. Le collecteur de journaux nomme la collection de fichiers zip en concaténant le nom de l’ordinateur et l’horodatage du fichier.  
  
3. Une barre de progression s'affiche pendant la collecte des journaux.  
  
4. Pour afficher le contenu du fichier de collecte des journaux, cochez la case **Ouvrir l'emplacement du fichier dans lequel les journaux ont été enregistrés**, puis cliquez sur **Fermer** pour fermer l'Assistant et ouvrir le fichier de collecte des journaux.  
  
#### <a name="to-run-the-log-collector-on-a-network-computer-by-using-the-wizard"></a>Pour exécuter Log Collector sur un ordinateur réseau à l'aide de l'Assistant  
  
1.  Accédez à **fichiers%System%\Program Files (x86) \Windows Server Essentials log Collector**, puis double-cliquez sur le fichier **LogCollector. exe**.  
  
    > [!NOTE]
    >  Si vous n'êtes pas connecté à l'ordinateur réseau avec des privilèges d'administration, entrez votre nom d'utilisateur et votre mot de passe quand vous y êtes invité, puis cliquez sur **Suivant**.  
  
2.  Sélectionnez les journaux à collecter, comme suit :  
  
    1.  Cochez la case **Fichiers journaux du serveur** pour collecter les fichiers journaux sur le serveur.  
  
    2.  La case **Fichiers journaux de l'ordinateur client (cet ordinateur)** est cochée par défaut, ce qui signifie que Log Collector collecte les journaux de l'ordinateur réseau. Si vous souhaitez uniquement collecter des journaux du serveur, décochez la case **Fichiers journaux de l'ordinateur client (cet ordinateur)** .  
  
    3.  Cliquez sur **Suivant**.  
  
3.  Quand vous y êtes invité, tapez le nom d'utilisateur et le mot de passe d'un administrateur de serveur, puis cliquez sur **Suivant**.  
  
4.  Tapez ou recherchez l'emplacement dans lequel vous souhaitez enregistrer les fichiers journaux, puis cliquez sur **Suivant**.  
  
    > [!NOTE]
    >  Vous n'avez pas besoin de fournir les noms des fichiers journaux. Le collecteur de journaux nomme la collection de fichiers zip en concaténant le nom de l’ordinateur et l’horodatage du fichier.  
  
5.  Une barre de progression s'affiche pendant la collecte des journaux.  
  
6.  Pour afficher le contenu du fichier de collecte des journaux, cochez la case **Ouvrir l'emplacement du fichier dans lequel les journaux ont été enregistrés**, puis cliquez sur **Fermer** pour fermer l'Assistant et ouvrir le fichier de collecte des journaux.  
  
### <a name="running-the-log-collector-manually"></a>Exécution manuelle de Log Collector  
 Une fois Log Collector installé, une tâche planifiée est créée pour exécuter l'outil. Vous pouvez ensuite exécuter Log Collector à partir du **Gestionnaire de tâches planifiées** sans passer par l'Assistant si le démarrage de celui-ci pose problème.  
  
##### <a name="to-manually-run-the-log-collector-on-the-server"></a>Pour exécuter manuellement Log Collector sur le serveur  
  
1.  Connectez-vous directement ou à distance au serveur.  
  
2.  Ouvrez le **Planificateur de tâches**.  
  
3.  À la racine de la **Bibliothèque du Planificateur de tâches**, accédez à la tâche planifiée nommée **LogCollector**.  
  
4.  Cliquez avec le bouton droit sur **LogCollector**, puis cliquez sur **Exécuter**. Le collecteur de journaux place les journaux dans le dossier par défaut sur le serveur, **\\\\< ServerName\>\Logs**. Si vous n’avez pas d’autorisation d’écriture sur le dossier ou si le dossier n’existe pas, les journaux sont placés dans le sous-répertoire **temp\><** .  
  
##### <a name="to-manually-run-the-log-collector-on-a-network-computer"></a>Pour exécuter manuellement Log Collector sur un ordinateur réseau  
  
1.  Connectez-vous directement ou à distance à l'ordinateur réseau.  
  
2.  Ouvrez le **Planificateur de tâches**.  
  
3.  À la racine de la **Bibliothèque du Planificateur de tâches**, accédez à la tâche planifiée nommée **LogCollector**.  
  
4.  Cliquez avec le bouton droit sur **LogCollector**, puis cliquez sur **Exécuter**. Le collecteur de journaux place les journaux dans le dossier **< temp\>** sur l’ordinateur réseau.
