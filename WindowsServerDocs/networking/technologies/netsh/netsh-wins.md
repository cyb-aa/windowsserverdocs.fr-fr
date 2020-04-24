---
title: Exemple de fichier de commandes Netsh (Network Shell)
description: Vous pouvez utiliser cette rubrique pour apprendre à créer un fichier de commandes qui effectue plusieurs tâches à l’aide de Netsh dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 73798ff4c8af11cc5cfb6461245ba7873f5d6f36
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80316727"
---
# <a name="network-shell-netsh-example-batch-file"></a>Exemple de fichier de commandes Netsh \(Network Shell\)

S'applique à : Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à créer un fichier de commandes qui effectue plusieurs tâches à l’aide de Netsh dans Windows Server 2016. Dans cet exemple de fichier de commandes, le contexte **netsh wins** est utilisé.

## <a name="example-batch-file-overview"></a>Vue d’ensemble de l’exemple de fichier de commandes

Vous pouvez utiliser des commandes Netsh pour WINS \(Windows Internet Name Service\) dans des fichiers de commandes et d’autres scripts afin d’automatiser des tâches. L’exemple de fichier de commandes suivant montre comment utiliser des commandes Netsh pour WINS afin d’effectuer diverses tâches connexes.

Dans cet exemple de fichier de commandes, WINS\-A est un serveur WINS avec l’adresse IP 192.168.125.30, tandis que WINS\-B est un serveur WINS avec l’adresse IP 192.168.0.189.

L’exemple de fichier de commandes effectue les tâches suivantes.

- Ajoute un enregistrement de nom dynamique avec l’adresse IP 192.168.0.205, MY\_RECORD \[04h\], à WINS\-A
- Définit WINS\-B comme partenaire de réplication par émission/réception de WINS\-A
- Se connecte à WINS\-B, puis définit WINS\-A comme partenaire de réplication par émission/réception de WINS\-B
- Lance une réplication par émission depuis WINS\-A vers WINS\-B
- Se connecte à WINS\-B pour vérifier que le nouvel enregistrement, MY\_RECORD, a été correctement répliqué

## <a name="netsh-example-batch-file"></a>Exemple de fichier de commandes netsh

Dans l’exemple de fichier de commandes suivant, les lignes qui contiennent des commentaires sont précédées de la mention « rem » (pour « remarque »). Netsh ignore les commentaires.

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

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>Commandes netsh WINS utilisées dans l’exemple de fichier de commandes

La section suivante répertorie les commandes **netsh wins** utilisées dans cet exemple de procédure.

- **server**. Déplace le contexte de ligne de commande WINS actuel vers le serveur spécifié par son nom ou son adresse IP.
- **add name**. Inscrit un nom sur le serveur WINS.
- **add partner**. Ajoute un partenaire de réplication sur le serveur WINS.
- **init push**. Lance un déclencheur d’émission et l’envoie à un serveur WINS.
- **show name**. Affiche des informations détaillées sur un enregistrement particulier de la base de données du serveur WINS.  

Pour plus d’informations, consultez [Netsh (Network Shell)](netsh.md).
