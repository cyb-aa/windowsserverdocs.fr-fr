---
title: Exemple de fichier de commandes d'environnement réseau (Netsh)
description: Vous pouvez utiliser cette rubrique pour apprendre à créer un fichier de commandes qui effectue plusieurs tâches à l’aide de Netsh dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0528cfaef201ba30e00e30f56a763be39a6b828
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880170"
---
# <a name="network-shell-netsh-example-batch-file"></a>Réseau d’interpréteur de commandes \(Netsh\) fichier Batch

S'applique à : Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à créer un fichier de commandes qui effectue plusieurs tâches à l’aide de Netsh dans Windows Server 2016. Dans cet exemple de fichier batch, le **netsh wins** contexte est utilisé.

## <a name="example-batch-file-overview"></a>Vue d’ensemble du fichier exemple Batch

Vous pouvez utiliser les commandes Netsh pour le service Windows Internet Name Service \(WINS\) dans les fichiers de commandes et d’autres scripts pour automatiser des tâches. L’exemple de fichier batch suivant montre comment utiliser les commandes Netsh pour WINS pour effectuer diverses tâches associées.

Dans cet exemple de fichier batch, WINS\-A est un serveur WINS avec l’adresse IP 192.168.125.30 et WINS\-B est un serveur WINS avec l’adresse IP est192.168.0.189.

L’exemple de fichier batch effectue les tâches suivantes.

- Ajoute un enregistrement de noms dynamique avec l’adresse IP 192.168.0.205, MY\_enregistrement \[04h\], sur le service WINS\-A
- Définit WINS\-B comme un partenaire de réplication par émission/réception de WINS\-A
- Se connecte à WINS\-B, puis définit WINS\-A comme un partenaire de réplication par émission/réception de WINS\-B
- Lance une réplication par émission de WINS\-A à WINS\-B
- Se connecte à WINS\-B pour vérifier que le nouvel enregistrement, MY\_enregistrement, a été correctement répliqué

## <a name="netsh-example-batch-file"></a>Fichier de commandes Netsh exemple

Dans le fichier de commandes exemple suivant, les lignes qui contiennent des commentaires sont précédées par « rem », pour Remarque. Netsh ignore les commentaires.

    rem: Begin example batch file.
    
    rem two WINS servers:
    
    rem (WINS-A) 192.168.125.30
    
    rem (WINS-B) 192.168.0.189
    
    rem 1. Connect to (WINS-A), and add the dynamic name MY\_RECORD \[04h\] to the (WINS-A) database.
    
    netsh wins server 192.168.125.30 add name Name=MY\_RECORD EndChar=04 IP={192.168.0.205}
    
    rem 2. Connect to (WINS-A), and set (WINS-B) as a push/pull replication partner of (WINS-A).
    
    netsh wins server 192.168.125.30 add partner Server=192.168.0.189 Type=2
    
    rem 3. Connect to (WINS-B), and set (WINS-A) as a push/pull replication partner of (WINS-B).
    
    netsh wins server 192.168.0.189 add partner Server=192.168.125.30 Type=2
    
    rem 4. Connect back to (WINS-A), and initiate a push replication to (WINS-B).
    
    netsh wins server 192.168.125.30 init push Server=192.168.0.189 PropReq=0
    
    rem 5. Connect to (WINS-B), and check that the record MY\_RECORD \[04h\] was replicated successfully.
    
    netsh wins server 192.168.0.189 show name Name=MY\_RECORD EndChar=04
    
    rem 6. End example batch file.

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>Commandes Netsh pour WINS est utilisés dans l’exemple de fichier batch

La section suivante répertorie les **netsh wins** commandes qui sont utilisées dans cet exemple de procédure.

- **serveur**. Déplace la commande WINS actuelle\-contexte de ligne pour le serveur spécifié par son nom ou son adresse IP.
- **ajouter le nom**. Inscrit un nom sur le serveur WINS.
- **Ajouter un partenaire**. Ajoute un partenaire de réplication sur le serveur WINS.
- **init push**. Active et envoie un déclencheur d’émission à un serveur WINS.
- **afficher le nom**. Affiche des informations détaillées sur un enregistrement particulier dans la base de données du serveur WINS.  

Pour plus d’informations, consultez [Network Shell (Netsh)](netsh.md).
