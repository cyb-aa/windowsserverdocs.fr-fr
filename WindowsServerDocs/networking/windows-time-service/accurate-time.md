---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Heure exacte pour Windows Server 2016
description: La précision de la synchronisation du temps dans Windows Server 2016 a été considérablement améliorée, tout en conservant une compatibilité NTP complète avec les anciennes versions de Windows.
author: eross-msft
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 3320c67d52978f0e9abaae7d5bec9b4fcb727fd6
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315079"
---
# <a name="accurate-time-for-windows-server-2016"></a>Heure exacte pour Windows Server 2016

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou version ultérieure

Le service de temps Windows est un composant qui utilise un modèle de plug-in pour les fournisseurs de synchronisation de l’heure du client et du serveur.  Il existe deux fournisseurs clients intégrés sur Windows et des plug-ins tiers sont disponibles. L’un des fournisseurs utilise [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) ou [MS-NTP](https://msdn.microsoft.com/library/cc246877.aspx) pour synchroniser l’heure système locale sur un serveur de référence compatible avec NTP et/ou MS-NTP. L’autre fournisseur, destiné à Hyper-V, synchronise les machines virtuelles avec l’hôte Hyper-V.  Si plusieurs fournisseurs existent, Windows choisit le meilleur fournisseur en fonction, successivement, du niveau de couche, du délai racine, de la dispersion racine et du décalage de temps.

> [!NOTE]
> Pour une vue d’ensemble rapide du service de temps Windows, jetez un coup d’œil à cette [vidéo de présentation générale](https://aka.ms/WS2016TimeVideo).

Dans cette rubrique, nous aborderons les sujets ci-après, liés à l’activation du temps précis : 

- Améliorations
- Mesures
- Meilleures pratiques

> [!IMPORTANT]
> Un addendum référencé par l’article Précision de l’heure dans Windows Server 2016 peut être téléchargé [ici](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).  Ce document fournit plus de détails sur nos méthodologies de test et de mesure.

> [!NOTE] 
> Le modèle de plug-in du fournisseur de temps Windows est [documenté sur TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

## <a name="domain-hierarchy"></a>Hiérarchie de domaine
Les configurations de domaine et autonomes fonctionnent différemment.

- Les membres de domaine utilisent un protocole NTP sécurisé, qui se sert de l’authentification pour garantir la sécurité et l’authenticité de la référence d’heure.  Les membres de domaine se synchronisent avec une horloge maître déterminée par la hiérarchie de domaine et un système d’évaluation.  Dans un domaine, il existe une couche hiérarchique de couches de temps, selon laquelle chaque contrôleur de domaine pointe vers un contrôleur de domaine parent avec une couche temporelle plus précise.  La hiérarchie correspond au contrôleur de domaine principal ou à un contrôleur de domaine dans la forêt racine, ou à un contrôleur de domaine avec l’indicateur de domaine GTIMESERV, qui désigne un serveur de temps approprié pour le domaine.  Consultez la section Spécifier un service de temps fiable local à l’aide de GTIMESERV ci-dessous.

- Les ordinateurs autonomes sont configurés pour utiliser time.windows.com par défaut.  Ce nom est résolu par votre serveur DNS, qui doit pointer vers une ressource appartenant à Microsoft.  Comme toutes les références d’heure à distance, les pannes de réseau peuvent empêcher la synchronisation.  Les charges de trafic réseau et les chemins réseau asymétriques peuvent réduire la précision de la synchronisation du temps.  Pour une précision de 1 ms, vous ne pouvez pas dépendre d’une source de temps distante.

Étant donné que les invités Hyper-V ont le choix entre au moins deux fournisseurs de temps Windows, l’heure de l’hôte et NTP, vous risquez de voir des comportements différents avec la configuration de type domaine ou autonome lors de l’exécution en tant qu’invité.

> [!NOTE] 
> Pour plus d’informations sur la hiérarchie de domaine et le système d’évaluation, consultez le billet de blog [What is Windows Time Service?](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) .

> [!NOTE]
> La couche est un concept utilisé à la fois dans les fournisseurs NTP et Hyper-V, et sa valeur indique l’emplacement des horloges dans la hiérarchie.  La couche 1 est réservée à l’horloge de niveau supérieur, tandis que la couche 0 est réservée au matériel censé être précis et auquel est associé un délai infime ou nul.  La couche 2 communique avec les serveurs de couche 1, la couche 3 avec la couche 2, et ainsi de suite.  Bien qu’une couche inférieure indique souvent une horloge plus précise, il est possible de trouver des différences.  De plus, W32time accepte uniquement l’heure des couches 15 ou inférieures.  Pour voir la couche d’un client, utilisez *w32tm /query /status*.

## <a name="critical-factors-for-accurate-time"></a>Facteurs critiques pour une heure précise
Pour une heure précise, il existe systématiquement trois facteurs critiques :

1. **Horloge source solide** : l’horloge source dans votre domaine doit être stable et précise. Cela signifie généralement l’installation d’un appareil GPS ou le pointage vers une source de couche 1, en tenant compte du point 3. Par analogie, si vous avez deux bateaux sur l’eau et que vous essayez de mesurer l’altitude de l’un par rapport à l’autre, la précision est meilleure si le bateau source est très stable et ne bouge pas. Il en va de même pour l’heure, et si votre horloge source n’est pas stable, la chaîne entière des horloges synchronisées est affectée et agrandie à chaque étape. Elle doit également être accessible, car les interruptions de la connexion interfèrent avec la synchronisation de l’heure. Enfin, elle doit être sécurisée. Si la référence d’heure n’est pas correctement gérée ou qu’elle est gérée par un tiers potentiellement malveillant, vous risquez d’exposer votre domaine à des attaques basées sur l’heure.
2. **Horloge cliente stable** : les horloges clientes stables garantissent que la dérive naturelle de l’oscillateur est maîtrisable.  NTP utilise plusieurs échantillons à partir d’un voire plusieurs serveurs NTP pour conditionner et discipliner l’horloge de vos ordinateurs locaux.  Il n’arrête pas les changements de temps, mais ralentit ou accélère plutôt l’horloge locale, afin que vous puissiez vous approcher rapidement de l’heure précise et conserver cette précision entre les demandes NTP.  Toutefois, si l’oscillateur de l’horloge de l’ordinateur client n’est pas stable, des fluctuations entre les ajustements peuvent se produire et les algorithmes que Windows utilise pour conditionner l’horloge ne fonctionnent pas correctement.  Dans certains cas, des mises à jour du microprogramme peuvent être nécessaires pour avoir une heure précise.
3. **Communication NTP symétrique** : il est essentiel que la connexion pour la communication NTP soit symétrique.  Pour ajuster l’heure, le protocole NTP utilise des calculs qui partent du principe que le correctif réseau est symétrique.  Si le temps que met le paquet NTP pour atteindre le serveur diffère du temps du chemin inverse, la précision est affectée.  Par exemple, le chemin peut changer en raison de modifications apportées à la topologie du réseau ou du fait que les paquets sont routés par le biais d’appareils qui ont des vitesses d’interface différentes.

Pour les appareils fonctionnant sur batterie, à la fois mobiles et portables, vous devez envisager différentes stratégies.  Conformément à notre recommandation, le fait de conserver une heure précise exige que l’horloge soit disciplinée une fois par seconde, soit la fréquence de mise à jour de l’horloge. Ces paramètres consomment plus de batterie que prévu et peuvent interférer avec les modes d’économie d’énergie disponibles dans Windows pour ces appareils. Les appareils alimentés par batterie disposent également de certains modes de gestion de l’alimentation qui empêchent l’exécution de toutes les applications, ce qui interfère avec la capacité de W32time à discipliner l’horloge et à conserver une heure précise. De plus, les horloges des appareils mobiles peuvent ne pas être très précises au départ.  Les conditions environnementales affectent la précision de l’horloge et un appareil mobile peut passer d’une condition à une autre, ce qui peut interférer avec sa capacité à conserver un temps précis.  Ainsi, Microsoft ne vous recommande pas de configurer des appareils portables alimentés par batterie avec des paramètres de haute précision. 

## <a name="why-is-time-important"></a>Pourquoi l’heure est importante ?  
Il existe de nombreuses raisons pour lesquelles vous pouvez avoir besoin d’une heure précise.  Le cas typique pour Windows est Kerberos, qui nécessite une précision de 5 minutes entre le client et le serveur.  Toutefois, il existe de nombreux autres domaines qui peuvent être affectés par la précision du temps, notamment :


- Lois gouvernementales telles que :
    - Précision de 50 ms pour FINRA aux États-Unis
    - 1 ms : ESMA (MiFID II) dans l’Union européenne.
- Algorithmes de chiffrement
- Systèmes distribués tels que cluster/SQL/Exchange et les bases de données de documents
- Framework de blockchain pour les transactions en bitcoins
- Journaux distribués et analyse des menaces 
- Réplication AD
- PCI (Payment Card Industry) : précision de 1 seconde



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
