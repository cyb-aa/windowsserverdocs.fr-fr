---
title: Gestion du service Guardian hôte
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: eecb002e-6ae5-4075-9a83-2bbcee2a891c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 41912c90beacbb0c0c285ea4da8305c1afdf2a51
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322601"
---
# <a name="managing-the-host-guardian-service"></a>Gestion du service Guardian hôte

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Le service Guardian hôte (SGH) est la pièce maîtresse de la solution d’infrastructure protégée.
Il est chargé de s’assurer que les hôtes Hyper-V de l’infrastructure sont connus de l’hébergeur ou de l’entreprise et de l’exécution de logiciels approuvés et de la gestion des clés utilisées pour démarrer les machines virtuelles protégées.
Lorsqu’un locataire décide de vous faire confiance pour héberger ses machines virtuelles protégées, il fait confiance à la configuration et à la gestion du service Guardian hôte.
Par conséquent, il est très important de suivre les meilleures pratiques lors de la gestion du service Guardian hôte pour garantir la sécurité, la disponibilité et la fiabilité de votre infrastructure protégée.
Les instructions des sections suivantes traitent des problèmes opérationnels les plus courants auxquels font face les administrateurs de SGH.

## <a name="limiting-admin-access-to-hgs"></a>Limitation de l’accès administrateur à SGH
En raison de la sécurité liée à la sécurité SGH, il est important de s’assurer que ses administrateurs sont des membres hautement approuvés de votre organisation et, idéalement, séparés des administrateurs de vos ressources d’infrastructure.
En outre, il est recommandé de gérer uniquement SGH à partir de stations de travail sécurisées à l’aide de protocoles de communication sécurisés, tels que WinRM sur HTTPs.

### <a name="separation-of-duties"></a>Séparation des tâches
Quand vous configurez SGH, vous avez la possibilité de créer une forêt de Active Directory isolée uniquement pour le groupe de domaine spécifique ou de joindre le SGH à un domaine approuvé existant.
Cette décision, ainsi que les rôles que vous attribuez aux administrateurs dans votre organisation, déterminent la limite d’approbation pour SGH.
Quiconque a accès à SGH, que ce soit directement en tant qu’administrateur ou indirectement en tant qu’administrateur d’un autre nom (par exemple, Active Directory) qui peut influencer le service SGH, contrôle l’infrastructure protégée.
Les administrateurs SGH choisissent les hôtes Hyper-V autorisés à exécuter des machines virtuelles protégées et à gérer les certificats nécessaires au démarrage des machines virtuelles protégées.
Un pirate ou un administrateur malveillant qui a accès à SGH peut utiliser cette puissance pour autoriser les hôtes compromis à exécuter des machines virtuelles protégées, à lancer une attaque par déni de service en supprimant le matériel de clé, et bien plus encore.

Pour éviter ce risque, il est *fortement* recommandé de limiter le chevauchement entre les administrateurs de votre service SGH (y compris le domaine auquel le service SGH est joint) et les environnements Hyper-V.
En veillant à ce qu’aucun administrateur n’ait accès aux deux systèmes, un attaquant devrait compromettre 2 comptes différents de 2 personnes pour terminer sa mission afin de modifier les stratégies SGH.
Cela signifie également que les administrateurs du domaine et de l’entreprise pour les deux environnements Active Directory ne doivent pas être la même personne, et que SGH n’utilise pas la même forêt Active Directory que vos hôtes Hyper-V.
Toute personne pouvant s’octroyer l’accès à d’autres ressources présente un risque pour la sécurité.

### <a name="using-just-enough-administration"></a>Utilisation de l’administration juste assez
SGH est fourni avec [juste assez](https://aka.ms/JEAdocs) de rôles d’administration (JEA) intégrés pour vous aider à le gérer plus en toute sécurité.
JEA vous permet de déléguer des tâches d’administration à des utilisateurs non administrateurs, ce qui signifie que les personnes qui gèrent les stratégies SGH n’ont pas besoin d’être des administrateurs de l’ensemble de l’ordinateur ou du domaine.
JEA fonctionne en limitant les commandes qu’un utilisateur peut exécuter dans une session PowerShell et en utilisant un compte local temporaire en arrière-plan (unique pour chaque session utilisateur) pour exécuter les commandes qui nécessitent normalement une élévation.

SGH est fourni avec 2 rôles JEA préconfigurés :
- **Administrateurs SGH** qui permettent aux utilisateurs de gérer toutes les stratégies SGH, y compris l’autorisation de nouveaux hôtes pour exécuter des machines virtuelles protégées.
- **Réviseurs SGH** qui autorisent uniquement les utilisateurs à effectuer des audits sur les stratégies existantes. Ils ne peuvent pas apporter de modifications à la configuration SGH.

Pour utiliser JEA, vous devez d’abord créer un utilisateur standard et en faire un membre du groupe administrateurs SGH ou réviseurs SGH.
Si vous avez utilisé `Install-HgsServer` pour configurer une nouvelle forêt pour SGH, ces groupes sont nommés «*ServiceName*Administrators » et «*ServiceName*Reviewers », respectivement, où *ServiceName* est le nom réseau du cluster SGH.
Si vous avez joint SGH à un domaine existant, vous devez faire référence aux noms de groupes que vous avez spécifiés dans `Initialize-HgsServer`.

**Créer des utilisateurs standard pour les rôles d’administrateur et de réviseur SGH**

```powershell
$hgsServiceName = (Get-ClusterResource HgsClusterResource | Get-ClusterParameter DnsName).Value
$adminGroup = $hgsServiceName + "Administrators"
$reviewerGroup = $hgsServiceName + "Reviewers"

New-ADUser -Name 'hgsadmin01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Admin Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $adminGroup -Members 'hgsadmin01'

New-ADUser -Name 'hgsreviewer01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Reviewer Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $reviewerGroup -Members 'hgsreviewer01'
```

**Auditer les stratégies avec le rôle réviseur**

Sur un ordinateur distant disposant d’une connectivité réseau à SGH, exécutez les commandes suivantes dans PowerShell pour entrer la session JEA avec les informations d’identification du réviseur.
Il est important de noter que dans la mesure où le compte du réviseur n’est qu’un utilisateur standard, il ne peut pas être utilisé pour la communication à distance Windows PowerShell normale, Bureau à distance l’accès à SGH, etc.

```powershell
Enter-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsreviewer01' -ConfigurationName 'microsoft.windows.hgs'
```

Vous pouvez ensuite vérifier les commandes qui sont autorisées dans la session avec `Get-Command` et exécuter toutes les commandes autorisées pour auditer la configuration.
Dans l’exemple ci-dessous, nous vérifions les stratégies qui sont activées sur SGH.

```powershell
Get-Command

Get-HgsAttestationPolicy
```

Tapez la commande `Exit-PSSession` ou son alias, `exit`lorsque vous avez fini d’utiliser la session JEA. 

**Ajouter une nouvelle stratégie à SGH à l’aide du rôle administrateur**

Pour modifier réellement une stratégie, vous devez vous connecter au point de terminaison JEA avec une identité qui appartient au groupe « hgsAdministrators ».
Dans l’exemple ci-dessous, nous montrons comment vous pouvez copier une nouvelle stratégie d’intégrité du code dans SGH et l’inscrire à l’aide de JEA.
La syntaxe peut être différente de celle que vous utilisez.
Cela permet de prendre en compte certaines des restrictions dans JEA comme n’ayant pas accès au système de fichiers complet.

```powershell
$cipolicy = Get-Item "C:\temp\cipolicy.p7b"
$session = New-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsadmin01' -ConfigurationName 'microsoft.windows.hgs'
Copy-Item -Path $cipolicy -Destination 'User:' -ToSession $session

# Now that the file is copied, we enter the interactive session to register it with HGS
Enter-PSSession -Session $session
Add-HgsAttestationCiPolicy -Name 'New CI Policy via JEA' -Path 'User:\cipolicy.p7b'

# Confirm it was added successfully
Get-HgsAttestationPolicy -PolicyType CiPolicy

# Finally, remove the PSSession since it is no longer needed
Exit-PSSession
Remove-PSSession -Session $session
```

## <a name="monitoring-hgs"></a>Surveillance du SGH
### <a name="event-sources-and-forwarding"></a>Sources d’événements et transfert
Les événements de SGH s’affichent dans le journal des événements Windows, sous 2 sources :
- **HostGuardianService-attestation**
- **HostGuardianService-keyprotection**

Vous pouvez afficher ces événements en ouvrant observateur d’événements et en accédant à Microsoft-Windows-HostGuardianService-attestation et à Microsoft-Windows-HostGuardianService-keyprotection.

Dans un environnement de grande taille, il est souvent préférable de transférer les événements vers un collecteur d’événements Windows central pour faciliter la analyse des événements.
Pour plus d’informations, consultez la [documentation sur le transfert d’événements Windows](https://msdn.microsoft.com/library/windows/desktop/bb427443.aspx).

### <a name="using-system-center-operations-manager"></a>Utilisation de System Center Operations Manager
Vous pouvez également utiliser System Center 2016-Operations Manager pour surveiller les hôtes SGH et les hôtes service Guardian.
Le pack d’administration de la structure protégée dispose d’analyses d’événements pour rechercher les erreurs de configuration courantes pouvant entraîner un temps d’arrêt du centre de donnes, y compris les hôtes ne passant pas l’attestation et les serveurs SGH signalant des erreurs.

Pour commencer, [Installez et configurez SCOM 2016](https://technet.microsoft.com/system-center-docs/om/welcome-to-operations-manager) et [Téléchargez le pack d’administration de la structure protégée](https://www.microsoft.com/download/details.aspx?id=52764).
Le Guide du pack d’administration inclus explique comment configurer le pack d’administration et comprendre l’étendue de ses analyses.

## <a name="backing-up-and-restoring-hgs"></a>Sauvegarde et restauration de SGH
### <a name="disaster-recovery-planning"></a>Planification de la récupération d’urgence
Lorsque vous élaborez vos plans de récupération d’urgence, il est important de prendre en compte les exigences uniques du service Guardian hôte dans votre infrastructure protégée.
Si vous perdez une partie ou la totalité de vos nœuds SGH, vous pouvez faire face à des problèmes de disponibilité immédiats qui empêcheront les utilisateurs de démarrer leurs machines virtuelles protégées.
Dans un scénario où vous perdez l’ensemble de votre cluster SGH, vous devrez effectuer des sauvegardes complètes de la configuration SGH pour restaurer votre cluster SGH et reprendre les opérations normales.
Cette section décrit les étapes nécessaires à la préparation d’un tel scénario.

Tout d’abord, il est important de comprendre ce qui est important pour la sauvegarde de SGH.
SGH conserve plusieurs informations qui l’aident à déterminer les hôtes autorisés à exécuter des machines virtuelles dotées d’une protection maximale.
Cela comprend les éléments suivants :
1. Active Directory identificateurs de sécurité pour les groupes contenant des hôtes approuvés (lors de l’utilisation d’Active Directory attestation);
2. Identificateurs TPM uniques pour chaque hôte de votre environnement ;
3. Stratégies TPM pour chaque configuration unique de l’hôte ; les
4. Stratégies d’intégrité du code qui déterminent quels logiciels sont autorisés à s’exécuter sur vos ordinateurs hôtes.

Ces artefacts d’attestation requièrent une coordination avec les administrateurs de votre infrastructure d’hébergement pour obtenir, ce qui rend potentiellement difficile la récupération de ces informations après un sinistre.

En outre, SGH requiert l’accès à au moins 2 certificats utilisés pour chiffrer et signer les informations requises pour démarrer une machine virtuelle protégée (le protecteur de clé).
Ces certificats sont bien connus (utilisés par les propriétaires des machines virtuelles protégées pour autoriser votre infrastructure à exécuter leurs machines virtuelles) et doivent être restaurés après un sinistre pour une expérience de récupération transparente.
Si vous ne restaurez pas SGH avec les mêmes certificats après un incident, chaque machine virtuelle doit être mise à jour pour autoriser vos nouvelles clés à déchiffrer leurs informations.
Pour des raisons de sécurité, seul le propriétaire de la machine virtuelle peut mettre à jour la configuration de la machine virtuelle pour autoriser ces nouvelles clés, ce qui signifie que l’échec de la restauration de vos clés après un incident se traduira par le propriétaire de la machine virtuelle à effectuer des actions pour réexécuter leurs machines virtuelles.

#### <a name="preparing-for-the-worst"></a>Préparation au pire
Pour préparer une perte complète de SGH, vous devez effectuer deux étapes :
1. Sauvegarder les stratégies d’attestation SGH
2. Sauvegarder les clés SGH

Des conseils sur la façon d’effectuer ces deux étapes sont fournis dans la section [sauvegarde de SGH](#backing-up-hgs) .

Il est également recommandé, mais pas obligatoire, que vous sauvegardiez la liste des utilisateurs autorisés à gérer SGH dans son domaine Active Directory ou Active Directory lui-même.

Les sauvegardes doivent être effectuées régulièrement pour s’assurer que les informations sont à jour et stockées de manière sécurisée afin d’éviter toute falsification ou vol.

Il n’est **pas recommandé** de sauvegarder ou de restaurer l’intégralité d’une image système d’un nœud SGH.
En cas de perte de l’ensemble de votre cluster, la meilleure pratique consiste à configurer un tout nouveau nœud SGH et à restaurer uniquement l’État SGH, et non le système d’exploitation serveur entier.

#### <a name="recovering-from-the-loss-of-one-node"></a>Récupération après la perte d’un nœud
Si vous perdez un ou plusieurs nœuds (mais pas tous les nœuds) de votre cluster SGH, vous pouvez simplement [Ajouter des nœuds à votre cluster](guarded-fabric-configure-additional-hgs-nodes.md) en suivant les instructions du Guide de déploiement.
Les stratégies d’attestation sont automatiquement synchronisées, comme les certificats fournis à SGH en tant que fichiers PFX avec les mots de passe associés.
Pour les certificats ajoutés à SGH à l’aide d’une empreinte numérique (des certificats non exportables et matériels, généralement), vous devez vous assurer que chaque nouveau nœud a accès à la clé privée de chaque certificat.

#### <a name="recovering-from-the-loss-of-the-entire-cluster"></a>Récupération après la perte de l’ensemble du cluster
Si l’ensemble de votre cluster SGH tombe en panne et que vous ne parvenez pas à le remettre en ligne, vous devrez restaurer SGH à partir d’une sauvegarde.
La restauration de SGH à partir d’une sauvegarde implique la configuration préalable d’un nouveau cluster SGH conformément aux [instructions du Guide de déploiement](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).
Il est fortement recommandé, mais pas obligatoire, d’utiliser le même nom de cluster lors de la configuration de l’environnement SGH de récupération pour faciliter la résolution de noms à partir d’hôtes.
L’utilisation du même nom évite de devoir reconfigurer les ordinateurs hôtes avec les nouvelles URL d’attestation et de protection de clé.
Si vous restaurez des objets dans le domaine de la Active Directory de stockage SGH, il est recommandé de supprimer les objets représentant le cluster SGH, les ordinateurs, le compte de service et les groupes JEA avant d’initialiser le serveur SGH.

Une fois que vous avez configuré votre premier nœud SGH (par exemple, s’il a été installé et initialisé), vous devez suivre les procédures de la rubrique [restauration de SGH à partir d’une sauvegarde](#restoring-hgs-from-a-backup) pour restaurer les stratégies d’attestation et les moitiés publiques des certificats de protection de clé.
Vous devez restaurer manuellement les clés privées de vos certificats en fonction de l’aide de votre fournisseur de certificats (par exemple, importer le certificat dans Windows ou configurer l’accès aux certificats sauvegardés par HSM).
Une fois le premier nœud configuré, vous pouvez continuer à [installer des nœuds supplémentaires sur le cluster](guarded-fabric-configure-additional-hgs-nodes.md) jusqu’à ce que vous ayez atteint la capacité et la résilience souhaitées.

### <a name="backing-up-hgs"></a>Sauvegarde du SGH
L’administrateur SGH doit être responsable de la sauvegarde de SGH sur une base régulière.
Une sauvegarde complète contient des éléments de clé sensibles qui doivent être sécurisés de manière appropriée.
Si une entité non approuvée peut accéder à ces clés, elle peut utiliser ce matériel pour configurer un environnement SGH malveillant afin de compromettre les machines virtuelles protégées.

**Sauvegarde des stratégies d’attestation** Pour sauvegarder les stratégies d’attestation SGH, exécutez la commande suivante sur n’importe quel nœud de serveur SGH actif.
Vous serez invité à fournir un mot de passe.
Ce mot de passe est utilisé pour chiffrer les certificats ajoutés à SGH à l’aide d’un fichier PFX (au lieu d’une empreinte numérique de certificat).

```powershell
Export-HgsServerState -Path C:\temp\HGSBackup.xml
```

> [!NOTE]
> Si vous utilisez une attestation approuvée par l’administrateur, vous devez sauvegarder séparément l’appartenance aux groupes de sécurité utilisés par SGH pour autoriser les hôtes service Guardian.
> SGH sauvegarde uniquement le SID des groupes de sécurité, et non l’appartenance au sein de ceux-ci.
> Si ces groupes sont perdus en cas de sinistre, vous devez recréer le ou les groupes et rajouter chaque hôte service Guardian.

**Sauvegarde des certificats**

La commande `Export-HgsServerState` sauvegardera tous les certificats PFX ajoutés à SGH au moment de l’exécution de la commande.
Si vous avez ajouté des certificats à SGH à l’aide d’une empreinte numérique (typique pour les certificats non exportables et sauvegardés sur le matériel), vous devrez sauvegarder manuellement les clés privées de vos certificats.
Pour identifier les certificats inscrits auprès de SGH et qui doivent être sauvegardés manuellement, exécutez la commande PowerShell suivante sur n’importe quel nœud de serveur SGH opérationnel.

```powershell
Get-HgsKeyProtectionCertificate | Where-Object { $_.CertificateData.GetType().Name -eq 'CertificateReference' } | Format-Table Thumbprint, @{ Label = 'Subject'; Expression = { $_.CertificateData.Certificate.Subject } }
```

Pour chacun des certificats listés, vous devez sauvegarder manuellement la clé privée.
Si vous utilisez un certificat logiciel qui n’est pas exportable, vous devez contacter votre autorité de certification pour vous assurer qu’il dispose d’une sauvegarde de votre certificat et/ou peut le réémettre à la demande.
Pour les certificats créés et stockés dans des modules de sécurité matériels, vous devez consulter la documentation de votre appareil pour obtenir des conseils sur la planification de la récupération d’urgence.

Vous devez stocker les sauvegardes de certificat en même temps que vos sauvegardes de stratégie d’attestation dans un emplacement sécurisé afin que les deux pièces puissent être restaurées ensemble.

**Configuration supplémentaire à sauvegarder**

L’état du serveur SGH sauvegardé n’inclut pas le nom de votre cluster SGH, les informations de Active Directory ou les certificats SSL utilisés pour sécuriser les communications avec les API SGH.
Ces paramètres sont importants pour des tâches de cohérence, mais ils ne sont pas essentiels pour remettre en ligne votre cluster SGH après un incident.

Pour capturer le nom du service SGH, exécutez `Get-HgsServer` et notez le nom plat dans les URL d’attestation et de protection de clé.
Par exemple, si l’URL d’attestation est «<http://hgs.contoso.com/Attestation>», « SGH » est le nom du service SGH.

Le domaine Active Directory utilisé par SGH doit être géré comme tout autre domaine Active Directory.
Lors de la restauration du SGH après un incident, vous n’aurez pas forcément besoin de recréer les objets exacts présents dans le domaine actuel.
Toutefois, il facilitera la récupération si vous sauvegardez Active Directory et conservez une liste des utilisateurs JEA autorisés à gérer le système, ainsi que l’appartenance à des groupes de sécurité utilisés par l’attestation approuvée par l’administrateur pour autoriser les hôtes service Guardian.

Pour identifier l’empreinte numérique des certificats SSL configurés pour SGH, exécutez la commande suivante dans PowerShell.
Vous pouvez ensuite sauvegarder ces certificats SSL conformément aux instructions de votre fournisseur de certificats.

```powershell
Get-WebBinding -Protocol https | Select-Object certificateHash
```

### <a name="restoring-hgs-from-a-backup"></a>Restauration de SGH à partir d’une sauvegarde
Les étapes suivantes décrivent comment restaurer les paramètres SGH à partir d’une sauvegarde.
Les étapes s’appliquent aux deux situations dans lesquelles vous essayez d’annuler les modifications apportées à vos instances SGH déjà en cours d’exécution et lorsque vous êtes en train de créer un nouveau cluster SGH après une perte complète de votre précédent.

#### <a name="set-up-a-replacement-hgs-cluster"></a>Configurer un cluster SGH de remplacement
Avant de pouvoir restaurer SGH, vous devez disposer d’un cluster SGH initialisé dans lequel vous pouvez restaurer la configuration.
Si vous importez simplement des paramètres qui ont été accidentellement supprimés dans un cluster existant (en cours d’exécution), vous pouvez ignorer cette étape.
Si vous récupérez une perte complète de SGH, vous devrez installer et initialiser au moins un nœud SGH en suivant les [instructions du Guide de déploiement](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

Plus précisément, vous devrez :
1. [Configurer le domaine SGH](guarded-fabric-choose-where-to-install-hgs.md) ou joindre SGH à un domaine existant
2. [Initialisez le serveur SGH](guarded-fabric-initialize-hgs.md) à l’aide de vos clés existantes *ou* d’un ensemble de clés temporaires. Vous pouvez [Supprimer les clés temporaires](#renewing-or-replacing-keys) après l’importation de vos clés réelles à partir des fichiers de sauvegarde SGH.
3. [Importer les paramètres SGH](#import-settings-from-a-backup) à partir de votre sauvegarde pour restaurer les groupes hôtes approuvés, les stratégies d’intégrité du code, les lignes de base du module de plateforme sécurisée et les IDENTIFICATEURs TPM

> [!TIP]
> Le nouveau cluster SGH n’a pas besoin d’utiliser les mêmes certificats, nom de service ou domaine que l’instance SGH à partir de laquelle votre fichier de sauvegarde a été exporté.

#### <a name="import-settings-from-a-backup"></a>Importer des paramètres à partir d’une sauvegarde

Pour restaurer les stratégies d’attestation, les certificats basés sur PFX et les clés publiques des certificats non-PFX sur votre nœud SGH à partir d’un fichier de sauvegarde, exécutez la commande suivante sur un nœud de serveur SGH initialisé.
Vous serez invité à entrer le mot de passe que vous avez spécifié lors de la création de la sauvegarde.

```powershell
Import-HgsServerState -Path C:\Temp\HGSBackup.xml
```

Si vous souhaitez uniquement importer des stratégies d’attestation approuvées par l’administrateur ou des stratégies d’attestation approuvées par le module de plateforme sécurisée (TPM), vous pouvez le faire en spécifiant les indicateurs `-ImportActiveDirectoryModeState` ou `-ImportTpmModeState` à [Import-HgsServerState](https://technet.microsoft.com/library/mt652168.aspx).

Vérifiez que la dernière mise à jour cumulative pour Windows Server 2016 est installée avant d’exécuter `Import-HgsServerState`.
Dans le cas contraire, vous risquez d’obtenir une erreur d’importation.

> [!NOTE]
> Si vous restaurez des stratégies sur un nœud SGH existant sur lequel une ou plusieurs de ces stratégies sont déjà installées, la commande d’importation affiche une erreur pour chaque stratégie dupliquée.
> Il s’agit d’un comportement attendu qui peut être ignoré dans la plupart des cas.

#### <a name="reinstall-private-keys-for-certificates"></a>Réinstaller des clés privées pour les certificats
Si l’un des certificats utilisés sur le SGH à partir duquel la sauvegarde a été créée a été ajouté à l’aide des empreintes numériques, seule la clé publique de ces certificats sera incluse dans le fichier de sauvegarde.
Cela signifie que vous devrez installer et/ou accorder manuellement l’accès aux clés privées pour chacun de ces certificats avant que le service SGH puisse traiter les demandes des hôtes Hyper-V.
Les actions nécessaires pour effectuer cette étape varient en fonction de la façon dont votre certificat a été émis à l’origine.
Pour les certificats prédéfinis émis par une autorité de certification, vous devez contacter votre autorité de certification pour obtenir la clé privée et l’installer sur **chaque** nœud SGH selon leurs instructions.
De même, si vos certificats sont sauvegardés sur le matériel, vous devez consulter la documentation du fournisseur de votre module de sécurité matériel pour installer le ou les pilotes nécessaires sur chaque nœud SGH pour vous connecter au HSM et accorder à chaque ordinateur un accès à la clé privée.

En guise de rappel, les certificats ajoutés à SGH à l’aide d’empreintes numériques nécessitent une réplication manuelle des clés privées sur chaque nœud.
Vous devrez répéter cette étape sur chaque nœud supplémentaire que vous ajoutez au cluster SGH restauré.

#### <a name="review-imported-attestation-policies"></a>Examiner les stratégies d’attestation importées
Une fois vos paramètres importés à partir d’une sauvegarde, il est recommandé d’examiner attentivement toutes les stratégies importées à l’aide de `Get-HgsAttestationPolicy` pour vous assurer que seuls les hôtes auxquels vous faites confiance pour exécuter des machines virtuelles dotées d’une protection maximale sont en mesure d’effectuer une attestation.
Si vous trouvez des stratégies qui ne correspondent plus à votre position de sécurité, vous pouvez [les désactiver ou les supprimer](#review-attestation-policies).

#### <a name="run-diagnostics-to-check-system-state"></a>Exécuter les diagnostics pour vérifier l’état du système
Une fois que vous avez fini de configurer et de restaurer l’état de votre nœud SGH, vous devez exécuter l’outil de diagnostics SGH pour vérifier l’état du système.
Pour ce faire, exécutez la commande suivante sur le nœud SGH où vous avez restauré la configuration :

```powershell
Get-HgsTrace -RunDiagnostics
```

Si le « résultat global » n’est pas « Pass », des étapes supplémentaires sont nécessaires pour terminer la configuration du système.
Vérifiez les messages signalés dans le ou les sous-tests qui ont échoué pour plus d’informations.

## <a name="patching-hgs"></a>Mise à jour corrective du SGH
Il est important de tenir à jour vos nœuds de service Guardian hôte en installant la dernière mise à jour cumulative lorsqu’elle sort. Si vous configurez un tout nouveau nœud SGH, il est fortement recommandé d’installer toutes les mises à jour disponibles avant d’installer le rôle SGH ou de le configurer.
Cela permet de s’assurer que toutes les fonctionnalités nouvelles ou modifiées prennent effet immédiatement.

Lors de l’application d’un correctif à votre infrastructure protégée, il est fortement recommandé de commencer par mettre à niveau *tous les* ordinateurs hôtes Hyper-V **avant de procéder à la mise à niveau de SGH**.
Cela permet de s’assurer que toutes les modifications apportées aux stratégies d’attestation sur le SGH sont effectuées *une fois que* les hôtes Hyper-V ont été mis à jour pour fournir les informations nécessaires.
Si une mise à jour va modifier le comportement des stratégies, elles ne seront pas automatiquement activées pour éviter toute interruption de votre infrastructure.
Pour ces mises à jour, vous devez suivre les instructions de la section suivante pour activer les stratégies d’attestation nouvelles ou modifiées.
Nous vous encourageons à lire les notes de publication pour Windows Server et toutes les mises à jour cumulatives que vous installez pour vérifier si les mises à jour de stratégie sont requises.

### <a name="updates-requiring-policy-activation"></a>Mises à jour nécessitant l’activation de la stratégie
Si une mise à jour pour SGH introduit ou modifie de manière significative le comportement d’une stratégie d’attestation, une étape supplémentaire est nécessaire pour activer la stratégie modifiée.
Les modifications de stratégie sont uniquement effectuées après l’exportation et l’importation de l’État SGH.
Vous devez uniquement activer les stratégies nouvelles ou modifiées après avoir appliqué la mise à jour cumulative à tous les ordinateurs hôtes et tous les nœuds SGH de votre environnement.
Une fois que chaque ordinateur a été mis à jour, exécutez les commandes suivantes sur n’importe quel nœud SGH pour déclencher le processus de mise à niveau :

```powershell
$password = Read-Host -AsSecureString -Prompt "Enter a temporary password"
Export-HgsServerState -Path .\temporaryExport.xml -Password $password
Import-HgsServerState -Path .\temporaryExport.xml -Password $password
```

Si une nouvelle stratégie a été introduite, elle est désactivée par défaut.
Pour activer la nouvelle stratégie, commencez par la trouver dans la liste des stratégies Microsoft (avec le préfixe « HGS_ »), puis activez-la à l’aide des commandes suivantes :

```powershell
Get-HgsAttestationPolicy

Enable-HgsAttestationPolicy -Name <Hgs_NewPolicyName>
```

## <a name="managing-attestation-policies"></a>Gestion des stratégies d’attestation
SGH gère plusieurs stratégies d’attestation qui définissent l’ensemble minimal d’exigences qu’un hôte doit respecter pour être considéré comme sain et autorisé à exécuter des machines virtuelles protégées.
Certaines de ces stratégies sont définies par Microsoft, d’autres vous permettent de définir les stratégies d’intégrité du code autorisées, les lignes de base du module de plateforme sécurisée et les hôtes de votre environnement.
Une maintenance régulière de ces stratégies est nécessaire pour s’assurer que les hôtes peuvent continuer à attester correctement quand vous les mettez à jour et les remplacer, et pour vous assurer que les ordinateurs hôtes ou configurations non fiables ne peuvent pas être correctement attestés.

Pour une attestation approuvée par l’administrateur, il n’existe qu’une seule stratégie qui détermine si un ordinateur hôte est sain : l’appartenance à un groupe de sécurité connu et approuvé.
L’attestation TPM est plus compliquée et implique plusieurs stratégies pour mesurer le code et la configuration d’un système avant de déterminer s’il est sain.

Une seule SGH peut être configurée à la fois avec des Active Directory et des stratégies TPM, mais le service vérifie uniquement les stratégies pour le mode actuel sur lequel il est configuré lorsqu’un ordinateur hôte tente d’attester.
Pour vérifier le mode de votre serveur SGH, exécutez `Get-HgsServer`.

### <a name="default-policies"></a>Stratégies par défaut
Pour une attestation approuvée par le module de plateforme sécurisée, il existe plusieurs stratégies intégrées configurées sur le SGH.
Certaines de ces stratégies sont « verrouillées », ce qui signifie qu’elles ne peuvent pas être désactivées pour des raisons de sécurité.
Le tableau ci-dessous décrit l’objectif de chaque stratégie par défaut.

Nom de la stratégie                    | Objectif
-------------------------------|-----------------------------------------------------
Hgs_SecureBootEnabled          | Requiert l’activation du démarrage sécurisé pour les hôtes. Cela est nécessaire pour mesurer les binaires de démarrage et d’autres paramètres verrouillés.
Hgs_UefiDebugDisabled          | Garantit que les hôtes n’ont pas de débogueur de noyau activé. Les débogueurs en mode utilisateur sont bloqués avec les stratégies d’intégrité du code.
Hgs_SecureBootSettings         | Stratégie négative pour garantir que les hôtes correspondent à au moins une ligne de base TPM (définie par l’administrateur).
Hgs_CiPolicy                   | Stratégie négative pour garantir que les hôtes utilisent l’une des stratégies CI définies par l’administrateur.
Hgs_HypervisorEnforcedCiPolicy | Requiert que la stratégie d’intégrité du code soit appliquée par l’hyperviseur. La désactivation de cette stratégie affaiblit vos protections contre les attaques de stratégie d’intégrité du code en mode noyau.
Hgs_FullBoot                   | Garantit que l’ordinateur hôte n’a pas repris du mode veille ou veille prolongée. Les hôtes doivent être correctement redémarrés ou arrêtés pour passer cette stratégie.
Hgs_VsmIdkPresent              | Requiert l’exécution de la sécurité basée sur la virtualisation sur l’ordinateur hôte. IDK représente la clé nécessaire pour chiffrer les informations renvoyées à l’espace mémoire sécurisé de l’hôte.
Hgs_PageFileEncryptionEnabled  | Requiert le chiffrement des scripts d’échange sur l’hôte. La désactivation de cette stratégie peut entraîner une exposition des informations si un fichier d’échange non chiffré est inspecté pour identifier les secrets du locataire.
Hgs_BitLockerEnabled           | Nécessite l’activation de BitLocker sur l’hôte Hyper-V. Cette stratégie est désactivée par défaut pour des raisons de performances et n’est pas recommandée pour être activée. Cette stratégie n’a aucun impact sur le chiffrement des machines virtuelles protégées elles-mêmes.
Hgs_IommuEnabled               | Requiert l’utilisation d’un appareil IOMMU par l’hôte pour empêcher les attaques d’accès direct à la mémoire. La désactivation de cette stratégie et l’utilisation d’ordinateurs hôtes sans un IOMMU activé peuvent exposer les secrets des machines virtuelles clientes pour les attaques de mémoire directe.
Hgs_NoHibernation              | Nécessite la désactivation de la mise en veille prolongée sur l’hôte Hyper-V. La désactivation de cette stratégie peut permettre aux hôtes d’enregistrer la mémoire de machine virtuelle protégée dans un fichier de mise en veille prolongée non chiffré.
Hgs_NoDumps                    | Nécessite la désactivation des vidages de mémoire sur l’hôte Hyper-V. Si vous désactivez cette stratégie, il est recommandé de configurer le chiffrement de vidage pour empêcher l’enregistrement de la mémoire de machine virtuelle protégée dans des fichiers de vidage sur incident non chiffrés.
Hgs_DumpEncryption             | Requiert des vidages de mémoire, s’ils sont activés sur l’hôte Hyper-V, à chiffrer avec une clé de chiffrement approuvée par SGH. Cette stratégie ne s’applique pas si les dumps ne sont pas activés sur l’ordinateur hôte. Si cette stratégie et les *Novidages de\_SGH* sont désactivés, la mémoire de machine virtuelle protégée peut être enregistrée dans un fichier dump non chiffré.
Hgs_DumpEncryptionKey          | Stratégie négative pour garantir que les hôtes configurés pour autoriser les vidages de mémoire utilisent une clé de chiffrement de fichier dump définie par l’administrateur, connue de SGH. Cette stratégie ne s’applique pas quand le *DumpEncryption\_SGH* est désactivé.

### <a name="authorizing-new-guarded-hosts"></a>Autorisation des nouveaux hôtes service Guardian
Pour autoriser un nouvel hôte à devenir un hôte service Guardian (par exemple, attestation réussie), SGH doit approuver l’hôte et (lorsqu’il est configuré pour utiliser l’attestation de confiance TPM) le logiciel en cours d’exécution.
Les étapes permettant d’autoriser un nouvel hôte varient en fonction du mode d’attestation pour lequel SGH est actuellement configuré.
Pour vérifier le mode d’attestation pour votre infrastructure protégée, exécutez `Get-HgsServer` sur tout nœud SGH.

#### <a name="software-configuration"></a>Configuration logicielle
Sur le nouvel ordinateur hôte Hyper-V, assurez-vous que Windows Server 2016 Datacenter Edition est installé.
Windows Server 2016 standard ne peut pas exécuter de machines virtuelles protégées dans une infrastructure protégée.
L’ordinateur hôte peut être installé sur l’ordinateur de bureau ou sur Server Core.

Sur le serveur avec expérience utilisateur et Server Core, vous devez installer les rôles de serveur de prise en charge Hyper-V et Guardian hôte pour Hyper-V :

```powershell
Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
```

#### <a name="admin-trusted-attestation"></a>Attestation approuvée par l’administrateur
Pour inscrire un nouvel hôte dans SGH lors de l’utilisation d’une attestation approuvée par l’administrateur, vous devez d’abord ajouter l’ordinateur hôte à un groupe de sécurité dans le domaine auquel il est joint.
En règle générale, chaque domaine aura un groupe de sécurité pour les hôtes service Guardian.
Si vous avez déjà inscrit ce groupe auprès de SGH, la seule action que vous devez effectuer consiste à redémarrer l’hôte pour actualiser son appartenance à un groupe.

Vous pouvez vérifier quels groupes de sécurité sont approuvés par SGH en exécutant la commande suivante :

```powershell
Get-HgsAttestationHostGroup
```

Pour inscrire un nouveau groupe de sécurité avec SGH, commencez par capturer l’identificateur de sécurité (SID) du groupe dans le domaine de l’hôte et inscrivez le SID auprès de SGH.

```powershell
Add-HgsAttestationHostGroup -Name "Contoso Guarded Hosts" -Identifier "S-1-5-21-3623811015-3361044348-30300820-1013"
```

Des instructions sur la configuration de l’approbation entre le domaine hôte et le groupe de sécurité (SGH) sont disponibles dans le Guide de déploiement.

#### <a name="tpm-trusted-attestation"></a>Attestation approuvée par le module de plateforme sécurisée
Lorsque SGH est configuré en mode TPM, les hôtes doivent réussir toutes les stratégies verrouillées et les stratégies « activées » préfixées par le préfixe « Hgs_ », ainsi qu’au moins une ligne de base du module de plateforme sécurisée, un identificateur de module de plateforme sécurisée et une stratégie d’intégrité du code.
Chaque fois que vous ajoutez un nouvel ordinateur hôte, vous devez inscrire le nouvel identificateur du module de plateforme sécurisée auprès de SGH.
Tant que l’hôte exécute le même logiciel (et que la même stratégie d’intégrité du code est appliquée) et la ligne de base du module de plateforme sécurisée en tant qu’autre hôte dans votre environnement, vous n’avez pas besoin d’ajouter de nouvelles stratégies ou de nouvelles lignes de base.

**Ajout de l’identificateur TPM pour un nouvel hôte** Sur le nouvel ordinateur hôte, exécutez la commande suivante pour capturer l’identificateur TPM.
Veillez à spécifier un nom unique pour l’hôte qui vous permettra de le Rechercher sur le service SGH.
Vous aurez besoin de ces informations si vous désactivez l’ordinateur hôte ou souhaitez l’empêcher d’exécuter des machines virtuelles protégées dans le service SGH.

```powershell
(Get-PlatformIdentifier -Name "Host01").InnerXml | Out-File C:\temp\host01.xml -Encoding UTF8
```

Copiez ce fichier sur votre serveur SGH, puis exécutez la commande suivante pour inscrire l’hôte auprès de SGH.

```powershell
Add-HgsAttestationTpmHost -Name 'Host01' -Path C:\temp\host01.xml
```

**Ajout d’une nouvelle ligne de base TPM** Si le nouvel hôte exécute une nouvelle configuration matérielle ou de microprogramme pour votre environnement, vous devrez peut-être prendre une nouvelle ligne de base du module de plateforme sécurisée (TPM).
Pour ce faire, exécutez la commande suivante sur l’hôte.

```powershell
Get-HgsAttestationBaselinePolicy -Path 'C:\temp\hardwareConfig01.tcglog'
```

> [!NOTE]
> Si vous recevez une erreur indiquant que votre hôte n’a pas pu être validé et qu’il n’est pas sanctionné avec succès, ne vous inquiétez pas.
> Il s’agit d’une vérification de la configuration requise pour vous assurer que votre hôte peut exécuter des machines virtuelles dotées d’une protection maximale, et que vous n’avez probablement pas encore appliqué une stratégie d’intégrité du code ou un autre paramètre requis.
> Lisez le message d’erreur, apportez les modifications suggérées par ce dernier, puis réessayez.
> Vous pouvez également ignorer la validation pour l’instant en ajoutant l’indicateur `-SkipValidation` à la commande.

Copiez la ligne de base du module de plateforme sécurisée sur votre serveur SGH, puis inscrivez-la avec la commande suivante.
Nous vous encourageons à utiliser une convention d’affectation de noms qui vous aide à comprendre la configuration du matériel et du microprogramme de cette classe d’hôte Hyper-V.

```powershell
Add-HgsAttestationTpmPolicy -Name 'HardwareConfig01' -Path 'C:\temp\hardwareConfig01.tcglog'
```

**Ajout d’une nouvelle stratégie d’intégrité du code** Si vous avez modifié la stratégie d’intégrité du code en cours d’exécution sur vos ordinateurs hôtes Hyper-V, vous devez inscrire la nouvelle stratégie auprès de SGH pour que ces derniers puissent être sanctionnés correctement.
Sur un hôte de référence, qui sert d’image maître pour les machines Hyper-V approuvées dans votre environnement, capturez une nouvelle stratégie CI à l’aide de la commande `New-CIPolicy`.
Nous vous encourageons à utiliser le niveau **FilePublisher** et le secours de **hachage** pour les stratégies ci de l’hôte Hyper-V.
Vous devez d’abord créer une stratégie CI en mode audit pour vous assurer que tout fonctionne comme prévu.
Après avoir validé un exemple de charge de travail sur le système, vous pouvez appliquer la stratégie et copier la version appliquée dans SGH.
Pour obtenir la liste complète des options de configuration de la stratégie d’intégrité du code, consultez la [documentation de Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies).

```powershell
# Capture a new CI policy with the FilePublisher primary level and Hash fallback and enable user mode code integrity protections
New-CIPolicy -FilePath 'C:\temp\ws2016-hardware01-ci.xml' -Level FilePublisher -Fallback Hash -UserPEs

# Apply the CI policy to the system
ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\ws2016-hardware01-ci.xml' -BinaryFilePath 'C:\temp\ws2016-hardware01-ci.p7b'
Copy-Item 'C:\temp\ws2016-hardware01-ci.p7b' 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'
Restart-Computer

# Check the event log for any untrusted binaries and update the policy if necessary
# Consult the Device Guard documentation for more details

# Change the policy to be in enforced mode
Set-RuleOption -FilePath 'C:\temp\ws2016-hardare01-ci.xml' -Option 3 -Delete

# Apply the enforced CI policy on the system
ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\ws2016-hardware01-ci.xml' -BinaryFilePath 'C:\temp\ws2016-hardware01-ci.p7b'
Copy-Item 'C:\temp\ws2016-hardware01-ci.p7b' 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'
Restart-Computer
```

Une fois votre stratégie créée, testée et appliquée, copiez le fichier binaire (. p7b) sur votre serveur SGH et inscrivez la stratégie.

```powershell
Add-HgsAttestationCiPolicy -Name 'WS2016-Hardware01' -Path 'C:\temp\ws2016-hardware01-ci.p7b'
```

**Ajout d’une clé de chiffrement d’image mémoire**

Lorsque la stratégie de *Nodumps\_SGH* est désactivée et que la stratégie *DumpEncryption\_SGH* est activée, les hôtes service Guardian sont autorisés à avoir des vidages de mémoire (y compris des vidages sur incident) à activer tant que ces vidages sont chiffrés. Les hôtes service Guardian ne réussissent l’attestation que s’ils ont des vidages de mémoire désactivés ou s’ils les chiffrent avec une clé connue du SGH. Par défaut, aucune clé de chiffrement de vidage n’est configurée sur SGH.

Pour ajouter une clé de chiffrement de vidage à SGH, utilisez l’applet de commande `Add-HgsAttestationDumpPolicy` pour fournir SGH avec le hachage de votre clé de chiffrement de vidage.
Si vous capturez une ligne de base du module de plateforme sécurisée sur un ordinateur hôte Hyper-V configuré avec le chiffrement de vidage, le hachage est inclus dans le tcglog et peut être fourni à l’applet de commande `Add-HgsAttestationDumpPolicy`.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey01' -Path 'C:\temp\TpmBaselineWithDumpEncryptionKey.tcglog'
```

Vous pouvez également fournir directement la représentation sous forme de chaîne du hachage à l’applet de commande.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey02' -PublicKeyHash '<paste your hash here>'
```

Veillez à ajouter chaque clé de chiffrement de vidage unique à SGH si vous choisissez d’utiliser des clés différentes sur votre infrastructure protégée.
Les hôtes qui chiffrent les vidages de mémoire avec une clé non connue du SGH ne passeront pas l’attestation.

Pour plus d’informations sur la [configuration du chiffrement de vidage sur les ordinateurs hôtes](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption), consultez la documentation Hyper-V.

#### <a name="check-if-the-system-passed-attestation"></a>Vérifier si l’attestation du système a réussi
Après avoir enregistré les informations nécessaires avec SGH, vous devez vérifier si l’hôte passe l’attestation.
Sur l’hôte Hyper-V nouvellement ajouté, exécutez `Set-HgsClientConfiguration` et fournissez les URL correctes pour votre cluster SGH.
Ces URL peuvent être obtenues en exécutant `Get-HgsServer` sur n’importe quel nœud SGH.

```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'http://hgs.bastion.local/KeyProtection' -AttestationServerUrl 'http://hgs.bastion.local/Attestation'
```

Si l’état résultant n’indique pas « IsHostGuarded : true », vous devez résoudre les problèmes de configuration.
Sur l’ordinateur hôte dont l’attestation a échoué, exécutez la commande suivante pour obtenir un rapport détaillé sur les problèmes susceptibles de vous aider à résoudre l’échec de l’attestation.

```powershell
Get-HgsTrace -RunDiagnostics -Detailed
```

> [!IMPORTANT]
> Si vous utilisez Windows Server 2019 ou Windows 10, version 1809 et que vous utilisez des stratégies d’intégrité du code, `Get-HgsTrace` pouvez retourner un échec pour la **stratégie d’intégrité du code active** diagnostic.
> Vous pouvez ignorer ce résultat en toute sécurité lorsqu’il s’agit du seul diagnostic qui échoue.

### <a name="review-attestation-policies"></a>Examiner les stratégies d’attestation
Pour passer en revue l’état actuel des stratégies configurées sur SGH, exécutez les commandes suivantes sur tout nœud SGH :

```powershell
# List all trusted security groups for admin-trusted attestation
Get-HgsAttestationHostGroup

# List all policies configured for TPM-trusted attestation
Get-HgsAttestationPolicy
```

Si vous trouvez une stratégie activée qui ne répond plus à vos exigences de sécurité (par exemple, une ancienne stratégie d’intégrité du code qui est désormais considérée comme non sécurisée), vous pouvez la désactiver en remplaçant le nom de la stratégie dans la commande suivante :

```powershell
Disable-HgsAttestationPolicy -Name 'PolicyName'
```

De même, vous pouvez utiliser `Enable-HgsAttestationPolicy` pour réactiver une stratégie.

Si vous n’avez plus besoin d’une stratégie et souhaitez la supprimer de tous les nœuds SGH, exécutez `Remove-HgsAttestationPolicy -Name 'PolicyName'` pour supprimer définitivement la stratégie.

## <a name="changing-attestation-modes"></a>Modification des modes d’attestation
Si vous avez démarré votre infrastructure protégée à l’aide d’une attestation approuvée par l’administrateur, vous souhaiterez probablement effectuer une mise à niveau vers le mode d’attestation TPM le plus fort possible dès que vous disposez d’un nombre suffisant d’hôtes compatibles TPM 2,0 dans votre environnement.
Lorsque vous êtes prêt à basculer, vous pouvez précharger tous les artefacts d’attestation (stratégies CI, lignes de base du module de plateforme sécurisée et identificateurs TPM) dans SGH, tout en continuant à exécuter SGH avec l’attestation approuvée par l’administrateur.
Pour ce faire, suivez simplement les instructions de la section [autorisation d’un nouvel hôte service Guardian](#authorizing-new-guarded-hosts) .

Une fois que vous avez ajouté toutes vos stratégies à SGH, l’étape suivante consiste à exécuter une tentative d’attestation synthétique sur vos ordinateurs hôtes pour voir s’ils réussissent l’attestation en mode TPM.
Cela n’affecte pas l’état opérationnel actuel de SGH.
Les commandes ci-dessous doivent être exécutées sur un ordinateur qui a accès à tous les hôtes de l’environnement et au moins un nœud SGH.
Si votre pare-feu ou d’autres stratégies de sécurité l’empêchent, vous pouvez ignorer cette étape.
Dans la mesure du possible, nous vous recommandons d’exécuter l’attestation de synthèse pour vous donner une bonne indication que le fait de basculer en mode TPM entraînera un temps d’arrêt pour vos machines virtuelles. 

```powershell
# Get information for each host in your environment
$hostNames = 'host01.contoso.com', 'host02.contoso.com', 'host03.contoso.com'
$credential = Get-Credential -Message 'Enter a credential with admin privileges on each host'
$targets = @()
$hostNames | ForEach-Object { $targets += New-HgsTraceTarget -Credential $credential -Role GuardedHost -HostName $_ }

$hgsCredential = Get-Credential -Message 'Enter an admin credential for HGS'
$targets += New-HgsTraceTarget -Credential $hgsCredential -Role HostGuardianService -HostName 'HGS01.bastion.local'

# Initiate the synthetic attestation attempt
Get-HgsTrace -RunDiagnostics -Target $targets -Diagnostic GuardedFabricTpmMode 
```

Une fois les diagnostics terminés, examinez les informations générées pour déterminer si des ordinateurs hôtes auraient échoué à l’attestation en mode TPM.
Réexécutez les diagnostics jusqu’à ce que vous obteniez un « passage » de chaque ordinateur hôte, puis passez à modifier le mode SGH en mode TPM.

La **modification en mode TPM** prend juste une seconde.
Exécutez la commande suivante sur n’importe quel nœud SGH pour mettre à jour le mode d’attestation.

```powershell
Set-HgsServer -TrustTpm
```

Si vous rencontrez des problèmes et que vous devez revenir en mode Active Directory, vous pouvez le faire en exécutant `Set-HgsServer -TrustActiveDirectory`.

Une fois que vous avez vérifié que tout fonctionne comme prévu, vous devez supprimer tous les groupes hôtes de Active Directory approuvés de SGH et supprimer l’approbation entre les domaines SGH et fabric.
Si vous laissez la Active Directory confiance en place, vous risquez de réactiver l’approbation et de basculer le SGH en mode Active Directory, ce qui pourrait permettre l’exécution de code non fiable sur vos hôtes service Guardian.

## <a name="key-management"></a>Gestion des clés
La solution d’infrastructure protégée utilise plusieurs paires de clés publiques/privées pour valider l’intégrité de divers composants dans la solution et pour chiffrer les secrets des locataires.
Le service Guardian hôte est configuré avec au moins deux certificats (avec des clés publiques et privées), qui sont utilisés pour la signature et le chiffrement des clés utilisées pour démarrer les machines virtuelles protégées.
Ces clés doivent être gérées avec soin.
Si la clé privée est acquise par un adversaire, elle sera en mesure de déprotéger toutes les machines virtuelles s’exécutant sur votre infrastructure ou de configurer un cluster SGH imposteur qui utilise des stratégies d’attestation plus faibles pour contourner les protections que vous mettez en place.
Si vous perdez les clés privées pendant un sinistre et que vous ne les trouvez pas dans une sauvegarde, vous devez configurer une nouvelle paire de clés et faire en sorte que chaque machine virtuelle soit réaffectée pour autoriser vos nouveaux certificats.

Cette section traite des principales rubriques relatives à la gestion des clés pour vous aider à configurer vos clés afin qu’elles soient fonctionnelles et sécurisées.

### <a name="adding-new-keys"></a>Ajout de nouvelles clés
Alors que SGH doit être initialisé avec un ensemble de clés, vous pouvez ajouter plusieurs clés de chiffrement et de signature à SGH.
Les deux raisons les plus courantes pour lesquelles vous ajoutez de nouvelles clés à SGH sont les suivantes :
1. Pour prendre en charge les scénarios « apportez votre propre clé » où les locataires copient leurs clés privées dans votre module de sécurité matériel et autorisent uniquement leurs clés à démarrer leurs machines virtuelles protégées.
2. Pour remplacer les clés existantes pour SGH, commencez par ajouter les nouvelles clés et conserver les deux jeux de clés jusqu’à ce que chaque configuration de machine virtuelle ait été mise à jour pour utiliser les nouvelles clés.

Le processus d’ajout de vos nouvelles clés diffère selon le type de certificat que vous utilisez.

**Option 1 : ajout d’un certificat stocké dans un module HSM**

Notre approche recommandée pour sécuriser les clés SGH consiste à utiliser des certificats créés dans un module de sécurité matériel (HSM).
Les modules HSM garantissent l’utilisation de vos clés sont liés à un accès physique à un appareil sensible à la sécurité dans votre centre de données.
Chaque HSM est différent et possède un processus unique pour créer des certificats et les inscrire auprès de SGH.
Les étapes ci-dessous sont destinées à fournir des conseils approximatifs pour l’utilisation de certificats sauvegardés par HSM.
Consultez la documentation de votre fournisseur HSM pour obtenir des étapes et des fonctionnalités exactes.

1. Installez le logiciel HSM sur chaque nœud SGH de votre cluster. Selon que vous disposez d’un réseau ou d’un périphérique HSM local, vous devrez peut-être configurer le HSM pour accorder à votre ordinateur l’accès à son magasin de clés.
2. Créer 2 certificats dans le HSM avec des **clés RSA 2048 bits** pour le chiffrement et la signature
    1. Créer un certificat de chiffrement avec la propriété utilisation de la clé de chiffrement des **données** dans votre HSM
    2. Créer un certificat de signature avec la propriété d’utilisation de la clé de **signature numérique** dans votre HSM
3. Installez les certificats dans le magasin de certificats local de chaque nœud SGH conformément aux instructions de votre fournisseur de module de sécurité matériel.
4. Si votre module HSM utilise des autorisations granulaires pour accorder à des utilisateurs ou des applications spécifiques l’autorisation d’utiliser la clé privée, vous devez accorder à votre compte de service administré de groupe SGH l’accès au certificat. Vous pouvez rechercher le nom du compte gMSA SGH en exécutant `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
5. Ajoutez les certificats de signature et de chiffrement à SGH en remplaçant les empreintes numériques par celles de vos certificats dans les commandes suivantes :

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA"
    ```

**Option 2 : ajout de certificats logiciels non exportables**

Si vous disposez d’un certificat logiciel émis par le de votre entreprise ou une autorité de certification publique disposant d’une clé privée non exportable, vous devez ajouter votre certificat à SGH à l’aide de son empreinte numérique.
1. Installez le certificat sur votre ordinateur en fonction des instructions de votre autorité de certification.
2. Accordez au compte de service administré de groupe SGH un accès en lecture à la clé privée du certificat. Vous pouvez rechercher le nom du compte gMSA SGH en exécutant `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
3. Inscrivez le certificat auprès de SGH à l’aide de la commande suivante et en remplaçant l’empreinte numérique de votre certificat (modifiez le *chiffrement* pour *signer* les certificats de signature) :

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    ```

> [!IMPORTANT]
> Vous devez installer manuellement la clé privée et accorder l’accès en lecture au compte gMSA sur chaque nœud SGH.
> SGH ne peut pas répliquer automatiquement des clés privées pour *un* certificat enregistré par son empreinte numérique.

**Option 3 : ajout de certificats stockés dans des fichiers PFX**

Si vous disposez d’un certificat logiciel avec une clé privée exportable qui peut être stockée au format de fichier PFX et sécurisée à l’aide d’un mot de passe, SGH peut automatiquement gérer vos certificats pour vous.
Les certificats ajoutés aux fichiers PFX sont automatiquement répliqués sur chaque nœud de votre cluster SGH et SGH sécurise l’accès aux clés privées.
Pour ajouter un nouveau certificat à l’aide d’un fichier PFX, exécutez les commandes suivantes sur tout nœud SGH (modifier le *chiffrement* pour la *signature* des certificats de signature) :

```powershell
$certPassword = Read-Host -AsSecureString -Prompt "Provide the PFX file password"
Add-HgsKeyProtectionCertificate -CertificateType Encryption -CertificatePath "C:\temp\encryptionCert.pfx" -CertificatePassword $certPassword
```

**Identification et modification des certificats principaux** Alors que SGH peut prendre en charge plusieurs certificats de signature et de chiffrement, il utilise une paire comme certificat « principal ».
Il s’agit des certificats qui seront utilisés si quelqu’un télécharge les métadonnées du gardien pour ce cluster SGH.
Pour vérifier quels certificats sont actuellement marqués comme certificats principaux, exécutez la commande suivante :

```powershell
Get-HgsKeyProtectionCertificate -IsPrimary $true
```

Pour définir un nouveau certificat de chiffrement principal ou de signature, recherchez l’empreinte numérique du certificat souhaité et marquez-le comme principal à l’aide des commandes suivantes :

```powershell
Get-HgsKeyProtectionCertificate
Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899" -IsPrimary
Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA" -IsPrimary
```

### <a name="renewing-or-replacing-keys"></a>Renouvellement ou remplacement de clés
Lorsque vous créez les certificats utilisés par le service SGH, une date d’expiration est attribuée aux certificats en fonction de la stratégie de votre autorité de certification et de vos informations de demande.
Normalement, dans les scénarios où la validité du certificat est importante, par exemple la sécurisation des communications HTTP, les certificats doivent être renouvelés avant d’expirer afin d’éviter une interruption de service ou un message d’erreur inquiétante.
SGH n’utilise pas de certificats dans ce sens.
SGH utilise simplement des certificats comme un moyen pratique de créer et de stocker une paire de clés asymétriques.
Un certificat de chiffrement ou de signature expiré sur SGH n’indique pas une faiblesse ou une perte de protection pour les machines virtuelles protégées.
En outre, les vérifications de révocation de certificat ne sont pas effectuées par SGH.
Si un certificat SGH ou un certificat d’autorité émettrice est révoqué, il n’aura pas d’impact sur l’utilisation du certificat par le service SGH.

La seule fois où vous devez vous préoccuper d’un certificat SGH, c’est que vous avez des raisons de croire que sa clé privée a été volée.
Dans ce cas, l’intégrité de vos machines virtuelles protégées est menacée, car la moitié privée de la paire de clés de chiffrement et de signature SGH est suffisante pour supprimer les protections de protection sur une machine virtuelle ou pour mettre en place un serveur SGH factice qui a des stratégies d’attestation plus faibles.

Si vous vous trouvez dans cette situation ou si les normes de conformité sont requises pour actualiser les clés de certificat régulièrement, les étapes suivantes décrivent le processus de modification des clés sur un serveur SGH.
Notez que les conseils suivants représentent une entreprise importante qui entraîne une interruption de service pour chaque machine virtuelle servie par le cluster SGH.
Une planification appropriée pour la modification des clés SGH est nécessaire pour minimiser les interruptions de service et garantir la sécurité des machines virtuelles locataires.

Sur un nœud SGH, procédez comme suit pour inscrire une nouvelle paire de certificats de chiffrement et de signature.
Consultez la section sur l' [Ajout de nouvelles clés](#adding-new-keys) pour obtenir des informations détaillées sur les différentes façons d’ajouter de nouvelles clés à SGH.
1. Créez une nouvelle paire de certificats de chiffrement et de signature pour votre serveur SGH. Dans l’idéal, ceux-ci seront créés dans un module de sécurité matériel.
2. Inscrire les nouveaux certificats de chiffrement et de signature avec **Add-HgsKeyProtectionCertificate**

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>
    ```
3. Si vous avez utilisé des empreintes numériques, vous devez accéder à chaque nœud du cluster pour installer la clé privée et accorder à l’gMSA SGH l’accès à la clé.
4. Faire des certificats les certificats par défaut dans SGH

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsPrimary
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsPrimary
    ```

À ce stade, les données de protection créées avec les métadonnées obtenues à partir du nœud SGH utilisent les nouveaux certificats, mais les machines virtuelles existantes continuent de fonctionner, car les anciens certificats sont toujours là.
Pour vous assurer que toutes les machines virtuelles existantes fonctionnent avec les nouvelles clés, vous devez mettre à jour le protecteur de clé sur chaque machine virtuelle.
Il s’agit d’une action qui requiert que le propriétaire de la machine virtuelle (personne ou entité en possession du gardien « propriétaire ») soit impliqué.
Pour chaque machine virtuelle protégée, procédez comme suit :
5. Arrêtez la machine virtuelle. La machine virtuelle ne peut pas être réactivée tant que les étapes restantes ne sont pas terminées ou que vous n’avez pas besoin de redémarrer le processus.
6. Enregistrer le protecteur de clé actuel dans un fichier : `Get-VMKeyProtector -VMName 'VM001' | Out-File '.\VM001.kp'`
7. Transférer le KP au propriétaire de la machine virtuelle
8. Demander au propriétaire de télécharger les informations de gardien mises à jour à partir de SGH et de les importer sur leur système local
9. Lisez le KP actuel en mémoire, accordez au nouvel gardien l’accès au KP, puis enregistrez-le dans un nouveau fichier en exécutant les commandes suivantes :

    ```powershell
    $kpraw = Get-Content -Path .\VM001.kp
    $kp = ConvertTo-HgsKeyProtector -Bytes $kpraw
    $newGuardian = Get-HgsGuardian -Name 'UpdatedHgsGuardian'
    $updatedKP = Grant-HgsKeyProtectorAccess -KeyProtector $kp -Guardian $newGuardian
    $updatedKP.RawData | Out-File .\updatedVM001.kp
    ```
10. Copier le KP mis à jour dans l’infrastructure d’hébergement
11. Appliquez le KP à la machine virtuelle d’origine :

   ```powershell
   $updatedKP = Get-Content -Path .\updatedVM001.kp
   Set-VMKeyProtector -VMName VM001 -KeyProtector $updatedKP
   ```
12. Enfin, démarrez la machine virtuelle et assurez-vous qu’elle s’exécute correctement.

> [!NOTE]
> Si le propriétaire de la machine virtuelle définit un protecteur de clé incorrect sur la machine virtuelle et n’autorise pas votre infrastructure à exécuter la machine virtuelle, vous ne pourrez pas démarrer la machine virtuelle protégée.
> Pour revenir au dernier protecteur de clé valide connu, exécutez `Set-VMKeyProtector -RestoreLastKnownGoodKeyProtector`

Une fois que toutes les machines virtuelles ont été mises à jour pour autoriser les nouvelles clés Guardian, vous pouvez désactiver et supprimer les anciennes clés.

13. Récupérez les empreintes des anciens certificats de `Get-HgsKeyProtectionCertificate -IsPrimary $false`

14. Désactivez chaque certificat en exécutant les commandes suivantes :  

   ```powershell
   Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsEnabled $false
   Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsEnabled $false
   ```

15. Après avoir vérifié que les machines virtuelles sont toujours en mesure de démarrer avec les certificats désactivés, supprimez les certificats de SGH en exécutant les commandes suivantes :

   ```powershell
   Remove-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>`
   Remove-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>`
   ```

> [!IMPORTANT]
> Les sauvegardes de machine virtuelle contiennent les anciennes informations de protecteur de clé qui permettent d’utiliser les anciens certificats pour démarrer la machine virtuelle.
> Si vous savez que votre clé privée a été compromise, vous devez supposer que les sauvegardes de machines virtuelles peuvent également être compromises et prendre les mesures appropriées.
> La destruction de la configuration de la machine virtuelle à partir des sauvegardes (. vmcx) supprimera les protecteurs de clé, au détriment de la nécessité d’utiliser le mot de passe de récupération BitLocker pour démarrer la machine virtuelle la prochaine fois.

### <a name="key-replication-between-nodes"></a>Réplication de clé entre les nœuds
Chaque nœud du cluster SGH doit être configuré avec les mêmes certificats SSL de chiffrement, de signature et (en cas de configuration).
Cela est nécessaire pour garantir que les demandes d’ordinateurs hôtes Hyper-V accèdent à n’importe quel nœud du cluster peuvent être correctement effectuées.

**Si vous avez initialisé le serveur SGH avec des certificats basés sur pfx** , SGH réplique automatiquement la clé publique et la clé privée de ces certificats sur chaque nœud du cluster.
Il vous suffit d’ajouter les clés sur un nœud.

**Si vous avez initialisé le serveur SGH avec des références de certificat** ou des empreintes numériques, SGH réplique uniquement la clé *publique* dans le certificat sur chaque nœud.
En outre, SGH ne peut pas s’accorder lui-même l’accès à la clé privée sur un nœud de ce scénario.
Par conséquent, il vous incombe de :
1. Installer la clé privée sur chaque nœud SGH
2. Accordez au compte de service administré de groupe SGH (gMSA) l’accès à la clé privée sur chaque nœud. ces tâches ajoutent de la charge opérationnelle supplémentaire. Toutefois, elles sont requises pour les clés et certificats sauvegardés par HSM avec des clés privées non exportables.

Les **certificats SSL** ne sont jamais répliqués sous quelque forme que ce soit.
Il vous incombe d’initialiser chaque serveur SGH avec le même certificat SSL et de mettre à jour chaque serveur chaque fois que vous choisissez de renouveler ou de remplacer le certificat SSL.
Lors du remplacement du certificat SSL, il est recommandé d’utiliser l’applet de commande [Set-HgsServer](https://technet.microsoft.com/library/mt652180.aspx) .

## <a name="unconfiguring-hgs"></a>Annulation de la configuration de SGH

Si vous devez désactiver ou reconfigurer de manière significative un serveur SGH, vous pouvez le faire à l’aide des applets [de commande Clear-hgsserver](https://technet.microsoft.com/library/mt652176.aspx) ou [Uninstall-hgsserver](https://technet.microsoft.com/library/mt652182.aspx) .

### <a name="clearing-the-hgs-configuration"></a>Effacement de la configuration SGH

Pour supprimer un nœud du cluster SGH, utilisez l’applet de commande [Clear-HgsServer](https://technet.microsoft.com/library/mt652176.aspx) .
Cette applet de commande apporte les modifications suivantes sur le serveur sur lequel elle est exécutée :

- Annule l’inscription des services d’attestation et de protection de clé
- Supprime le point de terminaison de gestion JEA de Microsoft. Windows. SGH.
- Supprime l’ordinateur local du cluster de basculement SGH

Si le serveur est le dernier nœud SGH du cluster, le cluster et sa ressource de nom de réseau distribué correspondante seront également détruits.

```powershell
# Removes the local computer from the HGS cluster
Clear-HgsServer
```

Une fois l’opération d’effacement terminée, le serveur SGH peut être réinitialisé avec [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx).
Si vous avez utilisé [install-HgsServer](https://technet.microsoft.com/library/mt652169.aspx) pour configurer un domaine de Active Directory Domain Services, ce domaine restera configuré et opérationnel après l’opération d’effacement.

### <a name="uninstalling-hgs"></a>Désinstallation de SGH

Si vous souhaitez supprimer un nœud du cluster SGH **et** rétrograder le contrôleur domaine Active Directory qui s’exécute sur celui-ci, utilisez l’applet de commande [Uninstall-HgsServer](https://technet.microsoft.com/library/mt652182.aspx) .
Cette applet de commande apporte les modifications suivantes sur le serveur sur lequel elle est exécutée :

- Annule l’inscription des services d’attestation et de protection de clé
- Supprime le point de terminaison de gestion JEA de Microsoft. Windows. SGH.
- Supprime l’ordinateur local du cluster de basculement SGH
- Rétrograde le contrôleur de domaine Active Directory, s’il est configuré

Si le serveur est le dernier nœud SGH du cluster, le domaine, le cluster de basculement et la ressource de nom de réseau distribué du cluster seront également détruits.

```powershell
# Removes the local computer from the HGS cluster and demotes the ADDC (restart required)
$newLocalAdminPassword = Read-Host -AsSecureString -Prompt "Enter a new password for the local administrator account"
Uninstall-HgsServer -LocalAdministratorPassword $newLocalAdminPassword -Restart
```

Une fois l’opération de désinstallation terminée et l’ordinateur redémarré, vous pouvez réinstaller ADDC et SGH à l’aide de [install-hgsserver](https://technet.microsoft.com/library/mt652169.aspx) ou joindre l’ordinateur à un domaine et initialiser le serveur SGH dans ce domaine avec [Initialize-hgsserver](https://technet.microsoft.com/library/mt652185.aspx).

Si vous n’avez plus l’intention d’utiliser l’ordinateur comme un nœud SGH, vous pouvez supprimer le rôle de Windows.

```powershell
Uninstall-WindowsFeature HostGuardianServiceRole
```
