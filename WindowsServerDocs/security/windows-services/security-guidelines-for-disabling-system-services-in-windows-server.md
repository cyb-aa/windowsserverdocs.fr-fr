---
title: Instructions de sécurité pour les services système dans Windows Server 2016
description: Recommandations en matière de sécurité pour la désactivation des services dans Windows Server 2016 avec expérience utilisateur
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/26/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: 1a9496a121fc45df0b788ea56d50db922fd24536
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749452"
---
## <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>Obtenir des conseils sur la désactivation des services système sur Windows Server 2016 avec expérience utilisateur

S’applique à : Windows Server 2016

Le système d’exploitation Windows inclut de nombreux services système qui fournissent des fonctionnalités importantes. Différents services ont des stratégies de démarrage par défaut différente : certaines sont démarrés par défaut (automatique), certains cas de besoin (manuels), et certaines sont désactivées par défaut et doivent être explicitement activés avant de pouvoir exécuter. Ces valeurs par défaut ont été choisies avec soin pour chaque service afin d’équilibrer les performances, fonctionnalités et la sécurité pour les clients standard.

Toutefois, certains clients d’entreprise peuvent préférer un équilibre plus axées de sécurité pour leurs PC Windows et des serveurs, ce qui réduit leur attaque surface au strict minimum et par conséquent souhaiterez peut-être désactiver complètement tous les services qui ne sont pas nécessaires dans leurs propres environnements. Pour les clients, Microsoft® fournit les instructions qui accompagne cet article sur lesquels les services peuvent être désactivés en toute sécurité à cet effet.

Les instructions sont uniquement pour Windows Server 2016 avec expérience utilisateur (sauf s’il est utilisé comme un remplacement de bureau pour les utilisateurs finaux). À compter de Windows Server 2019, ces instructions sont configurées par défaut. Chaque service sur le système est classé comme suit :

-   **Devez désactiver :** Une entreprise axée sur la sécurité préfèreront très probablement désactiver ce service et de renoncer à ses fonctionnalités (voir ci-dessous).
- **OK pour désactiver :** Ce service fournit des fonctionnalités qui sont utiles pour certains, mais pas toutes les entreprises et les entreprises axée sur la sécurité qui n’utilisent pas peuvent le désactiver en toute sécurité.
- **Ne désactivez pas :** La désactivation de ce service pour avoir un impact sur les fonctionnalités nécessaires ou empêcher les rôles spécifiques ou des fonctionnalités de fonctionner correctement. Il ne doit donc pas être désactivé.
-  **(Aucun Conseil :)** L’impact de la désactivation de ces services n’a pas été entièrement évaluée. Par conséquent, la configuration par défaut de ces services ne doit pas être modifiée.


Les clients peuvent configurer leurs serveurs et les PC Windows pour désactiver les services sélectionnés à l’aide de modèles de sécurité dans leurs stratégies de groupe ou à l’aide de PowerShell automation. Dans certains cas, le guide comprend des paramètres de stratégie de groupe spécifiques qui désactivent des fonctionnalités du service directement, comme alternative à la désactivation du service lui-même.

Microsoft recommande aux clients de désactiver les services suivants et leurs tâches respectives planifiées sur Windows Server 2016 avec expérience utilisateur :

Services : 
1. Xbox Live Auth Manager
2. Xbox Live Game enregistrer

Tâches planifiées : 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(Vous pouvez également accéder aux informations sur tous les services sont décrites dans cet article en affichant la feuille de calcul Microsoft Excel attaché : [Obtenir des conseils sur la désactivation des Services système sur Windows Server 2016 avec expérience utilisateur](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx))

<br />

### <a name="disabling-services-not-installed-by-default"></a>La désactivation de services non installés par défaut

Microsoft recommande par rapport à l’application de stratégies pour désactiver les services qui ne sont pas installés par défaut.
-  Le service est généralement nécessaire si la fonctionnalité est installée. L’installation du service ou la fonctionnalité requiert des droits d’administration. Interdire l’installation des composants, pas le démarrage du service.
-  Bloque le service Microsoft Windows ne s’arrête pas un administrateur (ou non-admin dans certains cas) à partir de l’installation d’un équivalent de tiers similaire, par exemple, un avec un risque de sécurité plus élevé.
-  Une ligne de base ou d’un test d’évaluation qui désactive un service de Windows non définis par défaut (par exemple, W3SVC) donnera des auditeurs l’impression erronée que la technologie (par exemple, IIS) est sécurisée par nature et ne doit jamais être utilisée.
-  Si la fonctionnalité (et le service) ne sont jamais installés, il ajoute simplement inutiles en bloc pour la ligne de base et travail de vérification.

<br />
Pour tous les services système répertoriées dans ce document, les deux tables qui suivent offrent une explication des colonnes et des recommandations de Microsoft pour l’activation et désactivation des services système dans Windows Server 2016 avec expérience utilisateur : 

<br />

### <a name="explanation-of-columns"></a>Explication des colonnes

| | |
|---|---|
|**Description du service**|   La description du service, à partir de sc.exe qdescription.|
|**Nom** |Nom de la clé (interne) du service|
|**Installation** |Toujours installé : Service se trouve sur Server Core et Server avec la fonctionnalité expérience utilisateur  <br /> Uniquement avec l’expérience utilisateur : Service est sur Windows Server 2016 avec expérience utilisateur, mais ***pas*** sur Server Core |
|**StartType**  |Type de démarrage de service sur Windows Server 2016|
|**Recommandation** |Recommandation Microsoft/conseils sur la désactivation de ce service sur Windows Server 2016 dans un déploiement d’entreprise standard et bien gérée et où le serveur n’est pas en cours utilisé comme un remplacement de bureau de l’utilisateur final.|
|**Comments** |Explication supplémentaire|

<br />

### <a name="explanation-of-microsoft-recommendations"></a>Explication des recommandations de Microsoft

| | |
|---|---|
|**Ne désactivez pas** |Ce service ne doit pas être désactivé.|
|**OK pour désactiver**| Ce service peut être désactivé si la fonctionnalité, qu'il prend en charge n’est pas utilisée.|
|**Déjà désactivé**|  Ce service est désactivé par défaut ; pas nécessaire d’appliquer la stratégie|
|**Doit être désactivé** |Ce service ne doit jamais être activé sur un système d’entreprise bien gérée.|

<br />

Les tableaux suivants offrent des conseils de Microsoft sur la désactivation de services système sur Windows Server 2016 avec expérience utilisateur :

<br />

##  <a name="activex-installer-axinstsv"></a>Programme d’installation ActiveX (AxInstSV)

| | |
|---|---|
|   **Description du service** |   Fournit la validation de contrôle de compte d’utilisateur pour l’installation des contrôles ActiveX à partir d’Internet et permet la gestion d’installation des contrôles ActiveX en fonction des paramètres de stratégie de groupe. Ce service est démarré à la demande et si désactivé l’installation des contrôles ActiveX se comporte conformément aux paramètres de navigateur par défaut.    |
|   **Nom du service**    |   AxInstSV    |
|   **Installation**    |   Uniquement avec l’expérience utilisateur    |
|   **StartType**   |   Manuelle  |
|   **Recommandation**  |   OK pour désactiver   |
|   **Comments**    |   OK pour désactiver la fonctionnalité pas besoin |


<br />

## <a name="alljoyn-router-service"></a>Service de routeur de AllJoyn   

| | |
|---|---|
|   **Description du service** |   Achemine les messages de AllJoyn pour les clients AllJoyn locales. Si ce service est arrêté, les clients AllJoyn qui n’ont pas leurs propres routeurs groupés sera impossible d’exécuter. |
|   **Nom du service**    |   AJRouter    |
|   **Installation**    |   Uniquement avec l’expérience utilisateur    |
|   **StartType**   |   Manuelle  |
|   **Recommandation**  | Aucune recommandation       |
|   **Comments**    |       |
| | |

<br />

## <a name="app-readiness"></a>Préparation de l’application

| | |
|---|---|
**Description du service** |   Obtient les applications prêt à utiliser la première fois qu’un utilisateur se connecte à ce PC et lors de l’ajout de nouvelles applications.
**Nom du service**    |   AppReadiness
**Installation**    |   Uniquement avec l’expérience utilisateur
**StartType**   |   Manuelle
**Recommandation**  |   Ne désactivez pas
**Comments**    |   
| | |

<br />

##  <a name="application-identity"></a>Identité de l’application

| | |       
|---|---|   
**Description du service** |   Détermine et vérifie l’identité d’une application. La désactivation de ce service empêchera AppLocker à partir de la mise en œuvre.
**Nom du service**    |   AppIDSvc
**Installation**    |   Toujours installé
**StartType**   |   Manuelle
**Recommandation**  |Aucune recommandation    
**Comments**    |   
|||     

<br />

##  <a name="application-information"></a>Informations sur l’application 

| | |       
|---|---|   
|   **Description du service** |   Facilite l’exécution d’applications interactives avec des privilèges d’administration supplémentaires.  Si ce service est arrêté, les utilisateurs ne pourront pas lancer des applications avec des privilèges d’administration supplémentaires que nécessaires pour effectuer les tâches utilisateur souhaitées.
|   **Nom du service**    |   AppInfo
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   Prend en charge une élévation de UAC même bureau
|||     

<br />

##  <a name="application-layer-gateway-service"></a>Service de passerelle de couche application       

| | |           
|---|---|           
|   **Description du service** |   Prend en charge par des tiers plug-ins de protocole de partage de connexion Internet
|   **Nom du service**    |   ALG
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |Aucune recommandation    
|   **Comments**    |   
|||     

<br />

##  <a name="application-management"></a>Gestion des applications      

| | |           
|---|---|       
|   **Description du service** |   Traite les demandes d’installation, de suppression et d’énumération pour les logiciels déployés via Stratégie de groupe. Si le service est désactivé, les utilisateurs ne pourront pas installer, supprimer ou énumérer des logiciels déployés par la stratégie de groupe. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   AppMgmt
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="appx-deployment-service-appxsvc"></a>Service de déploiement AppX (AppXSVC)       

| | |           
|---|---|
|   **Description du service** |   Fournit la prise en charge de l’infrastructure pour le déploiement d’applications de Store. Ce service est démarré à la demande et si les applications de Store désactivées ne seront pas déployées dans le système et ne peuvent pas fonctionnent correctement.
|   **Nom du service**    |   AppXSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="auto-time-zone-updater"></a>Mise à jour du fuseau horaire d’automatique           

| | |           
|---|---|           
|   **Description du service** |   Définit automatiquement le fuseau horaire système.
|   **Nom du service**    |   tzautoupdate
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Comments**    |   
|||         

<br />          

## <a name="background-intelligent-transfer-service"></a>Service de transfert intelligent en arrière-plan (BITS)          

| | |           
|---|---|   
|   **Description du service** |   Transfère les fichiers en arrière-plan à l’aide de la bande passante réseau inactive. Si le service est désactivé, toutes les applications qui dépendent de BITS, telles que la mise à jour de Windows ou MSN Explorer, ne pourront plus télécharger automatiquement des programmes et autres informations.
|   **Nom du service**    |   BITS
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          


## <a name="background-tasks-infrastructure-service"></a>Service d’Infrastructure des tâches en arrière-plan      

| | |           
|---|---|   
|   **Description du service** |   Service d’infrastructure Windows qui contrôle les tâches en arrière-plan peut s’exécuter sur le système.
|   **Nom du service**    |   BrokerInfrastructure
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="base-filtering-engine"></a>Moteur de filtrage de base            

| | |           
|---|---|       
|   **Description du service** |   Le moteur de filtrage de Base (BFE) est un service qui gère les pare-feu et les stratégies Internet Protocol security (IPsec) et implémente le filtrage en mode utilisateur. L’arrêt ou la désactivation du service BFE réduit de manière significative la sécurité du système. Il sera également entraîner un comportement imprévisible dans les applications de pare-feu et de gestion d’IPsec.
|   **Nom du service**    |   BFE
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="bluetooth-support-service"></a>Service de prise en charge de Bluetooth            

| | |           
|---|---|   
|   **Description du service** |   Le service Bluetooth prend en charge la découverte et l’association des périphériques Bluetooth distants.  L’arrêt ou la désactivation de ce service risque de ne parviennent pas à fonctionner correctement et empêchent nouveaux appareils découverts ou associés les périphériques Bluetooth déjà installés.
|   **Nom du service**    |   bthserv
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   OK pour désactiver si omise. Un autre mécanisme de désactivation : https://technet.microsoft.com/library/dd252791.aspx
|||         

<br />          


## <a name="cdpusersvc"></a>CDPUserSvc           

| | |           
|---|---|   
|   **Description du service** |   Ce service de l’utilisateur est utilisé pour les scénarios de plateforme de périphériques connectés
|   **Nom du service**    |   CDPUserSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Automatique
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Modèle de service utilisateur
|||         

<br />          


##  <a name="certificate-propagation"></a>Propagation de certificat     

| | |           
|---|---|
|   **Description du service** |   Copie des certificats d’utilisateur et les certificats racine à partir de cartes à puce dans le magasin de certificats de l’utilisateur actuel, détecte lorsqu’une carte à puce est insérée dans un lecteur de carte à puce et si nécessaire, installe le minipilote de Plug-and-Play de carte à puce.
|   **Nom du service**    |   CertPropSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="client-license-service-clipsvc"></a>Service de licence client (ClipSVC)        

| | |           
|---|---|   
|   **Description du service** |   Fournit la prise en charge de l’infrastructure pour le Microsoft Store. Ce service est démarré à la demande et si les applications désactivées a acheté à l’aide de Microsoft Store se comportera pas correctement.
|   **Nom du service**    |   ClipSVC
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="cng-key-isolation"></a>Isolation de clé CNG

| | |           
|---|---|   
|   **Description du service** |   Le service d’isolation de clé CNG est hébergé dans le processus LSA. Le service fournit une isolation de processus de la clé pour les clés privées et les opérations de chiffrement associées comme requis par la certification critères communs. Le service stocke et utilise des clés à long terme dans un processus sécurisé répondant aux exigences des critères communs.
|   **Nom du service**    |   KeyIso
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="com-event-system"></a>Système d’événement COM +       

| | |           
|---|---|       
|   **Description du service** |   Prend en charge System Event Notification Service (SENS), qui fournit la distribution automatique des événements vers des composants de l’objet COM (Component Model) d’abonnement. Si le service est arrêté, SENS se fermera et ne sera pas en mesure de fournir des notifications d’ouverture de session et de fermeture de session. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   EventSystem
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="com-system-application"></a>Application système COM +     

| | |           
|---|---|       
|   **Description du service** |   Gère la configuration et le suivi des composants de + en fonction du composant COM (Object Model). Si le service est arrêté, la plupart des COM +-des composants de base ne fonctionnera pas correctement. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   COMSysApp
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="computer-browser"></a>Explorateur d’ordinateurs        

| | |           
|---|---|       
|   **Description du service** |   Gère une liste mise à jour des ordinateurs sur le réseau et fournit cette liste aux ordinateurs désignés comme navigateurs. Si ce service est arrêté, cette liste ne sera pas mis à jour ou libérée. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   Navigateur
|   **Installation**    |   Toujours installé
|   **StartType**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Comments**    |   
|||         

<br />          

## <a name="connected-devices-platform-service"></a>Service de plateforme d’appareils connectés       

| | |           
|---|---|       
|   **Description du service** |   Ce service est utilisé pour les scénarios d’appareils connectés et de verre universelle
|   **Nom du service**    |   CDPSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="connected-user-experiences-and-telemetry"></a>Les expériences utilisateur connecté et les données de télémétrie     

| | |           
|---|---|       
|   **Description du service** |   Le service connecté des expériences utilisateur et la télémétrie Active les fonctionnalités qui prennent en charge des expériences utilisateur dans l’application et connecté. En outre, ce service gère la collection pilotée par événements et la transmission des informations de diagnostic et d’utilisation (utilisées pour améliorer l’expérience et la qualité de la plateforme Windows) lorsque les diagnostics et les paramètres d’option de confidentialité de l’utilisation sont activés sous Commentaires et Diagnostics.
|   **Nom du service**    |   DiagTrack
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="contact-data"></a>Données de contact        

| | |           
|---|---|       
|   **Description du service** |   Index des données de contacts pour la recherche rapide de contact. Si vous arrêtez ou désactivez ce service, les contacts peuvent être manquants à partir de vos résultats de recherche.
|   **Nom du service**    |   PimIndexMaintenanceSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Modèle de service utilisateur
|||         

<br />          

## <a name="coremessaging"></a>CoreMessaging            

| | |           
|---|---|           
|   **Description du service** |   Gère la communication entre les composants système.
|   **Nom du service**    |   CoreMessagingRegistrar
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="credential-manager"></a>Gestionnaire d’informations d’identification           

| | |           
|---|---|       
|   **Description du service** |   Fournit le stockage sécurisé et la récupération des informations d’identification aux utilisateurs, applications et packages de services de sécurité.
|   **Nom du service**    |   VaultSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="cryptographic-services"></a>Services de chiffrement           

| | |           
|---|---|       
|   **Description du service** |   Fournit trois services de gestion : Service de base de données de catalogue, qui confirme la signature des fichiers de Windows ainsi que de nouveaux programmes à installer ; Service de racine protégée, qui ajoute et supprime des certificats d’autorité de Certification racine de confiance de cet ordinateur ; et Service automatique des mises à jour de certificat racine, qui Récupère les certificats racine à partir de la mise à jour de Windows et activer des scénarios tels que SSL. Si ce service est arrêté, ces services de gestion ne fonctionnera pas correctement. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   CryptSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="data-sharing-service"></a>Service de partage de données         

| | |           
|---|---|       
|   **Description du service** |   Fournit des données courtage entre les applications.
|   **Nom du service**    |   DsSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          

| | |           
|---|---|       
|   **Description du service** |   Le service de regroupement de données (la collecte de données et la publication) prend en charge les applications internes pour charger des données dans le cloud.
|   **Nom du service**    |   DcpSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="dcom-server-process-launcher"></a>Lanceur de processus DCOM         

| | |           
|---|---|       
|   **Description du service** |   Le service DCOMLAUNCH lance des serveurs COM et DCOM en réponse aux demandes d’activation de l’objet. Si ce service est arrêté ou désactivé, les programmes à l’aide de COM ou DCOM ne fonctionnera pas correctement. Il est fortement recommandé que vous disposez de l’exécution du service DCOMLAUNCH.
|   **Nom du service**    |   DcomLaunch
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  |Aucune recommandation    
|   **Comments**    |   
|||         

<br />

##  <a name="device-association-service"></a>Service d’Association de périphérique      

| | |           
|---|---|       
|   **Description du service** |   Permet de couplage entre le système et les périphériques câblés ou sans fil.
|   **Nom du service**    |   DeviceAssociationService
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />

##  <a name="device-install-service"></a>Service d’installation de périphérique

| | |
|---|---|
|   **Description du service** |   Permet à un ordinateur de reconnaître et de s’adapter aux modifications matérielles avec un utilisateur peu ou pas d’entrée. L’arrêt ou la désactivation de ce service entraîne une instabilité du système.
|   **Nom du service**    |   DeviceInstall
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation
|   **Comments**    |
|||

<br />          

##  <a name="device-management-enrollment-service"></a>Service d’inscription de gestion de périphérique        

| | |           
|---|---|       
|   **Description du service** |   Effectue des activités d’inscription de périphérique pour la gestion des appareils
|   **Nom du service**    |   DmEnrollmentSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="device-setup-manager"></a>Le programme d’installation de Device Manager         

| | |           
|---|---|       
|   **Description du service** |   Permet de détecter, téléchargement et installation du logiciel liées aux appareils. Si ce service est désactivé, les appareils peuvent être configurés avec les logiciels obsolètes et peuvent ne pas fonctionnent correctement.
|   **Nom du service**    |   DsmSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="devquery-background-discovery-broker"></a>Service Broker de découverte en arrière-plan DevQuery         

| | |           
|---|---|           
|   **Description du service** |   Permet aux applications détecter des appareils avec une tâche d’arrière-plan
|   **Nom du service**    |   DevQueryBroker
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="dhcp-client"></a>Client DHCP          

| | |           
|---|---|       
|   **Description du service** |   Enregistre et met à jour des adresses IP et les enregistrements DNS pour cet ordinateur. Si ce service est arrêté, cet ordinateur ne recevra pas les adresses IP dynamiques et les mises à jour DNS. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   Dhcp
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="diagnostic-policy-service"></a>Service de stratégie de diagnostic            

| | |           
|---|---|       
|   **Description du service** |   Le Service de stratégie de Diagnostic permet la détection de problème, le dépannage et la résolution pour les composants de Windows.  Si ce service est arrêté, les diagnostics ne fonctionneront plus.
|   **Nom du service**    |   DPS
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="diagnostic-service-host"></a>Hôte de Service de diagnostic     

| | |           
|---|---|       
|   **Description du service** |   L’hôte de Service de Diagnostic est utilisé par le Service de stratégie de Diagnostic pour les diagnostics d’hôte qui doivent s’exécuter dans un contexte de Service Local.  Si ce service est arrêté, les diagnostics qui en dépendent ne fonctionneront plus.
|   **Nom du service**    |   WdiServiceHost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="diagnostic-system-host"></a>Hôte de système de diagnostic           

| | |           
|---|---|       
|   **Description du service** |   L’hôte de système de Diagnostic est utilisé par le Service de stratégie de Diagnostic pour les diagnostics d’hôte qui doivent s’exécuter dans un contexte de système Local.  Si ce service est arrêté, les diagnostics qui en dépendent ne fonctionneront plus.
|   **Nom du service**    |   WdiSystemHost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="distributed-link-tracking-client"></a>Client de suivi de lien distribué            

| | |           
|---|---|   
|   **Description du service** |   Maintient les liens entre les fichiers NTFS sur un ordinateur ou entre ordinateurs dans un réseau.
|   **Nom du service**    |   TrkWks
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="distributed-transaction-coordinator"></a>Distributed Transaction Coordinator     

| | |           
|---|---|   
|   **Description du service** |   Coordonne les transactions qui s’étendent sur plusieurs gestionnaires de ressources, telles que les bases de données, les files d’attente et les systèmes de fichiers. Si ce service est arrêté, ces transactions échouent. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   MSDTC
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />  

##  <a name="dmwappushsvc"></a>dmwappushsvc        

| | |           
|---|---|       
|   **Description du service** |   Service de routage de Message de Push WAP
|   **Nom du service**    |   dmwappushservice
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Service requis sur les appareils clients pour Intune, gestion des appareils mobiles et les technologies de gestion similaires et pour le filtre d’écriture unifié. Pas nécessaire pour le serveur.
|||         

<br />      

##  <a name="dns-client"></a>Client DNS      

| | |           
|---|---|       
|   **Description du service** |   Le service Client DNS (dnscache) met en cache les noms DNS (Domain Name System) et inscrit le nom complet de cet ordinateur. Si le service est arrêté, la résolution des noms DNS se poursuit. Toutefois, les résultats des requêtes de noms DNS ne sont pas mises en cache et le nom d’ordinateur ne sera pas enregistré. Si le service est désactivé, le démarrage des services qui en dépendent de façon explicite échoue.
|   **Nom du service**    |   Dnscache
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="downloaded-maps-manager"></a>Gestionnaire de cartes téléchargé     

| | |           
|---|---|   
|   **Description du service** |   Service Windows pour l’accès aux applications à maps téléchargés. Ce service est démarré à la demande par application l’accès à téléchargé les mappages. La désactivation de ce service empêchera les applications d’accéder à des cartes.
|   **Nom du service**    |   MapsBroker
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Automatique
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   La désactivation de sauts les applications qui s’appuient sur le service ; OK pour désactiver si les applications ne dépendez
|||         

<br />          

## <a name="embedded-mode"></a>En Mode incorporé            

| | |           
|---|---|       
|   **Description du service** |   Le service en Mode incorporé permet des scénarios liés aux Applications d’arrière-plan.  La désactivation de ce service sera empêchent les Applications en arrière-plan en cours d’activation.
|   **Nom du service**    |   embeddedmode
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="encrypting-file-system-efs"></a>Système de fichiers EFS (Encrypting File System)

| | |                   
|---|---|           
|   **Description du service** | Fournit la technologie de chiffrement de fichier core permet de stocker des fichiers chiffrés sur des volumes de système de fichiers NTFS. Si ce service est arrêté ou désactivé, les applications ne pourront pas accéder aux fichiers chiffrés.            
|   **Nom du service**  |  EFS            
|   **Installation**  |  Toujours installé           
|   **StartType**   |  Manuelle           
|   **Recommandation**  | Aucune recommandation           
|   **Comments**   |
|||                 

<br />  

## <a name="enterprise-app-management-service"></a>Service de gestion d’application Enterprise            

| | |           
|---|---|       
|   **Description du service** |   Permet la gestion des applications enterprise.
|   **Nom du service**    |   EntAppSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="extensible-authentication-protocol"></a>Protocole d’authentification extensible           

| | |           
|---|---|   
|   **Description du service** |   Le service de protocole EAP (Extensible Authentication) fournit l’authentification réseau dans des scénarios tels que 802. 1 x câblés et sans fil, VPN et Protection d’accès réseau (NAP).  EAP fournit également des interfaces de programmation d’application (API) qui sont utilisés par les clients d’accès réseau, y compris sans fil et les clients VPN, pendant le processus d’authentification.  Si vous désactivez ce service, cet ordinateur ne peut pas accéder aux réseaux qui nécessitent l’authentification EAP.
|   **Nom du service**    |   EapHost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="function-discovery-provider-host"></a>Hôte du fournisseur de découverte de fonctions         

| | |           
|---|---|       
|   **Description du service** |   Le service FDPHOST héberge les fournisseurs de découverte du réseau Function Discovery (FD). Ces fournisseurs FD fournissent des services de découverte de réseau pour les Services de découverte protocole SSDP (Simple) et les Services Web - protocole Discovery (WS-D). L’arrêt ou la désactivation du service FDPHOST désactivera la découverte du réseau pour ces protocoles lors de l’utilisation de FD. Lorsque ce service est indisponible, les services réseau à l’aide de FD et en s’appuyant sur ces protocoles de découverte ne pourront pas être rechercher des périphériques réseau ou des ressources.
|   **Nom du service**    |   fdPHost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="function-discovery-resource-publication"></a>Publication des ressources de découverte de fonctions      

| | |           
|---|---|       
|   **Description du service** |   Publie cet ordinateur et les ressources associées à cet ordinateur afin qu’ils peuvent être détectés sur le réseau.  Si ce service est arrêté, les ressources réseau seront n’est plus publiés et qu’ils ne seront pas découvert par d’autres ordinateurs sur le réseau.
|   **Nom du service**    |   FDResPub
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="geolocation-service"></a>Service de géolocalisation          

| | |           
|---|---|       
|   **Description du service** |   Ce service surveille l’emplacement actuel du système et gère les clôtures virtuelles (un emplacement géographique avec des événements associés).  Si vous désactivez ce service, les applications ne pourront pas être à utiliser ou de recevoir des notifications pour l’emplacement géographique ou de la clôture virtuelle.
|   **Nom du service**    |   lfsvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   La désactivation de sauts les applications qui s’appuient sur le service ; OK pour désactiver si les applications ne dépendez
|||         

<br />          

##  <a name="group-policy-client"></a>Client de stratégie de groupe     

| | |           
|---|---|       
|   **Description du service** |   Le service est chargé d’appliquer les paramètres configurés par les administrateurs de l’ordinateur et les utilisateurs via le composant de stratégie de groupe. Si le service est désactivé, les paramètres ne seront pas appliqués et les applications et les composants ne pourront pas être gérés via la stratégie de groupe. Les composants ou les applications qui dépendent du composant de stratégie de groupe n’est peut-être pas opérationnel si le service est désactivé.
|   **Nom du service**    |   gpsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          


## <a name="human-interface-device-service"></a>Service du périphérique d’interface utilisateur           

| | |           
|---|---|       
|   **Description du service** |   Active et gère l’utilisation des boutons actifs sur les claviers, les télécommandes et les autres périphériques multimédia. Il est recommandé de conserver ce service en cours d’exécution.
|   **Nom du service**    |   hidserv
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="hv-host-service"></a>Service d’hôte HV     

| | |           
|---|---|   
|   **Description du service** |   Fournit une interface pour l’hyperviseur Hyper-V fournir des compteurs de performance par partition pour le système d’exploitation hôte.
|   **Nom du service**    |   HvHost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Améliorateurs de performance pour les machines virtuelles invitées. Non utilisé aujourd'hui à l’exception d’explicitement rempli de machines virtuelles, mais sera utilisé dans l’Application Guard
|||         

<br />          

## <a name="hyper-v-data-exchange-service"></a>Service Échange de données Microsoft Hyper-V        

| | |           
|---|---|   
|   **Description du service** |   Propose un mécanisme d’échange de données entre l’ordinateur virtuel et le système d’exploitation exécuté sur l’ordinateur physique.
|   **Nom du service**    |   vmickvpexchange
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Consultez HvHost
|||         

<br />      

## <a name="hyper-v-guest-service-interface"></a>Interface de services d’invité Hyper-V          

| | |           
|---|---|   
|   **Description du service** |   Fournit une interface pour l’hôte Hyper-V interagir avec les services spécifiques en cours d’exécution à l’intérieur de la machine virtuelle.
|   **Nom du service**    |   vmicguestinterface
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Consultez HvHost
|||         

<br />  

## <a name="hyper-v-guest-shutdown-service"></a>Service Arrêt de l’invité Microsoft Hyper-V           

| | |           
|---|---|       
|   **Description du service** |   Fournit un mécanisme pour arrêter le système d’exploitation de cet ordinateur virtuel à partir des interfaces de gestion sur l’ordinateur physique.
|   **Nom du service**    |   vmicshutdown
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Consultez HvHost
|||         

<br />

## <a name="hyper-v-heartbeat-service"></a>Service Pulsation Microsoft Hyper-V
| | |
|---|---|
|   **Description du service** |   Surveille l’état de cet ordinateur virtuel en signalant une pulsation à intervalles réguliers. Ce service vous aide à identifier les machines virtuelles en cours d’exécution qui ont cessé de répondre.
|   **Nom du service**    |   vmicheartbeat
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Consultez HvHost
|||

<br />          

## <a name="hyper-v-powershell-direct-service"></a>Service PowerShell Direct Hyper-V            

| | |           
|---|---|       
|   **Description du service** |   Fournit un mécanisme permettant de gérer la machine virtuelle avec PowerShell via une session de machine virtuelle sans réseau virtuel.
|   **Nom du service**    |   vmicvmsession
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Consultez HvHost
|||         

<br />          

## <a name="hyper-v-remote-desktop-virtualization-service"></a>Service de virtualisation Bureau à distance Hyper-V            

| | |           
|---|---|       
|   **Description du service** |   Fournit une plateforme pour la communication entre la machine virtuelle et le système d’exploitation en cours d’exécution sur l’ordinateur physique.
|   **Nom du service**    |   vmicrdv
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Consultez HvHost
|||         

<br />          

## <a name="hyper-v-time-synchronization-service"></a>Service Synchronisation date/heure Microsoft Hyper-V         

| | |           
|---|---|       
|   **Description du service** |   Synchronise l’heure système de cet ordinateur virtuel avec l’heure système de l’ordinateur physique.
|   **Nom du service**    |   vmictimesync
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Consultez HvHost
|||         

<br />          

## <a name="hyper-v-volume-shadow-copy-requestor"></a>Requête du service VSS Microsoft Hyper-V         

| | |           
|---|---|           
|   **Description du service** |   Coordonne les communications qui sont requises pour utiliser le Service VSS pour sauvegarder les données et applications sur cet ordinateur virtuel à partir du système d’exploitation sur l’ordinateur physique.
|   **Nom du service**    |   vmicvss
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Consultez HvHost
|||         

<br />          

## <a name="ike-and-authip-ipsec-keying-modules"></a>Modules de génération de clés IKE et AuthIP          

| | |           
|---|---|       
|   **Description du service** |   Le service IKEEXT héberge l’Internet Key Exchange (IKE) et Authenticated Internet Protocol (AuthIP) modules de génération de clés. Ces modules de gestion de clés sont utilisés pour l’authentification et l’échange de clés dans la sécurité du protocole Internet (IPsec). L’arrêt ou la désactivation du service IKEEXT désactivera l’échange de clés IKE et AuthIP avec des ordinateurs homologues. IPsec est généralement configuré pour utiliser IKE et AuthIP ; Par conséquent, l’arrêt ou la désactivation du service IKEEXT peut entraîner un échec d’IPsec et peut compromettre la sécurité du système. Il est fortement recommandé que vous disposez de l’exécution du service IKEEXT.
|   **Nom du service**    |   IKEEXT
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |    
|||         

<br />          

## <a name="interactive-services-detection"></a>Détection de services interactifs           

| | |           
|---|---|   
|   **Description du service** |   Active la notification des entrées utilisateur pour les services interactifs, ce qui permet d’accéder aux boîtes de dialogue créées par les services interactifs lorsqu’ils apparaissent. Si ce service est arrêté, les notifications de nouvelles boîtes de dialogue de service interactif ne fonctionneront plus et sera peut-être pas accès aux boîtes de dialogue de service interactif. Si ce service est désactivé, les notifications d’et l’accès aux boîtes de dialogue Nouveau service interactif ne fonctionneront plus.
|   **Nom du service**    |   UI0Detect
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />  

## <a name="internet-connection-sharing-ics"></a>Partage de connexion Internet (ICS)            

| | |           
|---|---|           
|   **Description du service** |   Fournit la traduction d’adresses réseau, d’adressage, services de prévention des intrusions et/ou de résolution nom pour un réseau domestique ou de petite entreprise.
|   **Nom du service**    |   SharedAccess
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Requis pour les clients utilisés comme points d’accès Wi-Fi et également aux deux extrémités de projection Miracast. ICS peut être bloquée avec le paramètre de stratégie de groupe, « Interdire l’utilisation du partage de connexion Internet sur votre réseau de domaine DNS »
|||         

<br />          

## <a name="ip-helper"></a>Assistance IP            

| | |           
|---|---|       
|   **Description du service** |   Fournit la connectivité de tunnel à l’aide de technologies de transition IPv6 (6to4, Teredo, ISATAP et Port Proxy) et IP-HTTPS. Si ce service est arrêté, l’ordinateur n’aura pas les avantages d’une connectivité améliorée qui offrent ces technologies.
|   **Nom du service**    |   iphlpsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          


##  <a name="ipsec-policy-agent"></a>Agent de stratégie IPSec      

| | |           
|---|---|       
|   **Description du service** |   Internet Protocol security (IPsec) prend en charge au niveau du réseau homologue d’authentification, authentification de l’origine, l’intégrité des données, la confidentialité des données (chiffrement) et protection de la relecture.  Ce service applique les stratégies IPsec créées via le composant logiciel enfichable Stratégies de sécurité IP ou l’outil de ligne de commande « netsh ipsec ».  Si vous arrêtez ce service, vous pouvez rencontrer des problèmes de connectivité réseau si votre stratégie nécessite des connexions IPsec.  En outre, la gestion à distance du pare-feu de Windows n’est pas disponible lorsque ce service est arrêté.
|   **Nom du service**    |   PolicyAgent
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />

##  <a name="kdc-proxy-server-service-kps"></a>Service de serveur Proxy KDC (KPS)      

| | |           
|---|---|       
|   **Description du service** |   Service de serveur Proxy KDC s’exécute sur les serveurs edge vers le proxy Kerberos aux contrôleurs de domaine sur le réseau d’entreprise, les messages de protocole.
|   **Nom du service**    |   KPSSVC
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation    
|   **Comments**    |   
|||         

<br />          

## <a name="ktmrm-for-distributed-transaction-coordinator"></a>Service KtmRm pour Distributed Transaction Coordinator            

| | |           
|---|---|       
|   **Description du service** |   Coordonne les transactions entre Distributed Transaction Coordinator (MSDTC) et le noyau KTM (Kernel Transaction Manager). S’il n’est pas nécessaire, il est recommandé que ce service restent arrêté. S’il est nécessaire, MSDTC et KTM démarre ce service automatiquement. Si ce service est désactivé, toutes les transactions MSDTC interagir avec un gestionnaire de ressources noyau échoue et tous les services qui en dépend explicitement échouera.
|   **Nom du service**    |   KtmRm
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />

##  <a name="link-layer-topology-discovery-mapper"></a>Mappeur de détection de topologie de couche de liaison        

| | |       
|---|---|       
|   **Description du service** |   Crée un mappage réseau composé de PC et les informations de topologie (connectivité) d’appareil et les métadonnées décrivant chaque ordinateur et l’appareil.  Si ce service est désactivé, la carte réseau ne fonctionnera pas correctement.
|   **Nom du service**    |   lltdsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   OK pour désactiver si aucune dépendance sur la carte de réseau
|||         

<br />

## <a name="local-session-manager"></a>Gestionnaire de Session locale                    

| | |                   
|---|---|   
|   **Description du service** |   Service de Windows de base qui gère les sessions d’utilisateur local. L’arrêt ou la désactivation de ce service entraîne une instabilité du système.    
|   **Nom du service**    |   LSM |
|   **Installation**    |   Toujours installé    |
|   **StartType**   |   Automatique   |
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||                 

<br />                  

## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Collecteur Standard du Hub de diagnostic Microsoft (R)         

| | |           
|---|---|           
|   **Description du service** |   Service de Windows de base qui gère les sessions d’utilisateur local. L’arrêt ou la désactivation de ce service entraîne une instabilité du système.
|   **Nom du service**    |   LSM
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />

## <a name="microsoft-account-sign-in-assistant"></a>Assistant de connexion compte Microsoft
| | |
|---|---|
|   **Description du service** |   Permet la connexion de l’utilisateur via les services d’identité compte Microsoft. Si ce service est arrêté, les utilisateurs ne sera pas en mesure de se connecter à l’ordinateur avec leur compte Microsoft.
|   **Nom du service**    |   wlidsvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Accounts Microsoft sont n/a sur Windows Server
|||

<br />          

##  <a name="microsoft-app-v-client"></a>Client Microsoft App-V      

| | |           
|---|---|       
|   **Description du service** |   Gère les utilisateurs d’App-V et des applications virtuelles
|   **Nom du service**    |   AppVClient
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Comments**    |   
|||         

<br />          

## <a name="microsoft-iscsi-initiator-service"></a>Service initiateur Microsoft iSCSI            

| | |           
|---|---|       
|   **Description du service** |   Gère les sessions iSCSI (Internet SCSI) à partir de cet ordinateur pour les périphériques cible iSCSI à distance. Si ce service est arrêté, il se peut que cet ordinateur ne sera pas en mesure de se connecter ou accéder aux cibles iSCSI. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   MSiSCSI
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Nos données de diagnostic indiquent que cela est utilisé sur le client, ainsi que de serveur. Aucun avantage à la désactivation de ce.
|||         

<br />          

## <a name="microsoft-passport"></a>Microsoft Passport           

| | |           
|---|---|   
|   **Description du service** |   Fournit une isolation de processus pour les clés de chiffrement utilisé pour s’authentifier auprès de fournisseurs d’identité associé de l’utilisateur. Si ce service est désactivé, toutes les utilisations et la gestion de ces clés pas sera disponible, qui inclut une ouverture de session de machine et l’authentification unique, sur des applications et sites Web. Ce service démarre et s’arrête automatiquement. Il est recommandé que vous ne reconfigurez pas ce service.
|   **Nom du service**    |   NgcSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Nécessaire pour les connexions du code confidentiel/Hello, qui ne sont pas pris en charge sur le serveur
|||         

<br />          

## <a name="microsoft-passport-container"></a>Conteneur de Microsoft Passport         

| | |           
|---|---|       
|   **Description du service** |   Gère les clés d’identité utilisateur local utilisés pour authentifier l’utilisateur pour les fournisseurs d’identité, ainsi que les cartes à puce virtuelles TPM. Si ce service est désactivé, les clés d’identité utilisateur local et les cartes à puce virtuelles TPM ne sera pas accessibles. Il est recommandé que vous ne reconfigurez pas ce service.
|   **Nom du service**    |   NgcCtnrSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

## <a name="microsoft-software-shadow-copy-provider"></a>Fournisseur de clichés instantanés logiciels Microsoft          

| | |           
|---|---|       
|   **Description du service** |   Gère les clichés instantanés de volume de logiciel pris par le service de cliché instantané de Volume. Si ce service est arrêté, les clichés instantanés de volume de logiciel ne peut être gérées. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   swprv
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="microsoft-storage-spaces-smp"></a>Espaces de stockage de Microsoft SMP         

| | |           
|---|---|       
|   **Description du service** |   Service hôte pour le fournisseur de gestion des espaces de stockage de Microsoft. Si ce service est arrêté ou désactivé, les espaces de stockage ne peut pas être gérés.
|   **Nom du service**    |   smphost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Gestion du stockage API échouent sans ce service. Exemple : « Get-WmiObject-classe MSFT_Disk - Namespace Root\Microsoft\Windows\Storage ».
|||         

<br />          

## <a name="nettcp-port-sharing-service"></a>Service de partage de ports Net.Tcp         

| | |           
|---|---|       
|   **Description du service** |   Fournit la possibilité de partager des ports TCP sur le protocole net.tcp.
|   **Nom du service**    |   NetTcpPortSharing
|   **Installation**    |   Toujours installé
|   **StartType**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Comments**    |   
|||         

<br />          

## <a name="netlogon"></a>Netlogon         

| | |           
|---|---|           
|   **Description du service** |   Gère un canal sécurisé entre cet ordinateur et le contrôleur de domaine pour authentifier les utilisateurs et services. Si ce service est arrêté, l’ordinateur ne peut pas authentifier les utilisateurs et services et le contrôleur de domaine ne peut pas inscrire les enregistrements DNS. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   Netlogon
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="network-connection-broker"></a>Service Broker pour les connexions réseau            

| | |           
|---|---|       
|   **Description du service** |   Connexions de répartiteurs qui permettent aux applications du Microsoft Store recevoir des notifications à partir d’internet.
|   **Nom du service**    |   NcbService
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

##  <a name="network-connections"></a>Connexions réseau         

| | |           
|---|---|   
|   **Description du service** |   Gère les objets dans le dossier Connexions réseau et accès à distance, dans lequel vous pouvez afficher le réseau local et les connexions à distance.
|   **Nom du service**    |   Netman
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="network-connectivity-assistant"></a>Assistant de connectivité réseau      

| | |           
|---|---|       
|   **Description du service** |   Fournit une notification d’état de DirectAccess pour les composants d’interface utilisateur
|   **Nom du service**    |   NcaSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />  

##  <a name="network-list-service"></a>Service liste des réseaux        

| | |           
|---|---|   
|   **Description du service** |   Identifie les réseaux auxquels l’ordinateur s’est connecté, collecte et stocke les propriétés de ces réseaux et avertit les applications lorsque ces propriétés sont modifiées.
|   **Nom du service**    |   netprofm
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="network-location-awareness"></a>Connaissance des emplacements réseau           

| | |           
|---|---|       
|   **Description du service** |   Collecte et stocke les informations de configuration pour le réseau et notifie les programmes lors de la modification de ces informations. Si ce service est arrêté, les informations de configuration peuvent être indisponibles. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   NlaSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="network-setup-service"></a>Configuration du Service réseau       

| | |           
|---|---|       
|   **Description du service** |   Le Service de programme d’installation réseau gère l’installation des pilotes réseau et autorise la configuration des paramètres de réseau de bas niveau.  Si ce service est arrêté, toutes les installations de pilotes qui sont en cours d’exécution peuvent être annulées.
|   **Nom du service**    |   NetSetupSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="network-store-interface-service"></a>Service d’Interface réseau Store      

| | |           
|---|---|   
|   **Description du service** |   Ce service fournit des notifications de réseau (par exemple, interface Ajout/Suppression etc.) pour les clients en mode utilisateur. Arrêt du service entraînera la perte de connectivité réseau. Si ce service est désactivé, tous les autres services qui en dépendent explicitement échoue à démarrer.
|   **Nom du service**    |   nsi
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="offline-files"></a>Fichiers hors connexion            

| | |           
|---|---|       
|   **Description du service** |   Les fichiers hors connexion de service effectue les activités de maintenance sur le cache de fichiers hors connexion, répond aux événements d’ouverture de session et de fermeture de session utilisateur, implémente les mécanismes internes de l’API publique, et distribue événements intéressants à ceux intéressés fichiers hors connexion des activités et modifications de l’état de cache.
|   **Nom du service**    |   CscService
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Comments**    |   
|||         

<br />          

## <a name="optimize-drives"></a>Optimiser les lecteurs          

| | |           
|---|---|   
|   **Description du service** |   Permet de s’exécuter plus efficacement en optimisant les fichiers des unités de stockage sur l’ordinateur.
|   **Nom du service**    |   defragsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />

## <a name="performance-counter-dll-host"></a>Hôte DLL de compteur de performances         

| | |           
|---|---|       
|   **Description du service** |   Permet aux utilisateurs distants et les processus 64 bits interroger les compteurs de performance fournis par la DLL 32 bits. Si ce service est arrêté, seuls les utilisateurs locaux et les processus 32 bits sera en mesure d’interroger les compteurs de performance fournis par la DLL 32 bits.
|   **Nom du service**    |   PerfHost
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation    
|   **Comments**    |   
|||         

<br />          

## <a name="performance-logs--alerts"></a>Alertes et les journaux de performances            

| | |           
|---|---|   
|   **Description du service** |   Les journaux de performances et alertes collecte des données de performances à partir d’ordinateurs locaux ou distants, en fonction des paramètres de planification préconfigurés, puis écrit les données dans un journal ou déclenche une alerte. Si ce service est arrêté, il se peut que les informations de performances ne seront pas collectées. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   Pla
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="phone-service"></a>Service téléphonique       

| | |           
|---|---|   
|   **Description du service** |   Gère l’état de la téléphonie sur l’appareil
|   **Nom du service**    |   PhoneSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Utilisé par les applications modernes VoIP
|||         

<br />          

##      <a name="plug-and-play"></a>Plug-and-Play       

| | |           
|---|---|   
|   **Description du service** |   Permet à un ordinateur de reconnaître et de s’adapter aux modifications matérielles avec un utilisateur peu ou pas d’entrée. L’arrêt ou la désactivation de ce service entraîne une instabilité du système.
|   **Nom du service**    |   PlugPlay
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="portable-device-enumerator-service"></a>Service d’énumérateur de périphérique portable           

| | |           
|---|---|       
|   **Description du service** |   Applique la stratégie de groupe pour les périphériques de stockage de masse amovibles. Permet aux applications telles que Windows Media Player et Assistant Importation d’images pour transférer et synchroniser le contenu à l’aide de périphériques de stockage de masse amovibles.
|   **Nom du service**    |   WPDBusEnum
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="power"></a>Alimentation            

| | |           
|---|---|       
|   **Description du service** |   Gère la stratégie d’alimentation et de remise des notifications de stratégie d’alimentation.
|   **Nom du service**    |   Alimentation
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="print-spooler"></a>Spouleur d’impression            

| | |           
|---|---|   
|   **Description du service** |   Ce service met en attente de travaux d’impression et gère l’interaction avec l’imprimante.  Si vous désactivez ce service, il se peut que vous ne pourrez pas imprimer ou de voir vos imprimantes.
|   **Nom du service**    |   Spouleur
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  |   OK pour désactiver si pas un serveur d’impression ou un contrôleur de domaine
|   **Comments**    |   Sur un contrôleur de domaine, l’installation du rôle de contrôleur de domaine ajoute un thread pour le service de spouleur est chargé d’effectuer le nettoyage de l’impression – supprimer les objets de file d’attente obsolète à partir de l’annuaire Active Directory.  Si le service spouleur n’est pas en cours d’exécution sur au moins un contrôleur de domaine dans chaque site, Active Directory n’a aucun moyen de supprimer des files d’attente à l’ancien qui n’existent plus. https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/
|||         

<br />          

##  <a name="printer-extensions-and-notifications"></a>Notifications et les Extensions de l’imprimante        

| | |           
|---|---|       
|   **Description du service** |   Ce service pour ouvrir les boîtes de dialogue imprimante personnalisé et gère les notifications à partir d’un serveur d’impression à distance ou une imprimante. Si vous désactivez ce service, vous ne pourrez pas voir les extensions de l’imprimante ou des notifications.
|   **Nom du service**    |   PrintNotify
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver si ce n’est pas un serveur d’impression
|   **Comments**    |   
|||         

<br />          

##  <a name="problem-reports-and-solutions-control-panel-support"></a>Rapports de problème et de Support de panneau de contrôle de Solutions     

| | |           
|---|---|   
|   **Description du service** |   Ce service prend en charge pour l’affichage, l’envoi et la suppression des rapports de problèmes de niveau système pour le panneau de configuration rapports et Solutions.
|   **Nom du service**    |   wercplsupport
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="program-compatibility-assistant-service"></a>Service Assistant Compatibilité des programmes     

| | |           
|---|---|       
|   **Description du service** |   Ce service prend en charge pour la compatibilité Compagnon des programmes.  PCA surveille les programmes installés et exécutés par l’utilisateur et détecte les problèmes de compatibilité connus. Si ce service est arrêté, PCA ne fonctionnera pas correctement.
|   **Nom du service**    |   PcaSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Automatique
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

##  <a name="quality-windows-audio-video-experience"></a>Expérience audio-vidéo haute qualité Windows      

| | |           
|---|---|   
|   **Description du service** |   Expérience Quality Windows audio-vidéo (qWave) est une plateforme de mise en réseau pour Audio vidéo (AV) diffusion en continu des applications sur les réseaux domestiques IP. qWave améliore AV en continu les performances et la fiabilité en veillant à ce réseau qualité de service (QoS) pour les applications antivirus. Il fournit des mécanismes de contrôle d’admission, exécutez la surveillance en temps et l’application commentaires sur l’application et définition des priorités du trafic.
|   **Nom du service**    |   QWAVE
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Service de qualité de service côté client
|||         

<br />          

##      <a name="radio-management-service"></a>Service de gestion de case d’option        

| | |           
|---|---|   
|   **Description du service** |   Case d’option de gestion et Service de Mode avion
|   **Nom du service**    |   RmSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

## <a name="remote-access-auto-connection-manager"></a>Gestionnaire de connexions à distance accès automatique            

| | |           
|---|---|   
|   **Description du service** |   Crée une connexion à un réseau à distance chaque fois qu’un programme fait référence à un nom DNS ou NetBIOS ou une adresse distante.
|   **Nom du service**    |   RasAuto
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="remote-access-connection-manager"></a>Gestionnaire de connexions d’accès à distance         

| | |           
|---|---|   
|   **Description du service** |   Gère les connexions d’accès à distance et virtuels réseau privé (VPN) à partir de cet ordinateur à Internet ou d’autres réseaux distants. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   RasMan
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="remote-desktop-configuration"></a>Configuration du Bureau à distance         

| | |           
|---|---|   
|   **Description du service** |   Service de Configuration du Bureau à distance (RDCS) est responsable de tous les Services Bureau à distance et Bureau à distance de configuration connexe et les activités de maintenance de session qui nécessitent le contexte du système. Ceux-ci incluent les dossiers temporaires par session, les thèmes du Bureau à distance et les certificats de bureau à distance.
|   **Nom du service**    |   SessionEnv
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   
|||         

<br />          

## <a name="remote-desktop-services"></a>Services Bureau à distance          

| | |           
|---|---|   
|   **Description du service** |   Permet aux utilisateurs de se connecter de manière interactive à un ordinateur distant. Bureau à distance et le serveur hôte de Session Bureau à distance dépendent de ce service.  Pour empêcher une utilisation à distance de cet ordinateur, désactivez les cases à cocher sous l’onglet à distance de l’élément du Panneau de configuration Propriétés système.
|   **Nom du service**    |   TermService
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   
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
|   **Comments**    |   Prend en charge les redirections côté serveur de la connexion.
|||         

<br />          

## <a name="remote-procedure-call-rpc"></a>Appel de procédure distante (RPC)          

| | |           
|---|---|   
|   **Description du service** |   Le service RPCSS est le Gestionnaire de contrôle de Service pour les serveurs COM et DCOM. Il effectue des requêtes d’activations d’objets, de résolutions d’exportateur d’objet et de garbage collection distribuée pour les serveurs COM et DCOM. Si ce service est arrêté ou désactivé, les programmes à l’aide de COM ou DCOM ne fonctionnera pas correctement. Il est fortement recommandé que vous disposez de l’exécution du service RPCSS.
|   **Nom du service**    |   RpcSs
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="remote-procedure-call-rpc-locator"></a>Localisateur de procédure distante (RPC) appel             

| | |               
|---|---|   
|   **Description du service** |   Dans Windows 2003 et versions antérieures de Windows, le service de recherche de l’appel de procédure distante (RPC) gère la base de données du service de nom RPC. Dans Windows Vista et versions ultérieures de Windows, ce service ne fournit pas toutes les fonctionnalités et n’est présent pour la compatibilité des applications.   |
|   **Nom du service**    |   RpcLocator  |
|   **Installation**    |   Uniquement avec l’expérience utilisateur    |
|   **StartType**   |   Manuelle  |
|   **Recommandation**  | Aucune recommandation   |
|   **Comments**    |       |
|||             

<br />              

## <a name="remote-registry"></a>Accès à distance au Registre          

| | |           
|---|---|   
|   **Description du service** |   Permet aux utilisateurs distants de modifier les paramètres du Registre sur cet ordinateur. Si ce service est arrêté, le Registre peut être modifié que par les utilisateurs sur cet ordinateur. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   RemoteRegistry
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   
|||         

<br />          

##  <a name="resultant-set-of-policy-provider"></a>Jeu de fournisseur de stratégie résultant            

| | |           
|---|---|       
|   **Description du service** |   Fournit un service de réseau qui traite les demandes pour simuler l’application des paramètres de stratégie de groupe pour un utilisateur ou ordinateur cible dans diverses situations et calcule les paramètres du jeu de stratégie résultant.
|   **Nom du service**    |   RSoPProv
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |Aucune recommandation    
|   **Comments**    |   
|||         

<br />          

## <a name="routing-and-remote-access"></a>Routage et accès à distance            

| | |           
|---|---|   
|   **Description du service** |   Offres de services de routage aux entreprises dans les environnements de réseau étendu et local.
|   **Nom du service**    |   RemoteAccess
|   **Installation**    |   Toujours installé
|   **StartType**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Comments**    |   Déjà désactivé
|||         

<br />          

## <a name="rpc-endpoint-mapper"></a>Mappeur de point de terminaison RPC          

| | |           
|---|---|   
|   **Description du service** |   Résout les identificateurs des interfaces RPC aux points de terminaison de transport. Si ce service est arrêté ou désactivé, les programmes à l’aide des services d’appel de procédure distante (RPC) ne fonctionnera pas correctement.
|   **Nom du service**    |   RpcEptMapper
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="secondary-logon"></a>Ouverture de session secondaire     

| | |           
|---|---|       
|   **Description du service** |   Permet le démarrage des processus sous d’autres informations d’identification. Si ce service est arrêté, ce type d’ouverture de session n’est pas disponible. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   Seclogon
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="secure-socket-tunneling-protocol-service"></a>Secure Socket Tunneling Protocol Service            

| | |               
|---|---|       
|   **Description du service** |   Prend en charge pour le Tunneling protocole SSTP (Secure Socket) pour se connecter à des ordinateurs distants à l’aide de VPN. Si ce service est désactivé, il se peut que les utilisateurs ne sera pas en mesure d’utiliser SSTP pour accéder aux serveurs distants.    |
|   **Nom du service**    |   SstpSvc |
|   **Installation**    |   Toujours installé    |
|   **StartType**   |   Manuelle  |
|   **Recommandation**  |   Ne désactivez pas  |
|   **Comments**    |   La désactivation des sauts de RRAS   |
|||             

<br />              

## <a name="security-accounts-manager"></a>Gestionnaire de comptes de sécurité            

| | |           
|---|---|       
|   **Description du service** |   Le démarrage de ce service signale des autres services que le Gestionnaire de comptes de sécurité (SAM) est prêt à accepter les demandes.  La désactivation de ce service empêche les autres services dans le système d’être averti lorsque le module SAM est prêt, qui à son tour risque de ne pas démarrer correctement de ces services. Ce service ne doit pas être désactivé.
|   **Nom du service**    |   SamSs
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Ne désactivez pas
|   **Comments**    |   
|||         

<br />          

## <a name="sensor-data-service"></a>Service de données de capteur  

| | |           
|---|---|   
|   **Description du service** |   Fournit des données à partir d’une variété de capteurs
|   **Nom du service**    |   SensorDataService
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />  

## <a name="sensor-monitoring-service"></a>Service de surveillance de capteur            

| | |           
|---|---|       
|   **Description du service** |   Surveille les divers capteurs afin d’exposer des données et de s’adapter à l’état système et utilisateur.  Si ce service est arrêté ou désactivé, la luminosité adaptent pas aux conditions d’éclairage. Arrêt du service peut affecter les autres fonctionnalités du système et les fonctionnalités ainsi.
|   **Nom du service**    |   SensrSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br /><br/>
## <a name="sensor-servicebr--br------br---strongservice-descriptionstrong----a-service-for-sensors-that-manages-different-sensors39-functionality-manages-simple-device-orientation-sdo-and-history-for-sensors-loads-the-sdo-sensor-that-reports-device-orientation-changes--if-this-service-is-stopped-or-disabled-the-sdo-sensor-will-not-be-loaded-and-so-auto-rotation-will-not-occur-history-collection-from-sensors-will-also-be-stopped"></a>Service de capteur<br/>| | |<br/>|---|---|<br/>|   <strong>Description du service</strong> |   Un service pour les capteurs qui gère les différents capteurs&#39; fonctionnalité. Gère l’Orientation de périphérique Simple (SDO) et l’historique pour les capteurs. Charge le capteur SDO qui signale les changements d’orientation de périphérique.  Si ce service est arrêté ou désactivé, le capteur SDO ne sera pas chargé et par conséquent, la rotation automatique n’a pas lieu. Collection de l’historique des capteurs est également arrêtée.
|   <strong>Nom du service</strong> |   SensorService |   <strong>Installation</strong> |   Uniquement avec l’expérience utilisateur |   <strong>StartType</strong> |   Manuel |   <strong>Recommandation</strong> |   OK pour désactiver |   <strong>Commentaires</strong>    |<br/>|||<br/>
<br />          

## <a name="server"></a>Server           

| | |           
|---|---|   
|   **Description du service** |   Prend en charge le fichier, d’impression et de canal nommé de partage sur le réseau pour cet ordinateur. Si ce service est arrêté, ces fonctions ne seront pas disponibles. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   LanmanServer
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Nécessaire pour la gestion à distance, IPC$, le partage de fichiers SMB
|||         

<br />          

## <a name="shell-hardware-detection"></a>Détection matériel noyau             

| | |           
|---|---|       
|   **Description du service** |   Fournit des notifications pour les événements de matériel de lecture automatique.
|   **Nom du service**    |   ShellHWDetection
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Automatique
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

## <a name="smart-card"></a>Carte à puce           

| | |           
|---|---|   
|   **Description du service** |   Gère l’accès aux cartes à puce lues par cet ordinateur. Si ce service est arrêté, cet ordinateur sera impossible de lire les cartes à puce. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   SCardSvr
|   **Installation**    |   Toujours installé
|   **StartType**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Comments**    |   
|||         

<br />          

## <a name="smart-card-device-enumeration-service"></a>Service d’énumération de périphériques de carte à puce                    

| | |               
|---|---|       
|   **Description du service** |   Crée les nœuds de périphérique de logiciels pour tous les lecteurs de carte à puce accessibles à une session donnée. Si ce service est désactivé, il se peut que WinRT APIs ne sera pas en mesure d’énumérer les lecteurs de carte à puce.   |
|   **Nom du service**    |   ScDeviceEnum    |
|   **Installation**    |   Toujours installé    |
|   **StartType**   |   Manuelle  |
|   **Recommandation**  |   OK pour désactiver   |
|   **Comments**    |   Nécessaire presque exclusivement pour les applications WinRT    |
|||             

<br />              

## <a name="smart-card-removal-policy"></a>Stratégie de suppression de carte à puce        

| | |           
|---|---|       
|   **Description du service** |   Permet au système d’être configurés pour verrouiller le bureau de l’utilisateur lors de la suppression de la carte à puce.
|   **Nom du service**    |   SCPolicySvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="snmp-trap"></a>Interruption SNMP            

| | |           
|---|---|       
|   **Description du service** |   Reçoit des messages d’interruption générés par les agents SNMP Simple Network Management Protocol () local ou distant et transfère les messages aux programmes de gestion SNMP s’exécutant sur cet ordinateur. Si ce service est arrêté, programmes SNMP sur cet ordinateur ne recevra pas de messages d’interruption SNMP. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   SNMPTRAP
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="software-protection"></a>Protection logicielle             

| | |           
|---|---|       
|   **Description du service** |   Permet le téléchargement, l’installation et la mise en œuvre de licences numériques pour Windows et Windows applications. Si le service est désactivé, le système d’exploitation et les applications sous licence peuvent s’exécuter dans un mode de notification. Il est fortement recommandé que vous ne désactivez pas le service de Protection logicielle.
|   **Nom du service**    |   sppsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="special-administration-console-helper"></a>Application d’assistance de la Console Administration spéciale        

| | |           
|---|---|   
|   **Description du service** |   Permet aux administrateurs d’accéder à distance à une invite de commandes à l’aide des Services de gestion d’urgence.
|   **Nom du service**    |   sacsvr
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="spot-verifier"></a>Vérification rapide            

| | |           
|---|---|   
|   **Description du service** |   Vérifie les altérations de système de fichiers potentiels.
|   **Nom du service**    |   svsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="ssdp-discovery"></a>Découverte SSDP           

| | |           
|---|---|   
|   **Description du service** |   Détecte les périphériques réseau et les services qui utilisent le protocole de découverte SSDP, tels que les appareils UPnP. Annonce également les périphériques SSDP et les services qui s’exécutent sur l’ordinateur local. Si ce service est arrêté, les périphériques SSDP ne seront pas découvert. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   SSDPSRV
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

## <a name="state-repository-service"></a>Service de l’état de référentiel         

| | |           
|---|---|   
|   **Description du service** |   Fournit la prise en charge de l’infrastructure requise pour le modèle d’application.
|   **Nom du service**    |   StateRepository
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="still-image-acquisition-events"></a>Événements d’Acquisition d’images fixes

| | |           
|---|---|   
|   **Description du service** |   Lance des applications associées à toujours les événements d’acquisition image.
|   **Nom du service**    |   WiaRpc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />  

## <a name="storage-service"></a>Service de stockage          

| | |           
|---|---|       
|   **Description du service** |   Fournit des services d’activation pour les paramètres de stockage et l’expansion de stockage externe
|   **Nom du service**    |   StorSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="storage-tiers-management"></a>Gestion des niveaux de stockage        

| | |           
|---|---|   
|   **Description du service** |   Optimise le positionnement des données dans les niveaux de stockage sur tous les espaces de stockage hiérarchisé dans le système.
|   **Nom du service**    |   TieringEngineService
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="superfetch"></a>SuperFetch          

| | |           
|---|---|       
|   **Description du service** |   Gère et améliore les performances du système au fil du temps.
|   **Nom du service**    |   SysMain
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="sync-host"></a>Hôte de synchronisation            

| | |           
|---|---|       
|   **Description du service** |   Ce service synchronise le courrier, contacts, calendrier et diverses autres données utilisateur. Messagerie et autres applications dépendantes de cette fonctionnalité ne fonctionnent pas correctement lorsque ce service n’est pas en cours d’exécution.
|   **Nom du service**    |   OneSyncSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Automatique
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Modèle de service utilisateur
|||         

<br />          

## <a name="system-event-notification-service"></a>Service de Notification d’événement système            

| | |           
|---|---|       
|   **Description du service** |   Surveille les événements système et informe les abonnés du système d’événement COM + de ces événements.
|   **Nom du service**    |   SENS
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="system-events-broker"></a>Service Broker pour les événements système             

| | |           
|---|---|       
|   **Description du service** |   Coordonne l’exécution du travail en arrière-plan pour les applications WinRT. Si ce service est arrêté ou désactivé, travail en arrière-plan ne peut pas être déclenché.
|   **Nom du service**    |   SystemEventsBroker
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   En dépit du fait que sa description implique seulement pour les applications WinRT, il est nécessaire pour le Planificateur de tâches, service broker d’infrastructure et d’autres composants internes.
|||         

<br />          

## <a name="task-scheduler"></a>Planificateur de tâches           

| | |           
|---|---|   
|   **Description du service** |   Permet à un utilisateur configurer et planifier des tâches automatisées sur cet ordinateur. Le service héberge également plusieurs tâches critiques pour le système Windows. Si ce service est arrêté ou désactivé, ces tâches ne seront pas s’exécuter sur leurs planifiée fois. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   Planification
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="tcpip-netbios-helper"></a>Assistance TCP/IP NetBIOS            

| | |           
|---|---|   
|   **Description du service** |   Prend en charge NetBIOS sur le service TCP/IP (NetBT) et la résolution de noms NetBIOS pour les clients sur le réseau, ce qui permet aux utilisateurs de partager des fichiers imprimer et ouvrir une session sur le réseau. Si ce service est arrêté, ces fonctions peuvent être indisponibles. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   lmhosts
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="telephony"></a>Telephony           

| | |           
|---|---|       
|   **Description du service** |   Fournit la prise en charge de l’API de téléphonie (TAPI) pour les programmes qui contrôlent les périphériques sur l’ordinateur local et, par le biais du réseau local, sur les serveurs qui exécutent également le service de téléphonie.
|   **Nom du service**    |   TapiSrv
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   La désactivation des sauts de RRAS
|||         

<br />          

## <a name="themes"></a>Thèmes           

| | |           
|---|---|
|   **Description du service** |   Fournit la gestion de thème l’expérience utilisateur.
|   **Nom du service**    |   Thèmes
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Automatique
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Impossible de définir les thèmes d’accessibilité lorsque ce service est désactivé.
|||         

<br />  

## <a name="tile-data-model-server"></a>Serveur de modèle de données de mosaïque           

| | |           
|---|---|   
|   **Description du service** |   Serveur de mosaïques pour les mises à jour de la vignette.
|   **Nom du service**    |   tiledatamodelsvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Automatique
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Démarrer les sauts de menu si ce service est désactivé.
|||         

<br />          

##  <a name="time-broker"></a>Service Broker de temps     

| | |           
|---|---|       
|   **Description du service** |   Coordonne l’exécution du travail en arrière-plan pour les applications WinRT. Si ce service est arrêté ou désactivé, travail en arrière-plan ne peut pas être déclenché.
|   **Nom du service**    |   TimeBrokerSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   En dépit du fait que sa description implique seulement pour les applications WinRT, il est nécessaire pour le Planificateur de tâches, service broker d’infrastructure et d’autres composants internes.
|||         

<br />          

## <a name="touch-keyboard-and-handwriting-panel-service"></a>Clavier tactile et Service de panneau de configuration de l’écriture manuscrite         

| | |           
|---|---|   
|   **Description du service** |   Active la fonctionnalité de stylet et entrée manuscrite clavier tactile et du volet de l’écriture manuscrite
|   **Nom du service**    |   TabletInputService
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

## <a name="update-orchestrator-service-for-windows-update"></a>Mettre à jour de Service Orchestrator pour la mise à jour de Windows           

| | |           
|---|---|       
|   **Description du service** |   Gère les mises à jour de Windows. Si arrêté, il se peut que vos appareils ne sera pas en mesure de télécharger et installer les dernières mises à jour.
|   **Nom du service**    |   UsoSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Description du service est manquante dans v1607 inclut ; Mise à jour de Windows (y compris WSUS) dépend de ce service.
|||         

<br />          

## <a name="upnp-device-host"></a>Hôte de périphérique UPnP         

| | |           
|---|---|   
|   **Description du service** |   Permet aux appareils UPnP être hébergé sur cet ordinateur. Si ce service est arrêté, tous les appareils UPnP hébergés cessera de fonctionner et aucun appareil hébergé supplémentaires ne peut être ajoutés. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   upnphost
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

## <a name="user-access-logging-service"></a>Service de journalisation des accès utilisateur          

| | |           
|---|---|   
|   **Description du service** |   Ce service enregistre les demandes d’accès client unique, sous la forme d’adresses IP et noms d’utilisateur, des produits installés et des rôles sur le serveur local. Ces informations peuvent être interrogées via Powershell, par les administrateurs de devoir quantifier à la demande du client de logiciel de serveur pour la gestion des licences d’accès Client (CAL) hors connexion. Si le service est désactivé, les demandes des clients ne seront pas enregistrés et ne seront plus récupérables via des requêtes de Powershell. L’arrêt du service n’affecte pas la requête de données d’historique (voir la documentation pour savoir comment supprimer les données historiques de prise en charge). L’administrateur système local doit consulter le contrat de licence de Windows Server, de son, pour déterminer le nombre de licences d’accès client qui sont requis pour le logiciel server sous licence ; utiliser le service de journalisation des accès utilisateur et les données ne modifie pas cette obligation.
|   **Nom du service**    |   UALSVC
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="user-data-access"></a>Accès de données utilisateur        

| | |           
|---|---|   
|   **Description du service** |   Fournit l’accès des applications aux données utilisateur structuré, y compris les informations de contact, des calendriers, des messages et d’autres contenus. Si vous arrêtez ou désactivez ce service, les applications qui utilisent ces données peuvent ne pas fonctionnent correctement.
|   **Nom du service**    |   UserDataSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Modèle de service utilisateur
|||         

<br />          

## <a name="user-data-storage"></a>Stockage des données utilisateur            

| | |           
|---|---|       
|   **Description du service** |   Gère le stockage des données structurées utilisateur, y compris les informations de contact, des calendriers, des messages et d’autres contenus. Si vous arrêtez ou désactivez ce service, les applications qui utilisent ces données peuvent ne pas fonctionnent correctement.
|   **Nom du service**    |   UnistoreSvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Modèle de service utilisateur
|||         

<br />          

## <a name="user-experience-virtualization-service"></a>Service de virtualisation d’expérience utilisateur           

| | |           
|---|---|       
|   **Description du service** |   Fournit la prise en charge pour les paramètres d’application et du système d’exploitation itinérance
|   **Nom du service**    |   UevAgentService
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Comments**    |   
|||         

<br />          

##  <a name="user-manager"></a>Gestionnaire des utilisateurs        

| | |           
|---|---|   
|   **Description du service** |   Gestionnaire des utilisateurs offre les composants d’exécution requis pour l’interaction des utilisateur multiples.  Si ce service est arrêté, certaines applications ne peuvent pas fonctionner correctement.
|   **Nom du service**    |   UserManager
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="user-profile-service"></a>Service de profil utilisateur         

| | |           
|---|---|   
|   **Description du service** |   Ce service est responsable du chargement et déchargement des profils utilisateur. Si ce service est arrêté ou désactivé, les utilisateurs ne seront plus en mesure de se connecter avec succès ou de se déconnecter, applications peuvent avoir des problèmes pour obtenir les données des utilisateurs et les composants inscrits pour recevoir des notifications d’événements de profil ne sont pas les recevoir.
|   **Nom du service**    |   ProfSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="virtual-disk"></a>Disque virtuel             

| | |           
|---|---|   
|   **Description du service** |   Fournit des services de gestion de disques, les volumes, les systèmes de fichiers et les baies de stockage.
|   **Nom du service**    |   vds
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Aucune recommandation
|   **Comments**    |   
|||         

<br />          

## <a name="volume-shadow-copy"></a>Cliché instantané de volume           

| | |           
|---|---|   
|   **Description du service** |   Gère et implémente des clichés instantanés de Volume utilisé pour la sauvegarde et d’autres fins. Si ce service est arrêté, les clichés instantanés ne seront pas disponibles pour la sauvegarde et la sauvegarde peut échouer. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   VSS
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Aucune recommandation
|   **Comments**    |   
|||         

<br />          

##  <a name="walletservice"></a>WalletService           

| | |           
|---|---|   
|   **Description du service** |   Objets hôtes utilisés par les clients de wallet
|   **Nom du service**    |   WalletService
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

## <a name="windows-audio"></a>Windows Audio            

| | |           
|---|---|       
|   **Description du service** |   Gère l’audio pour les programmes basés sur Windows.  Si ce service est arrêté, effets et périphériques audio fonctionnera pas correctement.  Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer
|   **Nom du service**    |   Audiosrv
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

## <a name="windows-audio-endpoint-builder"></a>Générateur de point de terminaison Audio de Windows           

| | |           
|---|---|
|   **Description du service** |   Gère les périphériques audio pour le service Windows Audio.  Si ce service est arrêté, effets et périphériques audio fonctionnera pas correctement.  Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer
|   **Nom du service**    |   AudioEndpointBuilder
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

## <a name="windows-biometric-service"></a>Service de biométrie Windows            

| | |           
|---|---|   
|   **Description du service** |   Le service de biométrie Windows permet aux applications clientes à capturer, de comparer, de manipuler et de stocker des données biométriques sans avoir directement accès à n’importe quel matériel biométrique ou exemples. Le service est hébergé dans un processus SVCHOST privilégié.
|   **Nom du service**    |   WbioSrvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="windows-camera-frame-server"></a>Serveur de Frame d’appareil photo Windows         

| | |           
|---|---|       
|   **Description du service** |   Permet à plusieurs clients à accéder à des images vidéo à partir d’appareils de l’appareil photo.
|   **Nom du service**    |   FrameServer
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

## <a name="windows-connection-manager"></a>Gestionnaire de connexions de Windows           

| | |           
|---|---|   
|   **Description du service** |   Décisions de connexion/déconnexion automatique rend basée sur les options de connectivité réseau actuellement disponibles sur le PC et permet la gestion de la connectivité réseau en fonction des paramètres de stratégie de groupe.
|   **Nom du service**    |   Wcmsvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="windows-defender-network-inspection-service"></a>Service d’Inspection réseau Windows Defender          

| | |           
|---|---|       
|   **Description du service** |   Vous aide à protéger contre les tentatives d’intrusion ciblant des vulnérabilités connues et nouvellement découvertes dans les protocoles réseau
|   **Nom du service**    |   WdNisSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation    
|   **Comments**    |   
|||         

<br />          

## <a name="windows-defender-service"></a>Service Windows Defender         

| | |           
|---|---|       
|   **Description du service** |   Vous aide à protéger les utilisateurs contre les logiciels malveillants et autres logiciels potentiellement indésirables
|   **Nom du service**    |   WinDefend
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Windows Driver Foundation - utilisateur-mode Driver Framework           

| | |           
|---|---|   
|   **Description du service** |   Crée et gère les processus de pilote en mode utilisateur. Ce service ne peut pas être arrêté.
|   **Nom du service**    |   wudfsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="windows-encryption-provider-host-service"></a>Service hôte de fournisseur de chiffrement Windows     

| | |           
|---|---|   
|   **Description du service** |   Service de hôte du fournisseur de chiffrement Windows courtiers liées au chiffrement fonctionnalités à partir de fournisseurs de chiffrement tiers aux processus nécessaires pour évaluer et appliquer des stratégies EAS. Un arrêt compromettra des vérifications de conformité EAS qui ont été établies par les comptes de messagerie connecté
|   **Nom du service**    |   WEPHOSTSVC
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="windows-error-reporting-service"></a>Service de rapport d'erreurs Windows          

| | |           
|---|---|       
|   **Description du service** |   Permet les erreurs soient signalées lorsque programmes cessent de fonctionner ou de répondre et des solutions existantes pour être remis. Permet également de journaux sont générées pour le diagnostic et de réparation des services. Si ce service est arrêté, le rapport d’erreurs peuvent ne pas fonctionne correctement et résultats des services de diagnostic et des réparations ne sont pas affichées.
|   **Nom du service**    |   WerSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Collecte et envoie les données d’incident/blocage utilisées par Microsoft et tiers ISV/IHV. Les données sont utilisées pour diagnostiquer des bogues de sources d’incident, qui peuvent inclure des bogues de sécurité. Également nécessaire pour le rapport d’erreurs d’entreprise
|||         

<br />          

## <a name="windows-event-collector"></a>Collecteur d'événements de Windows          

| | |           
|---|---|   
|   **Description du service** |   Ce service gère des abonnements persistants à des événements à partir de sources distantes qui prennent en charge le protocole WS-Management. Cela inclut les journaux des événements Windows Vista, matériel et les sources d’événements compatible IPMI. Le service stocke les événements transférés dans un journal des événements local. Si ce service est arrêté ou désactivé les abonnements aux événements ne peut pas être créés et événements transférés ne peut pas être acceptées.
|   **Nom du service**    |   Wecsvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Collecte les événements ETW (y compris les événements de sécurité) pour la gestion, de diagnostics.  Un grand nombre de fonctionnalités et des outils tiers s’appuient sur celui-ci, y compris les outils d’audit de sécurité
|||         

<br />          

## <a name="windows-event-log"></a>Journal des événements Windows            

| | |           
|---|---|       
|   **Description du service** |   Ce service gère les événements et journaux des événements. Il prend en charge la journalisation des événements, interrogation des événements, de s’abonner aux événements, l’archivage des journaux des événements et la gestion des métadonnées d’événement. Il peut afficher des événements au format XML et texte brut. Arrêt du service peut compromettre la sécurité et fiabilité du système.
|   **Nom du service**    |   EventLog
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="windows-firewall"></a>Pare-feu Windows         

| | |           
|---|---|   
|   **Description du service** |   Pare-feu de Windows vous aide à protéger votre ordinateur en empêchant les utilisateurs non autorisés d’accéder à votre ordinateur via Internet ou un réseau.
|   **Nom du service**    |   MpsSvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="windows-font-cache-service"></a>Service de Cache de polices Windows      

| | |           
|---|---|   
|   **Description du service** |   Optimise les performances des applications en mettant en cache les données de police fréquemment utilisées. Applications démarre ce service s’il n’est pas déjà en cours d’exécution. Il peut être désactivé, bien que cela entraîne la dégradation des performances de l’application.
|   **Nom du service**    |   FontCache
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="windows-image-acquisition-wia"></a>Acquisition d’images Windows (WIA)          

| | |           
|---|---|   
|   **Description du service** |   Fournit des services d’acquisition d’images pour les scanneurs et appareils photo
|   **Nom du service**    |   stisvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

##  <a name="windows-insider-service"></a>Windows Insider Service     

| | |           
|---|---|   
|   **Description du service** |   wisvc
|   **Nom du service**    |   wisvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Server ne gère pas de version d’évaluation, il est réalisé sur le serveur. Fonctionnalité peut également être désactivée via la stratégie de groupe.
|||         

<br />          

##  <a name="windows-installer"></a>Windows Installer       

| | |           
|---|---|
|   **Description du service** |   Ajoute, modifie et supprime des applications fournies en tant qu’un package Windows Installer (*.msi, *.msp). Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   MSIServer
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="windows-license-manager-service"></a>Service de gestionnaire de licences de Windows          

| | |           
|---|---|   
|   **Description du service** |   Fournit la prise en charge de l’infrastructure pour le Microsoft Store.  Ce service est démarré à la demande et si ensuite le contenu acquis via le Microsoft Store désactivé ne fonctionnera pas correctement.
|   **Nom du service**    |   LicenseManager
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="windows-management-instrumentation"></a>Windows Management Instrumentation       

| | |           
|---|---|       
|   **Description du service** |   Fournit un modèle d’interface et un objet commun pour accéder aux informations de gestion sur le système d’exploitation, appareils, applications et services. Si ce service est arrêté, la plupart des logiciels Windows ne fonctionnera pas correctement. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   WinMgmt
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

##  <a name="windows-mobile-hotspot-service"></a>Service de zone réactive mobiles Windows          

| | |           
|---|---|       
|   **Description du service** |   Offre la possibilité de partager une connexion de données cellulaires avec un autre appareil.
|   **Nom du service**    |   icssvc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   
|||         

<br />          

## <a name="windows-modules-installer"></a>Programme d’installation des Modules de Windows        

| | |           
|---|---|   
|   **Description du service** |   Permet l’installation, modification et suppression des mises à jour Windows et des composants facultatifs. Si ce service est désactivé, installer ou désinstaller des mises à jour peuvent échouer pour cet ordinateur de Windows.
|   **Nom du service**    |   TrustedInstaller
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="windows-push-notifications-system-service"></a>Service de système de Notifications Push Windows            

| | |           
|---|---|
|   **Description du service** |   Ce service s’exécute dans la session 0 et héberge le fournisseur de plateforme et de connexion de notification qui gère la connexion entre l’appareil et le serveur WNS.
|   **Nom du service**    |   WpnService
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Automatique
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Nécessaire pour les vignettes dynamiques et d’autres fonctionnalités
|||         

<br />      

## <a name="windows-push-notifications-user-service"></a>Service de Windows Push Notifications utilisateur          

| | |           
|---|---|   
|   **Description du service** |   Ce service héberge la plateforme de notification de Windows qui prend en charge locale et les notifications push. Prise en charge des notifications sont vignette, toast et raw.
|   **Nom du service**    |   WpnUserService
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   OK pour désactiver
|   **Comments**    |   Modèle de service utilisateur
|||         

<br />

## <a name="windows-remote-management-ws-management"></a>Gestion à distance de Windows (WS-Management)
| | |
|---|---|
|   **Description du service** |   Le service Gestion à distance de Windows (WinRM) implémente le protocole WS-Management pour la gestion à distance. WS-Management est un protocole des services web standard utilisé pour la gestion à distance de logiciels et de matériel. Le service Gestion à distance de Windows écoute sur le réseau les demandes WS-Management et les traite. Ce service doit être configuré avec un écouteur à l'aide de l'outil en ligne de commande winrm.cmd ou via la stratégie de groupe de façon à ce qu'il puisse être à l'écoute. Il permet d'accéder aux données WMI et de collecter des événements. La collecte d'événements et l'abonnement à des événements nécessitent que le service soit en cours d'exécution. Les messages du service Gestion à distance de Windows utilisent HTTP et HTTPS comme transport. Le service Gestion à distance de Windows ne dépend pas d'IIS mais il est préconfiguré pour partager un port avec IIS sur le même ordinateur.  Le service Gestion à distance de Windows réserve le préfixe d'URL /wsman. Pour éviter les conflits avec IIS, les administrateurs doivent vérifier que les sites web hébergés sur IIS n'utilisent pas le préfixe d'URL /wsman.
|   **Nom du service**    |   WinRM
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Nécessaire pour la gestion à distance
|||

<br />          

##  <a name="windows-search"></a>Windows Search      

| | |           
|---|---|       
|   **Description du service** |   Fournit l’indexation de contenu, la mise en cache de propriété et les résultats de recherche pour les fichiers, e-mails et autres contenus.
|   **Nom du service**    |   WSearch
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Comments**    |   
|||         

<br />          

##  <a name="windows-time"></a>Horloge Windows        

| | |           
|---|---|   
|   **Description du service** |   Assure la synchronisation de date et l’heure sur tous les clients et serveurs dans le réseau. Si ce service est arrêté, la date et l’heure de synchronisation n’est pas disponible. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   W32Time
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation
|   **Comments**    |   
|||         

<br />          

## <a name="windows-update"></a>Windows Update           

| | |           
|---|---|       
|   **Description du service** |   Permet la détection, le téléchargement et l’installation des mises à jour pour Windows et d’autres programmes. Si ce service est désactivé, les utilisateurs de cet ordinateur ne sera pas en mesure d’utiliser la mise à jour de Windows ou de sa fonctionnalité de mise à jour automatique, et les programmes ne seront pas en mesure d’utiliser l’API Windows Update Agent (WUA).
|   **Nom du service**    |   wuauserv
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="winhttp-web-proxy-auto-discovery-service"></a>Service de découverte automatique de Proxy Web de WinHTTP         

| | |           
|---|---|   
|   **Description du service** |   WinHTTP implémente la pile HTTP du client et fournit aux développeurs une API Win32 et le composant d’automatisation COM pour l’envoi de demandes HTTP et la réception de réponses. En outre, WinHTTP prend en charge de la découverte automatique d’une configuration de proxy via son implémentation du protocole Web Proxy Auto-Discovery (WPAD).
|   **Nom du service**    |   WinHttpAutoProxySvc
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Ne désactivez pas
|   **Comments**    |   Tout ce qui utilise la pile réseau peut avoir une dépendance fonctionnelle sur ce service. De nombreuses organisations s’appuient sur cette option pour configurer le proxy HTTP de leur réseau interne routage.  Sans cela, en interne d’origine des connexions HTTP à Internet tous échouera.
|||         

<br />          

## <a name="wired-autoconfig"></a>Configuration automatique de réseau câblé         

| | |           
|---|---|       
|   **Description du service** |   Le service Wired AutoConfig (DOT3SVC) est chargé d’effectuer IEEE 802. 1 X l’authentification sur les interfaces Ethernet. Si votre déploiement de réseau câblé actuel applique l’authentification de 802. 1 X, le service de DOT3SVC doit être configuré pour s’exécuter pour l’établissement d’une connectivité de couche 2 et/ou la fourniture d’un accès aux ressources réseau. Les réseaux câblés qui n’appliquent pas l’authentification de 802. 1 X ne sont pas affectés par le service DOT3SVC.
|   **Nom du service**    |   dot3svc
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation   
|   **Comments**    |   
|||         

<br />          

## <a name="wmi-performance-adapter"></a>Carte de Performance WMI          

| | |           
|---|---|   
|   **Description du service** |   Fournit des informations de bibliothèque de performances à partir de fournisseurs de Windows Management Instrumentation (WMI) pour les clients sur le réseau. Ce service s’exécute uniquement lors de la Performance Data Helper est activé.
|   **Nom du service**    |   wmiApSrv
|   **Installation**    |   Toujours installé
|   **StartType**   |   Manuelle
|   **Recommandation**  | Aucune recommandation       
|   **Comments**    |   
|||         

<br />          

## <a name="workstation"></a>station de travail          

| | |           
|---|---|   
|   **Description du service** |   Crée et gère les connexions réseau client aux serveurs distants à l’aide du protocole SMB. Si ce service est arrêté, ces connexions ne seront pas disponibles. Si ce service est désactivé, tout service qui en dépend explicitement échouera Démarrer.
|   **Nom du service**    |   LanmanWorkstation
|   **Installation**    |   Toujours installé
|   **StartType**   |   Automatique
|   **Recommandation**  | Aucune recommandation       
|   **Comments**    |   
|||         

<br />

## <a name="xbox-live-auth-manager"></a>Xbox Live Auth Manager           

| | |           
|---|---|   
|   **Description du service** |   Fournit des services d’authentification et d’autorisation permettant d’interagir avec Xbox Live. Si ce service est arrêté, certaines applications ne peuvent pas fonctionner correctement.
|   **Nom du service**    |   XblAuthManager
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Doit être désactivé
|   **Comments**    |   
|||         

<br />          

## <a name="xbox-live-game-save"></a>Xbox Live Game enregistrer          

| | |           
|---|---|   
|   **Description du service** |   Les synchronisations de ce service enregistrement des données pour Xbox Live enregistrer jeux est activées.  Si ce service est arrêté, jeu de données de sauvegarde ne sera pas charger vers ou télécharger à partir de la Xbox Live.
|   **Nom du service**    |   XblGameSave
|   **Installation**    |   Uniquement avec l’expérience utilisateur
|   **StartType**   |   Manuelle
|   **Recommandation**  |   Doit être désactivé
|   **Comments**    |   Les synchronisations de ce service enregistrement des données pour Xbox Live enregistrer jeux est activées.  Si ce service est arrêté, jeu de données de sauvegarde ne sera pas charger vers ou télécharger à partir de la Xbox Live.
|||         

<br /> 
<br /> 

