---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Heure précise Windows 2016
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 3/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: f4b61dbf07fbc21820dd7b9326bbc990e4db3602
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/05/2018
---
# <a name="windows-server-2016-accurate-time"></a>Heure précise de Windows Server 2016

>S’applique à: Windows Server2016

Précision de la synchronisation de temps dans Windows Server 2016 a été améliorée considérablement, tout en conservant une compatibilité NTP avec des versions antérieures de Windows. Conditions de fonctionnement raisonnable, vous pouvez conserver un 1 précision ms en ce qui concerne l’heure UTC ou supérieure pour les membres du domaine Windows Server 2016 et mise à jour anniversaire Windows 10.

Le service de temps Windows est un composant qui utilise un modèle de plug-in pour les fournisseurs de synchronisation de temps client et serveur.  Il existe deux fournisseurs client intégré sur Windows et les plug-ins tiers sont disponibles. Utilise un seul fournisseur [NTP (RFC1305)](https://tools.ietf.org/html/rfc1305) ou [MS-NTP](https://msdn.microsoft.com/en-us/library/cc246877.aspx) pour synchroniser l’heure système locale à un serveur de référence conforme NTP et/ou MS-NTP. L’autre fournisseur est pour Hyper-V et synchronise les machines virtuelles (VM) à l’ordinateur hôte Hyper-V.  Lorsque plusieurs fournisseurs existent, Windows choisir le meilleur fournisseur à l’aide de niveau de la couche tout d’abord, suivie de délai de racine, dispersion racine et enfin décalage du temps.

>[!NOTE]
>Pour obtenir une vue d’ensemble du service de temps Windows, jeter un coup de œil cela [vue d’ensemble vidéo ](https://aka.ms/WS2016TimeVideo).

<!-- Not sure what to do with the following -->
Dans cette rubrique, nous expliquons... ces rubriques car elles sont associées à l’activation de l’heure précise: 

- Améliorations
- Mesures
- Meilleures pratiques

>[!IMPORTANT]
> Un addendum référencé par l’article Windows 2016 précis temps peut être téléchargé [ici](http://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).  Ce document fournit des détails sur nos méthodes de test et d’évaluation.



> [!NOTE] 
> Le modèle de plug-in de fournisseur de temps windows est [documentées sur TechNet](https://msdn.microsoft.com/en-us/library/windows/desktop/ms725475%28v=vs.85%29.aspx).
<!-- -->





## <a name="domain-hierarchy"></a>Hiérarchie de domaine
Configurations de domaine et autonome fonctionnent différemment.

- Membres du domaine utilisent un protocole NTP sécurisé, qui utilise l’authentification pour garantir la sécurité et l’authenticité de la période de référence.  Les membres du domaine synchronisation avec une horloge maître déterminée par la hiérarchie de domaine et un système de score.  Dans un domaine, il existe une couche hiérarchique de stratums de temps, dans laquelle chaque contrôleur de domaine pointe vers un contrôleur de domaine parent avec un niveau plus précis de temps.  La hiérarchie résout le contrôleur de domaine principal ou d’un contrôleur de domaine dans la forêt racine ou d’un contrôleur de domaine avec l’indicateur de domaine GTIMESERV, qui indique un bon serveur de temps pour le domaine.  Consultez le [spécifier un Local fiable temps Service à l’aide de GTIMESERV](#GTIMESERV) ci-dessous.

- Ordinateurs autonomes sont configurés pour utiliser time.windows.com par défaut.  Ce nom est résolu par votre fournisseur de services Internet, qui doit pointer vers une propriété de ressource par Microsoft.  Comme toutes les références de temps situé à distance, les pannes de réseau peuvent empêcher la synchronisation.  Le trafic réseau et les chemins d’accès réseau asymétrique peuvent réduire la précision de la synchronisation de temps.  Pour une 1 précision ms, vous ne peut pas dépendre de des sources de temps à distance.

Étant donné que les invités Hyper-V doit avoir au moins deux fournisseurs de temps Windows à choisir, l’heure de l’hôte et NTP, vous pouvez voir des comportements différents avec autonome ou de domaine lors de l’exécution en tant qu’invité.

> [!NOTE] 
> Pour plus d’informations sur la hiérarchie de domaine et du système de score, voir la ["Qu’est le Service de temps Windows?»](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) Billet de blog.

> [!NOTE]
> Couche est un concept utilisé dans le protocole NTP et Hyper-V des fournisseurs, et sa valeur indique l’emplacement de l’horloge de la hiérarchie.  Couche 1 est réservée pour le plus haut niveau horloge et le niveau 0 est réservé pour le matériel supposé pour être précises et a peu ou aucun délai n’associé.  Niveau 2 communiquer avec les serveurs de niveau 1, niveau 3 au niveau 2 et ainsi de suite.  Tandis qu’un niveau inférieur indique souvent une horloge plus précise, il est possible de trouver les différences.  En outre, W32time accepte uniquement les temps de couche 15 ou en dessous.  Pour afficher le niveau d’un client, utilisez *w32tm /query /status*.

## <a name="critical-factors-for-accurate-time"></a>Facteurs critiques pour une heure précise
Dans chaque cas pour une heure précise, il existe trois facteurs importants:

1. **L’horloge Source solide** -l’horloge de la source dans votre domaine doit être stable et précises. Cela signifie généralement l’installation d’un périphérique GPS ou en pointant sur une source de niveau 1, compte tenu de 3 #. L’analogie est redirigée, si vous avez deux bateaux sur l’eau et que vous essayez de mesurer l’altitude de l’un par rapport à l’autre, votre précision est meilleure si le bateau source est très stables et déplacement pas. En va de même pour le moment, et si l’horloge de votre source n’est pas stable, puis la chaîne entière des horloges synchronisées est affectée et agrandie à chaque étape. Il doit également être accessible, car l’interruption de la connexion sera interférer avec la synchronisation de l’heure. Et, enfin, il doit être sécurisé. Si l’heure de référence n’est pas correctement gérées ou géré par un tiers potentiellement malveillant, vous pouvez exposer votre domaine à des attaques de l’heure en fonction.
2. **Horloge client stable** -un horloges client stable garantit que la dérive naturelle de l’oscillateur containable.  NTP utilise plusieurs échantillons à partir de plusieurs serveurs NTP potentiellement de condition discipline de l’horloge de votre ordinateur local.  Il ne pas étape les changements d’heure, mais plutôt ralentit ou accélère l’horloge locale qui approche la précision du temps rapidement et à rester précise entre les demandes NTP.  Toutefois, si oscillateur de l’horloge d’ordinateur client n’est pas stable, des fluctuations plus entre les ajustements peuvent se produire et les algorithmes de que Windows utilise pour la condition de l’horloge ne fonctionnent pas avec précision.  Dans certains cas, les mises à jour du microprogramme peuvent être nécessaire pour l’heure précise.
3. **Communication NTP symétrique** -il est essentiel que la connexion pour la communication NTP est symétrique.  NTP utilise des calculs pour régler l’heure qui supposent que le correctif de réseau est symétrique.  Si le chemin d’accès le paquet NTP prend atteindre le serveur prend un temps pour renvoyer différentes, la précision est affectée.  Par exemple, le chemin d’accès peut changer en raison de modifications dans la topologie de réseau, ou les paquets routés via les appareils disposant de vitesses différentes interface.


Pour les périphériques de l’alimentation par batterie, mobile et portable, vous devez tenir compte des stratégies différentes.  En fonction de notre recommandation, maintenant l’heure exacte nécessite l’horloge pour être assujetties à une fois par seconde, ce qui correspond à la fréquence d’horloge mise à jour. Ces paramètres consomme plus de puissance de batterie que prévu et peut interférer avec l’économie d’énergie modes disponibles dans Windows pour ces périphériques. Alimenté par batterie possèdent également certains modes d’alimentation qui empêche toutes les applications en cours d’exécution, ce qui interfère avec la possibilité de W32time discipline de l’horloge et mettre à jour de l’heure précise. En outre, les horloges dans les périphériques mobiles peut-être pas très précis tout d’abord.  Conditions d’environnement ambiantes affectent la précision de l’horloge et un périphérique mobile peut déplacer à partir d’une condition ambiante à la suivante qui peut interférer avec sa capacité de conserver un temps avec précision.  Par conséquent, Microsoft ne recommande pas la configurer avec les paramètres de haute précision alimenté par batterie des appareils mobiles. 

## <a name="why-is-time-important"></a>Pourquoi le temps est important?  


## <a name="windows-server-2016-improvements"></a>Améliorations de Windows Server 2016
### <a name="windows-time-service-and-ntp"></a>NTP et le Service de temps Windows
Windows Server 2016 a amélioré les algorithmes, qu'il utilise pour l’heure correcte et la condition de l’horloge locale pour synchroniser l’heure UTC.  NTP utilise 4 valeurs pour calculer le décalage, selon les horodatages de la demande/réponse du client et serveur demande/réponse.  Toutefois, les réseaux sont bruyants et il peut y avoir des pics dans les données à partir de NTP en raison de la congestion du réseau et d’autres facteurs qui affectent la latence du réseau.  Algorithmes Windows 2016 moyenne ce bruit à l’aide d’un nombre de différentes techniques qui se traduit par une horloge stable et précise.  En outre, la source de nous utilisons pour les références de l’heure précise une API améliorée qui nous donne une meilleure résolution.  Grâce à ces améliorations, nous sommes en mesure d’atteindre 1 précision de ms en ce qui concerne l’heure UTC dans un domaine.

### <a name="hyper-v"></a>Hyper-V
Windows 2016 a amélioré le service TimeSync Hyper-V. Améliorations comprennent heure initiale plus précis sur le démarrage de l’ordinateur virtuel ou restauration de l’ordinateur virtuel et correction de latence d’interruption pour les exemples fournis à w32time.  Cette amélioration permet de rester dans 10µs de l’ordinateur hôte avec un RMS (racine signifie carré, ce qui indique l’écart), de 50µs, même sur un ordinateur avec 75% de la charge.

> [!NOTE]
> Consultez cet article sur [architecture Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx) pour plus d’informations.

> [!NOTE]
> Charge a été créé à l’aide de banc d’essai prime95 à l’aide du profil équilibrée.

En outre, le niveau de la couche des rapports de l’ordinateur hôte à l’invité est plus transparent.  Précédemment, l’ordinateur hôte présenterait un fixe de niveau 2, quelle que soit sa précision.  Avec les modifications apportées dans Windows Server 2016, l’hôte signale un niveau supérieur de la couche d’hôte, ce qui entraîne le moment idéal pour les invités virtuelles.  Le niveau de l’ordinateur hôte est déterminée par w32time par le biais d’éteindre en fonction de son heure de la source.  Windows 2016 invités trouveront l’horloge plus précise, plutôt que par défaut à l’ordinateur hôte appartenant à un domaine.  Il a été pour cette raison que nous avons recommandé de désactiver manuellement le paramètre de fournisseur de temps Hyper-V pour les ordinateurs appartenant à un domaine dans Windows 2012R2 et en dessous.

### <a name="monitoring"></a>Analyse
Compteurs de performance ont été ajoutés.  Celles-ci vous permettent de ligne de base, surveiller et résoudre les problèmes de précision du temps.  Ces compteurs sont les suivantes:

Compteur|Description|
----- | ----- |
Calculé décalage|   L’heure absolue décalage entre l’horloge système et la source de temps choisie, calculées par le Service W32Time en microsecondes. Lorsqu’un nouvel échantillon valide est disponible, l’heure calculée est mis à jour avec le décalage indiqué par l’exemple. Il s’agit le décalage de l’heure réelle de l’horloge locale. W32Time lance la correction d’horloge à l’aide de ce décalage et met à jour de l’heure calculée entre les échantillons avec le décalage restants qui doit être appliquée à l’horloge locale. Précision de l’horloge peut être suivie à l’aide de ce compteur de performance avec un intervalle d’interrogation faible (par ex.: 256 secondes ou moins) et recherchez la valeur du compteur être inférieure à la limite de précision souhaitée horloge.|
Ajustement de fréquence d’horloge| L’ajustement de fréquence d’horloge absolu apportée à l’horloge système local par W32Time en parties par milliard. Ce compteur permet de visualiser les actions à entreprendre en W32time.|
Délai de traduction NTP|    Délai aller-retour plus récente rencontré par le Client NTP de recevoir une réponse à partir du serveur (en microsecondes). Il s’agit du temps écoulé sur le client NTP entre la transmission d’une demande au serveur NTP et reçoit une réponse valide à partir du serveur. Ce compteur permet de caractériser les retards rencontrés par le client NTP. Allers agrandir ou différents peuvent ajouter du bruit aux calculs de temps NTP, qui à son tour peuvent affecter la précision de la synchronisation horaire via NTP.|
Nombre de sources du Client NTP|    Nombre active des sources de temps NTP utilisé par le Client NTP. Il s’agit d’un nombre d’adresses IP actives distincts de serveurs de temps qui ne répondent pas aux demandes de ce client. Ce numéro peut être supérieure ou inférieure à homologues configurés, en fonction de la résolution DNS des noms d’homologues et la possibilité de portée actuelle.|
Serveur NTP les demandes entrantes|   Nombre de demandes reçues par le serveur NTP (demandes par seconde).|
Réponses sortant du serveur NTP|  Nombre de demandes traitées par le serveur NTP (réponses par seconde).|

Les compteurs de 3 premiers ciblent des scénarios pour la résolution des problèmes de précision.  La précision du temps de résolution des problèmes et NTP ci-dessous, sous [meilleures pratiques](#BestPractices), a plus de détails.
Les 3 derniers compteurs couvrent les scénarios de serveur NTP et sont utile quand déterminer la charge et la ligne de base les performances en cours.

### <a name="configuration-updates-per-environment"></a>Configuration des mises à jour par l’environnement
La section suivante décrit les modifications apportées à la configuration par défaut entre Windows 2016 et les versions antérieures pour chaque rôle.  Les paramètres de Windows Server 2016 et Windows 10 anniversaire Update(build 14393), sont désormais unique qui est la raison pour laquelle il visualise les colonnes. 

|Rôle|Paramètre|Windows Server2016|Windows 10 Version 1607|Windows Server2012R2</br>Windows Server2008R2</br>Windows10|
|---|---|---|---|---|
|**Autonome/Nano Server**||||
| |*Serveur de temps*|Time.Windows.com|NA|Time.Windows.com|
| |*Fréquence d’interrogation*|64 - 1024 secondes|NA|Une fois par semaine|
| |*Fréquence d’horloge mise à jour*|Une fois par seconde|NA|Une fois une heure|
|**Client autonome**||||
| |*Serveur de temps*|NA|Time.Windows.com|Time.Windows.com|
| |*Fréquence d’interrogation*|NA|Une fois par jour|Une fois par semaine|
| |*Fréquence d’horloge mise à jour*|NA|Une fois par jour|Une fois par semaine|
|**Contrôleur de domaine**||||
| |*Serveur de temps*|CONTRÔLEUR DE DOMAINE PRINCIPAL/GTIMESERV|NA|CONTRÔLEUR DE DOMAINE PRINCIPAL/GTIMESERV|
| |*Fréquence d’interrogation*|64-1024 secondes|NA|1024 - 32768 secondes|
| |*Fréquence d’horloge mise à jour*|Une fois par jour|NA|Une fois par semaine|
|**Serveur membre de domaine**||||
| |*Serveur de temps*|CONTRÔLEUR DE DOMAINE|NA|CONTRÔLEUR DE DOMAINE|
| |*Fréquence d’interrogation*|64-1024 secondes|NA|1024 - 32768 secondes|
| |*Fréquence d’horloge mise à jour*|Une fois par seconde|NA|Une fois toutes les 5 minutes|
|**Client membre de domaine**||||
| |*Serveur de temps*|NA|CONTRÔLEUR DE DOMAINE|CONTRÔLEUR DE DOMAINE|
| |*Fréquence d’interrogation*|NA|1204 - 32768 secondes|1024 - 32768 secondes|
| |*Fréquence d’horloge mise à jour*|NA|Une fois toutes les 5 minutes|Une fois toutes les 5 minutes|
|**Invité Hyper-V**||||
| |*Serveur de temps*|Choisit la meilleure option basée sur la couche du serveur hôte et l’heure|Choisit la meilleure option basée sur la couche du serveur hôte et l’heure|Défini par défaut sur l’ordinateur hôte|
| |*Fréquence d’interrogation*|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|
| |*Fréquence d’horloge mise à jour*|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|

>[!NOTE]
>Pour Linux dans Hyper-V, voir la [Linux de ce qui permet d’utiliser l’heure de l’hôte Hyper-V](#AllowingLinux) ci-dessous.

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impact de l’augmentation de l’interrogation et de fréquence d’horloge mise à jour
Afin de fournir une heure plus précise, les valeurs par défaut pour les mises à jour de l’horloge et les fréquences de l’interrogation sont augmentées qui nous permet d’effectuer de petites modifications plus souvent.  Cela entraîne une augmentation du trafic UDP/NTP, toutefois, ces paquets sont petites il doit y avoir très peu ou pas d’impact sur les liaisons haut débit. L’avantage, qui est toutefois, heure doit être une meilleure sur une plus grande variété de matériel et d’environnements.

Pour les périphériques de sauvegarde sur batterie, augmenter la fréquence d’interrogation peut entraîner des problèmes.  Appareils de la batterie ne stockent pas le temps pendant que désactivé.  Lors de leur reprise, elle peut nécessiter des corrections fréquentes à l’horloge.  Augmentation de la fréquence d’interrogation entraîne l’horloge dans un état instable et peut également utiliser plus de puissance.  Microsoft vous recommande de que ne pas modifier les paramètres par défaut du client.

Contrôleurs de domaine doivent être affectées au minimum même avec l’effet multipliée accrue mises à jour à partir de Clients NTP dans un domaine Active Directory.  NTP a une consommation de ressources beaucoup plus petite par rapport à d’autres protocoles et un impact marginal.  Vous êtes plus susceptible atteindre des limites pour les autres fonctionnalités de domaine avant affectées par les paramètres accrues pour Windows Server 2016.  Active Directory utilise NTP sécurisé, ce qui tend à synchroniser fois moins précise que NTP simple, mais nous avons vérifié qu’il évoluera à niveau les clients deux pas le contrôleur de domaine principal.

En tant que conservateur plan, vous devez réserver 100 demandes NTP par seconde par cœur.  Par exemple, un domaine est constitué de 4 contrôleurs de domaine avec 4 cœurs, vous devez être en mesure de traiter les demandes NTP 1600 par seconde.  Si vous disposez de 10 k clients configurés pour synchroniser l’heure une fois toutes les secondes 64, et les demandes sont reçues uniformément au fil du temps, vous pourriez voir 10 000/64 ou environ 160 demandes/seconde, réparties sur tous les contrôleurs de domaine.  Cela relève facilement notre 1600 NTP demandes par seconde selon cet exemple.  Ces conservateurs recommandations de planification et bien entendu ont une dépendance volumineuse sur votre réseau, les vitesses de processeur et les charges, donc comme toujours planifié et test dans vos environnements.

Il est également important de noter que si vos contrôleurs de domaine sont en cours d’exécution avec une charge importante du processeur, supérieure à 40%, cela certainement ajouter aux réponses NTP du bruit et affecter votre précision du temps dans votre domaine.  Là encore, vous devez tester dans votre environnement pour comprendre les résultats réels.

## <a name="time-accuracy-measurements"></a>Mesures de précision de temps
### <a name="methodology"></a>Méthodologie
Pour mesurer la précision du temps pour Windows Server 2016, nous avons utilisé une variété d’outils, les méthodes et les environnements.  Vous pouvez utiliser ces techniques de mesurer et optimiser votre environnement et de déterminer si les résultats de la précision répondre à vos besoins. 

Deux serveurs NTP de haute précision avec le matériel GPS consistait à notre horloge du domaine source.  Nous avons utilisé également un ordinateur de test de référence distincte pour les mesures ayant également fait matériel GPS de haute précision installé à partir d’un autre fabricant.  Pour certains des tests, vous devez une source fiable et précise horloge à utiliser en tant que référence en plus de votre source d’horloge de domaine.

Nous avons utilisé quatre méthodes différentes pour mesurer la précision avec les machines physiques et virtuelles. Plusieurs méthodes fournies moyens indépendants pour valider les résultats.


1. Mesurer l’horloge locale, qui est soumise par w32tm, par rapport à notre machine de test de référence qui dispose de matériel GPS distinct.  
2.  Mesure les pings NTP à partir du serveur NTP aux clients qui utilisent W32tm «stripchart»
3.  Mesure les pings NTP à partir du client vers le serveur NTP à l’aide de W32tm «stripchart»
4.  Résultats de mesure Hyper-V à partir de l’ordinateur hôte à l’invité à l’aide du compteur de tampon le temps (TSC).  Ce compteur est partagé entre les partitions et l’heure système dans les deux partitions.  Nous avons calculé la différence entre l’heure de l’hôte et l’heure du client sur l’ordinateur virtuel.  Ensuite, nous utilisons l’horloge TSC pour interpoler l’heure de l’hôte de l’invité, dans la mesure où les mesures de ne pas se produire en même temps.  En outre, nous utilisons le facteur d’horloge TSV des retards et la latence dans l’API.

W32tm est intégrée, mais les autres outils utilisés au cours de nos tests sont disponibles pour l’espace de stockage Microsoft sur GitHub comme open source pour vos tests et l’utilisation.  WIKI sur l’espace de stockage contient des informations supplémentaires décrivant comment utiliser les outils pour effectuer des mesures.

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

Les résultats des tests ci-dessous sont un sous-ensemble des mesures, nous avons un des environnements de test.  Ils illustrent la précision conservée au début de la hiérarchie de temps et les clients du domaine enfant à la fin de la hiérarchie de temps.  Cela est comparée aux ordinateurs mêmes dans une topologie 2012 en fonction de comparaison.

### <a name="topology"></a>Topologie
Par comparaison, nous avons testé une topologie de base Windows Server 2016 et Windows Windows Server 2012 R2.  Les deux topologies sont constituées de deux ordinateurs hôtes Hyper-V physiques qui font référence à un ordinateur Windows Server 2016 avec GPS horloge matériel installé.  Chaque hôte exécute 3 invités appartenant à un domaine windows, qui sont organisées en fonction de la topologie est la suivante.  Les lignes représentent la hiérarchie de temps et le protocole/transport qui est utilisé.

![Temps Windows](media/Windows-2016-Accurate-Time/topology1.png)

![Temps Windows](media/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Vue d’ensemble de résultats graphique
Les deux graphiques suivants représentent la précision du temps pour deux membres spécifiques dans un domaine basé sur la topologie ci-dessus.  Chaque graphique affiche le 2012R2 de Windows Server et les 2016 résultats superposés, qui illustre les améliorations visuellement.  La précision a été mesurer à partir avec l’ordinateur invité par rapport à l’ordinateur hôte.  Les données graphiques représente un sous-ensemble de l’ensemble des tests que nous avons réalisé et affiche le meilleur des cas et le pire des cas.  

![Temps Windows](media/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Performances du contrôleur de domaine principal du domaine racine
Le contrôleur de domaine principal racine est synchronisé à l’hôte Hyper-V (à l’aide de VMIC) qui est un Windows Server 2016 avec le matériel GPS est avéré pour être précis et stables.  Il s’agit d’une obligation indispensable pour 1 ms précision, qui s’affiche en tant que la zone ombrée verte.

![Temps Windows](media/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Performances du Client de domaine enfant
Les clients du domaine enfant est attaché à un contrôleur de domaine enfant principal qui communique avec le contrôleur de domaine principal racine.  Il est temps est également au sein de 1 ms exigence.

![Temps Windows](media/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>Test de longue Distance
Le tableau suivant compare les sauts de réseau virtuel 1 à 6 tronçons réseau physique avec Windows Server 2016.  Deux graphiques sont superposés sur eux avec une transparence pour afficher les données qui se chevauchent.  Augmentation tronçons réseau signifient latence plus élevée et les écarts de temps supérieure.  Le graphique est agrandie et, donc, de 1 ms limites, représentés par la zone verte, est la plus grande.  Comme vous pouvez le constater, l’heure est toujours réglée 1 ms avec plusieurs sauts.  Il est négatif décalée, qui montre une asymétrie réseau.  Bien entendu, chaque réseau est différent, et dépendent de mesures sur une multitude de facteurs environnementaux.

![Temps Windows](media/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>Meilleures pratiques pour la synchronisation d’horloge précise
### <a name="solid-source-clock"></a>Source solide horloge
Un temps d’ordinateurs virtuels n’est pas aussi bien que l’horloge source, avec qu'il se synchronise.  Pour obtenir un 1 ms de précision, vous devez matériel GPS ou un dispositif de temps sur votre réseau, vous référencez en tant que l’horloge source maître.  À l’aide de la valeur par défaut de time.windows.com, peut fournir pas un stable et une heure locale.  En outre, à mesure que vous plus éloigné de l’horloge de la source, le réseau affecte la précision.  Ayant une horloge principale source de données de chaque centre est requis pour la plus grande précision.

### <a name="hardware-gps-options"></a>Matériel GPS Options
Il existe différentes solutions matérielles que peuvent offrir une heure précise.  En règle générale, les solutions aujourd'hui reposent sur antennes GPS.  Il existe également des cases d’option et les solutions de modem d’accès à distance à l’aide de lignes dédiées.  Ils attacher à votre réseau, soit comme un dispositif ou branchez sur un PC, par exemple Windows via un dispositif PCIe ou USB.  Différentes options fournira différents niveaux de précision, et comme toujours, les résultats dépendent de votre environnement.  Les variables qui affectent la précision incluent la disponibilité GPS, la stabilité du réseau et de charge et du matériel PC.  Il s’agit des facteurs importants lorsque vous choisissez une horloge de la source, comme nous avons indiqué, est obligatoire pour stable et précis le temps.

### <a name="domain-and-synchronizing-time"></a>Domaine et l’heure de synchronisation
Membres du domaine utilisent la hiérarchie de domaine pour déterminer à quel ordinateur qu’ils utilisent en tant que source pour synchroniser l’heure.  Trouve un autre ordinateur à synchroniser avec et enregistrez-le en tant que source d’horloge de chaque membre du domaine.  Chaque type de membre de domaine suit un ensemble de règles différents afin de trouver une source de l’horloge pour la synchronisation de temps.  Le contrôleur de domaine principal à la racine de forêt est la source d’horloge par défaut pour tous les domaines.  Vous trouverez ci-dessous des rôles différents et description du niveau élevée pour la façon dont ils rechercher une source:


- **Contrôleur de domaine avec le rôle de contrôleur de domaine principal** – cet ordinateur est la source de temps faisant autorité pour un domaine. Il aura l’heure la plus précise disponible dans le domaine et doit synchroniser avec un contrôleur de domaine dans le domaine parent, sauf dans les cas où [GTIMESERV](#GTIMESERV) rôle est activé. 
- **Tout autre contrôleur de domaine** – cet ordinateur sera agir en tant que source de temps pour les clients et les serveurs membres du domaine. Un contrôleur de domaine peut être synchronisé avec le contrôleur de domaine principal de son propre domaine, ou n’importe quel contrôleur de domaine dans son domaine parent.
- **Les serveurs clients/membres** – cet ordinateur peut être synchronisé avec n’importe quel contrôleur de domaine ou le contrôleur de domaine principal de son propre domaine, ou un contrôleur de domaine ou contrôleur de domaine principal dans le domaine parent.

Selon les candidats disponibles, un système de score est utilisé pour trouver la meilleure source de temps.  Ce système prend en compte la fiabilité de la source de temps et son emplacement relatif.  Cela se produit une fois lorsque l’heure service est démarré.  Si vous devez disposer d’un contrôle plus précis de la synchronisation de temps, vous pouvez ajouter des serveurs de temps dans des emplacements spécifiques ou ajouter la redondance.  Consultez le [spécifier un Local fiable temps Service à l’aide de GTIMESERV](#GTIMESERV) section pour plus d’informations.

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Environnements mixtes du système d’exploitation (Win2012R2 et Win2008R2)
Pendant un environnement de domaine Windows Server 2016 est requis pour le niveau de précision, il existe toujours avantages dans un environnement mixte.  Déploiement de Windows Server 2016 Hyper-V dans un domaine Windows 2012 bénéficieront les invités en raison des améliorations que nous l’avons mentionné ci-dessus, mais uniquement si les invités sont également Windows Server 2016.  Un contrôleur principal de Windows Server 2016, sera en mesure de fournir des temps plus précise les algorithmes améliorés, il sera en raison d’une source plus stable.  Que le remplacement de votre contrôleur de domaine principal est peut-être pas une option, vous pouvez ajouter à la place d’un contrôleur de domaine Windows Server 2016 avec la [GTIMESERV](#GTIMESERV) cumulatif qui serait une mise à niveau de précision pour votre domaine.  Un contrôleur de domaine Windows Server 2016 peut fournir le moment idéal pour les clients de temps en aval, toutefois, il est uniquement aussi bien que son temps NTP de source.

Comme indiqué ci-dessus, les fréquences d’interrogation et actualiser d’horloge ont été modifiés avec Windows Server 2016.  Elles peuvent être modifiées manuellement pour vos contrôleurs de domaine de niveau inférieur ou appliquées via une stratégie de groupe.  Alors que nous n’avons pas testé ces configurations, ils doivent se comportent correctement dans Win2008R2 et Win2012R2 et certains avantages.

Versions avant Windows Server 2016 avait un plusieurs problèmes maintenant l’heure exacte en conservant qui a entraîné l’heure système dérive immédiatement après qu’un ajustement a été effectué.  Pour cette raison, obtenir des échantillons de temps à partir d’une source NTP précise fréquemment l’horloge local avec les données de conditionnement génère plus petits écarts dans leurs horloges système dans la période intra-d’échantillonnage, aboutissant à un meilleur temps de conservation sur les versions de système d’exploitation de bas niveau. La meilleure précision observée a été environ 5 ms lorsqu’un Client Windows Server 2012 R2 NTP, configuré avec les paramètres de grande précision, synchronisé l’heure à partir d’un serveur Windows 2016 NTP précis.

Dans certains scénarios impliquant des contrôleurs de domaine invité, exemples TimeSync Hyper-V peuvent perturber synchronisation du domaine.  Cela ne doit plus être un problème pour les invités Server 2016 en cours d’exécution sur des ordinateurs hôtes Server 2016 Hyper-V.

Pour désactiver le service Hyper-V TimeSync de fournir des exemples de w32time, définissez la clé de Registre invité suivantes:

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>Ce qui permet de Linux utiliser l’heure de l’hôte Hyper-V
Pour les invités Linux en cours d’exécution dans Hyper-V, les clients sont généralement configurés pour utiliser le démon NTP pour la synchronisation de temps sur les serveurs NTP.  Si la distribution de Linux prend en charge le protocole de version 4 TimeSync et l’invité Linux a activé le service d’intégration TimeSync, il se synchronisera par rapport à l’heure de l’hôte. Cela pourrait entraîner incohérent perdre du temps si les deux méthodes sont activées.

Pour synchroniser exclusivement par rapport à l’heure de l’hôte, il est recommandé pour désactiver la synchronisation de temps NTP à l’aide:

- La désactivation de tous les serveurs dans le fichier ntp.conf NTP
- ou en désactivant le démon NTP

Dans cette configuration, le paramètre de serveur de temps est cet ordinateur hôte.  Sa fréquence d’interrogation est de 5 secondes et la fréquence d’horloge mise à jour est également de 5 secondes.

Pour synchroniser exclusivement sur NTP, il est recommandé de désactiver le service d’intégration TimeSync dans l’invité.

> [!NOTE]
> Remarque: La prise en charge pour une heure précise avec les invités Linux nécessite une fonctionnalité qui est uniquement pris en charge dans les dernières noyaux Linux en amont et il n’est pas un élément qui est largement disponible sur toutes les versions de Linux encore. Veuillez vous reporter à [virtuels pris en charge de Linux et FreeBSD pour Hyper-V sur Windows](https://technet.microsoft.com/en-us/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows) pour plus d’informations sur les distributions de prise en charge.

#### <a name="GTIMESERV"></a>Spécifiez un Service de temps fiable Local à l’aide de GTIMESERV
Vous pouvez spécifier un ou plusieurs contrôleurs de domaine en tant que les horloges source précises à l’aide de la GTIMESERV, bon serveur de temps, les indicateurs.  Par exemple, les contrôleurs de domaine spécifiques équipés de matériel GPS peuvent être marquées comme un GTIMESERV.  Cela permet de garantir qu'une horloge en fonction du matériel GPS fait référence à votre domaine.

> [!NOTE]
> Vous trouverez plus d’informations sur les indicateurs de domaine dans le [documentation du protocole MS-ADTS](https://msdn.microsoft.com/library/mt226583.aspx).

TIMESERV est un autre connexes domaine Services indicateur qui indique si un ordinateur est actuellement faisant autorité, qui peut changer si un contrôleur de domaine perd la connexion.  Un contrôleur de domaine dans cet état retourne «Niveau inconnu» lorsqu’il est interrogé via NTP.  Après plusieurs tentatives, le contrôleur de domaine consigne 36 d’événements système événements-le Service de temps.

Si vous souhaitez configurer un contrôleur de domaine en tant qu’un GTIMESERV, cela peut être configuré manuellement à l’aide de la commande suivante.  Dans ce cas le contrôleur de domaine utilise un autre ordinateur que l’horloge maître.  Il peut s’agir d’un dispositif ou un ordinateur dédié.

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> Pour plus d’informations, voir [configurer le Service de temps Windows](https://technet.microsoft.com/library/cc731191.aspx)

Si le contrôleur de domaine a installé le matériel GPS, vous devez utiliser ces étapes pour désactiver le client NTP et activer le serveur NTP.

Démarrer en désactivant le Client NTP et activer le serveur NTP à l’aide de ces modifications de clé de Registre.

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

Ensuite, redémarrez le Service de temps Windows

    net stop w32time && net start w32time

Enfin, vous indiquez que cet ordinateur possède un à l’aide de la source de temps fiable.
   
    w32tm /config /reliable:yes /update

Pour vérifier que les modifications ont été correctement effectuées, vous pouvez exécuter les commandes suivantes qui affectent les résultats ci-dessous. 

    w32tm /query /configuration

Valeur|Paramètre attendu|
----- | ----- |
AnnounceFlags|  5 (local)|
NtpServer   |(Local)|
DllName |C:\WINDOWS\SYSTEM32\w32time. DLL (Local)|
Activé |1 (local)|
NtpClient|  (Local)|

    w32tm /query /status /verbose

Valeur|  Paramètre attendu|
----- | ----- |
Couche|    1 (référence principal -, synchronisée par l’horloge de cases d’option)|
ReferenceId|    0x4C4F434C (nom de la source: «LOCAL»)|
Source| Horloge CMOS local|
Décalage de phase|   0.0000000s|
Rôle de serveur|    576 (Service de temps fiable)|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server2016 sur des plateformes virtuelles 3ème parties
Lorsque Windows est virtualisé, par défaut, l’hyperviseur est responsable de la fourniture de temps.  Mais les membres doivent être synchronisé avec le contrôleur de domaine dans l’ordre pour Active Directory fonctionne correctement joints au domaine.  Il est préférable de désactiver la virtualisation de n’importe quel temps entre l’invité et l’hôte de n’importe quel des plateformes virtuelles partie 3.

#### <a name="discovering-the-hierarchy"></a>Découverte de la hiérarchie
Dans la mesure où la chaîne de la hiérarchie de temps à la source de l’horloge de référence est dynamique dans un domaine, et négocié, vous devez interroger l’état d’un ordinateur particulier de comprendre qu’il est source de temps et de la chaîne de l’horloge source maître.  Cela peut aider à diagnostiquer les problèmes de synchronisation de temps.

Compte tenu de vous voulez résoudre les problèmes d’un client spécifique; la première étape consiste à comprendre sa source de temps à l’aide de cette commande w32tm.

    w32tm /query /status

Les résultats affichent la Source entre autres choses.  La Source indique avec lesquels vous synchronisez temps dans le domaine.  Il s’agit de la première étape de cette hiérarchie de temps des ordinateurs virtuels.
Utilisez ensuite entrée Source à partir du haut et le paramètre /StripChart pour trouver la source de temps suivante dans la chaîne.

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

Également utile, la commande suivante répertorie chaque contrôleur de domaine que trouver dans le domaine spécifié et affiche un résultat qui vous permet de déterminer les chaque partenaire.  Cette commande inclura les ordinateurs virtuels qui ont été configurés manuellement.
    
    w32tm /monitor /domain:my_domain

À l’aide de la liste, vous pouvez suivre les résultats dans le domaine et comprendre la hiérarchie, ainsi que le décalage à chaque étape.  En plaçant le point où le décalage s’aggrave considérablement, vous pouvez déterminer la racine de l’heure incorrect.  À partir de là, vous pouvez essayer de comprendre pourquoi cette heure est incorrecte en activant [journalisation w32tm](#W32Logging). 

#### <a name="using-group-policy"></a>À l’aide de la stratégie de groupe
Vous pouvez utiliser Stratégie de groupe pour accomplir la précision plus stricte, par exemple, attribuez des clients à utiliser des serveurs NTP spécifiques ou pour contrôler comment de bas niveau du système d’exploitation est configurés lorsque virtualisés.  
Vous trouverez ci-dessous une liste des scénarios possibles et les paramètres de stratégie de groupe appropriés:

**Virtualisés domaines** - afin de contrôler les contrôleurs de domaine virtualisés dans Windows 2012R2 afin qu’ils synchronisent l’heure avec leur domaine, plutôt que de l’hôte Hyper-V, vous pouvez désactiver cette entrée de Registre.   Pour le contrôleur de domaine principal, vous ne souhaitez pas désactiver l’entrée comme l’ordinateur hôte Hyper-V fournira la source de temps plus stable.  L’entrée de Registre nécessite que vous redémarrez le service w32time lorsqu’il est modifié.

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**Charges sensibles précision** - pour les charges de travail sensibles temps précision, vous pouvez configurer des groupes d’ordinateurs pour définir les serveurs NTP et les paramètres liés au temps, telles que la fréquence de mise à jour de l’interrogation et horloge.  Ceci est habituellement géré par le domaine, mais pour un contrôle plus, vous pouvez cibler des ordinateurs spécifiques pour pointer directement maître.

Paramètre de stratégie de groupe|   Nouvelle valeur|
----- | ----- |
NtpServer|  ClockMasterName, 0 x 8|
MinPollInterval|    6 – 64 secondes|
MaxPollInterval|    6|
UpdateInterval| 100 à une seule fois par seconde|
EventLogFlags|  3 – la journalisation tous les temps spéciale|

> [!NOTE]
> Les paramètres NtpServer et EventLogFlags sont trouvent sous System\Widows temps Service\Time fournisseurs en utilisant les paramètres de configuration Windows NTP Client.  Les autres 3 sont situés sous le Service de temps Windows\Windows en utilisant les paramètres de Configuration globale.

**À distance précision sensibles charges à distance** : pour les systèmes dans les domaines de branche pour l’instance commerciale et le crédit secteur PCI (Payment), Windows utilise les informations du site actuel et configuré de source de temps de localisateur de contrôleur de domaine pour trouver un contrôleur de domaine local, sauf s’il existe un NTP manuelle.  Cet environnement requiert 1 seconde de précision, qui utilise la convergence plus rapide sur l’heure correcte.  Cette option permet le service w32time déplacer vers l’arrière de l’horloge.  Si cela est acceptable et répond à vos besoins, vous pouvez créer la stratégie suivante.   Comme avec n’importe quel environnement, permet de garantir au test et de la ligne de base de votre réseau. 

Paramètre de stratégie de groupe|   Nouvelle valeur|
----- | ----- |
MaxAllowedPhaseOffset|  1, si plus d’une seconde, définissez une horloge à l’heure correcte.|

Le paramètre MaxAllowedPhaseOffset se trouve dans le Service de temps Windows\Windows en utilisant les paramètres de Configuration globale.

> [!NOTE]
> Pour plus d’informations sur la stratégie de groupe et des entrées connexes, consultez [outils de Service de temps Windows](windows-time-service-tools-and-settings.md) et paramètres l’article sur TechNet.

#### <a name="azure-and-windows-iaas-considerations"></a>Considérations relatives à Azure et IaaS Windows

##### <a name="azure-virtual-machine-active-directory-domain-services"></a>Machine virtuelle Azure: Services de domaine Active Directory
Si la machine virtuelle Azure en cours d’exécution des Services de domaine Active Directory fait partie d’une locale existante forêt Active Directory, puis TimeSync(VMIC), doit être désactivé. Il s’agit pour permettre à tous les contrôleurs de domaine dans la forêt, physique et virtuelle, d’utiliser une hiérarchie de synchronisation de temps unique. Reportez-vous au livre blanc meilleures pratiques [«en cours d’exécution domaine contrôleurs dans Hyper-V»](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

##### <a name="azure-virtual-machine-domain-joined-machine"></a>Machine virtuelle Azure: Ordinateur joint au domaine
Si vous hébergez un ordinateur qui est le domaine joint à une forêt Active Directory existante, virtuels ou physiques, la meilleure pratique consiste à désactiver TimeSync pour l’invité et vérifiez W32Time est configuré pour être synchronisé avec son contrôleur de domaine via la configuration de temps pour le Type = NTP5

##### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Machine virtuelle Azure: Ordinateur de groupe de travail autonome
Si la machine virtuelle Azure n’est pas joint à un domaine, et n’est pas un contrôleur de domaine, il est recommandé de conserver la configuration de la durée par défaut et l’ordinateur virtuel se synchroniser avec l’ordinateur hôte.

### <a name="windows-application-requiring-accurate-time"></a>Application de Windows nécessitant une heure précise
#### <a name="time-stamp-api"></a>Horodatage API
Les programmes qui requièrent la plus grande précision en ce qui concerne l’heure UTC et pas le passage du temps, utilisez le [GetSystemTimePreciseAsFileTime API](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx).  Cela garantit que votre application obtienne l’heure système, qui est exposé par le service de temps Windows.

#### <a name="udp-performance"></a>Performances UDP
Si vous avez une application qu’utilise UDP communication pour les transactions et il l’important de réduire la latence, il existe certaines liées entrées de Registre que vous pouvez utiliser pour configurer une plage de ports à exclure de la base de moteur de filtrage de port.  Cela améliorera le temps de latence et augmenter le débit.  Toutefois, les modifications apportées au Registre doivent être limitées aux administrateurs expérimentés.  En outre, cette solution de contournement exclut ports sécurisés par le pare-feu.  Consultez la référence de l’article ci-dessous pour plus d’informations.

Pour Windows Server 2012 et Windows Server 2008, vous devez d’abord installer un correctif.  Vous pouvez faire référence à cette base de connaissances: [perte du datagramme lorsque vous exécutez une application de récepteur de multidiffusion dans Windows 8 et Windows Server 2012](https://support.microsoft.com/en-us/kb/2808584)

#### <a name="update-network-drivers"></a>Mettre à jour les pilotes réseau
Certains fournisseurs de réseau disposent des mises à jour de pilote et améliorer les performances en ce qui concerne la latence de pilote et de la mise en mémoire tampon des paquets UDP.  Contactez votre fournisseur de réseau pour voir s’il existe des mises à jour pour aider le débit UDP.

### <a name="logging-for-auditing-purposes"></a>Connexion à des fins d’audit
Pour être conformes aux réglementations de suivi de temps, vous pouvez manuellement archiver w32tm journaux, les journaux des événements et les informations d’analyse des performances.  Plus tard, les informations archivées peuvent être utilisées pour attester de la conformité à un moment donné dans le passé.  Les facteurs suivants sont utilisés pour indiquer la précision.


1. Précision de l’horloge à l’aide du compteur de moniteur de performance de décalage temporel calculée.  Affiche l’horloge avec dans la précision souhaitée.
2.  Source d’horloge recherchez «Homologue réponse» dans les journaux w32tm.   Le texte du message est l’adresse IP ou le VMIC, qui décrit la source de temps et de la prochaine dans la chaîne d’horloge de référence pour valider.
3.  État de condition à l’aide des journaux w32tm pour valider que d’horloge «ClockDispl Discipline: \*SKEW\*TIME\*» se produisent.  Cela indique que w32tm est active au moment.

#### <a name="event-logging"></a>Journalisation des événements
Pour obtenir l’histoire, vous devez également les informations du journal des événements.  En collectant le journal des événements système et le filtrage sur le serveur de synchronisation, Microsoft-Windows-Kernel-Boot, Microsoft-Windows-Kernel-général, vous ne pourrez pas détecter s’il existe d’autres influences qui ont été modifiés le temps, par exemple, des tiers.  Ces journaux peuvent être nécessaires pour écarter les interférences externe.  Stratégie de groupe peut affecter les journaux des événements sont écrits dans le journal.  Pour plus d’informations, consultez la section ci-dessus à l’aide de stratégies de groupe.

#### <a name="W32Logging"></a>La journalisation du débogage W32Time
Pour activer w32tm à des fins d’audit, la commande suivante active la journalisation qui affiche les mises à jour périodiques de l’horloge et indique l’horloge de la source.  Redémarrez le service pour activer la journalisation de nouveau.  

Pour plus d’informations, voir [comment activer le journal de débogage pour le Service de temps Windows](https://support.microsoft.com/en-us/kb/816043).

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

#### <a name="performance-monitor"></a>Analyse des performances
Le service de temps Windows de Windows Server 2016 expose les compteurs de performance qui peuvent être utilisés pour collecter la journalisation d’audit.  Elles peuvent être enregistrés localement ou à distance.  Vous pouvez enregistrer la valeur de décalage temporel ordinateur et des compteurs de délai aller-retour.  
Et comme tout compteur de performance, vous pouvez les surveiller de manière à distance et créer des alertes à l’aide de System Center Operations Manager.  Vous pouvez, par exemple, utiliser une alerte vous alarme lorsque la valeur de décalage temporel diffère de la précision souhaitée.  Le [System Center Management Pack](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx) contient des informations supplémentaires.

#### <a name="windows-traceability-example"></a>Exemple de traçabilité Windows
À partir des journaux w32tm, vous devez valider deux types d’informations.  La première est une indication que le fichier journal est actuellement horloge de condition.  Cela prouver que l’horloge a été en cours conditionnée par le Service de temps Windows à l’heure contesté.

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

L’essentiel est que vous voyez le préfixe ClockDispln Discipline qui est w32time preuve de messages interagit avec l’horloge système.
 
Ensuite, vous devez trouver le dernier rapport dans le journal avant l’heure contesté qui signale l’ordinateur source qui est actuellement utilisé en tant que l’horloge de référence.  Cela peut être une adresse IP, nom de l’ordinateur ou le fournisseur VMIC, ce qui indique qu’il est la synchronisation avec l’ordinateur hôte pour Hyper-V.  L’exemple suivant fournit une adresse IPv4 de 10.197.216.105.

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

Maintenant que vous avez validé le premier système dans la chaîne d’heure de référence, vous devez examiner le fichier journal sur la source de temps de référence et répétez les mêmes étapes.  Ce processus continue jusqu'à ce que vous atteigniez une horloge physique, comme le GPS ou d’une source de temps comme NIST.  Si l’horloge de référence est matériel GPS, journaux à partir de la fabrication peuvent également être nécessaires.

### <a name="network-considerations"></a>Considérations relatives au réseau
Les algorithmes de protocole NTP ont une dépendance sur la symétrie de votre réseau.  En tant que votre boîte de dialogue augmenter le nombre de tronçons réseau, la probabilité d’asymétrie augmente.  Il, il est difficile de prédire les types de précisions s’affiche dans votre environnement spécifique. 

Analyseur de performances et de nouveaux compteurs de temps Windows dans Windows Server 2016 peuvent être utilisés pour évaluer la précision de votre environnement et créer des lignes de base de. En outre, vous pouvez effectuer la résolution des problèmes pour déterminer le décalage en cours de n’importe quel ordinateur sur votre réseau.

Il existe deux normes générales pour l’heure précise sur le réseau.  PTP ([précision Time Protocol - 1588 IEEE](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) a des exigences plus strictes sur l’infrastructure réseau, mais peut fournir souvent de précision microseconde.  NTP ([Network Time Protocol – RFC 1305](https://tools.ietf.org/html/rfc1305)) fonctionne sur une plus grande variété de réseaux et d’environnements, ce qui rend plus facile à gérer. 

Windows prend en charge Simple NTP (RFC2030) par défaut pour les ordinateurs joints au domaine.  Pour les ordinateurs appartenant à un domaine, nous utilisons un NTP sécurisé appelé [MS-SNTP](https://msdn.microsoft.com/en-us/library/cc246877.aspx), qui tire parti des secrets domaine négocié qui offrent un avantage de gestion sur NTP authentifié décrit dans RFC1305 et RFC5905.   

Le domaine et les protocoles joint à un domaine nécessite le port UDP 123.  Pour plus d’informations sur les meilleures pratiques NTP, reportez-vous à [réseau temps protocole meilleures brouillon en cours pratiques IETF](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00).

### <a name="reliable-hardware-clock-rtc"></a>Matériel fiable horloge (RTC)
Windows ne prend pas durée de l’étape, sauf si certaines limites sont dépassés, mais plutôt disciplines l’horloge.  Cela signifie que w32tm ajuste la fréquence de l’horloge à intervalles réguliers, à l’aide de la fréquence d’horloge mise à jour de définition, les valeurs par défaut pour une fois par seconde avec Windows Server 2016.  Si l’horloge se trouve derrière, il accélère la fréquence et s’il est maintenant, il ralentit la fréquence.  Toutefois, pendant cette période entre les ajustements de fréquence d’horloge, l’horloge de matériel est dans un contrôle.  S’il existe un problème avec le microprogramme ou de l’horloge de matériel, l’heure sur l’ordinateur peut devenir moins précise.

Il s’agit d’une autre raison, que vous devez tester et la ligne de base dans votre environnement.  Si le compteur de performance «Calculée de décalage temporel» ne pas stabiliser à la précision que vous ciblez, vous devrez vérifier que votre microprogramme est à jour.  En tant qu’un autre test, vous pouvez voir si le matériel en double reproduire le problème.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>Résolution des problèmes de précision du temps et NTP
Vous pouvez utiliser la découverte la section hiérarchie ci-dessus pour comprendre la source de l’heure inexacts.  En examinant le décalage, trouver le point dans la hiérarchie où temps diverge le meilleur parti de sa Source NTP.  Une fois que vous comprenez la hiérarchie, vous souhaiterez comprendre pourquoi cette source de temps spécifique ne reçoit pas heure précise.  

Mettre l’accent sur le système avec le temps divergente, vous pouvez utiliser ces outils ci-dessous pour recueillir des informations plus pour vous aider à déterminer le problème et pour trouver une solution.  La référence UpstreamClockSource ci-dessous, est l’horloge détecté à l’aide de «w32tm /config /status».


- Journaux des événements système
- Activer à l’aide de la journalisation: journaux w32tm - w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-300
- clé de Registre w32Time HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- Traces de réseau local
- Compteurs de performance (à partir de l’ordinateur local ou le UpstreamClockSource)
- W32tm /stripchart /computer:UpstreamClockSource
- UpstreamClockSource PING pour comprendre la latence et le nombre de sauts pour la Source
- tracert UpstreamClockSource

Problème|    Symptômes|   Résolution|
----- | ----- | ----- |
Horloge TSC locale n’est pas stable.| À l’aide de la synchronisation de l’Analyseur de performances - ordinateur physique – horloge horloge stable, mais que vous toujours voir toutes les minutes de plusieurs 100us 1 et 2. |   Mettre à jour de microprogramme ou valider différents matériels n’affiche pas le même problème.|
Latence du réseau|    w32tm stripchart affiche un RoundTripDelay de plus de 10 ms.  Variante dans le délai de provoquer un bruit de la taille ½ de la durée des boucles, par exemple un délai dans une seule direction.</br></br>UpstreamClockSource est sauts multiples, comme indiqué par la commande PING.  Durée de vie devrait être proche de 128.</br></br>Tracert permet de rechercher la latence à chaque tronçon.    | Rechercher une source plus proche de l’horloge pour le moment.  Une solution consiste à installer une horloge source sur le même segment ou pointer manuellement vers l’horloge source qui est le plus proche géographiquement.  Pour un scénario de domaine, ajoutez un ordinateur avec le rôle GTimeServ. |  
Impossible d’atteindre fiable la source NTP|    W32tm /stripchart retourne par intermittence «Demande dépassé»    |Source NTP n’est pas réactive|
Source NTP n’est pas réactive|    Vérifiez les compteurs Perfmon pour les réponses sortant du serveur NTP NTP Client Source Count, demandes entrantes du serveur NTP et votre utilisation par rapport à vos lignes de base de.|    À l’aide des compteurs de performance serveur, déterminez si le chargement a changé dans le cas de vos lignes de base de.</br></br>Existe-t-il des réseau les problèmes de congestion?|
Contrôleur de domaine ne pas à l’aide de l’horloge plus précise|    Modifications de la topologie ou une horloge maître récemment ajouté.|   w32tm /resync /rediscover|
Horloges du client sont dérive| Le Service de temps événement 36 dans le journal des événements système et/ou de texte dans le fichier journal qui décrivant: compteur «Nombre Source temps NTP Client» allant de 1 sur 0|Résoudre les problèmes de la source en amont et de comprendre si elle est en cours d’exécution sur les problèmes de performances.|

### <a name="baselining-time"></a>Heure de la ligne de base
Ligne de base est importante afin que vous pouvez tout d’abord, comprendre les performances et la précision de votre réseau et comparer la ligne de base à l’avenir lorsque des problèmes surviennent.  Vous souhaiterez ligne de base de la racine du contrôleur de domaine principal ou tous les ordinateurs marqués avec le GTIMESRV.  Il vous est également préférable de ligne de base du contrôleur de domaine principal dans chaque forêt.  Enfin choisir des contrôleurs de domaine critiques ou d’ordinateurs qui ont des caractéristiques intéressantes, comme la distance ou de charges élevées et de ligne de base de ces.

Il est également utile de ligne de base Windows Server 2016 Visual Studio 2012 R2, toutefois, vous avez uniquement w32tm /stripchart comme un outil que vous pouvez utiliser pour comparer, dans la mesure où Windows Windows Server 2012 R2 ne dispose pas des compteurs de performances.  Vous devez sélectionner deux ordinateurs virtuels avec les mêmes caractéristiques, ou mettre à niveau un ordinateur et comparez les résultats après la mise à jour.  L’addendum de mesures de temps Windows a plus d’informations sur comment effectuer des mesures détaillées entre 2016 et 2012.

À l’aide des tous les w32time compteurs de performance, de collecter des données pour au moins une semaine.  Vous disposez de suffisamment d’une référence pour prendre en compte pour différents dans le réseau au fil du temps et suffisamment d’une exécution à assurent que la précision du temps est stable resteront.

### <a name="ntp-server-redundancy"></a>Redondance de serveur NTP
Pour la configuration du serveur NTP manuelle utilisée avec les ordinateurs joints au domaine ou le contrôleur de domaine principal, plus d’un serveur est une mesure la bonne redondance en cas de disponibilité.  Il peut également donner une meilleure précision, en supposant que toutes les sources sont précis et stables.  Toutefois, si la topologie n’est pas bien conçue, ou les sources de temps ne sont pas stables, la précision résultante peut se révéler plus perturbantes donc avec prudence.  La limite de temps pris en charge les serveurs w32time peut référencer manuellement est 10. 

## <a name="leap-seconds"></a>LEAP secondes
Période de rotation de la terre varie au fil du temps, provoquée par des événements climatiques et geological. En règle générale, la variante est environ une seconde après quelques années. Chaque fois que la variante de temps atomique devient trop volumineuse, une correction d’une seconde (le haut ou vers le bas) est inséré, appelé une seconde supplémentaire. Pour cela, de telle sorte que la différence ne dépasse jamais 0,9 secondes. Cette correction est annoncée six mois avant la correction réelle. Avant Windows Server 2016, le Service de temps Microsoft n’a pas connaissance des leap secondes, mais reposaient sur le service de temps externe à prendre en charge de ce. Avec la précision du temps accrue de Windows Server 2016, Microsoft travaille sur une solution plus appropriée pour le problème de la seconde supplémentaire.

## <a name="secure-time-seeding"></a>Sécuriser l’amorçage de l’heure
W32Time dans Server 2016 inclut la fonctionnalité d’amorçage de temps sécurisé. Cette fonctionnalité détermine le temps approximatif actuel de connexions sortantes de SSL.  Cette valeur de temps est utilisée pour surveiller l’horloge système local et corrigez les erreurs brutes. Vous pouvez en savoir plus sur la fonctionnalité dans [ce billet de blog](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/).  Dans les déploiements avec une source de temps fiable et bien analysé les ordinateurs qui comprennent la surveillance des décalages de temps, vous pouvez choisir de ne pas utiliser la fonctionnalité de sécuriser l’amorçage de temps et s’appuient sur votre infrastructure existante à la place. 

Vous pouvez désactiver la fonctionnalité avec les étapes suivantes:

1.  Définissez la valeur de configuration du Registre UtilizeSSLTimeData sur 0 sur un ordinateur spécifique:

    reg ajouter HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f


2.  Si vous ne parvenez pas à redémarrer l’ordinateur immédiatement en raison d’une raison quelconque, vous pouvez notifier le service W32time sur la mise à jour de configuration. Cela arrête la surveillance en temps et selon les données au moment de l’application collectées à partir de connexions SSL. 

    W32tm.exe/config /update

3.  Le redémarrage de l’ordinateur rend le paramètre effet immédiatement et entraîne également qu’il arrête de collecter des données de temps à partir de connexions SSL.  La dernière partie a une surcharge de très petite et ne doit pas être un problème de performances.

4.  Pour appliquer ce paramètre dans un domaine entier, veuillez la valeur UtilizeSSLTimeData dans le paramètre de stratégie de groupe W32time à 0 et le paramètre de publication.  Lorsque le paramètre est sélectionné par un Client de stratégie de groupe, service W32time est informé du problème et arrête l’application à l’aide de données au moment du protocole SSL et surveillance en temps. La collecte de données de temps SSL s’arrête lorsque chaque ordinateur redémarre. Si votre domaine possède des ordinateurs portables d’ULTRAFINS portables/tablettes et autres périphériques, vous voudrez peut-être exclure ces ordinateurs à partir de cette modification de la stratégie. Ces appareils seront finalement sont confrontés à la batterie et devez l’amorçage de temps sécurisé de fonctionnalité pour amorcer leur temps.














 













 




