---
title: Exporter une Configuration de serveur NPS pour l’importation sur un autre serveur
description: Vous pouvez utiliser cette rubrique pour savoir comment exporter une configuration de serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b95e39af63e284d0147335faabfb740c0dd175bc
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812295"
---
# <a name="export-an-nps-configuration-for-import-on-another-server"></a>Exporter une Configuration de serveur NPS pour l’importation sur un autre serveur

S'applique à : Windows Server 2016

Vous pouvez exporter la configuration NPS entière, y compris les clients RADIUS et serveurs de stratégie réseau, stratégie de demande de connexion, Registre et la configuration de journalisation, à partir d’un serveur NPS pour l’importation sur un autre serveur NPS. 

Pour exporter la configuration NPS, utilisez un des outils suivants :

- Dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012, vous pouvez utiliser l’outil Netsh ou vous pouvez utiliser Windows PowerShell.
- Dans Windows Server 2008 R2 et Windows Server 2008, utilisez Netsh.

> [!IMPORTANT]
> N’utilisez pas cette procédure si la base de données du serveur NPS source a un numéro de version supérieur au numéro de version de la base de données de serveur NPS de destination. Vous pouvez afficher le numéro de version de la base de données de serveur NPS à partir de l’affichage de la **netsh nps Afficher config** commande.

Étant donné que les configurations de serveur NPS ne sont pas chiffrées dans le fichier XML exporté, envoi sur un réseau peut poser un risque de sécurité, par conséquent, prendre des précautions lorsque vous déplacez le fichier XML à partir du serveur source vers les serveurs de destination. Par exemple, ajouter le fichier à un fichier d’archive protégé de mot de passe chiffré, avant de déplacer le fichier. En outre, stockez le fichier dans un emplacement sécurisé pour empêcher les utilisateurs malveillants d’y accéder.

> [!NOTE]
> Si la journalisation SQL Server est configurée sur la serveur NPS source, les paramètres de journalisation de SQL Server ne sont pas exportés dans le fichier XML. Après avoir importé le fichier sur un autre serveur NPS, vous devez configurer manuellement la journalisation SQL Server.

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>Exporter et importer la configuration NPS à l’aide de Windows PowerShell

Pour Windows Server 2012 et versions ultérieures de système d’exploitation, vous pouvez exporter la configuration NPS à l’aide de Windows PowerShell.

La syntaxe de commande pour exporter la configuration NPS est comme suit. 

    Export-NpsConfiguration -Path <filename>

Le tableau suivant répertorie les paramètres pour le **NpsConfiguration d’exportation** applet de commande dans Windows PowerShell. Paramètres en gras sont obligatoires.

|Paramètre|Description|
|---------|-----------|
|Path|Spécifie le nom et l’emplacement du fichier XML auquel vous souhaitez exporter la configuration du serveur NPS.|

**Informations d’identification administratives**

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

### <a name="export-example"></a>Exemple d’exportation 

Dans l’exemple suivant, la configuration NPS est exportée vers un fichier XML situé sur le lecteur local. Pour exécuter cette commande, exécutez Windows PowerShell en tant qu’administrateur sur la serveur NPS source, tapez la commande suivante et appuyez sur ENTRÉE.

`Export-NpsConfiguration –Path c:\config.xml` 

Pour plus d’informations, consultez [NpsConfiguration d’exportation](https://technet.microsoft.com/library/jj872749.aspx).

Une fois que vous avez exporté la configuration NPS, copiez le fichier XML pour le serveur de destination.

Voici la syntaxe de commande pour importer la configuration du serveur NPS sur le serveur de destination.

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>Exemple d’importation

La commande suivante importe les paramètres à partir du fichier nommé C:\Npsconfig.xml à NPS. Pour exécuter cette commande, exécutez Windows PowerShell en tant qu’administrateur sur le serveur NPS de destination, tapez la commande suivante et appuyez sur ENTRÉE.

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

Pour plus d’informations, consultez [Import-NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx).

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>Exporter et importer la configuration NPS à l’aide de Netsh

Vous pouvez utiliser Network Shell \(Netsh\) pour exporter la configuration NPS à l’aide de la **exportation netsh nps** commande.

Lorsque le **netsh nps importer** commande est exécutée, le serveur NPS est automatiquement actualisé avec les paramètres de configuration mis à jour. Vous n’avez pas besoin d’arrêter le serveur NPS sur l’ordinateur de destination pour exécuter le **netsh nps importer** cependant si la console NPS ou le composant logiciel enfichable MMC NPS est ouverte pendant l’importation de la configuration, modifications apportées à la configuration du serveur ne sont pas visibles jusqu'à ce que de la commande vous actualisez l’affichage. 

> [!NOTE]
> Lorsque vous utilisez le **exportation netsh nps** commande, vous devez fournir le paramètre de commande **exportPSK** avec la valeur **Oui**. Ce paramètre et la valeur de déclarer explicitement que vous comprenez que vous exportez la configuration NPS, et que le fichier XML exporté contienne non chiffrés secrets partagés pour les clients RADIUS et les membres de groupes de serveurs RADIUS distants.

**Informations d’identification administratives**

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

### <a name="to-copy-an-nps-configuration-to-another-nps-using-netsh-commands"></a>Pour copier une configuration de serveur NPS vers un autre serveur NPS à l’aide des commandes Netsh

1. Sur la serveur NPS source, ouvrez **invite de commandes**, type **netsh**, puis appuyez sur ENTRÉE.

2. À la **netsh** invite, tapez **nps**, puis appuyez sur ENTRÉE. 

3. À la **netsh nps** invite, tapez **exporter filename =** »*path\file.xml*» **exportPSK = YES**, où *chemin d’accès* est l’emplacement du dossier où vous souhaitez enregistrer le fichier de configuration de serveur NPS et *fichier* est le nom du fichier XML que vous souhaitez enregistrer. Appuyez sur Entrée. 

Il stocke les paramètres de configuration \(y compris les paramètres de Registre\) dans un fichier XML. Le chemin d’accès peut être relatif ou absolu, ou il peut être une Convention d’affectation de noms universelle \(UNC\) chemin d’accès. Une fois que vous appuyez sur entrée, un message apparaît et indique si l’exportation de fichier a réussi.

4. Copiez le fichier que vous avez créé pour la serveur NPS de destination.

5. À une invite de commandes sur le serveur NPS de destination, tapez **netsh nps importer filename =** »*path\file.xml*», puis appuyez sur ENTRÉE. Un message apparaît et indique si l’importation à partir du fichier XML a réussi.

Pour plus d’informations sur netsh, consultez [Network Shell (Netsh)](../netsh/netsh.md).

