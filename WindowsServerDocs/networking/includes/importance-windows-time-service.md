## <a name="importance-of-time-protocols"></a>Importance des protocoles de temps
Protocoles de temps communiquent entre deux ordinateurs pour échanger des informations de temps, puis utiliser ces informations pour synchroniser leur horloge. Avec le protocole de temps de service de temps Windows, un client demande des informations de temps à partir d’un serveur et synchronise son horloge basée sur les informations reçues.
  
Le service de temps Windows utilise le protocole NTP afin de synchroniser l’heure sur un réseau. NTP est un protocole de temps Internet qui inclut les algorithmes discipline nécessaires pour la synchronisation des horloges. NTP est un protocole de temps plus précis que le temps protocole SNTP (Simple Network) qui est utilisé dans certaines versions de Windows; Toutefois, W32Time continue de prendre en charge SNTP pour activer la compatibilité descendante avec les ordinateurs exécutant les services de temps SNTP base tels que Windows 2000.

Il existe différentes raisons que vous devrez peut-être heure précise.  Le cas par défaut pour Windows est Kerberos, ce qui nécessite de 5 minutes de précision entre le client et le serveur.  Toutefois, il existe de nombreux autres domaines peuvent être affectés par la précision du temps, y compris:


- Réglementations telles que:
    - précision de 50 ms pour FINRA aux États-Unis.
    - 1 ESMA ms (MiFID II) dans l’Union européenne.
- Algorithmes de chiffrement
- Systèmes distribués comme Cluster/SQL/Exchange et les bases de données de Document
- Framework Blockchain pour les transactions bitcoin
- Journaux distribuées et l’analyse des menaces 
- Réplication Active Directory
- PCI (Payment Card Industry), la précision deuxième actuellement 1