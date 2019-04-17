---
title: Exporter une Configuration du serveur NPS pour importer sur un autre serveur
description: Vous pouvez utiliser cette rubrique pour savoir comment exporter une configuration de serveur de stratégie réseau dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 141a99e930672d8403315cb6804290d184ef3007
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="export-an-nps-server-configuration-for-import-on-another-server"></a>Exporter une Configuration du serveur NPS pour importer sur un autre serveur

S’applique à: Windows Server2016

Vous pouvez exporter la configuration NPS entière, y compris les clients RADIUS et serveurs de stratégie réseau, stratégie de demande de connexion, Registre et configuration de la journalisation: à partir d’un serveur NPS pour importer sur un autre serveur NPS. 

Pour exporter la configuration NPS, utilisez un des outils suivants:

- Dans Windows Server2016, Windows Server2012R2 et Windows Server2012, vous pouvez utiliser l’outil Netsh ou vous pouvez utiliser Windows PowerShell.
- Dans Windows Server2008R2 et Windows Server2008, utilisez Netsh.

>[!IMPORTANT]
>N’utilisez pas cette procédure si la base de données du serveur NPS source a un numéro de version supérieur au numéro de version de la base de données du serveur NPS de destination. Vous pouvez afficher le numéro de version de la base de données du serveur NPS à l’affichage de la **netsh nps Afficher config** commande.

Étant donné que les configurations de serveur NPS ne sont pas chiffrées dans le fichier XML exporté, envoi qu'il sur un réseau peut poser un risque de sécurité, par conséquent, prenez des précautions lorsque vous déplacez le fichier XML à partir du serveur source pour les serveurs de destination. Par exemple, ajoutez le fichier à un fichier d’archive protégé de mot de passe chiffrés, avant de déplacer le fichier. En outre, stockez le fichier dans un emplacement sécurisé pour empêcher les utilisateurs malveillants d’y accéder.

>[!NOTE]
>Si la journalisation SQLServer est configurée sur le serveur NPS source, les paramètres de journalisation SQLServer ne sont pas exportés dans le fichier XML. Après avoir importé le fichier sur un autre serveur NPS, vous devez configurer manuellement la journalisation SQLServer.

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>Exporter et importer la configuration NPS à l’aide de Windows PowerShell

Pour Windows Server2012 et les versions de système d’exploitation ultérieures, vous pouvez exporter la configuration NPS à l’aide de Windows PowerShell.

La syntaxe de commande pour l’exportation de la configuration NPS est la suivante. 

    Export-NpsConfiguration -Path <filename>

Le tableau suivant répertorie les paramètres pour le **Export-NpsConfiguration** applet de commande dans Windows PowerShell. Paramètres en gras sont requis.

|Paramètre|Description|
|---------|-----------|
|Chemin d’accès|Spécifie le nom et l’emplacement du fichier XML auquel vous souhaitez exporter la configuration du serveur NPS.|

**Informations d’identification d’administration**

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

### <a name="export-example"></a>Exemple d’exportation 

Dans l’exemple suivant, la configuration NPS est exportée vers un fichier XML situé sur le lecteur local. Pour exécuter cette commande, exécutez Windows PowerShell en tant qu’administrateur sur le serveur NPS source, tapez la commande suivante et appuyez sur ENTRÉE.

`Export-NpsConfiguration –Path c:\config.xml` 

Pour plus d’informations, voir [Export-NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx).

Après avoir exporté la configuration NPS, copiez le fichier XML pour le serveur de destination.

La syntaxe de commande pour importer la configuration du serveur NPS sur le serveur de destination est la suivante.

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>Exemple d’importation

La commande suivante importe les paramètres à partir du fichier nommé C:\Npsconfig.xml au serveur NPS. Pour exécuter cette commande, exécutez Windows PowerShell en tant qu’administrateur sur le serveur NPS de destination, tapez la commande suivante et appuyez sur ENTRÉE.

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

Pour plus d’informations, voir [Import-NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx).

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>Exporter et importer la configuration NPS à l’aide de Netsh

Vous pouvez utiliser l’environnement réseau \(Netsh\) pour exporter la configuration du serveur NPS à l’aide de la **netsh nps exporter** commande.

Lorsque le **netsh nps importer** commande est exécutée, le serveur NPS est automatiquement actualisé avec les paramètres de configuration mis à jour. Vous n’avez pas besoin d’arrêter le serveur NPS sur l’ordinateur de destination pour exécuter le **netsh nps importer** toutefois si la console NPS ou le composant logiciel enfichable MMC NPS est ouvert pendant l’importation de la configuration, les modifications apportées à la configuration du serveur ne sont pas visibles jusqu'à ce que vous actualisez l’affichage de la commande. 

>[!NOTE]
>Lorsque vous utilisez le **netsh nps exporter** de commande, vous devez fournir le paramètre de commande **exportPSK** avec la valeur **Oui**. Ce paramètre et la valeur explicitement d’état que vous comprenez que vous exportez la configuration du serveur NPS, et que le fichier XML exporté contienne non chiffrée des secrets partagés pour les clients RADIUS et les membres de groupes de serveurs RADIUS distants.

**Informations d’identification d’administration**

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

### <a name="to-copy-an-nps-server-configuration-to-another-nps-server-using-netsh-commands"></a>Pour copier une configuration du serveur NPS vers un autre serveur NPS à l’aide des commandes Netsh

1. Sur le serveur NPS source, ouvrez **invite de commandes**, type **netsh**, puis appuyez sur ENTRÉE.

2. À la **netsh** tapez **nps**, puis appuyez sur ENTRÉE. 

3. À la **netsh nps** invite, tapez **exporter filename =**«*path\file.xml*» **exportPSK = YES**, où *chemin d’accès* est l’emplacement du dossier où vous souhaitez enregistrer des fichiers de configuration, le serveur NPS et *fichier* est le nom du fichier XML que vous souhaitez enregistrer. Appuyez sur ENTRÉE. 

Cette option stocke les paramètres de configuration \(including registry settings\) dans un fichier XML. Le chemin d’accès peut être relatif ou absolu, ou il peut être un chemin d’accès UNC \(UNC\). Une fois que vous appuyez sur entrée, un message s’affiche indiquant si l’exportation vers le fichier a réussi.

4. Copiez le fichier que vous avez créé sur le serveur NPS de destination.

5. À l’invite de commandes sur le serveur NPS de destination, tapez **netsh nps importer filename =**«*path\file.xml*», puis appuyez sur ENTRÉE. Un message s’affiche indiquant si l’importation à partir du fichier XML a réussi.

Pour plus d’informations sur netsh, consultez [Network Shell (Netsh)](../netsh/netsh.md).

