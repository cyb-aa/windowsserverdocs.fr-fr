---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurer la Protection par verrouillage Extranet AD FS
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 02/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 904b563da2f1404d873c7352db9eadb7bfe252f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869760"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS Extranet Lockout et Extranet verrouillage intelligent

# <a name="overview"></a>Vue d'ensemble

>S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Dans AD FS sur Windows Server 2012 R2, nous avons introduit une fonctionnalité de sécurité appelée [Extranet Lockout réversible](configure-ad-fs-extranet-soft-lockout-protection.md).  Avec cette fonctionnalité, AD FS arrête l’authentification des utilisateurs à partir de l’extranet pour une période donnée.  Cela empêche vos comptes d’utilisateur de leur compte est verrouillé dans Active Directory. Outre la protection de vos utilisateurs à partir d’un verrouillage de compte AD, verrouillage extranet AD FS protège également contre le mot de passe par force brute deviner les attaques.

En juin 2018, AD FS sur Windows Server 2016 introduit **verrouillage intelligent Extranet (ESL)**.  ESL permet à AD FS faire la distinction entre les tentatives de connexion qui semblent à partir de l’utilisateur valide et les connexions à partir de ce qui peut représenter une personne malveillante. Par conséquent, AD FS peut verrouiller des attaquants tout en laissant les utilisateurs valides à continuer à utiliser leurs comptes. Cela empêche un déni de service sur l’utilisateur et que vous protège contre les attaques ciblées telles que les attaques de « mot de passe-vaporisation ».  
ESL est disponible pour AD FS dans Windows Server 2016 et est intégré à AD FS dans Windows Server 2019.

> [!NOTE]
> Cette fonctionnalité fonctionne uniquement pour le **scénario extranet** dans les demandes d’authentification fournis par le biais du Proxy d’Application Web et s’applique uniquement aux **nom d’utilisateur et mot de passe de l’authentification**.

## <a name="advantages-of-extranet-smart-lockout-in-ad-fs-2016"></a>Avantages de l’Extranet verrouillage intelligent dans AD FS 2016
Verrouillage de manière réversible extranet dans AD FS 2012 R2 fourni les avantages clés suivants :
- Protège vos comptes d’utilisateur à partir de **attaques en force brute** dans lequel un pirate tente de deviner le mot de passe d’un utilisateur en envoyant des demandes d’authentification en continu et à partir de **les attaques de mot de passe pulvérisation** où les attaquants tentent d’utiliser des mots de passe communs avec un grand nombre de comptes
- Protège vos comptes d’utilisateur à partir de **le verrouillage de compte Active Directory** des demandes d’authentification malveillant avec des mots de passe incorrects. Dans ce cas, bien que le compte d’utilisateur sera verrouillé pour l’accès extranet, l’utilisateur peut toujours la connexion à Active Directory à partir du réseau d’entreprise. Il s’agit une **réversible verrouillage**.

Verrouillage intelligent extranet s’appuie sur les avantages de verrouillages de manière réversible extranet en ajoutant les éléments suivants :
- Protège vos utilisateurs à partir de la rencontre **verrouillage de compte extranet** des demandes d’authentification malveillant.  Verrouillage intelligent empêche les demandes potentiellement malveillants provenant d’emplacements inconnus tout en autorisant l’utilisateur réel se connectent à partir de l’extranet à partir des emplacements connus (emplacements à partir de laquelle l’utilisateur s’est connecté avant).
- A un seul mode de journal afin que le système peut apprendre signon bon et potentiellement malveillant activité sans désactiver tous les comptes

## <a name="additional-advantages-of-extranet-smart-lockout-in-ad-fs-2019"></a>Autres avantages de l’Extranet de verrouillage intelligent dans AD FS 2019
Le verrouillage intelligent extranet dans AD FS 2019 ajoute les avantages suivants par rapport à AD FS 2016 :
- Définir des seuils de verrouillage indépendants pour les emplacements inconnus et familiers afin que les utilisateurs à un emplacement adéquat peuvent avoir plus de place pour l’erreur que les requêtes à partir d’emplacements suspects
- Activer le mode d’audit pour le verrouillage intelligent tout en continuant à appliquer le comportement de verrouillage de manière réversible précédent

## <a name="pre-requisites-for-extranet-smart-lockout-in-ad-fs-2016"></a>Conditions préalables pour le verrouillage intelligent Extranet dans AD FS 2016
Les conditions préalables suivantes sont requises pour ESL avec AD FS 2019.

### <a name="install-updates-on-all-nodes-in-the-farm"></a>Installer les mises à jour sur tous les nœuds dans la batterie de serveurs
Tout d’abord, vérifiez tous les serveurs Windows Server 2016 AD FS sont à jour depuis les mises à jour de juin 2018 Windows et que la batterie de serveurs AD FS 2016 est exécuté au niveau du comportement de batterie de serveurs 2016.

### <a name="update-artifact-database-permissions"></a>Mettre à jour les autorisations de base de données d’artefact
Verrouillage intelligent extranet nécessite le compte de service AD FS pour disposer des autorisations dans une nouvelle table dans la base de données AD FS artefact.  Cette autorisation en exécutant la commande suivante dans une fenêtre de commande PowerShell :
``` powershell
PS C:\>$cred = Get-Credential
PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
```
Où `$cred` est un compte disposant des autorisations d’administrateur AD FS (autorisations d’administrateur AD FS sont requises pour que la base de données à modifier.)

>[!NOTE]
>Dans une batterie de serveurs multiples qui utilise la base de données WID, l’applet de commande ci-dessus nécessite que Windows Remote management soit activé sur chaque serveur AD FS

Si vous n’avez pas les autorisations d’administrateur AD FS, vous pouvez configurer les autorisations de base de données manuellement dans SQL ou WID en exécutant la commande suivante en cas de connexion à la base de données AdfsArtifactStore.
```
sp_addrolemember 'db_owner', 'db_genevaservice'
```
### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Assurez-vous que AD FS sécurité l’enregistrement d’Audit est activée
Cette fonctionnalité utilise un Audit de sécurité journaux, par conséquent, l’audit doivent être activées dans AD FS, ainsi que la stratégie locale sur tous les serveurs AD FS.

## <a name="pre-requisites-for-extranet-smart-lockout-in-ad-fs-2019"></a>Conditions préalables pour le verrouillage intelligent Extranet dans AD FS 2019
Les conditions préalables suivantes sont requises pour ESL avec AD FS 2016.

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Assurez-vous que AD FS sécurité l’enregistrement d’Audit est activée
Cette fonctionnalité utilise un Audit de sécurité journaux, par conséquent, l’audit doivent être activées dans AD FS, ainsi que la stratégie locale sur tous les serveurs AD FS.

## <a name="lockout-settings"></a>Paramètres de verrouillage
Verrouillage intelligent extranet se compose d’un ensemble de nouvelles fonctionnalités régi par les propriétés AD FS nouvelle et existantes.

### <a name="extranet-lockout-enabled"></a>Verrouillage extranet est activé
Verrouillage intelligent extranet utilise la même propriété AD FS qui précédemment a été utilisée pour contrôler le verrouillage extranet « soft ».  La propriété est appelée ExtranetLockoutEnabled et peut être consultée via Get-AdfsProperties.

### <a name="extranet-smart-lockout-mode"></a>Mode de verrouillage intelligent extranet
Une nouvelle propriété AD FS appelée ExtranetLockoutMode a été ajoutée pour contrôler le comportement de verrouillage « soft » intelligent Visual Studio.  Il peut être définie par le biais de Set-AdfsProperties et contient 3 valeurs :

    - **ADPasswordCounter** – il s’agit la hérité de mode de « verrouillage extranet de manière réversible » ADFS qui ne fait pas de distinction selon l’emplacement.  Valeur par défaut.

    - **ADFSSmartLockoutLogOnly** : il s’agit d’Extranet de verrouillage intelligent, mais au lieu de rejeter les demandes d’authentification, AD FS seront uniquement admin et audit d’événements d’écriture.

    - **ADFSSmartLockoutEnforce** -il s’agit d’Extranet de verrouillage intelligent avec prise en charge complète pour le blocage des demandes inconnus lorsque les seuils sont atteints.

Dans AD FS 2019, les valeurs ADPasswordCounter et ADFSSmartLockoutLogOnly peuvent être combinés afin que le verrouillage de manière réversible continue d’être appliquée pendant que vous préparez pour le verrouillage intelligent.

### <a name="lockout-threshold-and-observation-window"></a>Seuil de verrouillage et de la fenêtre d’Observation
Verrouillage intelligent dans AD FS 2019 utilise les mêmes deux propriétés AD FS en tant que le verrouillage souple utilisé précédemment : ExtranetObservationWindow et ExtranetLockoutThreshold.

- **ExtranetLockoutThreshold &lt;entier&gt;**  définit le nombre maximal de tentatives de mot de passe incorrect. Une fois que le seuil est atteint, dans ADFSSmartLockoutEnforce mode AD FS rejette les demandes provenant de l’extranet jusqu'à ce que la fenêtre d’observation se soit écoulé.  En mode ADFSSmartLockoutLogOnly, AD FS écrira uniquement les entrées de journal.  
- **ExtranetObservationWindow &lt;TimeSpan&gt;**  ce paramètre détermine la durée pendant laquelle nom d’utilisateur et mot de passe les demandes provenant d’emplacements inconnus sont verrouillés. AD FS commence à effectuer à nouveau l’authentification nom d’utilisateur et mot de passe quand la fenêtre est passée.

> [!NOTE]
> Fonctions de verrouillage extranet AD FS indépendamment des stratégies de verrouillage AD. Nous vous recommandons de définir le **ExtranetLockoutThreshold** valeur de paramètre à une valeur qui est inférieure au seuil de verrouillage de compte AD. Ne parvient pas à le faire entraînerait l’impossibilité de se protéger des comptes à partir de leur compte est verrouillé dans Active Directory Federation Services. 

Dans AD FS 2019, nous avons introduit un nouveau seuil de verrouillage spécifique à un emplacement adéquat : ExtranetLockoutThresholdFamiliarLocation.
- **ExtranetLockoutThresholdFamiliarLocation &lt;entier&gt;**  définit le nombre maximal de tentatives de mot de passe à partir des emplacements connus. Dans AD FS 2019, le paramètre d’origine ExtranetLockoutThreshold s’applique à des emplacements non connus (adresses IP ne pas reconnus comme fiables).

### <a name="primary-domain-controller-requirement"></a>Exigence de contrôleur de domaine principal
AD FS 2016 propose un paramètre qui permet à un autre contrôleur de domaine lorsque le contrôleur de domaine principal n’est pas disponible.

- **ExtranetLockoutRequirePDC &lt;booléenne&gt;**  lorsque activé, le verrouillage extranet requiert un contrôleur de domaine principal (PDC). Lorsque désactivé, verrouillage extranet recourra à un autre contrôleur de domaine au cas où le contrôleur de domaine principal n’est pas disponible.

   L’exemple suivant montre l’applet de commande pour activer le verrouillage de l’exigence de contrôleur de domaine principal désactivé :

    ```powershell
    Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
    ```

## <a name="configuring-ad-fs-with-smart-lockout-in-log-only-mode"></a>Configuration d’AD FS avec verrouillage intelligent en Mode de journal uniquement

### <a name="ad-fs-2016"></a>AD FS 2016
Nous vous recommandons de tout d’abord définir le comportement de verrouillage pour vous connecter uniquement en exécutant l’applet de commande suivante :

 ```powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly
 ```

Dans ce mode, AD FS remplit les informations utilisateurs emplacement connu et écrit les événements d’audit de sécurité, mais n’empêche pas toutes les demandes.  Ce mode est utilisé pour valider que le verrouillage intelligent est en cours d’exécution et pour activer AD FS pour « découvrir » des emplacements connus pour les utilisateurs avant d’activer « mode d’application ».
Que AD FS a connaissance, elle stocke l’activité de connexion par utilisateur (s’il en mode uniquement de journal ou en mode d’application). 

>[!NOTE]
>Configuration `ExtranetLockoutMode` à `AdfsSmartlockoutLogOnly` entraîne l’ancien comportement de AD FS « soft verrouillage extranet » ne peut plus être en vigueur, même si le `EnableExtranetLockout` propriété est définie sur True.  Cela signifie que les utilisateurs qui dépassent les seuils de verrouillage à partir des adresses IP inconnues ou familières ne sont pas être bloqués par le verrouillage intelligent de AD FS. Toutefois, sur site AD peut le verrouillage de l’utilisateur en fonction de la configuration.   Consultez [stratégie de verrouillage de compte](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/account-lockout-policy) pour en savoir plus comment local AD peut les utilisateurs de verrouillage. »  Cela est destiné à être un état temporaire afin que le système peut apprendre le comportement de connexion antérieur à la nouvelle introduction mise en œuvre de verrouillage avec le nouveau comportement de verrouillage intelligent.

Pour le nouveau mode entrent en vigueur, redémarrez le service AD FS sur tous les nœuds dans la batterie de serveurs
  
  ``` powershell
PS C:\>Restart-service adfssrv
  ```
Une fois que le mode est configuré, vous pouvez activer le verrouillage intelligent à l’aide du `EnableExtranetLockout` paramètre


``` powershell
PS C:\>Set-AdfsProperties -EnableExtranetLockout $true
```

Notez que vous pouvez utiliser l’applet de commande pour désactiver le verrouillage

Exemple : Désactiver le verrouillage

``` powershell
PS C:\>Set-AdfsProperties -EnableExtranetLockout $false
```
### <a name="ad-fs-2019"></a>AD FS 2019
Si vous n’utilisez pas actuellement de verrouillage de manière réversible Extranet AD FS, nous vous recommandons de suivre les mêmes recommandations que pour AD FS 2016 ci-dessus.
Si vous utilisez le verrouillage de manière réversible, toutefois, nous vous recommandons de définir le comportement de verrouillage AD FS 2019 pour vous connecter pour le verrouillage intelligent, mais conserver l’application de verrouillage de manière réversible, à l’aide du powershell ci-dessous :

 ```powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode 3
 ```

Une fois que vous exécutez cette applet de commande, vous pouvez ensuite utiliser Get-AdfsProperties pour interroger la valeur de la propriété ExtranetLockoutMode AD FS.  Vous verrez que sa valeur a été mis à jour à une combinaison au niveau du bit de ADPasswordCounter et ADFSSmartLockoutLogOnly.

## <a name="observing-audit-events"></a>En observant les événements d’Audit
AD FS écrira les événements de verrouillage extranet dans le journal d’audit de sécurité :
-   Quand un utilisateur est verrouillé hors (atteint le seuil de verrouillage des tentatives de connexion ayant échoué)
-   Quand AD FS reçoit une tentative de connexion pour un utilisateur qui est déjà dans un état de verrouillage

En mode journal uniquement, vous pouvez vérifier le journal d’audit de sécurité pour les événements de verrouillage.  Pour tous les événements trouvés, vous pouvez vérifier l’état de l’utilisateur à l’aide de l’applet de commande Get-ADFSAccountActivity pour déterminer si le verrouillage s’est produite à partir des adresses IP familiers ou inconnus et pour vérifier la liste d’adresses d’IP familiers pour cet utilisateur.

Exemple d’événement :
```
Log Name:      Security
Source:        AD FS Auditing
Date:          5/21/2018 12:55:59 AM
Event ID:      1210
Task Category: (3)
Level:         Information
Keywords:      Classic,Audit Failure
User:          CONTOSO\adfssvc
Computer:      ADFS2016FS1.corp.contoso.com
Description:
An extranet lockout event has occurred. See XML for failure details. 

Activity ID: fa7a8052-0694-48f0-84e2-b51cde40ac3d 

Additional Data 
XML: <?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://fs.contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>CONTOSO\user</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Extranet</NetworkLocation>
      <IpAddress>64.187.173.10</IpAddress>
      <ForwardedIpAddress>64.187.173.10</ForwardedIpAddress>
      <ProxyIpAddress>N/A</ProxyIpAddress>
      <NetworkIpAddress>N/A</NetworkIpAddress>
      <ProxyServer>ADFS2016PROXY2</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls/</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>05/21/2018 00:55:05</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase>
Event Xml:
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
  <System>
    <Provider Name="AD FS Auditing" />
    <EventID Qualifiers="0">1210</EventID>
    <Level>0</Level>
    <Task>3</Task>
    <Keywords>0x8090000000000000</Keywords>
    <TimeCreated SystemTime="2018-05-21T00:55:59.921880300Z" />
    <EventRecordID>35521235</EventRecordID>
    <Channel>Security</Channel>
    <Computer>ADFS2016FS1.contoso.com</Computer>
    <Security UserID="S-1-5-21-1156273042-1594504307-2076964089-1104" />
  </System>
  <EventData>
    <Data>fa7a8052-0694-48f0-84e2-b51cde40ac3d</Data>
    <Data><?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://fs.contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>CONTOSO\user</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Extranet</NetworkLocation>
      <IpAddress>64.187.173.10</IpAddress>
      <ForwardedIpAddress>64.187.173.10</ForwardedIpAddress>
      <ProxyIpAddress>N/A</ProxyIpAddress>
      <NetworkIpAddress>N/A</NetworkIpAddress>
      <ProxyServer>ADFS2016PROXY2</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls/</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>05/21/2018 00:55:05</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase></Data>
  </EventData>
</Event>
```

## <a name="observing-user-activity"></a>En observant l’activité des utilisateurs
AD FS fournit des applets de commande powershell pour afficher et gérer les données d’activité utilisateur compte.  Pour lire l’activité de compte en cours pour un compte d’utilisateur.  Utilisez l’applet de commande ci-dessous

``` powershell
PS C:\>Get-ADFSAccountActivity user@contoso.com
```

Exemple de sortie
```
Identifier             : CONTOSO\user
BadPwdCountFamiliar    : 0
BadPwdCountUnknown     : 0
LastFailedAuthFamiliar : 1/1/0001 12:00:00 AM
LastFailedAuthUnknown  : 1/1/0001 12:00:00 AM
FamiliarLockout        : False
UnknownLockout         : False
FamiliarIps            : {}
```

La sortie d’activité actuel contient les données suivantes :

**Identificateur**: il s’agit du nom d’utilisateur

**BadPwdCountFamiliar**: il s’agit du nombre actuel de tentatives de connexion de mot de passe incorrect à partir d’adresses IP qui étaient dans la liste des « FamiliarIps » au moment de la tentative de

**BadPwdCountUnknown**: il s’agit du nombre actuel de tentatives de connexion de mot de passe incorrect à partir d’adresses IP qui n’étaient pas dans la liste des « FamiliarIps » au moment de la tentative de

**LastFailedAuthFamiliar**: il s’agit du temps de la dernière tentative de connexion de mot de passe incorrect à partir d’une adresse IP qui était dans la liste des « FamiliarIps » au moment de la tentative de

**LastFailedAuthUnknown**: il s’agit du temps de la dernière tentative de connexion de mot de passe incorrect à partir d’une adresse IP qui n’était pas dans la liste des « FamiliarIps » au moment de la tentative de

**FamiliarLockout**: cela indique si l’utilisateur est actuellement dans un état de verrouillage des tentatives de mot de passe correct à partir d’adresses IP dans la liste des « FamiliarIps » 

**UnknownLockout**: cela indique si l’utilisateur est actuellement dans un état de verrouillage des tentatives de mot de passe correct à partir d’adresses IP non sur la liste des « FamiliarIps » FamiliarIps : Voici la liste actuelle des adresses d’IP familiers pour l’utilisateur

## <a name="adjust-threshold-and-window"></a>Ajuster le seuil et la fenêtre
Une fois que vous avez exécuté en mode uniquement de journal pour une durée suffisante pour AD FS pour en savoir plus les emplacements de connexion, vous pouvez souhaiter ajuster la fenêtre seuil ou d’observation à partir des paramètres par défaut.  Cette opération est effectuée à l’aide `Set-AdfsProperties` comme dans les exemples ci-dessous :

La fenêtre de l’observation est définie à l’aide de `ExtranetObservationWindow`:

Exemple : 

``` powershell
PS C:\>Set-AdfsProperties -ExtranetObservationWindow ( new-timespan -minutes 30 )
```

Où la valeur est une valeur TimeSpan.

### <a name="setting-threshold-value-in-ad-fs-2016"></a>Valeur de seuil de paramètre dans AD FS 2016
Dans AD FS 2016, le seuil est défini à l’aide de ExtranetLockoutThreshold :

Exemple :

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThreshold 5
```

### <a name="setting-threshold-values-in-ad-fs-2019"></a>Définition des valeurs de seuil dans AD FS 2019
Dans AD FS 2019, il existe des valeurs de seuil distinctes pour des emplacements connus de bons et inconnus

Pour définir la valeur de seuil pour les emplacements non connus, utilisez la même propriété utilisée pour AD FS 2016 ci-dessus :

Exemple :

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThreshold 5
```

Pour définir la valeur de seuil pour un emplacement adéquat, utilisez la nouvelle propriété ExtranetLockoutThresholdFamiliarLocation, comme indiqué dans l’exemple ci-dessous :

Exemple :

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThresholdFamiliarLocation 10
```


## <a name="enable-enforce-mode"></a>Activer en mode d’application
Une fois que vous exécutez en mode de journal uniquement de temps suffisant pour AD FS pour en savoir plus les emplacements de connexion et à observer toute activité de verrouillage, et une fois que vous êtes familiarisé avec la fenêtre de seuil et observation du verrouillage, verrouillage intelligent peut être déplacé vers « appliquer » à l’aide du mode le Applet de commande PSH ci-dessous :

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce
```

Pour le nouveau mode entrent en vigueur, redémarrez le service AD FS sur tous les nœuds dans la batterie de serveurs

``` powershell
PS C:\>Restart-service adfssrv
```


## <a name="manage-user-account-activity"></a>Gérer l’activité de compte d’utilisateur
AD FS fournit 3 applets de commande pour gérer les données d’activité utilisateur compte.  Ces applets de commande se connectent automatiquement au nœud dans la batterie de serveurs qui détient le rôle de maître (bien que ce comportement peut être remplacé en transmettant le paramètre - Server).

> [!NOTE] 
> Pour plus d’informations sur la délégation des autorisations requises pour utiliser ces applets de commande, consultez [déléguer AD FS un accès applet de commande Powershell pour les utilisateurs non-administrateurs](delegate-ad-fs-pshell-access.md)

Ces applets de commande sont :

`Get-ADFSAccountActivity`

Lire l’activité de compte en cours pour un compte d’utilisateur.  L’applet de commande se connecte automatiquement au contrôleur de batterie de serveurs à l’aide du point de terminaison REST d’activité de compte, toutes les données doivent toujours être cohérentes

``` powershell
Get-ADFSAccountActivity user@contoso.com
```
`
Set-ADFSAccountActivity
`

Mettre à jour de l’activité de compte pour un compte d’utilisateur.  Cela peut servir à ajouter de nouveaux emplacements connus ou effacer l’état de n’importe quel compte

``` powershell
Set-ADFSAccountActivity user@upnsuffix.com -FamiliarLocation “1.2.3.4”
```
`Reset-ADFSAccountLockout`

Réinitialise le compteur de verrouillage pour un compte d’utilisateur

``` powershell
Reset-ADFSAccountLockout user@upnsuffix.com -Familiar
```

## <a name="troubleshooting-esl"></a>Résolution des problèmes de ESL
Les éléments suivants peuvent vous aider à résoudre la fonctionnalité verrouillage intelligent extranet.

### <a name="updating-database-permissions-for-esl"></a>La mise à jour des autorisations de base de données pour ESL
Si des erreurs sont renvoyées à partir de la `Update-AdfsArtifactDatabasePermission` applet de commande, vérifiez les points suivants

1.  La liste des nœuds de batterie de serveurs est correcte.  Si un nœud est dans la liste de la batterie de serveurs AD FS, mais n’est plus active la vérification de correctif logiciel échoue.  Ce problème peut être solutionné qu’en exécutant `remove-adfsnode <node name >`
2.  Vérifier que le correctif logiciel est déployé sur tous les nœuds dans la batterie de serveurs
3.  Vérifiez les informations d’identification passées à l’applet de commande sont autorisé à modifier le propriétaire du schéma de base de données ad fs artefact.  

### <a name="logging--auditing"></a>Journalisation / audit
Lorsqu’une demande d’authentification est refusée, car le compte dépasse le seuil de verrouillage, AD FS écrira un `ExtranetLockoutEvent` dans le flux d’audit de sécurité.  

Exemple d’événement :

Verrouillage extranet d’un événement. Afficher le code XML pour les détails de l’échec. 

**ID d’activité : 172332e1-1301-4e56-0e00-0080000000db**

```
Additional Data 
XML: <?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>TQDFTD\Administrator</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Intranet</NetworkLocation>
      <IpAddress>4.4.4.4</IpAddress>
      <ForwardedIpAddress />
      <ProxyIpAddress>1.2.3.4</ProxyIpAddress>
      <NetworkIpAddress>1.2.3.4</NetworkIpAddress>
      <ProxyServer>N/A</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>02/07/2018 21:47:44</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase>

```

## <a name="banned-ip-addresses"></a>Adresses IP interdites
Outre les fonctionnalités de verrouillage intelligent extranet, la mise à jour de juin 2018 AD FS vous permet de configurer un ensemble d’adresses IP dans le monde entier dans AD FS, afin que les demandes provenant de ces adresses IP, ou qui ont ces adresses IP le **x-forwarded-for**  ou **x-ms-transférés-client-ip** en-têtes, sera bloqué par AD FS.

##### <a name="adding-banned-ips"></a>Ajout d’adresses IP interdites
Pour ajouter des adresses IP interdite à la liste globale, utilisez l’applet de commande Powershell ci-dessous :

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

Formats autorisés

1.  IPv4
2.  IPv6
3.  Format CIDR avec IPv4 ou IPv6
4.  Plage d’adresses IP avec IPv4 ou IPv6 (par exemple, 1.2.3.4-1.2.3.6)

#### <a name="removing-banned-ips"></a>Suppression d’adresses IP interdites
Pour supprimer des adresses IP interdites à partir de la liste globale, utilisez l’applet de commande Powershell ci-dessous :

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>Lecture d’adresses IP interdites
Pour lire l’ensemble actuel des adresses IP interdites, utilisez l’applet de commande Powershell ci-dessous :

``` powershell
PS C:\ >Get-AdfsProperties 
```

Exemple de sortie :

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>Références supplémentaires  
[Meilleures pratiques pour la sécurisation d’Active Directory Federation Services](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)

    
