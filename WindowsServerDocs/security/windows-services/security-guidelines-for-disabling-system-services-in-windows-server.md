---
title: Recommandations en matière de sécurité pour les services système sous Windows Server 2016
description: Recommandations en matière de sécurité pour la désactivation des services sous Windows Server 2016 avec Expérience utilisateur
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/26/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: fe2f82c373b0014a3f385dcfad77ec11a0b1e6c0
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870213"
---
## <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>Conseils sur la désactivation des services système sous Windows Server 2016 avec Expérience utilisateur

S’applique à : Windows Server 2016

Le système d'exploitation Windows comprend de nombreux services système offrant d'importantes fonctionnalités. Ces services présentent différentes stratégies de démarrage par défaut : certains sont démarrés par défaut (automatiquement) ou si nécessaire (manuellement), et d'autres sont désactivés par défaut et doivent être explicitement activés pour être exécutés. Ces paramètres par défaut ont été soigneusement sélectionnés pour chaque service à des fins d'équilibre des performances, des fonctionnalités et de la sécurité pour les clients types.

Toutefois, certains clients d'entreprise peuvent préférer un équilibre plus centré sur la sécurité de leurs PC et serveurs Windows, réduisant ainsi leur surface d’attaque au strict minimum, et peuvent alors souhaiter désactiver complètement tous les services inutiles dans leurs environnements spécifiques. Microsoft® fournit à ces clients les instructions correspondantes pour leur permettre de connaître les services qui peuvent être désactivés en toute sécurité.

Ces instructions sont réservées à Windows Server 2016 avec Expérience utilisateur (sauf utilisé en tant que remplacement de bureau pour les utilisateurs finaux). À partir de Windows Server 2019, ces instructions sont configurées par défaut. Chaque service présent sur le système est catégorisé comme suit :

-   **Doit être désactivé :** Une entreprise centrée sur la sécurité préférera très certainement désactiver ce service et renoncer à ses fonctionnalités (voir détails supplémentaires ci-dessous).
- **Possibilité de désactivation :** Ce service fournit des fonctionnalités utiles à certaines entreprises. Les entreprises centrées sur la sécurité qui ne s’en servent pas peuvent les désactiver en toute sécurité.
- **Ne pas désactiver :** La désactivation de ce service a un impact sur les fonctionnalités essentielles ou empêche le bon fonctionnement de certains rôles ou fonctionnalités. Dès lors, il ne doit pas être désactivé.
-  **(Pas de recommandations) :** L’impact d'une désactivation de ces services n’a pas été entièrement évalué. Dès lors, la configuration par défaut de ces services ne doit pas être modifiée.


Les clients peuvent configurer leurs PC et serveurs Windows de manière à désactiver les services sélectionnés à l'aide des modèles de sécurité de leurs stratégies de groupe ou à l'aide de l'automatisation PowerShell. Dans certains cas, les instructions incluent des paramètres de stratégie de groupe spécifiques qui désactivent directement la fonctionnalité du service, au lieu de désactiver le service lui-même.

Microsoft recommande aux clients de désactiver les services suivants et leurs tâches planifiées respectives sur Windows Server 2016 avec Expérience utilisateur :

Services : 
1. Gestionnaire d'authentification Xbox Live
2. Jeu sauvegardé sur Xbox Live

Tâches planifiées : 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(Vous pouvez également accéder aux informations relatives à tous les services décrits dans cet article en consultant la feuille de calcul Microsoft Excel jointe : [Conseils sur la désactivation des services système sous Windows Server 2016 avec Expérience utilisateur](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx))

<br />

### <a name="disabling-services-not-installed-by-default"></a>Désactivation de services non installés par défaut

Microsoft recommande de ne pas appliquer de stratégies pour désactiver les services qui ne sont pas installés par défaut.
-  Le service est généralement nécessaire si la fonctionnalité est installée. L’installation du service ou de la fonctionnalité requiert des droits d’administration. Interdisez l'installation de la fonctionnalité, pas le démarrage du service.
-  Le blocage du service Microsoft Windows n’empêche pas un administrateur (ou un non-administrateur dans certains cas) d’installer un équivalent tiers similaire, pouvant présenter un risque de sécurité plus élevé.
-  Une ligne de base ou un point de référence désactivant un service Windows autre que par défaut (W3SVC, par exemple) donnera à certains auditeurs la fausse impression que la technologie (par exemple, IIS) est intrinsèquement non sécurisée et ne doit jamais être utilisée.
-  Si la fonctionnalité (et le service) n'est jamais installée, cela ne fait qu'ajouter un encombrement inutile à la ligne de base et au travail de vérification.

<br />
Pour tous les services système répertoriés dans ce document, les deux tableaux suivants fournissent une explication des colonnes et les recommandations de Microsoft pour activer et désactiver des services système dans Windows Server 2016 avec Expérience utilisateur : 

<br />

### <a name="explanation-of-columns"></a>Explication des colonnes

| | |
|---|---|
|**Description du service**|   Description du service, à partir de sc.exe qdescription.|
|**Nom** |Nom de la clé (interne) du service|
|**Installation** |Toujours installé : le service se trouve sur Server Core et Serveur avec Expérience utilisateur  <br /> Avec Expérience utilisateur uniquement : Le service se trouve sur Windows Server 2016 avec Expérience utilisateur, mais ***pas*** sur Server Core |
|**Type de démarrage**  |Type de démarrage du service sur Windows Server 2016|
|**Recommandation** |Recommandation/Conseil de Microsoft concernant la désactivation de ce service sous Windows Server 2016 dans un déploiement d'entreprise classique bien géré et lorsque le serveur n'est pas utilisé en tant que remplacement de bureau de l'utilisateur final.|
|**Commentaires** |Explication supplémentaire|

<br />

### <a name="explanation-of-microsoft-recommendations"></a>Explication des recommandations de Microsoft

| | |
|---|---|
|**Ne pas désactiver** |Le service ne doit pas être désactivé|
|**Possibilité de désactivation**| Ce service peut être désactivé si la fonctionnalité qu'il prend en charge n’est pas utilisée.|
|**Déjà désactivé**|  Ce service est désactivé par défaut et il n'est pas nécessaire d’appliquer la stratégie|
|**Doit être désactivé** |Ce service ne doit jamais être activé sur un système d’entreprise bien géré.|

<br />

Les tableaux suivants présentent des conseils Microsoft en matière de désactivation des services système sous Windows Server 2016 avec Expérience utilisateur :

<br />

##  <a name="activex-installer-axinstsv"></a>Programme d'installation ActiveX (AxInstSV)

| | |
|---|---|
|   **Description du service** |   Valide le contrôle de compte d’utilisateur pour l’installation des contrôles ActiveX depuis Internet et permet de gérer leur installation d’après les paramètres de la stratégie de groupe. Ce service est démarré à la demande et, s’il est désactivé, l’installation des contrôles ActiveX se comporte conformément aux paramètres par défaut du navigateur.    |
|   **Nom du service**    |   AxInstSV    |
|   **Installation**    |   Avec Expérience utilisateur uniquement    |
|   **Type de démarrage**   |   Manuelle  |
|   **Recommandation**  |   Possibilité de désactivation   |
|   **Commentaires**    |   Possibilité de désactiver la fonctionnalité, si inutile |


<br />

## <a name="alljoyn-router-service"></a>Service de routeur AllJoyn   

| | |
|---|---|
|   **Description du service** |   Achemine les messages AllJoyn pour les clients AllJoyn locaux. Si ce service est arrêté, les clients AllJoyn ne possédant pas leur propre routeur groupé ne peuvent pas s'exécuter. |
|   **Nom du service**    |   AJRouter    |
|   **Installation**    |   Avec Expérience utilisateur uniquement    |
|   **Type de démarrage**   |   Manuelle  |
|   **Recommandation**  | Pas de recommandations       |
|   **Commentaires**    |       |
| | |

<br />

## <a name="app-readiness"></a>Préparation des applications

| | |
|---|---|
**Description du service** |   Prépare les applications pour la première connexion d’un utilisateur sur cet ordinateur et lors de l’ajout de nouvelles applications.
**Nom du service**    |   AppReadiness
**Installation**    |   Avec Expérience utilisateur uniquement
**Type de démarrage**   |   Manuelle
**Recommandation**  |   Ne pas désactiver
**Commentaires**    |   
| | |

<br />

##  <a name="application-identity"></a>Identité de l'application

| | |       
|---|---|   
**Description du service** |   Détermine et vérifie l’identité d’une application. La désactivation de ce service empêchera l’application d’AppLocker.
**Nom du service**    |   AppIDSvc
**Installation**    |   Toujours installé
**Type de démarrage**   |   Manuelle
**Recommandation**  |Pas de recommandations    
**Commentaires**    |   
|||     

<br />

##  <a name="application-information"></a>Informations sur l'application 

| | |       
|---|---|   
|   **Description du service** |   Permet d’exécuter les applications interactives avec des privilèges administratifs supplémentaires.  Si ce service est arrêté, les utilisateurs ne pourront pas lancer les applications avec les privilèges administratifs supplémentaires nécessaires pour effectuer les tâches utilisateur souhaitées.
|   **Nom du service**    |   Appinfo
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   Prend en charge une élévation de même bureau UAC
|||     

<br />

##  <a name="application-layer-gateway-service"></a>Service de passerelle de la couche Application       

| | |           
|---|---|           
|   **Description du service** |   Fournit la prise en charge de plug-ins de protocole tiers pour le partage de connexion Internet
|   **Nom du service**    |   ALG
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |Pas de recommandations    
|   **Commentaires**    |   
|||     

<br />

##  <a name="application-management"></a>Gestion des applications      

| | |           
|---|---|       
|   **Description du service** |   Traite les demandes d’installation, de suppression et d’énumération pour le logiciel déployé au moyen de la stratégie de groupe. Si le service est désactivé, les utilisateurs ne pourront pas installer, supprimer ou énumérer le logiciel déployé au moyen de la stratégie de groupe. Si le service est désactivé, le démarrage des services qui en dépendent de façon explicite échoue.
|   **Nom du service**    |   AppMgmt
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="appx-deployment-service-appxsvc"></a>Service de déploiement AppX (AppXSVC)       

| | |           
|---|---|
|   **Description du service** |   Assure la prise en charge de l’infrastructure pour le déploiement d’applications du Microsoft Store. Ce service démarre à la demande. S’il est désactivé, les applications du Microsoft Store ne sont pas déployées sur le système et peuvent ne pas fonctionner correctement.
|   **Nom du service**    |   AppXSvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="auto-time-zone-updater"></a>Programme de mise à jour automatique du fuseau horaire           

| | |           
|---|---|           
|   **Description du service** |   Définit automatiquement le fuseau horaire système.
|   **Nom du service**    |   tzautoupdate
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         

<br />          

## <a name="background-intelligent-transfer-service"></a>Service de transfert intelligent en arrière-plan (BITS)          

| | |           
|---|---|   
|   **Description du service** |   Transfère des fichiers en arrière-plan en utilisant la bande passante réseau inactive. Si le service est désactivé, toutes les applications dépendant du service de transfert intelligent en arrière-plan, telles que Windows Update ou MSN Explorer, ne pourront plus télécharger des programmes ni d’autres informations.
|   **Nom du service**    |   BITS
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          


## <a name="background-tasks-infrastructure-service"></a>Service d’infrastructure des tâches en arrière-plan      

| | |           
|---|---|   
|   **Description du service** |   Service d’infrastructure Windows qui contrôle les tâches en arrière-plan qui peuvent s’exécuter sur le système.
|   **Nom du service**    |   BrokerInfrastructure
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="base-filtering-engine"></a>Moteur de filtrage de base            

| | |           
|---|---|       
|   **Description du service** |   Le moteur de filtrage de base est un service qui gère les stratégies de pare-feu et de sécurité IP (IPsec), et qui implémente le filtrage en mode utilisateur. L’arrêt ou la désactivation du service Moteur de filtrage de base diminue significativement la sécurité du système. Cela aboutit également à un fonctionnement imprévisible des applications de gestion et de pare-feu IPsec.
|   **Nom du service**    |   BFE
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="bluetooth-support-service"></a>Service de prise en charge Bluetooth            

| | |           
|---|---|   
|   **Description du service** |   Le service Bluetooth prend en charge la découverte et l’association d’appareils Bluetooth distants.  L’arrêt ou la désactivation de ce service peut entraîner un dysfonctionnement des appareils Bluetooth déjà installés et peut empêcher la découverte et l’association de nouveaux appareils.
|   **Nom du service**    |   bthserv
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Possibilité de désactivation, s'il n'est pas utilisé. Autre mécanisme de désactivation : https://technet.microsoft.com/library/dd252791.aspx
|||         

<br />          


## <a name="cdpusersvc"></a>CDPUserSvc           

| | |           
|---|---|   
|   **Description du service** |   Ce service utilisateur est utilisé pour les scénarios de plateforme d’appareils connectés
|   **Nom du service**    |   CDPUserSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Modèle de service utilisateur
|||         

<br />          


##  <a name="certificate-propagation"></a>Propagation du certificat     

| | |           
|---|---|
|   **Description du service** |   Copie des certificats utilisateur et des certificats racines à partir de cartes à puce dans le magasin de certificats de l’utilisateur actuel, détecte quand une carte à puce est insérée dans un lecteur de cartes à puce, et, si nécessaire, installe le minipilote Plug and Play de cartes à puce.
|   **Nom du service**    |   CertPropSvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="client-license-service-clipsvc"></a>Service de licences de client (ClipSVC)        

| | |           
|---|---|   
|   **Description du service** |   Permet la prise en charge de l'infrastructure du Microsoft Store. Ce service démarre à la demande. S'il est désactivé, les applications achetées via le Microsoft Store ne se comportent pas correctement.
|   **Nom du service**    |   ClipSVC
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="cng-key-isolation"></a>Isolation de clé CNG

| | |           
|---|---|   
|   **Description du service** |   Le service d’isolation de clé CNG est hébergé dans le processus LSA. Le service fournit une isolation de processus de clé aux clés privés et aux opérations de chiffrement associées, conformément aux critères communs. Le service stocke et utilise des clés à long terme dans le cadre d’un processus sécurisé répondant aux exigences des critères communs.
|   **Nom du service**    |   KeyIso
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="com-event-system"></a>Système d’événement COM+       

| | |           
|---|---|       
|   **Description du service** |   Prend en charge le service de notification d’événements système (SENS, System Event Notification Service), qui fournit une distribution automatique d’événements aux composants COM (Component Object Model) abonnés. Si le service est arrêté, SENS sera fermé et ne pourra fournir de notifications d’ouverture et de fermeture de session. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   EventSystem
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="com-system-application"></a>Application système COM+     

| | |           
|---|---|       
|   **Description du service** |   Gère la configuration et le suivi des composants de base COM+ (Component Object Model). Si le service est arrêté, la plupart des composants de base COM+ ne fonctionneront pas correctement. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   COMSysApp
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="computer-browser"></a>Explorateur d’ordinateurs        

| | |           
|---|---|       
|   **Description du service** |   Tient à jour une liste des ordinateurs présents sur le réseau et fournit cette liste aux ordinateurs désignés comme navigateurs. Si ce service est arrêté, la liste ne sera pas mise ou tenue à jour. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   Navigateur
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         

<br />          

## <a name="connected-devices-platform-service"></a>Service de plateforme des appareils connectés       

| | |           
|---|---|       
|   **Description du service** |   Ce service est utilisé pour les appareils connectés et les scénarios Universal Glass
|   **Nom du service**    |   CDPSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="connected-user-experiences-and-telemetry"></a>Expériences des utilisateurs connectés et télémétrie     

| | |           
|---|---|       
|   **Description du service** |   Le service d'expériences des utilisateurs connectés et de télémétrie offre des fonctionnalités qui prennent en charge les expériences des utilisateurs connectés et in-app. En outre, ce service gère la collecte et la transmission des informations de diagnostic et d'utilisation en fonction des événements (afin d'améliorer l'expérience et la qualité de la plateforme Windows) lorsque les paramètres des options de confidentialité des diagnostics et de l'utilisation sont activés sous Commentaires et Diagnostics.
|   **Nom du service**    |   DiagTrack
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="contact-data"></a>Données de contact        

| | |           
|---|---|       
|   **Description du service** |   Indexe les données des contacts pour la recherche rapide des contacts. Si vous arrêtez ou désactivez ce service, des contacts risquent de manquer dans vos résultats de recherche.
|   **Nom du service**    |   PimIndexMaintenanceSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Modèle de service utilisateur
|||         

<br />          

## <a name="coremessaging"></a>CoreMessaging            

| | |           
|---|---|           
|   **Description du service** |   Gère la communication entre les composants système.
|   **Nom du service**    |   CoreMessagingRegistrar
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="credential-manager"></a>Gestionnaire d’informations d’identification           

| | |           
|---|---|       
|   **Description du service** |   Offre un service de stockage et de récupération sécurisé des informations d’identification pour les utilisateurs, les applications et les packages de services de sécurité.
|   **Nom du service**    |   VaultSvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="cryptographic-services"></a>Services de chiffrement           

| | |           
|---|---|       
|   **Description du service** |   Fournit trois services de gestion : le service de base de données de catalogues, qui confirme les signatures des fichiers Windows et autorise l’installation de nouveaux programmes, le service de racine protégée, qui ajoute et supprime les certificats d’autorité de certification racine de confiance sur cet ordinateur, et le service de mise à jour de certificats racines automatique, qui extrait les certificats racines à partir de Windows Update et autorise les scénarios tels que SSL. Si ce service est arrêté, ces services de gestion ne fonctionneront pas correctement. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   CryptSvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="data-sharing-service"></a>Service de partage des données         

| | |           
|---|---|       
|   **Description du service** |   Fournit l'intermédiation des données entre les applications.
|   **Nom du service**    |   DsSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          

| | |           
|---|---|       
|   **Description du service** |   Le service DCP (Data Collection and Publishing) prend en charge les applications internes pour charger des données dans le cloud.
|   **Nom du service**    |   DcpSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="dcom-server-process-launcher"></a>Lanceur de processus serveur DCOM         

| | |           
|---|---|       
|   **Description du service** |   Le service DCOMLAUNCH lance les serveurs COM et DCOM en réponse aux demandes d’activation d’objets. Si ce service est arrêté ou désactivé, les programmes qui utilisent COM ou DCOM ne fonctionnent pas correctement. Il est fortement recommandé d’exécuter le service DCOMLAUNCH.
|   **Nom du service**    |   DcomLaunch
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |Pas de recommandations    
|   **Commentaires**    |   
|||         

<br />

##  <a name="device-association-service"></a>Service d’association d'appareil      

| | |           
|---|---|       
|   **Description du service** |   Permet de coupler des appareils câblés ou sans fil au système.
|   **Nom du service**    |   DeviceAssociationService
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />

##  <a name="device-install-service"></a>Service d’installation d'appareil

| | |
|---|---|
|   **Description du service** |   Permet à l’ordinateur de reconnaître et d’adapter les modifications matérielles avec peu ou pas du tout d’intervention de l’utilisateur. Arrêter ou désactiver ce service provoque une instabilité du système.
|   **Nom du service**    |   DeviceInstall
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations
|   **Commentaires**    |
|||

<br />          

##  <a name="device-management-enrollment-service"></a>Service d'inscription de la gestion des appareils        

| | |           
|---|---|       
|   **Description du service** |   Assure les activités d'inscription d'appareils pour la gestion des appareils
|   **Nom du service**    |   DmEnrollmentSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="device-setup-manager"></a>Gestionnaire d’installation d'appareil         

| | |           
|---|---|       
|   **Description du service** |   Active la détection, le téléchargement et l’installation du logiciel associé à l'appareil. Si ce service est désactivé, les appareils risquent d’être configurés avec un logiciel obsolète et de ne pas fonctionner correctement.
|   **Nom du service**    |   DsmSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="devquery-background-discovery-broker"></a>Service Broker de découverte en arrière-plan DevQuery         

| | |           
|---|---|           
|   **Description du service** |   Permet aux applications de découvrir les appareils avec une tâche en arrière-plan
|   **Nom du service**    |   DevQueryBroker
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="dhcp-client"></a>Client DHCP          

| | |           
|---|---|       
|   **Description du service** |   Inscrit et met à jour les adresses IP et les enregistrements DNS pour cet ordinateur. Si ce service est arrêté, cet ordinateur ne recevra pas d’adresses IP et de mises à jour DNS dynamiques. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   Dhcp
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="diagnostic-policy-service"></a>Service de stratégie de diagnostic            

| | |           
|---|---|       
|   **Description du service** |   Le service de stratégie de diagnostic permet la détection, le dépannage et la résolution de problèmes pour les composants de Windows.  Si ce service est arrêté, les diagnostics ne fonctionnent plus.
|   **Nom du service**    |   DPS
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="diagnostic-service-host"></a>Service hôte WDIServiceHost     

| | |           
|---|---|       
|   **Description du service** |   Le service hôte WDIServiceHost est utilisé par le Service de stratégie de diagnostics pour héberger les diagnostics qui doivent être exécutés dans un contexte de service local.  Si ce service est arrêté, tous les diagnostics qui en dépendent ne fonctionneront plus.
|   **Nom du service**    |   WdiServiceHost
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="diagnostic-system-host"></a>Hôte système de diagnostics           

| | |           
|---|---|       
|   **Description du service** |   Le service Hôte système de diagnostics est utilisé par le service de stratégie de diagnostic pour héberger les diagnostics qui doivent être exécutés dans un contexte de système local.  Si ce service est arrêté, tous les diagnostics qui en dépendent ne fonctionneront plus.
|   **Nom du service**    |   WdiSystemHost
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="distributed-link-tracking-client"></a>Client de suivi de liaisons distribuées            

| | |           
|---|---|   
|   **Description du service** |   Conserve les liaisons entre des fichiers NTFS au sein d’un ordinateur ou d’un ensemble d’ordinateurs en réseau.
|   **Nom du service**    |   TrkWks
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="distributed-transaction-coordinator"></a>Distributed Transaction Coordinator     

| | |           
|---|---|   
|   **Description du service** |   Coordonne les transactions qui comportent plusieurs gestionnaires de ressources, tels que des bases de données, des files d’attente de messages et des systèmes de fichiers. Si ce service est arrêté, ces transactions échoueront. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   MSDTC
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />  

##  <a name="dmwappushsvc"></a>dmwappushsvc        

| | |           
|---|---|       
|   **Description du service** |   Service de routage de notification Push WAP
|   **Nom du service**    |   dmwappushservice
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Service requis sur les appareils clients pour Intune, GPM et les technologies de gestion similaires, ainsi que pour le Filtre d'écriture unifié. Non requis pour le serveur.
|||         

<br />      

##  <a name="dns-client"></a>Client DNS      

| | |           
|---|---|       
|   **Description du service** |   Le service Client DNS (dnscache) met en cache les noms DNS (Domain Name System) et inscrit le nom complet de cet ordinateur. Si le service est arrêté, la résolution des noms DNS se poursuit. Toutefois, les résultats des requêtes de nom DNS ne sont pas mis en cache et le nom des ordinateurs n'est pas inscrit. Si le service est désactivé, le démarrage des services qui en dépendent de façon explicite échoue.
|   **Nom du service**    |   Dnscache
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="downloaded-maps-manager"></a>Gestionnaire des cartes téléchargées     

| | |           
|---|---|   
|   **Description du service** |   Service Windows pour l’accès des applications à des cartes téléchargées. Ce service est démarré à la demande par les applications accédant à des cartes téléchargées. La désactivation de ce service empêche les applications d’accéder à des cartes.
|   **Nom du service**    |   MapsBroker
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   La désactivation compromet les applications qui utilisent le service ; possibilité de désactivation si les applications ne l'utilisent pas
|||         

<br />          

## <a name="embedded-mode"></a>Mode incorporé            

| | |           
|---|---|       
|   **Description du service** |   Le service Mode incorporé active les scénarios associés aux applications d’arrière-plan.  La désactivation de ce service empêche l’activation de ces applications d'arrière-plan.
|   **Nom du service**    |   embeddedmode
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="encrypting-file-system-efs"></a>Système de fichiers EFS (Encrypting File System)

| | |                   
|---|---|           
|   **Description du service** | Fournit la technologie de chiffrement des fichiers de base utilisée pour stocker les fichiers chiffrés sur les volumes NTFS. Si ce service est arrêté ou désactivé, les applications n’auront pas accès aux fichiers chiffrés.            
|   **Nom du service**  |  EFS            
|   **Installation**  |  Toujours installé           
|   **Type de démarrage**   |  Manuelle           
|   **Recommandation**  | Pas de recommandations           
|   **Commentaires**   |
|||                 

<br />  

## <a name="enterprise-app-management-service"></a>Service de gestion des applications d'entreprise            

| | |           
|---|---|       
|   **Description du service** |   Permet la gestion des applications d'entreprise.
|   **Nom du service**    |   EntAppSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="extensible-authentication-protocol"></a>Protocole EAP (Extensible Authentication Protocol)           

| | |           
|---|---|   
|   **Description du service** |   Le service EAP (Extensible Authentication Protocol) permet d’authentifier le réseau dans des scénarios tels que des réseaux câblés et sans fil 802.1x, des réseaux privés virtuels et NAP (Network Access Protection).  Ce service fournit également des API qui sont utilisées par les clients d’accès à distance, notamment les clients sans fil et VPN, lors de l’authentification.  Si vous désactivez ce service, cet ordinateur ne pourra pas accéder aux réseaux qui nécessitent l’authentification EAP.
|   **Nom du service**    |   EapHost
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="function-discovery-provider-host"></a>Hôte du fournisseur de découverte de fonctions         

| | |           
|---|---|       
|   **Description du service** |   Le service FDPHOST héberge les fournisseurs de découverte de réseau de découverte de fonction (FD). Ces fournisseurs de découverte de fonction fournissent des services de découverte de réseau pour les protocoles SSDP (Simple Services Discovery Protocol) et WS-D (Web Services - Discovery). L’arrêt ou la désactivation du service FDPHOST désactivera la découverte de réseau pour ces protocoles lors de l’utilisation de la découverte de fonction. Lorsque ce service n’est pas disponible, les services réseau qui utilisent la découverte de fonction et qui dépendent de ces protocoles de découverte ne pourront pas trouver des appareils ou des ressources réseau.
|   **Nom du service**    |   fdPHost
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="function-discovery-resource-publication"></a>Publication des ressources de découverte de fonctions      

| | |           
|---|---|       
|   **Description du service** |   Publie cet ordinateur et les ressources qui y sont attachées, de façon à ce que leur découverte soit possible sur le réseau.  Si ce service est arrêté, les ressources réseau ne sont plus publiées et ne seront donc pas découvertes par les autres ordinateurs du réseau.
|   **Nom du service**    |   FDResPub
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="geolocation-service"></a>Service de géolocalisation          

| | |           
|---|---|       
|   **Description du service** |   Ce service surveille l'emplacement actuel du système et gère les limites géographiques (un emplacement géographique accompagné d'événements associés).  Si vous désactivez ce service, les applications ne pourront pas utiliser ni recevoir de notification pour la géolocalisation ou des limites géographiques.
|   **Nom du service**    |   lfsvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   La désactivation compromet les applications qui utilisent le service ; possibilité de désactivation si les applications ne l'utilisent pas
|||         

<br />          

##  <a name="group-policy-client"></a>Client de stratégie de groupe     

| | |           
|---|---|       
|   **Description du service** |   Le service est responsable de l’application des paramètres configurés par les administrateurs pour l’ordinateur et pour les utilisateurs via le composant de stratégie de groupe. Si le service est désactivé, les paramètres ne seront pas appliqués, et les applications et les composants ne seront pas gérables par la stratégie de groupe. Tout composant ou application qui dépend du composant de stratégie de groupe risque de ne pas fonctionner si le service est désactivé.
|   **Nom du service**    |   gpsvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          


## <a name="human-interface-device-service"></a>Service du périphérique d’interface utilisateur           

| | |           
|---|---|       
|   **Description du service** |   Active et maintient l’utilisation des boutons actifs sur le clavier, les contrôles à distance et d’autres périphériques multimédias. Nous vous recommandons de veiller à ce que ce service soit constamment en cours d’exécution.
|   **Nom du service**    |   hidserv
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="hv-host-service"></a>Service d'hôte HV     

| | |           
|---|---|   
|   **Description du service** |   Propose une interface pour l’hyperviseur Hyper-V afin de fournir des compteurs de performance par partition au système d’exploitation hôte.
|   **Nom du service**    |   HvHost
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Optimiseurs de niveau de performance pour les machines virtuelles invitées. Non utilisé aujourd'hui, sauf pour les machines virtuelles explicitement remplies, mais utilisé dans Application Guard
|||         

<br />          

## <a name="hyper-v-data-exchange-service"></a>Service Échange de données Microsoft Hyper-V        

| | |           
|---|---|   
|   **Description du service** |   Propose un mécanisme d’échange de données entre l’ordinateur virtuel et le système d’exploitation exécuté sur l’ordinateur physique.
|   **Nom du service**    |   vmickvpexchange
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Voir HvHost
|||         

<br />      

## <a name="hyper-v-guest-service-interface"></a>Interface de services d’invité Hyper-V          

| | |           
|---|---|   
|   **Description du service** |   Fournit une interface à l’hôte Hyper-V afin qu’il puisse interagir avec des services spécifiques s’exécutant sur la machine virtuelle.
|   **Nom du service**    |   vmicguestinterface
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Voir HvHost
|||         

<br />  

## <a name="hyper-v-guest-shutdown-service"></a>Service Arrêt de l’invité Microsoft Hyper-V           

| | |           
|---|---|       
|   **Description du service** |   Propose un mécanisme permettant d’arrêter le système d’exploitation de cet ordinateur virtuel à partir des interfaces de gestion de l’ordinateur physique.
|   **Nom du service**    |   vmicshutdown
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Voir HvHost
|||         

<br />

## <a name="hyper-v-heartbeat-service"></a>Service Pulsation Microsoft Hyper-V
| | |
|---|---|
|   **Description du service** |   Surveille l‘état de cet ordinateur virtuel en émettant une pulsation à intervalles réguliers. Ce service vous permet d’identifier les ordinateurs virtuels en cours d’exécution qui ne répondent plus.
|   **Nom du service**    |   vmicheartbeat
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Voir HvHost
|||

<br />          

## <a name="hyper-v-powershell-direct-service"></a>Service PowerShell Direct Hyper-V            

| | |           
|---|---|       
|   **Description du service** |   Fournit un mécanisme de gestion des machines virtuelles avec PowerShell via une session de machine virtuelle sans réseau virtuel.
|   **Nom du service**    |   vmicvmsession
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Voir HvHost
|||         

<br />          

## <a name="hyper-v-remote-desktop-virtualization-service"></a>Service de virtualisation Bureau à distance Hyper-V            

| | |           
|---|---|       
|   **Description du service** |   Fournit une plateforme pour la communication entre l’ordinateur virtuel et le système d’exploitation exécuté sur l’ordinateur physique.
|   **Nom du service**    |   vmicrdv
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Voir HvHost
|||         

<br />          

## <a name="hyper-v-time-synchronization-service"></a>Service Synchronisation date/heure Microsoft Hyper-V         

| | |           
|---|---|       
|   **Description du service** |   Synchronise l’heure système de cet ordinateur virtuel avec l’heure système de l’ordinateur physique.
|   **Nom du service**    |   vmictimesync
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Voir HvHost
|||         

<br />          

## <a name="hyper-v-volume-shadow-copy-requestor"></a>Requête du service VSS Microsoft Hyper-V         

| | |           
|---|---|           
|   **Description du service** |   Coordonne les communications nécessaires à l’utilisation du service VSS afin de sauvegarder les applications et les données de cet ordinateur virtuel à partir du système d’exploitation de l’ordinateur physique.
|   **Nom du service**    |   vmicvss
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Voir HvHost
|||         

<br />          

## <a name="ike-and-authip-ipsec-keying-modules"></a>Modules de génération de clés IKE et AuthIP          

| | |           
|---|---|       
|   **Description du service** |   Le service IKEEXT héberge les modules de génération de clés IKE (Internet Key Exchange) et AuthIP (Authenticated Internet Protocol). Ces modules de génération de clés sont utilisés pour l’authentification et l’échange de clés dans la sécurité IP (IPsec). L’arrêt ou la désactivation du service IKEEXT entraînera la désactivation de l’échange de clés IKE et AuthIP avec des ordinateurs homologues. IPsec est généralement configuré pour utiliser IKE ou AuthIP ; par conséquent, l’arrêt ou la désactivation du service IKEEXT risque de conduire à la défaillance d’IPsec et de compromettre la sécurité du système. Il est fortement recommandé d’activer l’exécution du service IKEEXT.
|   **Nom du service**    |   IKEEXT
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |    
|||         

<br />          

## <a name="interactive-services-detection"></a>Détection de services interactifs           

| | |           
|---|---|   
|   **Description du service** |   Active la notification des entrées utilisateur pour les services interactifs, qui active l’accès aux boîtes de dialogue créées par les services interactifs lorsqu’elles apparaissent. Si ce service est arrêté, les notifications de nouvelles boîtes de dialogue de services interactifs ne fonctionneront plus et l’accès aux boîtes de dialogue des services interactifs sera peut-être perdu. Si ce service est désactivé, les notifications des nouvelles boîtes de dialogue de services interactifs ainsi que leur accès ne fonctionneront plus.
|   **Nom du service**    |   UI0Detect
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />  

## <a name="internet-connection-sharing-ics"></a>Partage de connexion Internet (ICS)            

| | |           
|---|---|           
|   **Description du service** |   Assure la traduction d’adresses de réseau, l’adressage, les services de résolution de noms et/ou les services de prévention d’intrusion pour un réseau de petite entreprise ou un réseau domestique.
|   **Nom du service**    |   SharedAccess
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Requis pour les clients utilisés comme points d’accès WiFi, de même qu'aux deux extrémités de la projection Miracast. Le partage de connexion Internet peut être bloqué avec le paramètre de stratégie de groupe, « Interdire l’utilisation du partage de connexion Internet sur votre réseau de domaine DNS »
|||         

<br />          

## <a name="ip-helper"></a>Assistance IP            

| | |           
|---|---|       
|   **Description du service** |   Fournit une connectivité de tunnel à l’aide des technologies de transition IPv6 (6to4, ISATAP, Proxy de port et Teredo) et IP-HTTPS. Si ce service est arrêté, l’ordinateur ne disposera pas des avantages de la connectivité améliorée offerts par ces technologies.
|   **Nom du service**    |   iphlpsvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          


##  <a name="ipsec-policy-agent"></a>Agent de stratégie IPSec      

| | |           
|---|---|       
|   **Description du service** |   La sécurité du protocole Internet (IPSec) prend en charge l’authentification de l’homologue réseau, l’authentification de l’origine des données, l’intégrité des données, la confidentialité des données (chiffrement) et la protection de la relecture.  Ce service met en œuvre des stratégies IPSec créées via le composant logiciel enfichable Stratégies de sécurité IP ou l’outil de ligne de commande « netsh ipsec ».  Si vous arrêtez ce service, vous pourrez rencontrer des problèmes de connectivité réseau si votre stratégie nécessite des connexions IPSec.  En outre, la gestion à distance du Pare-feu Windows n’est pas disponible lorsque ce service est arrêté.
|   **Nom du service**    |   PolicyAgent
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />

##  <a name="kdc-proxy-server-service-kps"></a>Service Serveur proxy KDC      

| | |           
|---|---|       
|   **Description du service** |   Le service Serveur proxy KDC s’exécute sur les serveurs Edge pour permettre la redirection via proxy des messages du protocole Kerberos vers les contrôleurs de domaine sur le réseau de l’entreprise.
|   **Nom du service**    |   KPSSVC
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations    
|   **Commentaires**    |   
|||         

<br />          

## <a name="ktmrm-for-distributed-transaction-coordinator"></a>Service KtmRm pour Distributed Transaction Coordinator            

| | |           
|---|---|       
|   **Description du service** |   Coordonne les transactions entre Distributed Transaction Coordinator (MSDTC) et le gestionnaire de transactions du noyau (KTN). S’il n’est pas requis, il est recommandé que ce service reste arrêté. S’il est requis, MSDTC et KTM démarrent ce service automatiquement. Si ce service est désactivé, toute transaction MSDTC interagissant avec un gestionnaire de transactions du noyau échoue et tout service qui en dépend de façon explicite ne parvient pas à démarrer.
|   **Nom du service**    |   KtmRm
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />

##  <a name="link-layer-topology-discovery-mapper"></a>Mappage de découverte de topologie de la couche de liaison        

| | |       
|---|---|       
|   **Description du service** |   Crée un mappage réseau, consistant en informations sur la topologie des ordinateurs et des appareils (connectivité) et en métadonnées décrivant chaque ordinateur et chaque appareil.  Si ce service est désactivé, le mappage réseau ne fonctionne pas correctement.
|   **Nom du service**    |   lltdsvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Possibilité de désactivation en l'absence de dépendance sur le mappage réseau
|||         

<br />

## <a name="local-session-manager"></a>Gestionnaire de session locale                    

| | |                   
|---|---|   
|   **Description du service** |   Service Windows de base gérant les sessions locales utilisateur. Arrêter ou désactiver ce service provoque une instabilité du système.    
|   **Nom du service**    |   LSM |
|   **Installation**    |   Toujours installé    |
|   **Type de démarrage**   |   Automatique   |
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||                 

<br />                  

## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Service Collecteur standard du concentrateur de diagnostic Microsoft (R)         

| | |           
|---|---|           
|   **Description du service** |   Service Windows de base gérant les sessions locales utilisateur. Arrêter ou désactiver ce service provoque une instabilité du système.
|   **Nom du service**    |   LSM
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />

## <a name="microsoft-account-sign-in-assistant"></a>Assistant Connexion avec un compte Microsoft
| | |
|---|---|
|   **Description du service** |   Autorise la connexion des utilisateurs par le biais des services d’identité de compte Microsoft. Si ce service est arrêté, les utilisateurs ne pourront pas ouvrir une session sur l’ordinateur avec leur compte Microsoft.
|   **Nom du service**    |   wlidsvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Les comptes Microsoft sont sans objet sur Windows Server
|||

<br />          

##  <a name="microsoft-app-v-client"></a>Client Microsoft App-V      

| | |           
|---|---|       
|   **Description du service** |   Gère les utilisateurs App-V et les applications virtuelles
|   **Nom du service**    |   AppVClient
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         

<br />          

## <a name="microsoft-iscsi-initiator-service"></a>Service Initiateur iSCSI de Microsoft            

| | |           
|---|---|       
|   **Description du service** |   Gère les sessions iSCSI (Internet SCSI) à partir de cet ordinateur sur les appareils cibles iSCSI distants. Si ce service est arrêté, l’ordinateur ne pourra plus ouvrir de session sur les cibles iSCSI ni y accéder. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   MSiSCSI
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Nos données de diagnostic indiquent une utilisation sur le client, ainsi que sur le serveur. Il n'est pas utile de désactiver ce service.
|||         

<br />          

## <a name="microsoft-passport"></a>Microsoft Passport           

| | |           
|---|---|   
|   **Description du service** |   Assure l'isolation des processus pour les clés de chiffrement servant à l'authentification auprès des fournisseurs d'identité associés à un utilisateur. Si ce service est désactivé, les fonctions d'utilisation et de gestion de ces clés, telles que l'ouverture de session et l'authentification unique pour les applications et les sites web, ne seront pas disponibles. Ce service démarre et s'arrête automatiquement. Il est recommandé de ne pas le reconfigurer.
|   **Nom du service**    |   NgcSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Requis pour les ouvertures de session PIN/Hello non prises en charge sur le serveur
|||         

<br />          

## <a name="microsoft-passport-container"></a>Conteneur Microsoft Passport         

| | |           
|---|---|       
|   **Description du service** |   Gère les clés d'identité d'utilisateur local servant à authentifier l'utilisateur auprès des fournisseurs d'identité, ainsi que les cartes à puce virtuelles du module de plateforme sécurisée (TPM). Si ce service est désactivé, les clés d'identité d'utilisateur local et les cartes à puce virtuelles TPM ne seront pas accessibles. Nous vous recommandons de ne pas reconfigurer ce service.
|   **Nom du service**    |   NgcCtnrSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

## <a name="microsoft-software-shadow-copy-provider"></a>Fournisseur de cliché instantané de logiciel Microsoft          

| | |           
|---|---|       
|   **Description du service** |   Gère les copies logicielles de clichés instantanés de volumes créés par le service de cliché instantané de volumes. Si ce service est arrêté, les copies logicielles de clichés instantanés ne peuvent pas être gérées. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   swprv
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="microsoft-storage-spaces-smp"></a>SMP de l’Espace de stockages Microsoft         

| | |           
|---|---|       
|   **Description du service** |   Service hôte du fournisseur de gestion de l’Espace de stockages Microsoft. Si ce service est arrêté ou désactivé, l’Espace de stockages ne peut pas être géré.
|   **Nom du service**    |   smphost
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Sans ce service, les API de gestion du stockage échouent. Exemple : « Get-WmiObject -class MSFT_Disk -Namespace Root\Microsoft\Windows\Storage ».
|||         

<br />          

## <a name="nettcp-port-sharing-service"></a>Service Partage de port Net.Tcp         

| | |           
|---|---|       
|   **Description du service** |   Permet de partager des ports TCP sur le protocole net.tcp.
|   **Nom du service**    |   NetTcpPortSharing
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         

<br />          

## <a name="netlogon"></a>Netlogon         

| | |           
|---|---|           
|   **Description du service** |   Maintient un canal sécurisé entre cet ordinateur et le contrôleur de domaine pour authentifier les utilisateurs et les services. Si ce service est arrêté, l’ordinateur peut ne pas authentifier les utilisateurs et les services et le contrôleur de domaine ne peut pas inscrire les enregistrements DNS. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   Netlogon
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="network-connection-broker"></a>Service Broker pour les connexions réseau            

| | |           
|---|---|       
|   **Description du service** |   Connexions du service Broker qui permettent aux applications du Microsoft Store de recevoir des notifications d’Internet.
|   **Nom du service**    |   NcbService
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

##  <a name="network-connections"></a>Connexions réseau         

| | |           
|---|---|   
|   **Description du service** |   Prend en charge les objets dans le dossier Connexions réseau et accès à distance, dans lequel vous pouvez afficher à la fois les connexions du réseau local et les connexions à distance.
|   **Nom du service**    |   Netman
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="network-connectivity-assistant"></a>Assistant Connectivité réseau      

| | |           
|---|---|       
|   **Description du service** |   Fournit la notification du statut DirectAccess aux composants de l’interface utilisateur
|   **Nom du service**    |   NcaSvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />  

##  <a name="network-list-service"></a>Service Liste des réseaux        

| | |           
|---|---|   
|   **Description du service** |   Identifie les réseaux auxquels l’ordinateur s’est connecté, collecte et stocke les propriétés de ces réseaux, et signale aux applications toute modification des propriétés.
|   **Nom du service**    |   netprofm
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="network-location-awareness"></a>Connaissance des emplacements réseau           

| | |           
|---|---|       
|   **Description du service** |   Collecte et stocke les informations de configuration du réseau, puis notifie les programmes en cas de modification de ces informations. Si ce service est arrêté, les informations de configuration risquent de ne pas être disponibles. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   NlaSvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="network-setup-service"></a>Service Configuration du réseau       

| | |           
|---|---|       
|   **Description du service** |   Le service Configuration du réseau gère l'installation des pilotes réseau et autorise la configuration de paramètres réseau de bas niveau.  Si ce service est interrompu, il se peut que les installations de pilote en cours soient annulées.
|   **Nom du service**    |   NetSetupSvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="network-store-interface-service"></a>Service Interface du magasin réseau      

| | |           
|---|---|   
|   **Description du service** |   Ce service fournit des notifications réseau (ajout/suppression d’interface, etc.) aux clients en mode utilisateur. L’arrêt de ce service entraîne la perte de la connectivité réseau. Si ce service est désactivé, les autres services qui en dépendent explicitement ne pourront pas démarrer.
|   **Nom du service**    |   nsi
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="offline-files"></a>Fichiers hors connexion            

| | |           
|---|---|       
|   **Description du service** |   Le service Fichiers hors connexion effectue des activités de maintenance sur le cache Fichiers hors connexion, répond aux événements d’ouverture et de fermeture de session des utilisateurs, implémente les données internes de l’API public, et distribue aux utilisateurs intéressés les événements pertinents relatifs aux activités Fichiers hors connexion et aux modifications apportées en mémoire cache.
|   **Nom du service**    |   CscService
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         

<br />          

## <a name="optimize-drives"></a>Optimiser les lecteurs          

| | |           
|---|---|   
|   **Description du service** |   Permet à l’ordinateur de fonctionner plus efficacement en optimisant les fichiers sur les lecteurs de stockage.
|   **Nom du service**    |   defragsvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />

## <a name="performance-counter-dll-host"></a>Hôte de DLL de compteur de performance         

| | |           
|---|---|       
|   **Description du service** |   Permet aux utilisateurs à distances et aux processus 64 bits d’interroger les compteurs de performances fournis par des DLL 32 bits. Si ce service est arrêté, seuls les utilisateurs locaux et les processus 32 bits peuvent interroger les compteurs fournis par des DLL 32 bits.
|   **Nom du service**    |   PerfHost
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations    
|   **Commentaires**    |   
|||         

<br />          

## <a name="performance-logs--alerts"></a>Journaux et alertes de performance            

| | |           
|---|---|   
|   **Description du service** |   Le service des journaux et des alertes de performance collecte des données de performance sur des ordinateurs locaux ou distants, en se basant sur des paramètres de planification préconfigurés, puis écrit ces données dans un journal ou déclenche une alerte. Si ce service est arrêté, les informations de performance ne seront plus collectées. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   pla
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="phone-service"></a>Service téléphonique       

| | |           
|---|---|   
|   **Description du service** |   Gère l'état de téléphonie de l'appareil
|   **Nom du service**    |   PhoneSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Utilisé par les applications VoIP modernes
|||         

<br />          

##      <a name="plug-and-play"></a>Plug-and-play       

| | |           
|---|---|   
|   **Description du service** |   Permet à l’ordinateur de reconnaître et d’adapter les modifications matérielles avec peu ou pas du tout d’intervention de l’utilisateur. Arrêter ou désactiver ce service provoque une instabilité du système.
|   **Nom du service**    |   PlugPlay
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="portable-device-enumerator-service"></a>Service Énumérateur d’appareil mobile           

| | |           
|---|---|       
|   **Description du service** |   Met en place une stratégie de groupe pour les dispositifs de stockage de masse amovibles. Permet à des applications telles que le Lecteur Windows Media et l’Assistant Importation d’images de transférer et de synchroniser du contenu à l’aide de dispositifs de stockage de masse amovibles.
|   **Nom du service**    |   WPDBusEnum
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="power"></a>Alimentation            

| | |           
|---|---|       
|   **Description du service** |   Gère la stratégie d’alimentation et la remise de notification de stratégie d’alimentation.
|   **Nom du service**    |   Alimentation
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="print-spooler"></a>Spouleur d’impression            

| | |           
|---|---|   
|   **Description du service** |   Ce service met en spoule les travaux d’impression et gère l’interaction avec l’imprimante.  Si vous désactivez ce service, vous ne pourrez plus imprimer ni voir vos imprimantes.
|   **Nom du service**    |   Spouleur
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |   Possibilité de désactivation s'il ne s'agit pas d'un serveur d'impression ou d'un contrôleur de domaine
|   **Commentaires**    |   Sur un contrôleur de domaine, l’installation du rôle de contrôleur de domaine ajoute un thread au service de spouleur chargé d’effectuer le nettoyage de l’impression – suppression des objets obsolètes de la file d'attente à l'impression dans Active Directory.  Si le service de spouleur n’est pas en cours d’exécution sur au moins un contrôleur de domaine dans chaque site, Active Directory n'est pas en mesure de supprimer les files d’attente qui n’existent plus. https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/
|||         

<br />          

##  <a name="printer-extensions-and-notifications"></a>Extensions et notifications des imprimantes        

| | |           
|---|---|       
|   **Description du service** |   Ce service ouvre les boîtes de dialogue d’impression personnalisée et gère les notifications d’une imprimante ou d’un serveur d’impression distant. Si vous désactivez ce service, vous ne pourrez plus voir les extensions ou notifications relatives aux imprimantes.
|   **Nom du service**    |   PrintNotify
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation s'il ne s'agit pas d'un serveur d’impression
|   **Commentaires**    |   
|||         

<br />          

##  <a name="problem-reports-and-solutions-control-panel-support"></a>Prise en charge de l’application Rapports et solutions aux problèmes du Panneau de configuration     

| | |           
|---|---|   
|   **Description du service** |   Ce service prend en charge l’affichage, l’envoi et la suppression des rapports à l’échelle du système pour l’application Rapports et solutions aux problèmes du Panneau de configuration.
|   **Nom du service**    |   wercplsupport
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="program-compatibility-assistant-service"></a>Service de l’Assistant Compatibilité des programmes     

| | |           
|---|---|       
|   **Description du service** |   Ce service fournit une prise en charge de l’Assistant Compatibilité des programmes.  Cet Assistant surveille les programmes installés et exécutés par l’utilisateur et détecte les problèmes de compatibilité connus. Si ce service est arrêté, cet Assistant ne fonctionnera pas correctement.
|   **Nom du service**    |   PcaSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

##  <a name="quality-windows-audio-video-experience"></a>Expérience audio-vidéo haute qualité Windows      

| | |           
|---|---|   
|   **Description du service** |   qWave (Quality Windows Audio Video Experience) est une plateforme réseau destinée aux applications de diffusion en continu AV (Audio Video) sur des réseaux domestiques IP. qWave améliore les performances et la fiabilité des flux AV en assurant la qualité de service (QoS) sur le réseau des applications AV. Cette plateforme fournit des mécanismes concernant le contrôle d’admission, l’analyse et la mise en œuvre du temps d'exécution, la rétroaction des applications et la définition des priorités du trafic.
|   **Nom du service**    |   QWAVE
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Service QoS côté client
|||         

<br />          

##      <a name="radio-management-service"></a>Service de gestion radio        

| | |           
|---|---|   
|   **Description du service** |   Service de gestion radio et de mode Avion
|   **Nom du service**    |   RmSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

## <a name="remote-access-auto-connection-manager"></a>Gestionnaire des connexions automatiques d’accès à distance            

| | |           
|---|---|   
|   **Description du service** |   Crée une connexion vers un réseau distant à chaque fois qu’un programme référence un nom ou une adresse DNS ou NetBIOS distant.
|   **Nom du service**    |   RasAuto
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="remote-access-connection-manager"></a>Gestionnaire des connexions d’accès à distance         

| | |           
|---|---|   
|   **Description du service** |   Gère les connexions d’accès à distance et les connexions de réseau privé virtuel (VPN) entre cet ordinateur et Internet ou d’autres réseaux distants. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   RasMan
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="remote-desktop-configuration"></a>Configuration du Bureau à distance         

| | |           
|---|---|   
|   **Description du service** |   Le service Configuration des services Bureau à distance (RDCS) est responsable de toutes les activités de maintenance de session et de configuration du Bureau à distance et des services Bureau à distance nécessitant un contexte SYSTEM. Il faut y inclure les dossiers temporaires par session, ainsi que les thèmes et certificats des services Bureau à distance.
|   **Nom du service**    |   SessionEnv
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   
|||         

<br />          

## <a name="remote-desktop-services"></a>Services Bureau à distance          

| | |           
|---|---|   
|   **Description du service** |   Autorise les utilisateurs à se connecter de manière interactive à un ordinateur distant. Le Bureau à distance et le serveur hôte de session Bureau à distance dépendent de ce service.  Pour empêcher l’utilisation à distance de cet ordinateur, désactivez les cases à cocher situées sous l’onglet Utilisation à distance des Propriétés système via l’élément Système du Panneau de configuration.
|   **Nom du service**    |   TermService
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   
|||         

<br />          

##  <a name="remote-desktop-services-usermode-port-redirector"></a>Redirecteur de port du mode utilisateur des services Bureau à distance        

| | |           
|---|---|   
|   **Description du service** |   Permet la redirection des imprimantes/lecteurs/ports pour les connexions RDP
|   **Nom du service**    |   UmRdpService
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Prend en charge les redirections côté serveur de la connexion.
|||         

<br />          

## <a name="remote-procedure-call-rpc"></a>Appel de procédure distante (RPC)          

| | |           
|---|---|   
|   **Description du service** |   Le service RPCSS est le Gestionnaire de contrôle des services pour les serveurs COM et DCOM. Il traite les demandes d’activation d’objets, les résolutions d’exportateur d’objets et le nettoyage de la mémoire distribuée pour les serveurs COM et DCOM. Si ce service est arrêté ou désactivé, les programmes qui utilisent COM ou DCOM ne fonctionnent pas correctement. Il est fortement recommandé d’exécuter le service RPCSS.
|   **Nom du service**    |   RpcSs
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="remote-procedure-call-rpc-locator"></a>Localisateur d’appels de procédure distante (RPC)             

| | |               
|---|---|   
|   **Description du service** |   Dans Windows 2003 et les versions antérieures de Windows, le service Localisateur d’appels de procédure distante (RPC) gère la base de données du service de nom RPC. Dans Windows Vista et les versions ultérieures de Windows, ce service ne fournit aucune fonctionnalité et est présent à des fins de compatibilité applicative.   |
|   **Nom du service**    |   RpcLocator  |
|   **Installation**    |   Avec Expérience utilisateur uniquement    |
|   **Type de démarrage**   |   Manuelle  |
|   **Recommandation**  | Pas de recommandations   |
|   **Commentaires**    |       |
|||             

<br />              

## <a name="remote-registry"></a>Accès à distance au Registre          

| | |           
|---|---|   
|   **Description du service** |   Permet aux utilisateurs à distance de modifier les paramètres du Registre sur cet ordinateur. Si ce service est arrêté, le Registre ne pourra être modifié que par les utilisateurs de cet ordinateur. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   RemoteRegistry
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   
|||         

<br />          

##  <a name="resultant-set-of-policy-provider"></a>Fournisseur d’un jeu de stratégie résultant            

| | |           
|---|---|       
|   **Description du service** |   Fournit un service réseau qui traite les demandes pour simuler l’application des paramètres de stratégie de groupe à un utilisateur ou un ordinateur cible dans différentes situations et qui calcule les paramètres du jeu de stratégie résultant.
|   **Nom du service**    |   RSoPProv
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |Pas de recommandations    
|   **Commentaires**    |   
|||         

<br />          

## <a name="routing-and-remote-access"></a>Routage et accès à distance            

| | |           
|---|---|   
|   **Description du service** |   Offre aux entreprises des services de routage dans les environnements de réseau local ou étendu.
|   **Nom du service**    |   RemoteAccess
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   Déjà désactivé
|||         

<br />          

## <a name="rpc-endpoint-mapper"></a>Mappeur de point de terminaison RPC          

| | |           
|---|---|   
|   **Description du service** |   Résout les identificateurs des interfaces RPC en points de terminaison de transport. Si ce service est arrêté ou désactivé, les programmes utilisant des services d’appel de procédure distante (RPC) ne fonctionneront pas correctement.
|   **Nom du service**    |   RpcEptMapper
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="secondary-logon"></a>Ouverture de session secondaire     

| | |           
|---|---|       
|   **Description du service** |   Permet le démarrage des processus sous d’autres informations d’identification. Si ce service est arrêté, ce type d’ouverture de session sera indisponible. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   seclogon
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="secure-socket-tunneling-protocol-service"></a>Service SSTP (Secure Socket Tunneling Protocol)            

| | |               
|---|---|       
|   **Description du service** |   Prend en charge la connexion par le protocole SSTP (Secure Socket Tunneling Protocol) à des ordinateurs distants via un réseau VPN. Si ce service est désactivé, les utilisateurs ne peuvent pas utiliser le protocole SSTP pour accéder à des serveurs distants.    |
|   **Nom du service**    |   SstpSvc |
|   **Installation**    |   Toujours installé    |
|   **Type de démarrage**   |   Manuelle  |
|   **Recommandation**  |   Ne pas désactiver  |
|   **Commentaires**    |   La désactivation compromet le RRAS   |
|||             

<br />              

## <a name="security-accounts-manager"></a>Gestionnaire de comptes de sécurité            

| | |           
|---|---|       
|   **Description du service** |   Le démarrage de ce service signale à d’autres services que le Gestionnaire de comptes de sécurité est prêt à accepter des demandes.  La désactivation de ce service empêchera d’autres services dans le système d’être prévenus lorsque le Gestionnaire de comptes de sécurité sera prêt, ce qui pourrait provoquer le démarrage incorrect de ces services. Ce service ne doit pas être désactivé.
|   **Nom du service**    |   SamSs
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Ne pas désactiver
|   **Commentaires**    |   
|||         

<br />          

## <a name="sensor-data-service"></a>Service Données de capteur  

| | |           
|---|---|   
|   **Description du service** |   Fournit les données d'une variété de capteurs
|   **Nom du service**    |   SensorDataService
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />  

## <a name="sensor-monitoring-service"></a>Service de surveillance des capteurs            

| | |           
|---|---|       
|   **Description du service** |   Surveille plusieurs capteurs pour exposer les données et s’adapter aux divers états utilisateur et du système.  Si le service est arrêté ou désactivé, la luminosité de l’affichage ne s’adaptera pas aux conditions d’éclairage. L’arrêt de ce service peut également affecter d’autres fonctionnalités du système.
|   **Nom du service**    |   SensrSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br /><br/>
## <a name="sensor-servicebr--br------br---strongservice-descriptionstrong----a-service-for-sensors-that-manages-different-sensors39-functionality-manages-simple-device-orientation-sdo-and-history-for-sensors-loads-the-sdo-sensor-that-reports-device-orientation-changes--if-this-service-is-stopped-or-disabled-the-sdo-sensor-will-not-be-loaded-and-so-auto-rotation-will-not-occur-history-collection-from-sensors-will-also-be-stopped"></a>Service de capteur<br/>| | |<br/>|---|---|<br/>|   <strong>Description du service</strong> |   Service pour les capteurs permettant de gérer différentes fonctionnalités. Gère l’Orientation de périphérique simple (SDO) et l’historique pour les capteurs. Charge le capteur d'orientation de périphérique simple rapportant les changements d'orientation du périphérique.  Si le service est arrêté ou désactivé, le capteur d'orientation de périphérique simple ne sera pas chargé et la rotation automatique n'aura pas lieu. La collecte d'historiques des capteurs sera également arrêtée.
|   <strong>Nom du service</strong> |   SensorService |   <strong>Installation</strong> |   Uniquement avec Expérience utilisateur |   <strong>Type de démarrage</strong> |   Manuel |   <strong>Recommandation</strong> |   Possibilité de désactivation |   <strong>Commentaires</strong>    |<br/>|||<br/>
<br />          

## <a name="server"></a>Server           

| | |           
|---|---|   
|   **Description du service** |   Prend en charge le partage de fichiers, d’impression et des canaux nommés via le réseau pour cet ordinateur. Si ce service est arrêté, ces fonctions ne seront pas disponibles. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   LanmanServer
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Requis pour la gestion à distance, IPC$, le partage de fichiers SMB
|||         

<br />          

## <a name="shell-hardware-detection"></a>Détection matériel noyau             

| | |           
|---|---|       
|   **Description du service** |   Fournit des notifications à des événements matériels de lecture automatique.
|   **Nom du service**    |   ShellHWDetection
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

## <a name="smart-card"></a>Carte à puce           

| | |           
|---|---|   
|   **Description du service** |   Gère l’accès aux cartes à puce lues par cet ordinateur. Si ce service est arrêté, cet ordinateur ne pourra plus lire de cartes à puces. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   SCardSvr
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         

<br />          

## <a name="smart-card-device-enumeration-service"></a>Service d’énumération de périphériques de carte à puce                    

| | |               
|---|---|       
|   **Description du service** |   Crée des nœuds de périphériques logiciels pour tous les lecteurs de cartes à puce accessibles à une session donnée. Si ce service est désactivé, les API WinRT ne peuvent pas énumérer les lecteurs de cartes à puce.   |
|   **Nom du service**    |   ScDeviceEnum    |
|   **Installation**    |   Toujours installé    |
|   **Type de démarrage**   |   Manuelle  |
|   **Recommandation**  |   Possibilité de désactivation   |
|   **Commentaires**    |   Requis presque exclusivement pour les applications WinRT    |
|||             

<br />              

## <a name="smart-card-removal-policy"></a>Stratégie de retrait de la carte à puce        

| | |           
|---|---|       
|   **Description du service** |   Autorise la configuration du système pour verrouiller le Bureau de l’utilisateur au moment du retrait de la carte à puce.
|   **Nom du service**    |   SCPolicySvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="snmp-trap"></a>Interruption SNMP            

| | |           
|---|---|       
|   **Description du service** |   Reçoit les messages d’interception générés par les agents SNMP (Simple Network Management Protocol) locaux ou distants et transmet les messages aux programmes de gestion SNMP s’exécutant sur cet ordinateur. Si ce service est arrêté, les programmes à base SNMP sur cet ordinateur ne recevront pas les messages d’interception SNMP. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   SNMPTRAP
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="software-protection"></a>Protection logicielle             

| | |           
|---|---|       
|   **Description du service** |   Permet le téléchargement, l’installation et l’application de licences numériques pour Windows et des applications Windows. Si le service est désactivé, le système d’exploitation et les applications sous licence peuvent s’exécuter dans un mode de notification. Il est vivement recommandé de ne pas désactiver le service de protection logicielle.
|   **Nom du service**    |   sppsvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="special-administration-console-helper"></a>Application d’assistance de la Console d’administration spéciale        

| | |           
|---|---|   
|   **Description du service** |   Permet aux administrateurs d’accéder à distance à une invite de commandes en utilisant les services de gestion d’urgence.
|   **Nom du service**    |   sacsvr
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="spot-verifier"></a>Vérificateur de points            

| | |           
|---|---|   
|   **Description du service** |   Vérifie l'altération potentielle du système de fichiers.
|   **Nom du service**    |   svsvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="ssdp-discovery"></a>Découverte SSDP           

| | |           
|---|---|   
|   **Description du service** |   Découvre les périphériques et services en réseau qui utilisent le protocole de découverte SSDP, tels que les périphériques UPnP. Annonce également les périphériques et services SSDP exécutés sur l’ordinateur local. Si ce service est arrêté, les périphériques SSDP ne seront pas découverts. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   SSDPSRV
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

## <a name="state-repository-service"></a>Service State Repository         

| | |           
|---|---|   
|   **Description du service** |   Fournit la prise en charge d'infrastructure nécessaire au modèle d'application.
|   **Nom du service**    |   StateRepository
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="still-image-acquisition-events"></a>Événements d’acquisition d’images fixes

| | |           
|---|---|   
|   **Description du service** |   Lance les applications associées aux événements d’acquisition d’images fixes.
|   **Nom du service**    |   WiaRpc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />  

## <a name="storage-service"></a>Service de stockage          

| | |           
|---|---|       
|   **Description du service** |   Fournit des services d'activation pour les paramètres de stockage et l'extension de stockage externe
|   **Nom du service**    |   StorSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="storage-tiers-management"></a>Gestion des niveaux de stockage        

| | |           
|---|---|   
|   **Description du service** |   Optimise le placement des données dans les niveaux de stockage de tous les espaces de stockage hiérarchisé sur le système.
|   **Nom du service**    |   TieringEngineService
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="superfetch"></a>Superfetch          

| | |           
|---|---|       
|   **Description du service** |   Gère et améliore le niveau de performance du système dans le temps.
|   **Nom du service**    |   SysMain
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="sync-host"></a>Hôte de synchronisation            

| | |           
|---|---|       
|   **Description du service** |   Ce service synchronise la messagerie, les contacts, le calendrier et diverses autres données utilisateur. La messagerie et d'autres applications dépendantes de cette fonctionnalité ne fonctionnent pas correctement lorsque ce service n'est pas exécuté.
|   **Nom du service**    |   OneSyncSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Modèle de service utilisateur
|||         

<br />          

## <a name="system-event-notification-service"></a>Service de notification d’événements système            

| | |           
|---|---|       
|   **Description du service** |   Analyse les événements système et notifie leur existence aux abonnés du système d’événements COM+.
|   **Nom du service**    |   SENS
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="system-events-broker"></a>Service Broker pour les événements système             

| | |           
|---|---|       
|   **Description du service** |   Coordonne l’exécution de travail en arrière-plan pour l’application WinRT. Si ce service est arrêté ou désactivé, le travail en arrière-plan risque de ne pas être déclenché.
|   **Nom du service**    |   SystemEventsBroker
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Bien que sa description implique uniquement les applications WinRT, il est requis pour le planificateur de tâches, service Broker d’infrastructure et d’autres composants internes.
|||         

<br />          

## <a name="task-scheduler"></a>Planificateur de tâches           

| | |           
|---|---|   
|   **Description du service** |   Permet à un utilisateur de configurer et de planifier des tâches automatisées sur cet ordinateur. Le service héberge également plusieurs tâches critiques du système Windows. Si ce service est arrêté ou désactivé, ces tâches ne seront pas exécutées à l’heure prévue. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   Planification
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="tcpip-netbios-helper"></a>Assistance NetBIOS sur TCP/IP            

| | |           
|---|---|   
|   **Description du service** |   Prend en charge le service NetBIOS sur TCP/IP (NetBT) et la résolution de noms NetBIOS pour les clients présents sur le réseau, ce qui permet aux utilisateurs de partager des fichiers, d’imprimer et d’ouvrir des sessions sur le réseau. Si ce service est arrêté, ces fonctions risquent de ne pas être disponibles. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   lmhosts
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="telephony"></a>Telephony           

| | |           
|---|---|       
|   **Description du service** |   Prend en charge l’interface TAPI (Telephony API) pour les programmes qui contrôlent les périphériques de téléphonie sur l’ordinateur local et, via le réseau local (LAN), sur les serveurs qui exécutent également le service.
|   **Nom du service**    |   TapiSrv
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   La désactivation compromet le RRAS
|||         

<br />          

## <a name="themes"></a>Thèmes           

| | |           
|---|---|
|   **Description du service** |   Fournit un système de gestion de thème de l’expérience utilisateur.
|   **Nom du service**    |   Thèmes
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Il est impossible de définir des thèmes d’accessibilité si ce service est désactivé
|||         

<br />  

## <a name="tile-data-model-server"></a>Serveur de modèles de données de vignette           

| | |           
|---|---|   
|   **Description du service** |   Serveur de vignettes pour la mise à jour des vignettes.
|   **Nom du service**    |   tiledatamodelsvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Le menu Démarrer est compromis si ce service est désactivé
|||         

<br />          

##  <a name="time-broker"></a>Service Broker pour les événements horaires     

| | |           
|---|---|       
|   **Description du service** |   Coordonne l’exécution de travail en arrière-plan pour l’application WinRT. Si ce service est arrêté ou désactivé, le travail en arrière-plan risque de ne pas être déclenché.
|   **Nom du service**    |   TimeBrokerSvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Bien que sa description implique uniquement les applications WinRT, il est requis pour le planificateur de tâches, service Broker d’infrastructure et d’autres composants internes.
|||         

<br />          

## <a name="touch-keyboard-and-handwriting-panel-service"></a>Service du clavier tactile et du volet d’écriture manuscrite         

| | |           
|---|---|   
|   **Description du service** |   Active les fonctionnalités de stylet et d’entrée manuscrite du clavier tactile et du volet d’écriture manuscrite
|   **Nom du service**    |   TabletInputService
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

## <a name="update-orchestrator-service-for-windows-update"></a>Service Update Orchestrator pour Windows Update           

| | |           
|---|---|       
|   **Description du service** |   Gère les mises à jour Windows. S'il est arrêté, vos appareils ne seront pas en mesure de télécharger et d'installer les dernières mises à jour.
|   **Nom du service**    |   UsoSvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   La description du service était absente dans v1607 ; Windows Update (y compris WSUS) dépend de ce service.
|||         

<br />          

## <a name="upnp-device-host"></a>Hôte de périphérique UPnP         

| | |           
|---|---|   
|   **Description du service** |   Autorise l’hébergement des périphériques UPnP sur cet ordinateur. Si ce service est arrêté, tous les périphériques UPnP hébergés cesseront de fonctionner et aucun autre périphérique hébergé ne pourra être ajouté. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   upnphost
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

## <a name="user-access-logging-service"></a>Service de journalisation des accès utilisateur          

| | |           
|---|---|   
|   **Description du service** |   Ce service consigne les demandes d’accès de chaque client, sous forme d’adresses IP et de noms d’utilisateur, des produits et rôles installés sur le serveur local. Ces informations peuvent être obtenues via des requêtes PowerShell par les administrateurs qui ont besoin de quantifier les demandes clients en logiciels serveur pour la gestion des licences d’accès client (CAL) hors connexion. Si le service est désactivé, les demandes des clients ne sont pas consignées et ne pourront pas être extraites via des requêtes PowerShell. L’arrêt du service n’affectera pas l’interrogation des données de l’historique (voir la documentation correspondante pour connaître les étapes nécessaires à la suppression des données de l’historique). L’administrateur système local doit consulter les termes du contrat de licence Windows Server pour déterminer le nombre de CAL nécessaires pour que la licence du logiciel serveur soit correcte ; l’utilisation du service et des données UAL ne décharge pas de cette obligation.
|   **Nom du service**    |   UALSVC
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="user-data-access"></a>Accès aux données utilisateur        

| | |           
|---|---|   
|   **Description du service** |   Fournit l’accès des applications aux données utilisateur structurées, notamment aux coordonnées, calendriers, messages et autres éléments de contenu. Si vous arrêtez ou désactivez ce service, les applications qui utilisent ces données risquent de ne pas fonctionner correctement.
|   **Nom du service**    |   UserDataSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Modèle de service utilisateur
|||         

<br />          

## <a name="user-data-storage"></a>Stockage des données utilisateur            

| | |           
|---|---|       
|   **Description du service** |   Gère le stockage des données utilisateur structurées, notamment les coordonnées, les calendriers, les messages et d’autres éléments de contenu. Si vous arrêtez ou désactivez ce service, les applications qui utilisent ces données risquent de ne pas fonctionner correctement.
|   **Nom du service**    |   UnistoreSvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Modèle de service utilisateur
|||         

<br />          

## <a name="user-experience-virtualization-service"></a>Service User Experience Virtualization           

| | |           
|---|---|       
|   **Description du service** |   Assure la prise en charge de l’itinérance des paramètres d’application et de système d’exploitation
|   **Nom du service**    |   UevAgentService
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         

<br />          

##  <a name="user-manager"></a>Gestionnaire des utilisateurs        

| | |           
|---|---|   
|   **Description du service** |   Le Gestionnaire des utilisateurs fournit les composants d'exécution requis pour une interaction multi-utilisateur.  Si ce service est arrêté, certaines applications risquent de ne pas fonctionner correctement.
|   **Nom du service**    |   UserManager
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="user-profile-service"></a>Service de profil utilisateur         

| | |           
|---|---|   
|   **Description du service** |   Ce service est responsable du chargement et du déchargement des profils utilisateur. Si ce service est arrêté ou désactivé, les utilisateurs ne pourront plus se connecter ou se déconnecter, les applications pourront avoir des problèmes pour obtenir les données des utilisateurs, et les composants inscrits pour recevoir les notifications d’événements de profils ne les recevront pas.
|   **Nom du service**    |   ProfSvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="virtual-disk"></a>Disque virtuel             

| | |           
|---|---|   
|   **Description du service** |   Fournit des services de gestion des disques, des volumes, des systèmes de fichiers et des groupes de stockage.
|   **Nom du service**    |   vds
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Pas de recommandations
|   **Commentaires**    |   
|||         

<br />          

## <a name="volume-shadow-copy"></a>Cliché instantané des volumes           

| | |           
|---|---|   
|   **Description du service** |   Gère et implémente les clichés instantanés de volumes pour les sauvegardes et autres utilisations. Si ce service est arrêté, les clichés instantanés ne seront pas disponibles pour la sauvegarde et la sauvegarde échouera. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   VSS
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Pas de recommandations
|   **Commentaires**    |   
|||         

<br />          

##  <a name="walletservice"></a>WalletService           

| | |           
|---|---|   
|   **Description du service** |   Objets d'hôtes utilisés par les clients du portefeuille
|   **Nom du service**    |   WalletService
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-audio"></a>Audio Windows            

| | |           
|---|---|       
|   **Description du service** |   Gère les périphériques audio pour les programmes compatibles Windows.  Si ce service est arrêté, les périphériques et les effets audio ne fonctionneront pas correctement.  S’il est désactivé, les services qui en dépendent explicitement ne démarreront pas
|   **Nom du service**    |   Audiosrv
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-audio-endpoint-builder"></a>Générateur de points de terminaison du service Audio Windows           

| | |           
|---|---|
|   **Description du service** |   Gère les périphériques audio pour le service Audio Windows.  Si ce service est arrêté, les périphériques et les effets audio ne fonctionneront pas correctement.  S’il est désactivé, les services qui en dépendent explicitement ne démarreront pas
|   **Nom du service**    |   AudioEndpointBuilder
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-biometric-service"></a>Service de biométrie Windows            

| | |           
|---|---|   
|   **Description du service** |   Le service de biométrie Windows permet aux applications clientes de capturer, de comparer, de manipuler et de stocker des données biométriques sans avoir directement accès au matériel ou aux échantillons biométriques. Le service est hébergé dans un processus SVCHOST privilégié.
|   **Nom du service**    |   WbioSrvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="windows-camera-frame-server"></a>Serveur de trame de la Caméra Windows         

| | |           
|---|---|       
|   **Description du service** |   Permet à plusieurs clients d’accéder aux trames vidéo des appareils photo.
|   **Nom du service**    |   FrameServer
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-connection-manager"></a>Gestionnaire de connexions Windows           

| | |           
|---|---|   
|   **Description du service** |   Prend des décisions automatiques de connexion/déconnexion en fonction des options de connectivité réseau actuellement disponibles à l’ordinateur, et active la gestion de la connectivité réseau en fonction des paramètres de la stratégie de groupe.
|   **Nom du service**    |   Wcmsvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-defender-network-inspection-service"></a>Service Inspection du réseau Windows Defender          

| | |           
|---|---|       
|   **Description du service** |   Empêche les tentatives d’intrusion ciblant les vulnérabilités connues et nouvellement découvertes des protocoles réseau
|   **Nom du service**    |   WdNisSvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations    
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-defender-service"></a>Service Windows Defender         

| | |           
|---|---|       
|   **Description du service** |   Protège les utilisateurs contre les logiciels malveillants et les autres logiciels potentiellement indésirables
|   **Nom du service**    |   WinDefend
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Windows Driver Foundation - Infrastructure de pilote mode-utilisateur           

| | |           
|---|---|   
|   **Description du service** |   Crée et gère les processus de pilote en mode utilisateur. Ce service ne peut pas être arrêté.
|   **Nom du service**    |   wudfsvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-encryption-provider-host-service"></a>Service hôte du fournisseur de chiffrement Windows     

| | |           
|---|---|   
|   **Description du service** |   Le service hôte du fournisseur de chiffrement Windows négocie les fonctionnalités liées au chiffrement avec les fournisseurs de chiffrement tiers pour les processus qui ont besoin d’évaluer et d’appliquer des stratégies EAS. L’arrêt de cette activité compromettra les vérifications de conformité EAS qui ont été établies par les comptes de messagerie connectés
|   **Nom du service**    |   WEPHOSTSVC
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-error-reporting-service"></a>Service de rapport d'erreurs Windows          

| | |           
|---|---|       
|   **Description du service** |   Permet le signalement des erreurs quand des programmes cessent de fonctionner ou de répondre, et permet de délivrer des solutions existantes. Permet également la génération des journaux pour les services de diagnostic et de réparation. Si ce service est arrêté, le signalement des erreurs risque de ne pas fonctionner correctement ; par ailleurs, les résultats des services de diagnostic et de réparation risquent de ne pas s’afficher.
|   **Nom du service**    |   WerSvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Collecte et envoie les données d’incident et de blocage utilisées par les éditeurs de logiciels indépendants et fabricants de matériel MS et tiers. Les données sont utilisées pour diagnostiquer les bogues à l'origine des incidents, ce qui peut inclure des bogues de sécurité. Également requis pour les rapports d’erreurs d’entreprise
|||         

<br />          

## <a name="windows-event-collector"></a>Collecteur d'événements de Windows          

| | |           
|---|---|   
|   **Description du service** |   Ce service gère des abonnements persistants à des événements de sources distantes prenant en charge le protocole Gestion de services Web. Cela inclut les journaux d’événements de Windows Vista, les sources d’événements matériels et IPMI. Le service stocke les événements transférés dans un journal d’événements local. Si ce service est arrêté ou désactivé, des abonnements aux événements ne peuvent pas être créés et les événements transférés ne peuvent pas être acceptés.
|   **Nom du service**    |   Wecsvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Collecte les événements ETW (y compris les événements de sécurité) pour faciliter la gestion, les diagnostics.  Un grand nombre de fonctionnalités et d'outils tiers s’appuient sur ce service, y compris des outils d’audit de sécurité
|||         

<br />          

## <a name="windows-event-log"></a>Journal des événements Windows            

| | |           
|---|---|       
|   **Description du service** |   Ce service gère les événements et les journaux d’événements. Il prend en charge l’enregistrement des événements, les requêtes sur les événements, l’abonnement aux événements, l’archivage des journaux d’événements et la gestion des métadonnées d’événements. Il peut afficher des événements en XML et en format de texte brut. L’arrêt de ce service peut compromettre la sécurité et la fiabilité du système.
|   **Nom du service**    |   EventLog
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-firewall"></a>Pare-feu Windows         

| | |           
|---|---|   
|   **Description du service** |   Le Pare-feu Windows vous aide à protéger votre ordinateur en empêchant les utilisateurs non autorisés d'accéder à votre ordinateur via Internet ou un réseau.
|   **Nom du service**    |   MpsSvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="windows-font-cache-service"></a>Service de cache de polices Windows      

| | |           
|---|---|   
|   **Description du service** |   Optimise le niveau de performance des applications en mettant en cache les données des polices communément utilisées. Les applications démarrent ce service s’il n’est pas déjà en fonctionnement. Il peut être désactivé ; il en résulte cependant une dégradation du niveau de performance des applications.
|   **Nom du service**    |   FontCache
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-image-acquisition-wia"></a>Acquisition d’image Windows (WIA)          

| | |           
|---|---|   
|   **Description du service** |   Fournit des services d’acquisition d’images pour les scanneurs et les appareils photo
|   **Nom du service**    |   stisvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

##  <a name="windows-insider-service"></a>Service Windows Insider     

| | |           
|---|---|   
|   **Description du service** |   wisvc
|   **Nom du service**    |   wisvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Server ne prend pas en charge la distribution de version d'évaluation et dès lors, il n'est pas opérationnel sur Server. Ce service peut aussi être désactivé via la stratégie de groupe.
|||         

<br />          

##  <a name="windows-installer"></a>Windows Installer       

| | |           
|---|---|
|   **Description du service** |   Ajoute, modifie et supprime des applications fournies en tant que package Windows Installer (*.msi, *.msp, *.appx). Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   msiserver
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-license-manager-service"></a>Serveur Gestionnaire de licences Windows          

| | |           
|---|---|   
|   **Description du service** |   Permet la prise en charge de l'infrastructure du Microsoft Store.  Ce service démarre à la demande. S'il est désactivé, le contenu acheté via le Microsoft Store ne se comporte pas correctement.
|   **Nom du service**    |   LicenseManager
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-management-instrumentation"></a>WMI (Windows Management Instrumentation)       

| | |           
|---|---|       
|   **Description du service** |   Fournit une interface commune et un modèle objet pour accéder aux informations de gestion du système d’exploitation, des périphériques, des applications et des services. Si ce service est arrêté, la plupart des logiciels sur base Windows ne fonctionneront pas correctement. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   Winmgmt
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

##  <a name="windows-mobile-hotspot-service"></a>Service Point d'accès sans fil mobile Windows          

| | |           
|---|---|       
|   **Description du service** |   Permet de partager une connexion de données cellulaires avec un autre appareil.
|   **Nom du service**    |   icssvc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-modules-installer"></a>Programme d’installation pour les modules Windows        

| | |           
|---|---|   
|   **Description du service** |   Permet l’installation, la modification et la suppression de composants facultatifs et de mises à jour Windows. Si ce service est désactivé, l’installation ou la désinstallation de mises à jour Windows peut échouer pour cet ordinateur.
|   **Nom du service**    |   TrustedInstaller
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-push-notifications-system-service"></a>Service du système de notifications Push Windows            

| | |           
|---|---|
|   **Description du service** |   Ce service s’exécute dans la session 0 et héberge la plateforme de notification et le fournisseur de connexion qui gère la connexion entre l’appareil et le serveur WNS.
|   **Nom du service**    |   WpnService
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Requis pour les vignettes dynamiques et autres fonctionnalités
|||         

<br />      

## <a name="windows-push-notifications-user-service"></a>Service utilisateur de notifications Push Windows          

| | |           
|---|---|   
|   **Description du service** |   Ce service héberge la plateforme de notification Windows qui fournit la prise en charge des notifications locales et Push. Les notifications prises en charge sont tile, toast et raw.
|   **Nom du service**    |   WpnUserService
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Possibilité de désactivation
|   **Commentaires**    |   Modèle de service utilisateur
|||         

<br />

## <a name="windows-remote-management-ws-management"></a>Windows Remote Management (WS-Management)
| | |
|---|---|
|   **Description du service** |   Le service Gestion à distance de Windows (WinRM) implémente le protocole WS-Management pour la gestion à distance. WS-Management est un protocole des services web standard utilisé pour la gestion à distance de logiciels et de matériel. Le service Gestion à distance de Windows écoute sur le réseau les demandes WS-Management et les traite. Ce service doit être configuré avec un écouteur à l'aide de l'outil en ligne de commande winrm.cmd ou via la stratégie de groupe de façon à ce qu'il puisse être à l'écoute. Il permet d'accéder aux données WMI et de collecter des événements. La collecte d'événements et l'abonnement à des événements nécessitent que le service soit en cours d'exécution. Les messages du service Gestion à distance de Windows utilisent HTTP et HTTPS comme transport. Le service Gestion à distance de Windows ne dépend pas d'IIS mais il est préconfiguré pour partager un port avec IIS sur le même ordinateur.  Le service Gestion à distance de Windows réserve le préfixe d'URL /wsman. Pour éviter les conflits avec IIS, les administrateurs doivent vérifier que les sites web hébergés sur IIS n'utilisent pas le préfixe d'URL /wsman.
|   **Nom du service**    |   WinRM
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Requis pour la gestion à distance
|||

<br />          

##  <a name="windows-search"></a>Windows Search      

| | |           
|---|---|       
|   **Description du service** |   Fournit des fonctionnalités d’indexation de contenu, de mise en cache des propriétés, de résultats de recherche pour les fichiers, les messages électroniques et autres contenus.
|   **Nom du service**    |   WSearch
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Désactivée
|   **Recommandation**  |   Déjà désactivé
|   **Commentaires**    |   
|||         

<br />          

##  <a name="windows-time"></a>Horloge Windows        

| | |           
|---|---|   
|   **Description du service** |   Assure la synchronisation de la date et de l’heure sur tous les clients et serveurs sur le réseau. Si ce service est arrêté, la synchronisation de la date et de l’heure sera indisponible. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   W32Time
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations
|   **Commentaires**    |   
|||         

<br />          

## <a name="windows-update"></a>Windows Update           

| | |           
|---|---|       
|   **Description du service** |   Active la détection, le téléchargement et l’installation des mises à jour de Windows et d’autres programmes. Si ce service est désactivé, les utilisateurs de cet ordinateur ne pourront pas utiliser Windows Update ou sa fonctionnalité de mise à jour automatique, et les programmes ne pourront pas utiliser l’API de l’Agent de mise à jour automatique Windows Update (WUA).
|   **Nom du service**    |   wuauserv
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="winhttp-web-proxy-auto-discovery-service"></a>Service de découverte automatique de Proxy Web pour les services HTTP Windows         

| | |           
|---|---|   
|   **Description du service** |   WinHTTP implémente la pile HTTP du client et fournit aux développeurs une API Win32 et un composant d’automatisation COM pour l’envoi des demandes HTTP et la réception des réponses. En outre, WinHTTP prend en charge la découverte automatique d’une configuration de proxy via son implémentation du protocole WPAD (Web Proxy Auto-Discovery).
|   **Nom du service**    |   WinHttpAutoProxySvc
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Ne pas désactiver
|   **Commentaires**    |   Tous les éléments utilisant la pile réseau peut dépendre de ce service. De nombreuses organisations l'utilisent pour configurer le routage de proxy HTTP de leurs réseaux internes.  Sans ce service, les connexions HTTP internes vers Internet échouent.
|||         

<br />          

## <a name="wired-autoconfig"></a>Service de configuration automatique de réseau câblé         

| | |           
|---|---|       
|   **Description du service** |   Le service Wired AutoConfig (DOT3SVC) est responsable de l’exécution de l’authentification IEEE 802.1X sur les interfaces Ethernet. Si votre déploiement de réseau câblé actuel applique l’authentification 802.1X, le service DOT3SVC doit être configuré de façon à s’exécuter pour l’établissement de la connectivité de Couche 2 et/ou fournir l’accès aux ressources réseau. Les réseaux câblés qui n’appliquent pas l’authentification 802.1X ne sont pas concernés par le service DOT3SVC.
|   **Nom du service**    |   dot3svc
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations   
|   **Commentaires**    |   
|||         

<br />          

## <a name="wmi-performance-adapter"></a>Carte adaptateur de performance WMI          

| | |           
|---|---|   
|   **Description du service** |   Fournit des informations concernant la bibliothèque de performance à partir des fournisseurs WMI (Windows Management Instrumentation) à des clients sur le réseau. Ce service s’exécute uniquement quand l’assistant de performance des données est activé.
|   **Nom du service**    |   wmiApSrv
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  | Pas de recommandations       
|   **Commentaires**    |   
|||         

<br />          

## <a name="workstation"></a>Station de travail          

| | |           
|---|---|   
|   **Description du service** |   Crée et maintient des connexions réseau client à des serveurs distants à l’aide du protocole SMB. Si ce service est arrêté, ces connexions ne seront pas disponibles. Si ce service est désactivé, le démarrage de tout service qui en dépend explicitement échouera.
|   **Nom du service**    |   LanmanWorkstation
|   **Installation**    |   Toujours installé
|   **Type de démarrage**   |   Automatique
|   **Recommandation**  | Pas de recommandations       
|   **Commentaires**    |   
|||         

<br />

## <a name="xbox-live-auth-manager"></a>Gestionnaire d'authentification Xbox Live           

| | |           
|---|---|   
|   **Description du service** |   Fournit des services d'authentification et d'autorisation pour interagir avec Xbox Live. Si ce service est arrêté, certaines applications risquent de ne pas fonctionner correctement.
|   **Nom du service**    |   XblAuthManager
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Doit être désactivé
|   **Commentaires**    |   
|||         

<br />          

## <a name="xbox-live-game-save"></a>Jeu sauvegardé sur Xbox Live          

| | |           
|---|---|   
|   **Description du service** |   Ce service synchronise les données enregistrées pour les jeux dont la sauvegarde sur Xbox Live est activée.  Si ce service est arrêté, les données enregistrées des jeux ne seront téléchargées ni vers, ni depuis Xbox Live.
|   **Nom du service**    |   XblGameSave
|   **Installation**    |   Avec Expérience utilisateur uniquement
|   **Type de démarrage**   |   Manuelle
|   **Recommandation**  |   Doit être désactivé
|   **Commentaires**    |   Ce service synchronise les données enregistrées pour les jeux dont la sauvegarde sur Xbox Live est activée.  Si ce service est arrêté, les données enregistrées des jeux ne seront téléchargées ni vers, ni depuis Xbox Live.
|||         

<br /> 
<br /> 

