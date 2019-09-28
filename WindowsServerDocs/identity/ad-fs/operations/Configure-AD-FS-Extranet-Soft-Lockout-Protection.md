---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurer AD FS protection par verrouillage extranet
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/01/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bb5958f8205271fe3ab2258ed9812ae03f2a0be0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358209"
---
# <a name="configure-ad-fs-extranet-lockout-protection"></a>Configurer AD FS protection par verrouillage extranet

Dans AD FS sur Windows Server 2012 R2, nous avons introduit une fonctionnalité de sécurité appelée verrouillage extranet.  Avec cette fonctionnalité, AD FS « arrêtez » l’authentification du compte d’utilisateur « malveillant » hors de l’extérieur pendant un certain temps.  Cela empêche les comptes d’utilisateurs d’être verrouillés dans Active Directory.  Outre la protection de vos utilisateurs contre le verrouillage de compte Active Directory, AD FS le verrouillage extranet protège également contre les attaques par déduction de mot de passe en force brute

> [!NOTE]
> Cette fonctionnalité fonctionne uniquement pour le **scénario extranet** dans lequel les demandes d’authentification transitent par le proxy d’application Web et s’appliquent uniquement à l' **authentification par nom d’utilisateur et mot de passe**.

## <a name="advantages-of-extranet-lockout"></a>Avantages du verrouillage extranet
Le verrouillage extranet offre les principaux avantages suivants :
- Il protège vos comptes d’utilisateur contre **les attaques en force brute** lorsqu’un pirate tente de deviner le mot de passe d’un utilisateur en envoyant continuellement des demandes d’authentification. Dans ce cas, AD FS verrouillera le compte d’utilisateur malveillant pour l’accès extranet
- Il protège vos comptes d’utilisateur contre le **verrouillage de compte malveillant** lorsqu’un attaquant souhaite verrouiller un compte d’utilisateur en envoyant des demandes d’authentification avec des mots de passe incorrects. Dans ce cas, bien que le compte d’utilisateur soit verrouillé par AD FS pour l’accès extranet, le compte d’utilisateur réel dans Active Directory n’est pas verrouillé et l’utilisateur peut toujours accéder aux ressources de l’entreprise au sein de l’organisation. C’est ce qu’on appelle un **verrouillage à chaud**.

## <a name="how-it-works"></a>Fonctionnement
Il existe 3 paramètres dans AD FS que vous devez configurer pour activer cette fonctionnalité : 
- **EnableExtranetLockout &lt;Boolean @ no__t-2** définissez cette valeur booléenne sur true si vous souhaitez activer le verrouillage extranet.
- **ExtranetLockoutThreshold &lt;Integer @ no__t-2** définit le nombre maximal de tentatives de mot de passe incorrectes. Une fois le seuil atteint, AD FS rejette immédiatement les demandes provenant de l’extranet sans tenter de contacter le contrôleur de domaine pour l’authentification, quel que soit le mot de passe est correct ou incorrect, jusqu’à ce que la fenêtre d’observation extranet soit transmise. Cela signifie que la valeur de l’attribut **badPwdCount** d’un compte Active Directory n’augmente pas lorsque le compte est verrouillé de manière réversible.
- **ExtranetObservationWindow &lt;TimeSpan @ no__t-2** détermine la durée pendant laquelle le compte d’utilisateur sera verrouillé de manière réversible. AD FS commencera à effectuer une nouvelle authentification du nom d’utilisateur et du mot de passe lorsque la fenêtre sera transmise. AD FS utilise l’attribut AD badPasswordTime comme référence pour déterminer si la fenêtre d’observation extranet a réussi ou non. La fenêtre est passée si l’heure actuelle > badPasswordTime + ExtranetObservationWindow. 

> [!NOTE]
> AD FS les fonctions de verrouillage extranet indépendamment des stratégies de verrouillage Active Directory. Toutefois, nous vous recommandons vivement de définir la valeur du paramètre **ExtranetLockoutThreshold** sur une valeur inférieure au seuil de verrouillage du compte Active Directory. Si vous ne le faites pas, AD FS ne peut pas empêcher le verrouillage des comptes dans Active Directory. 

Un exemple d’activation de la fonctionnalité de verrouillage extranet avec un maximum de 15 tentatives de mot de passe incorrect et une durée de verrouillage logiciel de 30 minutes est le suivant :

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30)
```

Ces paramètres s’appliquent à tous les domaines que le service AD FS peut authentifier. Dans ce cas, lorsque AD FS reçoit une demande d’authentification, il accède au contrôleur de domaine principal (PDC) via un appel LDAP et effectue une recherche de l’attribut **badPwdCount** pour l’utilisateur sur le contrôleur de domaine principal. Si AD FS trouve la valeur du paramètre **badPwdCount** > = ExtranetLockoutThreshold et que l’heure définie dans la fenêtre d’observation extranet n’est pas encore passée, AD FS rejette la demande immédiatement, ce qui signifie que l’utilisateur entre un bon ou un mauvais mot de passe de l’extranet, la connexion échoue car AD FS n’envoie pas les informations d’identification à Active Directory. AD FS ne conserve aucun État en ce qui concerne les comptes d’utilisateur **badPwdCount** ou verrouillés. AD FS utilise Active Directory pour tous les suivis d’État. 

> [!warning]
> Lorsque AD FS le verrouillage extranet sur le serveur 2012 R2 est activé, toutes les demandes d’authentification via le WAP sont validées par AD FS sur le contrôleur de domaine principal. Lorsque le contrôleur de domaine principal n’est pas disponible, les utilisateurs ne peuvent pas s’authentifier à partir de l’extranet.

Le serveur 2016 offre un paramètre supplémentaire qui permet à AD FS de revenir à un autre contrôleur de domaine lorsque le contrôleur de domaine principal n’est pas disponible :

- **ExtranetLockoutRequirePDC &lt;Boolean @ no__t-2** -quand cette option est activée : le verrouillage extranet requiert un contrôleur de domaine principal (PDC). En cas de désactivation : le verrouillage extranet fait un secours sur un autre contrôleur de domaine si le contrôleur de domaine principal n’est pas disponible.

Vous pouvez utiliser la commande Windows PowerShell suivante pour configurer l’AD FS le verrouillage extranet sur le serveur 2016 :

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```

## <a name="working-with-the-active-directory-lockout-policy"></a>Utilisation de la stratégie de verrouillage Active Directory
La fonctionnalité de verrouillage extranet dans AD FS fonctionne indépendamment de la stratégie de verrouillage Active Directory. Toutefois, vous devez vous assurer que les paramètres du verrouillage extranet sont correctement configurés afin qu’il puisse répondre à son objectif de sécurité avec la stratégie de verrouillage AD.
Examinons d’abord la stratégie de verrouillage AD. Il existe trois paramètres relatifs à la stratégie de verrouillage dans Active Directory :
- **Seuil de verrouillage de compte**: ce paramètre est semblable au paramètre ExtranetLockoutThreshold dans AD FS. Il détermine le nombre d’échecs de tentative de connexion qui entraînent le verrouillage d’un compte d’utilisateur. Pour protéger vos comptes d’utilisateur contre une attaque de verrouillage de compte malveillant, vous devez définir la valeur de ExtranetLockoutThreshold dans AD FS &lt; la valeur du seuil de verrouillage de compte dans Active Directory.
- **Durée de verrouillage de compte**: ce paramètre détermine la durée pendant laquelle un compte d’utilisateur est verrouillé. Ce paramètre n’a pas d’importance dans cette conversation, car le verrouillage extranet doit toujours se produire avant que le verrouillage Active Directory ne se produise correctement
- **Réinitialiser le compteur de verrouillage de compte après**: ce paramètre détermine le temps qui doit s’écouler après l’échec de la dernière ouverture de session de l’utilisateur avant que **badPwdCount** soit réinitialisé à 0. Pour que la fonctionnalité de verrouillage extranet dans AD FS fonctionne correctement avec la stratégie de verrouillage AD, vous devez vous assurer que la valeur de ExtranetObservationWindow dans AD FS &gt; la valeur réinitialiser le compteur de verrouillage du compte après dans Active Directory. Les exemples ci-dessous expliquent pourquoi.  

Jetons un coup d’œil à deux exemples et voyons comment **badPwdCount** change dans le temps en fonction de paramètres et d’États différents. Supposons dans les deux exemples **seuil de verrouillage de compte** = 4 et **ExtranetLockoutThreshold** = 2. La flèche **rouge** représente une tentative de mot de passe incorrecte, la flèche **verte** représente une bonne tentative de mot de passe. Dans l’exemple #1, **ExtranetObservationWindow** &gt; **Réinitialiser le compteur de verrouillage de compte après**. Dans l’exemple #2, **ExtranetObservationWindow** &lt; **Réinitialiser le compteur de verrouillage de compte après**. 

### <a name="example-1"></a>Exemple 1
![Example1](media/Configure-AD-FS-Extranet-Lockout-Protection/one.png)

### <a name="example-2"></a>Exemple 2
![Example1](media/Configure-AD-FS-Extranet-Lockout-Protection/two.png)

Comme vous pouvez le voir dans l’exemple ci-dessus, il existe deux conditions lorsque **badPwdCount** sera réinitialisé à 0. L’une d’entre elles est en cas de réussite de l’ouverture de session. L’autre est lorsqu’il est temps de réinitialiser ce compteur, comme défini dans le paramètre **Réinitialiser le compteur de verrouillage du compte après** . Si vous **réinitialisez le compteur de verrouillage de compte après** &lt; **ExtranetObservationWindow**, un compte ne risque pas d’être verrouillé par ad. Toutefois, si vous **réinitialisez le compteur de verrouillage de compte après** &gt; **ExtranetObservationWindow**, il est possible qu’un compte soit verrouillé par AD, mais en mode « différé ». L’obtention d’un compte verrouillé par Active Directory peut prendre un certain temps en fonction de votre configuration, étant donné que AD FS n’autorise qu’une seule tentative de mot de passe incorrect au cours de sa fenêtre d’observation jusqu’à ce que **badPwdCount** atteigne le **seuil de verrouillage de compte**.

Pour plus d’informations, consultez [configuration du verrouillage de compte](https://blogs.technet.microsoft.com/secguide/2014/08/13/configuring-account-lockout/). 

## <a name="known-issues"></a>Problèmes connus
Il existe un problème connu où le compte d’utilisateur Active Directory ne peut pas s’authentifier auprès de AD FS, car l’attribut **badPwdCount** n’est pas répliqué sur le contrôleur de domaine que ADFS interroge. Pour plus d’informations, consultez [2971171](https://support.microsoft.com/help/2971171/adfs-authentication-issue-for-active-directory-users-when-extranet-loc) . Vous pouvez trouver tous les AD FS QFE qui ont été publiés jusqu' [ici](../deployment/updates-for-active-directory-federation-services-ad-fs.md).

## <a name="key-points-to-remember"></a>Points clés à retenir
- La fonctionnalité de verrouillage extranet ne fonctionne que pour le **scénario extranet** dans lequel les demandes d’authentification transitent par le proxy d’application Web.
- La fonctionnalité de verrouillage extranet s’applique uniquement au **nom d’utilisateur & l’authentification par mot de passe**
- AD FS ne conserve aucune trace des **badPwdCount** ou des utilisateurs qui sont verrouillés de manière réversible. AD FS utilise Active Directory pour tous les suivis d’État
- AD FS effectue une recherche de l’attribut **badPwdCount** par le biais de l’appel LDAP pour l’utilisateur sur le contrôleur de domaine principal pour chaque tentative d’authentification.  
- AD FS antérieure à 2016 échouera s’il ne peut pas accéder au contrôleur de domaine principal. AD FS 2016 a introduit des améliorations qui permettront à AD FS de revenir à d’autres contrôleurs de domaine si le contrôleur de domaine principal n’est pas disponible. 
- AD FS autorisera les demandes d’authentification à partir de l’extranet si badPwdCount < ExtranetLockoutThreshold 
- Si **badPwdCount** >= **ExtranetLockoutThreshold** et **badPasswordTime** + **ExtranetObservationWindow** < heure actuelle, AD FS rejette les demandes d’authentification de l’extranet
- Pour éviter le verrouillage de compte malveillant, vous devez vous assurer que **ExtranetLockoutThreshold** < **seuil de verrouillage de compte** et **ExtranetObservationWindow** >  réinitialiser le compteur de**verrouillage de compte**


## <a name="additional-references"></a>Références supplémentaires  
- [Meilleures pratiques pour la sécurisation des Services ADFS](../../ad-fs/deployment/best-practices-securing-ad-fs.md)
- [Déléguer l’accès aux applets de commande Powershell AD FS aux utilisateurs non-administrateurs](delegate-ad-fs-pshell-access.md)
- [Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)

    
