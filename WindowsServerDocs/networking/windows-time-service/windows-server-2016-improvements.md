---
title: Améliorations apportées à Windows Server 2016
description: Windows Server 2016 a amélioré les algorithmes qu’il utilise pour corriger l’heure et la condition de synchronisation de l’horloge locale avec le temps universel coordonné (UTC).
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 2723868251f90429fb0ad5e966c9222a6a22ab0c
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77520685"
---
# <a name="windows-server-2016-improvements"></a>Améliorations apportées à Windows Server 2016

Cet article contient des informations sur les améliorations apportées à Windows Server 2016.

## <a name="windows-time-service-and-ntp"></a>Service de temps Windows et NTP

Windows Server 2016 a amélioré les algorithmes qu’il utilise pour corriger l’heure et la condition de synchronisation de l’horloge locale avec le temps universel coordonné (UTC). NTP utilise 4 valeurs pour calculer le décalage de temps, en fonction des horodatages de la demande/réponse du client et de la demande/réponse du serveur. Toutefois, les réseaux sont bruyants et il peut y avoir des pics de données à partir de NTP en raison de la surcharge du réseau et d’autres facteurs qui affectent la latence du réseau. Les algorithmes Windows 2016 répartissent ce bruit à l’aide d’un certain nombre de techniques différentes, ce qui aboutit à une horloge stable et précise. De plus, la source utilisée pour la précision de l’heure fait référence à une API améliorée qui nous offre une meilleure résolution. Grâce à ces améliorations, nous sommes en mesure d’atteindre une précision de 1 ms en ce qui concerne l’heure UTC dans un domaine.

## <a name="hyper-v"></a>Hyper-V

Windows 2016 a amélioré le service TimeSync Hyper-V. Les améliorations incluent une heure initiale plus précise pour le démarrage ou la restauration des machines virtuelles et la correction de la latence d’interruption pour les échantillons fournis à w32time. Cette amélioration nous permet de rester dans les 10 μs de l’hôte avec une valeur efficace (ou RMS, Root Mean Squared, qui indique la variance), de 50 μs, même sur un ordinateur avec une charge de 75 %. Pour plus d’informations, consultez [Architecture Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx).

> [!NOTE]
> La charge a été créée avec Prime95 Benchmark à l’aide d’un profil équilibré.

De plus, le niveau de strate que l’hôte signale à l’invité est plus transparent. Avant, l’hôte présentait une strate fixe de 2, quelle que soit sa précision. Avec les changements apportés à Windows Server 2016, l’hôte signale une strate de 1 supérieure à la strate de l’hôte, ce qui produit un meilleur temps pour les invités virtuels. La strate de l’hôte est déterminée par w32time via les moyens habituels, en fonction de son heure source. Les invités Windows 2016 joints à un domaine trouveront l’horloge la plus précise, plutôt que d’utiliser par défaut l’hôte. C’était pour cette raison que nous recommandions de désactiver manuellement le paramètre de fournisseur de temps Hyper-V pour les ordinateurs participant à un domaine dans Windows 2012 R2 et versions antérieures.

## <a name="monitoring"></a>Analyse
Des compteurs de l’Analyseur de performances ont été ajoutés. Ils vous permettent de planifier, de superviser et de résoudre les problèmes de précision de l’heure. Ces compteurs sont notamment les suivants :

|Compteur|Description|
|----- | ----- |
|Décalage de temps calculé| Décalage de temps absolu entre l’horloge système et la source de temps choisie, comme calculé par le service w32time en microsecondes. Quand un nouvel échantillon valide est disponible, le temps calculé est mis à jour avec le décalage de temps indiqué par l’échantillon. Il s’agit du décalage de temps réel de l’horloge locale. w32time lance la correction de l’horloge à l’aide de ce décalage et met à jour le temps calculé entre les échantillons avec le décalage de temps restant qui doit être appliqué à l’horloge locale. La précision de l’horloge peut être suivie à l’aide de ce compteur de performances avec un intervalle d’interrogation faible (par exemple, 256 secondes ou moins) et en comptant sur le fait que la valeur du compteur soit inférieure à la limite de précision de l’horloge souhaitée.|
|Ajustement de fréquence d’horloge| Ajustement de fréquence d’horloge absolue apporté à l’horloge du système local par le service w32time (parties par milliard). Ce compteur aide à visualiser les actions entreprises par w32time.|
|Délai de l’aller-retour NTP| Délai du dernier aller-retour rencontré par le client NTP lors de la réception d’une réponse du serveur (en microsecondes). Il s’agit du temps écoulé sur le client NTP entre la transmission d’une demande au serveur NTP et la réception d’une réponse valide de la part du serveur. Ce compteur aide à caractériser les délais expérimentés par le client NTP. Des allers-retours supérieurs ou variables peuvent ajouter du bruit aux calculs de temps NTP, ce qui peut affecter la précision de la synchronisation date/heure par le biais de NTP.|
|Nombre de sources de temps de client NTP| Nombre actif de sources de temps NTP utilisées par le client NTP. Il s’agit du nombre d’adresses IP actives distinctes des serveurs de temps qui répondent aux demandes de ce client. Ce nombre peut être supérieur ou inférieur aux pairs configurés, en fonction de la résolution DNS des noms de pair et de l’accessibilité actuelle.|
|Requêtes entrantes de serveur NTP| Nombre de demandes reçues par le serveur NTP (demandes/s).|
|Réponses sortantes de serveur NTP| Nombre de demandes auxquelles le serveur NTP a répondu (réponses/s).|

Les 3 premiers compteurs ciblent des scénarios de résolution des problèmes de précision. La section « Résolution des problèmes de précision de l’heure et NTP » ci-dessous, sous Bonnes pratiques, offre plus de détails.
Les 3 derniers compteurs couvrent les scénarios de serveur NTP et sont utiles lors de la détermination de la charge et de la planification des performances actuelles.

## <a name="configuration-updates-per-environment"></a>Mises à jour de la configuration par environnement
Le tableau suivant décrit les modifications apportées à la configuration par défaut entre Windows 2016 et les versions précédentes pour chaque rôle. Les paramètres de Windows Server 2016 et Mise à jour anniversaire Windows 10 (build 14393) sont désormais uniques, ce qui explique pourquoi ils s’affichent dans des colonnes distinctes.

|Rôle|Paramètre|Windows Server 2016|Windows 10|Windows Server 2012 R2<br>Windows Server 2008 R2<br>Windows 10|
|---|---|---|---|---|
|**Autonome/Nano Server**||||
| |*Serveur de temps*|time.windows.com|NA|time.windows.com|
| |*Fréquence d’interrogation*|64 à 1 024 secondes|NA|Une fois par semaine|
| |*Fréquence de mise à jour de l’horloge*|Une fois par seconde|NA|Une fois par heure|
|**Client autonome**||||
| |*Serveur de temps*|NA|time.windows.com|time.windows.com|
| |*Fréquence d’interrogation*|NA|Une fois par jour|Une fois par semaine|
| |*Fréquence de mise à jour de l’horloge*|NA|Une fois par jour|Une fois par semaine|
|**Contrôleur de domaine**||||
| |*Serveur de temps*|PDC/GTIMESERV|NA|PDC/GTIMESERV|
| |*Fréquence d’interrogation*|64 à 1 024 secondes|NA|1 024 à 32 768 secondes|
| |*Fréquence de mise à jour de l’horloge*|Une fois par jour|NA|Une fois par semaine|
|**Serveur membre du domaine**||||
| |*Serveur de temps*|DC|NA|DC|
| |*Fréquence d’interrogation*|64 à 1 024 secondes|NA|1 024 à 32 768 secondes|
| |*Fréquence de mise à jour de l’horloge*|Une fois par seconde|NA|Une fois toutes les 5 minutes|
|**Client membre du domaine**||||
| |*Serveur de temps*|NA|DC|DC|
| |*Fréquence d’interrogation*|NA|1 024 à 32 768 secondes|1 024 à 32 768 secondes|
| |*Fréquence de mise à jour de l’horloge*|NA|Une fois toutes les 5 minutes|Une fois toutes les 5 minutes|
|**Invité Hyper-V**||||
| |*Serveur de temps*|Choisit la meilleure option en fonction de la strate d’hôte et du serveur de temps|Choisit la meilleure option en fonction de la strate d’hôte et du serveur de temps|Hôte par défaut|
| |*Fréquence d’interrogation*|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|
| |*Fréquence de mise à jour de l’horloge*|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|

>[!NOTE]
>Pour Linux dans Hyper-V, consultez la section Autoriser Linux à utiliser l’heure de l’hôte Hyper-V ci-dessous.

## <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impact de l’augmentation des fréquences d’interrogation et de mise à jour de l’horloge

Pour offrir une plus grande précision de l’heure, les valeurs par défaut pour les fréquences d’interrogation et les mises à jour de l’horloge sont augmentées, ce qui nous permet d’effectuer de petits ajustements plus fréquemment. Cela entraînera davantage de trafic UDP/NTP. Toutefois, ces paquets sont peu volumineux, de sorte qu’il ne doit y avoir que peu ou pas d’impact sur les liaisons haut débit. L’avantage, cependant, est que l’heure doit être plus précise sur une large gamme de matériel et d’environnements.

Pour les appareils alimentés par batterie, l’augmentation de la fréquence d’interrogation peut entraîner des problèmes. Les appareils sur batterie ne stockent pas l’heure quand ils sont éteints. Leur remise sous tension peut nécessiter des corrections fréquentes de l’horloge. L’augmentation de la fréquence d’interrogation entraîne une instabilité de l’horloge et peut également consommer plus d’énergie. Microsoft vous recommande de ne pas changer les paramètres par défaut du client.

Les contrôleurs de domaine doivent être impactés au minimum, même avec l’effet multiplié des mises à jour accrues des clients NTP dans un domaine Active Directory. La consommation de ressources de NTP est plus faible par rapport à d’autres protocoles et son impact est également négligeable. Vous êtes plus susceptible d’atteindre les limites d’autres fonctionnalités de domaine avant d’être impacté par l’augmentation de la valeur des paramètres pour Windows Server 2016. Active Directory utilise le protocole NTP sécurisé, qui a tendance à synchroniser le temps moins précisément que le protocole NTP simple, mais nous avons vérifié qu’il effectue un scale-up jusqu’aux clients à deux strates du contrôleur de domaine principal.

En guise de plan minimal, vous devez réserver 100 demandes NTP par seconde par cœur. Par exemple, pour un domaine composé de 4 contrôleurs de domaine avec 4 cœurs chacun, vous devez être en mesure de traiter 1 600 demandes NTP par seconde. Si vous avez 10 000 clients configurés pour synchroniser l’heure une fois toutes les 64 secondes et que les demandes sont reçues uniformément au fil du temps, vous verrez 10 000/64 ou environ 160 demandes/seconde, réparties sur tous les contrôleurs de domaine. Ce chiffre respecte facilement les 1 600 demandes NTP/s selon cet exemple. Ce sont des recommandations de planification minimale qui dépendent bien sûr grandement de votre réseau et des vitesses et charges du processeur, donc comme toujours, établissez une base de référence et faites des tests dans vos environnements.

Il est également important de noter que, si vos contrôleurs de domaine s’exécutent avec une charge processeur considérable, supérieure à 40 %, cela ajoutera presque certainement du bruit aux réponses NTP et affectera la précision de l’heure dans votre domaine. Là encore, vous devez tester dans votre environnement pour comprendre les résultats réels.

## <a name="time-accuracy-measurements"></a>Mesures de précision de l’heure
### <a name="methodology"></a>Méthodologie
Pour mesurer la précision de l’heure pour Windows Server 2016, nous avons utilisé divers outils, méthodes et environnements. Vous pouvez utiliser ces techniques pour mesurer et ajuster votre environnement et déterminer si les résultats de précision répondent à vos besoins. 

Notre horloge source de domaine est composée de deux serveurs NTP de haute précision avec un matériel GPS. Nous avons aussi utilisé un ordinateur de test de référence distinct pour les mesures, équipé également d’un matériel GPS de haute précision installé à partir d’un autre fabricant. Pour certains tests, vous aurez besoin d’une source d’horloge précise et fiable à utiliser comme référence en plus de la source d’horloge de domaine.

Nous avons utilisé quatre méthodes différentes pour mesurer la précision avec des machines physiques et virtuelles. Plusieurs méthodes ont fourni des moyens indépendants de valider les résultats.

1. Mesurez l’horloge locale, qui est conditionnée par w32tm, par rapport à notre ordinateur de test de référence qui dispose d’un matériel GPS distinct. 
2. Mesurez les tests Ping NTP entre le serveur NTP et les clients à l’aide du « graphique à bandes » w32tm
3. Mesurez les tests Ping NTP entre le client et le serveur NTP à l’aide du « graphique à bandes » w32tm
4. Mesurez les résultats Hyper-V entre l’hôte et l’invité à l’aide du compteur d’horodatage (TSC). Ce compteur est partagé entre les deux partitions et l’heure système dans les deux partitions. Nous avons calculé la différence entre l’heure de l’hôte et l’heure du client dans la machine virtuelle. Ensuite, nous utilisons l’horloge TSC pour interpoler l’heure de l’hôte à partir de l’invité, car les mesures ne se produisent pas en même temps. Nous utilisons aussi l’horloge TSV pour supprimer les délais et la latence dans l’API.

W32tm est intégré, mais les autres outils que nous avons utilisés pendant nos tests sont disponibles pour le dépôt Microsoft sur GitHub en open source pour vos tests et votre utilisation. Le wiki sur le dépôt contient des informations supplémentaires qui décrivent comment utiliser les outils pour effectuer des mesures.

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

Les résultats des tests indiqués ci-dessous sont un sous-ensemble de mesures que nous avons effectuées dans l’un des environnements de test. Ils illustrent la précision conservée au début de la hiérarchie temporelle et le client de domaine enfant à la fin de la hiérarchie temporelle. Ces résultats sont comparés aux mêmes ordinateurs dans une topologie 2012.

### <a name="topology"></a>Topologie
À des fins de comparaison, nous avons testé à la fois une topologie Windows Server 2012 R2 et Windows Server 2016. Les deux topologies sont constituées de deux ordinateurs hôtes Hyper-V physiques qui font référence à une machine Windows Server 2016 sur laquelle le matériel d’horloge GPS est installé. Chaque hôte exécute 3 invités Windows joints à un domaine, qui sont organisés selon la topologie suivante. Les lignes représentent la hiérarchie temporelle et le protocole/transport utilisé.

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Vue d’ensemble des résultats graphiques
Les deux graphiques suivants représentent la précision de l’heure pour deux membres spécifiques dans un domaine basé sur la topologie ci-dessus. Chaque graphique affiche à la fois les résultats de Windows Server 2012 R2 et 2016 superposés, description visuelle des améliorations. La précision a été mesurée à partir de l’ordinateur invité par rapport à l’hôte. Les données graphiques représentent un sous-ensemble de l’ensemble des tests que nous avons effectués et illustrent les meilleurs et pires scénarios d’utilisation. 

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Performances du contrôleur de domaine principal du domaine racine
Le contrôleur de domaine principal racine est synchronisé avec l’hôte Hyper-V (à l’aide de VMIC), qui est un ordinateur Windows Server 2016 avec GPS réputé comme étant à la fois précis et stable. Il s’agit d’une condition essentielle pour une précision de 1 ms, indiquée comme zone ombrée verte.

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Performances du client de domaine enfant
Le client de domaine enfant est attaché à un contrôleur de domaine principal de domaine enfant qui communique avec le contrôleur racine. Son heure entre également dans la précision obligatoire de 1 ms.

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)

### <a name="long-distance-test"></a>Test longue distance
Le graphique suivant compare 1 saut de réseau virtuel à 6 sauts de réseau physique avec Windows Server 2016. Deux graphiques sont superposés avec une certaine transparence pour afficher les données qui se chevauchent. L’augmentation du nombre de sauts réseau signifie une latence plus élevée et des écarts de temps plus importants. Le graphique est agrandi et les limites de 1 ms, représentées par la zone verte, sont donc plus grandes. Comme vous pouvez le voir, l’heure garde une précision de 1 ms avec plusieurs sauts. Elle est décalée dans les négatifs, ce qui illustre une asymétrie réseau. Bien entendu, chaque réseau est différent et les mesures dépendent d’une multitude de facteurs environnementaux.

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="best-practices-for-accurate-timekeeping"></a>Bonnes pratiques pour un chronométrage précis
### <a name="solid-source-clock"></a>Horloge source fiable
L’heure d’un ordinateur est aussi précise que celle de l’horloge source avec laquelle il se synchronise. Pour obtenir une précision de 1 ms, vous avez besoin d’un matériel GPS ou d’une appliance de temps sur votre réseau que vous référencez comme horloge source maître. L’utilisation de la valeur par défaut time.windows.com peut ne pas fournir une source de temps stable et locale. De plus, à mesure que vous vous éloignez de l’horloge source, le réseau a une incidence sur la précision. Il est obligatoire de disposer d’une horloge source maître dans chaque centre de données pour une précision optimale.

### <a name="hardware-gps-options"></a>Options GPS matérielles
Différentes solutions matérielles peuvent offrir une heure exacte. En règle générale, les solutions actuelles sont basées sur des antennes GPS. Il existe également des solutions de modem d’accès à distance et radio utilisant des lignes dédiées. Elles sont reliées à votre réseau en tant qu’appliance, ou branchées à un PC, par exemple Windows via un périphérique PCIe ou USB. Différentes options offrent des niveaux de précision différents et, comme toujours, les résultats dépendent de votre environnement. Les variables qui affectent la précision incluent la disponibilité GPS, la stabilité et la charge du réseau ainsi que le matériel PC. Ce sont tous des facteurs importants lors du choix d’une horloge source qui, comme nous l’avons indiqué, est une condition essentielle pour une heure stable et précise.

### <a name="domain-and-synchronizing-time"></a>Domaine et synchronisation de l’heure
Les membres du domaine utilisent la hiérarchie de domaine pour identifier l’ordinateur qu’ils utilisent comme source pour synchroniser l’heure. Chaque membre du domaine trouvera un autre ordinateur avec lequel se synchroniser et l’enregistrera comme sa source d’horloge. Chaque type de membre de domaine suit un ensemble différent de règles afin de rechercher une source d’horloge pour la synchronisation de l’heure. Le contrôleur de domaine principal de la racine de la forêt est la source d’horloge par défaut de tous les domaines. Vous trouverez ci-dessous différents rôles et une description générale de la façon dont ils trouvent une source :


- **Contrôleur de domaine avec rôle PDC** : cet ordinateur est la source de temps faisant autorité pour un domaine. Il disposera de l’heure la plus précise disponible dans le domaine et doit se synchroniser avec un contrôleur de domaine dans le domaine parent, sauf dans les cas où le rôle GTIMESERV est activé.
- **Tout autre contrôleur de domaine** : cet ordinateur agit en tant que source de temps pour les clients et les serveurs membres du domaine. Un contrôleur de domaine peut se synchroniser avec le contrôleur de domaine principal de son propre domaine ou n’importe quel contrôleur de domaine de son domaine parent.
- **Clients/serveurs membres** : cet ordinateur peut se synchroniser avec n’importe quel contrôleur de domaine ou contrôleur de domaine principal de son propre domaine, ou un contrôleur de domaine ou un contrôleur de domaine principal dans le domaine parent.

En fonction des candidats disponibles, un système de scoring est utilisé pour rechercher la meilleure source de temps. Ce système prend en compte la fiabilité de la source de temps et de son emplacement relatif. Cela se produit une fois lorsque le service est démarré. Si vous avez besoin de plus de contrôle sur la synchronisation de l’heure, vous pouvez ajouter des serveurs de temps appropriés à des emplacements spécifiques ou ajouter une redondance. Pour plus d’informations, consultez la section Spécifier un service de temps fiable local à l’aide de GTIMESERV.

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Environnements de systèmes d’exploitation mixtes (Win2012R2 et Win2008R2)
Bien qu’un environnement de domaine Windows Server 2016 pur soit requis pour une précision optimale, un environnement mixte présente toujours des avantages. Le déploiement de Windows Server 2016 Hyper-V sur un domaine Windows 2012 est bénéfique pour les invités en raison des améliorations que nous avons mentionnées ci-dessus, mais uniquement si les invités utilisent également Windows Server 2016. Un contrôleur de domaine principal Windows Server 2016 est en mesure de fournir une plus grande précision de l’heure en raison des algorithmes améliorés. Il s’agit d’une source plus stable. Comme le remplacement de votre contrôleur de domaine principal n’est peut-être pas possible, vous pouvez ajouter un contrôleur de domaine Windows Server 2016 avec le rôle GTIMESERV défini qui constitue une mise à niveau de la précision pour votre domaine. Un contrôleur de domaine Windows Server 2016 peut offrir un meilleur temps aux clients du service de temps en aval, mais il est simplement aussi performant que son temps NTP source.

Comme indiqué ci-dessus, les fréquences d’interrogation et d’actualisation de l’horloge ont été modifiées avec Windows Server 2016. Celles-ci peuvent être modifiées manuellement sur vos contrôleurs de domaine de bas niveau ou appliquées via une stratégie de groupe. Bien que nous n’ayons pas testé ces configurations, elles doivent se comporter correctement dans Win2008R2 et Win2012R2 et offrir certains avantages.

Les versions antérieures à Windows Server 2016 connaissaient de nombreux problèmes pour assurer un chronométrage précis, entraînant une dérive du temps système juste après un ajustement. Pour cette raison, l’obtention d’échantillons de temps fréquemment à partir d’une source NTP précise et le conditionnement de l’horloge locale avec les données entraîne une plus petite dérive de leurs horloges système dans la période d’échantillonnage, ce qui permet d’assurer un meilleure chronométrage des versions de système d’exploitation de bas niveau. La meilleure précision observée était d’environ 5 ms lorsqu’un client NTP Windows Server 2012 R2, configuré avec les paramètres de haute précision, a synchronisé son heure à partir d’un serveur NTP Windows 2016 précis.

Dans certains scénarios impliquant des contrôleurs de domaine invités, les échantillons TimeSync Hyper-V peuvent perturber la synchronisation de l’heure du domaine. Cela ne devrait plus être un problème pour les invités Windows Server 2016 s’exécutant sur des hôtes Hyper-V Server 2016.

Pour empêcher le service TimeSync Hyper-V de fournir des échantillons à w32time, définissez la clé de Registre d’invité suivante :

 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider "Enabled"=dword:00000000

#### <a name="allowing-linux-to-use-hyper-v-host-time"></a>Autoriser Linux à utiliser l’heure de l’hôte Hyper-V
Pour les invités Linux exécutés dans Hyper-V, les clients sont généralement configurés afin d’utiliser le démon NTP pour la synchronisation de l’heure sur les serveurs NTP. Si la distribution Linux prend en charge le protocole TimeSync version 4 et que le service d’intégration TimeSync est activé sur l’invité Linux, il se synchronisera en fonction de l’heure de l’hôte. Cela peut entraîner un chronométrage incohérent si les deux méthodes sont activées.

Pour synchroniser exclusivement par rapport à l’heure de l’hôte, il est recommandé de désactiver la synchronisation de l’heure NTP par l’une des deux opérations suivantes :

- Désactivation de tous les serveurs NTP dans le fichier ntp.conf
- ou Désactivation du démon NTP

Dans cette configuration, le paramètre de serveur de temps est cet hôte. Sa fréquence d’interrogation est de 5 secondes et la fréquence de mise à jour de l’horloge est également de 5 secondes.

Pour synchroniser exclusivement sur NTP, il est recommandé de désactiver le service d’intégration TimeSync dans l’invité.

> [!NOTE]
> Remarque : la prise en charge de la précision de l’heure avec les invités Linux nécessite une fonctionnalité qui est uniquement prise en charge dans les derniers noyaux Linux en amont et qui n’est pas encore disponible sur l’ensemble des distributions Linux. Pour plus d’informations sur la prise en charge des distributions, consultez [Machines virtuelles Linux et FreeBSD prises en charge pour Hyper-V sur Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows).

#### <a name="specify-a-local-reliable-time-service-using-gtimeserv"></a>Spécifier un service de temps fiable local à l’aide de GTIMESERV
Vous pouvez spécifier un ou plusieurs contrôleurs de domaine en tant qu’horloges sources précises à l’aide des indicateurs GTIMESERV ou de serveur de temps approprié. Par exemple, les contrôleurs de domaine spécifiques équipés du matériel GPS peuvent être marqués comme GTIMESERV. Cela permet de s’assurer que votre domaine fait référence à une horloge basée sur le matériel GPS.

> [!NOTE]
> Vous trouverez plus d’informations sur les indicateurs de domaine dans la [documentation du protocole MS-ADTS](https://msdn.microsoft.com/library/mt226583.aspx).

TIMESERV est un autre indicateur des services de domaine associé qui indique si un ordinateur fait actuellement autorité, ce qui peut changer si un contrôleur de domaine perd la connexion. Un contrôleur de domaine dans cet état retourne « Strate inconnue » lorsqu’il est interrogé via NTP. Après plusieurs tentatives, le contrôleur de domaine consigne l’événement du service de temps des événements système 36.

Si vous souhaitez configurer un contrôleur de domaine en tant que GTIMESERV, vous pouvez le faire manuellement à l’aide de la commande suivante. Dans ce cas, le contrôleur de domaine utilise un ou plusieurs autres ordinateurs comme horloge maître. Il peut s’agir d’une appliance ou d’un ordinateur dédié.

 w32tm /config /manualpeerlist:"master_clock1,0x8 master_clock2,0x8" /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> Pour plus d’informations, consultez [Configurer le service de temps Windows](https://technet.microsoft.com/library/cc731191.aspx)

Si du matériel GPS est installé sur le contrôleur de domaine, vous devez suivre les étapes ci-dessous pour désactiver le client NTP et activer le serveur NTP.

Commencez par désactiver le client NTP et activez le serveur NTP à l’aide de ces modifications de clé de Registre.

 reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

 reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

Redémarrez ensuite le service de temps Windows

 net stop w32time && net start w32time

Enfin, vous indiquez que cet ordinateur dispose d’une source de temps fiable avec
  
 w32tm /config /reliable:yes /update

Pour vérifier que les modifications ont été effectuées correctement, vous pouvez exécuter les commandes suivantes qui affectent les résultats affichés ci-dessous. 

 w32tm /query /configuration

Value|Paramètre attendu|
----- | ----- |
AnnounceFlags| 5 (Local)|
NtpServer |(Local)|
DllName |C:\WINDOWS\SYSTEM32\w32time.DLL (Local)|
Activé |1 (Local)|
NtpClient| (Local)|

 w32tm /query /status /verbose

Value| Paramètre attendu|
----- | ----- |
Strate| 1 (référence principale, synchronisée par l’horloge du réveil)|
ReferenceId| 0x4C4F434C (nom de la source : « LOCAL »)|
Source| Horloge CMOS locale|
Décalage de phase| 0,0000000 s|
Rôle de serveur| 576 (service de temps fiable)|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 sur des plateformes virtuelles tierces
Quand Windows est virtualisé, l’hyperviseur est par défaut responsable de la fourniture de l’heure. Toutefois, les membres joints à un domaine doivent être synchronisés avec le contrôleur de domaine pour qu’Active Directory fonctionne correctement. Il est préférable de désactiver toute virtualisation temporelle entre l’invité et l’hôte des plateformes virtuelles tierces.

#### <a name="discovering-the-hierarchy"></a>Découverte de la hiérarchie
Dans la mesure où la chaîne de hiérarchie temporelle pour la source de l’horloge maître est dynamique dans un domaine et négociée, vous devez interroger l’état d’un ordinateur particulier pour comprendre sa source de temps et sa chaîne pour l’horloge source maître. Cela peut aider à diagnostiquer les problèmes de synchronisation de l’heure.

Vous voulez dépanner un client spécifique et la première étape consiste à comprendre sa source de temps à l’aide de cette commande w32tm.

 w32tm /query /status

Les résultats affichent la source parmi d’autres éléments. La source indique avec qui vous synchronisez l’heure dans le domaine. Il s’agit de la première étape de la hiérarchie temporelle de cet ordinateur.
Utilisez ensuite l’entrée source ci-dessus et le paramètre /StripChart pour rechercher la source de temps suivante dans la chaîne.

 w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

La commande suivante est également utile, car elle liste chaque contrôleur de domaine qu’elle peut trouver dans le domaine spécifié et imprime un résultat qui vous permet de déterminer chaque partenaire. Cette commande inclut les ordinateurs qui ont été configurés manuellement.
 
 w32tm /monitor /domain:my_domain

À l’aide de la liste, vous pouvez suivre les résultats dans le domaine et comprendre la hiérarchie ainsi que le décalage de temps à chaque étape. En localisant le point où le décalage de temps est nettement pire, vous pouvez identifier la cause de l’heure incorrecte. À partir de là, vous pouvez essayer de comprendre pourquoi cette heure est incorrecte en activant la journalisation w32tm.

#### <a name="using-group-policy"></a>Utilisation de la stratégie de groupe
Vous pouvez utiliser une stratégie de groupe pour obtenir une plus grande précision, par exemple en demandant aux clients d’utiliser des serveurs NTP spécifiques ou de contrôler le mode de configuration des systèmes d’exploitation de bas niveau quand ils sont virtualisés. Vous trouverez ci-dessous une liste des scénarios possibles et des paramètres de stratégie de groupe pertinents :

**Domaines virtualisés** : afin de contrôler les contrôleurs de domaine virtualisés dans Windows 2012 R2 pour qu’ils synchronisent l’heure avec leur domaine, plutôt qu’avec l’hôte Hyper-V, vous pouvez désactiver cette entrée de Registre.  Pour le contrôleur de domaine principal, il est préférable de ne pas désactiver l’entrée, car l’hôte Hyper-V fournira la source de temps la plus stable. L’entrée de Registre nécessite le redémarrage du service w32time après sa modification.

 [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider] "Enabled"=dword:00000000

**Charges sensibles à la précision** : pour les charges de travail sensibles à la précision de l’heure, vous pouvez configurer des groupes d’ordinateurs afin de définir les serveurs NTP et les paramètres d’heure associés, comme les fréquences d’interrogation et de mise à jour de l’horloge. Cela est normalement géré par le domaine mais, pour plus de contrôle, vous pouvez cibler des machines spécifiques pour pointer directement vers l’horloge maître.

Paramètre de stratégie de groupe| Nouvelle valeur|
----- | ----- |
NtpServer| ClockMasterName,0x8|
MinPollInterval| 6 à 64 secondes|
MaxPollInterval| 6|
UpdateInterval| 100, une fois par seconde|
EventLogFlags| 3, journalisation de toutes les heures spéciales|

> [!NOTE]
> Les paramètres NtpServer et EventLogFlags se trouvent sous Système\Service de temps Windows\Fournisseurs de temps en utilisant les paramètres de Configurer le client NTP Windows. Les 3 autres sont situés sous Système\Service de temps Windows en utilisant les paramètres de configuration globale.

**Charges sensibles à la précision à distance** : pour les systèmes dans les domaines comme la vente au détail et le secteur des cartes de paiement, Windows utilise les informations de site actuelles et le localisateur de contrôleur de domaine pour trouver un contrôleur de domaine local, sauf si une source de temps NTP manuelle est configurée. Cet environnement nécessite une précision de 1 seconde, qui utilise une convergence plus rapide jusqu’à l’heure correcte. Cette option permet au service w32time de retarder l’heure de l’horloge. Si cela est acceptable et répond à vos besoins, vous pouvez créer la stratégie suivante.  Comme avec n’importe quel environnement, veillez à tester et à planifier votre réseau. 

Paramètre de stratégie de groupe| Nouvelle valeur|
----- | ----- |
MaxAllowedPhaseOffset| 1, si plus d’une seconde, affectez une heure correcte à l’horloge.|

Le paramètre MaxAllowedPhaseOffset est situé sous Système\Service de temps Windows en utilisant les paramètres de configuration globale.

> [!NOTE]
> Pour plus d’informations sur la stratégie de groupe et les entrées associées, consultez l’article [Paramètres et outils du service de temps Windows](windows-time-service-tools-and-settings.md) sur TechNet.

## <a name="azure-and-windows-iaas-considerations"></a>Éléments relatifs à Azure et IaaS Windows à prendre en considération

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Machine virtuelle Azure : Services de domaine Active Directory
Si la machine virtuelle Azure exécutant Active Directory Domain Services fait partie d’une forêt Active Directory locale existante, TimeSync (VMIC) doit être désactivé. Cela permet d’autoriser tous les contrôleurs de domaine de la forêt, physiques et virtuels, à utiliser une seule hiérarchie de synchronisation de l’heure. Reportez-vous au livre blanc sur les bonnes pratiques [« Exécution de contrôleurs de domaine dans Hyper-V »](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

### <a name="azure-virtual-machine-domain-joined-machine"></a>Machine virtuelle Azure : ordinateur joint à un domaine
Si vous hébergez un ordinateur qui est joint à un domaine dans une forêt Active Directory existante, virtuelle ou physique, il est judicieux de désactiver TimeSync pour l’invité et de vérifier que w32time est configuré pour se synchroniser avec son contrôleur de domaine via la configuration de l’heure pour Type = NTP5

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Machine virtuelle Azure : ordinateur de groupe de travail autonome
Si la machine virtuelle Azure n’est pas jointe à un domaine et qu’il ne s’agit pas d’un contrôleur de domaine, il est recommandé de conserver la configuration de l’heure par défaut et de synchroniser la machine virtuelle avec l’hôte.

## <a name="windows-application-requiring-accurate-time"></a>Application Windows nécessitant une heure exacte
### <a name="time-stamp-api"></a>API d’horodatage
Les programmes qui requièrent la plus grande précision en ce qui concerne l’heure UTC, et non la dimension temporelle, doivent utiliser l’[API GetSystemTimePreciseAsFileTime](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx). Cela garantit que votre application obtient l’heure système, qui est conditionnée par le service de temps Windows.

### <a name="udp-performance"></a>Performances UDP
Si vous disposez d’une application qui utilise la communication UDP pour les transactions et qu’il est important de réduire la latence, il existe des entrées de Registre associées que vous pouvez utiliser pour configurer une plage de ports à exclure du moteur de filtrage de base. Cela permet d’améliorer la latence et d’augmenter votre débit. Toutefois, les modifications apportées au Registre doivent être réservées aux administrateurs expérimentés. De plus, cette solution de contournement empêche la sécurisation des ports par le pare-feu. Pour plus d’informations, consultez la référence d’article ci-dessous.

Pour Windows Server 2012 et Windows Server 2008, vous devez d’abord installer un correctif. Vous pouvez vous référer à cet article de la Base de connaissances : [Perte du datagramme quand vous exécutez une application réceptrice de multidiffusion dans Windows 8 et Windows Server 2012](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>Mettre à jour les pilotes réseau
Certains fournisseurs réseau ont des mises à jour de pilotes qui améliorent les performances en ce qui concerne la latence des pilotes et la mise en mémoire tampon des paquets UDP. Contactez votre fournisseur réseau afin de savoir s’il existe des mises à jour pour optimiser le débit UDP.

## <a name="logging-for-auditing-purposes"></a>Journalisation à des fins d’audit
Pour vous conformer aux réglementations de suivi du temps, vous pouvez archiver manuellement les journaux w32tm, les journaux des événements et les informations de l’Analyseur de performances. Plus tard, les informations archivées peuvent être utilisées pour certifier la conformité à un moment précis dans le passé. Les facteurs suivants sont utilisés pour indiquer la précision.

1. Précision de l’horloge à l’aide du compteur Décalage de temps calculé de l’Analyseur de performances. L’horloge est affichée avec la précision souhaitée.
2. Source d’horloge à la recherche d’un message de type « Réponse du pair de » dans les journaux w32tm.  À la suite du texte du message figure l’adresse IP ou VMIC, qui décrit la source de temps et la suivante dans la chaîne des horloges de référence à valider.
3. État de la condition d’horloge avec les journaux w32tm pour valider l’occurrence de « ClockDispl Discipline: \*SKEW\*TIME\* ». Cela indique que w32tm est alors actif.

### <a name="event-logging"></a>Journalisation des événements
Pour avoir tous les éléments, vous aurez également besoin des informations du journal des événements. En recueillant le journal des événements système et en filtrant sur Time-Server, Microsoft-Windows-Kernel-Boot, Microsoft-Windows-Kernel-General, vous pourrez peut-être déterminer si d’autres influences ont changé l’heure, par exemple des tiers. Ces journaux peuvent être nécessaires pour éliminer les interférences externes. La stratégie de groupe peut affecter les journaux des événements qui sont écrits dans le journal. Pour plus d’informations, consultez la section ci-dessus, Utilisation de la stratégie de groupe.

### <a name="W32Logging"></a>Journalisation du débogage w32time
Pour activer w32tm à des fins d’audit, la commande suivante active la journalisation qui affiche les mises à jour périodiques de l’horloge et indique l’horloge source. Redémarrez le service pour activer la nouvelle journalisation. 

Pour plus d’informations, consultez [Guide pratique pour activer la journalisation du débogage dans le service de temps Windows](https://support.microsoft.com/kb/816043).

 w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>Analyseur de performances
Le service de temps Windows Server 2016 expose des compteurs de performances qui peuvent être utilisés pour collecter des données de journalisation à des fins d’audit. Celles-ci peuvent être journalisées localement ou à distance. Vous pouvez enregistrer les compteurs Décalage de temps calculé et Délai de l’aller-retour. Comme n’importe quel compteur de performances, vous pouvez les superviser à distance et créer des alertes à l’aide de System Center Operations Manager. Vous pouvez, par exemple, utiliser une alerte pour vous avertir lorsque le décalage de temps dérive de la précision souhaitée. Le [pack d’administration System Center](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx) contient plus d’informations.

### <a name="windows-traceability-example"></a>Exemple de traçabilité Windows
À partir de fichiers journaux w32tm, vous souhaitez valider deux informations. La première indique que le fichier journal est actuellement une horloge de condition. Cela prouve que votre horloge était conditionnée par le service de temps Windows à l’heure contestée.

 151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307 151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41 151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

Le point essentiel est que vous voyez des messages précédés de ClockDispln Discipline, ce qui prouve que w32time interagit avec l’horloge système.
 
Vous devez ensuite rechercher le dernier rapport dans le journal avant l’heure contestée qui signale l’ordinateur source actuellement utilisé comme horloge de référence. Il peut s’agir d’une adresse IP, d’un nom d’ordinateur ou du fournisseur VMIC, qui indique une synchronisation avec l’hôte pour Hyper-V. L’exemple suivant fournit une adresse IPv4 de 10.197.216.105.

 151802 20:18:54.6531515s - Réponse du pair 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

Maintenant que vous avez validé le premier système dans la chaîne de temps de référence, vous devez examiner le fichier journal sur la source de temps de référence et répéter la même procédure. Ce processus se poursuit jusqu’à ce que vous accédiez à une horloge physique telle que GPS ou une source de temps connue comme NIST. Si l’horloge de référence est un matériel GPS, les journaux du fabricant peuvent également être requis.

## <a name="network-considerations"></a>Éléments relatifs au réseau à prendre en considération
Les algorithmes de protocole NTP dépendent de la symétrie de votre réseau. À mesure que vous augmentez le nombre de sauts réseau, la probabilité d’asymétrie augmente. Il est par conséquent difficile de prédire les types de précisions que vous verrez dans vos environnements spécifiques. 

L’Analyseur de performances et les nouveaux compteurs de temps Windows dans Windows Server 2016 peuvent être utilisés pour évaluer la précision de vos environnements et créer des bases de référence. De plus, vous pouvez effectuer des dépannages pour déterminer le décalage actuel de n’importe quel ordinateur de votre réseau.

Il existe deux normes générales pour obtenir une heure exacte sur le réseau. Le protocole PTP ([Precision Time Protocol - IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) a des exigences plus strictes en matière d’infrastructure réseau, mais peut souvent fournir une précision sous la microseconde. Le protocole NTP ([Network Time Protocol – RFC 1305](https://tools.ietf.org/html/rfc1305)) fonctionne sur un grand nombre de réseaux et d’environnements, ce qui en facilite la gestion. 

Windows prend en charge le protocole NTP simple (RFC2030) par défaut pour les ordinateurs non joints à un domaine. Pour les ordinateurs joints à un domaine, nous utilisons un protocole NTP sécurisé appelé [MS-SNTP](https://msdn.microsoft.com/library/cc246877.aspx), qui tire parti des secrets négociés par domaine qui fournissent un avantage de gestion par rapport au protocole NTP authentifié décrit dans RFC1305 et RFC5905.  

Les protocoles joints et non joints à un domaine nécessitent tous deux le port UDP 123. Pour plus d’informations sur les bonnes pratiques NTP, reportez-vous au [brouillon IETF Network Time Protocol Best Current Practices](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00).

### <a name="reliable-hardware-clock-rtc"></a>Horloge matérielle fiable (RTC)
Windows n’arrête pas le temps, sauf si certaines limites sont dépassées, mais contrôle strictement l’horloge. Cela signifie que w32tm ajuste la fréquence de l’horloge à intervalles réguliers, en utilisant le paramètre Fréquence de mise à jour de l’horloge, qui prend la valeur par défaut d’une fois par seconde avec Windows Server 2016. Si l’horloge retarde, elle accélère la fréquence et, si elle avance, elle ralentit la fréquence. Toutefois, pendant ce délai entre les ajustements de fréquence d’horloge, l’horloge matérielle a le contrôle. En cas de problème avec le microprogramme ou l’horloge matérielle, l’heure de l’ordinateur peut devenir moins précise.

Il s’agit d’une autre raison pour laquelle vous devez tester et planifier votre environnement. Si le compteur de performances « Décalage de temps calculé » ne se stabilise pas à la précision que vous ciblez, vous pouvez vérifier que le microprogramme est à jour. En guise d’autre test, vous pouvez voir si le matériel en double reproduit le même problème.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>Résolution des problèmes de précision de l’heure et NTP
Vous pouvez utiliser la section Découverte de la hiérarchie ci-dessus pour comprendre la source de l’inexactitude de l’heure. En examinant le décalage de temps, recherchez le point dans la hiérarchie où l’heure diverge le plus de sa source NTP. Une fois que vous avez compris la hiérarchie, vous allez essayer de comprendre pourquoi cette source de temps particulière ne reçoit pas une heure exacte. 

En vous concentrant sur le système avec une heure divergente, vous pouvez utiliser les outils ci-dessous pour recueillir des informations supplémentaires qui vous aideront à identifier le problème et à trouver une solution. La référence UpstreamClockSource ci-dessous est l’horloge découverte à l’aide de « w32tm /config /status ».


- Journaux des événements système
- Activez la journalisation avec : journaux w32tm - w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-300
- Clé de Registre w32tm HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- Traces de réseau local
- Compteurs de performances (de l’ordinateur local ou l’horloge UpstreamClockSource)
- W32tm /stripchart /computer:UpstreamClockSource
- Test PING sur UpstreamClockSource pour comprendre la latence et le nombre de sauts jusqu’à la source
- Tracert UpstreamClockSource

Problème| Symptômes| Solution|
----- | ----- | ----- |
| L’horloge TSC locale n’est pas stable.| Utilisation de l’Analyseur de performances - Ordinateur physique : synchronisation de l’horloge stable, mais vous voyez toujours ce message toutes les 1 à 2 minutes de plusieurs 100 μs. | La mise à jour du microprogramme ou la validation d’un matériel différent n’affiche pas le même problème.|
| Latence du réseau| Le graphique à bandes w32tm affiche une valeur RoundTripDelay de plus de 10 ms. La variation du délai génère un bruit équivalent à la moitié de la durée du parcours circulaire, par exemple un délai dans une seule direction.<br><br>UpstreamClockSource comporte plusieurs sauts, comme indiqué par le test PING. La durée de vie doit être proche de 128.<br><br>Utilisez Tracert pour rechercher la latence au niveau de chaque saut.  | Trouvez une source d’horloge plus proche pour l’heure. Une solution consiste à installer une horloge source sur le même segment ou à pointer manuellement vers l’horloge source qui est géographiquement plus proche. Pour un scénario de domaine, ajoutez un ordinateur avec le rôle GTimeServ. | 
Impossible d’atteindre la source NTP de manière fiable| W32tm /stripchart retourne par intermittence « Délai d’attente de la demande dépassé » |La source NTP ne répond pas|
La source NTP ne répond pas| Vérifiez les compteurs de l’Analyseur de performances pour Nombre de sources de temps de client NTP, Requêtes entrantes de serveur NTP, Réponses sortantes de serveur NTP et déterminez votre utilisation par rapport à vos bases de référence.| À l’aide des compteurs de performances du serveur, déterminez si la charge a changé par rapport à vos bases de référence.<br><br>Existe-t-il des problèmes de surcharge du réseau ?|
Le contrôleur de domaine n’utilise pas l’horloge la plus précise| Modifications de la topologie ou de l’horloge maître récemment ajoutée.| w32tm /resync /rediscover|
Les horloges clientes dérivent| Événement du service de temps 36 dans le journal des événements système et/ou dans le texte du fichier journal décrivant : Le compteur « Nombre de sources de temps de client NTP » passe de 1 à 0|Résolvez les problèmes de la source amont et voyez si elle rencontre des problèmes de performances.|

### <a name="baselining-time"></a>Heure de la planification
La planification est importante afin que vous puissiez d’abord comprendre les performances et la précision de votre réseau, puis les comparer à la base de référence ultérieurement quand des problèmes surviennent. Vous allez planifier le contrôleur de domaine principal racine ou n’importe quel ordinateur marqué avec GTIMESRV. Nous vous recommandons également de planifier le contrôleur de domaine principal dans chaque forêt. Enfin, choisissez les contrôleurs de domaine ou ordinateurs essentiels qui présentent des caractéristiques intéressantes, comme la distance ou les charges élevées, et planifiez-les.

Il est également utile de planifier Windows Server 2016 par rapport à 2012 R2. Toutefois, vous ne disposez de w32tm/stripchart qu’en tant qu’outil de comparaison, puisque Windows Server 2012 R2 n’a pas de compteurs de performances. Vous devez choisir deux ordinateurs ayant les mêmes caractéristiques, ou mettre à niveau un ordinateur et comparer les résultats après la mise à jour. L’addendum Mesures de temps Windows contient plus d’informations sur la façon d’effectuer des mesures détaillées entre 2016 et 2012.

À l’aide de tous les compteurs de performances w32time, collectez des données pendant au moins une semaine. Cela vous permet de vous assurer que vous disposez d’une référence suffisante pour en représenter plusieurs dans le réseau au fil du temps et d’une exécution suffisante pour garantir la stabilité de la précision de l’heure.

### <a name="ntp-server-redundancy"></a>Redondance des serveurs NTP
Pour une configuration de serveur NTP manuelle utilisée avec des ordinateurs non joints à un domaine ou le contrôleur de domaine principal, le fait de disposer de plusieurs serveurs est une bonne mesure de redondance en cas de disponibilité. Elle peut également offrir une meilleure précision, en supposant que toutes les sources sont exactes et stables. Toutefois, si la topologie n’est pas bien conçue, ou si les sources de temps ne sont pas stables, la précision résultante peut être pire. Procédez donc avec prudence. La limite des serveurs de temps pris en charge que w32time peut référencer manuellement est 10. 

## <a name="leap-seconds"></a>Secondes intercalaires
La période de rotation de la terre varie dans le temps, en raison des événements climatiques et géologiques. En règle générale, la variation est d’environ une seconde tous les deux ans. Chaque fois que la variation par rapport à l’heure atomique devient trop importante, une correction d’une seconde (ajoutée ou retranchée) est insérée, appelée seconde intercalaire. Cette opération est effectuée de manière à ce que la différence ne dépasse jamais 0,9 secondes. Cette correction est annoncée six mois avant la correction réelle. Avant Windows Server 2016, le service de temps Microsoft ne prenait pas en charge les secondes intercalaires, mais comptait sur le service de temps externe pour s’en occuper. Avec l’amélioration de la précision de l’heure de Windows Server 2016, Microsoft travaille sur une solution plus appropriée pour le problème de seconde intercalaire.

## <a name="secure-time-seeding"></a>Secure Time Seeding
W32time dans Windows Server 2016 comprend la fonctionnalité Secure Time Seeding. Cette fonctionnalité détermine l’heure actuelle approximative à partir des connexions SSL sortantes. Cette valeur de temps est utilisée pour superviser l’horloge système locale et corriger les erreurs grossières. Pour plus d’informations sur cette fonctionnalité, consultez [ce billet de blog](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/). Dans les déploiements avec une ou plusieurs sources de temps fiables et des ordinateurs correctement supervisés qui incluent la surveillance des décalages de temps, vous pouvez choisir de ne pas utiliser la fonctionnalité Secure Time Seeding et de vous appuyer sur votre infrastructure existante à la place. 

Vous pouvez désactiver la fonctionnalité en procédant comme suit :

1. Définissez la valeur de configuration du Registre UtilizeSSLTimeData sur 0 sur un ordinateur spécifique :

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f

2. Si vous ne parvenez pas à redémarrer l’ordinateur immédiatement pour une raison quelconque, vous pouvez informer le service w32time de la mise à jour de la configuration. Cela arrête la supervision et l’application de l’heure en fonction des données de temps collectées à partir des connexions SSL.

    W32tm.exe /config /update

3. Le redémarrage de l’ordinateur rend le paramètre immédiatement effectif et l’amène également à arrêter la collecte des données de temps à partir des connexions SSL. La dernière partie présente une très légère surcharge et ne devrait pas constituer un problème de performances.

4. Pour appliquer ce paramètre dans l’ensemble d’un domaine, définissez la valeur UtilizeSSLTimeData dans le paramètre de stratégie de groupe w32time sur 0 et publiez le paramètre. Lorsque le paramètre est sélectionné par un client de stratégie de groupe, le service w32time est averti et arrête la supervision et l’application de l’heure à l’aide des données de temps SSL. La collecte des données de temps SSL s’arrête au redémarrage de chaque ordinateur. Si votre domaine contient des tablettes/ordinateurs portables et d’autres appareils, vous pouvez les exclure de cette modification de stratégie. La batterie de ces appareils finira par être épuisée et ils auront besoin de la fonctionnalité Secure Time Seeding pour amorcer leur heure.
