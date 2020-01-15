---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: Paramètres d’authentification unique AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 76c34dc518f4578b4ae2ead3459f1d79c191b3d7
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949198"
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS les paramètres d’authentification unique

L’authentification unique (SSO) permet aux utilisateurs de s’authentifier une seule fois et d’accéder à plusieurs ressources sans être invité à fournir des informations d’identification supplémentaires.  Cet article décrit le comportement de AD FS par défaut pour l’authentification unique, ainsi que les paramètres de configuration qui vous permettent de personnaliser ce comportement.  

## <a name="supported-types-of-single-sign-on"></a>Types d’authentification unique pris en charge

AD FS prend en charge plusieurs types d’expériences d’authentification unique :  
  
-   **Authentification unique de session**  
  
     Les cookies SSO de session sont écrits pour l’utilisateur authentifié, ce qui élimine les invites supplémentaires lorsque l’utilisateur bascule des applications pendant une session particulière. Toutefois, si une session particulière se termine, l’utilisateur est invité à entrer à nouveau ses informations d’identification.  
  
     AD FS définira les cookies SSO de session par défaut si les appareils des utilisateurs ne sont pas inscrits. Si la session du navigateur est terminée et qu’elle est redémarrée, ce cookie de session est supprimé et n’est plus valide.  
  
-   **Authentification unique permanente**  
  
     Les cookies SSO persistants sont écrits pour l’utilisateur authentifié, ce qui élimine les invites supplémentaires lorsque l’utilisateur bascule les applications tant que le cookie SSO persistant est valide. La différence entre l’authentification unique permanente et l’authentification unique de session est que l’authentification unique permanente peut être conservée entre différentes sessions.  
  
     AD FS définira des cookies d’authentification unique persistants si l’appareil est inscrit. AD FS définira également un cookie SSO persistant si un utilisateur sélectionne l’option « maintenir la connexion ». Si le cookie SSO persistant n’est plus valide, il sera rejeté et supprimé.  
  
-   **Authentification unique spécifique à l’application**  
  
     Dans le scénario OAuth, un jeton d’actualisation est utilisé pour maintenir l’état de l’authentification unique de l’utilisateur dans l’étendue d’une application particulière.  
  
     Si un appareil est inscrit, AD FS définit l’heure d’expiration d’un jeton d’actualisation en fonction de la durée de vie des cookies SSO persistants pour un appareil inscrit qui est de 7 jours par défaut pour AD FS 2012 R2 et jusqu’à un maximum de 90 jours avec AD FS 2016 s’ils utilisent leur appareil pour Accédez aux ressources de AD FS dans une fenêtre de 14 jours. 

Si l’appareil n’est pas inscrit mais qu’un utilisateur sélectionne l’option « maintenir la connexion », l’heure d’expiration du jeton d’actualisation sera égale à la durée de vie des cookies SSO persistants pour « maintenir la connexion », soit 1 jour par défaut, avec un maximum de 7 jours. Sinon, la durée de vie du jeton d’actualisation est égale à la durée de vie du cookie SSO de session, qui est de 8 heures par défaut  
  
 Comme indiqué ci-dessus, les utilisateurs sur les appareils inscrits obtiendront toujours une authentification unique persistante, sauf si l’authentification unique persistante est désactivée. Pour les appareils non inscrits, l’authentification unique permanente peut être obtenue en activant la fonctionnalité « maintenir la connexion » (KMSI). 
 
 Pour Windows Server 2012 R2, pour activer PSSO pour le scénario « maintenir la connexion », vous devez installer ce [correctif](https://support.microsoft.com/kb/2958298/) qui fait également partie du correctif [cumulatif de 2014 d’août pour Windows RT 8,1, Windows 8.1 et Windows Server 2012 R2](https://support.microsoft.com/kb/2975719).   

Tâche | PowerShell | Description
------------ | ------------- | -------------
Activer/désactiver l’authentification unique permanente | ```` Set-AdfsProperties –EnablePersistentSso <Boolean> ````| L’authentification unique permanente est activée par défaut. Si elle est désactivée, aucun cookie PSSO n’est écrit.
« Activer/désactiver » maintenir la connexion» | ```` Set-AdfsProperties –EnableKmsi <Boolean> ```` | La fonctionnalité « maintenir la connexion » est désactivée par défaut. S’il est activé, l’utilisateur final voit un choix « maintenir la connexion » sur AD FS page de connexion



## <a name="ad-fs-2016---single-sign-on-and-authenticated-devices"></a>AD FS 2016-authentification unique et appareils authentifiés
AD FS 2016 modifie le PSSO lorsque le demandeur s’authentifie à partir d’un appareil inscrit qui passe à 90 jours maximum, mais nécessite une authentification dans un délai de 14 jours (fenêtre utilisation de l’appareil).
Après avoir fourni les informations d’identification pour la première fois, par défaut, les utilisateurs disposant d’appareils inscrits obtiennent une authentification unique pour une période maximale de 90 jours, à condition qu’ils utilisent l’appareil pour accéder aux ressources AD FS au moins une fois tous les 14 jours.  S’ils attendent 15 jours après avoir fourni des informations d’identification, les utilisateurs seront de nouveau invités à saisir leurs informations d’identification.  

L’authentification unique permanente est activée par défaut. Si elle est désactivée, aucun cookie PSSO ne sera écrit. |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
La fenêtre utilisation de l’appareil (14 jours par défaut) est régie par la propriété AD FS **DeviceUsageWindowInDays**.

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
La période d’authentification unique maximale (90 jours par défaut) est régie par la propriété AD FS **PersistentSsoLifetimeMins**.

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>Maintenir la connexion pour les appareils non authentifiés 
Pour les appareils non inscrits, la période d’authentification unique est déterminée par les paramètres de la fonctionnalité maintenir la connexion **(KMSI)** .  KMSI est désactivé par défaut et peut être activé en affectant à la propriété AD FS KmsiEnabled la valeur true.

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

Avec KMSI désactivé, la période d’authentification unique par défaut est de 8 heures.  Cela peut être configuré à l’aide de la propriété SsoLifetime.  La propriété est mesurée en minutes, donc sa valeur par défaut est 480.  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

Lorsque KMSI est activé, la période d’authentification unique par défaut est de 24 heures.  Cela peut être configuré à l’aide de la propriété KmsiLifetimeMins.  La propriété est mesurée en minutes, donc sa valeur par défaut est 1440.

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>Comportement de Multi-Factor Authentication (MFA)  
Il est important de noter que, tout en fournissant des périodes relativement longues d’authentification unique, AD FS demande une authentification supplémentaire (Multi-Factor Authentication) quand une connexion précédente était basée sur des informations d’identification principales et non MFA, mais sur l’authentification active requiert MFA.  Quelle que soit la configuration de l’authentification unique. AD FS, lorsqu’il reçoit une demande d’authentification, détermine d’abord s’il existe un contexte d’authentification unique (tel qu’un cookie), puis, si l’authentification multifacteur est requise (par exemple, si la demande provient de l’extérieur), elle évalue si le contexte SSO contient ou non MFA.  Dans le cas contraire, l’authentification MFA est demandée.  


  
## <a name="psso-revocation"></a>Révocation PSSO  
 Pour protéger la sécurité, AD FS rejette tout cookie SSO persistant précédemment émis lorsque les conditions suivantes sont remplies. Cela oblige l’utilisateur à fournir ses informations d’identification pour s’authentifier auprès de AD FS. 
  
- L’utilisateur change le mot de passe  
  
- Le paramètre SSO persistant est désactivé dans AD FS  
  
- L’appareil est désactivé par l’administrateur en cas de perte ou de vol  
  
- AD FS reçoit un cookie SSO persistant émis pour un utilisateur inscrit, mais l’utilisateur ou l’appareil n’est plus inscrit  
  
- AD FS reçoit un cookie SSO persistant pour un utilisateur inscrit mais l’utilisateur a été réinscrit  
  
- AD FS reçoit un cookie SSO persistant qui est émis à la suite du paramètre « maintenir la connexion » mais que le paramètre « maintenir la connexion » est désactivé dans AD FS  
  
- AD FS reçoit un cookie SSO persistant qui est émis pour un utilisateur inscrit mais dont le certificat de périphérique est manquant ou modifié pendant l’authentification  
  
- AD FS administrateur a défini un délai de coupure pour l’authentification unique permanente. Une fois cette configuration effectuée, AD FS rejette tous les cookies d’authentification unique persistants émis avant cette heure  
  
  Pour définir le temps de coupure, exécutez l’applet de commande PowerShell suivante :  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>Autoriser PSSO pour les utilisateurs d’Office 365 à accéder à SharePoint Online  
 Une fois PSSO activé et configuré dans AD FS, AD FS écrit un cookie persistant après l’authentification d’un utilisateur. La prochaine fois que l’utilisateur arrivera, si un cookie persistant est toujours valide, un utilisateur n’a pas besoin de fournir des informations d’identification pour s’authentifier à nouveau. Vous pouvez également éviter l’invite d’authentification supplémentaire pour les utilisateurs d’Office 365 et SharePoint Online en configurant les deux règles de revendication suivantes dans AD FS pour déclencher la persistance sur Microsoft Azure AD et SharePoint Online.  Pour permettre à PSSO pour les utilisateurs d’Office 365 d’accéder à SharePoint Online, vous devez installer ce [correctif](https://support.microsoft.com/kb/2958298/) qui fait également partie du [correctif cumulatif de 2014 d’août pour Windows RT 8,1, Windows 8.1 et Windows Server 2012 R2](https://support.microsoft.com/kb/2975719).  
  
 Une règle de transformation d’émission à passer par la revendication InsideCorporateNetwork  
  
```  
@RuleTemplate = "PassThroughClaims"  
@RuleName = "Pass through claim - InsideCorporateNetwork"  
c:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"]  
=> issue(claim = c);   
A custom Issuance Transform rule to pass through the persistent SSO claim  
@RuleName = "Pass Through Claim - Psso"  
c:[Type == "https://schemas.microsoft.com/2014/03/psso"]  
=> issue(claim = c);  
  
```
  
Pour résumer :
<table>
  <tr>
    <th colspan="1">Expérience d’authentification unique</th>
    <th colspan="3">ADFS 2012 R2 <br> L’appareil est-il inscrit ?</th>
        <th colspan="1"></th>
    <th colspan="3">ADFS 2016 <br> L’appareil est-il inscrit ?</th>
  </tr>

  <tr align="center">
    <th></th>
    <th>NON</th>
    <th>NON, mais KMSI</th>
    <th>OUI</th>
    <th></th>
    <th>NON</th>
    <th>NON, mais KMSI</th>
    <th>OUI</th>
  </tr>
 <tr align="center">
    <td>SSO =&gt;définir le jeton d’actualisation =&gt;</td>
    <td>8 h</td>
    <td>NON APPLICABLE</td>
    <td>NON APPLICABLE</td>
    <th></th>
    <td>8 h</td>
    <td>NON APPLICABLE</td>
    <td>NON APPLICABLE</td>
  </tr>

 <tr align="center">
    <td>PSSO =&gt;définir le jeton d’actualisation =&gt;</td>
    <td>NON APPLICABLE</td>
    <td>24 h</td>
    <td>7 jours</td>
    <th></th>
    <td>NON APPLICABLE</td>
    <td>24 h</td>
    <td>90 jours maximum avec une fenêtre de 14 jours</td>
  </tr>

 <tr align="center">
    <td>Durée de vie du jeton</td>
    <td>1 h</td>
    <td>1 h</td>
    <td>1 h</td>
    <th></th>
    <td>1 h</td>
    <td>1 h</td>
    <td>1 h</td>
  </tr>
</table>

**Appareil inscrit ?** Vous recevez un PSSO/une authentification unique persistante <br>
**Appareil non inscrit ?** Vous recevez une authentification unique <br>
**Appareil non inscrit mais KMSI ?** Vous recevez un PSSO/une authentification unique persistante <p>
QUE
 - [x] l’administrateur a activé la fonctionnalité KMSI [et]
 - [x] l’utilisateur clique sur la case à cocher KMSI sur la page de connexion aux formulaires
 
**Bon à savoir :** <br>
Les utilisateurs fédérés qui n’ont pas l’attribut **LastPasswordChangeTimestamp** synchronisé reçoivent des cookies de session et des jetons d’actualisation qui ont une **valeur d’âge maximale de 12 heures**.<br>
Cela est dû au fait que Azure AD ne peut pas déterminer quand révoquer les jetons associés à des informations d’identification anciennes (par exemple, un mot de passe qui a été modifié). Par conséquent, Azure AD doit vérifier plus fréquemment pour s’assurer que l’utilisateur et les jetons associés sont toujours en bonne position.
