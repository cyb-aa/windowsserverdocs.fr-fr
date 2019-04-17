---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: "ADFS2016 d’authentification unique sur les paramètres"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e8c24399949efc1b8d0b1782e299593c02374c62
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-single-sign-on-settings"></a>ADFS Single Sign-On paramètres

>S’applique à: Windows Server2016, Windows Server2012R2

Single Sign-On (SSO) permet aux utilisateurs de s’authentifier une seule fois et accéder à plusieurs ressources sans demande d’informations d’identification supplémentaires.  Cet article décrit le comportement d’ADFS par défaut pour l’authentification unique, ainsi que les paramètres de configuration qui vous permettent de personnaliser ce comportement.  

## <a name="supported-types-of-single-sign-on"></a>Types pris en charge de Single Sign-On

ADFS prend en charge plusieurs types d’expériences Single Sign-On:  
  
-   **Session de l’authentification unique**  
  
     Les cookies de session unique sont écrits pour l’utilisateur authentifié, ce qui élimine les invites supplémentaires lorsque l’utilisateur bascule applications lors d’une session particulière. Toutefois, si une session particulière se termine, l’utilisateur sera invité leurs informations d’identification à nouveau.  
  
     ADFS définit session les cookies d’authentification unique par défaut si les appareils des utilisateurs ne sont pas enregistrés. Si la session de navigateur est terminée et est redémarrée, ce cookie de session est supprimé et n’est pas valide plus.  
  
-   **Authentification unique persistante**  
  
     Les cookies persistants SSO sont écrits pour l’utilisateur authentifié, ce qui élimine les invites lorsque l’utilisateur bascule pour les applications tant que le cookie d’authentification unique persistant est valide. La différence entre persistant l’authentification unique et l’authentification unique de session est que l’authentification unique persistante peut être conservée entre différentes sessions.  
  
     ADFS définit les cookies persistants SSO si l’appareil est inscrit. ADFS définit un cookie d’authentification unique persistant également si un utilisateur sélectionne l’option «rester connecté». Si le cookie SSO persistant n’est plus valide, il sera rejetée et supprimé.  
  
-   **Authentification unique spécifique d’application**  
  
     Dans le scénario OAuth, un jeton d’actualisation est utilisé pour maintenir l’état de l’authentification unique de l’utilisateur dans le cadre d’une application particulière.  
  
     Si un appareil est inscrit, ADFS définit le délai d’expiration d’un jeton d’actualisation de la fonction de la durée de vie les cookies persistante l’authentification unique pour un appareil inscrit qui est de 7jours par défaut. Si un utilisateur sélectionne l’option «rester connecté», le délai d’expiration du jeton d’actualisation est égale à la durée de vie les cookies persistante l’authentification unique pour «keep m’enregistré dans» qui est de 1 jour par défaut de 7jours maximum. Dans le cas contraire, actualisation de la durée de vie du jeton est égale à l’authentification unique cookie durée de vie session qui est de 8heures par défaut  
  
 Comme indiqué ci-dessus, les utilisateurs sur les appareils inscrits toujours obtenir une authentification unique persistante, sauf si l’authentification unique persistante est désactivée. Pour les appareils non enregistrés, l’authentification unique persistante peut être obtenue à l’activation de la «garder m’enregistré dans» fonctionnalité (KMSI). 
 
 Pour Windows Server2012R2, pour activer PSSO pour le scénario «Rester connecté», vous devez installer ce [correctif](https://support.microsoft.com/en-us/kb/2958298/) qui est également inclus dans le de [août 2014 correctif cumulatif pour WindowsRT8.1, Windows8.1 et Windows Server2012R2](https://support.microsoft.com/en-us/kb/2975719).   
 
  
## <a name="single-sign-on-and-authenticated-devices"></a>Single Sign-On et périphériques authentifiés  
Après avoir fournissant des informations d’identification pour la première fois, par défaut les utilisateurs avec les appareils inscrits obtiennent single Sign-On pendant une période maximale de 90jours, condition qu’ils utilisent l’appareil pour accéder aux ressources ADFS au moins une fois tous les 14jours.  S’ils attendent 15jours après fournissant des informations d’identification, les utilisateurs seront invités pour les informations d’identification à nouveau.  

L’authentification unique persistante est activée par défaut. Si elle est désactivée, aucun cookie PSSO n’est écrit. |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
La fenêtre de l’utilisation du périphérique (14jours par défaut) est régie par la propriété ADFS **DeviceUsageWindowInDays**.

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
La seule Sign-On durée maximale (90jours par défaut) est régie par la propriété ADFS **PersistentSsoLifetimeMins**.

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>Rester connecté pour les appareils non authentifiées 
Pour les appareils non enregistrée, la période d’ouverture de session unique est déterminée par le **conserver Me connecté dans (KMSI)** paramètres de fonctionnalité.  KMSI est désactivé par défaut et peut être activée en définissant la propriété ADFS KmsiEnabled sur True.

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

Avec KMSI désactivé, la période par défaut unique ouverture de session est de 8heures.  Cela peut être configuré à l’aide de la propriété SsoLifetime.  La propriété est mesurée en minutes, donc sa valeur par défaut est de 480.  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

Avec KMSI activé, la période par défaut unique ouverture de session est 24heures.  Cela peut être configuré à l’aide de la propriété KmsiLifetimeMins.  La propriété est exprimée en minutes, sa valeur par défaut est 1440.

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>Comportement de l’authentification multifacteur (MFA)  
Il est important de noter que, en offrant relativement longtemps d’authentification unique, ADFS demande une authentification supplémentaire (multi-Factor authentication) lorsqu’une ouverture de session précédente est basée sur les informations d’identification principales et pas l’authentification Multifacteur, mais en cours de session exige l’authentification Multifacteur.  Il s’agit, quelle que soit la configuration de l’authentification unique. ADFS, lorsqu’il reçoit une demande d’authentification, détermine d’abord ou non un contexte de l’authentification unique (par exemple, un cookie) est et, si l’authentification Multifacteur est requise (par exemple si la demande provient à l’extérieur) il évalue le contexte de l’authentification unique contient l’authentification Multifacteur ou non.  Si ce n’est pas le cas, l’authentification Multifacteur est invité.  


  
## <a name="psso-revocation"></a>Révocation PSSO  
 Pour protéger la sécurité, ADFS rejette tout cookie SSO persistante précédemment émis lorsque les conditions suivantes sont remplies. Cela nécessite l’utilisateur à fournir leurs informations d’identification afin d’authentifier à nouveau avec ADFS. 
  
-   Mot de passe utilisateur modifications  
  
-   Paramètre de l’authentification unique persistante est désactivé dans ADFS  
  
-   Périphérique est désactivé par l’administrateur dans le cas de perte ou de vol  
  
-   ADFS reçoit un cookie SSO persistant est émis pour un utilisateur inscrit, mais l’utilisateur ou le périphérique n’est pas enregistré plus  
  
-   ADFS reçoit un cookie SSO persistant pour un utilisateur inscrit, mais l’utilisateur de nouveau enregistré  
  
-   ADFS reçoit un cookie SSO persistant qui est émis à la suite de «rester connecté» mais «rester connecté» est désactivé dans ADFS  
  
-   ADFS reçoit un cookie SSO persistant est émis pour un utilisateur inscrit mais le certificat de périphérique est manquant ou altéré lors de l’authentification  
  
-   Administrateur de ADFS a défini les six fois pour l’authentification unique persistante. Lorsque ce paramètre est configuré, ADFS rejettent tout cookie SSO persistante émis avant cette heure  
  
 Pour définir l’heure limite, exécutez l’applet de commande PowerShell suivante:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>Activer PSSO Office 365 accessible aux utilisateurs SharePoint Online  
 Une fois PSSO est activé et configuré dans ADFS, ADFS écrit un cookie persistant après qu’un utilisateur authentifié. La prochaine fois que l’utilisateur est fourni, si un cookie persistant est toujours valide, un utilisateur n’a pas besoin de fournir des informations d’identification pour s’authentifier à nouveau. Vous pouvez également éviter l’invite d’authentification supplémentaires pour Office 365 et SharePoint Online utilisateurs en configurant les deux règles dans ADFS à la persistance de déclencheur chez MicrosoftAzure AD et SharePoint Online de revendications.  Pour activer PSSO Office 365 accessible aux utilisateurs SharePoint online, vous devez installer ce [correctif](https://support.microsoft.com/en-us/kb/2958298/) qui est également inclus dans le de [août 2014 correctif cumulatif pour WindowsRT8.1, Windows8.1 et Windows Server2012R2](https://support.microsoft.com/en-us/kb/2975719).  
  
 Une règle de transformation d’émission à passer par le biais de la revendication InsideCorporateNetwork  
  
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
  
  
    


