---
title: Échec du protocole de transfert TCP bidirectionnel pendant la connexion SMB
description: Présente l’échec du protocole de transfert TCP bidirectionnel pendant la connexion SMB.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: cb88fa89344172cfc1ed036865a4496ed73e9a22
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815322"
---
# <a name="tcp-three-way-handshake-failure-during-smb-connection"></a>Échec du protocole de transfert TCP bidirectionnel pendant la connexion SMB

Lorsque vous analysez un suivi réseau, vous remarquez qu’il existe un échec de la négociation de protocole TCP (Transmission Control Protocol) qui provoque le problème SMB. Cet article explique comment résoudre ce problème.

## <a name="troubleshooting"></a>Résolution des problèmes

En règle générale, il s’agit d’un pare-feu local ou d’infrastructure qui bloque le trafic. Ce problème peut se produire dans l’un des scénarios suivants.

## <a name="scenario-1"></a>Scénario 1

Le paquet TCP SYN arrive sur le serveur SMB, mais le serveur SMB ne retourne pas de paquet TCP SYN-ACK.

Pour dépanner ce scénario, procédez comme suit.

### <a name="step-1"></a>Étape 1

Exécutez **netstat** ou **obtenir-NetTcpConnection** pour vous assurer qu’il existe un écouteur sur le port TCP 445 qui doit être détenu par le processus système.

```cmd
netstat -ano | findstr :445
```

```PowerShell
Get-NetTcpConnection -LocalPort 445
```

### <a name="step-2"></a>Étape 2

Assurez-vous que le service serveur est démarré et en cours d’exécution.

### <a name="step-3"></a>Étape 3

Effectuez une capture de la plateforme de filtrage Windows (WFP) pour déterminer la règle ou le programme qui supprime le trafic. Pour ce faire, exécutez la commande suivante dans une fenêtre d’invite de commandes :

```cmd
netsh wfp capture start
```

Reproduisez le problème, puis exécutez la commande suivante :

```cmd
netsh wfp capture stop
```

Exécutez une trace de scénario et recherchez les chutes de WFP dans le trafic SMB (sur le port TCP 445).

Si vous le souhaitez, vous pouvez supprimer les programmes antivirus, car ils ne sont pas toujours basés sur la plateforme WFP.

### <a name="step-4"></a>Étape 4

Si le pare-feu Windows est activé, activez la journalisation du pare-feu pour déterminer s’il enregistre une chute du trafic.

Assurez-vous que les règles de partage de fichiers et d’imprimantes (SMB-in) appropriées sont activées dans le **pare-feu Windows avec fonctions avancées de sécurité \> les** **règles de trafic entrant**.

> [!NOTE]
> Selon la configuration de votre ordinateur, le « pare-feu Windows » peut être appelé « pare-feu Windows Defender ».

## <a name="scenario-2"></a>Scénario 2

Le paquet TCP SYN n’arrive jamais sur le serveur SMB.

Dans ce scénario, vous devez examiner les appareils sur le chemin d’accès réseau. Vous pouvez analyser les suivis réseau capturés sur chaque appareil pour déterminer quel périphérique bloque le trafic.
