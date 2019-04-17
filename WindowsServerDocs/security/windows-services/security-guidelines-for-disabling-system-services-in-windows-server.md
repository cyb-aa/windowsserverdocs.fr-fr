---
title: Recommandations en matière de sécurité pour les services système dans Windows Server2016
description: Recommandations en matière de sécurité pour la désactivation des services dans Windows Server2016 avec expérience utilisateur
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 5/22/2017
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: 8f60d5095a3e4cebdffba5d3c02c69bc06326b3a
ms.sourcegitcommit: 68952ac7a29d0e2f07023958ad949fecc1b783b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2018
---
## <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>Obtenir des instructions sur la désactivation des services système sur Windows Server2016 avec expérience utilisateur

S’applique à: Windows Server2016

Le système d’exploitation Windows inclut de nombreux services système qui fournissent des fonctionnalités importantes. Différents services ont des stratégies de démarrage par défaut différente: certains sont démarrés par défaut (automatique), certains si nécessaire (manuels), et certaines sont désactivées par défaut et doivent être explicitement activées avant qu’ils s’exécutent. Ces valeurs par défaut ont été choisies avec soin pour chaque service afin d’équilibrer les performances, fonctionnalités et la sécurité pour les clients standard.

Toutefois, certains clients d’entreprise peuvent préférer un solde plus axées de sécurité pour leurs PC Windows et les serveurs, ce qui réduit au strict minimum leur surface d’attaque et peut donc le souhaitent entièrement désactiver tous les services qui ne sont pas nécessaires dans leurs environnements spécifiques. Pour les clients, Microsoft® fournit le Guide d’accompagnement sur lesquels les services peuvent être désactivés en toute sécurité à cet effet.

Le guide est pour Windows Server2016 avec expérience (sauf s’il est utilisé comme un remplacement de bureau pour les utilisateurs finaux). Chaque service sur le système est classé comme suit:

-   **Désactivez:** une axées sur la sécurité d’entreprise sera probablement préférez désactiver ce service et de renoncer à ses fonctionnalités (voir ci-dessous).
- **OK pour désactiver:** ce service fournit des fonctionnalités qui sont utile pour certaines entreprises et aux entreprises de sécurité qui n’utilisent pas peuvent la désactiver en toute sécurité.
- **Ne désactivez pas:** la désactivation de ce service sera impact sur les fonctionnalités essentielles ou empêcher les rôles spécifiques ou des fonctionnalités de fonctionner correctement. Il ne doit donc pas être désactivé.
-  **(Pas d’instructions):** l’impact de la désactivation de ces services n’a pas été entièrement évaluée. Par conséquent, la configuration par défaut de ces services ne doit pas être modifiée.


Clients peuvent configurer leurs PC Windows et les serveurs pour désactiver les services sélectionnés à l’aide de modèles de sécurité dans leurs stratégies de groupe ou à l’aide d’automatisation de PowerShell. Dans certains cas, le guide comprend des paramètres de stratégie de groupe spécifiques qui désactiver les fonctionnalités du service directement, comme alternative à la désactivation du service proprement dit.

Microsoft recommande aux clients de désactiver les services suivants et leurs tâches planifiées respectifs sur Windows Server2016 avec expérience utilisateur:

Services: 
1. Xbox Live Auth Manager
2. Xbox Live enregistrer de jeu

Tâches planifiées: 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(Vous pouvez également accéder aux informations sur tous les services sont décrites dans cet article en affichant la feuille de calcul MicrosoftExcel jointe: [obtenir des instructions sur la désactivation des Services système sur Windows Server2016 avec expérience](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx))

<br />

### <a name="disabling-services-not-installed-by-default"></a>La désactivation des services non installés par défaut

Microsoft déconseille d’appliquer des stratégies pour désactiver les services qui ne sont pas installés par défaut.
-  Le service est généralement nécessaire si la fonctionnalité est installée. L’installation du service ou la fonctionnalité requiert des droits d’administration. Ne pas autoriser l’installation des composants, pas le démarrage du service.
-  Bloquer le service MicrosoftWindows ne s’arrête pas un administrateur (ou non-admin dans certains cas) à partir de l’installation d’un équivalent tiers similaire, par exemple, un avec un risque de sécurité plus élevé.
-  Une ligne de base ou le banc d’essai qui désactive un service de Windows par défaut (par exemple, W3SVC) vous donne l’impression facilitant ainsie que la technologie (par exemple, IIS) n’est pas intrinsèquement sécurisée et ne doit jamais être utilisée à certains comptes.
-  Si la fonctionnalité (et le service) ne sont jamais installés, cela ajoute simplement en bloc inutile pour la ligne de base et pour le travail de vérification.

<br />
Pour tous les services système répertoriés dans ce document, les deux tableaux qui suivent proposent une explication des colonnes et des recommandations de Microsoft pour activer et désactiver les services système dans Windows Server2016 avec expérience utilisateur: 
 
<br />

### <a name="explanation-of-columns"></a>Explication de colonnes

| | |
|---|---|
|**Description du service**|   La description du service, à partir de sc.exe qdescription.|
|**Nom** |Nom de la clé (interne) du service|
|**Installation** |Toujours installé: Service est sur Server Core et serveur avec expérience utilisateur  <br /> Uniquement sur Datacenter Edition: Service est sur Server2016 avec expérience, mais ***pas*** sur Server Core |
|**StartType**  |Type de démarrage du service sur Windows Server2016|
|**Recommandation** |Recommandation Microsoft/conseils concernant la désactivation de ce service sur Windows Server2016dans un déploiement d’entreprise standard et bien gérée et où le serveur n’est pas en cours utilisé comme un remplacement de bureau de l’utilisateur final.|
|**Commentaires** |Explication supplémentaire|

<br />

### <a name="explanation-of-microsoft-recommendations"></a>Explication des recommandations de Microsoft

| | |
|---|---|
|**Ne désactivez pas** |Ce service ne doit pas être désactivé.|
|**Pour désactiver OK**| Ce service peut être désactivé si la fonctionnalité de que prise en charge n’est pas utilisée.|
|**Déjà désactivé**|  Ce service est désactivé par défaut; pas nécessaire d’appliquer la stratégie|
|**Doit être désactivé** |Ce service ne doit jamais être activé sur un système d’entreprise bien gérée.|

<br />

Les tableaux suivants offrent des conseils de Microsoft sur la désactivation des services système sur Windows Server2016 avec expérience utilisateur:

<br />

##  <a name="activex-installer-axinstsv"></a>Programme d’installation ActiveX (AxInstSV)

| | |
|---|---|
|   **Description du service** |   Fournit la validation de contrôle de compte d’utilisateur pour l’installation des contrôles ActiveX à partir d’Internet et permet la gestion de l’installation des contrôles ActiveX en fonction des paramètres de stratégie de groupe. Ce service est démarré à la demande et si désactivé l’installation des contrôles ActiveX se comportent en fonction des paramètres du navigateur par défaut.    |
|   **Nom du service**    |   AxInstSV    |
|   **Installation**    |   Uniquement sur Datacenter Edition  |
|   **StartType**   |   Manuelle  |
|   **Recommandation**  |   Pour désactiver OK   |
|   **Commentaires**    |   Pour désactiver si la fonctionnalité ne nécessaire pas OK |


<br />

## <a name="alljoyn-router-service"></a>Service de routeur AllJoyn   
| | |
|---|---|
|   **Description du service** |   Achemine les messages de AllJoyn pour les clients AllJoyn locales. Si ce service est arrêté, les clients AllJoyn qui n’ont pas leurs propres routeurs groupées sera impossible d’exécuter. |
|   **Nom du service**    |   AJRouter    |
|   **Installation**    |   Uniquement sur Datacenter Edition  |
|   **StartType**   |   Manuelle  |
|   **Recommandation**  | Pas d’instructions       |
|   **Commentaires**    |       |
| | |

<br />

## <a name="app-readiness"></a>Préparation de l’application
| | |
|---|---|
**Description du service** |   Obtient les applications prêts pour une utilisation la première fois qu’un utilisateur se connecte à cet ordinateur et lorsque vous ajoutez de nouvelles applications.
**Nom du service**    |   AppReadiness
**Installation**    |   Uniquement sur Datacenter Edition
**StartType**   |   Manuelle
**Recommandation**  |   Ne désactivez pas
**Commentaires**    |   
| | |

<br />

##  <a name="application-identity"></a>Identité de l’application
| | |       
|---|---|   
**Description du service** |   Détermine et vérifie l’identité d’une application. La désactivation de ce service empêchera AppLocker appliquées.
**Nom du service**    |   AppIDSvc
**Installation**    |   Toujours installé
**StartType**   |   Manuelle
**Recommandation**  |Pas d’instructions    
**Commentaires**    |   
|||     

<br />

##  <a name="application-information"></a>Informations de l’application 
| | |       
|---|---|   
|   **Description du service** |   Facilite l’exécution des applications interactives avec des privilèges d’administration supplémentaires.  Si ce service est arrêté, les utilisateurs ne pourront pas lancer des applications avec des privilèges d’administration supplémentaires que nécessaires pour effectuer des tâches de l’utilisateur de votre choix.
|   **Nom du service**    |   AppInfo
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   Prend en charge une élévation le même bureau UAC
|||     

<br />

##  <a name="application-layer-gateway-service"></a>Service de passerelle de couche application       
| | |           
|---|---|           
|   **Description du service** |   Prend en charge les plug-ins de protocole tiers pour le partage de connexion Internet
|   **Nom du service**    |   MOTEUR
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |Pas d’instructions    
|   **Commentaires**    |   
|||     

<br />

##  <a name="application-management"></a>Gestion d’applications      
| | |           
|---|---|       
|   **Description du service** |   Traite les demandes d’énumération, installation et suppression de logiciels déployés via Stratégie de groupe. Si le service est désactivé, les utilisateurs ne pourront pas installer, supprimer ou énumérer le logiciel déployé via la stratégie de groupe. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   AppMgmt
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="appx-deployment-service-appxsvc"></a>Service de déploiement AppX (AppXSVC)       
| | |           
|---|---|
|   **Description du service** |   Fournit la prise en charge de l’infrastructure pour déployer des applications du Windows Store. Ce service est démarré à la demande et si les applications du Windows Store désactivées ne seront pas déployées sur le système et ne peuvent pas fonctionner correctement.
|   **Nom du service**    |   AppXSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="auto-time-zone-updater"></a>Mise à jour de fuseau horaire automatique           
| | |           
|---|---|           
|   **Description du service** |   Définit automatiquement le fuseau horaire du système.
|   **Nom du service**    |   tzautoupdate
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Désactivé
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="background-intelligent-transfer-service"></a>Service de transfert Intelligent en arrière-plan          
| | |           
|---|---|   
|   **Description du service** |   Transferts de fichiers en arrière-plan à l’aide de la bande passante réseau inactive. Si le service est désactivé, toutes les applications qui dépendent de BITS, tels que Windows Update ou MSN Explorer, sera impossible de télécharger automatiquement les programmes et autres informations.
|   **Nom du service**    |   BITS
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          


## <a name="background-tasks-infrastructure-service"></a>Service Infrastructure de tâches en arrière-plan      
| | |           
|---|---|   
|   **Description du service** |   Service d’infrastructure Windows qui contrôle les tâches en arrière-plan permettre s’exécuter sur le système.
|   **Nom du service**    |   BrokerInfrastructure
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="base-filtering-engine"></a>Filtrage de moteur de base            
| | |           
|---|---|       
|   **Description du service** |   La Base de filtrage moteur est un service qui gère les pare-feu et les stratégies Internet Protocol security (IPsec) et implémente le filtrage en mode utilisateur. Arrêtez ou désactivez le service BFE permet de réduire considérablement la sécurité du système. Elle entraîne également un comportement imprévisible dans les applications de pare-feu et de gestion d’IPsec.
|   **Nom du service**    |   BFE
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="bluetooth-support-service"></a>Service de prise en charge Bluetooth            
| | |           
|---|---|   
|   **Description du service** |   Le service Bluetooth prend en charge la découverte et association de périphériques Bluetooth distants.  Arrêt ou la désactivation de ce service risque de ne parviennent pas à fonctionner correctement et empêcher nouveaux appareils détectés ou à associer les périphériques Bluetooth déjà installés.
|   **Nom du service**    |   bthserv
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   OK pour la désactiver si ne pas utilisé. Un autre mécanisme de désactivation:https://technet.microsoft.com/en-us/library/dd252791.aspx
|||         
            
<br />          


## <a name="cdpusersvc"></a>CDPUserSvc           
| | |           
|---|---|   
|   **Description du service** |   Ce service de l’utilisateur est utilisé pour les scénarios de plate-forme de périphériques connectés
|   **Nom du service**    |   CDPUserSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Automatique
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Modèle de service utilisateur
|||         
            
<br />          


##  <a name="certificate-propagation"></a>Propagation de certificat     
| | |           
|---|---|
|   **Description du service** |   Copie des certificats d’utilisateur et des certificats racine à partir de cartes à puce dans le magasin de certificats de l’utilisateur actuel, détecte lorsqu’une carte à puce est insérée dans un lecteur de carte à puce et si nécessaire, installe le minipilote de Plug-and-Play de carte à puce.
|   **Nom du service**    |   CertPropSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="client-license-service-clipsvc"></a>Service de licence client (ClipSVC)        
| | |           
|---|---|   
|   **Description du service** |   Fournit la prise en charge de l’infrastructure pour le MicrosoftStore. Ce service est démarré à la demande et si vous avez acheté des applications désactivées à l’aide de MicrosoftStore se comportera pas correctement.
|   **Nom du service**    |   ClipSVC
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="cng-key-isolation"></a>Isolation de clé CNG
| | |           
|---|---|   
|   **Description du service** |   Le service d’isolation de clé CNG est hébergé dans le processus LSA. Le service fournit une isolation de processus de la clé pour les clés privées et les opérations de chiffrement associées comme requis par les critères communs. Le service stocke et utilise des clés à long terme dans un processus sécurisé répondant aux exigences des critères communs.
|   **Nom du service**    |   KeyIso
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="com-event-system"></a>Système d’événements de COM +       
| | |           
|---|---|       
|   **Description du service** |   Prend en charge système événement Notification Service (SENS), qui assure la distribution automatique des événements en vous abonnant composants (Component Object Model). Si le service est arrêté, SENS se ferme et ne sera pas en mesure de fournir des notifications d’ouverture de session et la fermeture de session. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   EventSystem
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="com-system-application"></a>Application système COM +     
| | |           
|---|---|       
|   **Description du service** |   Gère la configuration et le suivi du modèle COM (Component Object) + composants. Si le service est arrêté, la plupart des COM +-des composants de base ne fonctionnera pas correctement. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   COMSysApp
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="computer-browser"></a>Explorateur d’ordinateurs        
| | |           
|---|---|       
|   **Description du service** |   Gère une liste de mises à jour des ordinateurs sur le réseau et fournit cette liste aux ordinateurs désignés comme navigateurs. Si ce service est arrêté, cette liste ne sera pas mis à jour ou conservée. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   Navigateur
|   **Installation**    |   Toujours installé
|   **StartType**   |   Désactivé
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="connected-devices-platform-service"></a>Service de plateforme d’appareils connectés       
| | |           
|---|---|       
|   **Description du service** |   Ce service est utilisé pour les scénarios de périphériques connectés et verre universelle
|   **Nom du service**    |   CDPSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="connected-user-experiences-and-telemetry"></a>Expériences utilisateur de connexion et télémétrie     
| | |           
|---|---|       
|   **Description du service** |   Le service rencontre des utilisateurs connectés et télémétrie Active les fonctionnalités qui prennent en charge les expériences utilisateur dans l’application et connecté. En outre, ce service gère la collection piloté par événement et la transmission des informations de diagnostic et d’utilisation (utilisées pour améliorer l’expérience et la qualité de la plateforme Windows) lorsque les diagnostics et les paramètres d’option de confidentialité de l’utilisation sont activés dans les commentaires et Diagnostics.
|   **Nom du service**    |   DiagTrack
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="contact-data"></a>Données de contact        
| | |           
|---|---|       
|   **Description du service** |   Index des données de contact pour la recherche rapide de contact. Si vous arrêtez ou désactivez ce service, les contacts peuvent être absentes de vos résultats de recherche.
|   **Nom du service**    |   PimIndexMaintenanceSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Modèle de service utilisateur
|||         
            
<br />          

## <a name="coremessaging"></a>CoreMessaging            
| | |           
|---|---|           
|   **Description du service** |   Gère la communication entre les composants du système.
|   **Nom du service**    |   CoreMessagingRegistrar
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="credential-manager"></a>Gestionnaire d’informations d’identification           
| | |           
|---|---|       
|   **Description du service** |   Fournit le stockage sécurisé et la récupération des informations d’identification pour les utilisateurs, les applications et les packages de service de sécurité.
|   **Nom du service**    |   VaultSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="cryptographic-services"></a>Services de chiffrement           
| | |           
|---|---|       
|   **Description du service** |   Fournit trois services de gestion: Service de base de données du catalogue, qui confirme la signature des fichiers Windows et permet de nouveaux programmes à installer; service de racine protégée, qui ajoute et supprime les certificats d’autorité de Certification racine de confiance de cet ordinateur; et Service automatique des mises à jour de certificat racine, qui Récupère les certificats racine à partir de Windows Update et activer des scénarios tels que SSL. Si ce service est arrêté, ces services de gestion ne fonctionnera pas correctement. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   CryptSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="data-sharing-service"></a>Service de partage de données         
| | |           
|---|---|       
|   **Description du service** |   Fournit des données intermédiaire entre les applications.
|   **Nom du service**    |   DsSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          
| | |           
|---|---|       
|   **Description du service** |   Le service DCP (collecte de données et la publication) prend en charge les applications de premier tiers pour charger des données vers le cloud.
|   **Nom du service**    |   DcpSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="dcom-server-process-launcher"></a>Lanceur de processus DCOM         
| | |           
|---|---|       
|   **Description du service** |   Le service DCOMLAUNCH lance des serveurs COM et DCOM en réponse aux demandes d’activation d’objet. Si ce service est arrêté ou désactivé, les programmes à l’aide de COM ou DCOM ne fonctionnera pas correctement. Il est fortement recommandé d’avoir l’exécution du service DCOMLAUNCH.
|   **Nom du service**    |   DcomLaunch
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  |Pas d’instructions    
|   **Commentaires**    |   
|||         
            
<br />

##  <a name="device-association-service"></a>Service d’Association de périphérique      
| | |           
|---|---|       
|   **Description du service** |   Active le couplage entre le système et les appareils câblés ou sans fil.
|   **Nom du service**    |   DeviceAssociationService
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          
        
##  <a name="device-install-service"></a>Service d’installation de périphériques      
| | |           
|---|---|       
|   **Description du service** |   Permet à un ordinateur de reconnaître et de s’adapter aux modifications matérielles avec peu ou pas l’entrée utilisateur. Arrêt ou la désactivation de ce service entraîne une instabilité du système.
|   **Nom du service**    |   DeviceInstall
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="device-management-enrollment-service"></a>Service d’inscription de gestion de périphérique        
| | |           
|---|---|       
|   **Description du service** |   Effectue des activités d’inscription de périphérique pour la gestion des périphériques
|   **Nom du service**    |   DmEnrollmentSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="device-setup-manager"></a>Gestionnaire d’installation de périphériques         
| | |           
|---|---|       
|   **Description du service** |   Permet la détection, le téléchargement et l’installation de logiciels liés à l’appareil. Si ce service est désactivé, les périphériques peuvent être configurés avec un logiciel obsolète et peut ne pas fonctionneront correctement.
|   **Nom du service**    |   DsmSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="devquery-background-discovery-broker"></a>Broker de découverte en arrière-plan DevQuery         
| | |           
|---|---|           
|   **Description du service** |   Permet d’applications détecter les périphériques avec une tâche d’arrière-plan
|   **Nom du service**    |   DevQueryBroker
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="dhcp-client"></a>Client DHCP          
| | |           
|---|---|       
|   **Description du service** |   Inscrit et met à jour des adresses IP et les enregistrements DNS pour cet ordinateur. Si ce service est arrêté, cet ordinateur ne recevra pas les adresses IP dynamiques et les mises à jour DNS. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   DHCP
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="diagnostic-policy-service"></a>Service de stratégie de diagnostic            
| | |           
|---|---|       
|   **Description du service** |   Le Service de stratégie de Diagnostic permet de détection, de résolution des problèmes et de résolution pour les composants Windows.  Si ce service est arrêté, diagnostics ne fonctionneront plus.
|   **Nom du service**    |   STD
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="diagnostic-service-host"></a>Hôte du Service de diagnostic     
| | |           
|---|---|       
|   **Description du service** |   L’hôte du Service de diagnostics est utilisé par le Service de stratégie de Diagnostic de diagnostic hôte qui doivent être exécutées dans un contexte de Service Local.  Si ce service est arrêté, les tests de diagnostic qui en dépendent ne fonctionneront plus.
|   **Nom du service**    |   WdiServiceHost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="diagnostic-system-host"></a>Hôte de système de diagnostic           
| | |           
|---|---|       
|   **Description du service** |   L’hôte de système de diagnostics est utilisé par le Service de stratégie de Diagnostic de diagnostic hôte qui doivent être exécutées dans un contexte système Local.  Si ce service est arrêté, les tests de diagnostic qui en dépendent ne fonctionneront plus.
|   **Nom du service**    |   WdiSystemHost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="distributed-link-tracking-client"></a>Client de suivi de lien distribué            
| | |           
|---|---|   
|   **Description du service** |   Conserve les liens entre les fichiers NTFS sur un ordinateur ou entre les ordinateurs dans un réseau.
|   **Nom du service**    |   TrkWks
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="distributed-transaction-coordinator"></a>Coordinateur de transactions distribuées     
| | |           
|---|---|   
|   **Description du service** |   Coordonne les transactions qui s’étendent sur plusieurs gestionnaires de ressources, telles que les bases de données, les files d’attente et les systèmes de fichiers. Si ce service est arrêté, ces transactions échouera. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   MSDTC
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />  

##  <a name="dmwappushsvc"></a>dmwappushsvc        
| | |           
|---|---|       
|   **Description du service** |   Service de routage de messages WAP Push
|   **Nom du service**    |   dmwappushservice
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Service requis sur les périphériques clients pour Intune, GPM et technologies similaires de gestion et de filtre d’écriture unifié. Pas nécessaire pour le serveur.
|||         
            
<br />      

##  <a name="dns-client"></a>Client DNS      
| | |           
|---|---|       
|   **Description du service** |   Le service Client DNS (cache DNS) met en cache les noms du système DNS (Domain Name) et enregistre le nom complet de l’ordinateur pour cet ordinateur. Si le service est arrêté, les noms DNS continue à résoudre. Toutefois, les résultats des requêtes de noms DNS ne sont pas mises en cache et le nom d’ordinateur ne sera pas enregistré. Si le service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   Cache DNS
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="downloaded-maps-manager"></a>Cartes téléchargées Manager     
| | |           
|---|---|   
|   **Description du service** |   Service Windows pour l’application à accéder à des cartes téléchargées. Ce service est démarré à la demande par application l’accès à téléchargé maps. La désactivation de ce service empêchera les applications d’accéder à des cartes.
|   **Nom du service**    |   MapsBroker
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Automatique
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   La désactivation de sauts les applications qui s’appuient sur le service; oK désactiver si les applications ne vous basez ne pas sur ce dernier
|||         
            
<br />          

## <a name="embedded-mode"></a>En Mode incorporé            
| | |           
|---|---|       
|   **Description du service** |   Le service en Mode incorporé permet des scénarios relatifs aux Applications en arrière-plan.  La désactivation de ce service empêchera d’Applications en arrière-plan en cours d’activation.
|   **Nom du service**    |   embeddedmode
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="encrypting-file-system-efs"></a>Système de fichiers EFS (Encrypting File System)
| | |                   
|---|---|           
|   **Description du service** | Fournit la technologie de chiffrement de fichier importante utilisée pour stocker les fichiers chiffrés sur des volumes de système de fichiers NTFS. Si ce service est arrêté ou désactivé, les applications ne pourront pas accéder aux fichiers chiffrés.            
|   **Nom du service**  |  SYSTÈME EFS            
|   **Installation**  |  Toujours installé           
|   **StartType**   |  Manuelle           
|   **Recommandation**  | Pas d’instructions           
|   **Commentaires**   |
|||                 
                            
<br />  

## <a name="enterprise-app-management-service"></a>Service de gestion d’application entreprise            
| | |           
|---|---|       
|   **Description du service** |   Permet la gestion des applications.
|   **Nom du service**    |   EntAppSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="extensible-authentication-protocol"></a>Protocole EAP           
| | |           
|---|---|   
|   **Description du service** |   Le service de protocole EAP (Extensible Authentication) fournit l’authentification réseau dans des scénarios tels que 802. 1 x câblé et sans fil, VPN et Protection d’accès réseau (NAP).  EAP fournit également des interfaces de programmation d’application (API) qui sont utilisés par les clients d’accès réseau, y compris sans fil et les clients VPN, pendant le processus d’authentification.  Si vous désactivez ce service, cet ordinateur ne peut pas accéder aux réseaux qui nécessitent l’authentification EAP.
|   **Nom du service**    |   EapHost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="function-discovery-provider-host"></a>Hôte de fournisseur de découverte de fonctions         
| | |           
|---|---|       
|   **Description du service** |   Le service FDPHOST héberge les fournisseurs de découverte du réseau découverte de fonction (DP). Ces fournisseurs FD fournissent des services de découverte de réseau pour la Simple Services SSDP (Discovery Protocol) et les Services Web - protocole de découverte (WS-D). Arrêtez ou désactivez le service FDPHOST va désactiver la découverte du réseau pour ces protocoles lors de l’utilisation du lecteur de disquette. Lorsque ce service n’est pas disponible, les services réseau à l’aide du lecteur de disquette et appuyer sur ces protocoles découverte sera impossible de trouver des ressources ou périphériques réseau.
|   **Nom du service**    |   fdPHost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="function-discovery-resource-publication"></a>Publication de ressources de découverte de fonctions      
| | |           
|---|---|       
|   **Description du service** |   Publie cet ordinateur et les ressources connectées à cet ordinateur afin qu’ils peuvent être détectés sur le réseau.  Si ce service est arrêté, les ressources de réseau seront n’est plus publiés et ils ne seront pas découverts par d’autres ordinateurs sur le réseau.
|   **Nom du service**    |   FDResPub
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="geolocation-service"></a>Service de géolocalisation          
| | |           
|---|---|       
|   **Description du service** |   Ce service surveille l’emplacement actuel du système et gère les limites géographiques (un emplacement géographique avec des événements associés).  Si vous désactivez ce service, les applications ne pourront pas utiliser ou de recevoir des notifications de géolocalisation ou les limites géographiques.
|   **Nom du service**    |   lfsvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   La désactivation de sauts les applications qui s’appuient sur le service; oK désactiver si les applications ne vous basez ne pas sur ce dernier
|||         
            
<br />          

##  <a name="group-policy-client"></a>Client de stratégie de groupe     
| | |           
|---|---|       
|   **Description du service** |   Le service est responsable de l’application des paramètres configurés par les administrateurs de l’ordinateur et les utilisateurs via le composant de stratégie de groupe. Si le service est désactivé, les paramètres ne seront pas appliqués et les applications et composants ne seront pas facile à gérer par le biais de stratégie de groupe. Les composants ou les applications qui dépendent du composant de stratégie de groupe n’est peut-être pas fonctionnelles si le service est désactivé.
|   **Nom du service**    |   (gpsvc)
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          


## <a name="human-interface-device-service"></a>Service de périphériques d’Interface utilisateur           
| | |           
|---|---|       
|   **Description du service** |   Active et gère l’utilisation des boutons actifs sur les claviers, les commandes à distance et les autres périphériques multimédias. Il est recommandé de conserver ce service en cours d’exécution.
|   **Nom du service**    |   Hidserv
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="hv-host-service"></a>HV hôte Service     
| | |           
|---|---|   
|   **Description du service** |   Fournit une interface pour l’hyperviseur Hyper-V fournir des compteurs de performance par partition du système d’exploitation hôte.
|   **Nom du service**    |   HvHost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Goût de performances pour les ordinateurs virtuels invités. Non utilisé aujourd'hui à l’exception d’explicitement rempli des ordinateurs virtuels, mais sera utilisé dans l’Application Guard
|||         
            
<br />          

## <a name="hyper-v-data-exchange-service"></a>Service échange de données Hyper-V        
| | |           
|---|---|   
|   **Description du service** |   Fournit un mécanisme d’échange de données entre l’ordinateur virtuel et le système d’exploitation en cours d’exécution sur l’ordinateur physique.
|   **Nom du service**    |   vmickvpexchange
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Voir HvHost
|||         
            
<br />      

## <a name="hyper-v-guest-service-interface"></a>Interface de services d’invité Hyper-V          
| | |           
|---|---|   
|   **Description du service** |   Fournit une interface pour l’ordinateur hôte Hyper-V interagir avec des services spécifiques à l’intérieur de l’ordinateur virtuel.
|   **Nom du service**    |   vmicguestinterface
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Voir HvHost
|||         
            
<br />  

## <a name="hyper-v-guest-shutdown-service"></a>Service arrêt de l’invité Hyper-V           
| | |           
|---|---|       
|   **Description du service** |   Fournit un mécanisme pour arrêter le système d’exploitation de cet ordinateur virtuel à partir des interfaces de gestion sur l’ordinateur physique.
|   **Nom du service**    |   vmicshutdown
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Voir HvHost
|||         
            
<br />          
    
## <a name="hyper-v-heartbeat-service"></a>Service pulsation MicrosoftHyper-V            
| | |           
|---|---|           
|   **Description du service** |   Analyse l’état de cet ordinateur virtuel par le rapport d’une pulsation à intervalles réguliers. Ce service vous permet d’identifier les machines virtuelles en cours d’exécution qui ont cessé de répondre.
|   **Nom du service**    |   vmicheartbeat
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Voir HvHost
|||         
            
<br />          

## <a name="hyper-v-powershell-direct-service"></a>Service Hyper-V PowerShell Direct            
| | |           
|---|---|       
|   **Description du service** |   Fournit un mécanisme pour gérer la machine virtuelle avec PowerShell via une session de machine virtuelle sans réseau virtuel.
|   **Nom du service**    |   vmicvmsession
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Voir HvHost
|||         
            
<br />          

## <a name="hyper-v-remote-desktop-virtualization-service"></a>Service de virtualisation des services Bureau à distance Hyper-V            
| | |           
|---|---|       
|   **Description du service** |   Fournit une plateforme pour la communication entre l’ordinateur virtuel et le système d’exploitation en cours d’exécution sur l’ordinateur physique.
|   **Nom du service**    |   vmicrdv
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Voir HvHost
|||         
            
<br />          

## <a name="hyper-v-time-synchronization-service"></a>Service de synchronisation Hyper-V         
| | |           
|---|---|       
|   **Description du service** |   Synchronise l’heure système de cet ordinateur virtuel avec l’heure système de l’ordinateur physique.
|   **Nom du service**    |   vmictimesync
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Voir HvHost
|||         
            
<br />          

## <a name="hyper-v-volume-shadow-copy-requestor"></a>Hyper-V VSS         
| | |           
|---|---|           
|   **Description du service** |   Coordonne les communications qui sont nécessaires pour utiliser le Service VSS pour sauvegarder des applications et des données sur cet ordinateur virtuel à partir du système d’exploitation sur l’ordinateur physique.
|   **Nom du service**    |   vmicvss
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Voir HvHost
|||         
            
<br />          

## <a name="ike-and-authip-ipsec-keying-modules"></a>IKE et AuthIP Modules de génération de clés          
| | |           
|---|---|       
|   **Description du service** |   Le service IKEEXT héberge le Exchange IKE (Internet Key) et Internet AuthIP (Authenticated Protocol) modules de génération de clés. Ces modules de gestion de clés sont utilisés pour l’authentification et l’échange de clés de sécurité du protocole Internet (IPsec). Arrêtez ou désactivez le service IKEEXT désactivera l’échange de clés IKE et AuthIP avec des ordinateurs homologues. IPsec est généralement configuré pour utiliser IKE et AuthIP; par conséquent, arrêtez ou désactivez le service IKEEXT peut entraîner un échec d’IPsec et peut compromettre la sécurité du système. Il est fortement recommandé d’avoir l’exécution du service IKEEXT.
|   **Nom du service**    |   IKEEXT
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |    
|||         
            
<br />          

## <a name="interactive-services-detection"></a>Détection de Services interactifs           
| | |           
|---|---|   
|   **Description du service** |   Active la notification des entrées utilisateur pour les services interactifs, ce qui permet d’accéder aux boîtes de dialogue créées par les services interactifs lorsqu’elles apparaissent. Si ce service est arrêté, les notifications de boîtes de dialogue Nouveau service interactif ne fonctionneront plus et il peut ne pas être accès aux boîtes de dialogue service interactif. Si ce service est désactivé, les notifications et l’accès aux boîtes de dialogue Nouveau service interactif ne fonctionneront plus.
|   **Nom du service**    |   UI0Detect
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />  

## <a name="internet-connection-sharing-ics"></a>Connexion Internet (ICS) de partage            
| | |           
|---|---|           
|   **Description du service** |   Fournit la traduction d’adresses réseau, d’adressage, services de prévention des intrusions et/ou de la résolution de nom pour un réseau domestique ou de petite entreprise.
|   **Nom du service**    |   SharedAccess
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Requis pour les clients utilisés comme points d’accès Wi-Fi et également sur les deux extrémités de la projection Miracast. Partage de connexion Internet peut être bloqué avec le paramètre de stratégie de groupe, «Interdire l’utilisation du partage de connexion Internet sur votre réseau de domaine DNS»
|||         
            
<br />          

## <a name="ip-helper"></a>Application d’assistance IP            
| | |           
|---|---|       
|   **Description du service** |   Fournit la connectivité de tunnel à l’aide de technologies de transition IPv6 (6to4, Teredo, ISATAP et Port Proxy) et IP-HTTPS. Si ce service est arrêté, l’ordinateur n’aura pas les avantages de connectivité améliorée ces technologies offrent.
|   **Nom du service**    |   Iphlpsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          


##  <a name="ipsec-policy-agent"></a>Agent de stratégie IPsec      
| | |           
|---|---|       
|   **Description du service** |   Internet Protocol security (IPsec) prend en charge au niveau du réseau homologue d’authentification, authentification de l’origine, l’intégrité des données, confidentialité des données (chiffrement) et protection contre la relecture.  Ce service applique les stratégies IPsec créées par le biais du composant logiciel enfichable Stratégies de sécurité IP ou l’outil de ligne de commande «netsh ipsec».  Si vous arrêtez ce service, vous pouvez rencontrer des problèmes de connectivité réseau si votre stratégie nécessite des connexions IPsec.  En outre, la gestion à distance du pare-feu Windows n’est pas disponible lorsque ce service est arrêté.
|   **Nom du service**    |   Agent de stratégie
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />

##  <a name="kdc-proxy-server-service-kps"></a>Service de serveur de Proxy KDC (KPS)      
| | |           
|---|---|       
|   **Description du service** |   Serveur de Proxy KDC service s’exécute sur les serveurs de périphérie au proxy Kerberos aux contrôleurs de domaine sur le réseau d’entreprise, les messages de protocole.
|   **Nom du service**    |   KPSSVC
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions    
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="ktmrm-for-distributed-transaction-coordinator"></a>Service KtmRm pour Distributed Transaction Coordinator (MSDTC)            
| | |           
|---|---|       
|   **Description du service** |   Coordonne les transactions entre Distributed Transaction Coordinator (MSDTC) et le Gestionnaire de transactions du noyau (KTM). S’il n’est pas nécessaire, il est recommandé que ce rester service s’est arrêté. S’il est nécessaire, MSDTC et KTM démarrera ce service automatiquement. Si ce service est désactivé, toutes les transactions MSDTC interagir avec un gestionnaire de ressources du noyau échoue et tous les services qui en dépendent explicitement échouera.
|   **Nom du service**    |   KtmRm
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />

##  <a name="link-layer-topology-discovery-mapper"></a>Mappeur de détection de topologie de couche de liaison        
| | |       
|---|---|       
|   **Description du service** |   Crée une carte réseau, consistant en PC et les informations de topologie (connectivité) de périphérique et les métadonnées décrivant chaque ordinateur et l’appareil.  Si ce service est désactivé, la carte réseau ne fonctionnera pas correctement.
|   **Nom du service**    |   lltdsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   OK désactiver si aucune dépendance sur une carte réseau
|||         
            
<br />

## <a name="local-session-manager"></a>Gestionnaire de Session locale                    
| | |                   
|---|---|   
|   **Description du service** |   Principaux services Windows qui gère les sessions utilisateur local. Arrêt ou la désactivation de ce service entraîne une instabilité du système.    
|   **Nom du service**    |   LSM |
|   **Installation**    |   Toujours installé    |
|   **StartType**   |   Automatique   |
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||                 
                    
<br />                  

## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Collecteur Standard du Hub de diagnostic Microsoft (R)         
| | |           
|---|---|           
|   **Description du service** |   Principaux services Windows qui gère les sessions utilisateur local. Arrêt ou la désactivation de ce service entraîne une instabilité du système.
|   **Nom du service**    |   LSM
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          
            
## <a name="microsoft-account-sign-in-assistant"></a>Assistant de connexion compte Microsoft          
| | |           
|---|---|       
|   **Description du service** |   Permet la connexion de l’utilisateur par le biais de services d’identité compte Microsoft. Si ce service est arrêté, les utilisateurs ne seront pas en mesure d’ouvrir une session sur l’ordinateur avec son compte Microsoft.
|   **Nom du service**    |   wlidsvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Accounts Microsoft sont non applicable sur Windows Server
|||         
            
<br />          

##  <a name="microsoft-app-v-client"></a>Client MicrosoftApp-V      
| | |           
|---|---|       
|   **Description du service** |   Gère les utilisateurs App-V et des applications virtuelles
|   **Nom du service**    |   AppVClient
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Désactivé
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="microsoft-iscsi-initiator-service"></a>Service initiateur Microsoft iSCSI            
| | |           
|---|---|       
|   **Description du service** |   Gère les sessions iSCSI (Internet SCSI) à partir de cet ordinateur pour les périphériques cible iSCSI à distance. Si ce service est arrêté, cet ordinateur ne sera pas en mesure de se connecter ni accéder aux cibles iSCSI. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   MSiSCSI
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Nos données de diagnostic indiquent que ce est utilisé sur le client, ainsi que des serveurs. Aucun avantage à la désactivation de.
|||         
            
<br />          

## <a name="microsoft-passport"></a>MicrosoftPassport           
| | |           
|---|---|   
|   **Description du service** |   Fournit une isolation de processus pour les clés de chiffrement utilisée pour s’authentifier auprès des fournisseurs d’identité associé d’un utilisateur. Si ce service est désactivé, toutes les utilisations et la gestion de ces clés sera pas disponibles, qui inclut une ouverture de session ordinateur et d’authentification unique, sur des applications et des sites Web. Ce service démarre et arrête automatiquement. Il est recommandé que vous ne reconfigurez pas ce service.
|   **Nom du service**    |   NgcSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Requis pour les ouvertures de session PIN/Hello, qui ne sont pas pris en charge sur le serveur
|||         
            
<br />          

## <a name="microsoft-passport-container"></a>Conteneur MicrosoftPassport         
| | |           
|---|---|       
|   **Description du service** |   Gère les clés d’identité utilisateur local utilisés pour authentifier l’utilisateur pour les fournisseurs d’identité, ainsi que les cartes à puce virtuelles TPM. Si ce service est désactivé, les clés d’identité utilisateur local et les cartes à puce virtuelles TPM ne sera pas accessibles. Il est recommandé que vous ne reconfigurez pas ce service.
|   **Nom du service**    |   NgcCtnrSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="microsoft-software-shadow-copy-provider"></a>Fournisseur de clichés instantanés logiciels Microsoft          
| | |           
|---|---|       
|   **Description du service** |   Gère les clichés instantanés de volume de logiciel pris par le service de cliché instantané de Volume. Si ce service est arrêté, les clichés instantanés de volume de logiciel ne peut être gérés. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   SWPRV
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="microsoft-storage-spaces-smp"></a>Espaces de stockage MicrosoftSMP         
| | |           
|---|---|       
|   **Description du service** |   Service de l’hôte pour le fournisseur de gestion des espaces de stockage Microsoft. Si ce service est arrêté ou désactivé, les espaces de stockage ne peut pas être gérés.
|   **Nom du service**    |   smphost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Gestion du stockage API échouent sans ce service. Exemple: «Get-WmiObject-classe MSFT_Disk - Namespace Root\Microsoft\Windows\Storage».
|||         
            
<br />          

## <a name="nettcp-port-sharing-service"></a>Service de partage de Port Net.Tcp         
| | |           
|---|---|       
|   **Description du service** |   Offre la possibilité de partager des ports TCP sur le protocole net.tcp.
|   **Nom du service**    |   NetTcpPortSharing
|   **Installation**    |   Toujours installé
|   **StartType**   |   Désactivé
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="netlogon"></a>Accès réseau         
| | |           
|---|---|           
|   **Description du service** |   Maintient un canal sécurisé entre cet ordinateur et le contrôleur de domaine pour l’authentification des utilisateurs et les services. Si ce service est arrêté, l’ordinateur ne peut pas authentifier les utilisateurs et les services et le contrôleur de domaine ne peut pas inscrire des enregistrements DNS. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   Accès réseau
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="network-connection-broker"></a>Service Broker pour les connexions réseau            
| | |           
|---|---|       
|   **Description du service** |   Connexions aux courtiers qui permettent aux applications du WindowsStoreMicrosoft recevoir des notifications à partir d’internet.
|   **Nom du service**    |   NcbService
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="network-connections"></a>Connexions réseau         
| | |           
|---|---|   
|   **Description du service** |   Gère les objets dans le dossier Connexions réseau et accès à distance, dans lequel vous pouvez afficher le réseau local et les connexions à distance.
|   **Nom du service**    |   NETMAN
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="network-connectivity-assistant"></a>Assistant de connectivité réseau      
| | |           
|---|---|       
|   **Description du service** |   Fournit une notification d’état de DirectAccess pour les composants d’interface utilisateur
|   **Nom du service**    |   NcaSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />  

##  <a name="network-list-service"></a>Service de liste de réseau        
| | |           
|---|---|   
|   **Description du service** |   Identifie les réseaux auquel l’ordinateur est connecté, collecte et stocke les propriétés de ces réseaux et avertit les applications lors de la modification des propriétés.
|   **Nom du service**    |   netprofm
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="network-location-awareness"></a>Connaissance des emplacements réseau           
| | |           
|---|---|       
|   **Description du service** |   Collecte et stocke les informations de configuration pour le réseau et avertit les programmes lors de la modification de ces informations. Si ce service est arrêté, les informations de configuration ne soient pas disponibles. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   NlaSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="network-setup-service"></a>Service de configuration réseau       
| | |           
|---|---|       
|   **Description du service** |   Le Service de configuration de réseau gère l’installation de pilotes réseau et permet la configuration des paramètres réseau de bas niveau.  Si ce service est arrêté, il sont possible d’annuler toutes les installations de pilotes qui sont en cours.
|   **Nom du service**    |   NetSetupSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="network-store-interface-service"></a>Service d’Interface réseau Store      
| | |           
|---|---|   
|   **Description du service** |   Ce service fournit des notifications de réseau (par exemple, interface Ajout/Suppression d’etc.) pour les clients en mode utilisateur. Arrêt du service entraîne la perte de connectivité réseau. Si ce service est désactivé, tous les autres services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   nSi
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="offline-files"></a>Fichiers hors connexion            
| | |           
|---|---|       
|   **Description du service** |   Le service de fichiers hors connexion effectue les opérations de maintenance sur le cache des fichiers hors connexion, répond à la fermeture de session et d’ouverture de session utilisateur événements implémente les détails internes de l’API publique et distribue les événements intéressants à ceux qui souhaitent dans les activités des fichiers hors connexion et les modifications d’état du cache.
|   **Nom du service**    |   CscService
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Désactivé
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="optimize-drives"></a>Optimiser les lecteurs          
| | |           
|---|---|   
|   **Description du service** |   Permet à l’ordinateur de s’exécuter plus efficacement par l’optimisation des fichiers sur les lecteurs de stockage.
|   **Nom du service**    |   defragsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />

## <a name="performance-counter-dll-host"></a>Hôte DLL de compteur de performances         
| | |           
|---|---|       
|   **Description du service** |   Permet aux utilisateurs distants et les processus 64bits d’interroger les compteurs de performance fournis par les DLL 32bits. Si ce service est arrêté, seuls les utilisateurs locaux et les processus 32bits sera en mesure d’interroger les compteurs de performance fournis par les DLL 32bits.
|   **Nom du service**    |   PerfHost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions    
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="performance-logs--alerts"></a>Alertes et les journaux de performances            
| | |           
|---|---|   
|   **Description du service** |   Les données de performances des journaux de performances et la collecte d’alertes à partir d’ordinateurs locaux ou distants, en fonction des paramètres de planification préconfigurés, puis écrit les données dans un journal ou déclenche une alerte. Si ce service est arrêté, les informations sur les performances ne seront pas collectées. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   Pla
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="phone-service"></a>Service téléphonique       
| | |           
|---|---|   
|   **Description du service** |   Gère l’état de téléphonie sur l’appareil
|   **Nom du service**    |   PhoneSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Utilisé par les applications modernes VoIP
|||         
            
<br />          

##      <a name="plug-and-play"></a>Plug-and-Play       
| | |           
|---|---|   
|   **Description du service** |   Permet à un ordinateur de reconnaître et de s’adapter aux modifications matérielles avec peu ou pas l’entrée utilisateur. Arrêt ou la désactivation de ce service entraîne une instabilité du système.
|   **Nom du service**    |   PlugPlay
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="portable-device-enumerator-service"></a>Service d’énumérateur dispositif portable           
| | |           
|---|---|       
|   **Description du service** |   Applique la stratégie de groupe pour les périphériques de stockage de masse amovibles. Permet aux applications telles que le lecteur WindowsMedia et Assistant Importation d’images pour transférer et synchroniser du contenu à l’aide de périphériques de stockage de masse amovibles.
|   **Nom du service**    |   WPDBusEnum
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="power"></a>Mise sous tension            
| | |           
|---|---|       
|   **Description du service** |   Gère la stratégie d’alimentation et de remise de notification de stratégie de l’alimentation.
|   **Nom du service**    |   Mise sous tension
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="print-spooler"></a>Spouleur d’impression            
| | |           
|---|---|   
|   **Description du service** |   Ce service met en attente des travaux d’impression et gère l’interaction avec l’imprimante.  Si vous désactivez ce service, vous ne serez pas en mesure d’imprimer ou de voir vos imprimantes.
|   **Nom du service**    |   Spouleur
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  |   Pour désactiver si pas un serveur d’impression ou un contrôleur de domaine OK
|   **Commentaires**    |   Sur un contrôleur de domaine, l’installation du rôle de contrôleur de domaine ajoute un thread pour le service spouleur est chargé d’exécuter le nettoyage d’impression: supprimer les objets de la file d’attente d’impression obsolètes à partir d’ActiveDirectory.  Si le service spouleur n’est pas en cours d’exécution sur au moins un contrôleur de domaine dans chaque site, la publicité n’a aucun moyen pour supprimer l’anciens files d’attente qui n’existent plus. https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/
|||         
            
<br />          

##  <a name="printer-extensions-and-notifications"></a>Notifications et les Extensions de l’imprimante        
| | |           
|---|---|       
|   **Description du service** |   Ce service s’ouvre les boîtes de dialogue personnalisées et gère les notifications à partir d’un serveur d’impression distant ou une imprimante. Si vous désactivez ce service, vous ne serez pas en mesure de voir les extensions de l’imprimante ou les notifications.
|   **Nom du service**    |   PrintNotify
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK si ce n’est pas un serveur d’impression
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="problem-reports-and-solutions-control-panel-support"></a>Rapports et Solutions contrôle la prise en charge du Panneau de configuration     
| | |           
|---|---|   
|   **Description du service** |   Ce service prend en charge pour l’affichage, l’envoi et la suppression de rapports au niveau du système pour le rapports et Solutions le panneau de configuration.
|   **Nom du service**    |   wercplsupport
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="program-compatibility-assistant-service"></a>Service Assistant Compatibilité des programmes     
| | |           
|---|---|       
|   **Description du service** |   Ce service prend en charge pour la compatibilité Assistant (des programmes).  PCA surveille les programmes installés et exécutés par l’utilisateur et détecte les problèmes de compatibilité connus. Si ce service est arrêté, PCA ne fonctionnera pas correctement.
|   **Nom du service**    |   PcaSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Automatique
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="quality-windows-audio-video-experience"></a>Qualité Windows expérience Audio-vidéo      
| | |           
|---|---|   
|   **Description du service** |   Qualité Windows expérience Audio-vidéo (qWave) est une plateforme de mise en réseau pour AV (Audio Video) diffusion en continu des applications sur des réseaux domestiques IP. qWave améliore les performances et la fiabilité des flux en veillant à ce réseau qualité de service (QoS) pour les applications antivirus AV. Il fournit des mécanismes de contrôle d’admission, exécuter surveillance en temps et l’application, des commentaires de l’application et définition des priorités du trafic.
|   **Nom du service**    |   QWAVE
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Service de qualité de service côté client
|||         
            
<br />          

##      <a name="radio-management-service"></a>Service de gestion de cases d’option        
| | |           
|---|---|   
|   **Description du service** |   Gestion et Service du Mode avion radio
|   **Nom du service**    |   RmSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="remote-access-auto-connection-manager"></a>Gestionnaire de connexion automatique accès à distance            
| | |           
|---|---|   
|   **Description du service** |   Crée une connexion à un réseau distant chaque fois qu’un programme fait référence à un nom DNS ou NetBIOS ou une adresse distante.
|   **Nom du service**    |   RasAuto
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="remote-access-connection-manager"></a>Gestionnaire de connexions d’accès à distance         
| | |           
|---|---|   
|   **Description du service** |   Gère les connexions d’accès à distance réseau privé virtuel (VPN) à partir de cet ordinateur à Internet ou d’autres réseaux distants. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   RasMan
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="remote-desktop-configuration"></a>Configuration du Bureau à distance         
| | |           
|---|---|   
|   **Description du service** |   Service de Configuration de bureau à distance (RDCS) est responsable de tous les Services Bureau à distance et Bureau à distance de configuration associée et les activités de maintenance de session qui nécessitent le contexte du système. Ceux-ci incluent les dossiers temporaires par session, thèmes du Bureau à distance et les certificats de bureau à distance.
|   **Nom du service**    |   SessionEnv
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="remote-desktop-services"></a>Services Bureau à distance          
| | |           
|---|---|   
|   **Description du service** |   Permet aux utilisateurs de se connecter de manière interactive à un ordinateur distant. Bureau à distance et le serveur hôte de Session Bureau à distance dépendent de ce service.  Pour empêcher l’utilisation à distance de cet ordinateur, désactivez les cases à cocher sous l’onglet de l’élément du Panneau de configuration système propriétés du contrôle à distance.
|   **Nom du service**    |   Terminal Server
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="remote-desktop-services-usermode-port-redirector"></a>Redirecteur de Port du mode utilisateur Services Bureau à distance        
| | |           
|---|---|   
|   **Description du service** |   Permet la redirection des imprimantes/lecteurs/Ports pour les connexions RDP
|   **Nom du service**    |   UmRdpService
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Prend en charge les redirections côté serveur de la connexion.
|||         
            
<br />          

## <a name="remote-procedure-call-rpc"></a>Appel de procédure distante (RPC)          
| | |           
|---|---|   
|   **Description du service** |   Le service RPCSS est le Gestionnaire de contrôle de Service pour les serveurs COM et DCOM. Il effectue des requêtes d’activations d’objets, les résolutions exportateur objet et garbage collection distribué pour les serveurs COM et DCOM. Si ce service est arrêté ou désactivé, les programmes à l’aide de COM ou DCOM ne fonctionnera pas correctement. Il est fortement recommandé d’avoir l’exécution du service RPCSS.
|   **Nom du service**    |   RpcSs
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="remote-procedure-call-rpc-locator"></a>Localisateur de procédure distante (RPC) d’appel             
| | |               
|---|---|   
|   **Description du service** |   Dans Windows2003 et versions antérieures de Windows, le service de localisation de l’appel de procédure distante (RPC) gère la base de données du service de nom RPC. Dans WindowsVista et versions ultérieures de Windows, ce service ne fournit pas de toutes les fonctionnalités et n’est présent pour la compatibilité des applications.   |
|   **Nom du service**    |   RpcLocator  |
|   **Installation**    |   Uniquement sur Datacenter Edition  |
|   **StartType**   |   Manuelle  |
|   **Recommandation**  | Pas d’instructions   |
|   **Commentaires**    |       |
|||             
                
<br />              

## <a name="remote-registry"></a>Registre à distance          
| | |           
|---|---|   
|   **Description du service** |   Permet aux utilisateurs distants modifier les paramètres du Registre sur cet ordinateur. Si ce service est arrêté, le Registre peut être modifié uniquement par les utilisateurs sur cet ordinateur. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   RemoteRegistry
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="resultant-set-of-policy-provider"></a>Jeu de fournisseur de stratégie résultant            
| | |           
|---|---|       
|   **Description du service** |   Fournit un service réseau qui traite les demandes pour simuler l’application des paramètres de stratégie de groupe pour un utilisateur ou ordinateur cible dans diverses situations et calcule les paramètres du jeu de stratégie résultant.
|   **Nom du service**    |   RSoPProv
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |Pas d’instructions    
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="routing-and-remote-access"></a>Routage et accès distant            
| | |           
|---|---|   
|   **Description du service** |   Offre des services de routage aux entreprises dans le réseau local et les environnements de réseau étendu.
|   **Nom du service**    |   Accès à distance
|   **Installation**    |   Toujours installé
|   **StartType**   |   Désactivé
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   Déjà désactivé
|||         
            
<br />          

## <a name="rpc-endpoint-mapper"></a>Mappeur de point de terminaison RPC          
| | |           
|---|---|   
|   **Description du service** |   Résout les identificateurs des interfaces RPC pour les points de terminaison transport. Si ce service est arrêté ou désactivé, les programmes à l’aide des services d’appel de procédure distante (RPC) ne fonctionnera pas correctement.
|   **Nom du service**    |   RpcEptMapper
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="secondary-logon"></a>Ouverture de session secondaire     
| | |           
|---|---|       
|   **Description du service** |   Permet le démarrage des processus sous d’autres informations d’identification. Si ce service est arrêté, ce type d’ouverture de session ne seront pas disponible. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   Seclogon
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="secure-socket-tunneling-protocol-service"></a>Secure Socket Tunneling Protocol Service            
| | |               
|---|---|       
|   **Description du service** |   Prend en charge pour le Socket protocole SSTP (Secure Tunneling) pour se connecter à des ordinateurs distants à l’aide de VPN. Si ce service est désactivé, les utilisateurs ne seront pas en mesure d’utiliser SSTP pour accéder aux serveurs distants.    |
|   **Nom du service**    |   SstpSvc |
|   **Installation**    |   Toujours installé    |
|   **StartType**   |   Manuelle  |
|   **Recommandation**  |   Ne désactivez pas  |
|   **Commentaires**    |   La désactivation de sauts de RRAS   |
|||             
                
<br />              

## <a name="security-accounts-manager"></a>Gestionnaire des comptes de sécurité            
| | |           
|---|---|       
|   **Description du service** |   Le démarrage de ce service signale des autres services que le Gestionnaire de comptes de sécurité (SAM) est prêt à accepter les demandes.  La désactivation de ce service empêche les autres services dans le système d’être averti lorsque le SAM est prêt, qui à son tour risque de ces services pour ne pas démarrer correctement. Ce service ne doit pas être désactivé.
|   **Nom du service**    |   SamSs
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Ne désactivez pas
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="sensor-data-service"></a>Service de données de capteur  
| | |           
|---|---|   
|   **Description du service** |   Fournit des données à partir d’une variété de capteurs
|   **Nom du service**    |   SensorDataService
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />  

## <a name="sensor-monitoring-service"></a>Service de surveillance de capteur            
| | |           
|---|---|       
|   **Description du service** |   Analyse des capteurs différents pour exposer les données et s’adapter à l’état du système et utilisateur.  Si ce service est arrêté ou désactivé, la luminosité de l’affichage s’adapte pas aux conditions d’éclairage. Arrêt du service peut affecter les autres fonctionnalités du système et des fonctionnalités également.
|   **Nom du service**    |   SensrSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          
        
## <a name="sensor-service"></a>Service de capteur           
| | |           
|---|---|       
|   **Description du service** |   Un service pour les capteurs qui gère les fonctionnalités des capteurs différents. Gère l’Orientation de périphérique Simple (SDO) et l’historique pour les capteurs. Charge le capteur SDO qui signale les modifications de l’orientation de l’appareil.  Si ce service est arrêté ou désactivé, le capteur SDO n’est pas chargé et par conséquent, la rotation automatique ne se produit. Collection de l’historique des capteurs est également arrêtée.
|   **Nom du service**    |   SensorService
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="server"></a>Serveur           
| | |           
|---|---|   
|   **Description du service** |   Prend en charge des fichiers, d’impression et des canaux nommés partage sur le réseau de cet ordinateur. Si ce service est arrêté, ces fonctions ne seront pas disponibles. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   LanmanServer
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Nécessaires pour la gestion à distance, IPC$, le partage de fichiers SMB
|||         
            
<br />          

## <a name="shell-hardware-detection"></a>Détection matériel noyau             
| | |           
|---|---|       
|   **Description du service** |   Fournit des notifications pour les événements matériel de lecture automatique.
|   **Nom du service**    |   ShellHWDetection
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Automatique
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="smart-card"></a>Carte à puce           
| | |           
|---|---|   
|   **Description du service** |   Gère l’accès aux cartes à puce lues par cet ordinateur. Si ce service est arrêté, cet ordinateur sera impossible de lire les cartes à puce. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   SCardSvr
|   **Installation**    |   Toujours installé
|   **StartType**   |   Désactivé
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="smart-card-device-enumeration-service"></a>Service d’énumération de périphériques de carte à puce                    
| | |               
|---|---|       
|   **Description du service** |   Crée des nœuds de périphérique de logiciels pour tous les lecteurs de carte à puce accessibles à une session donnée. Si ce service est désactivé, APIs WinRT ne sera pas en mesure d’énumérer les lecteurs de carte à puce.   |
|   **Nom du service**    |   ScDeviceEnum    |
|   **Installation**    |   Toujours installé    |
|   **StartType**   |   Manuelle  |
|   **Recommandation**  |   Pour désactiver OK   |
|   **Commentaires**    |   Nécessaire presque exclusivement pour les applications WinRT    |
|||             
                
<br />              

## <a name="smart-card-removal-policy"></a>Stratégie de suppression de carte à puce        
| | |           
|---|---|       
|   **Description du service** |   Permet au système d’être configuré pour verrouiller le bureau de l’utilisateur lors de la suppression de la carte à puce.
|   **Nom du service**    |   SCPolicySvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="snmp-trap"></a>Interruption SNMP            
| | |           
|---|---|       
|   **Description du service** |   Reçoit des messages d’interruption générés par les agents SNMP Simple Network Management Protocol () locaux ou distants et transfère les messages aux programmes de gestion SNMP en cours d’exécution sur cet ordinateur. Si ce service est arrêté, programmes SNMP sur cet ordinateur ne recevra pas de messages d’interruption SNMP. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   SNMPTRAP
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="software-protection"></a>Protection logicielle             
| | |           
|---|---|       
|   **Description du service** |   Permet de téléchargement, installation et mise en œuvre de licences numériques pour Windows et Windows applications. Si le service est désactivé, le système d’exploitation et les applications sous licence peuvent s’exécuter en mode de notification. Il est fortement recommandé que vous ne désactivez pas le service de Protection logicielle.
|   **Nom du service**    |   sppsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="special-administration-console-helper"></a>Application d’assistance de la Console Administration spéciale        
| | |           
|---|---|   
|   **Description du service** |   Permet aux administrateurs d’accéder à distance à une invite de commandes à l’aide des Services de gestion d’urgence.
|   **Nom du service**    |   Sacsvr
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="spot-verifier"></a>Vérification rapide            
| | |           
|---|---|   
|   **Description du service** |   Vérifie les altérations de système de fichiers potentiels.
|   **Nom du service**    |   svsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="ssdp-discovery"></a>Découverte SSDP           
| | |           
|---|---|   
|   **Description du service** |   Détecte les périphériques réseau et les services qui utilisent le protocole de découverte SSDP, comme des appareils UPnP. Annonce également les périphériques SSDP et les services exécutés sur l’ordinateur local. Si ce service est arrêté, les périphériques SSDP ne seront pas détectés. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   SSDPSRV
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="state-repository-service"></a>Service de l’espace de stockage de l’état         
| | |           
|---|---|   
|   **Description du service** |   Fournit la prise en charge de l’infrastructure requise pour le modèle d’application.
|   **Nom du service**    |   StateRepository
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="still-image-acquisition-events"></a>Événements d’Acquisition d’images fixes
| | |           
|---|---|   
|   **Description du service** |   Lance les applications associées à toujours les événements d’acquisition image.
|   **Nom du service**    |   WiaRpc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />  

## <a name="storage-service"></a>Service de stockage          
| | |           
|---|---|       
|   **Description du service** |   Fournit des services d’activation pour les paramètres de stockage et l’extension de stockage externe
|   **Nom du service**    |   StorSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="storage-tiers-management"></a>Gestion des niveaux de stockage        
| | |           
|---|---|   
|   **Description du service** |   Permet d’optimiser le placement des données dans des niveaux de stockage sur tous les espaces de stockage hiérarchisé dans le système.
|   **Nom du service**    |   TieringEngineService
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="superfetch"></a>SuperFetch          
| | |           
|---|---|       
|   **Description du service** |   Gère et améliore les performances du système au fil du temps.
|   **Nom du service**    |   SysMain
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="sync-host"></a>Hôte de synchronisation            
| | |           
|---|---|       
|   **Description du service** |   Ce service synchronise le courrier, contacts, calendrier et diverses autres données utilisateur. Messagerie et autres applications dépendantes de cette fonctionnalité ne fonctionnera pas correctement lorsque ce service n’est pas en cours d’exécution.
|   **Nom du service**    |   OneSyncSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Automatique
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Modèle de service utilisateur
|||         
            
<br />          

## <a name="system-event-notification-service"></a>Service de Notification d’événements système            
| | |           
|---|---|       
|   **Description du service** |   Analyse des événements système et informe les abonnés aux événements système de ces événements de COM +.
|   **Nom du service**    |   SENS
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="system-events-broker"></a>Service Broker pour les événements système             
| | |           
|---|---|       
|   **Description du service** |   Coordonnées de l’exécution du travail en arrière-plan de l’application WinRT. Si ce service est arrêté ou désactivé, tâches en arrière-plan ne peut pas être déclenchée.
|   **Nom du service**    |   SystemEventsBroker
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Bien que sa description implique qu’il est uniquement pour les applications WinRT, il est nécessaire pour le Planificateur de tâches, service Broker pour les infrastructures et autres composants internes.
|||         
            
<br />          

## <a name="task-scheduler"></a>Planificateur de tâches           
| | |           
|---|---|   
|   **Description du service** |   Permet à un utilisateur configurer et planifier des tâches automatisées sur cet ordinateur. Le service héberge également plusieurs tâches critiques pour le système Windows. Si ce service est arrêté ou désactivé, ces tâches ne seront pas exécutées à leurs heures. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   Calendrier
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="tcpip-netbios-helper"></a>Assistance TCP/IP NetBIOS            
| | |           
|---|---|   
|   **Description du service** |   Prend en charge NetBIOS sur le service TCP/IP (NetBT) et la résolution de noms NetBIOS pour les clients sur le réseau, ce qui permet aux utilisateurs de partager des fichiers, imprimer et ouvrez une session sur le réseau. Si ce service est arrêté, ces fonctions ne soient pas disponibles. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   LMHOSTS
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="telephony"></a>Téléphonie           
| | |           
|---|---|       
|   **Description du service** |   Fournit la prise en charge des API de téléphonie (TAPI) pour les programmes qui contrôlent les périphériques de téléphonie sur l’ordinateur local et, par le biais du réseau local, sur les serveurs qui exécutent également le service.
|   **Nom du service**    |   TapiSrv
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   La désactivation de sauts de RRAS
|||         
            
<br />          

## <a name="themes"></a>Thèmes           
| | |           
|---|---|
|   **Description du service** |   Fournit la gestion de thème l’expérience utilisateur.
|   **Nom du service**    |   Thèmes
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Automatique
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Ne peut pas définir des thèmes d’accessibilité lorsque ce service est désactivé.
|||         
            
<br />  

## <a name="tile-data-model-server"></a>Serveur de modèle de données de vignette           
| | |           
|---|---|   
|   **Description du service** |   Vignette serveur pour les mises à jour de vignette.
|   **Nom du service**    |   tiledatamodelsvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Automatique
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Démarrer les sauts de menu si ce service est désactivé.
|||         
            
<br />          

##  <a name="time-broker"></a>Service Broker pour les temps     
| | |           
|---|---|       
|   **Description du service** |   Coordonnées de l’exécution du travail en arrière-plan de l’application WinRT. Si ce service est arrêté ou désactivé, tâches en arrière-plan ne peut pas être déclenchée.
|   **Nom du service**    |   TimeBrokerSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Bien que sa description implique qu’il est uniquement pour les applications WinRT, il est nécessaire pour le Planificateur de tâches, service Broker pour les infrastructures et autres composants internes.
|||         
            
<br />          

## <a name="touch-keyboard-and-handwriting-panel-service"></a>Clavier tactile et le Service du Panneau de configuration de l’écriture manuscrite         
| | |           
|---|---|   
|   **Description du service** |   Active la fonctionnalité de stylet et d’entrée manuscrite du clavier tactile et d’écriture manuscrite
|   **Nom du service**    |   TabletInputService
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="update-orchestrator-service-for-windows-update"></a>Mise à jour orchestrateur du Service de mise à jour de Windows           
| | |           
|---|---|       
|   **Description du service** |   Gère les mises à jour Windows. En cas d’arrêt, il se peut que vos appareils ne sera pas en mesure de télécharger et installer les dernières mises à jour.
|   **Nom du service**    |   UsoSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Description du service était manquante dans v1607; dépend de la mise à jour de Windows (y compris WSUS) sur ce service.
|||         
            
<br />          

## <a name="upnp-device-host"></a>Hôte de périphérique UPnP         
| | |           
|---|---|   
|   **Description du service** |   Permet de périphériques UPnP doit être hébergée sur cet ordinateur. Si ce service est arrêté, tous les appareils UPnP hébergés cesse de fonctionner et aucun périphérique hébergé supplémentaires ne peut être ajoutés. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   trafic
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="user-access-logging-service"></a>Service de journalisation des accès utilisateur          
| | |           
|---|---|   
|   **Description du service** |   Ce service enregistre les demandes d’accès client uniques, sous la forme d’adresses IP et noms d’utilisateurs, des rôles sur le serveur local et produits installés. Ces informations peuvent être interrogées via Powershell, par des administrateurs d’avoir à quantifier à la demande du client de logiciel de serveur pour la gestion des licences d’accès Client (CAL) en mode hors connexion. Si le service est désactivé, les demandes client ne seront pas enregistrés et ne seront pas récupérables par le biais des requêtes Powershell. Arrêt du service n’affecte pas la requête de données d’historique (voir la documentation pour connaître les étapes supprimer les données d’historique de prise en charge). L’administrateur système local doit consulter le contrat de licence Windows Server, de son, pour déterminer le nombre de licences d’accès client qui sont requis pour le logiciel serveur sous licence; utiliser le service de journalisation des accès utilisateur et les données ne modifient pas cette obligation.
|   **Nom du service**    |   UALSVC
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="user-data-access"></a>Accès des données utilisateur        
| | |           
|---|---|   
|   **Description du service** |   Fournit des applications d’accéder aux données utilisateur structuré, y compris les informations de contact, calendriers, messages et tout autre contenu. Si vous arrêtez ou désactivez ce service, les applications qui utilisent ces données peut ne pas fonctionnent correctement.
|   **Nom du service**    |   UserDataSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Modèle de service utilisateur
|||         
            
<br />          

## <a name="user-data-storage"></a>Stockage des données utilisateur            
| | |           
|---|---|       
|   **Description du service** |   Gère le stockage de données structurées utilisateur, y compris les informations de contact, calendriers, messages et tout autre contenu. Si vous arrêtez ou désactivez ce service, les applications qui utilisent ces données peut ne pas fonctionnent correctement.
|   **Nom du service**    |   UnistoreSvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Modèle de service utilisateur
|||         
            
<br />          

## <a name="user-experience-virtualization-service"></a>Service de virtualisation d’expérience utilisateur           
| | |           
|---|---|       
|   **Description du service** |   Prend en charge les applications et l’itinérance des paramètres du système d’exploitation
|   **Nom du service**    |   UevAgentService
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Désactivé
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="user-manager"></a>Gestionnaire des utilisateurs        
| | |           
|---|---|   
|   **Description du service** |   Gestionnaire des utilisateurs fournit les composants d’exécution requis pour l’interaction entre plusieurs utilisateurs.  Si ce service est arrêté, certaines applications ne peuvent pas fonctionner correctement.
|   **Nom du service**    |   UserManager
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="user-profile-service"></a>Service de profil utilisateur         
| | |           
|---|---|   
|   **Description du service** |   Ce service est responsable du chargement et de déchargement de profils utilisateur. Si ce service est arrêté ou désactivé, les utilisateurs ne seront plus en mesure de se connecter avec succès ou se déconnecter, les applications peuvent avoir des problèmes pour obtenir les données des utilisateurs et ne recevront pas les composants inscrits pour recevoir des notifications d’événements de profil.
|   **Nom du service**    |   ProfSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="virtual-disk"></a>Disque virtuel             
| | |           
|---|---|   
|   **Description du service** |   Fournit des services de gestion des disques, des volumes, des systèmes de fichiers et des baies de stockage.
|   **Nom du service**    |   VDS
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pas d’instructions
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="volume-shadow-copy"></a>Clichés instantanés de volume           
| | |           
|---|---|   
|   **Description du service** |   Gère et implémente les clichés instantanés de Volume utilisé pour la sauvegarde et d’autres fins. Si ce service est arrêté, les clichés instantanés ne seront pas disponibles pour la sauvegarde et la sauvegarde peut échouer. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   VSS
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pas d’instructions
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="walletservice"></a>WalletService           
| | |           
|---|---|   
|   **Description du service** |   Objets hôtes utilisés par les clients de wallet
|   **Nom du service**    |   WalletService
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-audio"></a>Audio Windows            
| | |           
|---|---|       
|   **Description du service** |   Gère les périphériques audio pour les programmes basés sur Windows.  Si ce service est arrêté, des effets et des périphériques audio fonctionnera pas correctement.  Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer
|   **Nom du service**    |   AudioSrv à
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-audio-endpoint-builder"></a>Générateur de point de terminaison Audio Windows           
| | |           
|---|---|
|   **Description du service** |   Gère les périphériques audio pour le service Windows Audio.  Si ce service est arrêté, des effets et des périphériques audio fonctionnera pas correctement.  Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer
|   **Nom du service**    |   AudioEndpointBuilder
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-biometric-service"></a>Service de biométrie Windows            
| | |           
|---|---|   
|   **Description du service** |   Le service de biométrie Windows permet aux applications clientes pour capturer, de comparer, de manipuler et de stocker des données biométriques sans avoir directement accès à n’importe quel matériel biométrique ou des échantillons de. Le service est hébergé dans un processus SVCHOST privilégié.
|   **Nom du service**    |   WbioSrvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="windows-camera-frame-server"></a>Image de caméra de Windows Server         
| | |           
|---|---|       
|   **Description du service** |   Permet à plusieurs clients à accéder aux trames vidéo à partir d’appareils photo.
|   **Nom du service**    |   FrameServer
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-connection-manager"></a>Gestionnaire de connexions Windows           
| | |           
|---|---|   
|   **Description du service** |   Décisions de connexion/déconnexion automatique rend basée sur les options de connectivité réseau actuellement disponibles pour le PC et permet la gestion de la connectivité réseau en fonction des paramètres de stratégie de groupe.
|   **Nom du service**    |   Wcmsvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-defender-network-inspection-service"></a>Service d’Inspection réseau Windows Defender          
| | |           
|---|---|       
|   **Description du service** |   Vous aide à protéger contre les tentatives d’intrusion ciblant des vulnérabilités connues et récemment découvertes dans les protocoles de réseau
|   **Nom du service**    |   WdNisSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions    
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-defender-service"></a>Service Windows Defender         
| | |           
|---|---|       
|   **Description du service** |   Vous aide à protéger les utilisateurs contre les programmes malveillants et autres logiciels potentiellement indésirables
|   **Nom du service**    |   WinDefend
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-driver-foundation---user-mode-driver-framework"></a>WindowsDriverFoundation - pilote en mode utilisateur Framework           
| | |           
|---|---|   
|   **Description du service** |   Crée et gère le processus de pilote en mode utilisateur. Ce service ne peut pas être arrêté.
|   **Nom du service**    |   wudfsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-encryption-provider-host-service"></a>Service hôte de fournisseur de chiffrement Windows     
| | |           
|---|---|   
|   **Description du service** |   Service hôte de fournisseur de chiffrement Windows courtiers chiffrement liés les fonctionnalités à partir de fournisseurs de chiffrement tiers aux processus que vous avez besoin pour évaluer et appliquer des stratégies EAS. Un arrêt compromettre les vérifications de la conformité EAS qui ont été établies par les comptes de messagerie connecté
|   **Nom du service**    |   WEPHOSTSVC
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-error-reporting-service"></a>Service de rapport d’erreurs Windows          
| | |           
|---|---|       
|   **Description du service** |   Permet d’erreurs lorsque les programmes cesser de fonctionner ou de répondre et les solutions existantes pour être remis. Permet également de journaux soit générée pour le diagnostic et la réparation. Si ce service est arrêté, le rapport d’erreurs peut ne pas fonctionne correctement et résultats de services de diagnostic et de réparation ne peuvent pas s’afficher.
|   **Nom du service**    |   WerSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Collecte et envoie les données de blocage/utilisées par Microsoft et tiers éditeurs de logiciels indépendants/fabricants de matériel. Les données sont utilisées pour diagnostiquer les bogues induire d’incident, ce qui peuvent inclure des bogues de sécurité. Également nécessaire pour le rapport d’erreurs d’entreprise
|||         
            
<br />          

## <a name="windows-event-collector"></a>Collecteur d’événements Windows          
| | |           
|---|---|   
|   **Description du service** |   Ce service gère des abonnements persistants à des événements à partir de sources distantes qui prennent en charge le protocole WS-Management. Cela inclut les journaux des événements WindowsVista, matériel et les sources d’événement compatible IPMI. Le service stocke les événements transférés dans un journal des événements local. Si ce service est arrêté ou désactivé les abonnements aux événements ne peuvent pas être créés et événements transférés ne peut pas être acceptés.
|   **Nom du service**    |   Wecsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Collecte les événements ETW (y compris les événements de sécurité) pour la gestion, les tests de diagnostic.  Un grand nombre de fonctionnalités et outils tiers s’appuient sur celui-ci, y compris les outils d’audit de sécurité
|||         
            
<br />          

## <a name="windows-event-log"></a>Journal des événements Windows            
| | |           
|---|---|       
|   **Description du service** |   Ce service gère les événements et des journaux des événements. Il prend en charge les événements de journalisation, requêtes sur les événements, l’abonnement aux événements, l’archivage des journaux des événements et la gestion des métadonnées de l’événement. Il peut afficher les événements au format XML et en texte brut. Arrêt du service peut compromettre la sécurité et la fiabilité du système.
|   **Nom du service**    |   Journal des événements
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-firewall"></a>Pare-feu Windows         
| | |           
|---|---|   
|   **Description du service** |   Le pare-feu Windows vous aide à protéger votre ordinateur en empêchant les utilisateurs non autorisés d’accéder à votre ordinateur via Internet ou à un réseau.
|   **Nom du service**    |   MpsSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="windows-font-cache-service"></a>Service Windows police du Cache      
| | |           
|---|---|   
|   **Description du service** |   Optimise les performances des applications en mettant en cache les données de police fréquemment utilisées. Applications démarrent ce service s’il n’est pas déjà en cours d’exécution. Il peut être désactivée, même si cette opération sera la dégradation des performances de l’application.
|   **Nom du service**    |   Cache de police
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-image-acquisition-wia"></a>D’acquisition d’images Windows (WIA)          
| | |           
|---|---|   
|   **Description du service** |   Fournit des services d’acquisition d’images pour les scanneurs et appareils photo
|   **Nom du service**    |   stisvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="windows-insider-service"></a>Service Windows Insider     
| | |           
|---|---|   
|   **Description du service** |   wisvc
|   **Nom du service**    |   wisvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Serveur ne gère pas de version d’évaluation, il s’agit d’un no-op sur le serveur. Fonctionnalité peut également être désactivée via la stratégie de groupe.
|||         
            
<br />          

##  <a name="windows-installer"></a>Windows Installer       
| | |           
|---|---|
|   **Description du service** |   Ajoute, modifie et supprime les applications fournies en tant qu’un package Windows Installer (*.msi, *.msp). Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   MSIServer
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-license-manager-service"></a>Service Gestionnaire de licences de Windows          
| | |           
|---|---|   
|   **Description du service** |   Fournit la prise en charge de l’infrastructure pour le MicrosoftStore.  Ce service est démarré à la demande et si désactivée puis contenu acquis via le MicrosoftStore ne fonctionne pas correctement.
|   **Nom du service**    |   LicenseManager
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-management-instrumentation"></a>WindowsManagementInstrumentation       
| | |           
|---|---|       
|   **Description du service** |   Fournit un modèle commun interface et un objet pour accéder aux informations de gestion sur le système d’exploitation, périphériques, applications et services. Si ce service est arrêté, la plupart des logiciels Windows ne fonctionnera pas correctement. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   WinMgmt
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="windows-mobile-hotspot-service"></a>Service de point d’accès Mobile Windows          
| | |           
|---|---|       
|   **Description du service** |   Offre la possibilité de partager une connexion de données cellulaires avec un autre appareil.
|   **Nom du service**    |   icssvc
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-modules-installer"></a>Programme d’installation de Modules Windows        
| | |           
|---|---|   
|   **Description du service** |   Permet l’installation, la modification et suppression des mises à jour Windows et des composants facultatifs. Si ce service est désactivé, installer ou désinstaller des mises à jour peuvent échouer pour cet ordinateur de Windows.
|   **Nom du service**    |   TrustedInstaller
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-push-notifications-system-service"></a>Service de système de Notifications Push de Windows            
| | |           
|---|---|
|   **Description du service** |   Ce service s’exécute dans la session 0 et héberge le fournisseur de plate-forme et de connexion de notification qui gère la connexion entre le périphérique et le serveur WNS.
|   **Nom du service**    |   WpnService
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Automatique
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Requis pour les vignettes dynamiques et d’autres fonctionnalités
|||         
            
<br />      

## <a name="windows-push-notifications-user-service"></a>WindowsPushNotifications utilisateur Service          
| | |           
|---|---|   
|   **Description du service** |   Ce service héberge plateforme de notification Windows qui prend en charge local et des notifications push. Prise en charge les notifications sont vignette, toast et raw.
|   **Nom du service**    |   WpnUserService
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Pour désactiver OK
|   **Commentaires**    |   Modèle de service utilisateur
|||         
            
<br />          
    
## <a name="windows-remote-management-ws-management"></a>WindowsRemoteManagement (WS-Management)            
| | |           
|---|---|   
|   **Description du service** |   Service WindowsRemoteManagement (WinRM) implémente le protocole WS-Management pour la gestion à distance. WS-Management est un protocole de services web standard utilisé pour la gestion à distance des logiciels et matériels. Le service WinRM est à l’écoute sur le réseau pour les demandes WS-Management et les traite. Le WinRM Service doit être configuré avec un écouteur à l’aide d’outil de ligne de commande winrm.cmd ou via la stratégie de groupe dans l’ordre pour qu’il écoute sur le réseau. Le service WinRM fournit l’accès aux données WMI et permet la collecte d’événements. Collecte d’événements et d’abonnement aux événements nécessitent que le service est en cours d’exécution. Messages de WinRM utilisent HTTP et HTTPS en tant que transports. Le service WinRM ne dépend pas d’IIS, mais est préconfiguré pour partager un port avec IIS sur le même ordinateur.  Les réserves de ressources du service WinRM le /wsman préfixe d’URL. Pour éviter tout conflit avec IIS, les administrateurs doivent s’assurer que les sites Web hébergés sur IIS n’utilisent pas le /wsman préfixe d’URL.
|   **Nom du service**    |   WinRM
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Nécessaires pour la gestion à distance
|||         
            
<br />          

##  <a name="windows-search"></a>Windows Search      
| | |           
|---|---|       
|   **Description du service** |   Fournit l’indexation du contenu, la mise en cache de propriété et résultats de recherche pour tout autre contenu, des fichiers et des messages électroniques.
|   **Nom du service**    |   WSearch
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Désactivé
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         
            
<br />          

##  <a name="windows-time"></a>Temps Windows        
| | |           
|---|---|   
|   **Description du service** |   Assure la synchronisation de date et l’heure sur tous les clients et serveurs du réseau. Si ce service est arrêté, date et heure de synchronisation ne sera pas disponible. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   W32Time
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="windows-update"></a>Mise à jour de Windows           
| | |           
|---|---|       
|   **Description du service** |   Permet la détection, le téléchargement et l’installation des mises à jour pour Windows et d’autres programmes. Si ce service est désactivé, les utilisateurs de cet ordinateur ne sera pas en mesure d’utiliser Windows Update ou sa fonctionnalité de mise à jour automatique et programmes ne seront pas en mesure d’utiliser l’API de l’Agent Windows Update (WUA).
|   **Nom du service**    |   wuauserv
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="winhttp-web-proxy-auto-discovery-service"></a>Service de découverte automatique de Proxy WinHTTP Web         
| | |           
|---|---|   
|   **Description du service** |   WinHTTP implémente la pile HTTP du client et fournit aux développeurs une API Win32 et le composant Automation COM pour l’envoi des demandes HTTP et la réception de réponses. En outre, WinHTTP prend en charge de la découverte automatique d’une configuration de proxy via son implémentation du protocole Web Proxy Auto-Discovery (WPAD).
|   **Nom du service**    |   WinHttpAutoProxySvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Commentaires**    |   Tout ce qui utilise la pile réseau peut avoir une dépendance fonctionnelle sur ce service. De nombreuses entreprises s’appuient sur cette option pour configurer le routage du proxy HTTP des leurs réseaux internes.  Sans cela, en interne provenant des connexions HTTP à Internet tous échouera.
|||         
            
<br />          

## <a name="wired-autoconfig"></a>Configuration automatique de réseau câblé         
| | |           
|---|---|       
|   **Description du service** |   Le service de configuration automatique WLAN (DOT3SVC) câblé est chargé d’exécuter IEEE 802. 1 X sur des interfaces Ethernet l’authentification. Si votre déploiement de réseau câblé actuel applique l’authentification de 802. 1 X, le service DOT3SVC doit être configuré pour exécuter pour établir une connectivité de couche 2 et/ou permettant l’accès aux ressources réseau. Réseaux câblés qui n’appliquent pas d’authentification de 802. 1 X ne sont pas affectés par le service DOT3SVC.
|   **Nom du service**    |   DOT3SVC
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions   
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="wmi-performance-adapter"></a>Carte de performances WMI          
| | |           
|---|---|   
|   **Description du service** |   Fournit des informations de bibliothèque de performances à partir des fournisseurs WindowsManagementInstrumentation (WMI) pour les clients du réseau. Ce service s’exécute uniquement lorsque l’application d’assistance des données de performances est activée.
|   **Nom du service**    |   wmiApSrv
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Pas d’instructions       
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="workstation"></a>Station de travail          
| | |           
|---|---|   
|   **Description du service** |   Crée et gère les connexions réseau client aux serveurs distants à l’aide du protocole SMB. Si ce service est arrêté, ces connexions ne seront pas disponibles. Si ce service est désactivé, tous les services qui en dépendent explicitement échouera à démarrer.
|   **Nom du service**    |   LanmanWorkstation
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Pas d’instructions       
|   **Commentaires**    |   
|||         
            
<br />

## <a name="xbox-live-auth-manager"></a>Xbox Live Auth Manager           
| | |           
|---|---|   
|   **Description du service** |   Fournit des services d’authentification et d’autorisation pour l’interaction avec Xbox Live. Si ce service est arrêté, certaines applications ne peuvent pas fonctionner correctement.
|   **Nom du service**    |   XblAuthManager
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Doit être désactivé
|   **Commentaires**    |   
|||         
            
<br />          

## <a name="xbox-live-game-save"></a>Xbox Live enregistrer de jeu          
| | |           
|---|---|   
|   **Description du service** |   Les synchronisations ce service enregistrement les données de Xbox Live activé des jeux de sauvegarde.  Si ce service est arrêté, jeu de données de sauvegarde ne sera pas télécharger vers ou télécharger à partir de Xbox Live.
|   **Nom du service**    |   XblGameSave
|   **Installation**    |   Uniquement sur Datacenter Edition
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Doit être désactivé
|   **Commentaires**    |   Les synchronisations ce service enregistrement les données de Xbox Live activé des jeux de sauvegarde.  Si ce service est arrêté, jeu de données de sauvegarde ne sera pas télécharger vers ou télécharger à partir de Xbox Live.
|||         
                
<br /> 
<br /> 

