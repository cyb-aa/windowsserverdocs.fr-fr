---
title: Agrégateur de journalisation de l’inventaire logiciel
description: Décrit comment installer et gérer l’agrégateur de journalisation de l’inventaire logiciel
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4230a75-6bcd-47d9-ba92-a052a90a6abc
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0edecb86c7d5afa7d267c75ec858ded9af36e4c0
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866319"
---
# <a name="software-inventory-logging-aggregator"></a>Agrégateur de journalisation de l’inventaire logiciel

>S'applique à : Windows Server 2012 R2

## <a name="what-is-software-inventory-logging-aggregator"></a>Qu’est-ce que l’agrégateur de journalisation de l’inventaire logiciel ?
Software Inventory Logging Aggregator (SILA) reçoit, compile et produit des rapports simples sur le nombre et le type de logiciels professionnels Microsoft installés sur les serveurs Windows d’un centre de données.

SILA est un logiciel que vous installez sur Windows Server, mais qui n’est pas inclus dans l’installation de Windows Server. Pour installer le logiciel, commencez par le télécharger gratuitement à partir du centre de téléchargement Windows : [Agrégateur de journalisation de l’inventaire logiciel 1,0 pour Windows Server](https://www.microsoft.com/en-us/download/details.aspx?id=49046)

L’infrastructure de la Journalisation de l’inventaire logiciel permet de réduire les coûts de fonctionnement lors de l’inventaire des logiciels Microsoft déployés sur les nombreux serveurs d’un environnement informatique. Cette infrastructure se compose de deux composants : cette agrégation SIL et la fonctionnalité Windows Server, introduite dans Windows Server 2012 R2, Software Inventory Logging (SIL). Software Inventory Logging Aggregator 1.0 s’installe sur un seul serveur et reçoit des données d’inventaire des serveurs Windows Server configurés pour lui transférer des données via SIL. La conception du logiciel permet aux administrateurs de centre de données d’activer SIL dans des images Windows Server maîtres destinées à une large distribution au sein de leur environnement.  Ce package logiciel est le point cible que les clients installent localement pour faciliter la journalisation des données d’inventaire au fil du temps. Ce logiciel permet également de créer régulièrement des rapports d’inventaire simples dans Microsoft Excel. Les rapports Software Inventory Logging Aggregator 1.0 indiquent le nombre d’installations de Windows Server, System Center et SQL Server.

> [!IMPORTANT]
> Aucune donnée n’est envoyée à Microsoft dans le cadre de l’utilisation de ce logiciel.

### <a name="data-sil-collects-over-time"></a>Données collectées par SIL au fil du temps
Une fois le logiciel déployé correctement, les données suivantes peuvent être consultées dans SIL Aggregator :

-   Installations uniques de Windows Server dans votre centre de données

-   Nom de domaine complet (FQDN)

-   GUID d’identification

-   Nombre de processeurs et de cœurs physiques

-   Nombre de processeurs virtuels (dans le cas d’une machine virtuelle)

-   Modèle et type des processeurs physiques

-   Si l’Hyper-Threading est activé ou non sur les processeurs physiques

-   Numéro de série du châssis

-   Borne haute et identité des machines virtuelles Windows Server s’exécutant simultanément (dans le cas d’un hôte exécutant un hyperviseur) sur chaque hôte, au fil du temps

-   Nombre de bornes haute, et nom d’hôte, qui exécutent simultanément \(l’agent System Center\) géré qui présente des machines virtuelles Windows Server sur chaque ordinateur hôte, au fil du temps

-   Nom des agents System Center installés sur les machines virtuelles comptabilisées dans la borne haute\-gérée

-   Nombre et emplacement des installations de SQL Server dans \(le temps uniquement les éditions SKU et les éditions qui nécessitent une licence\)

-   Listes des logiciels installés dans Ajout\//suppression de programmes

### <a name="who-will-use-sil"></a>Qui utilise SIL ?

-   **Les professionnels de l’informatique ou les administrateurs de centre de données**, qui recherchent une méthode économique pour collecter des données d’inventaire logiciel précieuses, automatiquement, au fil du temps.

-   Les **CIO et les contrôleurs financiers**, qui ont besoin de signaler l’utilisation des logiciels d’entreprise Microsoft dans les déploiements informatiques de leur organisation.

## <a name="getting-started"></a>Prise en main
**Conditions préalables**

Software Inventory Logging Aggregator (SIL Aggregator) installé sur au moins un serveur pour l’agrégation et les rapports, dans une machine virtuelle ou sur du matériel physique :

-   **Windows Server 2012 R2** (édition Standard ou Datacenter)

-   **Rôle serveur IIS** avec .NET Framework 4.5, les services WCF et l’activation HTTP dans la même arborescence de sélection dans l’**Assistant Ajout de rôles et de fonctionnalités**.

-   **SQL Server** 2012 SP2 Standard Edition ou SQL Server 2014 Standard Edition

-   **Microsoft Excel 64 bits** 2013 (facultatif pour l’installation, mais nécessaire pour la création de rapports)

-   Facultatif : **VMware PowerCLI 5.5.0.5836** (nécessaire dans les environnements VMware)

>[!Note]
>Lors de l’utilisation de Windows Management Framework, il existe un problème de compatibilité connu avec la version 5,1 de WMF, uniquement sur l’agrégation SIL.  Il n’est pas nécessaire de dépasser WMF version 4,0 sur les serveurs sur lesquels l’agrégateur SIL est installé.

La journalisation de l’inventaire logiciel (SIL) existe dans les versions de Windows Server disposant des mises à jour suivantes :

-   **Windows Server 2016**ou version ultérieure

-   **Windows Server 2012 R2** (édition Standard ou Datacenter)

    -   Mise à jour de Windows Server 2012 R2 **KB3000850** (novembre 2014)

    -   Mise à jour de Windows Server 2012 R2 **KB3060681** (juin 2015) (peut apparaître comme une mise à jour facultative sur Windows Update)

### <a name="security-and-account-types"></a>Sécurité et types de comptes
**Certificat requis**

SIL et SIL Aggregator utilisent des certificats SSL pour la communication authentifiée. Pour cela, il suffit généralement d’installer SIL Aggregator avec un certificat (le nom de serveur et le nom de certificat doivent correspondre) pour héberger le service web qui reçoit les données d’inventaire. Ensuite, les serveurs Windows qui doivent être inventoriés à l’aide de la fonctionnalité SIL utilisent un autre certificat client, pour transmettre des données à SIL Aggregator. Une applet de commande PowerShell (Set-SilAggregator, plus de détails ci-dessous) doit être utilisée pour ajouter des empreintes numériques de certificat à la liste de certificats approuvés de l’agrégateur SIL, à partir de laquelle l’agrégation acceptera les données associées. SIL Aggregator effectue le traitement et l’insertion dans sa base de données après l’authentification de chaque charge utile de données avec un certificat. Consultez la section **Détails des applets de commande SIL Aggregator** pour plus d’informations sur le fonctionnement.

### <a name="polling-account-setup"></a>Configuration du compte d’interrogation
Quand vous ajoutez des informations d’identification à SIL Aggregator pour activer les opérations d’interrogation, vous devez configurer un compte avec le moins de privilèges possibles. En outre, comme meilleure pratique de sécurité, vous ne devez pas utiliser les mêmes informations d’identification pour tous les hôtes d’un centre de données ou d’un autre déploiement informatique.

Sur l’hôte Windows Server que vous voulez configurer pour qu’il soit interrogé par SIL Aggregator et afin d’éviter le recours à un utilisateur du groupe Administrateurs, procédez comme suit pour accorder l’accès minimum à un compte d’utilisateur :

##### <a name="to-setup-a-polling-account"></a>Pour configurer un compte d’interrogation

1.  Sur l’hôte Hyper-V Windows Server que vous voulez interroger avec SIL Aggregator, créez un compte d’utilisateur local à l’aide de **Gestion de l’ordinateur** dans Windows (veillez à décocher la case qui oblige à modifier le mot de passe lors de la première ouverture de session).

2.  Ajoutez cet utilisateur au groupe **Utilisateurs de gestion à distance**.

3.  Ajoutez cet utilisateur au groupe **Administrateurs Hyper-V**.

4.  Ouvrez **WMIMgmt.msc** avec **Démarrer**->**Exécuter**.

5.  Cliquez sur **Autres actions** dans la section **Actions** et sélectionnez **Propriétés**.

6.  Cliquez sur **Sécurité**.

7.  Sélectionnez l’ **espace de noms cimv2** dans l’affichage de l’arborescence **Espace de noms** .

8.  Cliquez sur le bouton **Sécurité**.

9. Ajoutez le groupe **Utilisateurs de gestion à distance** au format **nom_ordinateur\nom_groupe**

10. Cliquez sur **OK**.

11. Dans la fenêtre Sécurité de **root\cimv2**, sélectionnez les **Utilisateur de gestion à distance**.

12. Dans la section autorisations en bas, vérifiez que la case **à cocher activation à distance** est activée.

13. Cliquez sur **Appliquer**, puis sur **OK**.

14. Cliquez sur **OK** dans la fenêtre **Propriétés** .

### <a name="installing-sil-aggregator"></a>Installation de SIL Aggregator
Vous devez vérifier les points suivants avant d’installer SIL Aggregator sur un serveur Windows Server :

-   **Vous disposez d’un certificat SSL valide** que vous souhaitez utiliser pour héberger le service Web de ce logiciel.

    -   Le certificat doit être au format **.pfx**

    -   Le nom du serveur Windows et le nom du certificat doivent correspondre.

-   **SQL Server Standard Edition est installé**ou est installé sur un serveur distant que vous comptez utiliser avec ce logiciel.

    -   SIL Aggregator fonctionne avec SQL Server 2012 SP2 et SQL Server 2014. Aucune configuration particulière de SQL Server n’est nécessaire.

    -   Le compte utilisé pour installer SIL Aggregator doit être un rôle administrateur système sur SQL pour pouvoir créer la base de données pendant l’installation.

    -   Le compte utilisé pour installer SIL Aggregator doit être ajouté en tant qu’administrateur sur SQL Analysis Services avant d’installer SIL Aggregator.

    -   Une fois installé, SQL Server Agent doit être configuré pour s’exécuter automatiquement.

-   **Le rôle serveur IIS est ajouté** avec le .Net Framework 4.5, les Services WCF et l’activation HTTP dans la même arborescence de sélection dans l’**Assistant Ajout de rôles et de fonctionnalités**.

-   Vous êtes **connecté au serveur avec un compte disposant de privilèges d’administrateur** sur le serveur.

-   Vous êtes **connecté au serveur avec un compte disposant des privilèges d’administrateur système sur le serveur SQL Server**, si vous voulez appliquer l’authentification Windows

    Ou

    Si vous voulez utiliser l’authentification SQL, **vous disposez du mot de passe pour un compte disposant des privilèges d’administrateur SQL**.

##### <a name="to-install-software-inventory-logging-aggregator"></a>Pour installer Software Inventory Logging Aggregator

1.  Double-cliquez sur **Setup.exe** pour démarrer l’installation.

2.  Cliquez sur **Suivant** dans la fenêtre d’accueil.

3.  Si vous acceptez le CLUF, cochez la case pour accepter le contrat, puis cliquez sur **Suivant**.

4.  Dans **Choisir des fonctionnalités**, sélectionnez **Installer Sofware Inventory Logging Aggregator et le module Rapports**, puis cliquez sur **Suivant**.

    Pour plus d’informations sur l’installation du module Rapports uniquement, consultez `Publish-SilReport` dans la section **Détails des applets de commande SIL Aggregator** .

5.  Une fois toutes les conditions préalables vérifiées, cliquez sur **Suivant**.

6.  Dans **Choisir un type de compte**, sélectionnez **utilisateur local** ou **gMSA**, selon votre préférence.

    L’option de compte utilisateur local crée un utilisateur local avec un mot de passe fort généré automatiquement. Ce compte est utilisé pour tous les services et opérations de tâches de SIL Aggregator sur le serveur local.  L’utilisation des comptes gMSA (comptes de service administrés de groupe) est recommandée si l’Aggregator fait partie d’un domaine Active Directory (Windows Server 2012 et versions ultérieures). Pour plus d’informations sur gMSA, consultez : [Présentation des comptes de service administrés de groupe](https://technet.microsoft.com/library/hh831782.aspx)

    -   L’option de compte gMSA doit être utilisée si vous prévoyez d’exécuter la base de données SQL Server sur un serveur distinct que celui sur lequel SIL Aggregator est installé.

    -   N’oubliez pas de redémarrer le serveur après avoir ajouté le compte d’ordinateur au groupe de sécurité gMSA activé dans Active Directory.

7.  Dans **Choisir un serveur SQL Server**, entrez le serveur SQL Server sur lequel votre instance SQL est installée, ou **localhost**, si elle est installée sur le serveur local.

    Un seul SIL Aggregator est pris en charge par instance SQL.

8.  Sélectionnez le type d’authentification et cliquez sur **Vérifier SQL**.

9. Cliquez sur **Suivant**, puis sélectionnez un numéro de port dans **Détails du serveur IIS**, ou conservez la valeur par défaut.

10. Accédez à l’emplacement du fichier **.pfx**, tapez son mot de passe, puis cliquez sur **Suivant**.

11. Le dernier écran affiche la progression de l’installation. Une fois terminé, cliquez sur **Terminer**.

### <a name="uninstalling-sil-aggregator"></a>Désinstallation de SIL Aggregator

##### <a name="to-uninstall-software-inventory-logging-aggregator"></a>Pour désinstaller Software Inventory Logging Aggregator

1.  Ouvrez **PowerShell** en tant qu’administrateur, puis tapez `Stop-SilAggregator`. Au retour de l’invite, SIL Aggregator est arrêté.

    Par conception, SIL Aggregator traite les fichiers au bout de 20 minutes ou une fois que 100 fichiers ont été reçus.  Dans les environnements à grande échelle, ce scénario ne se produit jamais, mais à petite échelle, certains fichiers peuvent ne pas avoir encore été traités à l’arrêt de l’Aggregator. Utilisez le paramètre `–Force` si vous n’avez pas besoin de conserver ces fichiers et données.

2.  Accédez au **Panneau de configuration**, cliquez sur **Programmes et fonctionnalités**, sur **Désinstaller des programmes**, sur **Software Inventory Logging Aggregator**, puis sur **Désinstaller**.

    Software Inventory Logging Aggregator ouvre une fenêtre pour vous inviter à choisir entre la suppression ou la conservation de toutes les données de la base de données. La sélection par défaut est de les conserver (si une réinstallation est nécessaire, vous pouvez attacher la base de données pour reprendre là où l’Aggregator s’était arrêté).

3.  Sélectionnez **Conserver** ou **Supprimer**, puis cliquez sur **Suivant**.

4.  Une fois la barre de progression terminée, cliquez sur **Terminer**.

### <a name="start-using-sil-and-the-sil-aggregator"></a>Commencer à utiliser SIL et SIL Aggregator

#### <a name="introduction-to-sil-aggregator-powershell-cmdlets"></a>Présentation des applets de commande PowerShell SIL Aggregator
Les commandes suivantes peuvent être exécutées à partir de la console Windows PowerShell en tant qu’administrateur.

|Applet de commande Windows PowerShell|Fonction|
|-----------------------------|------------|
|`Start-SilAggregator`|Démarre tous les services et tâches de Software Inventory Logging Aggregator. Cette commande permet à l’Aggregator de recevoir des données sur HTTPS à partir de serveurs sur lesquels la journalisation SIL a démarré.|
|`Stop-SilAggregator`|Arrête tous les services et tâches de Software Inventory Logging Aggregator. Si des tâches ou des services se trouvent au milieu d’une opération, cette commande peut s’exécuter après un certain délai.|
|`Set-SilAggregator`|Permet à l’administrateur de modifier la configuration de Software Inventory Logging Aggregator.|
|`Add-SilVmHost`|Utilisé pour ajouter des noms d’hôte spécifiques ou un tableau de noms d’hôte à interroger selon un intervalle régulier \(, la valeur par défaut\)est un intervalle d’une heure.|
|`Remove-SilVmHost`|Utilisée pour supprimer des noms d’hôte spécifiques ou un tableau de noms d’hôte à interroger selon un intervalle régulier.|
|`Get-SilVMHost`|Utilisée pour récupérer la liste des hôtes physiques auxquels Software Inventory Logging Aggregator doit demander les données d’état d’exécution des machines virtuelles.|
|`Get-SILAggregatorData`|Utilisée pour récupérer des données de la base de données dans la console PowerShell.|
|`Publish-SilReport`|Permet de créer des rapports de base de données sur les données de journalisation de l’inventaire logiciel. **Remarque :** Le traitement du cube sur l’Aggregator ne se produit qu’une fois par jour. Par conséquent, les données capturées au niveau de l’Aggregator n’apparaissent dans les rapports que le jour suivant.|

#### <a name="suggested-order-to-start"></a>Ordre de démarrage suggéré
Une fois que vous avez installé Software Inventory Logging Aggregator sur votre serveur, ouvrez PowerShell en tant qu’administrateur.

-   Sur votre SIL Aggregator :

    -   Exécutez `Start-SilAggregator`

        Cette opération est nécessaire pour que votre Aggregator reçoive activement sur HTTPS les données des serveurs que vous avez configurés pour l’inventaire. Notez que même si vous avez activé vos serveurs pour qu’ils commencent à effectuer le transfert vers l’Aggregator, cela ne pose pas de problème, car ils mettent en cache leurs charges utiles de données localement pendant 30 jours. Une fois que l’agrégateur, son « targetUri », est en cours d’exécution, toutes les données mises en cache sont transférées en même temps à l’agrégateur et toutes les données sont traitées.

    -   Exécutez `Add-SilVMHost`

        Exemple : `add-silvmhost –vmhostname contoso1 –hostcredential get-credential`

        -   Dans cet exemple, **contoso1** est le nom de réseau (ou adresse IP) du serveur hôte physique que votre Aggregagor doit interroger pour consulter les mises à jour régulières concernant les machines virtuelles en cours d’exécution, afin d’assurer le suivi de ces données au fil du temps. Get-Credential invite l’utilisateur connecté à entrer le compte à utiliser pour commencer à interroger cet hôte. L’exécution de la même commande sur le même hôte vous permet de mettre à jour le compte utilisé à tout moment. Soyez attentif aux modifications de mot de passe de compte et aux délais d’expiration dans le temps. Si les informations d’identification sont modifiées ou expirent, l’interrogation sur l’hôte échoue.

        -   Par défaut, l’interrogation s’effectue toutes les heures et commence une heure après l’exécution de `Start-SilAggregator` ou une heure après qu’un hôte a été ajouté à la liste d’interrogation.  L’intervalle d’interrogation peut être modifié à l’aide de `Set-SilAggregator cmdlet`.

        -   Cette applet de commande détecte automatiquement dans une liste prédéfinie d’options (consultez la section **Détails des applets de commande SIL Aggregator**), les valeurs HostType et HyperVisorType appropriées pour l’hôte que vous ajoutez. Si elle ne parvient pas à reconnaître ces options ou si les informations d’identification indiquées sont incorrectes, une invite s’affiche. Si vous acceptez avec une entrée **Y**, l’hôte est ajouté, répertorié comme **Inconnu**, mais il n’est pas interrogé.

    -   Exécutez `Set-SilAggregator –AddCertificateThumbprint` « l’empreinte numérique de votre certificat client »

        Cette commande permet de recevoir des données sur HTTPS des serveurs Windows sur lesquels la journalisation SIL est activée. L’empreinte numérique est ajoutée à la liste des empreintes numériques à partir desquelles SIL Aggregator accepte les données. SIL Aggregator est conçu pour accepter les certificats valides d’authentification client d’entreprise. Le certificat utilisé doit être installé dans le  **\\magasin localmachine\MY (ordinateur local-> personnel**) sur le serveur qui transfère les données.

-   Sur vos serveurs Windows à inventorier, ouvrez PowerShell en tant qu’administrateur et exécutez les commandes suivantes :

    -   Exécutez `Set-SilLogging –TargetUri "https://contososilaggregator" –CertificateThumbprint "your client certificate's thumbprint"`

        -   Cette commande indique à SIL dans Windows Server l’emplacement où envoyer les données d’inventaire et le certificat à utiliser pour l’authentification.

            > [!IMPORTANT]
            > Vérifiez que « https:// » est dans la valeur TargetUri.

        -   Vous devez installer le certificat client d’entreprise avec cette empreinte numérique dans **\localmachine\MY** ou utiliser **certmgr.msc** pour installer le certificat dans le magasin **Ordinateur Local -> Personnel**.

            > [!IMPORTANT]
            > Si ces valeurs ne sont pas correctes ou si le certificat n’est pas installé dans le magasin approprié (ou n’est pas valide), les transferts vers la cible échouent quand la journalisation SIL est lancée. Les données sont mises en cache localement pendant 30 jours.

    -   Exécutez `Start-SilLogging`

        Cette commande démarre la journalisation SIL. Toutes les heures, à des intervalles aléatoires, SIL transfère ses données d’inventaire à l’Aggregator spécifié avec le paramètre `–targeturi` . Le premier transfert est un jeu de données complet. Chaque prochain transfert sera plus une « pulsation » avec simplement l’identification des données qui n’ont pas changé. Si le jeu de données a été modifié, un autre jeu de données complet est transféré.

    -   Exécutez `Publish-SilData`

        -   La première fois que SIL est activé pour la journalisation, cette étape est facultative.

        -   Il s’agit d’un transfert manuel unique d’un jeu complet de données.

        -   Si la journalisation SIL a été démarrée depuis un certain temps et qu’un nouveau SIL Aggregator est désigné par `Set-SilLogging`, vous devez exécuter cette applet de commande, une seule fois, pour envoyer un jeu complet de données au nouvel Aggregator.

Une fois que vous avez suivi ces étapes pour ajouter des hôtes physiques exécutant des machines virtuelles Windows Server, ET que vous avez activé la journalisation de l’inventaire logiciel (ou journalisation SIL) au sein de ces serveurs Windows, vous pouvez exécuter `Publish-SilReport –OpenReport` à tout moment sur SIL Aggregator (nécessite Excel 2013). Notez toutefois que le cube SQL Server Analysis Services cube effectue le traitement une fois par jour, les données ne sont donc pas disponibles dans les rapports le jour-même.

## <a name="architectural-overview"></a>Vue d’ensemble de l’architecture
SIL fonctionne en mode push et pull et se compose de deux composants qui fonctionnent en parallèle : La fonctionnalité de journalisation de l’inventaire logiciel (SIL) dans Windows Server et l’outil d’agrégation de journalisation de l’inventaire logiciel (SILA), MSI, téléchargeable. Les serveurs à inventorier envoient des données d’inventaire logiciel sur HTTPS, à l’aide de la journalisation SIL, à SIL Aggregator (toutes les heures à des points aléatoires au sein de chaque heure). L’Aggregator, à son tour, interroge les hôtes hyperviseur physiques pour extraire des données d’inventaire matériel toutes les heures. Les modes par envoi et par extraction doivent être configurés correctement pour activer toutes les fonctionnalités de SIL. Ils peuvent être configurés dans n’importe quel ordre. Toutefois, le traitement du cube sur l’Aggregator se produit une seule fois par jour, les données capturées sur l’Aggregator, par envoi ou par extraction, n’apparaissent donc pas dans les rapports avant le jour suivant.

![](../media/software-inventory-logging/SILA_Architecture.png)

> [!IMPORTANT]
> Aucune donnée n’est envoyée à Microsoft dans le cadre de l’utilisation de ce logiciel.

## <a name="enable-sil-on-multiple-servers"></a>Activer SIL sur plusieurs serveurs
Il existe plusieurs façons d’activer SIL dans une infrastructure de serveurs distribués, comme un cloud privé de machines virtuelles.  Voici un exemple de la manière de configurer des images Windows Server pour qu’elles envoient automatiquement des données d’inventaire à SIL Aggregator quand elles démarrent sur le réseau pour la première fois.

Exécutez les applets de commande suivantes dans la console PowerShell en tant qu’administrateur sur chaque machine virtuelle ou appareil/ordinateur physique où Windows Server est installé (voir la section **Conditions préalables**) :

Vous devez disposer d’un certificat SSL client valide au format .pfx pour effectuer ces étapes.  Vous devez ajouter l’empreinte numérique de ce certificat à SIL Aggregator à l’aide de l’applet de commande `Set-SILAggregator –AddCertificateThumbprint`. Ce certificat client n’a pas besoin de correspondre au nom du SIL Aggregator.

-   `$secpasswd = ConvertTo-SecureString "`**<password for the account with permissions to the network location holding your client pfx file>**`" -AsPlainText –Force`

-   `$mycreds = New-Object System.Management.Automation.PSCredential ("`**<user account with permissions to the network location holding your client  pfx file>**`", $secpasswd)`

-   `$driveLetters = ([int][char]'C')..([int][char]'Z') | % {[char]$_}`

-   `$occupiedDriveLetters = Get-Volume | % DriveLetter`

-   `$availableDriveLetters = $driveLetters | ? {$occupiedDriveLetters -notcontains $_}`

-   `$firstAvailableDriveLetter = $availableDriveLetters[0]`

-   `New-PSDrive -Name $firstAvailableDriveLetter -PSProvider filesystem -root`**server\path à partager contenant votre fichier de certificat pfx > < \\** `-credential $mycreds`

-   `Copy-Item ${firstAvailableDriveLetter}:\` **< fichier CertificateName. pfx dans le répertoire du nouveau lecteur > c\<: emplacement de votre choix >**

-   `Remove-PSDrive –Name $firstAvailableDriveLetter`

-   `$mypwd = ConvertTo-SecureString -String "`**<password for the certificate pfx file>**`" -Force –AsPlainText`

-   `Import-PfxCertificate -FilePath c:\`**emplacement\\< CertificateName. pfx >** `cert:\localMachine\my -Password $mypwd`

-   `Set-sillogging –targeturi "https://` **<machinename of your SIL Aggregator>** `–certificatethumbprint`

> [!NOTE] 
> Utilisez l’empreinte numérique du certificat de votre fichier PFX client et ajoutez à votre agrégateur SIL à l’aide de l’applet de commande **Set-SilAggregator'-AddCertificateThumbprint** .

-   `Start-sillogging`

Chaque fois que SIL Aggregator n’est pas accessible, les données d’inventaire SIL sont mises en cache localement sur les serveurs Windows pendant 30 jours. Une fois l’envoi correctement effectué à l’Aggregator, toutes les données mises en cache sont transférées.

Ajoutez `Publish-SilData` à la liste ci-dessus si vous envoyez des données SIL à un nouveau SIL Aggregator après avoir réussi plusieurs envois à un ancien Aggregator (cette commande envoie un complément de données SIL dont le nouvel Aggregator a besoin pour cet ordinateur).

## <a name="software-inventory-logging-aggregator-reports"></a>Rapports Software Inventory Logging Aggregator
![](../media/software-inventory-logging/SILA_Report.png)

### <a name="cube-processing"></a>Traitement du cube
Dans Software Inventory Logging Aggregator, le cube SQL Server Analysis Services est traité une fois par jour à 3:00:00 AM, heure du système local. Les rapports reflètent toutes les données jusqu’à cette heure, mais aucune après cette heure le même jour.

### <a name="high-water-mark"></a>Borne haute
Un aspect fondamental des rapports de l’agrégateur de journalisation de l’inventaire logiciel est la capture de ce qui est communément appelé une « borne haute » de serveurs Windows exécutant simultanément. Cela s’applique au nombre d’installations de Windows Server et System Center dans ces rapports. Pour Windows Server, chaque hôte physique a un point dans le temps (indépendamment du type du système d’exploitation sur l’hôte), au cours d’un mois, où la plupart des machines virtuelles Windows Server s’exécutent simultanément. Il s’agit de la borne haute du mois. En outre, pour System Center, il existe un point dans le temps dans le mois où la plupart des serveurs Windows gérés s’exécutent simultanément pour chaque hôte physique (un serveur géré est identifié quand un ou plusieurs agents System Center sont présents). Seule la borne haute la plus récente pour un hôte physique est affichée dans le rapport. Aucune donnée après la borne haute n’est affichée. Et on peut supposer que le nombre machines virtuelles Windows Server (onglets WS) ou de machines virtuelles Windows Server gérées (onglets SC), se trouve en dessous de la borne haute au-delà de ce point. Ce mode de suivi et de représentation de l’utilisation doit permettre de planifier la capacité et de s’aligner avec les modèles de licence de ces produits.

Dans les onglets relatifs à SQL dans le rapport, les installations de SQL Server sont comptabilisées de façon cumulative. pas par HIG-Water. Les totaux représentent le nombre d’installations de SQL Server en cours d’exécution.

> [!NOTE]
> L’utilisation de la journalisation de l’inventaire logiciel ne remplace pas l’obligation de signaler précisément l’utilisation des logiciels Microsoft dans le cadre des termes du contrat de licence applicable.

### <a name="poll-date-time"></a>Date et heure d’interrogation
Quand vous utilisez Software Inventory Logging Aggregator, l’agrégation des valeurs de borne haute est pilotée par l’interrogation. En d’autres termes, une borne haute ne peut être capturée que par l’interrogation de l’hôte physique sous-jacent. Les nombres de bornes haute sont donc directement associés à une « date et heure de l’interrogation » correspondante. Bien que l’intervalle d’interrogation soit réglable, la fidélité des bornes hautes capturées est affectée si une valeur d’intervalle supérieure est utilisée. Plus l’intervalle est grand, moins les données sont représentatives de l’utilisation réelle.

### <a name="reports-are-month-by-month"></a>Les rapports sont effectués mois par mois
Tous les rapports, y compris les rapports annuels, sont représentés sous forme de rapports mois par mois. Les bornes hautes, les totaux et les données de l’ordinateur sont réinitialisées au début de chaque mois calendaire.

Les données de rapport suivantes, entre autres, sont affectées par le passage à un nouveau mois :

-   Toutes les bornes hautes de tous les hôtes sont réinitialisées au début d’un nouveau mois.

-   Si l’Aggregator reçoit au moins une charge utile complète d’une machine virtuelle (sur HTTPS), mais ne reçoit plus de pulsations, toutes les interrogations de l’hôte sous-jacent dans le mois interprètent l’association entre l’hôte, la machine virtuelle et les données de machine virtuelle comme si la machine virtuelle était en cours d’exécution ou arrêtée pendant tout le mois. Au début du nouveau mois, cette association est effacée jusqu’à ce qu’une charge utile complète ou une pulsation soit reçue de la part de cette machine virtuelle.

### <a name="additional-notes-on-report-behavior"></a>Remarques supplémentaires sur le comportement du rapport

-   Les onglets Résumé sont des listes de référence rapide de l’inventaire. Les hôtes et leurs machines virtuelles sont répertoriés dans la même colonne.

-   Ignorez toutes les valeurs qui sont grisées ou faibles. Il s’agit d’artefacts de la création de rapports à partir du cube SSAS.

-   Si une machine virtuelle est listée avec « système d’exploitation inconnu », cela signifie que l’agrégation n’a pas reçu une charge utile de données complète de cette machine virtuelle via SIL sur HTTPs.

-   Les machines virtuelles répertoriées sous « hôte inconnu » sont des machines virtuelles Windows Server qui transfèrent correctement les données d’inventaire sur HTTPs à l’agrégateur, mais l’agrégation n’intervient pas activement ou correctement sur l’hôte sous-jacent pour cette machine virtuelle. Le décompte sera toujours de zéro pour ces entrées, car l’hôte sous-jacent est inconnu. Utilisez l’applet de commande `Add-SilVMHost`, avec des informations d’identification correctes, pour ajouter l’hôte (ou tous les hôtes) à SIL Aggregator pour l’interrogation. Une fois l’interrogation correctement activée, les données de la machine virtuelle et de l’hôte sont associées sur les rapports.

-   Toutes les dates et heures sont locales par rapport à l’heure et aux paramètres régionaux système de SIL Aggregator. Cela inclut les données d’inventaire reçues des systèmes SIL sur HTTPS. Quand ces fichiers sont traités (pas plus de 20 minutes après la réception) les données sont insérées dans la base de données avec l’heure système locale.

-   « SIL Aggregater » est indiqué sur tout serveur sur lequel l’agrégateur de journalisation de l’inventaire logiciel est installé.

-   Si la quantité de mémoire physique ou le nombre de processeurs est modifié sur un hôte physique, une nouvelle ligne s’affiche dans le rapport en plus de l’ancienne ligne. L’interrogation des mises à jour cesse sur l’ancienne ligne et continue sur la nouvelle comme s’il s’agissait d’un hôte récemment ajouté.

-   Sous les onglets **Résumé** et **Détails** , le total des colonnes Serveurs Windows exécutés simultanément ou serveurs Windows gérés indique le total de toutes les bornes hautes pour tous les hôtes répertoriés. Il s’agit notamment des serveurs Windows qui ne sont pas des hôtes hyperviseur et qui n’ont pas d’ordinateurs virtuels en cours d’exécution, ainsi que de serveurs qui peuvent avoir des machines virtuelles en cours d’exécution, mais qui sont « inconnus », car aucune donnée n’est reçue depuis la machine virtuelle à partir de SIL via HTTPs. Ceux-ci sont totalisés pour des raisons pratiques.

-   Dans la section **SQL Server** de l’onglet **Tableau de bord**, le nombre total d’installations de SQL Server est un résumé de tous les totaux de l’édition dans le tableau de bord.  Il peut y avoir une incohérence entre le total de l’onglet **Détails SQL** dans le cas où plusieurs éditions de SQL sont installées sur un seul serveur.  Le tableau de bord les compte séparément sur chaque serveur, contrairement à l’onglet **Détails**.  Plusieurs éditions SQL installées sur un serveur Windows Server sont toujours comptabilisées comme une seule édition, par contrat de licence.

-   Dans la section **Windows Server** de l’onglet **Tableau de bord**, les lignes **Autres hôtes hyperviseur** et **Nombre total d’hôtes hyperviseur** incluent les hôtes Windows Server physiques qui peuvent ou NON exécuter Hyper-V.

### <a name="column-descriptions"></a>Description des colonnes
Voici la description de chaque colonne de l’onglet **Détails de Windows Server** dans le rapport Excel créé par SIL Aggregator. Les autres onglets de données sont identiques ou représentent un sous-ensemble de ces colonnes. La seule exception est le « nombre d’installations » dans les onglets SQL Server (voir la section **borne haute** ).

|En-tête de colonne|Description|
|-----------------|---------------|
|Mois calendaire|Les données des rapports sont regroupées par mois, en commençant par le plus récent. Les données du mois ne sont pas affichées dans un ordre spécifique.|
|Nom d'hôte|Nom de réseau ou nom de domaine complet de l’hôte physique que SIL Aggregator parvient à interroger.<br /><br />Utilisez l’applet de commande Get-SilVMHost pour rechercher les hôtes qui ont été ajoutés, mais ne sont pas ou plus correctement interrogés. La dernière interrogation réussie est affichée.|
|Type d’hôte|Fabricant du système d’exploitation de l’hôte physique.|
|Type d’hyperviseur|Fabricant de l’hyperviseur de l’hôte physique.|
|Fabricant du processeur|Fabricant des processeurs de l’hôte physique.|
|Modèle de processeur|Modèle des processeurs de l’hôte physique.|
|L’Hyper-Threading est-il activé ?|Affiche True ou False selon que l’Hyper-Threading est activé ou non sur les processeurs de l’hôte physique.|
|Nom de l'ordinateur virtuel|Nom de réseau ou nom de domaine complet de la machine virtuelle Windows Server. Si l’Aggregator n’a pas reçu de données de cet ordinateur sur HTTPS, le nom convivial de la machine virtuelle dans l’hyperviseur est indiqué.|
|Machines virtuelles Windows Server s’exécutant simultanément, par hôte|Nombre de machines virtuelles Windows Server s’exécutant simultanément sur l’hôte. Le nombre le plus élevé du mois pour cet hôte est la borne haute indiquée et capturée à cet instant précis.<br /><br />Consultez la section **Borne haute** de cette documentation.<br /><br />Les hôtes physiques sur lesquels Windows Server est installé, ou sur lesquels Windows Server est installé et aucune machine virtuelle Windows Server connue ne s’exécute, sont comptabilisés comme un seul hôte. Si au moins une machine virtuelle Windows Server connue est en cours d’exécution sur l’hôte et que Windows Server est en cours d’exécution sur l’hôte lui-même, le système d’exploitation de l’hôte n’est pas comptabilisé.|
|Nombre de processeurs physiques|Nombre de processeurs physiques installés sur l’hôte physique.|
|Nombre de cœurs physiques|Nombre de cœurs de processeur physiques installés sur l’hôte physique.|
|Nombre de processeurs virtuels|Nombre de processeurs virtuels que Windows reconnaît dans la machine virtuelle. Cette valeur est fournie uniquement à partir des données transférées sur HTTPS à l’aide de SIL dans un serveur Windows Server.|
|Date et heure d’interrogation|Date et heure du dernier point de borne haute des machines virtuelles Windows Server s’exécutant simultanément sur cet hôte physique.<br /><br />Consultez la section **Date et heure d’interrogation** de cette documentation.|
|Date et heure du dernier affichage de la machine virtuelle|Date et heure auxquelles l’Aggregator a reçu le dernier inventaire des données sur HTTPS de la part de cette machine virtuelle Windows Server.|
|Date et heure du dernier affichage de l’hôte|Date et heure auxquelles l’Aggregator a reçu le dernier inventaire des données sur HTTPS de la part de cet hôte physique Windows Server.<br /><br />L’activation de SIL et le transfert des données d’inventaire sur HTTPS à un SIL Aggregator sont pris en charge sur des hôtes physiques exécutant Windows Server et Hyper-V.|

## <a name="sil-aggregator-cmdlets-detail"></a>Détails des applets de commande SIL Aggregator
Voici les détails des applets de commande SIL Aggregator. Pour obtenir la documentation complète de l’applet de commande, consultez : [Applets de commande PowerShell de l’agrégateur SIL](https://technet.microsoft.com/library/mt548455.aspx)

### <a name="publish-silreport"></a>Publish-SilReport

-   Cette applet de commande, utilisée en l’État, crée un rapport de journalisation de l’inventaire logiciel et le place dans le répertoire documents de l’utilisateur connecté (Excel 2013 est requis sur l’ordinateur sur lequel l’applet de commande est exécutée).

-   Utilisée avec le paramètre `–OpenReport` , elle crée le rapport et l’ouvre dans Excel pour consultation.

-   Notez que lors de l’installation de SIL Aggregator, vous avez la possibilité d’installer uniquement le module Rapports. Vous pouvez installer le module Rapports sur un système d’exploitation client Windows, tel que Windows 8.1 ou Windows 10. Cela permet à un client léger, comme un PC portable ou une tablette, de se connecter à un serveur de base de données SIL Aggregator pour publier directement des rapports SIL.

    -   L’exemple suivant vous invite à indiquer les informations d’identification à utiliser, se connecte à un serveur de base de données SIL Aggregator nommé SILContoso, et crée et ouvre un rapport SIL sur l’ordinateur local.

        `Publish-SilReport -DBServerName "SILContoso" -DBServerCredential Get-Credential –OpenReport`

    -   Avant de vous connecter pour la première fois, dans la plupart des cas vous devez ouvrir un port dans le pare-feu sur le serveur de base de données SIL Aggregator pour autoriser les connexions. Les professionnels de l’informatique peuvent configurer ce port à l’avance pour permettre à leurs contrôleurs financiers ou autres gestionnaires d’inventaire de créer leurs propres rapports. Pour savoir comment procéder, consultez le lien ci-dessous. Le port par défaut généralement utilisé pour SQL Server Analysis Services est 2383.

### <a name="add-silvmhost"></a>Add-SilVMHost
Les types d’hôtes et versions d’hyperviseur suivants sont pris en charge quand vous utilisez l’applet de commande `Add-SilVMHost` . Notez qu’il n’est pas nécessaire de les spécifier. L’applet de commande `Add-SilVMHost` détecte automatiquement une combinaison prise en charge. Dans le cas contraire ou si les informations d’identification indiquées sont incorrectes, une invite s’affiche. Si l’utilisateur accepte avec une entrée « Y », l’hôte est ajouté, mais il n’est pas interrogé. Il sera ajouté comme « inconnu ».

|Version d’hyperviseur|SIL Aggregator         Valeur HostType|SIL Aggregator Valeur HypervisorType|
|----------------------|-----------------------------------------|---------------------------------------|
|Windows Server, 2012 R2|Windows|Hyper-V|
|VMware 5.5|VMware|Esxi|
|Xen 4.x|Ubuntu, OpenSuse ou CentOS|Xen|
|XenServer 6.2|Citrix|XenServer|
|KVM|Ubuntu, OpenSuse ou CentOS|KVM|

D’autres versions de ces types et plateformes d’hyperviseur peuvent également fonctionner.  SIL Aggregator inclut la version sshnet ci-dessous.  Elle est utilisée pour communiquer avec les plateformes de virtualisation Linux.

<pre>sshnet 2014.4.6-beta1
https://sshnet.codeplex.com/releases/view/120504
Copyright (c) 2010, RENCI</pre>

### <a name="get-silaggregator"></a>Get-SilAggregator
`Get-SilAggregator` fournit des informations de configuration pour votre application Software Inventory Logging Aggregator. L’exemple de sortie suivant affiche les informations suivantes :

-   L’application est en cours d’exécution

-   L’intervalle d’interrogation est toutes les heures (peut être modifié en incréments d’heure)

-   Heure de début de l’interrogation

-   URI cible que les autres ordinateurs doivent définir pour transférer des données à cet Aggregator

-   Empreintes numériques de certificat dont cet Aggregator accepte des données

-   Type de compte spécifié à l’installation

    `PS C:\Windows\system32> Get-SilAggregator`

    ``

    `State          : Running HostPollIntervalInHours : Every 1 Hour(s)`

    `PollStartTime      : 8/24/2015 5:07:33 AM`

    `TargetURI        : https://SilContoso`

    `CertificateThumbprint  : 3efc6b8ce7d5eefba5107ede9d1caca550417452, 2dc4ea8bfb64b1246a8c1ffa1b701cd1042a3412`

    `UserProfile       : Local`

### <a name="set-silaggregator"></a>Set-SilAggregator
Avec l’applet de commande `Set-SilAggregator` , vous pouvez :

-   Modifier l’intervalle horaire de l’interrogation.

-   Modifier la date et l’heure de début de l’interrogation pour démarrer à l’intervalle spécifié.

-   Ajouter ou supprimer des empreintes numériques de certificat dont SIL Aggregator accepte des données ou qui sont associées à ce dernier.

### <a name="get-aggregatordata"></a>Get-AggregatorData

-   Utilisée seule, cette applet de commande affiche le contenu de l’onglet Détail des serveurs Windows d’un rapport Excel SIL Aggregator.

-   Utilisée avec des paramètres, cette applet de commande récupère les données directement de la base de données destinée à permettre des utilisations personnalisées de la solution SIL globale.

-   Notez que les paramètres `–StartTime` et `–Endtime` affichent des données de rapport à partir du premier jour du mois de la date de début et du dernier jour du mois de la date de fin.

![](../media/software-inventory-logging/SILA_Get-SILAggregator.png)

### <a name="get-silvmhost"></a>Get-SilVMHost

-   Cette applet de commande renvoie la liste des hôtes physiques que SIL Aggregator doit interroger, la date et l’heure de la dernière interrogation réussie et les valeurs HostType (fabricant du système d’exploitation) et HypervisorType (fabricant de l’hyperviseur). Consultez les détails d’Add-SilVMHost pour plus d’informations sur HostType et HypervisorType.

    Si un hôte n’a aucune date et heure d’interrogation, mais qu’il a des valeurs HostType et HypervisorType prises en charge, cela signifie que l’interrogation n’a pas encore commencé ou n’a pas encore été réussie.

-   Cette applet de commande affiche également la liste des noms d’hôte qui ont été ajoutés par l’intermédiaire des données provenant des machines virtuelles elles-mêmes, si elles sont disponibles dans la machine virtuelle. Celles-ci s’affichent dans la liste, mais n’ont pas de valeur HostType ou HypervisorType. Ces données peuvent permettre de mettre en correspondance les machines virtuelles et les hôtes qui ne sont peut-être pas configurés pour l’interrogation.

-   Utilisez les paramètres `–StartTime` et`–EndTime` pour mieux comprendre à quel moment les hôtes ont été ajoutés pour la première fois ou interrogés pour la dernière fois.

### <a name="remove-silvmhost"></a>Remove-SilVMHost

-   Cette applet de commande supprime tout hôte de la liste des hôtes à interroger. Si un hôte est supprimé, il est possible qu’une machine virtuelle sur l’hôte rajoute ce dernier à la liste, mais l’hôte n’est pas interrogé avec les informations d’identification correctes spécifiées à l’aide de l’applet de commande `Add-SilVMHost` .

-   Si un hôte est supprimé, il n’est pas interrogé, mais n’est pas supprimé des rapports. En revanche, comme il n’est pas interrogé, il n’apparaîtra pas dans les rapports du mois suivant.

-   Utilisez les paramètres `–StartTime` et`–EndTime` individuellement pour supprimer des groupes d’hôtes correctement interrogés jusqu’à une date, ou correctement interrogé à partir d’une date.

## <a name="avoid-these-errors-and-issues-with-sil-and-sil-aggregator-troubleshooting-guide"></a>Éviter les erreurs et problèmes suivants avec SIL et SIL Aggregator (Guide de résolution des problèmes)

-   Éléments à vérifier en cas d’échec de l’applet de commande `SilLogging` ou `Publish-Sildata` , ou en cas d’erreur :

    -   Vérifiez que **targeturi** contient **https://** dans l’entrée.

    -   Vérifiez que toutes les mises à jour nécessaires pour Windows Server sont installées (consultez la section Conditions préalables pour SIL).  Pour vérifier rapidement, il est recommandé de rechercher ces éléments à l’aide de l’applet de commande suivante :`Get-SilWindowsUpdate *3060*, *3000*`

    -   Vérifiez que le certificat utilisé pour l’authentification auprès de l’Aggregator est installé dans le magasin approprié sur le serveur local à inventorier avec SilLogging (consultez la section Prise en main).

    -   Dans SIL Aggregator, vérifiez que l’empreinte numérique du certificat utilisé pour l’authentification auprès de l’Aggregator est ajoutée à la liste à l’aide de l’applet de commande `Set-SilAggregator –AddCertificateThumbprint` (consultez la section Prise en main).

    -   Si vous utilisez des certificats d’entreprise, vérifiez que le serveur sur lequel SIL est activé est joint au domaine pour lequel le certificat a été créé, ou qu’il peut être vérifié avec une autorité racine. Si un certificat n’est pas approuvé sur l’ordinateur local qui tente de transférer/envoyer des données à un Aggregator, cette action échoue avec une erreur.

    -   Si tous les éléments ci-dessus ont été vérifiés, vous pouvez vérifier que le certificat utilisé pour installer SIL Aggregator est intègre et qu’il correspond au nom du serveur SIL Aggregator lui-même (cette étape est inutile si d’autres ordinateurs parviennent à transférer correctement des données au même SIL Aggregator).

    -   Vous pouvez vérifier l’emplacement suivant pour les fichiers sil mis en cache sur le serveur qui tente de transférer/\\envoyer,\\\Windows\System32 LogFiles sil. Si `SilLogging` a démarré et s’exécute depuis plus d’une heure, ou que `Publish-SilData` a été exécuté récemment et qu’il n’existe aucun fichier dans ce répertoire, la journalisation dans l’Aggregator a réussi.

-   Vérifiez que l’utilisateur connecté dispose d’un accès à la base de données SQL et à Analysis Services.

    -   Cela est nécessaire pour l’installation de SIL Aggregator.

    -   Cela est nécessaire lors de l’utilisation de PowerShell à distance pour gérer l’agrégateur SIL.

-   Pour publier des rapports SIL Aggregator à partir d’un système d’exploitation de bureau client.

    -   Utilisez l’option permettant d’installer uniquement le module Rapports sur votre client Windows (8.1/10).

    -   Si vous rencontrez des problèmes quand vous tentez de créer un rapport à l’aide de PowerShell à distance, vous devez probablement ouvrir un port de pare-feu sur le SIL Aggregator auquel vous essayez de vous connecter (consultez l’applet de commande `Publish-SilReport` dans la section Détails des applets de commande SIL Aggregator).

-   Quand vous utilisez l’option gMSA :

    -   N’oubliez pas de redémarrer le serveur après l’avoir joint au groupe de machines gMSA activé dans Active Directory.

    -   Dans le processus d’installation, n’utilisez pas de domaine complet lors de l’entrée de domaine\utilisateur. Par exemple, utilisez **mydomain\gmsaaccount**. N’entrez pas **MyDomain<i> </i> . com\gmsaaccount**.

-   Lorsque vous utilisez Windows Management Framework dans votre environnement :

    -   Vérifiez que WMF 5,1 n’est pas installé sur le ou les serveurs avec SILA installé.  Il est possible d’atteindre une erreur dans le journal des événements concernant la DLL **'mpunits. dll'** .  Cela empêchera le bon fonctionnement.  SILA nécessite uniquement WMF 4,0.

## <a name="managing-sil-over-time"></a>Gestion de SIL dans le temps

### <a name="uninstallreinstall-sil-aggregator"></a>Désinstaller/réinstaller SIL Aggregator
Si vous devez désinstaller, puis réinstaller SIL Aggregator, vous pouvez le faire sans perdre les données d’inventaire existantes et historiques. Désinstallez simplement le logiciel (en suivant les étapes décrites dans cette documentation) et sélectionnez l’option permettant de conserver la base de données de journalisation de l’inventaire logiciel. Ensuite, réinstallez SIL Aggregator (en suivant les étapes décrites dans cette documentation), puis sélectionnez l’option permettant d’attacher une base de données existante.

Après avoir effectué cette opération, vous devez mettre à jour les informations d’identification à l’aide de l’applet de commande `Add-SilVMHost` sur tous les hôtes qui ont été précédemment interrogés par SIL Aggregator (en supposant que vous vouliez continuer à collecter des données sur ces hôtes). En outre, pour éviter les doublons d’entrées pour le même hôte dans les rapports, vous devez rajouter des hôtes pour l’interrogation à l’aide de la même adresse réseau que celle ajoutée à l’origine. Voici les trois types de vmhostname pris en charge qui peuvent être utilisés pour ajouter un hôte à l’aide de l’applet de commande `Add-SilVMHost` :

-   Adresse IP

-   Nom de domaine complet

-   Nom NetBIOS

### <a name="changing-sil-aggregators"></a>Modification de SIL Aggregators
Quand vous voulez démarrer l’inventaire des serveurs de votre environnement avec un autre SIL Aggregator, utilisez simplement l’applet de commande SIL sur ces serveurs pour modifier le targeturi (et l’empreinte numérique du certificat, si nécessaire), `Set-SilLogging –TargetUri`. Notez que vous devez ensuite utiliser l’applet de commande `Publish-SilData` au moins une fois pour transférer un inventaire complet au SIL Aggregator nouvellement spécifié.

### <a name="changing-or-updating-certificates"></a>Modification ou mise à jour des certificats
**ÉTAPES IMPORTANTES POUR ÉVITER LA PERTE DE DONNÉES :** S’il est nécessaire de modifier le certificat que les serveurs utilisent pour transférer des données vers une agrégation SIL, mais que l’agrégateur cible restera le même, utilisez ces étapes pour éviter une perte de données potentielle en transit vers l’agrégateur :

-   Sur le SIL Aggregator, utilisez l’applet de commande `Set-SilAggregator –AddCertificateThumbprint` pour ajouter la nouvelle empreinte numérique du SIL Aggregator.

-   Sur TOUS les serveurs qui transfèrent des données, installez le nouveau certificat à utiliser dans **\LOCALMACHINE\MY** à l’aide de la méthode de votre choix.

-   Sur TOUS les serveurs qui transfèrent des données, utilisez l’applet de commande `Set-SilLogging –CertificateThumbprint` pour mettre à jour l’empreinte numérique du nouveau certificat.

-   **CRITIQUE Une fois que tous les serveurs qui ont transféré des données ont été mis à** jour, supprimez l’ancienne `Set-SilAggregator –RemoveCertificateThumbprint` empreinte numérique de l’agrégateur sil à l’aide de l’applet de commande. Si un serveur qui transfère des données continue à le faire à l’aide d’un ancien certificat qui a été supprimé du SIL Aggregator, **des données seront perdues** et ne seront pas insérées dans la base de données de l’Aggregator. Cela n’affecte que les scénarios dans lesquels un serveur a précédemment transféré des données à une agrégation SIL et que le certificat est ensuite supprimé de la liste d’empreintes numériques de l’agrégateur SIL à partir de laquelle les données doivent être acceptées.

## <a name="release-notes"></a>Notes de publication

-   Le fait que SIL Aggregator ne gère ni ne signale la présence de l’installation de SQL Server Standard Edition est un problème connu.  Pour le résoudre, effectuez les étapes suivantes :

    1.  Ouvrez SQL Server Management Studio sur SIL Aggregator.

    2.  Connectez-vous au moteur de base de données.

    3.  Développez la base de données SoftwareInventoryLogging puis Tables dans l'arborescence de sélection.

    4.  Cliquez avec le bouton droit sur **dbo. SqlServerEdition**, puis sélectionnez «**modifier les 200 premières lignes**».

    5.  Remplacez le PropertyNumValue en regard de « Standard Edition » par **2760240536** (de-1534726760).

    6.  Fermez la requête pour enregistrer la modification.

    7.  Il sera peut-être nécessaire d'exécuter une fois l’applet de commande `Publish-SilData` sur tous les serveurs qui exécutent SIL et qui ont déjà journalisé des données sur cet Aggregator pour que celui-ci gère correctement la présence de SQL Server Standard Edition.

-   Dans les rapports SIL générés, le total des cœurs de processeur inclut le nombre de threads si l’Hyper-Threading est activé sur le serveur physique.  Pour obtenir le nombre réel de cœurs physiques sur les serveurs sur lesquels l’Hyper-Threading est activé, vous devez réduire ce nombre de moitié.

-   Les totaux dans les lignes (sous l’onglet **tableau de bord** ) et les colonnes (dans les onglets **Résumé et détails** ) étiquetés «**exécution simultanée**... », pour Windows Server et System Center, ne correspondent pas exactement entre les deux emplacements. Sous l’onglet **tableau de bord** , vous devez ajouter la valeur «**appareils Windows Server (** sans machine virtuelle connue) » à la valeur «**simultanément en cours d’exécution**... » valeur égale à ce nombre sur les onglets **Résumé et détails** .

-   Consultez **ÉTAPES IMPORTANTES POUR ÉVITER LA PERTE DE DONNÉES** lors de la modification ou mise à jour des certificats, dans la section **Gestion de SIL au fil du temps** de cette documentation.

-   S’il est possible d’ajouter des hôtes Windows Server 2008 R2 et Windows Server 2012 à la liste d’hôtes à interroger, cette version (1.0) de SIL Aggregator prend uniquement en charge l’interrogation de Windows Server 2012 R2, pour des hôtes Windows/Hyper-V, afin que toutes les fonctionnalités soient disponibles.  En particulier, nous savons que lors de l’interrogation d’hôtes Windows Server 2008 R2, les machines virtuelles et les hôtes peuvent ne pas correspondre dans les rapports SIL Aggregator.

## <a name="see-also"></a>Voir aussi
[Agrégateur de journalisation de l’inventaire logiciel 1,0 pour Windows Server](https://www.microsoft.com/en-us/download/details.aspx?id=49046)<br>
[Applets de commande PowerShell de l’agrégateur SIL](https://technet.microsoft.com/library/mt548455.aspx)<br>
[Applets de commande PowerShell SIL](https://technet.microsoft.com/library/dn283390.aspx)<br>
[Vue d’ensemble de SIL](https://technet.microsoft.com/library/dn268301.aspx)<br>
[Gestion de SIL](https://technet.microsoft.com/library/dn383584.aspx)

