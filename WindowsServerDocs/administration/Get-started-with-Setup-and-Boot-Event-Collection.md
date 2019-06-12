---
title: Prise en main de la collecte d’événements de configuration et de démarrage
description: Configurer les collecteurs et les cibles de collecte d’événements de configuration et de démarrage
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-sbec
ms.localizationpriority: medium
ms.date: 10/16/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247b3f8
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: e94659c62db574dc8779c8246d471ab401414ddb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435806"
---
# <a name="get-started-with-setup-and-boot-event-collection"></a>Prise en main de la collecte d’événements de configuration et de démarrage

>S'applique à : Windows Server

  
## <a name="overview"></a>Vue d'ensemble  
La collecte d’événements de configuration et de démarrage est une nouvelle fonctionnalité dans Windows Server 2016 qui vous permet de désigner un ordinateur « collecteur » qui peut collecter différents événements importants qui se produisent sur d’autres ordinateurs lorsqu’ils démarrer ou passent par le processus de configuration. Vous pouvez ensuite analyser les événements collectés avec l’Observateur d’événements, l’Analyseur de messages, Wevtutil ou les applets de commande Windows PowerShell.  
  
Auparavant, ces événements étaient impossibles à surveiller, car l’infrastructure nécessaire pour les collecter n’existe pas tant qu’un ordinateur n'est pas déjà configuré. Les types d’événements de configuration et de démarrage que vous pouvez surveiller sont les suivants :  
  
-   Chargement des modules et des pilotes de noyau  
  
-   Énumération des périphériques et initialisation de leurs pilotes (y compris les « périphériques » de type unité centrale)  
  
-   Vérification et montage de systèmes de fichiers  
  
-   Démarrage des fichiers exécutables  
  
-   Démarrage et achèvement des mises à jour du système  
  
-   Les moments où le système devient disponible pour l’ouverture de session, établit la connexion avec un contrôleur de domaine, l’achèvement des démarrages du service et la disponibilité des partages réseau  
  
L’ordinateur collecteur doit exécuter Windows Server 2016 (il peut être en mode Serveur avec Expérience utilisateur ou en mode d’installation minimale). L’ordinateur cible doit exécuter Windows 10 ou Windows Server 2016. Vous pouvez également exécuter ce service sur une machine virtuelle hébergée sur un ordinateur qui n'exécute **pas** Windows Server 2016. Les combinaisons suivantes de collecteur virtualisé et d'ordinateurs cibles sont compatibles :  
  
|Hôte de virtualisation|Machine virtuelle de collecteur|Machine virtuelle cible|  
|-----------------------|-----------------------------|--------------------------|  
|Windows 8.1|oui|oui|  
|Windows 10|oui|oui|  
|Windows Server 2016|oui|oui|  
|Windows Server 2012 R2|oui|non|  
  
## <a name="installing-the-collector-service"></a>Installation du service collecteur  
À partir de Windows Server 2016, le service de collecteur d’événements est disponible en tant que fonctionnalité facultative. Dans cette version, vous pouvez l'installer à l’aide de DISM.exe avec cette commande à une invite Windows PowerShell avec élévation de privilèges :  
  
`dism /online /enable-feature /featurename:SetupAndBootEventCollection`  
  
Cette commande crée un service appelé BootEventCollector et le démarre avec un fichier de configuration vide.  
  
Confirmez la réussite de l’installation en vérifiant `get-service -displayname *boot*`. Le collecteur d’événements de démarrage doit être en cours d’exécution. Il s’exécute sous le compte de service réseau et crée un fichier de configuration vide (Active.xml) dans **%SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config**.  
  
Vous pouvez également installer le service de collecte d’événements de configuration et de démarrage avec l’Assistant Ajout de rôles et de fonctionnalités dans le Gestionnaire de serveur.  
  
## <a name="configuration"></a>Configuration  
Vous devez configurer deux éléments pour collecter des événements de configuration et de démarrage.  
  
-   Sur les ordinateurs cibles qui envoient les événements (autrement dit, les ordinateurs dont vous voulez surveiller la configuration et le démarrage), activez le transport KDNET/EVENT-NET et activez le transfert des événements.  
  
-   Sur l’ordinateur collecteur, spécifiez les ordinateurs dont vous voulez accepter les événements, ainsi que leur emplacement d’enregistrement.  
  
> [!NOTE]  
> Vous ne pouvez pas configurer un ordinateur pour envoyer les événements de configuration ou de démarrage à lui-même. Mais si vous souhaitez surveiller deux ordinateurs, vous pouvez les configurer pour qu'ils s'envoient les événements entre eux.  
  
### <a name="configuring-a-target-computer"></a>Configuration d’un ordinateur cible  
Sur chaque ordinateur cible, vous activez tout d’abord le transport KDNET/EVENT-NET, puis vous activez l’envoi des événements ETW via le transport, puis redémarrez l’ordinateur cible. EVENT-NET est un protocole de transport intégré au noyau similaire à KDNET (protocole de débogueur du noyau). EVENT-NET transmet uniquement les événements et ne permet pas d'accéder au débogueur. Ces deux protocoles s'excluent mutuellement ; vous ne pouvez en activer qu'un à la fois.  
  
Vous pouvez activer le transport des événements à distance (avec Windows PowerShell) ou localement.  
  
##### <a name="to-enable-event-transport-remotely"></a>Pour activer le transport d'événements à distance  
  
1.  Si vous avez déjà configuré un accès distant Windows PowerShell sur l’ordinateur cible, passez à l’étape 3. Si ce n’est pas le cas, sur l’ordinateur cible, ouvrez une invite de commandes et exécutez la commande suivante :  
  
    winrm quickconfig  
  
2.  Répondez aux invites, puis redémarrez l’ordinateur cible. Si les ordinateurs cible ne se trouvent pas dans le même domaine que l’ordinateur collecteur, vous devrez peut-être les définir comme hôtes approuvés. Pour ce faire :  
  
3.  Sur l’ordinateur collecteur, exécutez l'une de ces commandes :  
  
    -   Dans une invite Windows PowerShell : `Set-Item -Force WSMan:\localhost\Client\TrustedHosts "<target1>,<target2>,..."`, suivie `Set-Item -Force WSMan:\localhost\Client\AllowUnencrypted true` où \<target1 >, etc. sont les noms ou adresses IP des ordinateurs cibles.  
  
    -   Ou dans une invite de commandes : **winrm définie winrm/config/client @{TrustedHosts = »\<target1 >,\<target2 >,... » ; AllowUnencrypted = « true »}**  
  
        > [!IMPORTANT]  
        > Cela définit une communication non chiffrée, donc ne l'exécutez pas en dehors d’un environnement de laboratoire.  
  
4.  Testez la connexion à distance en accédant à l’ordinateur collecteur et en exécutant l'une de ces commandes Windows PowerShell :  
  
    Si l’ordinateur cible est dans le même domaine que l’ordinateur collecteur, exécutez `New-PSSession -Computer <target> | Remove-PSSession`  
  
    Si l’ordinateur cible ne se trouve pas dans le même domaine, exécutez `New-PSSession -Computer  <target>  -Credential Administrator | Remove-PSSession`, qui vous invite à indiquer vos informations d’identification.  
  
    Si la commande ne renvoie rien, la communication à distance a réussi.  
  
5.  Sur l’ordinateur cible, ouvrez une invite Windows PowerShell avec élévation de privilèges et exécutez la commande suivante :  
  
    `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d>`  
  
    Ici < target_name > est le nom de l’ordinateur cible, \<ip > est l’adresse IP de l’ordinateur collecteur. \<port > est le numéro de port où le collecteur s’exécute. La clé <a.b.c.d> est une clé de chiffrement requise pour la communication, composée de quatre chaînes alphanumériques séparées par des points. Cette même clé est utilisée sur l’ordinateur collecteur. Si vous n’entrez pas de clé, le système génère une clé aléatoire ; vous en aurez besoin pour l’ordinateur collecteur, donc prenez note de celle-ci.  
  
6.  Si vous avez déjà configuré un ordinateur collecteur, mettez à jour le fichier de configuration sur l’ordinateur collecteur avec les informations pour le nouvel ordinateur cible. Consultez la section « Configuration de l’ordinateur collecteur » pour plus d’informations.  
  
##### <a name="to-enable-event-transport-locally-on-the-target-computer"></a>Pour activer le transport d'événements localement sur l’ordinateur cible  
  
1.  Démarrez une invite de commandes avec élévation de privilèges et exécutez les commandes suivantes :  
  
    **Oui /event de bcdedit**  
  
    **bcdedit /eventsettings net hostip:1.2.3.4 port : 50000 key:a.b.c.d**  
  
    « 1.2.3.4 » est ici un exemple ; remplacez-le par l’adresse IP de l’ordinateur collecteur. Remplacez également « 50000 » par le numéro de port dans lequel s’exécute le collecteur et « a.b.c.d » par la clé de chiffrement requise pour la communication. Cette même clé est utilisée sur l’ordinateur collecteur. Si vous n’entrez pas de clé, le système génère une clé aléatoire ; vous en aurez besoin pour l’ordinateur collecteur, donc prenez note de celle-ci.  
  
2.  Si vous avez déjà configuré un ordinateur collecteur, mettez à jour le fichier de configuration sur l’ordinateur collecteur avec les informations pour le nouvel ordinateur cible. Consultez la section « Configuration de l’ordinateur collecteur » pour plus d’informations.  
  
**Maintenant que le transport d’événement lui-même est activé, vous devez activer le système envoyer des événements ETW via ce transport.**  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-remotely"></a>Pour activer l’envoi des événements ETW via le transport à distance  
  
1.  Sur l’ordinateur collecteur, ouvrez une invite Windows PowerShell avec élévation de privilèges.  
  
2.  Exécutez `Enable-SbecAutologger -ComputerName <target_name>`, où <target_name> est le nom de l’ordinateur cible.  
  
Si vous n’êtes pas en mesure de configurer un accès distant Windows PowerShell, vous pouvez toujours activer l’envoi d’événements directement sur l’ordinateur cible.  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-locally"></a>Pour activer l’envoi des événements ETW via le transport localement  
  
1.  Sur l’ordinateur cible, démarrez Regedit.exe et recherchez cette clé de Registre :  
  
    **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\WMI\AutoLogger**. Plusieurs sessions de connexion sont répertoriées comme sous-clés sous cette clé. **La plateforme d’installation**, le **Suivi du noyau Windows NT**et **Microsoft-Windows-Setup** sont les choix possibles pour une utilisation avec la collecte d’événements de configuration et de démarrage, mais l’option recommandée est **Système du journal des événements**. Ces clés sont détaillées dans [Configuration et démarrage d'une session du journal automatique](https://msdn.microsoft.com/library/windows/desktop/aa363687(v=vs.85).aspx).  
  
2.  Dans la clé EventLog-System, modifiez la valeur de **LogFileMode** de **0x10000180** par **0x10080180**. Pour plus d’informations sur les détails de ces paramètres, voir [Constantes du mode journalisation](https://msdn.microsoft.com/library/windows/desktop/aa364080(v=vs.85).aspx).  
  
3.  Si vous le souhaitez, vous pouvez également activer le transfert de données de vérification de bogue vers l’ordinateur collecteur. Pour ce faire, recherchez la clé de Registre HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager et créez la clé de **Debug Print Filter** avec une valeur de **0x1**.  
  
4.  Redémarrez l’ordinateur cible.  
  
### <a name="choosing-the-network-adapter"></a>Choix de la carte réseau  
Si l’ordinateur cible possède plusieurs cartes réseau, le pilote KDNET choisit le premier répertorié pris en charge. Vous pouvez spécifier une carte réseau à utiliser pour le transfert des événements de configuration en suivant ces étapes :  
  
##### <a name="to-specify-a-network-adapter"></a>Pour spécifier une carte réseau  
  
1.  Sur l’ordinateur cible, ouvrez le Gestionnaire de périphériques, développez **Cartes réseau**, recherchez la carte réseau que vous souhaitez utiliser et cliquez dessus avec le bouton droit.  
  
2.  Dans le menu qui s’ouvre, cliquez sur **propriétés**, puis cliquez sur le **détails** onglet. Développez le menu dans le **propriété** champ, faites défiler jusque **informations d’emplacement** (la liste n’est probablement pas dans l’ordre alphabétique), puis cliquez dessus. La valeur sera une chaîne au format **bus PCI X, appareil, Y, Z (fonction)** . Prenez note de X.Y.Z ; Voici les paramètres bus que vous avez besoin pour la commande suivante.  
  
3.  Exécutez l’une des commandes suivantes :  
  
    À partir d’une invite Windows PowerShell avec élévation de privilèges : `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d> -BusParams <X.Y.Z>`  
  
    À partir d’une invite de commandes avec élévation de privilèges : **bcdedit /eventsettings  net hostip:aaa port:50000 key:bbb busparams:X.Y.Z**  
  
### <a name="validate-target-computer-configuration"></a>Valider la configuration de l’ordinateur cible  
Pour vérifier les paramètres sur l’ordinateur cible, ouvrez une invite de commandes avec élévation de privilèges et exécutez **bcdedit/enum**. Une fois terminé, exécutez **bcdedit /eventsettings**. Vous pouvez vérifier les valeurs suivantes :  
  
-   Touche  
  
-   Debugtype = NET  
  
-   Hostip = \<adresse IP du collecteur de >  
  
-   Port = \<vous avez spécifié pour le collecteur pour utiliser de numéro de port >  
  
-   DHCP = oui  
  
Vérifiez également que vous avez activé **bcdedit /event**, car **/debug** et **/event** s’excluent mutuellement. Vous ne pouvez exécuter que l'un ou l’autre. De même, vous ne pouvez pas combiner /eventsettings avec/Debug ou /dbgsettings avec /event.  
  
Notez également que la collecte d’événements ne fonctionne pas si vous le configurez sur un port série.  
  
### <a name="configuring-the-collector-computer"></a>Configuration de l’ordinateur collecteur  
Le service collecteur reçoit les événements et les enregistre dans les fichiers ETL. Ces fichiers ETL peuvent alors être lus par d’autres outils, tels que l’Observateur d’événements, l’Analyseur de Message, Wevtutil et les applets de commande Windows PowerShell.  
  
Comme le format ETW ne vous permet de spécifier le nom de l’ordinateur cible, les événements pour chaque ordinateur cible doivent être enregistrés dans un fichier distinct. Les outils d’affichage peuvent indiquer un nom d’ordinateur, mais il s'agira du nom de l’ordinateur sur lequel s’exécute l’outil.  
  
Plus précisément, chaque ordinateur cible est affecté un anneau de fichiers ETL. Chaque nom de fichier inclut un index de 000 jusqu'à une valeur maximale que vous configurez (jusqu'à 999). Lorsque le fichier atteint la taille maximale configurée, il passe les événements d’écriture au fichier suivant. Après le fichier le plus élevé possible, il revient à l'index 000 du fichier. De cette façon, les fichiers sont automatiquement recyclés, ce qui limite l’utilisation d’espace disque. Vous pouvez également définir des stratégies de rétention externes supplémentaires pour limiter l’utilisation du disque ; par exemple, vous pouvez supprimer les fichiers antérieurs à un nombre défini de jours.  
  
Les fichiers ETL collectés sont généralement conservés dans le répertoire **c:\ProgramData\Microsoft\BootEventCollector\Etl** (qui peut avoir des sous-répertoires supplémentaires). Vous trouverez le fichier journal le plus récent en les triant par heure de dernière modification. Il existe également un journal d’état (généralement dans **c:\ProgramData\Microsoft\BootEventCollector\Logs**), qui enregistre chaque fois que le collecteur passe l’écriture à un nouveau fichier.  
  
Il existe également un journal de collecteur, qui enregistre des informations sur le collecteur lui-même. Vous pouvez conserver ce journal au format ETW (dans lequel les événements sont signalés au service Journal Windows ; il s’agit de la valeur par défaut) ou dans un fichier (normalement dans **c:\ProgramData\Microsoft\BootEventCollector\Logs**). L'utilisation d’un fichier peut être utile si vous souhaitez activer les modes détaillés qui génèrent une grande quantité de données. Vous pouvez également configurer le journal pour écrire dans une sortie standard en exécutant le collecteur à partir de la ligne de commande.  
  
**Création du fichier de configuration de collecteur**  
  
Lorsque vous activez le service, les trois fichiers de configuration XML sont créées et stockées dans **c:\ProgramData\Microsoft\BootEventCollector\Config**:  
  
-   **Active.XML** Ce fichier contient la configuration active actuelle du service collecteur.  Directement après l’installation, ce fichier a le même contenu que Empty.xml. Lorsque vous définissez une nouvelle configuration de collecteur, enregistrez-la dans ce fichier.  
  
-   **Empty.XML** Ce fichier contient les éléments de configuration minimale nécessaires définis avec leurs valeurs par défaut. Il ne permet pas d'activer une collecte ; il permet uniquement au service collecteur de démarrer en mode inactif.  
  
-   **Example.XML** Ce fichier fournit des exemples et des explications des éléments de configuration possibles.  
  
**Choix d’une limite de taille de fichier**  
  
Parmi les décisions que vous devez faire, vous devez définir une limite de taille de fichier. La limite de taille de fichier la plus adaptée varie en fonction du volume prévu des événements et l'espace disque disponible. Les fichiers plus petits sont plus pratiques pour le nettoyage des anciennes données. Toutefois, chaque fichier s’accompagne la surcharge d’un en-tête de 64 Ko et la lecture de fichiers pour obtenir l’historique combiné peut s’avérer fastidieux. La limite de taille de fichier minimale absolue est 256 Ko. La limite raisonnable pour une taille de fichier pratique doit être supérieure à 1 Mo, et 10 Mo est probablement la valeur type correcte. Une limite supérieure peut être raisonnable si vous prévoyez de nombreux événements.  
  
Plusieurs détails sont à prendre en compte concernant le fichier de configuration :  
  
-   L’adresse de l’ordinateur cible. Vous pouvez utiliser son adresse IPv4, une adresse MAC ou un GUID SMBIOS. Tenez compte de ces facteurs lors du choix de l’adresse à utiliser :  
  
    -   L’adresse IPv4 fonctionne mieux avec une affectation statique des adresses IP. Toutefois, même les adresses IP statiques doivent être disponibles via DHCP.  
  
    -   L'adresse MAC ou le GUID SMBIOS est pratique lorsqu’ils sont connus à l’avance, mais les adresses IP sont attribuées dynamiquement.  
  
    -   Les adresses IPv6 ne sont pas prises en charge par le protocole EVENT-NET.  
  
    -   Il est possible de spécifier plusieurs façons d'identifier l’ordinateur. Par exemple, si le matériel physique est sur le point d’être remplacé, vous pouvez entrer l’ancienne et la nouvelle adresse MAC et l'une ou l'autre sera acceptée.  
  
-   Clé de chiffrement utilisée pour la communication avec l’ordinateur collecteur  
  
-   Nom de l’ordinateur cible. Vous pouvez utiliser l’adresse IP, le nom d’hôte ou tout autre nom comme nom d’ordinateur.  
  
-   Nom du fichier ETL à utiliser et configuration de la taille d'anneau pour celui-ci  
  
##### <a name="to-create-the-configuration-file"></a>Pour créer le fichier de configuration  
  
1.  Ouvrez une invite Windows PowerShell avec élévation de privilèges et accédez au répertoire %SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config.  
  
2.  Tapez `notepad .\newconfig.xml` et appuyez sur ENTRÉE.  
  
3.  Copiez cet exemple de configuration dans la fenêtre du Bloc-notes :  
  
    ```  
    <collector configVersionMajor="1" statuslog="c:\ProgramData\Microsoft\BootEventCollector\Logs\statuslog.xml">  
      <common>  
        <collectorport value="50000"/>  
        <forwarder type="etl">  
          <set name="file" value="c:\ProgramData\Microsoft\BootEventCollector\Etl\{computer}\{computer}_{#3}.etl"/>  
          <set name="size" value="10mb"/>  
          <set name="nfiles" value="10"/>  
          <set name="toxml" value="none"/>  
        </forwarder>  
        <target>  
          <ipv4 value="192.168.1.1"/>  
          <key value="a.b.c.d"/>  
          <computer value="computer1"/>  
        </target>  
        <target>  
          <ipv4 value="192.168.1.2"/>  
          <key value="d1.e2.f3.g4"/>  
          <computer value="computer2"/>  
        </target>  
      </common>  
    </collector>  
    ```  
  
    > [!NOTE]  
    > Le nœud racine est \<collecteur >. Ses attributs spécifient la version de la syntaxe du fichier de configuration et le nom du fichier journal d'état.  
    >   
    > Le \<courantes > élément regroupe plusieurs cibles en spécifiant les éléments de configuration communs pour eux, très bien comme un groupe d’utilisateurs peut être utilisé pour spécifier les autorisations courantes pour plusieurs utilisateurs.  
    >   
    > Le \<collectorport > élément définit le numéro de port UDP où le collecteur doit écouter les données entrantes. Il s’agit du même port que celui spécifié dans l’étape de configuration cible pour Bcdedit. Le collecteur ne prend en charge qu’un seul port et toutes les cibles doivent se connecter au même port.  
    >   
    > Le \<redirecteur > élément spécifie comment les événements ETW reçus à partir d’ordinateurs cibles seront transférés. Il n'existe qu’un seul type de redirecteur, qui les écrit dans les fichiers ETL. Les paramètres spécifient le modèle de nom de fichier, la limite de taille pour chaque fichier dans l’anneau et la taille de l’anneau pour chaque ordinateur. Le paramètre « toxml » spécifie que les événements ETW seront écrits sous forme binaire tels qu'ils ont été reçus, sans conversion en XML. Consultez la section « Conversion des événements en XML » pour plus d’informations sur le choix de conférer les événements au format XML ou non. Le modèle de nom de fichier contient les remplacements suivants : {computer} pour le nom d’ordinateur et {#3} pour l’index du fichier de l’anneau.  
    >   
    > Dans cet exemple de fichier, les deux ordinateurs cibles sont définis avec la \<cible > élément. Chaque définition spécifie l’adresse IP avec \<ipv4 >, mais vous pouvez également utiliser l’adresse MAC (par exemple, < valeur de mac = « 11:22:33:44:55:66 »\/> ou < mac value="11-22-33-44-55-66 «\/>) ou le GUID SMBIOS (pour exemple, < valeur guid = « {269076F9-4B77-46E1-B03B-CA5003775B88} »\/>) pour identifier l’ordinateur cible. Notez également la clé de chiffrement (la même que celle qui a été spécifiée ou générée avec Bcdedit sur l’ordinateur cible) et le nom de l’ordinateur.  
  
4.  Entrez les détails de chaque ordinateur cible en tant que distinct \<cible > élément dans le fichier de configuration, puis enregistrer Newconfig.xml et fermez le bloc-notes.  
  
5.  Appliquez la nouvelle configuration avec `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result`. La sortie doit revenir avec le champ Réussite « true ». Si vous obtenez un autre résultat, voir la section Résolution des problèmes de cette rubrique.  
  
Vous pouvez toujours vérifier la configuration active avec `(Get-SbecActiveConfig).text`.  
  
Vous pouvez effectuer une vérification de validité sur le fichier de configuration avec `$result = (Get-Content .\newconfig.xml | Check-SbecConfig); $result`.  
  
Bien que la commande Windows PowerShell pour appliquer une nouvelle configuration met automatiquement à jour le service sans que vous ayez besoin de le redémarrer, vous pouvez toujours redémarrer le service vous-même à l'aide d'une de ces commandes :  
  
-   Avec Windows PowerShell : `Restart-Service BootEventCollector`  
  
-   Dans une invite de commandes ordinaire : **sc stop BootEventCollector; sc start BootEventCollector**  
  
## <a name="configuring-nano-server-as-a-target-computer"></a>Configuration de Nano Server comme ordinateur cible  
L’interface minimale offerte par Nano Server rend parfois difficile le diagnostic des problèmes. Vous pouvez configurer votre image Nano Server pour participer automatiquement à la collecte d’événements de configuration et de démarrage, en envoyant des données de diagnostic sur un ordinateur collecteur, sans autre intervention de votre part. Pour cela, procédez comme suit:  
  
#### <a name="to-configure-nano-server-as-a-target-computer"></a>Pour configurer Nano Server comme ordinateur cible  
  
1.  Créez votre image Nano Server de base. Pour plus d’informations, voir [Prise en main de Nano Server](https://technet.microsoft.com/library/mt126167.aspx).  
  
2.  Configurez un ordinateur collecteur comme dans la section « Configuration de l’ordinateur collecteur » de cette rubrique.  
  
3.  Ajoutez des clés de Registre du Journal automatique pour permettre l’envoi de messages de diagnostic. Pour ce faire, montez le disque dur virtuel de Nano Server créé à l’étape 1, chargez la ruche du Registre, puis ajoutez certaines clés de Registre. Dans cet exemple, l’image Nano Server est dans C:\NanoServer ; votre chemin d’accès peut être différent, donc ajustez les étapes en conséquence.  
  
    1.  Sur l’ordinateur collecteur, copiez le dossier **..\Windows\System32\WindowsPowerShell\v1.0\Modules\BootEventCollector** et collez-le dans le répertoire **..\Windows\System32\WindowsPowerShell\v1.0\Modules** sur l’ordinateur que vous utilisez pour modifier le disque dur virtuel de Nano Server.  
  
    2.  Démarrez la console Windows PowerShell avec des autorisations élevées et exécutez `Import-Module BootEventCollector`.  
  
    3.  Mettez à jour le Registre du disque dur virtuel de Nano Server pour activer les Journaux automatiques. Pour ce faire, exécutez `Enable-SbecAutoLogger -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd`. Cette opération ajoute une liste de base des événements de configuration et de démarrage les plus classiques ; vous pouvez en rechercher d’autres dans [Contrôle des sessions de suivi d’événements](https://msdn.microsoft.com/library/windows/desktop/aa363694(v=vs.85).aspx).  
  
4.  Mettez à jour les paramètres BCD de l’image Nano Server pour activer l’indicateur d’événements et configurez l’ordinateur collecteur pour assurer l'envoi des événements de diagnostic au serveur adéquat. Notez l’adresse IPv4 de l’ordinateur collecteur, le port TCP et la clé de chiffrement que vous avez configurés dans le fichier Active.XML du collecteur (décrit ailleurs dans cette rubrique). Utilisez cette commande dans une console Windows PowerShell avec des autorisations élevées : `Enable-SbecBcd -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd -CollectorIp 192.168.100.1 -CollectorPort 50000 -Key a.b.c.d`  
  
5.  Mettez à jour l’ordinateur collecteur pour recevoir les événements envoyés par l’ordinateur Nano Server en ajoutant la plage d’adresses IPv4, l’adresse IPv4 spécifique ou l’adresse MAC de Nano Server au fichier Active.XML sur l’ordinateur collecteur (voir la section « Configuration de l’ordinateur collecteur » de cette rubrique).  
  
## <a name="starting-the-event-collector-service"></a>Démarrage du service de collecteur d’événements  
Une fois qu'un fichier de configuration valide est enregistré sur l’ordinateur collecteur et qu'un ordinateur cible est configuré, dès le redémarrage de l’ordinateur cible, la connexion avec le collecteur est établie et les événements sont collectés.  
  
Vous trouverez le journal du service collecteur proprement dit (c'est-à-dire distinct des données de configuration et de démarrage collectées par le service) sous Microsoft-Windows-BootEvent-Collector/Admin. Pour une interface graphique des événements, utilisez l’Observateur d’événements. Créez un nouvel affichage ; développez **Journaux des applications et des services**, puis développez **Microsoft** et **Windows**. Trouvez **BootEvent-Collector**, développez-le et recherchez **Admin**.  

-   Avec Windows PowerShell : `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin`  
  
-   Dans une invite de commandes ordinaire : **wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
## <a name="troubleshooting"></a>Résolution des problèmes  
  
### <a name="troubleshooting-installation-of-the-feature"></a>Résolution des problèmes d’installation de la fonctionnalité  
  
||Erreur|Description de l’erreur|Symptôme|Problème potentiel|  
|-|---------|---------------------|-----------|---------------------|  
|Dism.exe|87|L’option du nom de la fonctionnalité n’est pas reconnue dans ce contexte||-   Cela peut se produire si le nom de la fonctionnalité a été mal orthographié. Vérifiez que vous l'avez correctement orthographié et réessayez.<br />-   Vérifiez que cette fonctionnalité est disponible sur la version du système d’exploitation que vous utilisez. Dans Windows PowerShell, exécutez **dism /online /get-features &#124; ?{$_ -match "boot"}** . Si aucune correspondance n’est renvoyée, vous exécutez probablement une version qui ne prend pas en charge cette fonctionnalité.|  
|Dism.exe|0x800f080c|Fonctionnalité \<nom > est inconnu.||Comme ci-dessus|  
  
### <a name="troubleshooting-the-collector"></a>Résolution des problèmes du collecteur  
  
**Journalisation :**  
Le collecteur enregistre ses propres événements en tant que fournisseur ETW Microsoft-Windows-BootEvent-Collector. C'est là que vous devez rechercher en premier pour résoudre les problèmes du collecteur. Vous pouvez les trouver dans l’Observateur d’événements sous Journaux des applications et des services > Microsoft > Windows > BootEvent-Collector > Admin, ou les lire dans une fenêtre de commande avec une de ces commandes :  
  
Dans une invite de commandes ordinaire : **wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
Dans une invite de commandes Windows PowerShell : `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin` (vous pouvez ajouter `-Oldest` pour renvoyer la liste dans l’ordre chronologique avec les événements les plus anciens en premier)  
  
Vous pouvez ajuster le niveau de détail dans les journaux avec « erreur », « avertissement », « Info » (par défaut), « détaillé » et « débogage ». Les niveaux plus détaillés que « info » sont utiles pour diagnostiquer les problèmes liés aux ordinateurs cible qui ne se connectent pas, mais ils peuvent générer une grande quantité de données. Par conséquent, utilisez-les avec précaution.  
  
Vous définissez le journal minimal niveau dans le \<collecteur > élément du fichier de configuration. Par exemple : < collecteur configVersionMajor = « 1 » minlog\=« commentaires » >.  
  
Le niveau détaillé consigne un enregistrement pour chaque paquet reçu comme il est traité. Le niveau de débogage ajoute davantage de détails sur le traitement et vide également le contenu de tous les paquets ETW reçus.  
  
Au niveau débogage, il peut être utile d’écrire le journal dans un fichier plutôt que d’essayer de l’afficher dans le système de journalisation habituel. Pour ce faire, ajoutez un élément supplémentaire dans le \<collecteur > élément du fichier de configuration :  
  
<collector configVersionMajor="1" minlog="debug" log\="c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt">  
      
 **Une approche conseillée pour la résolution des problèmes du collecteur :**  
   
1. Tout d’abord, vérifiez que le collecteur a reçu la connexion de la cible (il crée le fichier uniquement lorsque la cible commence à envoyer les messages) avec   
   ```  
   Get-SbecForwarding  
   ```  
   Si une connexion à cette cible est retournée, le problème vient peut être des paramètres du Journal automatique. Si rien n'est renvoyé, le problème vient d’abord de la connexion KDNET. Pour diagnostiquer les problèmes de connexion KDNET, essayez de vérifier la connexion aux deux extrémités (autrement dit, le collecteur et la cible).  
  
2. Pour afficher les diagnostics étendus à partir du collecteur, l’ajouter à la \<collecteur > élément du fichier de configuration :  
   \<collector ... minlog="verbose">  
   Cela activera les messages sur tous les paquets reçus.  
3. Vérifiez si des paquets sont reçus. Si vous le souhaitez, vous pouvez écrire le journal en mode détaillé directement dans un fichier plutôt que via ETW. Pour ce faire, ajoutez cette option pour le \<collecteur > élément du fichier de configuration :  
   \<collector ... minlog="verbose" log="c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt">  
      
4. Vérifiez si des messages sur les paquets reçus se trouvent dans les journaux des événements. Vérifiez si des paquets sont reçus. Si les paquets sont reçus mais sont incorrects, vérifiez les messages d’événements pour plus d’informations.  
5. À partir du côté cible, KDNET écrit des informations de diagnostic dans le Registre. Consulter   
   **HKLM\SYSTEM\CurrentControlSet\Services\kdnet** si des messages s'y trouvent.  
   KdInitStatus (DWORD) = 0 en cas de réussite et affiche un code d’erreur en cas d’erreur  
   KdInitErrorString = explication de l’erreur (contient également des messages d’information s'il n'y a aucune erreur)  
  
6. Exécutez Ipconfig.exe sur la cible et vérifiez le nom du périphérique qu'il indique. Si KDNET s'est chargé correctement, le nom du périphérique doit ressembler à « kdnic » au lieu du nom de la carte du fournisseur d’origine.  
7. Vérifiez si DHCP est configuré pour la cible. KDNET nécessite absolument DHCP.  
8. Vérifiez que le collecteur est sur le même réseau que la cible. Dans le cas contraire, vérifiez si le routage est configuré correctement, notamment le paramètre de passerelle par défaut pour DHCP.  
  
  
**État de la connexion**  
  
Vous pouvez consulter la liste actuelle des connexions établies et des informations sur l'emplacement vers lequel les données sont transférées avec `Get-SbecForwarding`.  
  
Vous pouvez également obtenir l’historique récent des modifications de statut dans les connexions avec `Get-SbecHistory`.  
  
### <a name="troubleshooting-setting-a-new-configuration"></a>Résolution des problèmes de définition d’une nouvelle configuration  
Si vous avez appliqué la configuration avec la commande Windows PowerShell `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result`, la variable `$result` contient des informations sur le déploiement. Vous pouvez interroger cette variable pour en obtenir différentes informations :  
  
Obtenez des informations sur les erreurs avec `$result.ErrorString`. Si des erreurs sont signalées ici, la nouvelle configuration n'a pas été appliquée et l’ancienne configuration est inchangée.  
  
Obtenez des avertissements avec `$result.WarningString`.  
  
Obtenez des informations sur les détails de la configuration avec `$result.InfoString`.  
  
Vous pouvez obtenir le résultat complet avec `$result | fl *`.  
Si vous ne souhaitez pas enregistrer le résultat dans une variable, vous pouvez également utiliser `Get-Content .\newconfig.xml | Set-SbecActiveConfig | fl *`.  
  
### <a name="troubleshooting-target-computers"></a>Résolution des problèmes d’ordinateurs cibles  
  
||Erreur|Description de l’erreur|Symptôme|Problème potentiel|  
|-|---------|---------------------|-----------|---------------------|  
|Ordinateur cible||La cible se connecte pas au collecteur||-   L’ordinateur cible n’a pas été redémarré une fois configuré. Redémarrez l’ordinateur cible.<br />-   L’ordinateur cible comporte des paramètres BCD incorrects. Vérifiez les paramètres dans la section « Valider les paramètres de l’ordinateur cible ». Corriger si nécessaire, puis redémarrez l’ordinateur cible.<br />-   Le pilote de KDNET/EVENT-NET n’a pas pu se connecter à une carte réseau ou s'est connecté à la mauvaise carte réseau. Dans Windows PowerShell, exécutez `gwmi Win32_NetworkAdapter` et vérifiez la sortie avec le ServiceName **kdnic**. Si la mauvaise carte réseau est sélectionnée, refaites les étapes décrites dans « Pour spécifier une carte réseau ». Si la carte réseau n’apparaît pas du tout, il se peut que le pilote ne prend en charge aucune de vos cartes réseau.<br>**Voir aussi** « Méthode suggérée pour résoudre les problèmes de collecteur » ci-dessus, en particulier les étapes 5 à 8.|  
|Collecteur||Je ne vois plus aucun événement après la migration de la machine virtuelle sur laquelle mon collecteur est hébergé.||Vérifiez que l’adresse IP de l’ordinateur collecteur n’a pas changé. Si tel est le cas, consultez « Pour activer l’envoi des événements ETW via le transport à distance ».|  
|Collecteur||Les fichiers ETL ne sont pas créés.|`Get-SbecForwarding` Indique que la cible est connecté, sans aucune erreur, mais les fichiers ETL ne sont pas créés.|L’ordinateur cible n'a probablement pas encore envoyé toutes les données ; les fichiers ETL ne sont créés que lors de la réception de données.|  
|Collecteur||Un événement n’est pas visible dans le fichier ETL.|L’ordinateur cible a envoyé un événement, mais lorsque le fichier ETL est lu avec l’Observateur d’événements de l’Analyseur de messages, l’événement n’est pas présent.|-   L’événement peut encore se trouver dans la mémoire tampon. Les événements ne sont pas écrits dans le fichier ETL tant qu'une mémoire tampon complète de 64 Ko n'est pas collectée ou avant un délai d’environ 10 à 15 secondes sans qu'aucun nouvel événement ne se produise. Attendez l'expiration du délai ou videz les mémoires tampon avec `Save-SbecInstance`.<br />-   Le manifeste de l’événement n’est pas disponible sur l’ordinateur collecteur ou l’ordinateur sur lequel s’exécute l’Observateur d’événements ou l’Analyseur de messages.  Dans ce cas, le collecteur peut ne pas être en mesure de traiter l’événement (vérifiez le journal du collecteur) ou la visionneuse ne parvient peut-être pas à l'afficher.  Il est recommandé d’installer tous les manifestes sur l’ordinateur collecteur et d'installer les mises à jour sur l’ordinateur collecteur avant de les installer sur les ordinateurs cibles.|
