

## <a name="windows-server-2016-improvements"></a>Améliorations de Windows Server 2016
### <a name="windows-time-service-and-ntp"></a>NTP et le Service de temps Windows
Windows Server 2016 a amélioré les algorithmes, qu'il utilise pour corriger le temps et la condition de l’horloge locale à synchroniser avec l’heure UTC.  NTP utilise 4 valeurs pour calculer le décalage horaire, selon les horodatages de la demande/réponse du client et demande/réponse du serveur.  Toutefois, les réseaux sont bruyants et il peut y avoir des pics dans les données à partir de NTP en raison de la congestion du réseau et d’autres facteurs qui affectent la latence du réseau.  Algorithmes de Windows 2016 moyenne ce bruit à l’aide d’un nombre de techniques différentes, ce qui entraîne une horloge stable et précise.  En outre, la source de nous utilisons pour les références de l’heure précise une API améliorée, ce qui nous donne une meilleure résolution.  Grâce à ces améliorations, nous sommes en mesure d’atteindre 1 précision de ms en ce qui concerne l’heure UTC dans un domaine.

### <a name="hyper-v"></a>Hyper-V
Windows 2016 a amélioré le service Hyper-V TimeSync. Améliorations incluent la durée initiale plus précise sur le début de la machine virtuelle ou de restauration de la machine virtuelle et de correction de latence d’interruption pour les exemples fournis pour w32time.  Cette amélioration permet de rester 10µs avec sorti de l’hôte avec un RMS (racine signifie carré, ce qui indique l’écart), de 50µs, même sur un ordinateur avec une charge de 75 %. Pour plus d’informations, consultez [architecture Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx).

> [!NOTE]
> Charge a été créé à l’aide de banc d’essai prime95 à l’aide du profil à charge équilibrée.

En outre, le niveau de niveau que l’hôte signale à l’invité est plus transparent.  Précédemment, l’hôte présentera un fixe de niveau 2, quelle que soit sa précision.  Avec les modifications apportées dans Windows Server 2016, l’hôte signale une couche supérieure de la couche hôte, ce qui entraîne le meilleur moment pour invités virtuels.  Le niveau de l’hôte est déterminée par w32time via des moyens normaux en fonction de son heure de la source.  Domaine joint 2016 Windows invités trouveront l’horloge plus précise, au lieu d’utilisation de l’hôte par défaut.  Il était pour cette raison que nous avons conseillé de désactiver manuellement le paramètre de fournisseur de temps de Hyper-V pour les ordinateurs participant à un domaine dans Windows 2012R2 et en dessous.

### <a name="monitoring"></a>Surveillance
Compteurs Analyseur de performances ont été ajoutées.  Ceux-ci vous permettent de ligne de base, surveiller et résoudre les problèmes de précision du temps.  Ces compteurs sont les suivantes :

Compteur|Description|
----- | ----- |
Calculée du décalage horaire|   Le décalage entre l’horloge système et de la source de temps choisi, calculées par W32Time Service (en microsecondes) de temps absolu. Lorsqu’un nouvel échantillon valid est disponible, l’heure calculée est mis à jour avec le décalage horaire indiqué par l’exemple. Il s’agit de l’offset de l’heure réelle de l’horloge locale. W32Time lance la correction de l’horloge à l’aide de ce décalage et met à jour de l’heure calculée entre les échantillons avec le décalage horaire restants qui doit être appliquée à l’horloge locale. Précision de l’horloge peut être suivie à l’aide de ce compteur de performance avec une faible fréquence d’interrogation (par exemple : 256 secondes ou moins) et en recherchant la valeur du compteur soit inférieure à la limite de précision d’horloge de votre choix.|
Ajustement de fréquence d’horloge| L’ajustement de fréquence d’horloge absolu effectuée à l’horloge système local par W32Time en parties par milliard. Ce compteur vous permet de visualiser les actions prises par W32time.|
Délai de l’aller-retour NTP|    Délai aller-retour plus récente rencontrée par le Client NTP de recevoir une réponse à partir du serveur (en microsecondes). Il s’agit du temps écoulé sur le client NTP entre la transmission d’une demande sur le serveur NTP et recevoir une réponse valide à partir du serveur. Ce compteur permet de caractériser les retards rencontrés par le client NTP. Agrandir ou différents de boucles peuvent ajouter du bruit à calculs de temps NTP, qui à son tour peuvent affecter la précision de la synchronisation horaire via NTP.|
Nombre de sources du Client NTP|    Nombre de sources de temps NTP utilisé par le Client NTP. Il s’agit d’un nombre d’adresses IP actives distincts de serveurs de temps qui répondent aux requêtes de ce client. Ce nombre peut être supérieure ou inférieure aux homologues configurés, en fonction de la résolution DNS des noms d’homologues et capacité de portée actuelle.|
Serveur NTP les demandes entrantes|   Nombre de demandes reçues par le serveur NTP (demandes/s).|
Réponses sortantes du serveur NTP|  Nombre de demandes traitées par le serveur NTP (réponses/s).|

Les 3 premiers compteurs conviennent à des scénarios de résolution des problèmes de précision.  La précision du temps de résolution des problèmes et NTP section ci-dessous, sous [meilleures pratiques](#BestPractices), comporte plus de détails.
Les 3 derniers compteurs couvrent les scénarios de serveur NTP et sont utile quand déterminer la charge et la ligne de base, vos performances en cours.

### <a name="configuration-updates-per-environment"></a>Mises à jour de la configuration par environnement
La section suivante décrit les modifications apportées à la configuration par défaut entre Windows 2016 et les versions précédentes pour chaque rôle.  Les paramètres de Windows Server 2016 et de mise à jour anniversaire Windows 10 (build 14393), sont désormais unique c’est pourquoi il sont affichés en tant que colonnes distinctes. 

|Rôle|Paramètre|Windows Server 2016|Windows 10|Windows Server 2012 R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**Autonome/Nano Server**||||
| |*Serveur de temps*|time.windows.com|N/A|time.windows.com|
| |*Fréquence d’interrogation*|64 - 1024 secondes|N/A|Une fois par semaine|
| |*Fréquence de mise à jour d’horloge*|Une fois par seconde|N/A|Une fois par heure|
|**Client autonome**||||
| |*Serveur de temps*|N/A|time.windows.com|time.windows.com|
| |*Fréquence d’interrogation*|N/A|Une fois par jour|Une fois par semaine|
| |*Fréquence de mise à jour d’horloge*|N/A|Une fois par jour|Une fois par semaine|
|**Contrôleur de domaine**||||
| |*Serveur de temps*|PDC/GTIMESERV|N/A|PDC/GTIMESERV|
| |*Fréquence d’interrogation*|64-1024 secondes|N/A|1024 - 32768 secondes|
| |*Fréquence de mise à jour d’horloge*|Une fois par jour|N/A|Une fois par semaine|
|**Serveur membre de domaine**||||
| |*Serveur de temps*|DC|N/A|DC|
| |*Fréquence d’interrogation*|64-1024 secondes|N/A|1024 - 32768 secondes|
| |*Fréquence de mise à jour d’horloge*|Une fois par seconde|N/A|Une fois toutes les 5 minutes|
|**Client membre du domaine**||||
| |*Serveur de temps*|N/A|DC|DC|
| |*Fréquence d’interrogation*|N/A|1204 - 32768 secondes|1024 - 32768 secondes|
| |*Fréquence de mise à jour d’horloge*|N/A|Une fois toutes les 5 minutes|Une fois toutes les 5 minutes|
|**Invité Hyper-V**||||
| |*Serveur de temps*|Choisit la meilleure option selon le niveau du serveur hôte et l’heure|Choisit la meilleure option selon le niveau du serveur hôte et l’heure|Valeurs par défaut à l’hôte|
| |*Fréquence d’interrogation*|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|
| |*Fréquence de mise à jour d’horloge*|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|En fonction du rôle ci-dessus|

>[!NOTE]
>Pour Linux dans Hyper-V, consultez le [Linux de ce qui permet d’utiliser Hyper-V hôte délai](#AllowingLinux) section ci-dessous.

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impact de l’augmentation de l’interrogation et la fréquence de mise à jour d’horloge
Afin de fournir l’heure la plus précise, les valeurs par défaut pour l’interrogation des fréquences et les mises à jour de l’horloge sont augmentées qui nous permet d’apporter de légères modifications plus fréquemment.  Cela entraîne davantage de trafic UDP/NTP, toutefois, ces paquets sont petits devrait donc très peu ou pas d’impact sur les liaisons haut débit. L’avantage, toutefois, est que l’heure doit être mieux sur une plus grande variété de matériel et d’environnements.

Pour les périphériques de sauvegarde sur batterie, augmenter la fréquence d’interrogation peut entraîner des problèmes.  Appareils de la batterie ne stockez pas le moment lorsqu’ils sont mis hors tension.  Lors de leur reprise, elle peut nécessiter des corrections fréquentes à l’horloge.  Augmentation de la fréquence d’interrogation entraîne l’horloge instabilité du système et peut également utiliser davantage de puissance.  Microsoft recommande que vous ne modifiez pas les paramètres par défaut du client.

Contrôleurs de domaine doivent être affectées au minimum, même avec l’effet multiplié les mises à jour accrue des Clients NTP dans un domaine Active Directory.  NTP a une quantité inférieure la consommation des ressources par rapport à d’autres protocoles et un impact marginal.  Vous êtes plus susceptible d’atteindre les limites pour les autres fonctionnalités de domaine avant a un impact sur les paramètres augmentés pour Windows Server 2016.  Active Directory utilise NTP sécurisé, ce qui tend à synchroniser fois moins précise que NTP simple, mais nous avons vérifié qu’il monte en puissance à niveau les clients deux en dehors de contrôleur de domaine principal.

En tant que conservateur plan, vous devez réserver 100 demandes NTP par seconde par cœur.  Par exemple, un domaine est composé de 4 contrôleurs de domaine avec 4 cœurs, vous devez être en mesure de traiter les demandes NTP 1600 par seconde.  Si vous disposez de 10 k clients configurés pour synchroniser l’heure toutes les 64 secondes, et les demandes sont reçus de manière uniforme au fil du temps, vous voyez 10 000/64, ou environ 160 demandes/seconde, réparties sur tous les contrôleurs de domaine.  Cela relève facilement notre 1600 NTP demandes par seconde en fonction de cet exemple.  Celles-ci sont conservatrices recommandations de planification et que vous ont bien entendu une dépendance volumineuse sur votre réseau, les vitesses de processeur et les charges, par conséquent, comme toujours de base et de test dans vos environnements.

Il est également important de noter que si vos contrôleurs de domaine sont en cours d’exécution avec une charge considérable de processeur, supérieure à 40 %, cela sera certainement ajouter du bruit aux réponses NTP et réduire la précision de votre temps dans votre domaine.  Là encore, vous devez tester dans votre environnement pour comprendre les résultats réels.

## <a name="time-accuracy-measurements"></a>Mesures de précision
### <a name="methodology"></a>Méthodologie
Pour mesurer la précision du temps pour Windows Server 2016, nous avons utilisé une variété d’outils, les méthodes et les environnements.  Vous pouvez utiliser ces techniques pour mesurer et d’optimiser votre environnement et de déterminer si les résultats de précision répondent à vos besoins. 

L’horloge de source de notre domaine est composé de deux serveurs NTP de haute précision avec un matériel GPS.  Aussi, nous avons utilisé un ordinateur de test de référence distincte pour les mesures, qui devait également installé à partir d’un autre fabricant de matériel GPS de haute précision.  Pour certains des tests, vous aurez besoin d’une source de horloge précises et fiables à utiliser en tant que référence en plus de votre source d’horloge de domaine.

Nous avons utilisé des quatre méthodes différentes pour mesurer la précision avec des ordinateurs virtuels et physiques. Plusieurs méthodes fourni un moyen indépendant pour valider les résultats.


1. Mesurer l’horloge locale, qui est conditionnée par w32tm, par rapport à notre ordinateur de test de référence qui a un matériel distinct GPS.  
2.  Pings NTP de mesure à partir du serveur NTP aux clients à l’aide de W32tm « stripchart »
3.  Pings NTP de mesure à partir du client sur le serveur NTP à l’aide de W32tm « stripchart »
4.  Résultats de la mesure Hyper-V à partir de l’hôte à l’invité avec le compteur d’horodatage temps (TSC).  Ce compteur est partagé entre les partitions et l’heure système dans les deux partitions.  Nous avons calculé la différence entre l’heure de l’hôte et la durée du client dans la machine virtuelle.  Ensuite, nous utilisons l’horloge TSC pour interpoler le temps de l’hôte de l’invité, étant donné que les mesures ne se produisent en même temps.  En outre, nous utilisons le facteur d’horloge TSV out des retards et la latence dans l’API.

W32tm est intégrée, mais les autres outils que nous avons utilisé au cours de nos tests sont disponibles pour le référentiel Microsoft sur GitHub en open source pour les tests et l’utilisation.  Le WIKI sur le référentiel comporte des informations supplémentaires décrivant comment utiliser les outils pour effectuer des mesures.

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

Vous trouverez ci-dessous les résultats des tests sont un sous-ensemble des mesures que nous avons apporté dans un des environnements de test.  Elles illustrent la précision maintenue au début de la hiérarchie de temps et le client du domaine enfant à la fin de la hiérarchie de temps.  Cela est comparée aux ordinateurs mêmes dans une topologie de 2012 en fonction de comparaison.

### <a name="topology"></a>Topologie
Pour la comparaison, nous avons testé un Windows Server 2012 R2 et la topologie de base Windows Server 2016.  Les deux topologies sont constituées de deux ordinateurs hôtes Hyper-V physiques qui font référence à un ordinateur Windows Server 2016 avec GPS horloge matériel installé.  Chaque hôte exécute 3 invités joints à un domaine windows, qui sont organisées en fonction de la topologie suivante.  Les lignes représentent la hiérarchie de temps et le protocole/transport qui est utilisé.

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Vue d’ensemble de résultats de graphique
Les deux graphiques suivantes représentent la précision du temps pour deux membres spécifiques dans un domaine basé sur la topologie ci-dessus.  Chaque graphique affiche le Windows Server 2012 R2 et 2016 résultats superposées, qui illustre les améliorations visuellement.  La précision a été mesurer à partir de l’ordinateur invité par rapport à l’hôte.  Les données du graphiques représente un sous-ensemble de l’ensemble des tests, que nous l’avons fait et présente le meilleur des cas et le pire des cas.  

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Performances du contrôleur de domaine principal du domaine racine
Le contrôleur de domaine principal racine est synchronisé à l’hôte Hyper-V (à l’aide de VMIC) qui est un serveur Windows Server 2016 avec le matériel GPS qui s’avère être précises et stable.  Il s’agit d’une condition essentielle pour le 1 exactitude ms, ce qui est indiqué en tant que la zone ombrée vert.

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Performances du Client de domaine enfant
Le Client de domaine enfant est attaché à un contrôleur de domaine enfant principal qui communique avec le contrôleur de domaine principal racine.  Date et heure est également dans le 1 ms exigence.

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>Test de longues distances
Le tableau suivant compare le tronçon de réseau virtuel 1 à 6 tronçons réseau physique avec Windows Server 2016.  Deux graphiques sont superposés sur eux avec la transparence pour afficher les données qui se chevauche.  Les tronçons réseau croissant signifient une latence plus élevée et les écarts de temps supérieure.  Le graphique est agrandie et c’est le cas le 1 ms limites, représentés par la zone verte, est supérieure.  Comme vous pouvez le voir, l’heure est toujours au sein de 1 ms avec plusieurs sauts.  Est négatif déplacée, qui montre une asymétrie de réseau.  Bien sûr, chaque réseau est différent, et des mesures dépendent d’une multitude de facteurs environnementaux.

![Horloge Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>Meilleures pratiques pour la synchronisation d’horloge précise
### <a name="solid-source-clock"></a>Horloge de la Source de Solid
Une fois les machines n’est pas aussi bien que l’horloge de la source avec qu'il se synchronise.  Pour atteindre 1 ms de précision, vous devez les matériel GPS ou une appliance de temps sur votre réseau en tant que l’horloge de la source principale de la référence.  À l’aide de la valeur par défaut time.windows.com, peut ne pas fournir un stable et une heure locale.  En outre, à mesure que vous éloignent l’horloge de la source, le réseau affecte la précision.  Avoir une horloge source maître dans chaque data center est requise pour le niveau de précision.

### <a name="hardware-gps-options"></a>Options de matériel GPS
Il existe diverses solutions de matériel que peuvent offrir l’heure précise.  En règle générale, les solutions aujourd'hui reposent sur les antennes GPS.  Il existe également des cases d’option et les solutions de modem d’accès à distance à l’aide de lignes dédiées.  Ils attachement à votre réseau, soit comme une appliance ou branchez sur un PC, par exemple Windows via un périphérique de PCIe ou USB.  Différentes options fournira à différents niveaux de précision, et comme toujours, les résultats dépendent de votre environnement.  Les variables qui affectent la précision incluent la disponibilité, de stabilité de réseau et de charge et de matériel PC GPS.  Il s’agit des facteurs importants lors du choix d’une horloge de la source, comme nous l’avons indiqué, est requis pour la durée stable et précise.

### <a name="domain-and-synchronizing-time"></a>Domaine et synchroniser l’heure
Membres du domaine utilisent la hiérarchie de domaine pour déterminer quel ordinateur qu’ils utilisent en tant que source pour synchroniser l’heure.  Chaque membre de domaine se trouve à un autre ordinateur à synchroniser avec et enregistrez-le en tant que source de son horloge.  Chaque type de membre de domaine suit un autre ensemble de règles afin de trouver une source d’horloge pour synchronisation date / heure.  Le contrôleur de domaine principal à la racine de forêt est la source d’horloge par défaut pour tous les domaines.  Vous trouverez ci-dessous différents rôles et la description générale de comment ils pour rechercher une source :


- **Contrôleur de domaine avec le rôle de contrôleur de domaine principal** – cet ordinateur est la source de temps faisant autorité pour un domaine. Il aura l’heure la plus précise dans le domaine et doit synchroniser avec un contrôleur de domaine dans le domaine parent, sauf dans les cas où [GTIMESERV](#GTIMESERV) rôle est activé. 
- **Tout autre contrôleur de domaine** – cette machine agit comme une source de temps pour les clients et serveurs membres dans le domaine. Un contrôleur de domaine peut se synchroniser avec le contrôleur de domaine principal de son propre domaine, ou n’importe quel contrôleur de domaine dans son domaine parent.
- **Les serveurs clients/membres** – cet ordinateur peut se synchroniser avec n’importe quel contrôleur de domaine ou le contrôleur de domaine principal de son propre domaine, ou un contrôleur de domaine ou contrôleur de domaine principal dans le domaine parent.

Selon les candidats disponibles, un système de calcul de score est utilisé pour trouver la meilleure source de temps.  Ce système prend en compte la fiabilité de la source de temps et de son emplacement relatif.  Cela se produit une fois lorsque l’heure est le service a démarré.  Si vous devez disposer d’un contrôle plus précis de la synchronisation de temps, vous pouvez ajouter des serveurs de temps utile dans des emplacements spécifiques ou intégrer la redondance.  Consultez le [spécifier un Local fiable temps Service à l’aide de GTIMESERV](#GTIMESERV) section pour plus d’informations.

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Environnements de système d’exploitation mixtes (Win2012R2 et Win2008R2)
Un environnement de domaine Windows Server 2016 pur est obligatoire pour le niveau de précision, il existe toujours avantages dans un environnement mixte.  Déploiement de Windows Server 2016 Hyper-V dans un domaine Windows 2012 bénéficieront les invités en raison des améliorations que nous l’avons mentionné ci-dessus, mais uniquement si les invités sont également Windows Server 2016.  Un contrôleur principal de Windows Server 2016, sera en mesure de livrer heure la plus précise en raison des algorithmes améliorés qui sera une source plus stable.  En tant que remplacement de votre contrôleur de domaine principal est peut-être pas une option, vous pouvez ajouter à la place d’un contrôleur de domaine Windows Server 2016 avec le [GTIMESERV](#GTIMESERV) jeu qui serait une mise à niveau de précision pour votre domaine de restauration par.  Un contrôleur de domaine Windows Server 2016 peut fournir une meilleure heure pour les clients en aval, toutefois, il est uniquement aussi bon que son temps NTP de source.

Également, comme indiqué ci-dessus, les fréquences d’interrogation et l’actualisation de l’horloge ont été modifiés avec Windows Server 2016.  Il peuvent être modifiés manuellement pour vos contrôleurs de domaine de bas niveau ou appliquées via la stratégie de groupe.  Pendant que nous n’avons pas testé ces configurations, elles doivent se comportent correctement dans Win2008R2 et Win2012R2 et fournir certains avantages.

Versions antérieures à Windows Server 2016 avait un plusieurs problèmes maintenant l’heure exacte en conservant ce qui a entraîné l’heure système dérivantes immédiatement après un ajustement.  Pour cette raison, obtenir des échantillons de temps à partir d’une source NTP précise fréquemment et de l’horloge locale avec les données de conditionnement conduit à des dérives de plus petits dans leurs horloges système dans la période intra-échantillonnage, ce qui entraîne un meilleur temps de conservation sur les versions de système d’exploitation de bas niveau. La meilleure précision observée lors environ 5 ms un Client de NTP Windows Server 2012 R2, configuré avec les paramètres d’analyse de haute précision, synchronisé son temps à partir d’un serveur Windows 2016 NTP précis.

Dans certains scénarios impliquant des contrôleurs de domaine invité, Hyper-V TimeSync exemples peuvent interrompre synchronisation date / heure domaine.  Cela ne devrait plus être un problème pour les visiteurs Server 2016 s’exécutant sur des ordinateurs hôtes Server 2016 Hyper-V.

Pour désactiver le service Hyper-V TimeSync de fournir des exemples pour w32time, définissez la clé de Registre suivante invité :

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>Ce qui permet de Linux pour utiliser le temps d’hôte Hyper-V
Pour les invités Linux en cours d’exécution dans Hyper-V, les clients sont généralement configurés pour utiliser le démon NTP pour la synchronisation de l’heure par rapport à des serveurs NTP.  Si la distribution Linux prend en charge le protocole de la version 4 TimeSync et l’invité Linux a activé le service d’intégration TimeSync, il se synchronisera par rapport à l’heure de l’hôte. Cela peut entraîner l’incohérent perdre du temps si les deux méthodes sont activées.

Pour synchroniser exclusivement à la durée de l’hôte, il est recommandé pour désactiver la synchronisation de temps NTP à l’aide :

- La désactivation de tous les serveurs NTP dans le fichier ntp.conf
- ou la désactivation du démon NTP

Dans cette configuration, le paramètre de serveur de temps est cet hôte.  Sa fréquence d’interrogation est de 5 secondes et la fréquence de mise à jour d’horloge est également 5 secondes.

Pour synchroniser exclusivement sur NTP, il est recommandé de désactiver le service d’intégration TimeSync dans l’invité.

> [!NOTE]
> Remarque:  Prise en charge pour une heure précise avec les invités Linux nécessite une fonctionnalité qui est uniquement pris en charge dans les derniers noyaux Linux en amont et il n’est pas quelque chose qui est largement disponible sur toutes les distributions de Linux. Veuillez vous reporter [pris en charge de Linux et FreeBSD machines virtuelles pour Hyper-V sur Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows) pour plus d’informations sur les distributions de prise en charge.

#### <a name="GTIMESERV"></a>Spécifiez un Service de temps fiable Local à l’aide de GTIMESERV
Vous pouvez spécifier un ou plusieurs contrôleurs de domaine en tant que les horloges source précis à l’aide de la GTIMESERV, bon serveur de temps, d’indicateurs.  Par exemple, les contrôleurs de domaine spécifiques équipés de matériel GPS peuvent être marqués comme un GTIMESERV.  Cela garantira que votre domaine fait référence à une horloge en fonction du matériel GPS.

> [!NOTE]
> Vous trouverez plus d’informations sur les indicateurs de domaine dans le [documentation du protocole MS-ADTS](https://msdn.microsoft.com/library/mt226583.aspx).

TIMESERV est un autre connexes domaine Services indicateur qui détermine si un ordinateur est actuellement faisant autorité, qui peuvent changer si un contrôleur de domaine perd la connexion.  Un contrôleur de domaine dans cet état retourne « Inconnu de niveau » lorsque l’interrogé via NTP.  Après avoir essayé plusieurs fois, le contrôleur de domaine enregistrera 36 d’événement système événement Service de temps.

Si vous souhaitez configurer un contrôleur de domaine en tant qu’un GTIMESERV, cela peut être configuré manuellement à l’aide de la commande suivante.  Dans ce cas le contrôleur de domaine utilise une autre machine (s) en tant que l’horloge maître.  Il peut s’agir d’une appliance ou un ordinateur dédié.

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> Pour plus d’informations, consultez [configurer le Service de temps Windows](https://technet.microsoft.com/library/cc731191.aspx)

Si le contrôleur de domaine a installé le matériel GPS, vous devez utiliser ces étapes pour désactiver le client NTP et activer le serveur NTP.

Commencez par le Client NTP de désactiver et activer le serveur NTP à l’aide de ces changements de clés de Registre.

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

Ensuite, redémarrez le Service de temps Windows

    net stop w32time && net start w32time

Enfin, vous indiquez que cet ordinateur dispose d’une source de temps fiable à l’aide.
   
    w32tm /config /reliable:yes /update

Pour vérifier que les modifications ont été effectuées correctement, vous pouvez exécuter les commandes suivantes qui affectent les résultats ci-dessous. 

    w32tm /query /configuration

Value|Paramètre attendu|
----- | ----- |
AnnounceFlags|  5 (local)|
NtpServer   |(Local)|
DllName |C:\WINDOWS\SYSTEM32\w32time. DLL (Local)|
Enabled |1 (local)|
NtpClient|  (Local)|

    w32tm /query /status /verbose

Value|  Paramètre attendu|
----- | ----- |
Stratum|    1 (principal référence - syncd by horloge de case d’option)|
ReferenceId|    0x4C4F434C (nom de la source :  « LOCAL »)|
Source| Horloge CMOS local|
Décalage de phase|   0.0000000s|
Rôle serveur|    576 (Service de temps fiable)|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 sur des plateformes virtuelles 3e tiers
Lorsque Windows est virtualisé, par défaut, l’hyperviseur est responsable du temps.  Mais joint à un domaine membres doivent être synchronisé avec le contrôleur de domaine dans l’ordre pour Active Directory fonctionne correctement.  Il est préférable de désactiver la virtualisation de n’importe quel moment entre l’invité et l’hôte de n’importe quel plateformes virtuel tiers 3e.

#### <a name="discovering-the-hierarchy"></a>Découverte de la hiérarchie
Étant donné que la chaîne de hiérarchie de temps à la source de l’horloge de référence est dynamique dans un domaine, et négocié, vous devez interroger l’état d’un ordinateur particulier de comprendre qu’il est source de temps et de la chaîne à l’horloge de la source principale.  Cela peut aider à diagnostiquer les problèmes de synchronisation de temps.

Vous souhaitez résoudre les problèmes d’un client spécifique ; la première étape consiste à comprendre sa source de temps à l’aide de cette commande w32tm.

    w32tm /query /status

Les résultats affichent la Source entre autres choses.  La Source indique avec lesquels vous synchronisez l’heure dans le domaine.  Il s’agit de la première étape de cette hiérarchie de temps de machines.
Utilisez ensuite entrée Source ci-dessus et le paramètre /StripChart pour rechercher la source de temps suivante dans la chaîne.

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

Également utile, la commande suivante répertorie chaque contrôleur de domaine qu'il peut trouver dans le domaine spécifié et affiche un résultat qui vous permet de déterminer chaque partenaire.  Cette commande inclura les ordinateurs qui ont été configurés manuellement.
    
    w32tm /monitor /domain:my_domain

À l’aide de la liste, vous pouvez suivre les résultats via le domaine et comprendre la hiérarchie, ainsi que le décalage horaire à chaque étape.  En recherchant le point où le décalage horaire s’aggrave considérablement, vous pouvez identifier la racine de l’heure incorrecte.  À partir de là, vous pouvez essayer de comprendre pourquoi cette heure est incorrecte en activant [w32tm journalisation](#W32Logging). 

#### <a name="using-group-policy"></a>À l’aide de la stratégie de groupe
Vous pouvez utiliser Stratégie de groupe pour accomplir une précision plus stricte, par exemple, attribuer des clients à utiliser des serveurs NTP spécifiques ou à contrôle comment de bas niveau du système d’exploitation est configurées lorsque virtualisé.  
Voici une liste des scénarios possibles et les paramètres de stratégie de groupe appropriés :

**Domaines de virtualiser** - afin de contrôler les contrôleurs de domaine virtualisés dans Windows 2012R2 afin qu’ils synchronisent l’heure avec son domaine, plutôt que de l’hôte Hyper-V, vous pouvez désactiver cette entrée de Registre.   Pour le contrôleur de domaine principal, vous ne souhaitez pas désactiver l’entrée comme l’hôte Hyper-V fournira la source de temps plus stable.  L’entrée de Registre vous oblige à redémarrer le service w32time après sa modification.

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**Précision des charges sensibles** : pour les charges de travail sensibles temps précision, vous pouvez configurer des groupes d’ordinateurs pour définir les serveurs NTP et les consignées des paramètres d’heure, telles que la fréquence de mises à jour de l’interrogation et horloge.  Ceci est généralement géré par le domaine, mais pour davantage de contrôle, vous pouvez cibler des ordinateurs spécifiques pour pointer directement vers l’horloge maître.

Paramètre de stratégie de groupe|   Nouvelle valeur|
----- | ----- |
NtpServer|  ClockMasterName,0x8|
MinPollInterval|    6 – 64 secondes|
MaxPollInterval|    6|
UpdateInterval| 100 – une fois par seconde|
EventLogFlags|  3 – tous les connexion spéciale|

> [!NOTE]
> Les paramètres NtpServer et EventLogFlags sont situés sous Windows\Windows temps Service\Time fournisseurs en utilisant les paramètres de configuration Windows NTP Client.  Les 3 autres se trouvent sous le Service de temps Windows\Windows en utilisant les paramètres de Configuration globale.

**À distance précision sensibles charges à distance** : pour les systèmes dans des domaines de branche pour l’instance de vente au détail et le secteur de crédit de paiement (PCI), Windows utilise les informations de site actuel et source de temps du localisateur de contrôleur de domaine pour trouver un contrôleur de domaine local, sauf s’il existe un NTP manuelle configuré.  Cet environnement nécessite 1 seconde de précision, qui utilise la convergence plus rapide sur l’heure correcte.  Cette option permet le service w32time à déplacer vers l’arrière de l’horloge.  Si cela est acceptable et répond à vos besoins, vous pouvez créer la stratégie suivante.   Comme avec n’importe quel environnement, permet de s’assurer de test et de ligne de base de votre réseau. 

Paramètre de stratégie de groupe|   Nouvelle valeur|
----- | ----- |
MaxAllowedPhaseOffset|  1, si plus d’une seconde, définissez une horloge à l’heure correcte.|

Le paramètre MaxAllowedPhaseOffset se trouve sous le Service de temps Windows\Windows en utilisant les paramètres de Configuration globale.

> [!NOTE]
> Pour plus d’informations sur la stratégie de groupe et les entrées associées, consultez [outils de Service de temps Windows](windows-time-service-tools-and-settings.md) et article de paramètres sur TechNet.

## <a name="azure-and-windows-iaas-considerations"></a>Considérations relatives à Azure et Windows IaaS

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Machine virtuelle Azure : Services de domaine Active Directory
Si la machine virtuelle Azure exécutant les Services de domaine Active Directory fait partie d’un existant en local, forêt Active Directory, puis TimeSync(VMIC), doit être désactivée. Il s’agit pour autoriser tous les contrôleurs de domaine dans la forêt, physique et virtuelle, à utiliser une hiérarchie de synchronisation de temps unique. Consultez le livre blanc sur la meilleure pratique [« en cours d’exécution domaine contrôleurs dans Hyper-V »](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

### <a name="azure-virtual-machine-domain-joined-machine"></a>Machine virtuelle Azure : ordinateur joint au domaine
Si vous hébergez un ordinateur qui est joint à une forêt Active Directory existante, virtuel ou physique, la meilleure pratique consiste à désactiver TimeSync pour l’invité et vérifiez que W32Time est configuré pour être synchronisé avec son contrôleur de domaine via la configuration de l’heure pour Type = NTP5

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Machine virtuelle Azure : Machine de groupe de travail autonome
Si la machine virtuelle Azure n’est pas joint à un domaine, et n’est pas un contrôleur de domaine, la recommandation consiste à conserver la configuration de temps par défaut et ont la machine virtuelle à synchroniser avec l’hôte.

## <a name="windows-application-requiring-accurate-time"></a>Application de Windows nécessitant une heure précise
### <a name="time-stamp-api"></a>Horodatage API
Les programmes qui nécessitent la plus grande précision en ce qui concerne l’heure UTC et pas le passage du temps, doivent utiliser le [GetSystemTimePreciseAsFileTime API](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx).  Cela garantit que votre application obtient l’heure système est conditionnée par le service de temps de Windows.

### <a name="udp-performance"></a>Performances de UDP
Si vous disposez d’une application que les communications UDP utilise pour les transactions et il. il est important réduire la latence, il existe des associés d’entrées de Registre que vous pouvez utiliser pour configurer une plage de ports à exclure de la base de moteur de filtrage de port.  Cela améliorer le temps de latence et augmenter le débit.  Toutefois, les modifications au Registre doivent être limitées aux administrateurs expérimentés.  En outre, cette solution de contournement exclut ports sécurisées par le pare-feu.  Consultez la référence de l’article ci-dessous pour plus d’informations.

Pour Windows Server 2012 et Windows Server 2008, vous devez d’abord installer un correctif logiciel.  Vous pouvez référencer cet article de la base de connaissances : [Perte du datagramme lorsque vous exécutez une application réceptrice multidiffusion dans Windows 8 et Windows Server 2012](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>Mettre à jour les pilotes réseau
Certains fournisseurs réseau disposent des mises à jour de pilote qui améliorer les performances en ce qui concerne la latence de pilote et de mise en mémoire tampon les paquets UDP.  Veuillez contacter votre fournisseur de réseau pour voir s’il existe des mises à jour pour aider à avec un débit UDP.

## <a name="logging-for-auditing-purposes"></a>Journalisation des fins d’audit
Pour se conformer aux réglementations de suivi de temps vous pouvez archiver manuellement w32tm journaux, les journaux des événements et les informations d’analyse de performances.  Une version ultérieure, les informations archivées peuvent être utilisées pour attester de conformité à un point précis dans le passé.  Les facteurs suivants sont utilisés pour indiquer la précision.


1. Précision de l’horloge à l’aide du compteur du moniteur de performances calculée de décalage temporel.  Cela indique l’horloge avec la précision de l’ordre souhaité.
2.  Recherchez « Réponse d’homologue à partir de » dans les journaux w32tm source d’horloge.   Le texte du message est l’adresse IP ou le VMIC, qui décrit la source de temps et le suivant dans la chaîne d’horloges de référence pour valider.
3.  État condition l’utilisation des journaux w32tm pour valider d’horloge » ClockDispl Discipline : \*INCLINER\*temps\*« se produisent.  Cela indique que w32tm est active au moment.

### <a name="event-logging"></a>Journalisation des événements
Pour obtenir toute l’histoire, vous devez également les informations du journal des événements.  En recueillant le journal des événements système et un filtrage sur le serveur de synchronisation, Microsoft-Windows-Kernel-démarrage, Microsoft-Windows-Kernel-général, vous pourrez peut-être détecter s’il existe d’autres influences qui ont été modifiés le temps, par exemple, des tiers.  Ces journaux peuvent être nécessaires pour écarter les interférences externe.  Stratégie de groupe peut affecter les journaux des événements sont écrits dans le journal.  Pour plus d’informations, consultez la section ci-dessus sur à l’aide de la stratégie de groupe.

### <a name="W32Logging"></a>Journalisation du débogage W32Time
Pour activer w32tm à des fins d’audit, la commande suivante active la journalisation qui montre les mises à jour périodiques de l’horloge et indique l’horloge de la source.  Redémarrez le service pour activer la journalisation de nouveau.  

Pour plus d’informations, consultez [comment activer le journal de débogage pour le Service de temps Windows](https://support.microsoft.com/kb/816043).

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>Analyseur de performances
Le service de temps Windows de Windows Server 2016 expose les compteurs de performance qui peuvent être utilisés pour collecter la journalisation d’audit.  Il peuvent être connectés localement ou à distance.  Vous pouvez enregistrer l’ordinateur de décalage temporel et les compteurs de délai aller-retour.  
Et comme n’importe quel compteur de performances, vous pouvez les contrôler à distance et créer des alertes à l’aide de System Center Operations Manager.  Vous pouvez, par exemple, utiliser une alerte pour vous d’alarme lorsque la valeur de décalage temporel diffère de la précision de votre choix.  Le [System Center Management Pack](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx) a plus d’informations.

### <a name="windows-traceability-example"></a>Exemple de traçabilité de Windows
À partir de fichiers de journaux w32tm vous souhaitez valider les deux éléments d’information.  La première est une indication que le fichier journal est actuellement condition horloge.  Cela prouver que votre horloge a été est conditionnée par le Service de temps Windows au moment litigieuse.

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

Le point à retenir est que vous voyez messages précédés de ClockDispln Discipline qui est la preuve w32time interagit avec votre horloge système.
 
Ensuite, vous devez rechercher le dernier rapport dans le journal avant l’heure litigieuse qui signale l’ordinateur source qui est actuellement utilisé en tant que l’horloge de référence.  Cela peut être une adresse IP, nom de l’ordinateur ou le fournisseur VMIC, ce qui indique qu’il se synchronise avec l’hôte pour Hyper-V.  L’exemple suivant fournit une adresse IPv4 de 10.197.216.105.

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

Maintenant que vous avez validé le premier système dans la chaîne d’heure de référence, vous devez examiner le fichier journal sur la source de temps de référence et répétez ces étapes.  Ce processus se poursuit jusqu'à ce que vous atteigniez à une horloge physique, tels que GPS ou une source de temps comme NIST.  Si l’horloge de référence est un matériel GPS, journaux à partir de la fabrication peuvent également être requises.

## <a name="network-considerations"></a>Considérations relatives au réseau
Les algorithmes de protocole NTP ont une dépendance sur la symétrie de votre réseau.  Votre boîte de dialogue augmenter le nombre de tronçons réseau, augmente la probabilité d’asymétrie.  Il, il est difficile de prédire quels types de précisions que vous voyez dans vos environnements spécifiques. 

Analyseur de performances et les nouveaux compteurs de temps de Windows dans Windows Server 2016 peuvent être utilisés pour évaluer la précision de vos environnements et créer des lignes de base. En outre, vous pouvez effectuer la résolution des problèmes pour déterminer le décalage en cours de n’importe quel ordinateur sur votre réseau.

Il existe deux normes générales pour une heure précise sur le réseau.  PTP ([précision Time Protocol - IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) a des exigences plus strictes sur l’infrastructure réseau, mais peuvent souvent fournir la précision de microseconde.  NTP ([Network Time Protocol – RFC 1305](https://tools.ietf.org/html/rfc1305)) fonctionne sur un éventail plus large des réseaux et des environnements, ce qui le rend plus facile à gérer. 

Windows prend en charge Simple NTP (RFC2030) par défaut pour les machines jointes n’appartenant pas au domaine.  Pour les ordinateurs joints à un domaine, nous utilisons un NTP sécurisé appelé [MS-SNTP](https://msdn.microsoft.com/library/cc246877.aspx), qui exploite les secrets de domaine négocié qui fournissent un avantage de gestion sur NTP authentifié décrits dans RFC1305 et RFC5905.   

Le domaine et les protocoles joint à un domaine non requiert le port UDP 123.  Pour plus d’informations sur les meilleures pratiques NTP, reportez-vous à [réseau temps protocole meilleures pratiques IETF brouillon actif](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00).

### <a name="reliable-hardware-clock-rtc"></a>Horloge de matériel fiable (RTC)
Windows n'effectue pas les temps d’étape, à moins que certaines limites sont dépassées, mais plutôt disciplines l’horloge.  Cela signifie que w32tm ajuste la fréquence de l’horloge à intervalles réguliers, à l’aide de la fréquence de mise à jour d’horloge définissant, les valeurs par défaut à une fois par seconde avec Windows Server 2016.  Si l’horloge est protégée, il accélère la fréquence et s’il est à l’avance, il ralentit la fréquence.  Toutefois, pendant ce temps entre les ajustements de fréquence d’horloge, l’horloge de matériel est dans le contrôle.  S’il existe un problème avec le microprogramme ou de l’horloge de matériel, l’heure de l’ordinateur peut devenir moins précis.

Il s’agit d’une autre raison, que vous devez tester et la base de référence dans votre environnement.  Si le compteur de performances « Calculée de décalage temporel » n’est pas stabilisé à la précision que vous ciblez, puis vous voulez vérifier que votre microprogramme est à jour.  En tant qu’un autre test, vous pouvez voir si le matériel en double reproduire le même problème.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>Résolution des problèmes de précision du temps et NTP
Vous pouvez utiliser la découverte la section hiérarchie ci-dessus pour comprendre la source de l’heure inexacte.  En examinant le décalage horaire, de trouver le point dans la hiérarchie où temps diffère le meilleur parti de sa Source NTP.  Une fois que vous comprenez la hiérarchie, vous voudrez comprendre pourquoi cette source de temps spécifique ne reçoit pas de temps précise.  

Se concentrer sur le système avec heure divergente, vous pouvez utiliser ces outils ci-dessous pour recueillir plus d’informations pour vous aider à déterminer le problème et trouver une résolution.  La référence UpstreamClockSource ci-dessous, est l’horloge détectée à l’aide de « w32tm /config/Status ».


- Journaux des événements système
- Activer à l’aide de journalisation : w32tm journaux - w32tm/Debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-300
- clé de Registre w32Time HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- Traces de réseau local
- Compteurs de performances (à partir de l’ordinateur local ou le UpstreamClockSource)
- W32tm /stripchart /computer:UpstreamClockSource
- UpstreamClockSource PING pour comprendre la latence et le nombre de sauts pour la Source
- Tracert UpstreamClockSource

Problème|    Symptômes|   Résolution|
----- | ----- | ----- |
Horloge TSC locale n’est pas stable.| À l’aide de la synchronisation de l’Analyseur de performances - ordinateur physique – horloge horloge stable, mais vous toujours voir toutes les minutes de plusieurs 100us 1-2. |   Mettre à jour du microprogramme ou valider un matériel différent n’affiche pas le même problème.|
Latence du réseau|    w32tm stripchart affiche un RoundTripDelay de plus de 10 ms.  Variation du retard provoquer un bruit aussi grand que ½ du temps d’aller-retour, par exemple un délai qui est dans une seule direction.</br></br>UpstreamClockSource est sauts multiples, comme indiqué par la commande PING.  Durée de vie doit être proche 128.</br></br>Utilisation de Tracert pour trouver la latence à chaque tronçon.    | Rechercher une source d’horloge plus proche pour le moment.  Une solution consiste à installer une horloge de la source sur le même segment ou manuellement sur horloge source qui est géographiquement plus proche.  Pour un scénario de domaine, ajoutez un ordinateur avec le rôle GTimeServ. |  
Ne peut pas atteindre de manière fiable la source NTP|    W32tm /stripchart renvoie par intermittence « Expiration du délai de la demande »    |Source NTP n’est pas réactif|
Source NTP n’est pas réactif|    Vérifiez les compteurs Perfmon pour les réponses sortantes du serveur NTP nombre de Source Client NTP, requêtes entrantes du serveur NTP et votre utilisation par rapport à vos lignes de base de.|    À l’aide des compteurs de performances de serveur, déterminer si la charge a changé en référence à vos lignes de base.</br></br>Existe-t-il des réseau congestion problèmes ?|
Ne pas à l’aide de l’horloge plus précise de contrôleur de domaine|    Modifications dans la topologie ou une horloge maître récemment ajoutée.|   w32tm /resync /rediscover|
Dérivant des horloges du client| Service de temps événement 36 dans le journal des événements système et/ou de texte dans le fichier journal décrivant qui : Compteur de « nombre Source temps NTP Client » allant de 1 à 0|Résoudre les problèmes de la source en amont et à comprendre si elle est en cours d’exécution sur les problèmes de performances.|

### <a name="baselining-time"></a>Heure de la ligne de base
Ligne de base est important afin que vous pouvez tout d’abord, comprendre les performances et la précision de votre réseau et comparer avec la ligne de base à l’avenir lorsque des problèmes se produisent.  Vous voudrez ligne de base de la racine du contrôleur de domaine principal ou tous les ordinateurs marqués avec le GTIMESRV.  Nous vous recommanderais également planifié le contrôleur de domaine principal dans chaque forêt.  Sélectionnez enfin tous les contrôleurs de domaine ou les machines qui ont des caractéristiques intéressantes, telles que la distance ou des charges élevées et de ligne de base critiques ces.

Il est également utile de la ligne de base Windows Server 2016 vs 2012 R2, toutefois, vous avez uniquement w32tm /stripchart comme un outil que vous pouvez utiliser pour comparer, dans la mesure où Windows Server 2012 R2 n’a pas les compteurs de performances.  Vous devez choisir les deux ordinateurs présentant les mêmes caractéristiques, ou mettez à niveau un ordinateur et comparer les résultats après la mise à jour.  L’addenda de mesures de temps Windows a plus d’informations sur la procédure à suivre des mesures détaillées entre 2016 et 2012.

À l’aide des tous les w32time compteurs de performances, de collecter des données pour au moins une semaine.  Cela garantira que vous avez suffisamment d’une référence pour prendre en compte différents dans le réseau au fil du temps et suffisamment d’une exécution à assurent que votre analyse de précision de temps est stable.

### <a name="ntp-server-redundancy"></a>Redondance du serveur NTP
Pour la configuration de serveur NTP manuelle utilisée avec les machines jointes n’appartenant pas au domaine ou contrôleur de domaine principal, plus d’un serveur est une mesure de redondance satisfaisante en cas de disponibilité.  Il peut également donner une meilleure précision, en supposant que toutes les sources sont exactes et stable.  Toutefois, si la topologie n’est pas bien conçue, ou les sources de temps ne sont pas stables, la précision résultant peut être pire donc avec prudence.  La limite de temps pris en charge les serveurs w32time peut référencer manuellement est 10. 

## <a name="leap-seconds"></a>Secondes intercalaires
Période de rotation de la terre varie au fil du temps, provoquée par des événements climatiques et géologiques. En règle générale, la variante est environ une seconde après quelques années. Chaque fois que la variation de temps atomique devient trop volumineux, une correction d’une seconde (augmentation ou réduction) est inséré, appelé un saut de seconde. Pour cela de manière à ce que la différence ne dépasse jamais 0,9 secondes. Cette correction est annoncée avant la correction réelle des six derniers mois. Avant Windows Server 2016, le Service de temps Microsoft n’était pas conscient des secondes intercalaires, mais reposait sur le service de temps externe de s’occuper de cela. Avec la précision accrue des temps de Windows Server 2016, Microsoft travaille sur une solution plus appropriée pour le problème de saut de seconde.

## <a name="secure-time-seeding"></a>Sécuriser l’amorçage de temps
W32Time dans Server 2016 inclut la fonctionnalité de sécuriser l’amorçage de temps. Cette fonctionnalité détermine l’heure actuelle approximative à partir des connexions SSL sortantes.  Cette valeur de temps est utilisée pour surveiller l’horloge système local et corrigez les erreurs brutes. Vous trouverez plus d’informations sur la fonctionnalité dans [ce billet de blog](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/).  Dans les déploiements avec une ou plusieurs sources de temps fiable et des ordinateurs bien surveillés qui incluent la surveillance des décalages de temps, vous pouvez choisir de ne pas utiliser la fonctionnalité de sécuriser l’amorçage de temps et s’appuient sur votre infrastructure existante à la place. 

Vous pouvez désactiver la fonctionnalité avec ces étapes :

1.  Définissez la valeur de configuration du Registre UtilizeSSLTimeData sur 0 sur un ordinateur spécifique :

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f


2.  Si vous ne parvenez pas à redémarrer l’ordinateur immédiatement en raison d’une raison quelconque, vous pouvez notifier le service W32time sur la mise à jour de configuration. Cela arrête la surveillance en temps et en fonction des données au moment de l’application collectées à partir des connexions SSL. 

    W32tm.exe /config /update

3.  Le redémarrage de l’ordinateur rend le paramètre soit immédiatement effective et cela peut entraîne à arrêter la collecte des données de temps à partir des connexions SSL.  La dernière partie a une très petite surcharge et ne doit pas être un problème de performances.

4.  Pour appliquer ce paramètre dans un domaine entier, veuillez définir la valeur de UtilizeSSLTimeData dans le paramètre de stratégie de groupe W32time à 0 et le paramètre de publication.  Lorsque le paramètre est récupéré par un Client de stratégie de groupe, les service W32time est informé et il arrêtera la surveillance en temps et mise en œuvre à l’aide des données de temps SSL. La collecte de données de temps SSL s’arrête lorsque chaque ordinateur redémarre. Si votre domaine a les ordinateurs portables de slim portables/tablettes et autres périphériques, vous pouvez peut-être exclure ces ordinateurs à partir de ce changement de stratégie. Ces périphériques seront confronté finalement une batterie se décharge et avez besoin de l’amorçage de temps de sécuriser fonctionnalité pour amorcer leur temps.


