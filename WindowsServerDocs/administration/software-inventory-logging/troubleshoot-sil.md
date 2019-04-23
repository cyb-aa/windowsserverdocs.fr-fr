---
title: Résolution des problèmes de la journalisation de l’inventaire logiciel
description: Décrit comment résoudre les problèmes de déploiement de journalisation de l’inventaire logiciel courants.
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
author: brentfor
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 6ea8a336e2c40b55e2ad6b508d89db7dcb668315
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832850"
---
# <a name="troubleshoot-software-inventory-logging"></a>Résolution des problèmes de la journalisation de l’inventaire logiciel 

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

##<a name="understanding-sil"></a>Présentation de SIL

Avant de commencer la résolution des problèmes de SIL, doit avoir une bonne compréhension de ses composants et son fonctionnement. Les vidéos suivantes donnent une vue d’ensemble de SIL et SIL Aggregator et comment les utiliser pour transférer et données d’inventaire de rapport :

1. [Introduction à la journalisation SIL () (10:57) de l’inventaire logiciel](https://channel9.msdn.com/Blogs/Regular-IT-Guy/An-Introduction-to-Software-Inventory-Logging-SIL)

2. [Logiciel de journalisation de l’inventaire : Configuration de SIL Aggregator (34:14)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Setting-up-SIL-Aggregator)

3. [Logiciel de journalisation de l’inventaire : Activation du transfert de SIL (7:20)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Enabling-SIL-Forwarding)

##<a name="how-sil-data-flow-works"></a>Comment les flux de données SIL fonctionne

L’infrastructure SIL a deux composants principaux et deux canaux de communication. Flux de données sur les deux canaux et entre les deux composants, est nécessaire pour réussir le déploiement SIL (Cela suppose un environnement virtualisé ou cloud,--environnements purement physiques doivent uniquement un des canaux de communication). Vous devez comprendre les composants et les flux de données de SIL pour déployer correctement. Après avoir regardé les vidéos de présentation ci-dessus, vous avez vu le diagramme architectural qui illustre les composants et le flux de données sur les deux canaux. Flèches oranges indiquent les requêtes à distance via WinRM, des flèches en pointillés vertes indiquent le que HTTPS valide à SIL Aggregator à partir de SIL dans chaque nœud de fin WS :

![](../media/software-inventory-logging/image1.png)

Si vous rencontrez un problème avec SIL, il est probablement lié à une interruption dans le flux de données sur les canaux et entre les composants. Voici les problèmes les plus courants liés au flux de données, suivie dans la section suivante les étapes de dépannage pour résoudre de chacun des trois problèmes :

-   **Problème 1 de flux de données** – **aucune donnée dans le rapport lorsque vous utilisez l’applet de commande Publish-SilReport** (ou généralement les données manquantes).

-   **Erreur 2 du flux de données** – **trop nombreux serveurs sous hôte inconnu** dans le rapport.

-   **Problème 3 de flux de données** – **trop nombreuses machines virtuelles sous des hôtes physiques répertoriés en tant que système d’exploitation inconnu** dans le rapport et/ou une erreur généré lorsque vous utilisez **Publish-SilData** sur les serveurs Windows en cours d’exécution SIL.

##<a name="troubleshooting-data-flow-issues"></a>Résolution des problèmes de flux de données

Avant de commencer, vous devez savoir comment il y a longtemps SIL Aggregator démarrer avec le **Start-SilAggregator** applet de commande.

>[!IMPORTANT]
>Il n’y aura aucune donnée dans le rapport jusqu'à ce que le cube de données SQL est traité au moment de système local de 3 h 00. Ne poursuivez pas avec les étapes de dépannage jusqu'à ce que le cube a traité des données.

Si vous dépannez des données dans le rapport (ou manquants dans le rapport) qui sont plus récentes que la dernière fois le cube traité ou avant que le cube a déjà traité (pour une nouvelle installation), suivez ces étapes pour traiter le cube de données SQL en temps réel :

1. Connectez-vous en tant qu’administrateur de SQL Server et exécutez **SSMS** à une invite de commandes.
2. Connectez-vous au moteur de base de données.
3. Développez **Agent SQL Server** puis **travaux**.
4. Bouton droit sur **SILStagingRefresh** , puis sélectionnez **Start Job at Step**.
5. Cliquez sur **Démarrer** et attendez que la barre de progression de l’actualisation terminer.
6. Ouvrez PowerShell en tant qu’administrateur et exécutez le **silreport publier - openreport** applet de commande.

Si aucune donnée n’est toujours dans le rapport, passez à résoudre les problèmes de flux de données de trois.

###<a name="data-flow-issue-1"></a>Problème de flux de données 1 

####<a name="no-data-in-the-report-when-using-the-publish-silreport-cmdlet-or-data-is-generally-missing"></a>Aucune donnée dans le rapport lorsque vous utilisez l’applet de commande Publish-SilReport (ou les données sont généralement manquantes)

Si les données sont manquantes, il est probablement ne pas avoir encore traité le cube de données SQL. Si a été traitée récemment et que vous pensez que les données manquantes doivent sont arrivés à l’Aggregator avant le traitement du cube, suivez le chemin d’accès des données dans l’ordre inverse. Choisir un hôte unique et une machine virtuelle unique pour résoudre les problèmes. Le chemin d’accès de données dans l’ordre inverse serait **SILA rapport** &lt; **base de données SILA** &lt; **répertoire local SILA** &lt;  **hôte physique distant** ou **VM WS SIL/tâche de l’agent en cours d’exécution**.

####<a name="check-to-see-if-data-is-in-the-database"></a>Vérifier a posteriori si les données sont dans la base de données

Il existe deux façons pour vérifier les données : **PowerShell** ou **SSMS**.

>[!Important]
>Si le cube a au moins une fois traité dans la mesure où SILA inséré des données dans la base de données, ces données doivent être répercutées dans le rapport. Si aucune donnée dans la base de données puis d’interrogation hôtes physiques ne parvient pas, ou rien n’est reçu via HTTPS, ou les deux.

 ####<a name="powershell"></a>PowerShell

1. Ouvrez PowerShell en tant qu’administrateur et exécutez le **get-silvmhost** applet de commande, puis exécutez **get-silaggregator**.

    >[!NOTE]
    >La sortie de **get-silaggregator** seront toujours imiter le **onglet de détails de Windows Server** du rapport Excel. Ne pensez pas un résultat différent.

2. Exécutez **get-silvmhost**
 - Si il aucun hôte n’est répertorié, puis ajoutez les hôtes à l’aide du **ajouter-silvmhost** applet de commande.
 - Si les hôtes sont répertoriés en tant que **inconnu**, puis accédez à problème 2. d - si les hôtes sont répertoriés, mais aucun **datetime** sous le **interrogation récente** colonne, puis accédez à **problème 2** ci-dessous.

**Autres commandes associées**

**Get-SilAggregator - Computername &lt;nom de domaine complet d’un serveur connu transmettre des données&gt;**: Cela génère des informations à partir de la base de données sur un ordinateur (machine virtuelle) avant même que le cube a traité. Par conséquent cette applet de commande peut être utilisé pour vérifier des données dans la base de données pour une transmission des données SIL sur HTTPS, avant ou sans le processus de cube à 3 h 00 (ou si vous n’avez pas actualisé le cube en temps réel comme décrit au début de cette section) de Windows Server.

**Get-SilAggregator - VmHostName &lt;nom de domaine complet d’un hôte physique interrogée lorsqu’il existe une valeur sous la colonne interrogation récente lorsque vous utilisez l’applet de commande Get-SilVmHost&gt;**: Cela génère des informations à partir de la base de données sur un hôte physique avant même que le cube a traité.

####<a name="ssms"></a>SSMS

n**vérifier les données à partir d’hôtes interrogées :**
 
1. Ouvrez **SSMS** et se connecter à la **moteur de base de données**.
2. Développez **bases de données**, développez le **SoftwareInventoryLogging** de base de données, développez **Tables**, avec le bouton droit cliquez sur le **HostInfo** table, puis Sélectionner les 1000 premières lignes. 

    S’il existe des données pour un ou plusieurs hôtes dans la table, puis d’interrogation pour les hôtes celui ou ceux a réussi au moins une fois.

 **Vérifiez les données à partir de machines virtuelles ou des serveurs autonomes, qui envoie les données via le protocole HTTPS :** 

1. Ouvrez **SSMS** et se connecter à la **moteur de base de données**.
a2. Développez **bases de données**, développez le **SoftwareInventoryLogging** de base de données, développez **Tables**, avec le bouton droit cliquez sur le **VMInfo** table, puis Sélectionner les 1000 lignes du haut. 

    >[!NOTE] 
    >Chaque ligne d’une machine virtuelle unique représente un traitement **bmil** fichier correctement transmis via le protocole HTTPS et traités par SIL Aggregator. Les fichiers Bmil des fichiers propriétaires utilisés par SIL, un est créé chaque notre par chaque instance SIL Remarque que cela est nécessaire uniquement lorsque SIL et SILA sont utilisés dans les environnements virtuels. Sinon, seul le trafic HTTPS est attendu/nécessaire).

 Toutes les données dans la base de données doivent être répercutées dans les rapports SIL une fois que le cube a traité.

###<a name="data-flow-issue-2"></a>Problème de flux de données 2
####<a name="too-many-servers-under-unknown-host"></a>Trop de serveurs sous hôte inconnu

Cela est susceptible de se produire dans les environnements virtuels lors de SIL Aggregator n’est pas correctement l’interrogation des hôtes physiques qui hébergent les machines virtuelles.

1. Ouvrez **PowerShell** en tant qu’administrateur et exécutez le **get-silvmhost** applet de commande.

    -   Si les hôtes sont répertoriés en tant que **inconnu**, le **ajouter-silvmhost** applet de commande n’a pas fonctionné correctement, généralement en raison d’informations d’identification incorrectes ajoutées pour l’accès à ces hôtes (par conséquent, inconnu). Mais si les informations d’identification sont vérifiées, cela peut signifier la détection automatique de **hosttype** et **hypervisortype** dans le **ajouter-silvmhost** applet de commande n’a pas été capable de reconnaître le plateforme. Il sont de dépannage avancées disponibles pour ces situations, mais ne sont pas couverts ici (consultez les canaux de l’Observateur d’événements SIL Aggregator).

    -   Si les hôtes sont répertoriés, et **hosttype** et **hypervisortype** sont répertoriés avec des valeurs qui ne sont pas **inconnu**, c'est-à-dire Windows et Hyper-v, ou Ubuntu et Xen, etc., mais il n’est pas **datetime** sous **interrogation récente** colonne, puis d’interrogation n’est pas correctement apparue encore.

        -   Vous aurez besoin d’attendre une heure après l’ajout de l’hôte pour l’interrogation pour se produire (en supposant que cet intervalle est défini par défaut – peut être vérifiée à l’aide de la **get-silaggregator** applet de commande).

        -   S’il a été une heure dans la mesure où l’ordinateur hôte a été ajouté, vérifiez que la tâche d’interrogation est en cours d’exécution : Dans **Planificateur de tâches**, sélectionnez **Software Inventory Logging Aggregator** sous **Microsoft** &gt; **Windows** et vérifier l’historique de la tâche.

    -   Si un hôte est répertorié, mais il n’existe aucune valeur pour **RecentPoll**, **HostType**, ou **HypervisorType**, cela peut être en grande partie ignorée. Cela se produit uniquement dans les environnements Hyper-v. Ces données proviennent réellement de la machine virtuelle Windows Server, identifier l’hôte physique, qu'il est en cours d’exécution via le protocole HTTPS. Cela peut être utile d’identifier une machine virtuelle spécifique qui est signalé, mais nécessite d’exploration de données la base de données à l’aide de la **Get-SilAggregatorData** applet de commande.

Une fois que les hôtes sont interrogation correctement, vous serez en mesure de voir les données de ces hôtes physiques dans la base de données SILA où il existe un **datetime** sous **interrogation récente**. La section problème 1 ci-dessus fournit des étapes pour récupérer ces données.

###<a name="data-flow-issue-3"></a>Problème de flux de données 3
####<a name="too-many-physical-hosts-with-vms-listed-as-unknown-os"></a>Trop d’hôtes physiques avec des machines virtuelles répertoriés en tant que système d’exploitation inconnu

1. Choisissez un nœud Windows Server fin (VM) qui figure sur l’un de ces hôtes, connectez-vous en tant qu’administrateur.
2. Ouvrez PowerShell en tant qu’administrateur.
3. Vérifiez **SilLogging** est en cours d’exécution à l’aide de la **Get-SilLogging** applet de commande.
 - Si en cours d’exécution, essayez d’envoyer manuellement des données SIL à l’aide de **Publish-SilData**.

  - S’il existe une erreur :
     - Vérifiez le **targeturi** a **https://** dans l’entrée.
     - Vérifiez que toutes les conditions préalables sont remplies. 
     - Vérifiez que toutes les mises à jour requises pour Windows Server sont installés (voir les conditions préalables pour SIL). Un moyen rapide de vérifier (sur WS 2012 R2 uniquement) consiste à recherchez ces éléments à l’aide de l’applet de commande suivante : **Get-SilWindowsUpdate \*3060, \*3000**
     - Vérifiez le certificat utilisé pour l’authentification auprès de l’aggregator est installé dans le magasin approprié sur le serveur local à inventorier avec **SilLogging**.
     - Sur le SIL Aggregator, veillez à l’empreinte numérique du certificat utilisé pour l’authentification auprès de l’aggregator est ajoutée à la liste à l’aide du **Set-SilAggregator** **– AddCertificateThumbprint** l’applet de commande.
     - Si vous utilisez des certificats d’entreprise, vérifiez que le serveur sur lequel SIL est activé est joint au domaine pour lequel le certificat a été créé, ou qu’il peut être vérifié avec une autorité racine. Si un certificat n’est pas approuvé sur l’ordinateur local qui tente de transférer/envoyer des données à un Aggregator, cette action échoue avec une erreur.

    Si tous les éléments ci-dessus a été vérifiée et vérifié, mais le problème persiste :

    - Vérifiez que le certificat utilisé pour installer SIL Aggregator est intègre et qu’il correspond au nom du serveur SIL Aggregator lui-même. En outre, si les certificats d’entreprise sont utilisés pour l’installation de SIL Aggregator, l’Aggregator devra peut-être être joint au domaine dans lequel le certificat a été créé (ces étapes sont inutiles si d’autres machines sont correctement transférées vers le même SIL Aggregator).

    -  Enfin, vous pouvez vérifier l’emplacement suivant pour les fichiers SIL mis en cache sur le serveur tente de transférer/envoyer, **\Windows\System32\Logfiles\SIL**. Si **SilLogging** a démarré et a été exécutée pendant plus d’une heure, ou **Publish-SilData** a été exécuté récemment, et il n’y a aucun fichier dans ce répertoire, que la journalisation à l’Aggregator a été réussie.

S’il existe aucune erreur et aucune sortie sur la console, puis les push/publication de données à partir du nœud de fin de Windows Server à SIL Aggregator via le protocole HTTPS a été réussie. Pour suivre le chemin d’accès de la connexion vers l’avant, de données à SIL Aggregator en tant qu’administrateur et examiner les fichiers de données qui sont arrivés. Accédez à **Program Files (x86)** &gt; **Microsoft SIL Aggregator** &gt; répertoire SILA. Vous pouvez regarder les fichiers de données qui arrivent en temps réel.

>[!NOTE] 
>Plusieurs fichiers de données ont été transférés avec le **Publish-SilData** applet de commande. SIL sur le nœud de fin met en cache de notifications Push ayant échoué pendant 30 jours. Lors de la prochaine publication réussie, tous les fichiers de données passera à l’agrégation pour le traitement. De cette façon, un qui vient d’être définie de SIL Aggregator peut afficher les données à partir d’un nœud de fin bien avant son propre programme d’installation.

>[!NOTE] 
>Il existe des règles que SILA suit lors du traitement des fichiers de données dans le répertoire SILA qui sont uniquement pertinents dans les situations de faible trafic. Un trafic élevé sera toujours déclencher le traitement en temps réel. Le comportement par défaut est que le traitement commencera soit après que 100 fichiers arrivent dans le répertoire, ou après 15 minutes. Lors du dépannage de bout en bout dans un environnement de petite taille, il est souvent nécessaire d’attendre 15 minutes.

Une fois ces fichiers sont traités, vous verrez les données dans la base de données.
