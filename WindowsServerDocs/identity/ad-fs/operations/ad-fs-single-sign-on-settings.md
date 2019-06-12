---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: Paramètres d’authentification unique ADFS2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 97e1fa441c5fe4fb7d23743387392732663326de
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501588"
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS Single Sign-On Settings

L’authentification unique (SSO) permet aux utilisateurs de s’authentifier une seule fois et accéder à plusieurs ressources sans avoir à saisir les informations d’identification supplémentaires.  Cet article décrit le comportement d’AD FS par défaut pour l’authentification unique, ainsi que les paramètres de configuration qui vous permettent de personnaliser ce comportement.  

## <a name="supported-types-of-single-sign-on"></a>Types pris en charge de l’authentification unique

AD FS prend en charge plusieurs types d’expériences Single Sign-On :  
  
-   **Session de l’authentification unique**  
  
     Les cookies de session SSO sont écrits pour l’utilisateur authentifié, ce qui élimine les autres invites quand l’utilisateur passe d’applications pendant une session particulière. Toutefois, si une session particulière se termine, l’utilisateur sera invité pour leurs informations d’identification à nouveau.  
  
     AD FS définira session cookies d’authentification unique par défaut si les appareils des utilisateurs ne sont pas enregistrés. Si la session de navigateur s’est terminée et est redémarrée, ce cookie de session est supprimé et n’est pas plus valid.  
  
-   **Authentification unique persistante**  
  
     Les cookies persistants SSO sont écrits pour l’utilisateur authentifié, ce qui élimine les invites lorsque l’utilisateur pour les applications tant que le cookie persistant de l’authentification unique est valide. La différence entre persistant de l’authentification unique et de session SSO est que l’authentification unique persistante peut être maintenue entre les différentes sessions.  
  
     AD FS définit les cookies SSO persistants si l’appareil est inscrit. AD FS sera également définir un cookie persistant de l’authentification unique si un utilisateur sélectionne l’option « maintenir la connexion ». Si le cookie d’authentification unique persistant n’est plus valid, il est rejeté et supprimé.  
  
-   **Authentification unique spécifique d’application**  
  
     Dans le scénario OAuth, un jeton d’actualisation est utilisé pour maintenir l’état de l’authentification unique de l’utilisateur dans l’étendue d’une application particulière.  
  
     Si un appareil est inscrit, AD FS définit le délai d’expiration d’un jeton d’actualisation en fonction de la durée de vie de cookies SSO persistante pour un appareil inscrit qui est de 7 jours par défaut pour 2012 R2 AD FS et avec un maximum de 90 jours avec AD FS 2016 s’ils utilisent leur appareil pour accéder aux ressources AD FS dans une fenêtre de 14 jours. 

Si l’appareil n’est pas enregistré mais un utilisateur sélectionne l’option « maintenir la connexion », le délai d’expiration du jeton d’actualisation est égale à la durée de vie des cookies persistante SSO pour « maintenir la que connexion » qui est 1 jour par défaut avec un maximum de 7 jours. Sinon, durée de vie est égale à session SSO cookie durée de vie actualisation qui est de 8 heures par défaut  
  
 Comme mentionné ci-dessus, les utilisateurs sur les appareils inscrits obtiennent toujours une authentification unique persistante, sauf si l’authentification unique persistante est désactivé. Pour les appareils non inscrits, l’authentification unique persistante peut être obtenue en activant le « maintenir la connexion » fonctionnalité de (la connexion KMSI). 
 
 Pour Windows Server 2012 R2, pour activer PSSO pour le scénario « Maintenir la connexion », vous devez installer ce [correctif](https://support.microsoft.com/en-us/kb/2958298/) qui est également dans le cadre de la de [correctif cumulatif d’août 2014 pour Windows RT 8.1, Windows 8.1 et Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).   

Tâche | PowerShell | Description
------------ | ------------- | -------------
Activer/désactiver l’authentification unique persistante | ```` Set-AdfsProperties –EnablePersistentSso <Boolean> ````| Authentification unique persistante est activée par défaut. Si elle est désactivée, aucun cookie PSSO ne sera écrit.
« Activer/désactiver « maintenir la connexion » | ```` Set-AdfsProperties –EnableKmsi <Boolean> ```` | Fonctionnalité « Maintenir la connexion » est désactivée par défaut. Si elle est activée, l’utilisateur final verra un choix « maintenir la connexion » sur la page de connexion AD FS



## <a name="ad-fs-2016---single-sign-on-and-authenticated-devices"></a>AD FS 2016 - Single Sign-On et appareils authentifiés
AD FS 2016 modifie le PSSO lorsque le demandeur est l’authentification à partir d’un appareil inscrit augmenter jusqu'à 90 jours maximum, mais nécessitant une authentification dans un délai de 14 jours (fenêtre de l’utilisation de périphérique).
Après avoir entré les informations d’identification pour la première fois, par défaut les utilisateurs d’appareils inscrits obtiennent single Sign-On pour une période maximale de 90 jours, ils utilisent l’appareil pour accéder aux ressources AD FS au moins une fois tous les 14 jours.  S’ils attendent 15 jours après avoir entré les informations d’identification, les utilisateurs seront invités à nouveau pour les informations d’identification.  

Authentification unique persistante est activée par défaut. Si elle est désactivée, aucun cookie PSSO ne sera écrit. |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
La fenêtre de l’utilisation du périphérique (14 jours par défaut) est régie par la propriété AD FS **DeviceUsageWindowInDays**.

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
La seule authentification durée maximale (90 jours par défaut) est régie par la propriété AD FS **PersistentSsoLifetimeMins**.

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>Conserver la connexion pour les appareils non authentifiés 
Pour les appareils non inscrits, la période de l’authentification unique est déterminée par le **conserver Me connecté dans la connexion (KMSI)** les paramètres des fonctionnalités.  Maintenir la connexion est désactivée par défaut et peut être activée en définissant la propriété AD FS KmsiEnabled sur True.

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

Avec la fonctionnalité KMSI désactivée, la période par défaut unique l’authentification est 8 heures.  Cela peut être configuré à l’aide de la propriété SsoLifetime.  La propriété est mesurée en minutes, par conséquent, sa valeur par défaut est de 480.  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

Avec la fonctionnalité KMSI est activée, la période par défaut unique de session est de 24 heures.  Cela peut être configuré à l’aide de la propriété KmsiLifetimeMins.  La propriété est mesurée en minutes, par conséquent, sa valeur par défaut est 1440.

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>Comportement de l’authentification multifacteur (MFA)  
Il est important de noter que, tout en fournissant des périodes relativement longues de l’authentification unique sur, AD FS invitera à une authentification supplémentaire (l’authentification multifacteur) quand une ouverture de session précédente était basée sur les informations d’identification principales et pas l’authentification Multifacteur, mais en cours d’authentification exige l’authentification Multifacteur.  Il s’agit, quel que soit la configuration de l’authentification unique. AD FS, lorsqu’elle reçoit une demande d’authentification, détermine tout d’abord si il existe un contexte de l’authentification unique (par exemple, un cookie), puis, si l’authentification Multifacteur est requise (par exemple si la demande provient en dehors) il évalue si le contexte de l’authentification unique contient MFA ou non.  Si ce n’est pas le cas, MFA est invitée.  


  
## <a name="psso-revocation"></a>Révocation PSSO  
 Pour protéger la sécurité, AD FS rejette tout cookie persistant de l’authentification unique précédemment émis lorsque les conditions suivantes sont remplies. Cela nécessitera l’utilisateur à fournir leurs informations d’identification pour s’authentifier avec AD FS à nouveau. 
  
- Le mot de passe utilisateur modifications  
  
- Paramètre de l’authentification unique persistante est désactivé dans AD FS  
  
- Appareil est désactivé par l’administrateur dans le cas de perte ou de vol  
  
- AD FS reçoit un cookie persistant de l’authentification unique qui est émis pour un utilisateur inscrit, mais l’utilisateur ou de l’appareil n’est pas inscrit plus  
  
- AD FS reçoit un cookie persistant de l’authentification unique pour un utilisateur inscrit, mais l’utilisateur inscrit à nouveau  
  
- AD FS reçoit un cookie persistant de l’authentification unique qui est émis à la suite de « maintenir la connexion » mais « maintenir la connexion » paramètre est désactivé dans AD FS  
  
- AD FS reçoit un cookie persistant de l’authentification unique qui est émis pour un utilisateur inscrit mais le certificat de l’appareil est manquante ou modifiée lors de l’authentification  
  
- Administrateur de AD FS a défini une heure de coupure pour l’authentification unique persistante. Lorsque cela est configuré, AD FS rejette tout cookie persistant de l’authentification unique émis avant cette date  
  
  Pour définir l’heure de coupure, exécutez l’applet de commande PowerShell suivante :  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>Activer PSSO pour les utilisateurs d’Office 365 pour accéder à SharePoint Online  
 Une fois que PSSO est activé et configuré dans AD FS, AD FS écrit un cookie persistant après qu’un utilisateur est authentifié. La prochaine fois que l’utilisateur s’affiche, si un cookie persistant est toujours valide, un utilisateur n’a pas besoin fournir des informations d’identification pour s’authentifier à nouveau. Vous pouvez également éviter l’invite d’authentification supplémentaires pour Office 365 et SharePoint Online aux utilisateurs en configurant les deux revendications règles dans AD FS pour la persistance de déclencheur chez Microsoft Azure AD et SharePoint Online.  Pour activer PSSO pour les utilisateurs d’Office 365 accéder à SharePoint online, vous devez installer ce [correctif](https://support.microsoft.com/en-us/kb/2958298/) qui est également dans le cadre de la de [correctif cumulatif d’août 2014 pour Windows RT 8.1, Windows 8.1 et Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).  
  
 Une règle de transformation d’émission pour passer à la revendication InsideCorporateNetwork  
  
```  
@RuleTemplate = "PassThroughClaims"  
@RuleName = "Pass through claim - InsideCorporateNetwork"  
c:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"]  
=> issue(claim = c);   
A custom Issuance Transform rule to pass through the persistent SSO claim  
@RuleName = "Pass Through Claim - Psso"  
c:[Type == "http://schemas.microsoft.com/2014/03/psso"]  
=> issue(claim = c);  
  
```
  
Résumé :
<table>
  <tr>
    <th colspan="1">Expérience d’authentification unique</th>
    <th colspan="3">ADFS 2012 R2 <br> Appareil est-il enregistré ?</th>
        <th colspan="1"></th>
    <th colspan="3">ADFS 2016 <br> Appareil est-il enregistré ?</th>
  </tr>

  <tr align="center">
    <th></th>
    <th>NON</th>
    <th>NON, mais de maintenir la connexion</th>
    <th>OUI</th>
    <th></th>
    <th>NON</th>
    <th>NON, mais de maintenir la connexion</th>
    <th>OUI</th>
  </tr>
 <tr align="center">
    <td>L’authentification unique =&gt;définir un jeton d’actualisation =&gt;</td>
    <td>8 heures</td>
    <td>N/A</td>
    <td>N/A</td>
    <th></th>
    <td>8 heures</td>
    <td>N/A</td>
    <td>N/A</td>
  </tr>

 <tr align="center">
    <td>PSSO =&gt;définir un jeton d’actualisation =&gt;</td>
    <td>N/A</td>
    <td>24 heures</td>
    <td>7 derniers jours</td>
    <th></th>
    <td>N/A</td>
    <td>24 heures</td>
    <td>Max 90 jours avec la fenêtre de 14 jours</td>
  </tr>

 <tr align="center">
    <td>Durée de vie</td>
    <td>1 h</td>
    <td>1 h</td>
    <td>1 h</td>
    <th></th>
    <td>1 h</td>
    <td>1 h</td>
    <td>1 h</td>
  </tr>
</table>

**APPAREIL inscrit ?** Vous obtenez un PSSO / persistant de l’authentification unique <br>
**APPAREIL non inscrit ?** Vous obtenez une authentification unique <br>
**APPAREIL non inscrit mais maintenir la connexion ?** Vous obtenez un PSSO / persistant de l’authentification unique <p>
IF :
 - [x] administrateur a activé la fonctionnalité de maintenir la connexion [et]
 - [x] utilisateur clique sur la case à cocher maintenir la connexion sur la page de connexion de formes
 
**Bon à savoir :** <br>
Les utilisateurs fédérés qui n’ont pas la **LastPasswordChangeTimestamp** attribut synchronisé sont émis les cookies de session et jetons d’actualisation qui ont un **valeur âge maximal de 12 heures**.<br>
Cela se produit, car Azure AD ne peut pas déterminer à quel moment révoquer les jetons qui sont liées à des informations d’identification ancien (par exemple, un mot de passe qui a été modifié). Par conséquent, Azure AD doit vérifier plus fréquemment pour vous assurer que l’utilisateur et les jetons associés sont toujours d’arriérés.
