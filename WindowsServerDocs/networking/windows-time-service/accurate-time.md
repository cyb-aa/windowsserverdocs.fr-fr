---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Heure précise pour Windows Server 2016
description: Précision de synchronisation de temps dans Windows Server 2016 a été considérablement améliorée, tout en conservant une compatibilité NTP avec les versions antérieures de Windows.
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: ea4d957ee68f14f4568d3cefe664736585e50cce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864010"
---
# <a name="accurate-time-for-windows-server-2016"></a>Heure précise pour Windows Server 2016

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou version ultérieure

Le service de temps de Windows est un composant qui utilise un modèle de plug-in pour les fournisseurs de synchronisation de temps client et serveur.  Il existe deux fournisseurs de client intégré de Windows, et des plug-ins tiers sont disponibles. Utilise un seul fournisseur [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) ou [MS-NTP](https://msdn.microsoft.com/library/cc246877.aspx) pour synchroniser l’heure système locale à un serveur de référence conformes NTP et/ou MS-NTP. L’autre fournisseur est pour Hyper-V et synchronise les machines virtuelles (VM) à l’hôte Hyper-V.  Lorsque plusieurs fournisseurs existent, Windows sera choisir le meilleur fournisseur à l’aide de niveau de la couche tout d’abord, suivie de délai de racine, dispersion de racine et enfin temps décalage.

>[!NOTE]
>Pour une présentation rapide du service de temps de Windows, examinons ce [vidéo de présentation de haut niveau](https://aka.ms/WS2016TimeVideo).

<!-- Not sure what to do with the following -->
Dans cette rubrique, nous abordons... Ces rubriques qui sont liés à l’activation de l’heure précise : 

- Améliorations
- Mesures
- Meilleures pratiques

>[!IMPORTANT]
>Un addenda référencé par l’article Windows 2016 précise temps peut être téléchargé [ici](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).  Ce document fournit plus d’informations sur nos méthodes de test et de mesure.



>[!NOTE] 
>Le modèle de plug-in de fournisseur de temps windows est [documentée sur TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

## <a name="domain-hierarchy"></a>Hiérarchie de domaine
Configurations de domaine et autonome fonctionnent différemment.

- Membres du domaine utilisent un protocole NTP sécurisé qui utilise l’authentification pour garantir la sécurité et l’authenticité de la référence de temps.  Les membres du domaine synchroniser avec une horloge maître déterminée par la hiérarchie de domaine et un système de calcul de score.  Dans un domaine, il existe une couche hiérarchique de stratums de temps, selon laquelle chaque contrôleur de domaine pointe vers un contrôleur de domaine parent avec une couche de temps plus précis.  La hiérarchie se résout en contrôleur de domaine principal ou d’un contrôleur de domaine dans la forêt racine ou d’un contrôleur de domaine avec l’indicateur de domaine GTIMESERV, qui désigne un bon serveur de temps pour le domaine.  Consultez le [spécifier un Local fiable temps Service à l’aide de GTIMESERV](#GTIMESERV) section ci-dessous.

- Par défaut, les machines autonomes sont configurés pour utiliser time.windows.com.  Ce nom est résolu par votre serveur DNS, qui doit pointer vers une ressource détenue par Microsoft.  Comme toutes les références de temps situé à distance, les pannes de réseau peut empêchent la synchronisation.  Charge le trafic réseau et les chemins d’accès réseau asymétrique peuvent réduire la précision de la synchronisation de temps.  Précision de 1 ms, vous ne peut pas dépendre de des sources de temps à distance.

Étant donné que les invités Hyper-V doit avoir au moins deux fournisseurs de temps de Windows sélectionnables, l’heure de l’hôte et NTP, vous pouvez voir des comportements différents avec le domaine ou autonome lors de l’exécution en tant qu’invité.

> [!NOTE] 
> Pour plus d’informations sur la hiérarchie de domaine et le système de calcul de score, consultez le [« Quel est le Service de temps Windows ? »](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) billet de blog.

> [!NOTE]
> Couche est un concept utilisé dans les fournisseurs NTP et de Hyper-V, et sa valeur indique l’emplacement d’horloges dans la hiérarchie.  Couche 1 est réservée à l’horloge plus élevé et le niveau 0 est réservé pour le matériel supposé pour être précis et a peu ou aucun délai n’associé à.  Niveau 2 communiquer avec les serveurs de couche 1, au niveau 2 de niveau 3 et ainsi de suite.  Tandis qu’un niveau inférieur indique souvent une horloge plus précise, il est possible de détecter les écarts.  En outre, W32time accepte uniquement les temps de niveau 15 ou en dessous.  Pour afficher le niveau d’un client, utilisez *w32tm /query /status*.

## <a name="critical-factors-for-accurate-time"></a>Facteurs critiques pour une heure précise
Dans tous les cas pour une heure précise, il existe trois facteurs essentiels :

1. **Solid Source horloge** -l’horloge de la source dans votre domaine doit être stable et précis. Cela signifie généralement l’installation d’un périphérique GPS ou qui pointe vers une source de niveau 1, en tenant compte de #3. L’analogie atteint, si vous avez deux bateaux sur l’eau et que vous tentez de mesurer l’altitude d’un par rapport à l’autre, votre analyse de précision est préférable si la croisière source, c’est très stable et pas de déplacement. En va de même pour le moment, et si l’horloge de votre source n’est pas stable, la chaîne entière des horloges synchronisées est affectée, puis agrandie à chaque étape. Il doit également être accessible, car les interruptions dans la connexion interfèrent avec synchronisation date / heure. Et enfin, il doit être sécurisé. Si l’heure de référence n’est pas correctement conservées ou exploité par un tiers potentiellement malveillant, vous pouvez exposer votre domaine à des attaques de temporel.
2. **Horloge client stable** -un horloges client stable garantit que la dérive naturelle de l’oscillateur est containable.  NTP utilise plusieurs exemples à partir de plusieurs serveurs NTP potentiellement à la condition et la discipline de l’horloge de votre ordinateur local.  Il n’étape pas les modifications de temps, mais au lieu de cela ralentit ou accélère l’horloge locale afin que vous approcher de l’heure précise rapidement et à restez précises entre les demandes NTP.  Toutefois, si les oscillateur de l’horloge d’ordinateur client ne sont pas stable, des fluctuations plus entre ajustements peuvent se produire et les algorithmes que Windows utilise pour la condition de l’horloge ne fonctionnent pas avec précision.  Dans certains cas, les mises à jour du microprogramme peuvent être nécessaire pour une heure précise.
3. **Communication NTP symétrique** -il est essentiel que la connexion pour la communication de NTP est symétrique.  NTP utilise des calculs pour ajuster l’heure qui supposent que le correctif logiciel de réseau est symétrique.  Si le chemin d’accès le paquet NTP prend accédant au serveur prend un temps pour retourner différents, la précision est affectée.  Par exemple, le chemin d’accès peut changer en raison de modifications dans la topologie de réseau ou les paquets soient acheminés via les appareils qui ont des vitesses d’interface différente.


Pour les appareils de l’alimentation par batterie, mobiles et portable, vous devez prendre en compte différentes stratégies.  Conformément à notre recommandation, maintenant l’heure exacte nécessite l’horloge à être disciplinés une fois par seconde, ce qui correspond à la fréquence de mise à jour d’horloge. Ces paramètres consommera plus de puissance de batterie que prévu et peut interférer avec l’économie d’énergie modes disponibles dans Windows pour ces appareils. Appareils de l’alimentation par batterie ont également certains modes d’alimentation qui empêche toutes les applications en cours d’exécution, qui interfère avec possibilité de W32time discipline de l’horloge et tenir à jour heure précise. En outre, les horloges des appareils mobiles peut-être pas très précis pour commencer.  Conditions ambiantes affectent la précision de l’horloge et un appareil mobile peut déplacer à partir d’une condition ambiante à l’autre qui peut-être interférer avec sa capacité à conserver un temps avec précision.  Par conséquent, Microsoft ne recommande pas de configurer les périphériques portables alimenté par batterie avec les paramètres de haute précision. 

## <a name="why-is-time-important"></a>Pourquoi le temps est important ?  
Il existe de nombreuses raisons que vous devrez peut-être heure précise.  Le cas par défaut pour Windows est Kerberos, ce qui oblige les 5 minutes de précision entre le client et le serveur.  Toutefois, il existe beaucoup d’autres domaines qui peut être affectés par la précision du temps, y compris :


- Réglementations gouvernementales telles que :
    - précision de 50 ms pour la FINRA dans le fuseau horaire
    - 1 ESMA ms (MiFID II) dans l’Union européenne.
- Algorithmes de chiffrement
- Systèmes distribués tels que le Cluster/SQL/Exchange et les bases de données de Document
- Framework de Blockchain pour les transactions de bitcoin
- Les journaux distribués et analyse des menaces 
- Réplication AD
- Norme PCI (Payment Card Industry), précision deuxième actuellement 1



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
