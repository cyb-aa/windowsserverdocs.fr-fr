---
title: Résolution des problèmes de la journalisation de l’inventaire logiciel
description: Décrit comment résoudre les problèmes courants de déploiement de la journalisation de l’inventaire logiciel.
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.topic: article
author: brentfor
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 5a02caf63bbd02705aebb8306a7b50a32f3d6c82
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851412"
---
# <a name="troubleshoot-software-inventory-logging"></a>Résolution des problèmes de la journalisation de l’inventaire logiciel 

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

## <a name="understanding-sil"></a>Fonctionnement de SIL

Avant de commencer à dépanner SIL, vous devez avoir une bonne compréhension de ses composants et de son fonctionnement. Les vidéos suivantes offrent une vue d’ensemble de SIL et de l’agrégateur SIL, et expliquent comment les utiliser pour transférer et signaler des données d’inventaire :

1. [Présentation de la journalisation de l’inventaire logiciel (SIL) (10:57)](https://channel9.msdn.com/Blogs/Regular-IT-Guy/An-Introduction-to-Software-Inventory-Logging-SIL)

2. [Journalisation de l’inventaire logiciel : configuration de l’agrégateur SIL (14:34)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Setting-up-SIL-Aggregator)

3. [Journalisation de l’inventaire logiciel : activation du transfert SIL (7:20)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Enabling-SIL-Forwarding)

## <a name="how-sil-data-flow-works"></a>Fonctionnement du SIL Data Flow

L’infrastructure SIL a deux composants principaux et deux canaux de communication. Le transit de données sur les deux canaux, et entre les deux composants, est nécessaire pour un déploiement SIL réussi (cela suppose un environnement virtualisé, ou Cloud, les environnements purement physiques n’ont besoin que d’un seul canal de communication). Vous devez comprendre les composants et le flot de données de SIL pour les déployer correctement. Après avoir regardé les vidéos de présentation ci-dessus, vous aurez vu le diagramme architectural qui illustre les composants et le Flow de données sur les deux canaux. Les flèches orange indiquent les requêtes distantes sur WinRM, les flèches en pointillés vertes indiquent les publications HTTPs à l’agrégateur SIL à partir de SIL dans chaque nœud WS end :

![](../media/software-inventory-logging/image1.png)

Si vous rencontrez un problème avec SIL, il est probable qu’il soit lié à une interruption du workflow des données sur les canaux et entre les composants. Voici les problèmes les plus courants liés au Data Flow, suivis dans la section suivante des étapes de dépannage permettant de résoudre chacun des trois problèmes :

-   **Problème de workflow de données 1** - **aucune donnée dans le rapport lors de l’utilisation de l’applet de commande Publish-SilReport** (ou des données généralement manquantes).

-   **Problème de Data Flow 2** : **trop de serveurs sous un hôte inconnu** dans le rapport.

-   **Problème de transmission de données 3** : **nombre de machines virtuelles trop important dans les hôtes physiques répertoriés en tant que système d’exploitation inconnu** dans le rapport et/ou erreur produite lors de l’utilisation de **Publish-SilData** sur les serveurs Windows exécutant sil.

## <a name="troubleshooting-data-flow-issues"></a>Résolution des problèmes de workflow de données

Avant de commencer, vous devez connaître la durée de démarrage de l’agrégateur SIL à l’aide de l’applet de commande **Start-SilAggregator** .

>[!IMPORTANT]
>Il n’y aura pas de données dans le rapport tant que le cube de données SQL ne sera pas traité à l’heure du système local 3:00. Ne poursuivez pas les étapes de dépannage tant que le cube n’a pas traité les données.

Si vous dépannez des données dans le rapport (ou manquantes dans le rapport) qui sont plus récentes que la dernière fois que le cube a été traité, ou avant le traitement du cube (pour une nouvelle installation), procédez comme suit pour traiter le cube de données SQL en temps réel :

1. Connectez-vous en tant qu’administrateur de SQL Server et exécutez **SSMS** à partir d’une invite de commandes.
2. Connectez-vous au moteur de base de données.
3. Développez **SQL Server Agent** puis développez **travaux**.
4. Cliquez avec le bouton droit sur **SILStagingRefresh** , puis sélectionnez **Démarrer le travail à l’étape**.
5. Cliquez sur **Démarrer** et attendez la fin de la barre de progression de l’actualisation.
6. Ouvrez PowerShell en tant qu’administrateur et exécutez l’applet de commande **Publish-silreport-OpenReport** .

S’il n’y a toujours pas de données dans le rapport, poursuivez avec le dépannage des trois problèmes de Workflow.

### <a name="data-flow-issue-1"></a>Problème de transmission de données 1 

#### <a name="no-data-in-the-report-when-using-the-publish-silreport-cmdlet-or-data-is-generally-missing"></a>Aucune donnée dans le rapport lors de l’utilisation de l’applet de commande Publish-SilReport (ou les données sont généralement manquantes)

Si des données sont manquantes, cela est probablement dû au fait que le cube de données SQL n’a pas encore été traité. S’il a récemment été traité et que vous pensez que des données manquantes doivent avoir été reçues au niveau de l’agrégateur avant le traitement du cube, suivez le chemin d’accès aux données dans l’ordre inverse. Choisissez un ordinateur hôte unique et une machine virtuelle unique à dépanner. Le chemin d’accès aux données dans l’ordre inverse est **Sila Report** &lt; **base de données Sila** &lt; **Sila répertoire local** &lt; **hôte physique distant** ou la **machine virtuelle WS exécutant sil agent/tâche**.

#### <a name="check-to-see-if-data-is-in-the-database"></a>Vérifier si les données se trouvent dans la base de données

Il existe deux façons de rechercher des données : **PowerShell** ou **SSMS**.

>[!Important]
>Si le cube a été traité au moins une fois depuis que SILA inséré des données dans la base de données, ces données doivent être reflétées dans le rapport. S’il n’y a pas de données dans la base de données, l’interrogation des hôtes physiques échoue, la réception de la valeur HTTPs, ou les deux.

 #### <a name="powershell"></a>PowerShell

1. Ouvrez PowerShell en tant qu’administrateur et exécutez l’applet de commande **silvmhost** , puis exécutez la commande **-silaggregator**.

    >[!NOTE]
    >La sortie de l’opération de récupération de l’état **-silaggregator** reproduit toujours l' **onglet Détails de Windows Server** du rapport Excel. Ne vous attendez pas à obtenir un résultat différent.

2. Exécuter la **silvmhost**
   - Si aucun ordinateur hôte n’est répertorié, ajoutez des ordinateurs hôtes à l’aide de l’applet **de commande Add-silvmhost** .
   - Si les ordinateurs hôtes sont répertoriés comme étant **inconnus**, passez au problème 2. 
   d-si les hôtes sont répertoriés, mais qu’il n’y a pas de **date/heure** dans la colonne **sondage récent** , passez au **problème 2** ci-dessous.

**Autres commandes associées**

**Obtenir-SilAggregator-Computername &lt;nom de domaine complet d’un serveur connu exécutant un push de données&gt;** : cette opération génère des informations à partir de la base de données sur un ordinateur (machine virtuelle) avant le traitement du cube. Cette applet de commande peut donc être utilisée pour vérifier les données de la base de données d’un serveur Windows qui transmet les données SIL sur HTTPs, avant ou sans, le processus de cube à 3:00 (ou si vous n’avez pas actualisé le cube en temps réel comme décrit au début de cette section).

**Obtenir-SilAggregator-VmHostName &lt;nom de domaine complet d’un hôte physique interrogé où il existe une valeur sous la colonne sondage récente lors de l’utilisation de l’applet de commande SilVmHost-&gt;** : cette opération génère des informations à partir de la base de données sur un hôte physique même avant le traitement du cube.

#### <a name="ssms"></a>SSMS

n**vérifier les données des hôtes en cours d’interrogation :**
 
1. Ouvrez **SSMS** et connectez-vous au **moteur de base de données**.
2. Développez successivement **bases de données**, la base de données **base softwareinventorylogging** , **tables**, cliquez avec le bouton droit sur la table **hostInfo** , puis sélectionnez les 1000 premières lignes. 

    S’il existe des données pour un ou plusieurs ordinateurs hôtes dans la table, l’interrogation de ce ou ces hôtes a réussi au moins une fois.

   **Vérifiez les données des machines virtuelles ou des serveurs autonomes qui ont envoyé des données sur HTTPs :** 

3. Ouvrez **SSMS** et connectez-vous au **moteur de base de données**.
   R2. Développez **bases de données**, développez la base de données **base softwareinventorylogging** , développez **tables**, cliquez avec le bouton droit sur la table **VMInfo** , puis sélectionnez les 1000 lignes du haut. 

    >[!NOTE] 
    >Chaque ligne d’une machine virtuelle unique représente un fichier **BMIL** traité correctement envoyé via HTTPS et traité par l’agrégateur sil. Les fichiers BMIL sont des fichiers propriétaires utilisés par SIL, chacun d’entre eux étant créé par chaque instance de SIL. Notez qu’il n’est nécessaire que lorsque SIL et SILA sont utilisés dans des environnements virtuels. Dans le cas contraire, seul le trafic HTTPs est nécessaire/attendu).

   Toutes les données de la base de données doivent être reflétées dans les rapports de SIL une fois que le cube a été traité.

### <a name="data-flow-issue-2"></a>Problème de workflow 2
#### <a name="too-many-servers-under-unknown-host"></a>Trop de serveurs sous un hôte inconnu

Cela peut se produire dans les environnements virtuels quand SIL Aggregater n’interroge pas correctement les hôtes physiques qui hébergent les ordinateurs virtuels.

1. Ouvrez **PowerShell** en tant qu’administrateur et exécutez l’applet de commande **silvmhost** .

    -   Si les ordinateurs hôtes sont répertoriés comme étant **inconnus**, l’applet de commande **Add-silvmhost** ne fonctionnait pas correctement : généralement, en raison d’informations d’identification incorrectes pour l’accès à ces ordinateurs hôtes (par conséquent, inconnu). Toutefois, si les informations d’identification sont vérifiées, cela peut signifier que la détection automatique de **HostType** et **hypervisortype** dans l’applet de commande **Add-silvmhost** n’a pas pu reconnaître la plateforme. Des étapes de dépannage avancées sont disponibles pour ces situations, mais elles ne sont pas abordées ici (consultez les canaux de l’agrégateur observateur SIL).

    -   Si les ordinateurs hôtes sont répertoriés et que **HostType** et **hypervisortype** sont répertoriés avec des valeurs qui ne sont pas **inconnues**, par exemple Windows et HyperV, ou Ubuntu et Xen, etc., mais il n’existe pas de valeur **DateTime** sous la colonne d' **interrogation récente** , alors l’interrogation n’a pas encore été effectuée.

        -   Vous devez attendre une heure après avoir ajouté l’hôte pour l’interrogation (en supposant que cet intervalle est défini sur par défaut – peut être vérifié à l’aide de l’applet de commande Set **-silaggregator** ).

        -   S’il y a eu une heure depuis que l’ordinateur hôte a été ajouté, vérifiez que la tâche d’interrogation est en cours d’exécution : dans **Planificateur de tâches**, sélectionnez **agrégateur de journalisation de l’inventaire logiciel** sous **Microsoft** &gt; **Windows** et vérifiez l’historique de la tâche.

    -   Si un ordinateur hôte est listé, mais qu’il n’existe aucune valeur pour **RecentPoll**, **HostType**ou **HypervisorType**, cela peut être ignoré en grande partie. Cela ne se produit que dans les environnements HyperV. Ces données proviennent réellement de la machine virtuelle Windows Server, identifiant l’hôte physique qu’elle exécute sur HTTPs. Cela peut être utile pour identifier une machine virtuelle spécifique qui signale, mais nécessite l’exploration de la base de données à l’aide de l’applet de commande **SilAggregatorData** .

Une fois les hôtes correctement interrogés, vous pouvez voir les données de ces hôtes physiques dans la base de données SILA où il existe une valeur **DateTime** sous **sondage récent**. La section issue 1 ci-dessus fournit les étapes de récupération de ces données.

### <a name="data-flow-issue-3"></a>Problème de workflow de données 3
#### <a name="too-many-physical-hosts-with-vms-listed-as-unknown-os"></a>Trop d’hôtes physiques avec des machines virtuelles répertoriées comme système d’exploitation inconnu

1. Sélectionnez un nœud de terminaison Windows Server (machine virtuelle) que vous connaissez sur l’un de ces hôtes, connectez-vous en tant qu’administrateur.
2. Ouvrez PowerShell en tant qu’administrateur.
3. Vérifiez que **SilLogging** est en cours d’exécution à l’aide de l’applet de commande **-SilLogging** .
   - Si vous exécutez, essayez d’envoyer manuellement des données SIL à l’aide de **Publish-SilData**.

   - En cas d’erreur :
     - Vérifiez que l’élément **targetUri** contient **https://** dans l’entrée.
     - Vérifier que toutes les conditions préalables sont remplies 
     - Assurez-vous que toutes les mises à jour nécessaires pour Windows Server sont installées (voir conditions préalables pour SIL). Un moyen rapide de vérifier (sur WS 2012 R2 uniquement) consiste à les Rechercher à l’aide de l’applet de commande suivante : **SilWindowsUpdate \*3060, \*3000**
     - Assurez-vous que le certificat utilisé pour l’authentification auprès de l’agrégateur est installé dans le magasin approprié sur le serveur local à inventorier avec **SilLogging**.
     - Sur l’agrégateur SIL, vérifiez que l’empreinte numérique du certificat utilisé pour l’authentification auprès de l’agrégateur est ajoutée à la liste à l’aide de l’applet de commande **Set-SilAggregator** **– AddCertificateThumbprint** .
     - Si vous utilisez des certificats d’entreprise, vérifiez que le serveur sur lequel SIL est activé est joint au domaine pour lequel le certificat a été créé, ou qu’il peut être vérifié avec une autorité racine. Si un certificat n’est pas approuvé sur l’ordinateur local qui tente de transférer/envoyer des données à un Aggregator, cette action échoue avec une erreur.

     Si tous les éléments ci-dessus ont été vérifiés et vérifiés, mais que le problème persiste :

     - Vérifiez que le certificat utilisé pour installer l’agrégateur SIL est sain et qu’il correspond au nom du serveur d’agrégation SIL lui-même. En outre, si les certificats d’entreprise sont utilisés pour l’installation de SIL Aggregator, l’Aggregator devra peut-être être joint au domaine dans lequel le certificat a été créé (ces étapes sont inutiles si d’autres machines sont correctement transférées vers le même SIL Aggregator).

     -  Enfin, vous pouvez vérifier l’emplacement suivant pour les fichiers SIL mis en cache sur le serveur qui tente de transférer/envoyer, **\Windows\System32\Logfiles\SIL**. Si **SilLogging** a démarré et s’exécute depuis plus d’une heure, ou que **Publish-SilData** a été exécuté récemment et qu’il n’y a aucun fichier dans ce répertoire, la journalisation dans l’agrégateur a réussi.

S’il n’y a pas d’erreur et qu’aucune sortie ne s’affiche dans la console, le push/publication des données à partir du nœud de fin Windows Server vers l’agrégateur SIL sur HTTPs a réussi. Pour suivre le chemin d’accès aux données, connectez-vous à l’agrégateur SIL en tant qu’administrateur, puis examinez le ou les fichiers de données qui sont arrivés. Accédez à **Program Files (x86)** &gt; l' **agrégateur Microsoft sil** &gt; répertoire Sila. Vous pouvez surveiller les fichiers de données arrivant en temps réel.

>[!NOTE] 
>Plusieurs fichiers de données ont peut-être été transférés avec l’applet de commande **Publish-SilData** . SIL sur le nœud de fin met en cache les notifications push ayant échoué pendant 30 jours. À la prochaine transmission Push réussie, tous les fichiers de données seront envoyés à l’agrégateur pour traitement. De cette façon, un agrégateur SIL nouvellement configuré peut afficher les données d’un nœud de fin avant sa propre configuration.

>[!NOTE] 
>Des règles SILA suivent lors du traitement des fichiers de données dans le répertoire SILA qui ne sont pertinents que dans des situations de faible trafic. Le trafic élevé déclenchera toujours le traitement en temps réel. Le comportement par défaut est que le traitement commence après l’arrivée de 100 fichiers dans le répertoire ou après 15 minutes. Lors du dépannage de bout en bout dans un environnement de petite taille, il est souvent nécessaire d’attendre 15 minutes.

Une fois ces fichiers traités, vous verrez les données dans la base de données.
