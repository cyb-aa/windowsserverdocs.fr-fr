---
title: Résolution des problèmes de SMB Multichannel
description: Présente les méthodes de résolution des problèmes SMB Multichannel.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 91f034a0062f509b1185f04554af4383022a68e1
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654540"
---
# <a name="smb-multichannel-troubleshooting"></a>Résolution des problèmes de SMB Multichannel

Cet article explique comment résoudre les problèmes liés à SMB Multichannel.

## <a name="check-the-network-interface-status"></a>Vérifier l’état de l’interface réseau

Assurez-vous que la liaison de l’interface réseau est définie sur **true** sur le client SMB (MS\_client) et sur le serveur SMB (MS\_Server). Lorsque vous exécutez la commande suivante, la sortie doit afficher **true** sous **activé** pour les deux interfaces réseau :

```PowerShell
Get-NetAdapterBinding -ComponentID ms_server,ms_msclient
```

Après cela, assurez-vous que l’interface réseau est indiquée dans la sortie des commandes suivantes :

```PowerShell
Get-SmbServerNetworkInterface
```

```PowerShell
Get-SmbClientNetworkInterface
```

Vous pouvez également exécuter la commande **obtenir-NetAdapter** pour afficher l’index d’interface afin de vérifier le résultat. L’index d’interface affiche tous les adaptateurs SMB actifs qui sont liés activement à l’interface appropriée.

## <a name="check-the-firewall"></a>Vérifier le pare-feu

S’il n’existe qu’une adresse IP de lien local et qu’il n’y a pas d’adresse routable publiquement, le profil réseau est probablement défini sur **public**. Cela signifie que SMB est bloqué par défaut au niveau du pare-feu.

La commande suivante révèle le profil de connexion en cours d’utilisation. Vous pouvez également utiliser le centre réseau et partage pour récupérer ces informations.

**NetConnectionProfile**

Dans le groupe **partage de fichiers et d’imprimantes** , vérifiez les règles de trafic entrant du pare-feu pour vous assurer que la valeur « SMB-in » est activée pour le profil approprié.

![Règles SMB](media/smb-multichannel-troubleshooting-1.png)

Vous pouvez également activer le **partage de fichiers et d’imprimantes** dans la fenêtre **Centre réseau et partage** . Pour ce faire, sélectionnez **modifier les paramètres de partage avancés** dans le menu de gauche, puis sélectionnez **activer le partage de fichiers et d’imprimantes** pour le profil. Cette option active les règles de pare-feu de partage de fichiers et d’imprimantes.

![Modifier les paramètres de partage avancés](media/smb-multichannel-troubleshooting-2.png)

## <a name="capture-client-and-server-sided-traffic-for-troubleshooting"></a>Capturer le trafic côté client et côté serveur pour la résolution des problèmes

Vous avez besoin des informations de suivi de la connexion SMB qui démarrent à partir de la négociation TCP triple. Nous vous recommandons de fermer toutes les applications (en particulier l’Explorateur Windows) avant de commencer la capture. Redémarrez le service **station de travail** sur le client SMB, démarrez la capture de paquets, puis reproduisez le problème.

Assurez-vous que la connexion SMBv3.*x* est en cours de négociation et que rien dans entre le serveur et le client n’affecte la négociation de dialecte. SMBv2 et les versions antérieures ne prennent pas en charge Multichannel.

Recherchez l’INTERFACE\_réseau\_des paquets d’informations. C’est là que le client SMB demande une liste de cartes à partir du serveur SMB. Si ces paquets ne sont pas échangés, la multichaîne ne fonctionne pas.

Le serveur répond en renvoyant une liste d’interfaces réseau valides. Ensuite, le client SMB les ajoute à la liste des adaptateurs disponibles pour Multichannel. À ce stade, Multichannel doit démarrer et, au moins, essayer de démarrer la connexion.

Pour plus d’informations, consultez les articles suivants :

- [l’application 3.2.4.20.10 demande l’interrogation des interfaces réseau du serveur](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/147adde4-d936-4597-924a-8caa3429c6b0)

- [2.2.32.5 NETWORK\_INTERFACE\_réponse INFO](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/fcd862d1-1b85-42df-92b1-e103199f531f)

- [3.2.5.14.11 gestion d’une réponse d’interfaces réseau](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/5459722b-1eaa-4ead-b465-284363264cad)

Dans les scénarios suivants, un adaptateur ne peut pas être utilisé :

- Il y a un problème de routage sur le client. Cela est généralement dû à une table de routage incorrecte qui force le trafic sur une interface incorrecte.

- Des contraintes multicanaux ont été définies. Pour plus d’informations, consultez [New-SmbMultichannelConstraint](https://docs.microsoft.com/powershell/module/smbshare/new-smbmultichannelconstraint).

- Un événement a bloqué les paquets de demande et de réponse de l’interface réseau.

- Le client et le serveur ne peuvent pas communiquer via l’interface réseau supplémentaire. Par exemple, le protocole de transfert TCP bidirectionnel a échoué, la connexion est bloquée par un pare-feu, la configuration de session a échoué, et ainsi de suite.

Si l’adaptateur et son adresse IPv6 figurent dans la liste envoyée par le serveur, l’étape suivante consiste à déterminer si les communications sont tentées sur cette interface. Filtrez la trace à l’aide de l’adresse lien-local et du trafic SMB, et recherchez une tentative de connexion. S’il s’agit d’une trace NetConnection, vous pouvez également examiner les événements de la plateforme de filtrage Windows (WFP) pour voir si la connexion est bloquée.
