---
title: Résolution avancée des problèmes liés au protocole SMB (Server Message Block)
description: Présente les méthodes de résolution des problèmes du protocole SMB (Server Message Block).
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 433221f9846e9e071557b5537974b5739131742b
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949693"
---
# <a name="advanced-troubleshooting-server-message-block-smb"></a>Résolution avancée des problèmes liés au protocole SMB (Server Message Block)

Le protocole SMB (Server Message Block) est un protocole de transport réseau pour les opérations sur les systèmes de fichiers permettant à un client d’accéder aux ressources d’un serveur. L’objectif principal du protocole SMB est d’activer l’accès à distance du système de fichiers entre deux systèmes via TCP/IP.

La résolution des problèmes SMB peut être extrêmement complexe. Cet article n’est pas un guide de dépannage exhaustif. il s’agit d’une brève introduction pour comprendre les principes de base de la résolution des problèmes de SMB.

## <a name="tools-and-data-collection"></a>Outils et collecte de données

Un aspect clé de la résolution des problèmes de qualité SMB est la communication de la terminologie correcte. Par conséquent, cet article présente la terminologie de base SMB pour garantir la précision de la collecte et de l’analyse des données.

> [!Note]
> Le *serveur SMB (SRV)* fait référence au système qui héberge le système de fichiers, également appelé serveur de fichiers. *Le client SMB (CLI)* fait référence au système qui tente d’accéder au système de fichiers, quelle que soit la version ou l’édition du système d’exploitation.

Par exemple, si vous utilisez Windows Server 2016 pour atteindre un partage SMB hébergé sur Windows 10, Windows Server 2016 est le client SMB et Windows 10 le serveur SMB.

### <a name="collect-data"></a>Collecte des données

Avant de résoudre les problèmes liés à SMB, nous vous recommandons de commencer par collecter un suivi réseau sur les côtés client et serveur. Les consignes suivantes s'appliquent :

- Sur les systèmes Windows, vous pouvez utiliser NetShell (Netsh), Moniteur réseau, l’analyseur de messages ou Wireshark pour collecter un suivi réseau.

- Les appareils tiers disposent généralement d’un outil de capture de paquets intégré, tel que tcpdump (Linux/FreeBSD/UNIX) ou pktt (NetApp). Par exemple, si le client SMB ou le serveur SMB est un hôte UNIX, vous pouvez collecter des données en exécutant la commande suivante :
  
  ```cmd
  # tcpdump -s0 -n -i any -w /tmp/$(hostname)-smbtrace.pcap
  ```
  
  Arrêtez la collecte de données à l’aide de **Ctrl + C** à partir du clavier.

Pour découvrir la source du problème, vous pouvez vérifier les traces à deux côtés : CLI, SRV ou entre les deux.

#### <a name="using-netshell-to-collect-data"></a>Utilisation de NetShell pour collecter des données

Cette section décrit les étapes d’utilisation de NetShell pour collecter la trace réseau.

> [!NOTE]  
> Une trace netsh crée un fichier ETL. Les fichiers ETL peuvent être ouverts uniquement dans message Analyzer (MA) et Moniteur réseau 3,4 (définissez l’analyseur sur Moniteur réseau parseurs \> Windows).

1. Sur le serveur SMB et le client SMB, créez un dossier **temporaire** sur le lecteur **C**. Exécutez ensuite la commande suivante :

   ```cmd
   netsh trace start capture=yes report=yes scenario=NetConnection level=5 maxsize=1024 tracefile=c:\\Temp\\%computername%\_nettrace.etl**
   ```
   
   Si vous utilisez PowerShell, exécutez les applets de commande suivantes :
   
   ```PowerShell
   New-NetEventSession -Name trace -LocalFilePath "C:\Temp\$env:computername`_netCap.etl" -MaxFileSize 1024

   Add-NetEventPacketCaptureProvider -SessionName trace -TruncationLength 1500

   Start-NetEventSession trace
   ```
   
2. Reproduire le problème.

3. Arrêtez la trace en exécutant la commande suivante :

   ```cmd
   netsh trace stop
   ```
   
   Si vous utilisez PowerShell, exécutez les applets de commande suivantes :

   ```PowerShell
   Stop-NetEventSession trace  
   Remove-NetEventSession trace
   ```

> [!NOTE] 
> Vous ne devez tracer qu’une quantité minimale de données transférées. Pour les problèmes de performances, prenez toujours une trace correcte et incorrecte, si la situation le permet.

### <a name="analyze-the-traffic"></a>Analyser le trafic

SMB est un protocole au niveau de l’application qui utilise TCP/IP comme protocole de transport réseau. Par conséquent, un problème SMB peut également être provoqué par des problèmes TCP/IP.

Vérifiez si TCP/IP rencontre l’un de ces problèmes :

1. Le protocole de transfert TCP bidirectionnel ne se termine pas. Cela indique généralement qu’il existe un bloc de pare-feu ou que le service serveur n’est pas en cours d’exécution.

2. Des retransmissions sont en cours. Celles-ci peuvent entraîner des transferts de fichiers lents en raison de la limitation de congestion TCP composée.

3. Cinq retransmissions suivies d’une réinitialisation TCP peuvent signifier que la connexion entre les systèmes a été perdue ou que l’un des services SMB est bloqué ou a cessé de répondre.

4. La fenêtre de réception TCP diminue. Cela peut être dû à un ralentissement du stockage ou à un autre problème qui empêche la récupération des données à partir de la mémoire tampon Winsock du pilote de fonction auxiliaire (AFD).

S’il n’existe aucun problème de TCP/IP notable, recherchez les erreurs SMB. Pour cela, procédez comme suit :

1. Vérifiez toujours les erreurs SMB par rapport à la spécification du protocole MS-SMB2. De nombreuses erreurs SMB sont bénignes (non dangereuses). Reportez-vous aux informations suivantes pour déterminer la raison pour laquelle SMB a retourné l’erreur avant de conclure que l’erreur est liée à l’un des problèmes suivants :

   - La rubrique de [syntaxe de message MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/6eaf6e75-9c23-4eda-be99-c9223c60b181) détaille chaque commande SMB et ses options.
    
   - La rubrique [traitant du traitement du client MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/df0625a5-6516-4fbe-bf97-01bef451cab2) explique en détail comment le client SMB crée des demandes et répond aux messages du serveur.

   - La rubrique sur le [traitement du serveur MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/e1d08834-42e0-41ca-a833-fc26f5132a6f) explique en détail comment le serveur SMB crée des demandes et répond aux demandes des clients.

2. Vérifiez si une commande de réinitialisation TCP est envoyée immédiatement après un FSCTL\_valider\_commande NEGOTIATE\_INFO (Validate Negotiate). Si c’est le cas, reportez-vous aux informations suivantes :

   - La session SMB doit être arrêtée (réinitialisation TCP) lorsque le processus de validation de négociation échoue sur le client ou sur le serveur.

   - Ce processus peut échouer car un optimiseur de réseau étendu modifie le paquet de négociation SMB.

   - Si la connexion s’est terminée prématurément, identifiez la dernière communication Exchange entre le client et le serveur.

#### <a name="analyze-the-protocol"></a>Analyser le protocole

Examinez les détails du protocole SMB réel dans la trace réseau pour comprendre les commandes et les options qui sont utilisées.

> [!NOTE]
> Seul message Analyzer peut analyser les commandes SMBv3 et versions ultérieures.

- N’oubliez pas que SMB ne fait que ce qu’il est dit de faire.

- Vous pouvez apprendre beaucoup sur ce que l’application tente de faire en examinant les commandes SMB.

Comparez les commandes et les opérations à la spécification du protocole pour vous assurer que tout fonctionne correctement. Si ce n’est pas le cas, collectez des données qui sont plus proches ou à un niveau inférieur pour rechercher plus d’informations sur la cause racine. Pour cela, procédez comme suit :

1. Collectez une capture de paquets standard.

2. Exécutez la commande **netsh** pour suivre et rassembler des détails sur la présence de problèmes dans la pile réseau ou dans des applications de plateforme de filtrage Windows (WFP), telles que le pare-feu ou le programme antivirus.

3. Si toutes les autres options échouent, collectez t. cmd si vous soupçonnez que le problème se produit dans SMB lui-même, ou si aucune autre donnée n’est suffisante pour identifier une cause racine.

Par exemple :

- Vous constatez un ralentissement des transferts de fichiers vers un serveur de fichiers unique.

- Les traces à deux côtés montrent que le SRV répond lentement à une demande de lecture.

- La suppression d’un programme antivirus résout les transferts de fichiers lents.

- Vous contactez le programme antivirus Manufactory pour résoudre le problème.

> [!NOTE]
> Si vous le souhaitez, vous pouvez **également** désinstaller temporairement le programme antivirus pendant la résolution des problèmes.

#### <a name="event-logs"></a>Journaux des événements

Le client SMB et le serveur SMB présentent une structure détaillée des journaux des événements, comme illustré dans la capture d’écran suivante. Collectez les journaux des événements pour vous aider à trouver la cause racine du problème.

![Journaux des événements](media/troubleshooting-smb-1.png)

## <a name="smb-related-system-files"></a>Fichiers système liés à SMB

Cette section répertorie les fichiers système liés à SMB. Pour mettre à jour les fichiers système, assurez-vous que le [correctif cumulatif](https://support.microsoft.com/help/4498140/windows-10-update-history) le plus récent est installé.

Fichiers binaires du client SMB répertoriés sous **% windir%\\system32\\pilotes**:

- RDBSS. sys

- MRXSMB. sys

- MRXSMB10. sys

- MRXSMB20. sys

- MUP. sys

- SMBdirect. sys

Fichiers binaires du serveur SMB répertoriés sous **% windir%\\system32\\pilotes**:

- SRVNET. sys

- SRV. sys

- SRV2. sys

- SMBdirect. sys

- Sous **% windir%\\system32**

- srvsvc. dll

![Composants SMB](media/troubleshooting-smb-2.png)

### <a name="update-suggestions"></a>Mettre à jour les suggestions

Nous vous recommandons de mettre à jour les composants suivants avant de résoudre les problèmes liés à SMB :

- Un serveur de fichiers nécessite un stockage de fichiers. Si votre stockage dispose d’un composant iSCSI, mettez-les à jour.

- Mettez à jour les composants réseau.

- Pour améliorer les performances et la stabilité, mettez à jour Windows Core.

## <a name="reference"></a>Référence

[Scénario d’échange de paquets de protocole SMB Microsoft](https://docs.microsoft.com/windows/win32/fileio/microsoft-smb-protocol-packet-exchange-scenario)