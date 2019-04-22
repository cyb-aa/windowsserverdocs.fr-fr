---
title: AD FS, résolution des problèmes - événements d’audit et journalisation
description: Ce document décrit comment utiliser les différents journaux AD FS pour résoudre les problèmes
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 20e2d0747b98e7c7728230d0768506261f5b0d50
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825120"
---
# <a name="ad-fs-troubleshooting---events-and-logging"></a>Résolution des problèmes d’AD FS - événements et la journalisation
AD FS fournit deux journaux principales qui peuvent être utilisées dans la résolution des problèmes.  Celles-ci sont les suivantes :

- le journal de l’administrateur
- le journal des traces  
 
Chacune de ces journaux est expliquée ci-dessous.

## <a name="admin-log"></a>Journal de l’administrateur
Le journal de l’administrateur fournit des informations détaillées sur les problèmes qui sont produisent et sont activés par défaut.

### <a name="to-view-the-admin-log"></a>Pour afficher le journal de l’administrateur
1.  Ouvrez l’observateur d’événements
2.  Développez **Applications et Services journal**.
3.  Développez **AD FS**.
4.  Cliquez sur **administrateur**.

![améliorations d’audit](media/ad-fs-tshoot-logging/event1.PNG)  

## <a name="trace-log"></a>Journal des traces
Le journal des traces est où les messages détaillés sont enregistrés et seront le journal plus utiles lors du dépannage. Étant donné que beaucoup d’informations de journal de trace peut être générée dans un court laps de temps, ce qui peut avoir un impact sur les performances du système, les journaux de suivi sont désactivées par défaut. 

### <a name="to-enable-and-view-the-trace-log"></a>Pour activer et afficher le journal des traces
1.  Ouvrez l’observateur d’événements
2.  Avec le bouton droit sur **Applications et Services journal** , sélectionnez la vue, cliquez sur **afficher les journaux d’analyse et de débogage**.  Cette commande affiche les nœuds supplémentaires sur la gauche.
![améliorations d’audit](media/ad-fs-tshoot-logging/event2.PNG)  
3.  Développez le suivi AD FS
4.  Avec le bouton droit sur le débogage et sélectionnez **activer le journal**.
![améliorations d’audit](media/ad-fs-tshoot-logging/event3.PNG)  


## <a name="event-auditing-information-for-ad-fs-on-windows-server-2016"></a>Informations d’événement audit pour AD FS sur Windows Server 2016  
Par défaut, AD FS dans Windows Server 2016 a un niveau de base de l’audit est activé.  Avec l’audit de base, les administrateurs voient maximum 5 événements pour une demande unique.  Cela marque une réduction significative du nombre d’événements, les administrateurs disposent d’examiner, afin de voir une demande unique.   Le niveau d’audit peut être augmentée ou abaissée à l’aide de l’applet de commande PowerShell :  

```PowerShell
Set-AdfsProperties -AuditLevel 
```

Le tableau ci-dessous explique les niveaux d’audit disponibles.  

|Niveau d'audit|Syntaxe de PowerShell|Description|  
|----- | ----- | ----- |
|Aucune|Set-AdfsProperties - AuditLevel None|L’audit est désactivé et aucun événement ne sera consigné.|  
|Basic (valeur par défaut)|Set-AdfsProperties - AuditLevel Basic|Pas plus de 5 événements seront enregistrés pour une demande unique|  
|Verbose|Set-AdfsProperties - AuditLevel détaillée|Tous les événements seront enregistrés.  Ceci enregistrera une quantité significative d’informations par demande.|  
  
Pour afficher le niveau d’audit, vous pouvez utiliser l’applet de commande PowerShell :  Get-AdfsProperties.  
  
![améliorations d’audit](media/ad-fs-tshoot-logging/ADFS_Audit_1.PNG)  
  
Le niveau d’audit peut être augmentée ou abaissée à l’aide de l’applet de commande PowerShell :  Set-AdfsProperties - AuditLevel.  
  
![améliorations d’audit](media/ad-fs-tshoot-logging/ADFS_Audit_2.png)  
  
## <a name="types-of-events"></a>Types d’événements  
Les événements AD FS peuvent être de types différents, selon les différents types de demandes traitées par AD FS. Chaque type d’événement a des données spécifiques associées.  Le type d’événements peut être différencié entre les demandes de connexion (par exemple, les demandes de jeton) par rapport aux demandes du système (appels y compris l’extraction des informations de configuration de serveur).    

Le tableau ci-dessous décrit les types de base d’événements.  
  
|Type d'événement|ID d’événement|Description| 
|----- | ----- | ----- | 
|Réussite de la Validation des informations d’identification fraîches|1202|Une demande où les nouvelles informations d’identification sont validées avec succès par le Service de fédération. Cela inclut WS-Trust, WS-Federation, SAML-P (premier tronçon pour générer l’authentification unique) et les points de terminaison autoriser OAuth.|  
|Erreur de Validation des informations d’identification fraîches|1203|Une demande où Échec de la validation de nouvelles informations d’identification sur le Service de fédération. Cela inclut WS-Trust, WS-Fed, SAML-P (premier tronçon pour générer l’authentification unique) et les points de terminaison autoriser OAuth.|  
|Jeton de l’application réussi|1200|Une demande où un jeton de sécurité est émis avec succès par le Service de fédération. Pour WS-Federation, SAML-P, elle est consignée lorsque la demande est traitée avec l’artefact d’authentification unique. (par exemple, le cookie d’authentification unique).|  
|Échec de jeton d’application|1201|Une demande où les d’émission de jeton de sécurité a échoué sur le Service de fédération. Pour WS-Federation, SAML-P, elle est consignée lorsque la demande a été traitée avec l’artefact d’authentification unique. (par exemple, le cookie d’authentification unique).|  
|Demande de modification de mot de passe réussie|1204|Une transaction où la demande de modification du mot de passe a été correctement traitée par le Service de fédération.|  
|Erreur de demande de modification de mot de passe|1205|Une transaction où la demande de modification du mot de passe a échoué doivent être traités par le Service de fédération.| 
|Déconnectez-vous de réussite|1206|Décrit une demande de déconnexion réussie.|  
|Déconnectez-vous de défaillance|1207|Décrit une demande de déconnexion a échoué.|  

## <a name="security-auditing"></a>Audit de sécurité
L’audit de sécurité du compte de service AD FS peut parfois aider à le dépistage des problèmes de mises à jour du mot de passe, journalisation de demande/réponse, les en-têtes de concordances de requête et résultats de l’inscription d’appareil.  L’audit du compte de service AD FS est désactivé par défaut.

### <a name="to-enable-security-auditing"></a>Pour activer l’audit de sécurité
1.       Cliquez sur Démarrer, pointez sur **programmes**, pointez sur **outils d’administration**, puis cliquez sur **stratégie de sécurité locale**.
2.       Accédez au dossier **Paramètres de sécurité\Stratégies locales\Gestion des droits utilisateur**, puis double-cliquez sur **Générer des audits de sécurité**.
3.       Sur le **paramètre de sécurité locale** , vérifiez que le compte de service AD FS est répertorié. Si elle n’est pas présente, cliquez sur Ajouter un utilisateur ou groupe et ajoutez-le à la liste, puis cliquez sur OK.
4.       Ouvrez une invite de commandes avec élévation de privilèges et exécutez la commande suivante pour activer l’audit auditpol.exe /set/SubCategory : « Application générée » /failure:enable /success:enable 5.       Fermer **stratégie de sécurité locale**, puis ouvrez le composant logiciel enfichable Gestion AD FS.
 
Pour ouvrir le composant logiciel enfichable Gestion AD FS, cliquez sur Démarrer, pointez sur Programmes, pointez sur Outils d’administration, puis cliquez sur gestion AD FS.
 
6.       Dans le volet Actions, cliquez sur Modifier les propriétés de Service fédération 7.       Dans la boîte de dialogue Propriétés du Service de fédération, cliquez sur l’onglet d’événements. 8.       Sélectionnez le **audits des succès** et **audits des échecs** cases à cocher.
9.       Cliquez sur OK.

![améliorations d’audit](media/ad-fs-tshoot-logging/event4.PNG)  
 
>[!NOTE]
>Les instructions ci-dessus sont utilisées uniquement lorsque ADFS se trouve sur un serveur membre autonome.  Si AD FS est en cours d’exécution sur un contrôleur de domaine, au lieu de la stratégie de sécurité locale, utilisez la **stratégie des contrôleurs de domaine par défaut** situé dans **contrôleurs de gestion/forêt/domaines/domaines de stratégie de groupe**.  Cliquez sur Modifier et accédez à **ordinateur Configuration\Policies\Windows Settings\Security Settings\Local Policies\User Rights Management**

## <a name="windows-communication-foundation-and-windows-identity-foundation-messages"></a>Messages Windows Communication Foundation et Windows Identity Foundation
En plus de la journalisation du suivi, parfois, vous devrez peut-être afficher les messages Windows Communication Foundation (WCF) et Windows Identity Foundation (WIF) afin de résoudre un problème. Cela est possible en modifiant le **Microsoft.IdentityServer.ServiceHost.Exe.Config** fichier sur le serveur AD FS. 

Ce fichier se trouve dans **< % système racine % > \Windows\ADFS** et est au format XML. Les parties pertinentes du fichier sont présentées ci-dessous : 
```
<!-- To enable WIF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="Microsoft.IdentityModel" switchValue="Off"> … </source>

<!-- To enable WCF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="System.ServiceModel" switchValue="Off" > … </source>
```


Après avoir appliqué ces modifications, enregistrez la configuration et redémarrez le service AD FS. Une fois que vous activez ces traces en définissant les commutateurs appropriés, ils apparaîtront dans le journal de suivi AD FS dans l’Observateur d’événements Windows.

## <a name="correlating-events"></a>Corrélation des événements
Une des opérations plus difficiles à résoudre les problèmes est des problèmes d’accès de générer un grand nombre d’erreur ou déboguer des événements.

Pour cela, AD FS met en corrélation tous les événements qui sont enregistrés dans l’Observateur d’événements, dans l’administrateur et les journaux de débogage, qui correspondent à une requête particulière à l’aide d’un unique identificateur global Unique (GUID) appelé l’ID d’activité. Cet ID est généré lorsque la demande d’émission de jeton est initialement présentée à l’application web (pour les applications utilisant le profil du demandeur passif) ou les demandes envoyées directement au fournisseur de revendications (pour les applications à l’aide de WS-Trust). 

![activityid](media/ad-fs-tshoot-logging/activityid1.png)

Cet ID d’activité reste la même pendant toute la durée de la demande et est enregistré comme partie de chaque événement enregistré dans l’événement visionneuse pour cette demande. Cela signifie que :
 - que le filtrage ou la recherche de l’Observateur d’événements à l’aide de cette activité ID peut aider à effectuer le suivi de tous les événements associés qui correspondent à la demande de jeton
 - le même ID d’activité est connecté sur différents ordinateurs, ce qui vous permet de résolution des problèmes d’une demande d’utilisateur sur plusieurs ordinateurs, tels que le serveur proxy de fédération (FSP)
 - l’ID d’activité apparaît également dans le navigateur de l’utilisateur si la demande d’AD FS échoue de quelque façon, permettant ainsi l’utilisateur à communiquer cet ID au support technique ou le Support informatique.

![activityid](media/ad-fs-tshoot-logging/activityid2.png)

Pour faciliter le processus de dépannage, AD FS consigne également l’événement d’ID d’appelant chaque fois que le processus d’émission de jeton échoue sur un serveur AD FS. Cet événement contient le type de revendication et la valeur d’un des types de revendication suivants, en supposant que ces informations a été passées au Service de fédération dans le cadre d’une demande de jeton :
- http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountnameh
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upnh
- http://schemas.microsoft.com/ws/2008/06/identity/claims/upn
- http://schemas.xmlsoap.org/claims/UPN
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddressh
- http://schemas.microsoft.com/ws/2008/06/identity/claims/emailaddress 
- http://schemas.xmlsoap.org/claims/EmailAddress
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
- http://schemas.microsoft.com/ws/2008/06/identity/claims/name
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier 

L’événement d’ID d’appelant enregistre également l’ID d’activité afin que vous puissiez utiliser cet ID d’activité pour filtrer ou rechercher les journaux des événements pour une requête particulière.




## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes de AD FS](ad-fs-tshoot-overview.md)
