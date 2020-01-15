---
title: Résolution des problèmes de AD FS-audit des événements et de la journalisation
description: Ce document décrit comment utiliser les différents journaux de AD FS pour résoudre les problèmes
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e42c5b6d53cd3985fefc2c93ab10b59383a35af0
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950181"
---
# <a name="ad-fs-troubleshooting---events-and-logging"></a>Résolution des problèmes de AD FS des événements et de la journalisation
AD FS fournit deux journaux principaux qui peuvent être utilisés lors de la résolution des problèmes.  Ils sont les suivants :

- le journal de l’administrateur
- Journal des traces  
 
Chacun de ces journaux sera expliqué ci-dessous.

## <a name="admin-log"></a>Journal d’administration
Le journal d’administration fournit des informations de haut niveau sur les problèmes qui se produisent et est activé par défaut.

### <a name="to-view-the-admin-log"></a>Pour afficher le journal de l’administrateur
1.  Ouvrez l’observateur d’événements
2.  Développez **Journal des applications et des services**.
3.  Développez **AD FS**.
4.  Cliquez sur le titre **Admin**.

![améliorations de l’audit](media/ad-fs-tshoot-logging/event1.PNG)  

## <a name="trace-log"></a>Journal des traces
Le journal des traces est l’endroit où les messages détaillés sont journalisés et est le journal le plus utile lors du dépannage. Étant donné que de nombreuses informations de journal de suivi peuvent être générées dans un laps de temps réduit, ce qui peut avoir un impact sur les performances du système, les journaux de suivi sont désactivés par défaut. 

### <a name="to-enable-and-view-the-trace-log"></a>Pour activer et afficher le journal des traces
1.  Ouvrez l’observateur d’événements
2.  Cliquez avec le bouton droit sur le **Journal des applications et des services** , sélectionnez Afficher, puis cliquez sur **afficher les journaux d’analyse et de débogage**.  Cette opération affiche des nœuds supplémentaires sur la gauche.
améliorations apportées à l’audit ![](media/ad-fs-tshoot-logging/event2.PNG)  
3.  Développer le suivi AD FS
4.  Cliquez avec le bouton droit sur Debug et sélectionnez **activer le journal**.
améliorations apportées à l’audit ![](media/ad-fs-tshoot-logging/event3.PNG)  


## <a name="event-auditing-information-for-ad-fs-on-windows-server-2016"></a>Informations d’audit d’événements pour AD FS sur Windows Server 2016  
Par défaut, les AD FS dans Windows Server 2016 ont un niveau de base d’audit activé.  Avec l’audit de base, les administrateurs verront 5 événements ou moins pour une requête unique.  Cela marque une diminution significative du nombre d’événements que les administrateurs doivent examiner pour afficher une requête unique.   Le niveau d’audit peut être augmenté ou diminué à l’aide de la applet PowerShell :  

```PowerShell
Set-AdfsProperties -AuditLevel 
```

Le tableau ci-dessous décrit les niveaux d’audit disponibles.  

|Niveau d'audit|Syntaxe PowerShell|Description|  
|----- | ----- | ----- |
|Aucun(e)|Set-AdfsProperties-AuditLevel aucun|L’audit est désactivé et aucun événement n’est enregistré.|  
|De base (par défaut)|Set-AdfsProperties-AuditLevel de base|Plus de 5 événements seront journalisés pour une demande unique|  
|Verbose|Set-AdfsProperties-AuditLevel verbose|Tous les événements sont consignés.  Cela permet de consigner une quantité importante d’informations par demande.|  
  
Pour afficher le niveau d’audit actuel, vous pouvez utiliser PowerShell applet : obtenir-AdfsProperties.  
  
![améliorations de l’audit](media/ad-fs-tshoot-logging/ADFS_Audit_1.PNG)  
  
Le niveau d’audit peut être augmenté ou diminué à l’aide de PowerShell applet : Set-AdfsProperties-AuditLevel.  
  
![améliorations de l’audit](media/ad-fs-tshoot-logging/ADFS_Audit_2.png)  
  
## <a name="types-of-events"></a>Types d'événements  
Les événements de AD FS peuvent être de types différents, en fonction des différents types de demandes traités par AD FS. Chaque type d’événement est associé à des données spécifiques.  Le type d’événements peut être différencié entre les demandes de connexion (par exemple, les demandes de jeton) et les requêtes système (appels serveur-serveur, y compris la récupération des informations de configuration).    

Le tableau ci-dessous décrit les types d’événements de base.  
  
|Type d'événement|ID d’événement|Description| 
|----- | ----- | ----- | 
|Réussite de la validation des informations d’identification|1202|Demande dans laquelle les nouvelles informations d’identification sont validées correctement par le service FS (Federation Service). Cela comprend WS-Trust, WS-Federation, SAML-P (premier tronçon pour générer l’authentification unique) et des points de terminaison d’autorisation OAuth.|  
|Nouvelle erreur de validation des informations d’identification|1203|Demande dans laquelle la validation des informations d’identification a échoué sur le service FS (Federation Service). Cela comprend WS-Trust, WS-FED, SAML-P (premier tronçon pour générer l’authentification unique) et des points de terminaison d’autorisation OAuth.|  
|Jeton d’application réussi|1200|Demande dans laquelle un jeton de sécurité est émis avec succès par le service FS (Federation Service). Pour WS-Federation, SAML-P est consigné lors du traitement de la demande avec l’artefact SSO. (par exemple, le cookie SSO).|  
|Échec du jeton d’application|1201|Demande dans laquelle l’émission du jeton de sécurité a échoué sur le service FS (Federation Service). Pour WS-Federation, SAML-P est consigné lorsque la requête a été traitée avec l’artefact SSO. (par exemple, le cookie SSO).|  
|Réussite de la demande de modification de mot de passe|1204|Transaction dans laquelle la demande de modification de mot de passe a été traitée avec succès par le service FS (Federation Service).|  
|Erreur de demande de modification de mot de passe|1205|Transaction dans laquelle la demande de modification de mot de passe n’a pas pu être traitée par le service FS (Federation Service).| 
|Déconnexion réussie|1206|Décrit une demande de déconnexion réussie.|  
|Échec de la déconnexion|1207|Décrit une demande de déconnexion ayant échoué.|  

## <a name="security-auditing"></a>Audit de sécurité
L’audit de sécurité du compte de service AD FS peut parfois faciliter le suivi des problèmes liés aux mises à jour de mot de passe, à la journalisation des demandes/réponses, à la demande d’en-têtes contect et aux résultats de l’inscription des appareils.  L’audit du compte de service AD FS est désactivé par défaut.

### <a name="to-enable-security-auditing"></a>Pour activer l’audit de sécurité
1. Cliquez sur Démarrer, pointez sur **programmes**, sur **Outils d’administration**, puis cliquez sur **stratégie de sécurité locale**.
2. Accédez au dossier **Paramètres de sécurité\Stratégies locales\Gestion des droits utilisateur**, puis double-cliquez sur **Générer des audits de sécurité**.
3. Sous l’onglet **Paramètre de sécurité locale** , vérifiez que le compte de service AD FS est répertorié. Si tel n’est pas le cas, cliquez sur Ajouter un utilisateur ou un groupe et ajoutez-le à la liste, puis cliquez sur OK.
4. Ouvrez une invite de commandes avec des privilèges élevés et exécutez la commande suivante pour activer l’audit d’auditpol. exe/set/Subcategory : "application générée"/Failure : Enable/Success : Enable
5. Fermez **stratégie de sécurité locale**, puis ouvrez le composant logiciel enfichable gestion des AD FS.
 
Pour ouvrir le composant logiciel enfichable Gestion des AD FS, cliquez sur Démarrer, pointez sur programmes, sur outils d’administration, puis cliquez sur gestion des AD FS.
 
6. Dans le volet Actions, cliquez sur modifier les propriétés de l’service FS (Federation Service)
7. Dans la boîte de dialogue Propriétés du service de fédération, cliquez sur l’onglet Événements.
8. Cochez les cases **Audits des succès** et **Audits des échecs**.
9. Cliquez sur OK.

![améliorations de l’audit](media/ad-fs-tshoot-logging/event4.PNG)  
 
>[!NOTE]
>Les instructions ci-dessus sont utilisées uniquement lorsque AD FS se trouve sur un serveur membre autonome.  Si AD FS s’exécute sur un contrôleur de domaine, au lieu de la stratégie de sécurité locale, utilisez la **stratégie de contrôleur de domaine par défaut** située dans **stratégie de groupe gestion/forêt/domaines/contrôleurs de domaine**.  Cliquez sur modifier et accédez à **ordinateur \ stratégies \ paramètres de validation \ stratégies locales\attribution Rights Management**

## <a name="windows-communication-foundation-and-windows-identity-foundation-messages"></a>Messages Windows Communication Foundation et Windows Identity Foundation
En plus de la journalisation de suivi, vous pouvez parfois avoir besoin d’afficher des messages Windows Communication Foundation (WCF) et Windows Identity Foundation (WIF) afin de résoudre un problème. Pour ce faire, vous pouvez modifier le fichier **Microsoft. IdentityServer. ServiceHost. exe. config** sur le serveur de AD FS. 

Ce fichier se trouve dans **<% racine système% > \Windows\ADFS** et est au format XML. Les parties pertinentes du fichier sont présentées ci-dessous : 
```
<!-- To enable WIF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="Microsoft.IdentityModel" switchValue="Off"> … </source>

<!-- To enable WCF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="System.ServiceModel" switchValue="Off" > … </source>
```


Après avoir appliqué ces modifications, enregistrez la configuration et redémarrez le service AD FS. Une fois que vous avez activé ces traces en définissant les commutateurs appropriés, elles s’affichent dans le journal de suivi AD FS dans le observateur d’événements Windows.

## <a name="correlating-events"></a>Corrélation des événements
L’une des choses les plus difficiles à résoudre est l’accès aux problèmes qui génèrent un grand nombre d’événements d’erreur ou de débogage.

Pour faciliter cette opération, AD FS met en corrélation tous les événements enregistrés dans le observateur d’événements, à la fois dans les journaux d’administration et de débogage, qui correspondent à une demande particulière à l’aide d’un identificateur global unique (GUID) unique appelé ID d’activité. Cet ID est généré lorsque la demande d’émission de jeton est présentée initialement à l’application Web (pour les applications utilisant le profil de demandeur passif) ou aux demandes envoyées directement au fournisseur de revendications (pour les applications utilisant WS-Trust). 

![ActivityID](media/ad-fs-tshoot-logging/activityid1.png)

Cet ID d’activité reste le même pour toute la durée de la demande et est consigné dans le cadre de chaque événement enregistré dans le observateur d’événements pour cette demande. Cela signifie que :
 - que le filtrage ou la recherche de l’observateur d’événements à l’aide de cet ID d’activité peut aider à effectuer le suivi de tous les événements connexes qui correspondent à la demande de jeton
 - le même ID d’activité est enregistré sur différentes machines, ce qui vous permet de résoudre les problèmes liés à une demande de l’utilisateur sur plusieurs ordinateurs, tels que le serveur proxy de Fédération (FSP).
 - l’ID d’activité s’affiche également dans le navigateur de l’utilisateur en cas d’échec de la demande AD FS, ce qui permet à l’utilisateur de communiquer cet ID à l’assistance ou au support informatique.

![ActivityID](media/ad-fs-tshoot-logging/activityid2.png)

Pour faciliter le processus de dépannage, AD FS enregistre également l’événement ID d’appelant chaque fois que le processus d’émission de jetons échoue sur un serveur AD FS. Cet événement contient le type de revendication et la valeur de l’un des types de revendication suivants, en supposant que ces informations ont été passées au service FS (Federation Service) dans le cadre d’une demande de jeton :
- https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountnameh
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upnh
- https://schemas.microsoft.com/ws/2008/06/identity/claims/upn
- http://schemas.xmlsoap.org/claims/UPN
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddressh
- https://schemas.microsoft.com/ws/2008/06/identity/claims/emailaddress 
- http://schemas.xmlsoap.org/claims/EmailAddress
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
- https://schemas.microsoft.com/ws/2008/06/identity/claims/name
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier 

L’événement ID d’appelant enregistre également l’ID d’activité pour vous permettre d’utiliser cet ID d’activité pour filtrer ou rechercher une requête particulière dans les journaux des événements.




## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)
