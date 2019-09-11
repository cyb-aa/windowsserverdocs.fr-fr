---
title: Améliorations de Windows Server 2016
description: Windows Server 2016 a amélioré les algorithmes qu’il utilise pour corriger l’heure et la condition de synchronisation de l’horloge locale avec le temps universel coordonné (UTC).
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 2b8c6148af21e94e4a56661402f36dcb2e636461
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871835"
---
## <a name="windows-server-2016-improvements"></a>Améliorations de Windows Server 2016

### <a name="windows-time-service-and-ntp"></a>Service de temps Windows et NTP
Windows Server 2016 a amélioré les algorithmes qu’il utilise pour corriger l’heure et la condition de synchronisation de l’horloge locale avec le temps universel coordonné (UTC).  NTP utilise 4 valeurs pour calculer le décalage horaire, en fonction des horodateurs de la demande/réponse du client et de la demande/réponse du serveur.  Toutefois, les réseaux sont bruyants et il peut y avoir des pics de données à partir de NTP en raison de la congestion du réseau et d’autres facteurs qui affectent la latence du réseau.  Les algorithmes Windows 2016 dépassent ce bruit à l’aide d’un certain nombre de techniques différentes, ce qui aboutit à une horloge stable et précise.  En outre, la source utilisée pour le temps précis fait référence à une API améliorée qui nous offre une meilleure résolution.  Grâce à ces améliorations, nous sommes en mesure d’atteindre une précision de 1 ms en ce qui concerne l’heure UTC dans un domaine.

### <a name="hyper-v"></a>Hyper-V
Windows 2016 a amélioré le service TimeSync Hyper-V. Les améliorations incluent une heure initiale plus précise sur le démarrage de la machine virtuelle ou la restauration de la machine virtuelle et la correction de la latence d’interruption pour les exemples fournis à w32time.  Cette amélioration nous permet de rester à l' 10μs de l’hôte avec un RMS, (moyenne quadratique, qui indique la variance), de 50 μs, même sur un ordinateur avec une charge de 75%. Pour plus d’informations, consultez [architecture Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx).

> [!NOTE]
> Le chargement a été créé à l’aide de Prime95 Benchmark à l’aide d’un profil équilibré.

En outre, le niveau de couche que l’hôte signale à l’invité est plus transparent.  Auparavant, l’hôte présenterait un Stratum fixe de 2, quelle que soit sa précision.  Avec les modifications apportées à Windows Server 2016, l’hôte signale une couche supérieure à la strate de l’hôte, ce qui produit un meilleur temps pour les invités virtuels.  Le niveau de la couche hôte est déterminé par le mode de fonctionnement normal, en fonction de son heure source.  Les invités Windows 2016 joints à un domaine trouveront l’horloge la plus précise, plutôt que d’utiliser par défaut l’ordinateur hôte.  C’est pour cette raison que nous recommandons de désactiver manuellement le paramètre de fournisseur de temps Hyper-V pour les ordinateurs participant à un domaine dans Windows 2012 R2 et les versions antérieures.

### <a name="monitoring"></a>Surveillance
Des compteurs de l’analyseur de performances ont été ajoutés.  Elles vous permettent d’effectuer une ligne de base, de surveiller et de résoudre les problèmes de temps.  Ces compteurs sont les suivants :

Compteur|Description|
----- | ----- |
Décalage de temps calculé|   Décalage de temps absolu entre l’horloge système et la source de temps choisie, comme calculé par le service W32Time en microsecondes. Lorsqu’un nouvel exemple valide est disponible, le temps calculé est mis à jour avec le décalage horaire indiqué par l’exemple. Il s’agit du décalage de temps réel de l’horloge locale. W32time lance la correction de l’horloge à l’aide de ce décalage et met à jour le temps calculé entre les échantillons avec le décalage de temps restant qui doit être appliqué à l’horloge locale. La précision de l’horloge peut être suivie à l’aide de ce compteur de performances avec un intervalle d’interrogation faible (par exemple, 256 secondes ou moins) et en recherchant que la valeur du compteur est inférieure à la limite de précision de l’horloge souhaitée.|
Réglage de la fréquence d’horloge| Réglage de fréquence d’horloge absolu effectué sur l’horloge système locale de W32Time en parties par milliard. Ce compteur permet de visualiser les actions effectuées par w32time.|
Délai d’aller-retour NTP|    Délai d’aller-retour le plus récent rencontré par le client NTP pour la réception d’une réponse du serveur en microsecondes. Il s’agit du temps écoulé sur le client NTP entre la transmission d’une demande au serveur NTP et la réception d’une réponse valide du serveur. Ce compteur permet de caractériser les retards rencontrés par le client NTP. Les allers-retours plus grands ou variables peuvent ajouter du bruit à des calculs de temps NTP, qui peuvent, à leur tour, affecter la précision de la synchronisation de l’heure via NTP.|
Nombre de sources du client NTP|    Nombre actif de sources de temps NTP utilisées par le client NTP. Il s’agit du nombre d’adresses IP distinctes actives des serveurs de temps qui répondent aux demandes de ce client. Ce nombre peut être supérieur ou inférieur aux homologues configurés, en fonction de la résolution DNS des noms d’homologues et de la capacité de la portée actuelle.|
Demandes entrantes du serveur NTP|   Nombre de demandes reçues par le serveur NTP (demandes/s).|
Réponses sortantes du serveur NTP|  Nombre de demandes traitées par le serveur NTP (réponses/s).|

Les 3 premiers compteurs ciblent des scénarios de résolution des problèmes de précision.  La section « précision du temps et NTP » ci-dessous, sous [meilleures pratiques](#BestPractices), offre plus de détails.
Les 3 derniers compteurs couvrent les scénarios de serveur NTP et sont utiles lors de la détermination de la charge et de l’établissement de la ligne de performance actuelle.

### <a name="configuration-updates-per-environment"></a>Mises à jour de la configuration par environnement
Les éléments suivants décrivent les modifications apportées à la configuration par défaut entre Windows 2016 et les versions précédentes pour chaque rôle.  Les paramètres de la mise à jour anniversaire de Windows Server 2016 et Windows 10 (Build 14393) sont désormais uniques, ce qui explique pourquoi il s’agit de colonnes distinctes. 

|Rôle|Paramètre|Windows Server 2016|Windows 10|Windows Server 2012 R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**Autonome/nano Server**||||
| |*Serveur de temps*|time.windows.com|N/D|time.windows.com|
| |*Fréquence d’interrogation*|64-1024 secondes|N/D|Une fois par semaine|
| |*Fréquence de mise à jour de l’horloge*|Une fois par seconde|N/D|Une fois par heure|
|**Client autonome**||||
| |*Serveur de temps*|N/D|time.windows.com|time.windows.com|
| |*Fréquence d’interrogation*|N/D|Une fois par jour|Une fois par semaine|
| |*Fréquence de mise à jour de l’horloge*|N/D|Une fois par jour|Une fois par semaine|
|**Contrôleur de domaine**||||
| |*Serveur de temps*|CONTRÔLEUR DE DOMAINE PRINCIPAL/GTIMESERV|N/D|CONTRÔLEUR DE DOMAINE PRINCIPAL/GTIMESERV|
| |*Fréquence d’interrogation*|64-1024 secondes|N/D|1024-32768 secondes|
| |*Fréquence de mise à jour de l’horloge*|Une fois par jour|N/D|Une fois par semaine|
|**Serveur membre du domaine**||||
| |*Serveur de temps*|DC|N/D|DC|
| |*Fréquence d’interrogation*|64-1024 secondes|N/D|1024-32768 secondes|
| |*Fréquence de mise à jour de l’horloge*|Une fois par seconde|N/D|Une fois toutes les 5 minutes|
|**Client membre du domaine**||||
| |*Serveur de temps*|N/D|DC|DC|
| |*Fréquence d’interrogation*|N/D|1204-32768 secondes|1024-32768 secondes|
| |*Fréquence de mise à jour de l’horloge*|N/D|Une fois toutes les 5 minutes|Une fois toutes les 5 minutes|
|**Invité Hyper-V**||||
| |*Serveur de temps*|Choisit la meilleure option en fonction de la couche d’hôte et du serveur de temps|Choisit la meilleure option en fonction de la couche d’hôte et du serveur de temps|Par défaut, hôte|
| |*Fréquence d’interrogation*|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|
| |*Fréquence de mise à jour de l’horloge*|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|

>[!NOTE]
>Pour Linux dans Hyper-V, consultez la section [autoriser Linux à utiliser l’heure de l’hôte Hyper-v](#AllowingLinux) ci-dessous.

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impact de l’interrogation accrue et de la fréquence de mise à jour de l’horloge
Pour fournir une durée plus précise, les valeurs par défaut pour les fréquences d’interrogation et les mises à jour de l’horloge sont augmentées, ce qui nous permet d’effectuer des ajustements réduits plus fréquemment.  Cela entraînera davantage de trafic UDP/NTP. Toutefois, ces paquets sont peu volumineux, de sorte qu’il ne doit y avoir que peu ou pas d’impact sur les liaisons haut débit. L’avantage, toutefois, est que le temps doit être mieux adapté à une large gamme de matériel et d’environnements.

Pour les appareils avec batterie, l’amélioration de la fréquence d’interrogation peut entraîner des problèmes.  Les unités de batterie ne stockent pas l’heure lorsqu’elles sont désactivées.  Lorsqu’ils reprennent, ils peuvent nécessiter des corrections fréquentes de l’horloge.  L’amélioration de la fréquence d’interrogation entraîne une instabilité de l’horloge et peut également utiliser davantage de puissance.  Microsoft vous recommande de ne pas modifier les paramètres par défaut du client.

Les contrôleurs de domaine doivent avoir un impact minimal, même avec l’effet multiplié par les mises à jour accrues des clients NTP dans un domaine Active Directory.  La consommation de ressources de NTP est plus faible par rapport à d’autres protocoles et un impact marginal.  Vous êtes plus susceptible d’atteindre des limites pour d’autres fonctionnalités de domaine avant d’être affecté par les paramètres accrus pour Windows Server 2016.  Active Directory utilise le protocole NTP sécurisé, qui a tendance à synchroniser le temps moins précisément qu’un NTP simple, mais nous avons vérifié qu’il sera mis à l’échelle jusqu’à deux couches du contrôleur de domaine principal.

En guise de plan conservateur, vous devez réserver 100 demandes NTP par seconde par cœur.  Par exemple, un domaine composé de 4 contrôleurs de domaine avec 4 cœurs chacun, vous devez être en mesure de traiter 1600 demandes NTP par seconde.  Si vous avez 10 000 clients configurés pour synchroniser l’heure une fois toutes les 64 secondes et que les demandes sont reçues uniformément au fil du temps, vous verrez 10 000/64 ou environ 160 requêtes/seconde, réparties sur tous les contrôleurs de domaines.  Cela se fait facilement au sein de nos 1600 demandes NTP/s sur la base de cet exemple.  Ce sont des recommandations en matière de planification prudente et, bien sûr, vous avez une dépendance importante sur votre réseau, les vitesses et les charges du processeur, afin de toujours avoir une ligne de base et des tests dans vos environnements.

Il est également important de noter que si vos contrôleurs de domaine s’exécutent avec une charge considérable de l’UC, supérieure à 40%, cela ajoutera presque certainement du bruit aux réponses NTP et affectera la précision de votre temps dans votre domaine.  Là encore, vous devez tester dans votre environnement pour comprendre les résultats réels.

## <a name="time-accuracy-measurements"></a>Mesures de précision du temps
### <a name="methodology"></a>Méthodologie
Pour mesurer la précision du temps pour Windows Server 2016, nous avons utilisé une variété d’outils, de méthodes et d’environnements.  Vous pouvez utiliser ces techniques pour mesurer et ajuster votre environnement et déterminer si les résultats de précision répondent à vos besoins. 

Notre horloge source de domaine est composée de deux serveurs NTP à haute précision avec un matériel GPS.  Nous avons également utilisé un ordinateur de test de référence distinct pour les mesures, qui possédait également un matériel GPS de haute précision installé à partir d’un autre fabricant.  Pour certains tests, vous aurez besoin d’une source d’horloge précise et fiable à utiliser comme référence en plus de la source de l’horloge de votre domaine.

Nous avons utilisé quatre méthodes différentes pour mesurer la précision avec les machines physiques et virtuelles. Plusieurs méthodes fournies indépendantes signifient que les résultats sont validés.


1. Mesurez l’horloge locale, qui est conditionnée par w32tm, sur notre machine de test de référence qui dispose d’un matériel GPS distinct.  
2.  Mesurez les pings NTP entre le serveur NTP et les clients à l’aide de w32tm « stripchart »
3.  Mesurez les pings NTP entre le client et le serveur NTP à l’aide de w32tm « stripchart »
4.  Mesurez les résultats Hyper-V de l’hôte à l’invité à l’aide du compteur d’horodatage (TSC).  Ce compteur est partagé entre les partitions et l’heure système dans les deux partitions.  Nous avons calculé la différence entre l’heure de l’hôte et l’heure du client sur la machine virtuelle.  Ensuite, nous utilisons l’horloge TSC pour interpoler l’heure de l’hôte à partir de l’invité, car les mesures ne se produisent pas en même temps.  En outre, nous utilisons les retards et la latence du facteur d’horloge TSV dans l’API.

W32tm est intégré, mais les autres outils que nous avons utilisés pendant nos tests sont disponibles pour le référentiel Microsoft sur GitHub en tant que Open source pour vos tests et votre utilisation.  Le WIKI sur le référentiel contient des informations supplémentaires qui décrivent comment utiliser les outils pour effectuer des mesures.

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

Les résultats des tests indiqués ci-dessous sont un sous-ensemble de mesures que nous avons créé dans l’un des environnements de test.  Ils illustrent la précision conservée au début de la hiérarchie de temps et du client de domaine enfant à la fin de la hiérarchie de temps.  Cela est comparé aux mêmes ordinateurs dans une topologie basée sur 2012 pour la comparaison.

### <a name="topology"></a>Topologie
À des fins de comparaison, nous avons testé une topologie basée sur Windows Server 2012 R2 et Windows Server 2016.  Les deux topologies sont constituées de deux ordinateurs hôtes Hyper-V physiques qui font référence à une machine Windows Server 2016 avec le matériel d’horloge GPS installé.  Chaque hôte exécute 3 invités Windows joints à un domaine, qui sont organisés selon la topologie suivante.  Les lignes représentent la hiérarchie de temps et le protocole/transport utilisé.

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Vue d’ensemble des résultats graphiques
Les deux graphiques suivants représentent la précision du temps pour deux membres spécifiques dans un domaine en fonction de la topologie ci-dessus.  Chaque graphique affiche à la fois les résultats de Windows Server 2012 R2 et 2016, qui illustrent les améliorations visuelles.  La précision était mesurée à partir de avec-dans l’ordinateur invité par rapport à l’hôte.  Les données graphiques représentent un sous-ensemble de l’ensemble des tests que nous avons effectués et montre les scénarios les plus optimistes et les pires cas.  

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Performances du contrôleur de domaine principal du domaine racine
Le contrôleur de domaine principal racine est synchronisé avec l’hôte Hyper-V (à l’aide de VMIC), qui est un ordinateur Windows Server 2016 avec GPS qui est prouvé comme étant à la fois précis et stable.  Il s’agit d’une exigence critique pour une précision de 1 ms, qui est indiquée comme zone grisée.

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Performances du client de domaine enfant
Le client de domaine enfant est attaché à un contrôleur de domaine principal de domaine enfant qui communique avec le PDC racine.  L’heure est également comprise entre 1 et ms.

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>Test longue distance
Le graphique suivant compare 1 tronçon de réseau virtuel à 6 tronçons de réseau physique avec Windows Server 2016.  Deux graphiques sont superposés l’un à l’autre avec la transparence pour afficher les données qui se chevauchent.  L’augmentation du nombre de sauts réseau signifie une latence plus élevée et des écarts de temps plus importants.  Le graphique est agrandi et les limites de 1 ms, représentées par la zone verte, sont donc plus grandes.  Comme vous pouvez le voir, l’heure est toujours comprise entre 1 et plusieurs tronçons.  Elle est décalée de manière négative, ce qui illustre une asymétrie réseau.  Bien entendu, chaque réseau est différent et les mesures dépendent d’une multitude de facteurs environnementaux.

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>Meilleures pratiques pour une horloge précise
### <a name="solid-source-clock"></a>Horloge source pleine
La durée d’un ordinateur est aussi bonne que celle de l’horloge source avec laquelle il se synchronise.  Pour obtenir 1 ms de précision, vous avez besoin d’un matériel GPS ou d’une appliance de temps sur votre réseau que vous référencez comme horloge source principale.  L’utilisation de la valeur par défaut de time.windows.com peut ne pas fournir une source de temps stable et locale.  En outre, à mesure que vous vous éloignez de l’horloge source, le réseau a une incidence sur la précision.  Le fait de disposer d’une horloge source principale dans chaque centre de données est requis pour une meilleure précision.

### <a name="hardware-gps-options"></a>Options GPS matérielles
Différentes solutions matérielles peuvent offrir un temps précis.  En règle générale, les solutions actuelles sont basées sur des antennes GPS.  Il existe également des solutions de modems radio et Dial-up utilisant des lignes dédiées.  Ils sont attachés à votre réseau en tant qu’appliance, ou branchés sur un PC, par exemple Windows via un périphérique PCIe ou USB.  Différentes options offrent des niveaux de précision différents et, comme toujours, les résultats dépendent de votre environnement.  Les variables qui affectent la précision incluent la disponibilité GPS, la stabilité et la charge du réseau, ainsi que le matériel PC.  Ce sont tous des facteurs importants lors du choix d’une horloge source, qui, comme nous l’avons indiqué, est une exigence de temps stable et précis.

### <a name="domain-and-synchronizing-time"></a>Heure du domaine et synchronisation
Les membres du domaine utilisent la hiérarchie de domaine pour déterminer l’ordinateur qu’ils utilisent comme source pour synchroniser l’heure.  Chaque membre du domaine trouvera un autre ordinateur à synchroniser et l’enregistrera comme source d’horloge.  Chaque type de membre de domaine suit un ensemble différent de règles afin de rechercher une source d’horloge pour la synchronisation de l’heure.  Le contrôleur de domaine principal de la racine de la forêt est la source d’horloge par défaut de tous les domaines.  Vous trouverez ci-dessous différents rôles et une description de haut niveau pour la façon dont ils trouvent une source :


- **Contrôleur de domaine avec rôle** de contrôleur de domaine principal : cet ordinateur est la source de temps faisant autorité pour un domaine. Il disposera de l’heure la plus précise disponible dans le domaine et doit se synchroniser avec un contrôleur de domaine dans le domaine parent, sauf dans les cas où le rôle [GTIMESERV](#GTIMESERV) est activé. 
- **Tout autre contrôleur de domaine** : cet ordinateur agira comme source de temps pour les clients et les serveurs membres du domaine. Un contrôleur de domaine peut se synchroniser avec le contrôleur de domaine principal de son propre domaine ou n’importe quel contrôleur de domaine de son domaine parent.
- **Clients/serveurs membres** : cet ordinateur peut se synchroniser avec n’importe quel contrôleur de domaine ou contrôleur de domaine principal de son propre domaine, ou un contrôleur de domaine ou un contrôleur de domaine principal dans le domaine parent.

En fonction des candidats disponibles, un système de calcul de score est utilisé pour rechercher la meilleure source de temps.  Ce système prend en compte la fiabilité de la source de temps et de son emplacement relatif.  Cela se produit une fois lorsque le service est démarré.  Si vous avez besoin d’un contrôle plus fin de la synchronisation de l’heure, vous pouvez ajouter des serveurs de temps bien précis à des emplacements spécifiques ou ajouter une redondance.  Pour plus d’informations, consultez la section [spécifier un service de temps fiable local à l’aide de GTIMESERV](#GTIMESERV) .

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Environnements de système d’exploitation mixtes (Win2012R2 et Win2008R2)
Bien qu’un environnement de domaine Windows Server 2016 pur soit requis pour une meilleure précision, il existe toujours des avantages dans un environnement mixte.  Le déploiement de Windows Server 2016 Hyper-V dans un domaine Windows 2012 est bénéfique pour les invités en raison des améliorations que nous avons mentionnées ci-dessus, mais uniquement si les invités sont également Windows Server 2016.  Un contrôleur de domaine principal Windows Server 2016 est en mesure de fournir un temps plus précis en raison des algorithmes améliorés. il s’agit d’une source plus stable.  Comme le remplacement de votre contrôleur de domaine principal n’est peut-être pas une option, vous pouvez ajouter un contrôleur de domaine Windows Server 2016 avec le jeu de rouleaux [GTIMESERV](#GTIMESERV) qui constitue une mise à niveau de précision pour votre domaine.  Un contrôleur de service Windows Server 2016 peut offrir un meilleur temps pour les clients de temps en aval, mais il n’est pas aussi performant que son heure NTP source.

Comme indiqué ci-dessus, les fréquences d’interrogation et d’actualisation de l’horloge ont été modifiées avec Windows Server 2016.  Celles-ci peuvent être modifiées manuellement sur vos contrôleurs de l’application de niveau de plan ou appliquées via la stratégie de groupe.  Bien que nous n’ayons pas testé ces configurations, elles doivent se comporter correctement dans Win2008R2 et Win2012R2 et offrir certains avantages.

Les versions antérieures à Windows Server 2016 comportaient de nombreux problèmes afin de conserver un temps d’attente précis, entraînant une dérive du temps système juste après l’ajustement.  Pour cette raison, obtenir des échantillons de temps à partir d’une source NTP précise fréquemment et le conditionnement de l’horloge locale avec les données entraîne une plus petite dérive de leurs horloges système dans la période d’échantillonnage, ce qui permet d’assurer une meilleure conservation des versions de système d’exploitation de niveau inférieur. La meilleure précision observée est d’environ 5 ms lorsqu’un client NTP Windows Server 2012 R2, configuré avec les paramètres de haute précision, a synchronisé son temps à partir d’un serveur NTP Windows 2016 précis.

Dans certains scénarios impliquant des contrôleurs de domaine invités, les exemples Hyper-V TimeSync peuvent perturber la synchronisation de l’heure du domaine.  Cela ne devrait plus être un problème pour les invités du serveur 2016 s’exécutant sur des hôtes Hyper-V Server 2016.

Pour empêcher le service TimeSync Hyper-V de fournir des exemples à w32time, définissez la clé de Registre invité suivante :

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>Autoriser Linux à utiliser l’heure de l’hôte Hyper-V
Pour les invités Linux exécutés dans Hyper-V, les clients sont généralement configurés pour utiliser le démon NTP pour la synchronisation de l’heure avec les serveurs NTP.  Si la distribution Linux prend en charge le protocole TimeSync version 4 et que le service d’intégration TimeSync est activé sur l’invité Linux, il se synchronisera en fonction de l’heure de l’hôte. Cela peut entraîner un temps d’attente incohérent si les deux méthodes sont activées.

Pour synchroniser exclusivement avec l’heure de l’hôte, il est recommandé de désactiver la synchronisation de l’heure NTP par l’une des deux opérations suivantes :

- Désactivation de tous les serveurs NTP dans le fichier NTP. conf
- ou désactiver le démon NTP

Dans cette configuration, le paramètre de serveur de temps est cet hôte.  Sa fréquence d’interrogation est de 5 secondes et la fréquence de mise à jour de l’horloge est également de 5 secondes.

Pour synchroniser exclusivement sur NTP, il est recommandé de désactiver le service d’intégration TimeSync dans l’invité.

> [!NOTE]
> Remarque :  La prise en charge de l’heure précise avec les invités Linux nécessite une fonctionnalité qui est uniquement prise en charge dans les derniers noyaux Linux en amont et qui ne sont pas encore disponibles sur l’ensemble des distributions Linux. Pour plus d’informations sur la prise en charge des distributions, veuillez référencer [les machines virtuelles Linux et FreeBSD prises en charge pour Hyper-V sur Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows) .

#### <a name="GTIMESERV"></a>Spécifier un service de temps fiable local à l’aide de GTIMESERV
Vous pouvez spécifier un ou plusieurs contrôleurs de domaine en tant qu’horloges sources exactes à l’aide des indicateurs GTIMESERV, serveur de temps correct.  Par exemple, les contrôleurs de domaine spécifiques équipés du matériel GPS peuvent être marqués comme GTIMESERV.  Cela permet de s’assurer que votre domaine fait référence à une horloge basée sur le matériel GPS.

> [!NOTE]
> Vous trouverez plus d’informations sur les indicateurs de domaine dans la [documentation du protocole MS-ADTS](https://msdn.microsoft.com/library/mt226583.aspx).

TIMESERV est un autre indicateur des services de domaine associé qui indique si un ordinateur fait actuellement autorité, qui peut changer si un contrôleur de domaine perd la connexion.  Un contrôleur de périphérique dans cet État retourne « couche inconnue » lorsqu’il est interrogé via NTP.  Après plusieurs tentatives, le contrôleur de réseau consigne l’événement du service de temps des événements système 36.

Si vous souhaitez configurer un contrôleur de périphérique en tant que GTIMESERV, vous pouvez le configurer manuellement à l’aide de la commande suivante.  Dans ce cas, le contrôleur de bus utilise un autre ordinateur comme horloge maître.  Il peut s’agir d’un appareil ou d’un ordinateur dédié.

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> Pour plus d’informations, consultez [configurer le service de temps Windows](https://technet.microsoft.com/library/cc731191.aspx) .

Si le contrôleur de périphérique est équipé du matériel GPS, vous devez suivre les étapes ci-dessous pour désactiver le client NTP et activer le serveur NTP.

Commencez par désactiver le client NTP et activez le serveur NTP à l’aide de ces modifications de clé de registre.

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

Ensuite, redémarrez le service de temps Windows.

    net stop w32time && net start w32time

Enfin, vous indiquez que cet ordinateur dispose d’une source de temps fiable à l’aide de.
   
    w32tm /config /reliable:yes /update

Pour vérifier que les modifications ont été effectuées correctement, vous pouvez exécuter les commandes suivantes qui affectent les résultats affichés ci-dessous. 

    w32tm /query /configuration

Value|Paramètre ATTENDU|
----- | ----- |
AnnounceFlags|  5 (local)|
NtpServer   |Localisé|
DllName |C:\WINDOWS\SYSTEM32\w32time. DLL (locale)|
Enabled |1 (local)|
Essai|  Localisé|

    w32tm /query /status /verbose

Value|  Paramètre ATTENDU|
----- | ----- |
Stratum|    1 (référence principale-synchronisée par radio)|
ReferenceId|    0x4C4F434C (nom de la source :  « LOCAL »)|
`Source`| Horloge CMOS locale|
Décalage de phase|   0.0000000 s|
Rôle serveur|    576 (service de temps fiable)|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 sur des plateformes virtuelles tierces
Lorsque Windows est virtualisé, par défaut, l’hyperviseur est responsable de la fourniture du temps.  Toutefois, les membres appartenant à un domaine doivent être sychronized avec le contrôleur de domaine pour que Active Directory fonctionne correctement.  Il est préférable de désactiver toute virtualisation temporelle entre l’invité et l’hôte de plates-formes virtuelles tierces.

#### <a name="discovering-the-hierarchy"></a>Découverte de la hiérarchie
Dans la mesure où la chaîne de hiérarchie de temps à la source de l’horloge principale est dynamique dans un domaine et négociée, vous devez interroger l’état d’un ordinateur particulier pour comprendre la source de temps et la chaîne de l’horloge source principale.  Cela peut aider à diagnostiquer les problèmes de synchronisation de l’heure.

Vous voulez dépanner un client spécifique ; la première étape consiste à comprendre sa source de temps à l’aide de cette commande w32tm.

    w32tm /query /status

Les résultats affichent la source entre autres choses.  La source indique avec qui vous synchronisez l’heure dans le domaine.  Il s’agit de la première étape de cette hiérarchie de temps machines.
Utilisez ensuite l’entrée source ci-dessus et utilisez le paramètre/StripChart pour rechercher la source suivante dans la chaîne.

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

Également utile, la commande suivante répertorie chaque contrôleur de domaine qu’il peut trouver dans le domaine spécifié et imprime un résultat qui vous permet de déterminer chaque partenaire.  Cette commande inclut les ordinateurs qui ont été configurés manuellement.
    
    w32tm /monitor /domain:my_domain

À l’aide de la liste, vous pouvez suivre les résultats dans le domaine et comprendre la hiérarchie, ainsi que le décalage horaire à chaque étape.  En localisant le point où le décalage de temps est nettement pire, vous pouvez identifier la racine de l’heure incorrecte.  À partir de là, vous pouvez essayer de comprendre pourquoi cette durée est incorrecte en activant la [journalisation w32tm](#W32Logging). 

#### <a name="using-group-policy"></a>Utilisation de stratégie de groupe
Vous pouvez utiliser stratégie de groupe pour obtenir une plus grande précision, par exemple en affectant des clients pour utiliser des serveurs NTP spécifiques ou pour contrôler le mode de configuration des systèmes d’exploitation de niveau inférieure lorsqu’ils sont virtualisés.  
Vous trouverez ci-dessous une liste des scénarios possibles et des paramètres de stratégie de groupe pertinents :

**Domaines virtualisés** : afin de contrôler les contrôleurs de domaine virtualisés dans Windows 2012 R2 afin qu’ils synchronisent l’heure avec leur domaine, plutôt qu’avec l’hôte Hyper-V, vous pouvez désactiver cette entrée de registre.   Pour le contrôleur de domaine principal, vous ne souhaitez pas désactiver l’entrée, car l’hôte Hyper-V fournira la source de temps la plus stable.  L’entrée de Registre nécessite le redémarrage du service W32Time après sa modification.

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**Charges sensibles** à la précision : pour les charges de travail sensibles au temps, vous pouvez configurer des groupes d’ordinateurs pour définir les serveurs NTP et les paramètres d’heure associés, tels que l’interrogation et la fréquence de mise à jour de l’horloge.  Cela est normalement géré par le domaine, mais pour plus de contrôle, vous pouvez cibler des machines spécifiques pour pointer directement vers l’horloge principale.

Paramètre de stratégie de groupe|   Nouvelle valeur|
----- | ----- |
NtpServer|  ClockMasterName, 0x8|
MinPollInterval|    6 à 64 secondes|
MaxPollInterval|    6\.|
UpdateInterval| 100 – une fois par seconde|
EventLogFlags|  3 – toute la journalisation de l’heure spéciale|

> [!NOTE]
> Les paramètres NtpServer et EventLogFlags se trouvent sous System\Windows temps Service\Time fournisseurs à l’aide des paramètres configurer le client NTP Windows.  Les 3 autres sont situées sous le service de temps System\Windows à l’aide des paramètres de configuration globaux.

**Charges à distance sensibles à** distance : pour les systèmes des domaines de succursales pour l’instance de vente au détail et le secteur des crédits de paiement (PCI), Windows utilise les informations de site actuelles et le localisateur de contrôleur de domaine pour trouver un contrôleur de domaine local, sauf si une source de temps NTP manuelle est configurée .  Cet environnement nécessite 1 seconde de précision, qui utilise une convergence plus rapide jusqu’à l’heure correcte.  Cette option permet au service W32Time de déplacer l’horloge vers l’arrière.  Si cela est acceptable et répond à vos besoins, vous pouvez créer la stratégie suivante.   Comme pour n’importe quel environnement, veillez à tester et à établir une ligne de base de votre réseau. 

Paramètre de stratégie de groupe|   Nouvelle valeur|
----- | ----- |
MaxAllowedPhaseOffset|  1, si plus de la seconde, affectez à Clock une heure correcte.|

Le paramètre MaxAllowedPhaseOffset se trouve sous System\Windows Time service à l’aide des paramètres de configuration globaux.

> [!NOTE]
> Pour plus d’informations sur la stratégie de groupe et les entrées associées, consultez l’article outils et paramètres du [service de temps Windows](windows-time-service-tools-and-settings.md) sur TechNet.

## <a name="azure-and-windows-iaas-considerations"></a>Considérations relatives à Azure et à IaaS de Windows

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Machine virtuelle Azure : Services de domaine Active Directory
Si la machine virtuelle Azure exécutant Active Directory Domain Services fait partie d’une forêt Active Directory locale existante, TimeSync (VMIC) doit être désactivé. Cela permet d’autoriser tous les contrôleurs de l’ensemble de la forêt, physiques et virtuels, à utiliser une seule hiérarchie de synchronisation à l’heure. Reportez-vous au livre blanc sur les meilleures pratiques [« exécution de contrôleurs de domaine dans Hyper-V »](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

### <a name="azure-virtual-machine-domain-joined-machine"></a>Machine virtuelle Azure : Ordinateur joint à un domaine
Si vous hébergez un ordinateur qui est joint à un domaine existant Active Directory forêt, virtuel ou physique, la meilleure pratique consiste à désactiver TimeSync pour l’invité et à vérifier que W32Time est configuré pour se synchroniser avec son contrôleur de domaine via la configuration de l’heure pour Type = NTP5

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Machine virtuelle Azure : Ordinateur de groupe de travail autonome
Si la machine virtuelle Azure n’est pas jointe à un domaine, et qu’il ne s’agit pas d’un contrôleur de domaine, il est recommandé de conserver la configuration de l’heure par défaut et de la synchroniser avec l’ordinateur hôte.

## <a name="windows-application-requiring-accurate-time"></a>Application Windows nécessitant un temps précis
### <a name="time-stamp-api"></a>API d’horodatage
Les programmes qui requièrent la plus grande précision en ce qui concerne l’heure UTC, et non le passage du temps, doivent utiliser l' [API GetSystemTimePreciseAsFileTime](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx).  Cela garantit que votre application obtient l’heure système, qui est conditionnée par le service de temps Windows.

### <a name="udp-performance"></a>Performances UDP
Si vous disposez d’une application qui utilise la communication UDP pour les transactions et qu’il est important de réduire la latence, il existe des entrées de Registre associées que vous pouvez utiliser pour configurer une plage de ports à exclure du port du moteur de filtrage de base.  Cela permet d’améliorer la latence et d’augmenter votre débit.  Toutefois, les modifications apportées au registre doivent être limitées aux administrateurs expérimentés.  En outre, cette solution de contournement exclut le pare-feu de la sécurisation des ports.  Pour plus d’informations, consultez la référence d’article ci-dessous.

Pour Windows Server 2012 et Windows Server 2008, vous devez d’abord installer un correctif.  Vous pouvez vous référer à cet article de la base de connaissances : [Perte de datagrammes quand vous exécutez une application de récepteur de multidiffusion dans Windows 8 et Windows Server 2012](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>Mettre à jour les pilotes réseau
Certains fournisseurs de réseau ont des mises à jour de pilotes qui améliorent les performances en ce qui concerne la latence des pilotes et la mise en mémoire tampon des paquets UDP.  Contactez le fournisseur de votre réseau pour savoir s’il existe des mises à jour qui vous aideront à utiliser le débit UDP.

## <a name="logging-for-auditing-purposes"></a>Journalisation à des fins d’audit
Pour se conformer aux réglementations de suivi du temps, vous pouvez archiver manuellement les journaux w32tm, les journaux des événements et les informations de l’analyseur de performances.  Plus tard, les informations archivées peuvent être utilisées pour certifier la conformité à un moment précis dans le passé.  Les facteurs suivants sont utilisés pour indiquer la précision.


1. Précision de l’horloge à l’aide du compteur de l’analyseur de performances de décalage temporel calculé.  Cela indique l’horloge avec dans la précision souhaitée.
2.  Source de l’horloge à la recherche de « réponse homologue de » dans les journaux de w32tm.   À la suite du message, le texte est l’adresse IP ou VMIC, qui décrit la source de temps et la suivante dans la chaîne des horloges de référence à valider.
3.  État de la condition d’horloge à l’aide des journaux w32tm pour valider cette «discipline ClockDispl : \*L'\*heure\*d’inclinaison "se produit.  Cela indique que w32tm est actif à la fois.

### <a name="event-logging"></a>Journalisation des événements
Pour obtenir l’histoire complète, vous aurez également besoin des informations du journal des événements.  En recueillant le journal des événements système et en filtrant sur Time-Server, Microsoft-Windows-Kernel-boot, Microsoft-Windows-Kernel-General, vous pourrez peut-être déterminer s’il existe d’autres influences qui ont changé l’heure, par exemple, des tiers.  Ces journaux peuvent être nécessaires pour éliminer les interférences externes.  La stratégie de groupe peut affecter les journaux des événements qui sont écrits dans le journal.  Pour plus d’informations, consultez la section ci-dessus sur l’utilisation de stratégie de groupe.

### <a name="W32Logging"></a>Journalisation du débogage w32time
Pour activer w32tm à des fins d’audit, la commande suivante active la journalisation qui affiche les mises à jour périodiques de l’horloge et indique l’horloge source.  Redémarrez le service pour activer la nouvelle journalisation.  

Pour plus d’informations, consultez [Comment activer la journalisation du débogage dans le service de temps Windows](https://support.microsoft.com/kb/816043).

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>Analyseur de performances
Le service de temps Windows Windows Server 2016 expose des compteurs de performances qui peuvent être utilisés pour collecter les journaux d’audit.  Celles-ci peuvent être journalisées localement ou à distance.  Vous pouvez enregistrer les compteurs décalage horaire de l’ordinateur et délai aller-retour.  
Et comme n’importe quel compteur de performances, vous pouvez les surveiller à distance et créer des alertes à l’aide de System Center Operations Manager.  Vous pouvez, par exemple, utiliser une alerte pour vous avertir lorsque le décalage de temps dérive de la précision souhaitée.  Le [Pack d’administration de System Center](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx) contient plus d’informations.

### <a name="windows-traceability-example"></a>Exemple de traçabilité Windows
À partir de fichiers journaux de w32tm, vous souhaiterez valider deux informations.  La première indique que le fichier journal est actuellement une horloge de condition.  Cela prouve que votre horloge était conditionnée par le service de temps Windows au moment du litige.

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

Le point principal est que vous voyez des messages précédés de la discipline ClockDispln, ce qui prouve que w32time interagit avec l’horloge système.
 
Vous devez ensuite rechercher le dernier rapport dans le journal avant le temps de litige qui signale l’ordinateur source actuellement utilisé comme horloge de référence.  Il peut s’agir d’une adresse IP, d’un nom d’ordinateur ou du fournisseur VMIC, qui indique qu’il est en synchronisation avec l’hôte pour Hyper-V.  L’exemple suivant fournit une adresse IPv4 de 10.197.216.105.

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

Maintenant que vous avez validé le premier système dans la chaîne de temps de référence, vous devez examiner le fichier journal sur la source de temps de référence et répéter la même procédure.  Cela se poursuit jusqu’à ce que vous obteniez une horloge physique, telle que GPS ou une source de temps connue comme NIST.  Si l’horloge de référence est du matériel GPS, les journaux du fabriqué peuvent également être requis.

## <a name="network-considerations"></a>Considérations relatives au réseau
Les algorithmes de protocole NTP dépendent de la symétrie de votre réseau.  À mesure que vous augmentez le nombre de sauts réseau, la probabilité d’asymétrie augmente.  Là-là, il est difficile de prédire les types de précisions que vous verrez dans vos environnements spécifiques. 

L’analyseur de performances et les nouveaux compteurs de temps Windows dans Windows Server 2016 peuvent être utilisés pour évaluer la précision de vos environnements et créer des lignes de base. En outre, vous pouvez effectuer des dépannages pour déterminer le décalage actuel de n’importe quel ordinateur de votre réseau.

Il existe deux normes générales pour un temps précis sur le réseau.  PTP ([PRECISION Time Protocol-IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) a des exigences plus strictes en matière d’infrastructure réseau, mais peut souvent fournir une précision de sous-microseconde.  Le protocole NTP ([Network Time Protocol – RFC 1305](https://tools.ietf.org/html/rfc1305)) fonctionne sur un grand nombre de réseaux et d’environnements, ce qui en facilite la gestion. 

Windows prend en charge le protocole NTP simple (RFC2030) par défaut pour les ordinateurs non joints à un domaine.  Pour les machines jointes à un domaine, nous utilisons un NTP sécurisé appelé [MS-SNTP](https://msdn.microsoft.com/library/cc246877.aspx), qui tire parti des secrets négociés par domaine qui fournissent un avantage de gestion par rapport à NTP authentifié décrit dans rfc1305 et RFC5905.   

Le domaine et les protocoles non joints à un domaine nécessitent le port UDP 123.  Pour plus d’informations sur les meilleures pratiques NTP, consultez la [norme IETF (Network Time Protocol Best Practice Practices](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00)).

### <a name="reliable-hardware-clock-rtc"></a>Horloge matérielle fiable (RTC)
Windows n’a pas de temps pas à pas, sauf si certaines limites sont dépassées, mais plutôt la discipline de l’horloge.  Cela signifie que w32tm ajuste la fréquence de l’horloge à intervalles réguliers, en utilisant le paramètre fréquence de mise à jour de l’horloge, qui prend la valeur par défaut une fois par seconde avec Windows Server 2016.  Si l’horloge est en retard, elle accélère la fréquence et, si elle est en avance, ralentit la fréquence.  Toutefois, pendant ce délai entre les ajustements de fréquence d’horloge, l’horloge matérielle est dans le contrôle.  En cas de problème avec le microprogramme ou l’horloge matérielle, l’heure de l’ordinateur peut être moins précise.

Il s’agit d’une autre raison pour laquelle vous devez tester et la base de référence dans votre environnement.  Si le compteur de performances « décalage de temps calculé » ne se stabilise pas à la précision que vous ciblez, vous souhaiterez peut-être vérifier que le microprogramme est à jour.  En guise d’autre test, vous pouvez voir si le matériel dupliqué reproduire le même problème.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>Résolution des problèmes de précision du temps et NTP
Vous pouvez utiliser la section découverte de la hiérarchie ci-dessus pour comprendre la source de l’heure inexacte.  En examinant le décalage de temps, recherchez le point dans la hiérarchie où le temps est le plus élevé de sa source NTP.  Une fois que vous avez compris la hiérarchie, vous pouvez essayer de comprendre pourquoi cette source de temps ne reçoit pas de temps précis.  

En vous concentrant sur le système avec un temps divergent, vous pouvez utiliser ces outils ci-dessous pour recueillir des informations supplémentaires qui vous aideront à déterminer le problème et à trouver une solution.  La référence UpstreamClockSource ci-dessous est l’horloge découverte à l’aide de « w32tm/config/Status ».


- Journaux des événements système
- Activez la journalisation à l’aide de : journaux w32tm-w32tm/Debug/Enable/file : C:\Windows\Temp\w32time-test.log/Size : 10000000/Entries : 0-300
- clé de Registre w32Time HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- Suivis de réseau local
- Compteurs de performances (à partir de l’ordinateur local ou UpstreamClockSource)
- W32tm/stripchart/Computer : UpstreamClockSource
- PING UpstreamClockSource pour comprendre la latence et le nombre de tronçons à la source
- Tracert UpstreamClockSource

Problème|    Symptômes|   Résolution :|
----- | ----- | ----- |
L’horloge TSC locale n’est pas stable.| À l’aide de l’utilitaire Perfmon-Physical Computer : synchronisation de l’horloge stable, mais vous voyez toujours que toutes les 1-2 minutes de plusieurs 100US. |   Mettre à jour le microprogramme ou valider un matériel différent n’affiche pas le même problème.|
Latence du réseau|    w32tm stripchart affiche un RoundTripDelay de plus de 10 ms.  La variation du délai est due à un bruit de 1/2 de la durée du trajet aller-retour, par exemple un délai qui n’est que dans une seule direction.</br></br>UpstreamClockSource est plusieurs sauts, comme indiqué par PING.  La durée de vie doit être proche de 128.</br></br>Utilisez tracert pour rechercher la latence à chaque tronçon.    | Trouvez une source d’horloge plus proche pour l’heure.  Une solution consiste à installer une horloge source sur le même segment ou à POINTER manuellement vers une horloge source qui est géographiquement plus proche.  Pour un scénario de domaine, ajoutez un ordinateur avec le rôle GTimeServ. |  
Impossible d’atteindre la source NTP de manière fiable|    W32tm/stripchart retourne par intermittence « la demande a expiré »    |La source NTP ne répond pas|
La source NTP ne répond pas|    Vérifiez les compteurs PerfMon pour le nombre de sources du client NTP, les demandes entrantes du serveur NTP, les réponses sortantes du serveur NTP et déterminez votre utilisation par rapport à vos lignes de base.|    À l’aide des compteurs de performances du serveur, déterminez si le chargement a changé en référence à vos lignes de base.</br></br>Existe-t-il des problèmes de congestion du réseau ?|
Le contrôleur de domaine n’utilise pas l’horloge la plus précise|    Modifications de la topologie ou de l’horloge principale récemment ajoutée.|   w32tm/Resync/rediscover|
Les horloges client se dérivent| Événement de service de temps 36 dans le journal des événements système et/ou dans le texte du fichier journal décrivant les éléments suivants : Le compteur « nombre de sources de temps du client NTP » passe de 1 à 0|Résolvez les problèmes de la source amont et comprenez si elle s’exécute en cas de problème de performances.|

### <a name="baselining-time"></a>Heure de la planification
La base de référence est importante afin que vous puissiez d’abord comprendre les performances et la précision de votre réseau, et comparer à la ligne de base à l’avenir lorsque des problèmes surviennent.  Vous devez effectuer une ligne de base du PDC racine ou de n’importe quelle machine marquée avec GTIMESRV.  Nous vous recommandons également de référencer le contrôleur de domaine principal dans chaque forêt.  Enfin, choisissez les contrôleurs de l’État ou les machines critiques qui présentent des caractéristiques intéressantes, telles que la distance ou les chargements élevés et les bases de référence.

Il est également utile de procéder à la ligne de base de Windows Server 2016 vs 2012 R2. Toutefois, vous ne disposez que de w32tm/stripchart en tant qu’outil que vous pouvez utiliser pour comparer, puisque Windows Server 2012 R2 n’a pas de compteurs de performances.  Vous devez choisir deux machines ayant les mêmes caractéristiques, ou mettre à niveau un ordinateur et comparer les résultats après la mise à jour.  L’addenda mesures de temps Windows contient plus d’informations sur la façon d’effectuer des mesures détaillées entre 2016 et 2012.

À l’aide de tous les compteurs de performances w32time, collectez les données pendant au moins une semaine.  Cela vous permet de vous assurer que vous disposez d’une référence suffisante pour prendre en compte plusieurs dans le réseau au fil du temps et d’une exécution suffisante pour garantir la stabilité de l’exactitude de votre temps.

### <a name="ntp-server-redundancy"></a>Redondance des serveurs NTP
Pour une configuration de serveur NTP manuelle utilisée avec des ordinateurs non joints à un domaine ou le contrôleur de domaine principal, le fait de disposer de plusieurs serveurs est une bonne mesure de redondance en cas de disponibilité.  Elle peut également offrir une meilleure précision, en supposant que toutes les sources sont exactes et stables.  Toutefois, si la topologie n’est pas bien conçue, ou si les sources de temps ne sont pas stables, l’exactitude résultante peut être pire. c’est la bonne prudence.  La limite des serveurs de temps pris en charge que w32time peut référencer manuellement est 10. 

## <a name="leap-seconds"></a>Secondes bissextiles
La période de rotation de la terre varie dans le temps, en raison des événements climatiques et géologiques. En règle générale, la variation est d’environ une seconde tous les deux ans. Chaque fois que la variation par rapport à l’heure atomique devient trop importante, la correction d’une seconde (en haut ou en aval) est insérée, appelée une seconde bissextile. Cela s’effectue de manière à ce que la différence ne dépasse jamais 0,9 secondes. Cette correction est annoncée six mois avant la correction réelle. Avant Windows Server 2016, le service de temps Microsoft n’était pas conscient des secondes bissextiles, mais reposait sur le service de temps externe pour s’en occuper. Avec l’amélioration de la précision du temps de Windows Server 2016, Microsoft travaille sur une solution plus appropriée pour le deuxième problème de la seconde.

## <a name="secure-time-seeding"></a>Amorçage de temps sécurisé
W32time dans le serveur 2016 comprend la fonctionnalité d’amorçage de temps sécurisé. Cette fonctionnalité détermine l’heure actuelle approximative des connexions SSL sortantes.  Cette valeur de temps est utilisée pour surveiller l’horloge système locale et corriger les erreurs brutes. Pour plus d’informations sur la fonctionnalité, consultez ce billet de [blog](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/).  Dans les déploiements avec une source de temps fiable et des ordinateurs surveillés qui incluent la surveillance des décalages de temps, vous pouvez choisir de ne pas utiliser la fonctionnalité d’amorçage de temps sécurisé et de vous appuyer sur votre infrastructure existante à la place. 

Vous pouvez désactiver la fonctionnalité en procédant comme suit :

1.  Définissez la valeur de configuration du Registre UtilizeSSLTimeData sur 0 sur un ordinateur spécifique :

    Reg Add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config/v UtilizeSslTimeData/t REG_DWORD/d 0/f


2.  Si vous ne parvenez pas à redémarrer l’ordinateur immédiatement en raison d’une raison quelconque, vous pouvez notifier le service W32Time sur la mise à jour de la configuration. Cela arrête la surveillance et la mise en œuvre du temps en fonction des données de temps collectées à partir des connexions SSL. 

    W32tm. exe/config/Update

3.  Le redémarrage de l’ordinateur rend le paramètre effectif immédiatement et entraîne l’arrêt de la collecte des données de temps des connexions SSL.  La dernière partie a une surcharge très petite et ne doit pas être un problème de performances.

4.  Pour appliquer ce paramètre dans l’ensemble d’un domaine, définissez la valeur UtilizeSSLTimeData dans le paramètre de stratégie de groupe w32time sur 0 et publiez le paramètre.  Lorsque le paramètre est sélectionné par un client stratégie de groupe, le service W32Time est averti et arrête la surveillance et l’application de l’heure à l’aide des données de temps SSL. La collecte de données de temps SSL s’arrête lorsque chaque ordinateur redémarre. Si votre domaine contient des ordinateurs portables/tablettes portables et d’autres appareils, vous pouvez exclure ces ordinateurs de cette modification de stratégie. Ces appareils finiront par se retrouver dans la batterie et ont besoin de la fonction d’amorçage de temps sécurisé pour démarrer leur temps.


