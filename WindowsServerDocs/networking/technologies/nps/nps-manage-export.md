---
title: Exporter une configuration NPS pour l’importer sur un autre serveur
description: Vous pouvez utiliser cette rubrique pour apprendre à exporter une configuration de serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cbebd0388ccd5dd2540a20f5d325d7f97c7e2bb3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405432"
---
# <a name="export-an-nps-configuration-for-import-on-another-server"></a>Exporter une configuration NPS pour l’importer sur un autre serveur

S'applique à : Windows Server 2016

Vous pouvez exporter l’intégralité de la configuration du serveur NPS, y compris les clients et serveurs RADIUS, la stratégie réseau, la stratégie de demande de connexion, le registre et la configuration de journalisation, d’un serveur NPS à importer sur un autre serveur NPS. 

Utilisez l’un des outils suivants pour exporter la configuration du serveur NPS :

- Dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012, vous pouvez utiliser la commande netsh ou vous pouvez utiliser Windows PowerShell.
- Dans Windows Server 2008 R2 et Windows Server 2008, utilisez netsh.

> [!IMPORTANT]
> N’utilisez pas cette procédure si la base de données NPS source a un numéro de version supérieur au numéro de version de la base de données NPS de destination. Vous pouvez afficher le numéro de version de la base de données NPS à partir de l’affichage de la commande **netsh nps Show config** .

Étant donné que les configurations NPS ne sont pas chiffrées dans le fichier XML exporté, leur envoi sur un réseau peut poser un risque de sécurité. par conséquent, prenez des précautions lors du déplacement du fichier XML du serveur source vers les serveurs de destination. Par exemple, ajoutez le fichier à un fichier d’archive chiffré protégé par mot de passe avant de déplacer le fichier. De plus, stockez le fichier dans un emplacement sécurisé pour empêcher les utilisateurs malveillants d’y accéder.

> [!NOTE]
> Si SQL Server la journalisation est configurée sur le serveur NPS source, les paramètres de journalisation SQL Server ne sont pas exportés vers le fichier XML. Après avoir importé le fichier sur un autre serveur NPS, vous devez configurer manuellement SQL Server la journalisation.

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>Exporter et importer la configuration du serveur NPS à l’aide de Windows PowerShell

Pour les versions de système d’exploitation Windows Server 2012 et versions ultérieures, vous pouvez exporter la configuration du serveur NPS à l’aide de Windows PowerShell.

La syntaxe de commande pour l’exportation de la configuration du serveur NPS est la suivante. 

    Export-NpsConfiguration -Path <filename>

Le tableau suivant répertorie les paramètres de l’applet de commande **Export-NpsConfiguration** dans Windows PowerShell. Les paramètres en gras sont obligatoires.

|Paramètre|Description|
|---------|-----------|
|Path|Spécifie le nom et l’emplacement du fichier XML dans lequel vous souhaitez exporter la configuration du serveur NPS.|

**Informations d’identification d’administration**

Pour effectuer cette procédure, vous devez être membre du groupe administrateurs.

### <a name="export-example"></a>Exemple d’exportation 

Dans l’exemple suivant, la configuration du serveur NPS est exportée vers un fichier XML situé sur le lecteur local. Pour exécuter cette commande, exécutez Windows PowerShell en tant qu’administrateur sur le serveur NPS source, tapez la commande suivante et appuyez sur entrée.

`Export-NpsConfiguration –Path c:\config.xml` 

Pour plus d’informations, consultez [Export-NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx).

Une fois que vous avez exporté la configuration du serveur NPS, copiez le fichier XML sur le serveur de destination.

La syntaxe de commande pour l’importation de la configuration du serveur NPS sur le serveur de destination est la suivante.

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>Exemple d’importation

La commande suivante importe les paramètres du fichier nommé C:\Npsconfig.xml vers NPS. Pour exécuter cette commande, exécutez Windows PowerShell en tant qu’administrateur sur le serveur NPS de destination, tapez la commande suivante, puis appuyez sur entrée.

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

Pour plus d’informations, consultez [Import-NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx).

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>Exporter et importer la configuration du serveur NPS à l’aide de netsh

Vous pouvez utiliser Network Shell \(Netsh @ no__t-1 pour exporter la configuration du serveur NPS à l’aide de la commande **netsh nps Export** .

Lorsque la commande **netsh nps Import** est exécutée, le serveur NPS est automatiquement actualisé avec les paramètres de configuration mis à jour. Vous n’avez pas besoin d’arrêter NPS sur l’ordinateur de destination pour exécuter la commande **netsh nps Import** . Toutefois, si la console NPS ou le composant logiciel enfichable MMC NPS est ouvert pendant l’importation de la configuration, les modifications apportées à la configuration du serveur ne sont pas visibles tant que vous n’actualisez pas la vue. 

> [!NOTE]
> Lorsque vous utilisez la commande **netsh nps Export** , vous devez fournir le paramètre de commande **exportPSK** avec la valeur **Yes**. Ce paramètre et cette valeur indiquent explicitement que vous exportez la configuration du serveur NPS et que le fichier XML exporté contient des secrets partagés non chiffrés pour les clients RADIUS et les membres des groupes de serveurs RADIUS distants.

**Informations d’identification d’administration**

Pour effectuer cette procédure, vous devez être membre du groupe administrateurs.

### <a name="to-copy-an-nps-configuration-to-another-nps-using-netsh-commands"></a>Pour copier une configuration NPS sur un autre serveur NPS à l’aide des commandes netsh

1. Sur le serveur NPS source, ouvrez l' **invite de commandes**, tapez **netsh**, puis appuyez sur entrée.

2. À l’invite **netsh** , tapez **NPS**, puis appuyez sur entrée. 

3. À l’invite **netsh nps** , tapez **Export filename =** "*path\file.xml*" **exportPSK = Yes**, où *chemin* est l’emplacement du dossier dans lequel vous souhaitez enregistrer le fichier de configuration NPS, et *fichier* est le nom du fichier XML qui vous souhaitez enregistrer. Appuyez sur Entrée. 

Cela stocke les paramètres de configuration \(including-no__t-1 dans un fichier XML. Le chemin d’accès peut être relatif ou absolu, ou il peut s’agir d’une convention d’affectation de noms universelle \(UNC @ no__t-1. Une fois que vous avez appuyé sur entrée, un message s’affiche pour indiquer si l’exportation vers le fichier a réussi.

4. Copiez le fichier que vous avez créé sur le serveur NPS de destination.

5. À l’invite de commandes, sur le serveur NPS de destination, tapez **netsh nps Import filename =** "*path\file.xml*", puis appuyez sur entrée. Un message s’affiche pour indiquer si l’importation à partir du fichier XML a réussi.

Pour plus d’informations sur netsh, consultez [Network Shell (Netsh)](../netsh/netsh.md).

