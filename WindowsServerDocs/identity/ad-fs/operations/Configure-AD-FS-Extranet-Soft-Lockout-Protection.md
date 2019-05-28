---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurer la Protection par verrouillage Extranet AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/01/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6612c05e664b50c5a50b10b712b91715cc85d230
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189879"
---
# <a name="configure-ad-fs-extranet-lockout-protection"></a>Configurer la Protection par verrouillage Extranet AD FS

Dans AD FS sur Windows Server 2012 R2, nous avons introduit une fonctionnalité de sécurité appelée verrouillage Extranet.  Avec cette fonctionnalité, AD FS est « arrêter » authentifier le compte d’utilisateur « malveillants » à partir d’à l’extérieur pour une période donnée.  Cela empêche vos comptes d’utilisateur de leur compte est verrouillé dans Active Directory.  Outre la protection de vos utilisateurs à partir d’un verrouillage de compte AD, le verrouillage extranet AD FS protège également contre le mot de passe par force brute deviner les attaques

> [!NOTE]
> Cette fonctionnalité fonctionne uniquement pour le **scénario extranet** l’authentification à la demande sont fournis par le biais du Proxy d’Application Web et s’applique uniquement aux **nom d’utilisateur et mot de passe de l’authentification**.

## <a name="advantages-of-extranet-lockout"></a>Avantages de verrouillages Extranet
Verrouillage extranet offre les avantages clés suivants :
- Il protège vos comptes d’utilisateur à partir de **attaques en force brute** où un pirate tente de deviner le mot de passe d’un utilisateur en envoyant des demandes d’authentification en continu. Dans ce cas, AD FS sera verrouillé le compte d’utilisateur malveillant pour l’accès extranet
- Il protège vos comptes d’utilisateur à partir de **verrouillage de compte malveillant** où un attaquant souhaite verrouiller un compte d’utilisateur en envoyant des demandes d’authentification par mot de passe incorrect. Dans ce cas, bien que le compte d’utilisateur sera verrouillé par AD FS pour l’accès extranet, le compte d’utilisateur dans Active Directory n’est pas verrouillé et l’utilisateur peut accéder toujours des ressources d’entreprise au sein de l’organisation. Il s’agit une **réversible verrouillage**.

## <a name="how-it-works"></a>Fonctionnement
Il existe 3 paramètres dans les services AD FS que vous devez configurer pour activer cette fonctionnalité : 
- **EnableExtranetLockout &lt;booléenne&gt;**  définir cette valeur booléenne True si vous souhaitez activer le verrouillage Extranet.
- **ExtranetLockoutThreshold &lt;entier&gt;**  définit le nombre maximal de tentatives de mot de passe incorrect. Une fois que le seuil est atteint, AD FS sera immédiatement rejette les demandes provenant d’extranet sans essayer de contacter le contrôleur de domaine pour l’authentification, aucune question si le mot de passe est bon ou mauvais, jusqu'à ce que la fenêtre de l’extranet d’observation est passée. Cela signifie que la valeur de **badPwdCount** attribut d’un compte AD n’augmente pas alors que le compte est soft-verrouillé.
- **ExtranetObservationWindow &lt;TimeSpan&gt;**  ce paramètre détermine la durée pendant laquelle l’utilisateur compte sera soft-verrouillé. AD FS commence à effectuer à nouveau l’authentification nom d’utilisateur et mot de passe quand la fenêtre est passée. AD FS utilise le badPasswordTime attribut AD en tant que la référence pour déterminer si la fenêtre d’observation extranet a réussi ou non. La fenêtre a passé si actuel temps > badPasswordTime + ExtranetObservationWindow. 

> [!NOTE]
> Fonctions de verrouillage extranet AD FS indépendamment des stratégies de verrouillage AD. Toutefois, nous recommandons vivement que vous avez défini le **ExtranetLockoutThreshold** valeur de paramètre à une valeur qui est inférieure au seuil de verrouillage de compte AD. Ne parvient pas à le faire entraînerait l’impossibilité de se protéger des comptes à partir de leur compte est verrouillé dans Active Directory Federation Services. 

Voici un exemple d’activation de fonctionnalité de verrouillage Extranet avec 15 nombre maximal de tentatives de mot de passe incorrect et la durée de verrouillage de soft 30 minutes :

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30)
```

Ces paramètres s’appliquent à tous les domaines capable d’authentifier le service AD FS. La façon dont il fonctionne est que lorsque AD FS reçoit une demande d’authentification, il sera le contrôleur de domaine principal (PDC) d’accès via un appel LDAP et effectuer une recherche pour le **badPwdCount** attribut pour l’utilisateur sur le contrôleur de domaine principal. Si AD FS recherche la valeur de **badPwdCount** > = ExtranetLockoutThreshold configuration et l’heure définie dans la fenêtre d’Observation Extranet n’a pas été encore, ADFS rejette la demande immédiatement, ce qui signifie que quel que soit le s’il le utilisateur entre un mot de passe bon ou mauvais d’extranet, l’ouverture de session échouera parce que AD FS n’envoie pas les informations d’identification AD. AD FS ne conserve pas de n’importe quel état aux **badPwdCount** ou des comptes d’utilisateur verrouillés. AD FS utilise AD pour tous les États de suivi. 

> [!warning]
> Lorsque le verrouillage AD FS Extranet sur Server 2012 R2 est activé toutes les demandes d’authentification via le proxy d’application Web sont validés par AD FS sur le contrôleur de domaine principal. Lorsque le contrôleur de domaine principal est indisponible, les utilisateurs ne pourront pas s’authentifier à partir de l’extranet.

Server 2016 propose un paramètre supplémentaire qui permet à AD FS à revenir à un autre contrôleur de domaine lorsque le contrôleur de domaine principal n’est pas disponible :

- **ExtranetLockoutRequirePDC &lt;booléenne&gt;**  : quand activé : verrouillage extranet nécessite un contrôleur de domaine principal (PDC). Lorsque désactivé : verrouillage extranet recourra à un autre contrôleur de domaine au cas où le contrôleur de domaine principal n’est pas disponible.

Vous pouvez utiliser la commande Windows PowerShell suivante pour configurer le verrouillage extranet AD FS sur Server 2016 :

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```

## <a name="working-with-the-active-directory-lockout-policy"></a>Utilisation de la stratégie de verrouillage d’Active Directory
La fonctionnalité de verrouillage Extranet dans AD FS fonctionne indépendamment de la stratégie de verrouillage AD. Toutefois, vous n’avez pas besoin de vérifier les paramètres pour le verrouillage Extranet est correctement configuré afin qu’il puisse fournir son objectif de sécurité avec la stratégie de verrouillage AD.
Jetons un œil à la stratégie de verrouillage AD premier. Il existe trois paramètres concernant la stratégie de verrouillage dans Active Directory :
- **Seuil de verrouillage de compte**: ce paramètre est similaire au paramètre ExtranetLockoutThreshold dans AD FS. Il détermine le nombre de tentatives de connexion qui entraînera un compte d’utilisateur peuvent être verrouillés. Afin de protéger vos comptes d’utilisateur à partir d’une attaque de verrouillage de compte malveillant, vous souhaitez définir la valeur de ExtranetLockoutThreshold dans AD FS &lt; la valeur de seuil de verrouillage de compte dans Active Directory
- **Durée de verrouillage de compte**: ce paramètre détermine la durée pendant laquelle un utilisateur compte est verrouillé. Ce paramètre n’a aucune importance beaucoup dans cette conversation comme verrouillage Extranet doit toujours se produire avant le verrouillage AD se produise si configuré correctement
- **Réinitialiser le compteur de verrouillage du compte après**: ce paramètre détermine combien de temps doit s’écouler entre le dernier échec de connexion de l’utilisateur avant **badPwdCount** est réinitialisé à 0. Dans l’ordre pour la fonctionnalité de verrouillage Extranet AD FS pour fonctionner avec stratégie de verrouillage AD, vous souhaitez s’assurer que la valeur de ExtranetObservationWindow dans AD FS &gt; réinitialiser compte compteur de verrouillage après celle figurant dans AD. Les exemples ci-dessous explique pourquoi.  

Nous allons examiner deux exemples et voir comment **badPwdCount** change au fil du temps en fonction de différents paramètres et états. Supposons que dans les deux exemples **seuil de verrouillage de compte** = 4 et **ExtranetLockoutThreshold** = 2. Le **rouge** flèche représente la tentative de mot de passe incorrect, le **vert** flèche représente une tentative de mot de passe. Dans l’exemple #1, **ExtranetObservationWindow** &gt; **réinitialiser compte compteur de verrouillage après**. Dans l’exemple #2, **ExtranetObservationWindow** &lt; **réinitialiser compte compteur de verrouillage après**. 

### <a name="example-1"></a>Exemple 1
![Exemple 1](media/Configure-AD-FS-Extranet-Lockout-Protection/one.png)

### <a name="example-2"></a>Exemple 2
![Exemple 1](media/Configure-AD-FS-Extranet-Lockout-Protection/two.png)

Comme vous pouvez le voir à partir de la méthode ci-dessus, il existe deux conditions **badPwdCount** sera remis à 0. Un est lorsqu’il existe une ouverture de session réussie. L’autre est lorsqu’il est temps de se réinitialiser ce compteur tel que défini dans **réinitialiser compte compteur de verrouillage après** paramètre. Lorsque **réinitialiser compte compteur de verrouillage après** &lt; **ExtranetObservationWindow**, un compte ne dispose pas de risque d’être verrouillé par AD. Toutefois, si **réinitialiser compte compteur de verrouillage après** &gt; **ExtranetObservationWindow**, il est possible qu’un compte peut être verrouillé par AD, mais dans un « manière différée ». Il peut prendre un certain temps pour obtenir un compte verrouillé par AD selon votre configuration que AD FS permet uniquement une tentative de mot de passe incorrect lors de sa fenêtre d’observation jusqu'à **badPwdCount** atteint **seuil de verrouillage de compte** .

Pour plus d’informations, consultez [configuration de verrouillage de compte](https://blogs.technet.microsoft.com/secguide/2014/08/13/configuring-account-lockout/). 

## <a name="known-issues"></a>Problèmes connus
Il existe un problème connu où le compte d’utilisateur AD ne peut pas authentifier avec AD FS, car le **badPwdCount** attribut n’est pas répliqué vers le contrôleur de domaine ADFS effectue une requête. Consultez [2971171](https://support.microsoft.com/help/2971171/adfs-authentication-issue-for-active-directory-users-when-extranet-loc) pour plus d’informations. Vous trouverez tous les correctifs FS AD qui ont été publiées jusqu'à présent [ici](../deployment/updates-for-active-directory-federation-services-ad-fs.md).

## <a name="key-points-to-remember"></a>Points importants à retenir
- La fonctionnalité de verrouillage Extranet fonctionne uniquement pour le **scénario extranet** où les demandes d’authentification sont fournis par le biais du Proxy d’Application Web
- La fonctionnalité de verrouillage Extranet s’applique uniquement aux **nom d’utilisateur et mot de passe de l’authentification**
- AD FS ne conserve pas une quelconque piste de **badPwdCount** ou les utilisateurs qui sont soft-verrouillé. AD FS utilise AD pour tous les États de suivi
- AD FS effectue une recherche pour le **badPwdCount** attribut via un appel LDAP pour l’utilisateur sur le contrôleur de domaine principal pour chaque tentative d’authentification  
- AD FS antérieure à 2016 échouera s’il ne peut pas accéder à la conférence PDC. AD FS 2016 introduit des améliorations qui permettront à AD FS à revenir vers d’autres contrôleurs de domaine en cas de contrôleur de domaine principal n’est pas disponible. 
- AD FS autorise les demandes d’authentification à partir d’extranet si badPwdCount < ExtranetLockoutThreshold 
- Si **badPwdCount** >= **ExtranetLockoutThreshold** AND **badPasswordTime** + **ExtranetObservationWindow** < Heure actuelle, AD FS rejette les demandes d’authentification extranet
- Pour éviter le verrouillage de compte malveillant, vous devez vous assurer **ExtranetLockoutThreshold** < **seuil de verrouillage de compte** AND **ExtranetObservationWindow**  >  **Réinitialiser le compteur de verrouillage de compte**


## <a name="additional-references"></a>Références supplémentaires  
- [Meilleures pratiques pour la sécurisation d’Active Directory Federation Services](../../ad-fs/deployment/best-practices-securing-ad-fs.md)
- [Déléguer l’accès aux applets de commande Powershell AD FS aux utilisateurs non-administrateurs](delegate-ad-fs-pshell-access.md)
- [Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)

    
