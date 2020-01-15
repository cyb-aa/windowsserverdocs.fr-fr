---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Heure précise pour Windows Server 2016
description: La précision de la synchronisation de l’heure dans Windows Server 2016 a été considérablement améliorée, tout en conservant une compatibilité NTP complète avec les anciennes versions de Windows.
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 2e8e9e86f81596c85219c37c07d8fd2e95cc3a49
ms.sourcegitcommit: 10331ff4f74bac50e208ba8ec8a63d10cfa768cc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75953078"
---
# <a name="accurate-time-for-windows-server-2016"></a>Heure précise pour Windows Server 2016

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou version ultérieure

Le service de temps Windows est un composant qui utilise un modèle de plug-in pour les fournisseurs de synchronisation de l’heure du client et du serveur.  Il existe deux fournisseurs clients intégrés sur Windows et des plug-ins tiers sont disponibles. Un fournisseur utilise le [protocole NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) ou [MS-NTP](https://msdn.microsoft.com/library/cc246877.aspx) pour synchroniser l’heure système locale sur un serveur de référence NTP et/ou compatible MS-NTP. L’autre fournisseur est pour Hyper-V et synchronise les machines virtuelles (VM) avec l’hôte Hyper-V.  Lorsque plusieurs fournisseurs existent, Windows choisit d’abord le meilleur fournisseur à l’aide du niveau de couche, suivi du délai racine, de la dispersion racine et du décalage de temps final.

> [!NOTE]
> Pour une présentation rapide du service d’heure de Windows, regardez cette [vidéo de présentation globale](https://aka.ms/WS2016TimeVideo).

Dans cette rubrique, nous aborderons... ces rubriques sont liées à l’activation de l’heure précise : 

- Améliorations
- Mesures
- Meilleures pratiques

> [!IMPORTANT]
> Un addendum référencé par l’article précis sur l’heure de Windows 2016 peut être téléchargé [ici](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).  Ce document fournit plus de détails sur nos méthodologies de test et de mesure.

> [!NOTE] 
> Le modèle de plug-in du fournisseur de temps Windows est [documenté sur TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

## <a name="domain-hierarchy"></a>Hiérarchie de domaine
Les configurations de domaine et autonome fonctionnent différemment.

- Les membres du domaine utilisent un protocole NTP sécurisé, qui utilise l’authentification pour garantir la sécurité et l’authenticité de la référence temporelle.  Les membres de domaine se synchronisent avec une horloge maître déterminée par la hiérarchie de domaine et un système de notation.  Dans un domaine, il existe une couche hiérarchique de couches de temps, selon laquelle chaque contrôleur de domaine pointe vers un contrôleur de domaine parent avec une strate temporelle plus précise.  La hiérarchie correspond au PDC ou à un contrôleur de domaine dans la forêt racine, ou à un contrôleur de domaine avec l’indicateur de domaine GTIMESERV, qui désigne un serveur de temps approprié pour le domaine.  Consultez la section [spécifier un service de temps fiable local à l’aide de GTIMESERV ci-dessous.

- Les ordinateurs autonomes sont configurés pour utiliser time.windows.com par défaut.  Ce nom est résolu par votre serveur DNS, qui doit pointer vers une ressource appartenant à Microsoft.  Comme toutes les références horaires à distance, les pannes de réseau, peuvent empêcher la synchronisation.  Les chargements de trafic réseau et les chemins réseau asymétriques peuvent réduire la précision de la synchronisation de l’heure.  Pour une précision de 1 ms, vous ne pouvez pas dépendre d’une source de temps distant.

Étant donné que les invités Hyper-V auront un choix entre au moins deux fournisseurs de temps Windows, l’heure de l’hôte et le NTP, vous risquez de voir des comportements différents avec domaine ou autonome lors de l’exécution en tant qu’invité.

> [!NOTE] 
> Pour plus d’informations sur la hiérarchie de domaine et le système de calcul de score, consultez [« qu’est-ce que le service de temps Windows ? »](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) .

> [!NOTE]
> La couche est un concept utilisé à la fois dans les fournisseurs NTP et Hyper-V, et sa valeur indique l’emplacement des horloges dans la hiérarchie.  La couche 1 est réservée à l’horloge de niveau supérieur, et la couche 0 est réservée pour que le matériel soit considéré comme exact et n’a que peu ou pas de délai associé.  La couche 2 communique avec les serveurs de couche 1, la couche 3 à la strate 2, et ainsi de suite.  Bien qu’une couche inférieure indique souvent une horloge plus précise, il est possible de trouver des différences.  En outre, w32time accepte uniquement l’heure de la couche 15 ou inférieure.  Pour afficher la couche d’un client, utilisez *w32tm/Query/Status*.

## <a name="critical-factors-for-accurate-time"></a>Facteurs critiques pour un temps précis
Dans tous les cas pour une durée précise, il existe trois facteurs critiques :

1. **Horloge source pleine** : l’horloge source dans votre domaine doit être stable et précise. Cela signifie généralement que vous installez un appareil GPS ou que vous pointez vers une source de couche 1, en tenant #3 compte. L’analogie est, si vous avez deux bateaux sur l’eau et que vous essayez de mesurer l’altitude d’un autre par rapport à l’autre, votre précision est meilleure si le bateau source est très stable et ne bouge pas. Il en va de même pour l’heure et, si votre horloge source n’est pas stable, la chaîne entière des horloges synchronisées est affectée et agrandie à chaque étape. Il doit également être accessible, car les interruptions de la connexion interfèrent avec la synchronisation de l’heure. Enfin, il doit être sécurisé. Si la référence de temps n’est pas correctement gérée ou gérée par un tiers potentiellement malveillant, vous pouvez exposer votre domaine à des attaques basées sur le temps.
2. **Horloge client stable** : les horloges des clients stables garantissent que la dérive naturelle de l’oscillateur est contenance.  NTP utilise plusieurs échantillons de serveurs NTP potentiellement multiples pour conditionner et discipliner l’horloge de vos ordinateurs locaux.  Elle n’effectue pas de pas à pas les changements d’heure, mais elle ralentit ou accélère l’horloge locale, ce qui vous permet d’aborder rapidement le temps précis et de rester précis entre les demandes NTP.  Toutefois, si l’oscillateur de l’horloge de l’ordinateur client n’est pas stable, des fluctuations entre les ajustements peuvent se produire et les algorithmes que Windows utilise pour signaler que l’horloge ne fonctionne pas correctement.  Dans certains cas, les mises à jour du microprogramme peuvent être nécessaires pour des délais précis.
3. **Communication NTP symétrique** : il est essentiel que la connexion pour la communication NTP soit symétrique.  Le protocole NTP utilise des calculs pour ajuster le temps que le correctif réseau est symétrique.  Si le chemin d’accès du paquet NTP au serveur prend un temps de retour différent, la précision est affectée.  Par exemple, le chemin d’accès peut changer en raison de modifications apportées à la topologie du réseau ou lors de l’acheminement des paquets via des appareils qui ont des vitesses d’interface différentes.

Pour les appareils fonctionnant sur batterie, à la fois mobiles et portables, vous devez prendre en compte différentes stratégies.  Conformément à notre recommandation, le fait de conserver un temps précis exige que l’horloge soit organisée une fois par seconde, en corrélation avec la fréquence de mise à jour de l’horloge. Ces paramètres consommeront plus de batterie que prévu et peuvent interférer avec les modes d’économie d’énergie disponibles dans Windows pour ces appareils. Les périphériques alimentés par batterie disposent également de certains modes de gestion de l’alimentation, ce qui empêche l’exécution de toutes les applications, ce qui interfère avec la capacité de W32time’s à déterminer l’horloge et à conserver un temps précis. En outre, les horloges des appareils mobiles peuvent ne pas être très précises pour commencer.  Les conditions environnementales ambiantes affectent la précision de l’horloge et un appareil mobile peut passer d’une condition ambiante à la suivante, ce qui peut interférer avec sa capacité à conserver le temps avec précision.  Par conséquent, Microsoft ne recommande pas de configurer des appareils portables à batterie avec des paramètres de précision élevée. 

## <a name="why-is-time-important"></a>Pourquoi l’heure est-elle importante ?  
Il existe de nombreuses raisons pour lesquelles vous pouvez avoir besoin de temps précis.  Le cas typique de Windows est Kerberos, qui nécessite 5 minutes de précision entre le client et le serveur.  Toutefois, il existe de nombreuses autres zones qui peuvent être affectées par l’exactitude du temps, notamment :


- Réglementations gouvernementales telles que :
    - 50 ms de précision pour FINRA aux États-Unis
    - 1 ms ESMA (MiFID II) dans l’Union européenne.
- Algorithmes de chiffrement
- Systèmes distribués tels que cluster/SQL/Exchange et les bases de documents de documents
- Blockchain Framework pour les transactions Bitcoin
- Journaux distribués et analyse des menaces 
- Réplication AD
- PCI (le secteur des cartes de paiement), actuellement 1 seconde précision



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
