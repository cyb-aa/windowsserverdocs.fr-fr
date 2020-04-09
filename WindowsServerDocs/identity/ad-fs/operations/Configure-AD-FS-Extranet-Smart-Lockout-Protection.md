---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurer AD FS protection par verrouillage extranet
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 28f7fc4a8c7129d9f88cc030b1b150db44321bf9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859942"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS le verrouillage extranet et le verrouillage intelligent extranet

## <a name="overview"></a>Overview

Le verrouillage intelligent extranet (ESL) protège vos utilisateurs contre le verrouillage de compte extranet contre les activités malveillantes.  

ESL permet à AD FS de faire la différence entre les tentatives de connexion à partir d’un emplacement familier pour un utilisateur et les tentatives de connexion de ce qui peut être une personne malveillante. AD FS pouvez bloquer les attaquants tout en permettant aux utilisateurs valides de continuer à utiliser leurs comptes. Cela empêche et protège contre les attaques par déni de service et certaines classes d’attaques par pulvérisation de mot de passe sur l’utilisateur. ESL est disponible pour AD FS dans Windows Server 2016 et est intégré à AD FS dans Windows Server 2019.

ESL est uniquement disponible pour les demandes d’authentification de nom d’utilisateur et de mot de passe qui passent par l’extranet avec le proxy d’application Web ou un proxy tiers. N’importe quel proxy tiers doit prendre en charge le protocole MS-ADFSPIP à utiliser à la place du proxy d’application Web, par exemple [F5 BIG-IP Access Policy Manager](https://devcentral.f5.com/s/articles/ad-fs-proxy-replacement-on-f5-big-ip-30191). Consultez la documentation du proxy tiers pour déterminer si le proxy prend en charge le protocole MS-ADFSPIP.   

## <a name="additional-features-in-ad-fs-2019"></a>Fonctionnalités supplémentaires de AD FS 2019
Le verrouillage intelligent extranet dans AD FS 2019 ajoute les avantages suivants par rapport à AD FS 2016 :
- Définissez des seuils de verrouillage indépendants pour les emplacements familiers et inconnus afin que les utilisateurs disposant de bons emplacements connus puissent avoir plus de place pour les erreurs que les demandes provenant d’emplacements suspects.
- Activez le mode audit pour le verrouillage intelligent tout en continuant à appliquer le comportement de verrouillage logiciel précédent. Cela vous permet d’en savoir plus sur les emplacements familiers de l’utilisateur et de rester protégé par la fonctionnalité de verrouillage extranet disponible à partir de AD FS 2012 R2.  

## <a name="how-it-works"></a>Fonctionnement
### <a name="configuration-information"></a>Informations de configuration
Lorsque ESL est activé, une nouvelle table dans la base de données d’artefacts, AdfsArtifactStore. AccountActivity, est créée et un nœud est sélectionné dans la batterie de AD FS en tant que maître d’activité de l’utilisateur. Dans une configuration WID, ce nœud est toujours le nœud principal. Dans une configuration SQL, un nœud est sélectionné comme maître d’activité utilisateur.  

Pour afficher le nœud sélectionné en tant que maître d’activité utilisateur. AdfsFarmInformation. FarmRoles

Tous les nœuds secondaires contactent le nœud principal sur chaque nouvelle connexion via le port 80 pour connaître la valeur la plus récente du nombre de mots de passe incorrects et de nouvelles valeurs de localisation familières, et mettre à jour ce nœud après le traitement de la connexion.

![configuration](media/configure-ad-fs-extranet-smart-lockout-protection/esl1.png)

 Si le nœud secondaire ne parvient pas à contacter le maître, il écrit des événements d’erreur dans le journal d’administration AD FS. Les authentifications continuent d’être traitées, mais AD FS n’écrira que l’État mis à jour localement. AD FS tentera de contacter le maître toutes les 10 minutes et repassera au maître une fois que le serveur maître sera disponible.

### <a name="terminology"></a>Terminology
- **FamiliarLocation**: lors d’une demande d’authentification, ESL vérifie toutes les adresses IP présentées. Ces adresses IP sont une combinaison de l’adresse IP du réseau, de l’adresse IP transférée et de l’adresse IP (x-forwardd) facultative. Si la demande aboutit, toutes les adresses IP sont ajoutées à la table d’activité du compte comme « adresses IP familières ». Si toutes les adresses IP de la demande sont présentes dans les « adresses IP familières », la demande est traitée comme un emplacement « familier ».
- **UnknownLocation**: si une demande qui arrive dans a au moins une adresse IP qui n’est pas présente dans la liste « FamiliarLocation » existante, la demande sera traitée comme un emplacement « inconnu ». Cela permet de gérer les scénarios de proxy, tels que l’authentification héritée Exchange Online, où les adresses Exchange Online gèrent les demandes ayant réussi et échoué.  
- **badPwdCount**: valeur représentant le nombre de fois où un mot de passe incorrect a été envoyé et l’authentification a échoué. Pour chaque utilisateur, des compteurs distincts sont conservés pour les emplacements connus et les emplacements inconnus.
- **UnknownLockout**: valeur booléenne par utilisateur si l’utilisateur ne peut pas accéder à depuis des emplacements inconnus. Cette valeur est calculée en fonction des valeurs badPwdCountUnfamiliar et ExtranetLockoutThreshold.
- **ExtranetLockoutThreshold**: cette valeur détermine le nombre maximal de tentatives de mot de passe incorrectes. Lorsque le seuil est atteint, ADFS rejette les demandes provenant de l’extranet jusqu’à ce que la fenêtre d’observation soit passée.
- **ExtranetObservationWindow**: cette valeur détermine la durée pendant laquelle les demandes de nom d’utilisateur et de mot de passe provenant d’emplacements inconnus sont verrouillées. Lorsque la fenêtre est passée, ADFS commence à effectuer une nouvelle authentification de nom d’utilisateur et mot de passe à partir d’emplacements inconnus.
- **ExtranetLockoutRequirePDC**: lorsque cette option est activée, le verrouillage extranet requiert un contrôleur de domaine principal (PDC). Lorsqu’il est désactivé, le verrouillage extranet fait un secours vers un autre contrôleur de domaine si le contrôleur de domaine principal n’est pas disponible.  
- **ExtranetLockoutMode**: contrôle la journalisation uniquement en mode mis en œuvre du verrouillage intelligent extranet
    - **ADFSSmartLockoutLogOnly**: le verrouillage intelligent extranet est activé, mais AD FS n’écrira que les événements d’administrateur et d’audit, mais ne rejette pas les demandes d’authentification. Ce mode est destiné à être activé pour que FamiliarLocation soit rempli avant que « ADFSSmartLockoutEnforce » ne soit activé.
    - **ADFSSmartLockoutEnforce**: prise en charge complète du blocage des demandes d’authentification non familières lorsque les seuils sont atteints.

Les adresses IPv4 et IPv6 sont prises en charge.

### <a name="anatomy-of-a-transaction"></a>Anatomie d’une transaction
- **Vérification de pré-authentification**: lors d’une demande d’authentification, ESL vérifie toutes les adresses IP présentées. Ces adresses IP sont une combinaison de l’adresse IP du réseau, de l’adresse IP transférée et de l’adresse IP (x-forwardd) facultative. Dans les journaux d’audit, ces adresses IP sont répertoriées dans le champ <IpAddress> dans l’ordre x-ms-forwarded-client-IP, x-forwarded-for, x-ms-proxy-client-IP.

  Sur la base de ces adresses IP, ADFS détermine si la requête provient d’un emplacement familier ou inconnu, puis vérifie si le badPwdCount respectif est inférieur à la limite définie pour le seuil ou si la dernière tentative qui **a échoué** s’est produite plus longtemps que le délai de la fenêtre d’observation. Si l’une de ces conditions est vraie, ADFS autorise cette transaction à être traitée ultérieurement et la validation des informations d’identification. Si les deux conditions ont la valeur false, le compte est déjà verrouillé jusqu’à ce que la fenêtre d’observation passe. Une fois que la fenêtre d’observation a réussi, l’utilisateur est autorisé à tenter de s’authentifier. Notez que, dans 2019, ADFS effectue une vérification par rapport à la limite de seuil appropriée en fonction de si l’adresse IP correspond à un emplacement connu.
- **Connexion réussie**: si la connexion réussit, les adresses IP de la demande sont ajoutées à la liste d’adresses IP de localisation familière de l’utilisateur.  
- **Échec**de la connexion : si la connexion échoue, le badPwdCount est augmenté. L’utilisateur passera à l’état de verrouillage si l’attaquant envoie plus de mots de passe incorrects au système que le seuil autorisé par le seuil. (badPwdCount > ExtranetLockoutThreshold)  

![configuration](media/configure-ad-fs-extranet-smart-lockout-protection/esl2.png)

La valeur « UnknownLockout » est égale à true lorsque le compte est verrouillé. Cela signifie que le badPwdCount de l’utilisateur est supérieur au seuil, c’est-à-dire qu’un utilisateur a tenté plus de mots de passe que ce qui était autorisé par le système. Dans cet État, il existe deux façons pour un utilisateur valide de se connecter.
- L’utilisateur doit attendre que la durée de ObservationWindow soit écoulée ou
- Pour réinitialiser l’état de verrouillage, réinitialisez le badPwdCount à zéro avec « Reset-ADFSAccountLockout ».

Si aucune réinitialisation n’est effectuée, le compte sera autorisé à effectuer une tentative de mot de passe unique par rapport à Active Directory pour chaque fenêtre d’observation. Le compte reviendra à l’état verrouillé après cette tentative et la fenêtre d’observation redémarrera. La valeur badPwdCount est automatiquement réinitialisée après une connexion de mot de passe réussie.

### <a name="log-only-mode-versus-enforce-mode"></a>Mode de journalisation uniquement et mode « appliquer »
La table AccountActivity est remplie à la fois en mode « journal uniquement » et en mode « appliquer ». Si le mode « journal uniquement » est contourné et que ESL est déplacé directement en mode « d’application » sans la période d’attente recommandée, les adresses IP familières des utilisateurs ne sont pas connues d’ADFS. Dans ce cas, ESL se comporte comme « ADBadPasswordCounter », bloquant potentiellement le trafic utilisateur légitime si le compte d’utilisateur est soumis à une attaque par force brute active. Si le mode « journal uniquement » est ignoré et que l’utilisateur entre un état verrouillé avec « UnknownLockout » = TRUE et tente de se connecter avec un mot de passe correct à partir d’une adresse IP qui ne figure pas dans la liste des adresses IP « familières », il ne pourra pas se connecter. Le mode de journalisation uniquement est recommandé pour 3-7 jours pour éviter ce scénario. Si les comptes sont activement attaqués, un minimum de 24 heures de mode « journalisation uniquement » est nécessaire pour empêcher les verrouillages des utilisateurs légitimes.  

## <a name="extranet-smart-lockout-configuration"></a>Configuration du verrouillage intelligent extranet  

### <a name="prerequisites-for-ad-fs-2016"></a>Conditions préalables pour AD FS 2016

1. **Installer les mises à jour sur tous les nœuds de la batterie de serveurs**

   Tout d’abord, assurez-vous que tous les serveurs Windows Server 2016 AD FS sont à jour depuis les mises à jour Windows du 2018 juin et que la batterie AD FS 2016 s’exécute au niveau du comportement de la batterie de serveurs 2016.
1. **Vérifier les autorisations**

   Le verrouillage intelligent extranet requiert l’activation de la gestion à distance de Windows sur chaque serveur AD FS.
3. **Mettre à jour les autorisations de base de données d’artefact**

   Le verrouillage intelligent extranet nécessite que le compte de service AD FS dispose des autorisations nécessaires pour créer une table dans la base de données d’artefacts AD FS. Connectez-vous à un serveur AD FS en tant qu’administrateur de AD FS, puis accordez cette autorisation en exécutant les commandes suivantes dans une fenêtre d’invite de commandes PowerShell :

   ``` powershell
   PS C:\>$cred = Get-Credential
   PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
   ```
   >[!NOTE]
   >L’espace réservé $cred est un compte disposant d’autorisations d’administrateur AD FS. Cela doit fournir les autorisations en écriture pour créer la table.

   Les commandes ci-dessus peuvent échouer en raison d’un manque d’autorisations suffisantes, car votre batterie de AD FS utilise SQL Server et les informations d’identification fournies ci-dessus ne disposent pas de l’autorisation d’administrateur sur votre serveur SQL Server. Dans ce cas, vous pouvez configurer manuellement les autorisations de base de données dans SQL Server base de données en exécutant la commande suivante lorsque vous êtes connecté à la base de données AdfsArtifactStore.
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

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Vérifier AD FS la journalisation d’audit de sécurité est activée
Cette fonctionnalité utilise les journaux d’audit de sécurité, de sorte que l’audit doit être activé dans AD FS, ainsi que la stratégie locale sur tous les serveurs AD FS.

### <a name="configuration-instructions"></a>Instructions de configuration
Le verrouillage intelligent extranet utilise la propriété ADFS **ExtranetLockoutEnabled**. Cette propriété a été utilisée précédemment pour contrôler le « verrouillage logiciel extranet » dans le serveur 2012 R2. Si le verrouillage logiciel extranet a été activé, pour afficher la configuration actuelle de la propriété, exécutez ` Get-AdfsProperties`.

### <a name="configuration-recommendations"></a>Recommandations de configuration
Lors de la configuration du verrouillage intelligent extranet, suivez les meilleures pratiques pour définir les seuils :  

`ExtranetObservationWindow (new-timespan -Minutes 30)`

`ExtranetLockoutThreshold: – 2x AD Threshold Value`

Valeur Active Directory : 20, ExtranetLockoutThreshold : 10

Active Directory le verrouillage fonctionne indépendamment du verrouillage intelligent extranet. Toutefois, si Active Directory verrouillage est activé, ExtranetLockoutThreshold dans AD FS seuil de verrouillage de compte < dans Active Directory

`ExtranetLockoutRequirePDC - $false`

Lorsqu’il est activé, le verrouillage extranet requiert un contrôleur de domaine principal (PDC). En cas de désactivation et de configuration en tant que faux, le verrouillage Extranet permet de revenir à un autre contrôleur de domaine si le contrôleur de domaine principal n’est pas disponible.

Pour définir cette propriété, exécutez :

``` powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```
### <a name="enable-log-only-mode"></a>Activer le mode de journalisation uniquement

En mode journal uniquement, AD FS remplit les informations d’emplacement familières des utilisateurs et écrit les événements d’audit de sécurité, mais ne bloque pas les requêtes. Ce mode est utilisé pour vérifier que le verrouillage intelligent est en cours d’exécution et pour permettre à AD FS de « découvrir » des emplacements familiers pour les utilisateurs avant d’activer le mode « appliquer ». Comme AD FS apprend, il stocke l’activité de connexion par utilisateur (que ce soit en mode journal uniquement ou en mode d’application).
Définissez le comportement de verrouillage sur log uniquement en exécutant l’applet de commande suivante.  

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly`

Le mode journal uniquement est un état temporaire afin que le système puisse apprendre le comportement de connexion avant d’introduire l’application du verrouillage avec le comportement de verrouillage intelligent. La durée recommandée pour le mode de journalisation uniquement est de 3-7 jours. Si les comptes sont activement attaqués, le mode de journalisation uniquement doit être exécuté pendant un minimum de 24 heures.

Sur AD FS 2016, si le comportement de 2012 R2 « le verrouillage à chaud » est activé avant l’activation du verrouillage intelligent extranet, le mode de journalisation uniquement désactive le comportement de « verrouillage à chaud » de l’extranet. AD FS le verrouillage intelligent ne verrouille pas les utilisateurs en mode de journalisation uniquement. Toutefois, les services AD locaux peuvent verrouiller l’utilisateur en fonction de la configuration AD. Consultez les stratégies de verrouillage AD pour savoir comment local AD peut verrouiller les utilisateurs.

Sur AD FS 2019, un avantage supplémentaire est de pouvoir activer le mode de verrouillage du journal uniquement pour le verrouillage intelligent tout en continuant à appliquer le comportement de verrouillage logiciel précédent à l’aide de PowerShell ci-dessous.

`Set-AdfsProperties -ExtranetLockoutMode 3`

Pour que le nouveau mode prenne effet, redémarrez le service AD FS sur tous les nœuds de la batterie de serveurs.

`Restart-service adfssrv`

Une fois le mode configuré, vous pouvez activer le verrouillage intelligent à l’aide du paramètre EnableExtranetLockout

`Set-AdfsProperties -EnableExtranetLockout $true`

### <a name="enable-enforce-mode"></a>Activer le mode d’application

Une fois que vous êtes familiarisé avec le seuil de verrouillage et la fenêtre d’observation, ESL peut être déplacé en mode « appliquer » à l’aide de l’applet de commande PSH suivante :

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce`

Pour que le nouveau mode prenne effet, redémarrez le service AD FS sur tous les nœuds de la batterie à l’aide de la commande suivante.

`Restart-service adfssrv`

## <a name="manage-user-account-activity"></a>Gérer l’activité du compte d’utilisateur
AD FS fournit trois applets de commande pour gérer les données d’activité de compte. Ces applets de commande se connectent automatiquement au nœud de la batterie de serveurs qui détient le rôle de maître.
>[!NOTE]
>L’administration juste-à-temps (JEA) peut être utilisée pour déléguer AD FS applets pour réinitialiser les verrouillages de compte. Par exemple, le personnel du support technique peut être délégué des autorisations pour utiliser ESL applets. Pour plus d’informations sur la délégation d’autorisations pour l’utilisation de ces applets de commande, consultez [déléguer AD FS accès à l’applet de commande PowerShell aux utilisateurs non administrateurs](delegate-ad-fs-pshell-access.md)

Ce comportement peut être substitué en passant le paramètre-Server.

- ADFSAccountActivity-UserPrincipalName

  Lire l’activité actuelle du compte pour un compte d’utilisateur. L’applet de commande se connecte toujours automatiquement au maître de la batterie de serveurs à l’aide du point de terminaison REST de l’activité de compte. Par conséquent, toutes les données doivent toujours être cohérentes.

`Get-ADFSAccountActivity user@contoso.com`

  Propriétés :
    - BadPwdCountFamiliar : incrémenté lorsqu’une authentification réussit à partir d’un emplacement connu.
    - BadPwdCountUnknown : incrémenté lorsqu’une authentification échoue à partir d’un emplacement inconnu
    - LastFailedAuthFamiliar : si l’authentification a échoué à partir d’un emplacement connu, LastFailedAuthUnknown est défini sur l’heure de l’échec de l’authentification
    - LastFailedAuthUnknown : si l’authentification a échoué à partir d’un emplacement inconnu, LastFailedAuthUnknown est défini sur l’heure de l’échec de l’authentification
    - FamiliarLockout : valeur booléenne qui est « true » si « BadPwdCountFamiliar » > ExtranetLockoutThreshold
    - UnknownLockout : valeur booléenne qui est « true » si « BadPwdCountUnknown » > ExtranetLockoutThreshold  
    - FamiliarIPs : 20 adresses IP au maximum qui sont familières à l’utilisateur. Lorsque cette opération est dépassée, l’adresse IP la plus ancienne de la liste est supprimée.
-    Set-ADFSAccountActivity

     Ajoute de nouveaux emplacements familiers. La liste d’adresses IP familières a un maximum de 20 entrées, si cette valeur est dépassée, l’adresse IP la plus ancienne de la liste est supprimée.

`Set-ADFSAccountActivity user@contoso.com -AdditionalFamiliarIps “1.2.3.4”`

- Reset-ADFSAccountLockout

  Réinitialise le compteur de verrouillage pour un compte d’utilisateur pour chaque emplacement connu (badPwdCountFamiliar) ou des compteurs d’emplacement inconnus (badPwdCountUnfamiliar). En réinitialisant un compteur, la valeur « FamiliarLockout » ou « UnfamiliarLockout » est mise à jour, car le compteur de réinitialisation est inférieur au seuil.  

`Reset-ADFSAccountLockout user@contoso.com -Location Familiar`
`Reset-ADFSAccountLockout user@contoso.com -Location Unknown`

## <a name="event-logging--user-activity-information-for-ad-fs-extranet-lockout"></a>Journalisation des événements & des informations sur l’activité des utilisateurs pour AD FS le verrouillage extranet

### <a name="connect-health"></a>Connecter l’intégrité
La méthode recommandée pour surveiller l’activité d’un compte d’utilisateur consiste à connecter l’intégrité. Connect Health génère des rapports téléchargeables sur les adresses IP risquées et les tentatives de mot de passe erronées. Chaque élément du rapport d’adresse IP risquée affiche des informations agrégées sur les activités de connexion AD FS ayant échoué qui dépassent le seuil désigné. Les notifications par courrier électronique peuvent être définies pour alerter les administrateurs dès que cela se produit avec les paramètres de messagerie personnalisables. Pour obtenir des informations supplémentaires et des instructions d’installation, consultez la [documentation Connect Health](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs).

### <a name="ad-fs-extranet-smart-lockout-events"></a>AD FS les événements de verrouillage intelligent extranet.
Pour que les événements de verrouillage intelligent extranet soient écrits, ESL doit être activé en mode « journal uniquement » ou « appliquer » et l’audit de sécurité ADFS est activé.
AD FS écrira des événements de verrouillage extranet dans le journal d’audit de sécurité :
- Lorsqu’un utilisateur est verrouillé (atteint le seuil de verrouillage pour les tentatives de connexion ayant échoué)
- Lorsque AD FS reçoit une tentative de connexion pour un utilisateur qui est déjà à l’état de verrouillage

En mode journal uniquement, vous pouvez consulter le journal d’audit de sécurité pour rechercher les événements de verrouillage. Pour tout événement trouvé, vous pouvez vérifier l’état utilisateur à l’aide de l’applet de commande ADFSAccountActivity pour déterminer si le verrouillage s’est produit à partir d’adresses IP familières ou inconnues, et pour vérifier la liste des adresses IP familières pour cet utilisateur.


|ID d’événement|Description|
|-----|-----|
|1203|Cet événement est écrit pour chaque tentative de mot de passe incorrecte. Dès que le badPwdCount atteint la valeur spécifiée dans ExtranetLockoutThreshold, le compte est verrouillé sur ADFS pour la durée spécifiée dans ExtranetObservationWindow.</br>ID de l’activité : %1</br>XML : %2|
|1201|Cet événement est écrit chaque fois qu’un utilisateur est verrouillé. </br>ID de l’activité : %1</br>XML : %2|
|557 (ADFS 2019)| Une erreur s’est produite lors de la tentative de communication avec le service REST du magasin de comptes sur le nœud %1. S’il s’agit d’une batterie de serveurs WID, le nœud principal est peut-être hors connexion. S’il s’agit d’une batterie de serveurs SQL, ADFS sélectionne automatiquement un nouveau nœud pour héberger le rôle de maître du magasin d’utilisateurs.|
|562 (ADFS 2019)|Une erreur s’est produite lors de la communcating du point de terminaison du magasin de comptes sur le serveur %1.</br>Message d’exception : %2|
|563 (ADFS 2019)|Une erreur s’est produite lors du calcul de l’état du verrouillage extranet. En raison de la valeur du paramètre %1, l’authentification est autorisée pour cet utilisateur et l’émission du jeton se poursuit. S’il s’agit d’une batterie de serveurs WID, le nœud principal est peut-être hors connexion. S’il s’agit d’une batterie de serveurs SQL, ADFS sélectionne automatiquement un nouveau nœud pour héberger le rôle de maître du magasin d’utilisateurs.</br>Nom du serveur de magasin de comptes : %2</br>ID d’utilisateur : %3</br>Message d’exception : %4|
|512|Le compte de l’utilisateur suivant est verrouillé. Une tentative de connexion est autorisée en raison de la configuration du système.</br>ID de l’activité : %1 </br>Utilisateur : %2 </br>Adresse IP du client : %3 </br>Nombre de mots de passe incorrects : %4  </br>Dernière tentative de mot de passe incorrect : %5|
|515|Le compte d’utilisateur suivant était dans un état verrouillé et le mot de passe approprié vient d’être fourni. Ce compte risque d’être compromis.</br>Données supplémentaires </br>ID de l’activité : %1 </br>Utilisateur : %2 </br>Adresse IP du client : %3 |
|516|Le compte d’utilisateur suivant a été verrouillé en raison d’un trop grand nombre de tentatives de mot de passe incorrectes.</br>ID de l’activité : %1  </br>Utilisateur : %2  </br>Adresse IP du client : %3  </br>Nombre de mots de passe incorrects : %4  </br>Dernière tentative de mot de passe incorrect : %5|

## <a name="esl-frequently-asked-questions"></a>Forum aux questions sur ESL

**Une batterie de serveurs ADFS utilisant le verrouillage intelligent extranet en mode d’application sera-t-elle toujours visible par les verrouillages des utilisateurs malveillants ?** 

R : si le verrouillage intelligent ADFS est défini sur le mode « appliquer », vous ne verrez jamais le compte de l’utilisateur légitime verrouillé par force brute ou par déni de service. La seule façon de verrouiller un compte malveillant peut empêcher la connexion d’un utilisateur est si le mot de passe de l’utilisateur est incorrect ou s’il peut envoyer des requêtes à partir d’une adresse IP connue (familière) connue pour cet utilisateur. 

**Que se passe-t-il si ESL est activé et que le mauvais acteur a un mot de passe utilisateur ?** 

R : l’objectif type du scénario d’attaque par force brute est de deviner un mot de passe et de se connecter correctement.  Si un utilisateur est victime d’un hameçonnage ou si un mot de passe est deviné, la fonctionnalité ESL ne bloquera pas l’accès, car la connexion répondra aux critères « réussis » du mot de passe correct et à la nouvelle adresse IP. L’adresse IP des acteurs incorrects apparaît alors comme « familière ». La meilleure atténuation dans ce scénario consiste à effacer l’activité de l’utilisateur dans ADFS et à exiger une authentification multifacteur pour les utilisateurs. Nous vous recommandons vivement d’installer la protection par mot de passe AAD qui garantit que les mots de passe devinés ne sont pas dans le système.

**Si mon utilisateur ne s’est jamais connecté avec succès à partir d’une adresse IP et tente alors un mot de passe incorrect plusieurs fois, il pourra se connecter une fois qu’il aura finalement tapé son mot de passe correctement.** 

R : si un utilisateur envoie plusieurs mots de passe incorrects (par exemple, un typage légitime) et que, lors de la tentative suivante, le mot de passe est correct, l’utilisateur parvient immédiatement à se connecter.  Le nombre de mots de passe incorrects est alors effacé et l’adresse IP est ajoutée à la liste FamiliarIPs.  Toutefois, s’ils se trouvent au-dessus du seuil des échecs de connexion à partir de l’emplacement inconnu, ils entrent en état de verrouillage et doivent patienter au-delà de la fenêtre d’observation et se connectent avec un mot de passe valide ou nécessitent une intervention de l’administrateur pour réinitialiser leur compte.  
 
**ESL fonctionne-t-il également sur intranet ?**

R : si les clients se connectent directement aux serveurs ADFS et non via les serveurs proxy d’application Web, le comportement ESL ne s’applique pas.  

**Je vois des adresses IP Microsoft dans le champ adresse IP du client. ESL bloque-t-elle les attaques en force brute proxy EXO ?**  

R : ESL fonctionne bien pour empêcher Exchange Online ou d’autres scénarios d’attaque par force brute d’authentification. Une authentification héritée a un « ID d’activité » de 00000000-0000-0000-0000-000000000000. Dans ces attaques, l’acteur incorrect tire parti de l’authentification de base Exchange Online (également appelée authentification héritée) afin que l’adresse IP du client apparaisse en tant que Microsoft One. Les serveurs Exchange Online du Cloud proxy vérifient l’authentification pour le compte du client Outlook. Dans ces scénarios, l’adresse IP de l’expéditeur malveillant sera dans le x-ms-forwarded-client-IP et l’adresse IP du serveur Microsoft Exchange Online sera dans la valeur x-ms-client-IP.
Le verrouillage intelligent extranet vérifie les adresses IP du réseau, les adresses IP transférées, la valeur x-forwarded-client-IP et la valeur x-ms-client-IP. Si la demande aboutit, toutes les adresses IP sont ajoutées à la liste familière. Si une demande arrive et que l’une des adresses IP présentées ne figure pas dans la liste familière, la demande est marquée comme peu familière. L’utilisateur familier sera en mesure de se connecter avec succès alors que les demandes provenant des emplacements inconnus seront bloquées.  

\* * Q : puis-je estimer la taille du ADFSArtifactStore avant d’activer ESL ?

R : avec ESL activé, AD FS effectue le suivi de l’activité du compte et des emplacements connus pour les utilisateurs dans la base de données ADFSArtifactStore. Cette base de données est mise à l’échelle en fonction du nombre d’utilisateurs et d’emplacements connus suivis. Quand vous envisagez d’activer ESL, vous pouvez partir du principe que la taille de la base de données ADFSArtifactStore augmentera de 1 Go maximum tous les 100 000 utilisateurs. Si la batterie de AD FS utilise la base de données interne Windows (WID), l’emplacement par défaut des fichiers de base de données est C:\Windows\WID\Data\. Pour empêcher le remplissage de ce lecteur, assurez-vous de disposer d’au moins 5 Go de stockage gratuit avant d’activer ESL. En plus du stockage sur disque, planifiez l’augmentation de la mémoire totale de processus après l’activation d’ESL d’une valeur pouvant aller jusqu’à 1 Go de RAM supplémentaire pour les populations de 500 000 utilisateurs ou moins.


## <a name="additional-references"></a>Références supplémentaires  
[Meilleures pratiques pour la sécurisation des Services ADFS](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
