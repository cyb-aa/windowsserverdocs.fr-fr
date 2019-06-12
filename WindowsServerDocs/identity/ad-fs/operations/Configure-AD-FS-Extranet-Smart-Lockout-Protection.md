---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurer la Protection par verrouillage Extranet AD FS
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7a4ad8c0199f0f62d7cd69a43897cb4608ddb365
ms.sourcegitcommit: ccc802338b163abdad2e53b55f39addcfea04603
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687372"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS Extranet Lockout et Extranet verrouillage intelligent

## <a name="overview"></a>Vue d'ensemble

Verrouillage intelligent extranet (ESL) protège vos utilisateurs à partir de la rencontre de verrouillage de compte extranet de toute activité malveillante.  

ESL permet à AD FS faire la distinction entre les tentatives de connexion à partir d’un emplacement connu pour un utilisateur et les tentatives de connexion à partir de ce qui peut représenter une personne malveillante. AD FS peut verrouiller des attaquants tout en laissant les utilisateurs valides à continuer à utiliser leurs comptes. Cela empêche et protège contre le déni de service et de certaines classes d’attaques de pulvérisation de mot de passe de l’utilisateur. ESL est disponible pour AD FS dans Windows Server 2016 et est intégré à AD FS dans Windows Server 2019.

ESL est uniquement disponible pour le nom d’utilisateur et les demandes d’authentification de mot de passe fournis via l’extranet avec le Proxy d’Application Web ou un 3e tiers proxy. N’importe quel proxy tiers 3e doit prendre en charge le protocole MS-ADFSPIP pour être utilisée à la place le Proxy d’Application Web, tels que [F5 BIG-IP Access Policy Manager](https://devcentral.f5.com/s/articles/ad-fs-proxy-replacement-on-f5-big-ip-30191). Consultez la documentation de proxy 3ème partie pour déterminer si le proxy prend en charge le protocole MS-ADFSPIP.   

## <a name="additional-features-in-ad-fs-2019"></a>Fonctionnalités supplémentaires dans AD FS 2019
Le verrouillage intelligent extranet dans AD FS 2019 ajoute les avantages suivants par rapport à AD FS 2016 :
- Définir des seuils de verrouillage indépendants pour les emplacements inconnus et familiers afin que les utilisateurs à un emplacement adéquat peuvent avoir plus de place pour l’erreur que les requêtes à partir d’emplacements suspects
- Activer le mode d’audit pour le verrouillage intelligent tout en continuant à appliquer le comportement de verrouillage de manière réversible précédent. Cela vous permet en savoir plus sur les emplacements connus utilisateur et toujours protégé par la fonctionnalité de verrouillage extranet est disponible à partir de 2012 R2 AD FS.  

## <a name="how-it-works"></a>Son fonctionnement
### <a name="configuration-information"></a>informations de configuration
Lorsque ESL est activée, une nouvelle table dans la base de données d’artefact, AdfsArtifactStore.AccountActivity, est créée et un nœud est sélectionné dans la batterie de serveurs AD FS en tant que le maître « Activité utilisateur ». Dans une configuration WID, ce nœud est toujours le nœud principal. Dans une configuration de SQL, un seul nœud est sélectionné pour être le maître de l’activité des utilisateurs.  

Pour afficher le nœud sélectionné en tant que le maître de l’activité des utilisateurs. Get-AdfsFarmInformation.FarmRoles

Tous les nœuds secondaires contacte le nœud principal sur chaque nouvelle connexion via le Port 80 pour en savoir plus la valeur la plus récente des décomptes de mots de passe erronés et nouvelles valeurs d’emplacement connu et mettre à jour de ce nœud après le traitement de la connexion.

![configuration](media/configure-ad-fs-extranet-smart-lockout-protection/esl1.png)

 Si le nœud secondaire ne peut pas contacter le maître, il écrira les événements d’erreur dans le journal d’administrateur AD FS. Authentifications continueront à traiter, mais AD FS écrit uniquement l’état de mise à jour localement. AD FS réessaiera de contact du maître toutes les 10 minutes et bascule vers le maître une fois que le maître est disponible.

### <a name="terminology"></a>Terminology
- **FamiliarLocation**: Lors d’une demande d’authentification, ESL vérifie que tous présentés les adresses IP. Ces adresses IP sera une combinaison de réseau IP, transféré d’IP et l’IP x-forwarded-for facultatif. Si la demande est acceptée, toutes les adresses IP sont ajoutées à la table d’activité de compte en tant que « adresses IP familier ». Si la requête possède toutes les adresses IP présente dans les « adresses IP familier », la demande sera traitée comme un emplacement « Connu ».
- **UnknownLocation**: Si une demande entrante a au moins une adresse IP non présente dans la liste « FamiliarLocation » existante, puis la demande sera traitée comme un emplacement « Inconnu ». Il s’agit pour gérer des scénarios de proxy telles que l’authentification héritée Exchange Online où les adresses Exchange Online gérer les demandes réussies et échouées.  
- **badPwdCount**: Une valeur représentant le nombre de fois où qu'un mot de passe incorrect a été soumise et l’authentification a échoué. Pour chaque utilisateur, distinct de compteurs sont conservés pour les emplacements connus et des emplacements inconnus.
- **UnknownLockout**: Valeur booléenne par utilisateur si l’utilisateur est verrouillé à partir de l’accès à partir d’emplacements inconnus. Cette valeur est calculée en fonction des badPwdCountUnfamiliar et des valeurs de ExtranetLockoutThreshold.
- **ExtranetLockoutThreshold**: Cette valeur détermine le nombre maximal de tentatives de mot de passe incorrect. Lorsque le seuil est atteint, ADFS rejette les demandes provenant de l’extranet jusqu'à ce que la fenêtre d’observation se soit écoulé.
- **ExtranetObservationWindow**: Cette valeur détermine la durée pendant laquelle les demandes de nom d’utilisateur et mot de passe à partir d’emplacements inconnus sont verrouillés. Lorsque la fenêtre est passée, ADFS commence à effectuer l’authentification du nom d’utilisateur et mot de passe à partir d’emplacements inconnus à nouveau.
- **ExtranetLockoutRequirePDC**: Lorsque l’option est activée, le verrouillage extranet nécessite un contrôleur de domaine principal (PDC). Lorsque désactivé, verrouillage extranet recourra à un autre contrôleur de domaine au cas où le contrôleur de domaine principal n’est pas disponible.  
- **ExtranetLockoutMode**: Contrôles de connecter uniquement le mode de verrouillage intelligent Extranet vs appliquées
    - **ADFSSmartLockoutLogOnly**: Extranet de verrouillage intelligent est activé, mais AD FS sera uniquement écrire admin et événements d’audit, mais sera pas rejeter les demandes d’authentification. Ce mode est conçu pour initialement être activés pour FamiliarLocation à remplir avant l’activation de « ADFSSmartLockoutEnforce ».
    - **ADFSSmartLockoutEnforce**: Prise en charge complète pour le blocage des demandes d’authentification inconnu lorsque les seuils sont atteints.

Adresses IPv4 et IPv6 sont prises en charge.

### <a name="anatomy-of-a-transaction"></a>Anatomie d’une transaction
- **Vérification de Pre-Auth**: Lors d’une demande d’authentification, ESL vérifie que tous présentés les adresses IP. Ces adresses IP sera une combinaison de réseau IP, transféré d’IP et l’IP x-forwarded-for facultatif. Dans les journaux d’audit, ces adresses IP sont répertoriés dans le <IpAddress> champ dans l’ordre x-ms-transférés-client-ip, x-forwarded-for, x-ms-proxy-client-ip.

  En fonction de ces adresses IP, ADFS détermine si la demande provient d’un emplacement inconnu ou familière et vérifie ensuite si le badPwdCount respectif est inférieure à la limite de seuil définie ou si la dernière **échec** tentative est s’est produite plus de la laps de temps de fenêtre d’observation. Si une des conditions suivantes est vraie, ADFS permet à cette transaction pour un traitement ultérieur et validation des informations d’identification. Si les deux conditions ont la valeur false, le compte est déjà dans un état verrouillé jusqu'à ce que la fenêtre d’observation transmet. Une fois que la fenêtre d’observation réussit, l’utilisateur est autorisé tentent de s’authentifier. Notez que dans 2019, ADFS vérifie par rapport à la limite de seuil approprié selon si l’adresse IP correspond à un emplacement connu ou non.
- **Connexion réussie**: Si le journal réussit, les adresses IP à partir de la requête sont ajoutés à la liste l’utilisateur emplacement connu IP.  
- **Échec de la connexion**: En cas de journal dans le badPwdCount est augmentée. L’utilisateur passera à un état de verrouillage si l’attaquant envoie plus mauvais mots de passe pour le système que ne le permet le seuil. (badPwdCount > ExtranetLockoutThreshold)  

![configuration](media/configure-ad-fs-extranet-smart-lockout-protection/esl2.png)

La valeur « UnknownLockout » sera égal à la valeur true lorsque le compte est verrouillé. Cela signifie que badPwdCount de l’utilisateur est sur dépasse le seuil par exemple, qu'une personne a tenté de mots de passe plus d’ont été autorisés par le système. Dans cet état, il existe 2 façons dont un utilisateur valid peut se connecter.
- L’utilisateur doit attendre le laps de temps ObservationWindow ou
- Pour pouvoir réinitialiser l’état de verrouillage, réinitialiser le badPwdCount remettre à zéro avec « Réinitialisation ADFSAccountLockout ».

Si aucun réinitialise se produire, le compte sera autorisé à une tentative de mot de passe unique par rapport à Active Directory pour chaque fenêtre d’observation. Le compte renvoie à l’état verrouillé après que la tentative et la fenêtre d’observation va redémarrer. La valeur badPwdCount uniquement réinitialise automatiquement une fois la connexion de mot de passe réussie.

### <a name="log-only-mode-versus-enforce-mode"></a>Mode journal seule ou en mode « Appliquer »
La table AccountActivity est remplie à la fois au cours de mode journal uniquement et en mode « Appliquer ». Si mode journal uniquement est ignoré et ESL est déplacé directement en mode de « Appliquer » sans le délai d’attente recommandé, les adresses IP familier des utilisateurs ne seront pas connu à ADFS. Dans ce cas, ESL serait se comportent comme 'ADBadPasswordCounter', potentiellement bloquantes le trafic utilisateur légitime, si le compte d’utilisateur est soumis à une attaque par force brute active. Si le mode journal seule est ignoré et l’utilisateur entre un verrouillé état avec « UnknownLockout » = TRUE et tente de se connecter avec un mot de passe à partir d’une adresse IP qui n’est pas dans la liste IP « connue », puis ils ne seront pas en mesure de se connecter. Mode journal uniquement est recommandé pour 3-7 jours éviter ce scénario. Si les comptes sont activement d’attaque, un minimum de 24 heures de mode journal uniquement est nécessaire pour éviter les verrouillages à des utilisateurs légitimes.  

## <a name="extranet-smart-lockout-configuration"></a>Configuration de l’extranet de verrouillage intelligent  

### <a name="prerequisites-for-ad-fs-2016"></a>Conditions préalables pour AD FS 2016

1. **Installer les mises à jour sur tous les nœuds dans la batterie de serveurs**

   Tout d’abord, vérifiez tous les serveurs Windows Server 2016 AD FS sont à jour depuis les mises à jour de juin 2018 Windows et que la batterie de serveurs AD FS 2016 est exécuté au niveau du comportement de batterie de serveurs 2016.
1. **Vérifier les autorisations**

   Verrouillage intelligent extranet nécessite que Windows Remote management soit activé sur chaque serveur AD FS.
3. **Mettre à jour les autorisations de base de données d’artefact**

   Verrouillage intelligent extranet nécessite le compte de service AD FS pour avoir des autorisations pour créer une nouvelle table dans la base de données AD FS artefact. Se connecter à n’importe quel serveur AD FS en tant qu’administrateur AD FS et cette autorisation en exécutant les commandes suivantes dans une fenêtre d’invite de commandes PowerShell :

   ``` powershell
   PS C:\>$cred = Get-Credential
   PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
   ```
   >[!NOTE]
   >L’espace réservé $cred est un compte disposant des autorisations d’administrateur AD FS. Cela doit fournir les autorisations d’écriture pour créer la table.

   Les commandes ci-dessus peuvent échouer en raison d’un manque d’autorisations suffisantes, car votre batterie AD FS à l’aide de SQL Server et les informations d’identification fournie ci-dessus ne dispose pas d’autorisation d’administrateur sur votre serveur SQL server. Dans ce cas, vous pouvez configurer les autorisations de base de données manuellement dans la base de données SQL Server en exécutant la commande suivante lorsque vous êtes connecté à la base de données AdfsArtifactStore.
    ```  
    # when prompted with “Are you sure you want to perform this action?”, enter Y.

    [CmdletBinding(SupportsShouldProcess=$true,ConfirmImpact = 'High')]
    Param()

    $fileLocation = "$env:windir\ADFS\Microsoft.IdentityServer.Servicehost.exe.config"

    if (-not [System.IO.File]::Exists($fileLocation))
    {
    write-error "Unable to open ADFS configuration file."
    return
    }

    $doc = new-object Xml
    $doc.Load($fileLocation)
    $connString = $doc.configuration.'microsoft.identityServer.service'.policystore.connectionString
    $connString = $connString -replace "Initial Catalog=AdfsConfigurationV[0-9]*", "Initial Catalog=AdfsArtifactStore"

    if ($PSCmdlet.ShouldProcess($connString, "Executing SQL command sp_addrolemember 'db_owner', 'db_genevaservice' "))
    {
    $cli = new-object System.Data.SqlClient.SqlConnection
    $cli.ConnectionString = $connString
    $cli.Open()

    try
    {     

    $cmd = new-object System.Data.SqlClient.SqlCommand
    $cmd.CommandText = "sp_addrolemember 'db_owner', 'db_genevaservice'"
    $cmd.Connection = $cli
    $rowsAffected = $cmd.ExecuteNonQuery()  
    if ( -1 -eq $rowsAffected )
    {
    write-host "Success"
    }
    }
    finally
    {
    $cli.CLose()
    }
    }
    ```

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Assurez-vous que AD FS sécurité l’enregistrement d’Audit est activée
Cette fonctionnalité utilise un Audit de sécurité journaux, par conséquent, l’audit doivent être activées dans AD FS, ainsi que la stratégie locale sur tous les serveurs AD FS.

### <a name="configuration-instructions"></a>Instructions de configuration
Extranet de verrouillage intelligent utilise la propriété ADFS **ExtranetLockoutEnabled**. Cette propriété a été précédemment utilisée pour le contrôle « Verrouillage Extranet de manière réversible » dans Server 2012 R2. Si le verrouillage Extranet réversible a été activé, pour afficher la configuration actuelle de la propriété, exécutez ` Get-AdfsProperties` .

### <a name="configuration-recommendations"></a>Recommandations de configuration
Lorsque vous configurez le verrouillage intelligent Extranet, suivez les meilleures pratiques pour la définition des seuils :  

`ExtranetObservationWindow (new-timespan -Minutes 30)`

`ExtranetLockoutThreshold: – 2x AD Threshold Value`

Valeur de l’annonce : 20, ExtranetLockoutThreshold : 10

Verrouillage de Active Directory fonctionne indépendamment de verrouillage intelligent de l’Extranet. Toutefois, si le verrouillage de Active Directory est activé, ExtranetLockoutThreshold dans AD FS < seuil de verrouillage de compte dans Active Directory

`ExtranetLockoutRequirePDC - $false`

Lorsque l’option est activée, le verrouillage extranet nécessite un contrôleur de domaine principal (PDC). Désactivée et configurée comme false, le verrouillage extranet recourra à un autre contrôleur de domaine au cas où le contrôleur de domaine principal n’est pas disponible.

Pour définir cette propriété à exécuter :

``` powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```
### <a name="enable-log-only-mode"></a>Activer le Mode journal uniquement

En mode uniquement de journal, AD FS remplit les informations utilisateurs emplacement connu et écrit les événements d’audit de sécurité, mais n’empêche pas toutes les demandes. Ce mode est utilisé pour valider que le verrouillage intelligent est en cours d’exécution et pour activer AD FS pour « découvrir » des emplacements connus pour les utilisateurs avant d’activer « mode d’application ». Que AD FS a connaissance, elle stocke l’activité de connexion par utilisateur (s’il en mode uniquement de journal ou en mode d’application).
Définir le comportement de verrouillage pour vous connecter uniquement en exécutant l’applet de commande.  

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly`

Mode uniquement de journal est destiné à être un état temporaire afin que le système peut apprendre le comportement de connexion avant la mise en œuvre de verrouillage avec le comportement de verrouillage intelligent de présentation. La durée recommandée pour le mode journal uniquement est de 3 à 7 jours. Si les comptes sont activement d’attaque, mode journal uniquement doit être exécuté pour un minimum de 24 heures.

Sur AD FS 2016, si le comportement de « Verrouillage Extranet réversible » 2012 R2 est activé avant l’activation du verrouillage intelligent Extranet, mode journal uniquement désactiver le comportement de « Verrouillage Extranet réversible ». Verrouillage intelligent de AD FS ne sera pas verrouiller les utilisateurs en mode journal uniquement. Toutefois, sur site AD peut verrouiller l’utilisateur basé sur la configuration d’AD. Veuillez consulter les stratégies de verrouillage AD pour en savoir comment local AD peut les utilisateurs de verrouillage.

Sur AD FS 2019, un avantage supplémentaire doit être en mesure d’activer le mode de journal uniquement pour le verrouillage intelligent tout en continuant à appliquer le comportement de verrouillage de manière réversible précédente à l’aide du Powershell ci-dessous.

`Set-AdfsProperties -ExtranetLockoutMode 3`

Pour le nouveau mode entrent en vigueur, redémarrez le service AD FS sur tous les nœuds dans la batterie de serveurs

`Restart-service adfssrv`

Une fois que le mode est configuré, vous pouvez activer l’utilisation du paramètre EnableExtranetLockout de verrouillage intelligent

`Set-AdfsProperties -EnableExtranetLockout $true`

### <a name="enable-enforce-mode"></a>Activer en Mode d’application

Une fois que vous êtes familiarisé avec la fenêtre de seuil et observation du verrouillage, ESL peut être déplacé vers « » en mode d’application à l’aide de l’applet de commande PSH suivante :

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce`

Pour le nouveau mode entrent en vigueur, redémarrez le service AD FS sur tous les nœuds dans la batterie de serveurs à l’aide de la commande suivante.

`Restart-service adfssrv`

## <a name="manage-user-account-activity"></a>Gérer l’activité de compte d’utilisateur
AD FS fournit trois applets de commande pour gérer les données d’activité de compte. Ces applets de commande se connectent automatiquement au nœud dans la batterie de serveurs qui détient le rôle de maître.
>[!NOTE]
>Just Enough Administration (JEA) peuvent être utilisées pour déléguer les applets de commande AD FS pour réinitialiser les verrouillages de compte. Par exemple, les membres du support technique peut être autorisations nécessaires pour utiliser les applets de commande ESL. Pour plus d’informations sur la délégation des autorisations requises pour utiliser ces applets de commande, consultez [déléguer AD FS un accès applet de commande Powershell pour les utilisateurs non-administrateurs](delegate-ad-fs-pshell-access.md)

Ce comportement peut être remplacé en transmettant le paramètre - Server.

- Get-ADFSAccountActivity -UserPrincipalName

  Lire l’activité de compte en cours pour un compte d’utilisateur. L’applet de commande se connecte automatiquement au contrôleur de batterie de serveurs en utilisant le point de terminaison REST d’activité de compte. Par conséquent, toutes les données soient cohérentes.

`Get-ADFSAccountActivity user@contoso.com`

  Propriétés :
    - BadPwdCountFamiliar: Incrémenté lorsque l’authentification a réussi à partir d’un emplacement connu.
    - BadPwdCountUnknown: Incrémenté lorsque l’authentification échoue à partir d’un emplacement inconnu
    - LastFailedAuthFamiliar: Si l’authentification a échoué à partir d’un emplacement connu, LastFailedAuthUnknown est définie au moment de l’authentification échoue
    - LastFailedAuthUnknown: Si l’authentification a échoué à partir d’un emplacement inconnu, LastFailedAuthUnknown est définie au moment de l’authentification échoue
    - FamiliarLockout : Valeur booléenne qui est « True » si le « BadPwdCountFamiliar » > ExtranetLockoutThreshold
    - UnknownLockout : Valeur booléenne qui est « True » si le « BadPwdCountUnknown » > ExtranetLockoutThreshold  
    - FamiliarIPs : un maximum de 20 adresses IP qui sont familiers pour l’utilisateur. Lorsque cette valeur est dépassée, l’adresse IP plus ancienne dans la liste est supprimé.
-    Set-ADFSAccountActivity

     Ajoute de nouveaux emplacements connus. La liste d’IP familier a un maximum de 20 entrées, si cette valeur est dépassée, l’adresse IP plus ancienne dans la liste sera supprimée.

`Set-ADFSAccountActivity user@contoso.com -AdditionalFamiliarIps “1.2.3.4”`

- Reset-ADFSAccountLockout

  Réinitialise les compteurs de l’emplacement inconnu (badPwdCountUnfamiliar) ou un compteur de verrouillage d’un compte d’utilisateur pour chaque emplacement connu (badPwdCountFamiliar). En réinitialisant un compteur, la valeur « FamiliarLockout » ou « UnfamiliarLockout » met à jour, car le réinitialiser le compteur sera inférieur au seuil.  

`Reset-ADFSAccountLockout user@contoso.com -Location Familiar`
`Reset-ADFSAccountLockout user@contoso.com -Location Unknown`

## <a name="event-logging--user-activity-information-for-ad-fs-extranet-lockout"></a>Journalisation des événements et des informations sur l’activité utilisateur pour le verrouillage Extranet AD FS

### <a name="connect-health"></a>Connect Health
La méthode recommandée pour surveiller l’activité de compte d’utilisateur est via Connect Health. Se connecter Health génère des rapports téléchargeables sur des adresses IP à risque et de tentatives de mot de passe incorrect. Chaque élément dans le rapport d’adresse IP risquée affiche des informations agrégées sur les activités ayant échoué AD FS sign-in qui dépassent le seuil défini. Notifications par courrier électronique peuvent être définies pour alerter les administrateurs dès que cela se produit avec les paramètres d’e-mail personnalisables. Pour plus d’informations et instructions d’installation, visitez le [documentation de Connect Health](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-health-adfs).

### <a name="ad-fs-extranet-smart-lockout-events"></a>Événements AD FS de verrouillage intelligent Extranet.
Pour les événements de verrouillage intelligent Extranet à écrire, ESL doit être activé en mode « journal uniquement » ou « appliquer » et l’audit de sécurité AD FS est activé.
AD FS écrira les événements de verrouillage extranet dans le journal d’audit de sécurité :
- Quand un utilisateur est verrouillé hors (atteint le seuil de verrouillage des tentatives de connexion ayant échoué)
- Quand AD FS reçoit une tentative de connexion pour un utilisateur qui est déjà dans un état de verrouillage

En mode journal uniquement, vous pouvez vérifier le journal d’audit de sécurité pour les événements de verrouillage. Pour tous les événements trouvés, vous pouvez vérifier l’état de l’utilisateur à l’aide de l’applet de commande Get-ADFSAccountActivity pour déterminer si le verrouillage s’est produite à partir des adresses IP familiers ou inconnus et pour vérifier la liste d’adresses d’IP familiers pour cet utilisateur.


|ID d’événement|Description|
|-----|-----|
|1203|Cet événement est écrit pour chaque tentative de mot de passe incorrect. Dès que le badPwdCount atteint la valeur spécifiée dans ExtranetLockoutThreshold, le compte sera être verrouillé sur AD FS pour la durée spécifiée dans ExtranetObservationWindow.</br>ID d’activité : %1</br>XML : %2|
|1201|Cet événement est écrit à chaque fois qu’un utilisateur est verrouillé. </br>ID d’activité : %1</br>XML : %2|
|557 (ADFS 2019)| Une erreur s’est produite en essayant de communiquer avec le service de rest magasin compte sur le nœud %1. S’il s’agit d’une batterie de serveurs WID le nœud principal peut être hors connexion. S’il s’agit d’une batterie de serveurs SQL, AD FS sélectionne automatiquement un nouveau nœud pour héberger le rôle de maître de magasin utilisateur.|
|562 (ADFS 2019)|Une erreur s’est produite lorsque communcating avec le compte de stocker le point de terminaison sur le serveur %1.</br>Message d’exception : %2|
|563 (ADFS 2019)|Une erreur s’est produite lors du calcul d’état de verrouillage extranet. La valeur de %1 en raison d’une définition de l’authentification est autorisée pour cet utilisateur et d’émission de jeton se poursuit. S’il s’agit d’une batterie de serveurs WID le nœud principal peut être hors connexion. S’il s’agit d’une batterie de serveurs SQL, AD FS sélectionne automatiquement un nouveau nœud pour héberger le rôle de maître de magasin utilisateur.</br>Nom de serveur de magasin de compte : %2</br>Id d’utilisateur : %3</br>Message d’exception : %4|
|512|Le compte de l’utilisateur est verrouillé. Une tentative de connexion est autorisée en raison de la configuration du système.</br>ID d’activité : %1 </br>Utilisateur : %2 </br>Client IP : %3 </br>Nombre de mot de passe incorrects : %4  </br>Dernière tentative de mot de passe incorrect : %5|
|515|Le compte d’utilisateur suivant était dans un état verrouillé et le mot de passe correct a été simplement fourni. Ce compte peut être compromis.</br>Données supplémentaires </br>ID d’activité : %1 </br>Utilisateur : %2 </br>Client IP : %3 |
|516|Le compte d’utilisateur suivant a été verrouillé en raison du trop grand nombre de tentatives de mot de passe incorrect.</br>ID d’activité : %1  </br>Utilisateur : %2  </br>Client IP : %3  </br>Nombre de mot de passe incorrects : %4  </br>Dernière tentative de mot de passe incorrect : %5|

## <a name="esl-frequently-asked-questions"></a>Questions fréquentes sur ESL

**Une batterie de serveurs AD FS à l’aide de verrouillage intelligent Extranet dans appliquera le mode jamais voir verrouillages de l’utilisateur malveillant ?** 

R : Si le verrouillage intelligent ADFS est défini sur « mode d’application », vous ne verrez jamais verrouillé par force brute ou par déni de service à l’aide de compte de l’utilisateur légitime. La seule façon de qu'un verrouillage de compte malveillant peut empêcher un utilisateur de connexion est si l’acteur a le mot de passe utilisateur ou qu’il peut envoyer des demandes à partir d’une adresse IP (familière) correcte connue pour cet utilisateur. 

**Que se passe-t-il ESL est activé et que le mauvais acteur possède un mot de passe utilisateur ?** 

R : Le scénario d’attaque par force brute vise typique à deviner un mot de passe et être connecté avec succès.  Si un utilisateur est piratage ou si un mot de passe est deviné ensuite la fonctionnalité ESL ne bloque pas l’accès dans la mesure où la connexion répond aux critères de « réussite » de mot de passe correct plus de la nouvelle adresse IP. L’adresse IP de mauvais acteurs apparaissent alors comme un « connu ». L’atténuation meilleures dans ce scénario consiste à effacer l’activité de l’utilisateur dans AD FS et d’exiger l’authentification multifacteur pour les utilisateurs. Nous vous recommandons vivement de l’installation de Protection de mot de passe AAD qui garantit que les mots de passe à deviner n’obtiennent pas dans le système.

**Si mon utilisateur a jamais été connecté avec succès à partir d’une adresse IP et puis tentatives avec mot de passe incorrect plusieurs fois seront ils pourront se connecter une fois qu’ils enfin taper son mot de passe correctement ?** 

R : Si un utilisateur envoie plusieurs mots de passe incorrects (par exemple, légitimement mal tapé) sur la tentative suivante obtient le mot de passe corrects, puis l’utilisateur réussira immédiatement pour vous connecter.  Vous désactivez le nombre de mots de passe erronés et ajouter cette adresse IP à la liste FamiliarIPs.  Toutefois, si elles dépassent le seuil d’échecs de connexion à partir de l’emplacement inconnu, celui-ci doit entrer dans un état de verrouillage et ils nécessitent attendre au-delà de la fenêtre d’observation et connectez-vous avec un mot de passe ou nécessitent l’intervention de l’administrateur pour réinitialiser leur compte.  

**ESL fonctionne-t-il sur intranet trop ?**   
R : Si les clients se connectent directement aux serveurs ADFS et non par le biais de serveurs Proxy d’Application Web puis le comportement ESL n’applique pas. 

**Je vois les adresses IP Microsoft dans le champ d’adresse IP du Client. ESL bloc EXO traitée attaques en force brute ?**  

R : ESL fonctionnera bien pour empêcher Exchange Online ou autres scénarios d’attaque par force brute authentification hérités. Une authentification hérités a un « ID d’activité » de 00000000-0000-0000-0000-000000000000. Dans ces attaques, l’acteur tire parti de l’authentification de base Exchange Online (également appelé authentification hérités) afin que l’adresse IP du client s’affiche en tant que Microsoft. Les serveurs Exchange en ligne dans le proxy cloud la vérification de l’authentification au nom du client Outlook. Dans ces scénarios, l’adresse IP de l’émetteur malveillant sera dans le x-ms-transférés--adresse ip du client et le serveur Microsoft Exchange Online, qu'adresse IP sera dans la valeur de x-ms-client-ip.
Verrouillage intelligent extranet vérifie transféré les adresses IP, le x-transférés-client-IP et la valeur de x-ms-client-ip de réseau, des adresses IP. Si la demande est acceptée, toutes les adresses IP sont ajoutées à la liste familier. Si une demande arrive et un fournisseur d’identité présentée ne sont pas dans la liste familier puis la demande est marquée comme non connue. L’utilisateur familière seront puissent se connecter avec succès pendant que les demandes à partir des emplacements non connus sont bloquées.  


## <a name="additional-references"></a>Références supplémentaires  
[Meilleures pratiques pour la sécurisation d’Active Directory Federation Services](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
