---
title: Gérer le Service Guardian hôte
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: eecb002e-6ae5-4075-9a83-2bbcee2a891c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: dab27e71e42970507f321271edda90f6d161c691
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447392"
---
# <a name="managing-the-host-guardian-service"></a>Gérer le Service Guardian hôte

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Le Service Guardian hôte (SGH) est la pièce maîtresse de la solution d’infrastructure protégée.
Il est chargé de s’assurer que les hôtes Hyper-V dans l’infrastructure sont connus pour l’hébergeur ou d’une entreprise et en cours d’exécution des logiciels approuvés et pour gérer les clés utilisées pour démarrer des machines virtuelles protégées.
Quand un client décide de compter sur vous pour héberger leurs machines virtuelles protégées, ils sont placer leur confiance dans votre configuration et la gestion du Service Guardian hôte.
Par conséquent, il est très important de suivre les meilleures pratiques lors de la gestion du Service Guardian hôte pour garantir la sécurité, la disponibilité et la fiabilité de votre infrastructure protégée.
Les instructions dans les sections suivantes résout les problèmes opérationnels les plus courants sont confrontés les administrateurs de service SGH.

## <a name="limiting-admin-access-to-hgs"></a>Limiter l’accès administrateur à SGH
En raison de la nature sensible de sécurité de SGH, il est important de s’assurer que ses administrateurs sont membres hautement approuvés de votre organisation et, dans l’idéal, séparent les administrateurs de vos ressources de fabric.
En outre, il est recommandé que vous ne gérer SGH à partir de stations de travail sécurisées à l’aide de protocoles de communication sécurisés, tels que WinRM sur HTTPS.

### <a name="separation-of-duties"></a>Séparation des tâches
Lorsque vous configurez SGH, vous disposez de la possibilité de créer une forêt Active Directory isolée simplement pour SGH ou pour joindre des SGH à un domaine existant, approuvé.
Cette décision, ainsi que les rôles que vous affectez les administrateurs de votre organisation, de déterminer la limite d’approbation pour SGH.
Toute personne qui a accès à SGH, directement en tant qu’administrateur ou indirectement en tant qu’administrateur d’un autre élément (par exemple, Active Directory) qui peut influencer SGH, a un contrôle sur votre infrastructure protégée.
Les administrateurs de service SGH choisir les hôtes Hyper-V sont autorisés à exécuter des machines virtuelles protégées et de gérer les certificats nécessaires au démarrage de machines virtuelles protégées.
Un attaquant ou un administrateur malveillant qui a accès à SGH peut utiliser cette puissance pour autoriser des hôtes compromis pour exécuter des machines virtuelles protégées, de lancer une attaque par déni de service en supprimant le matériel de clé et bien plus encore.

Pour éviter ce risque, il est *fortement* recommandé de limiter le chevauchement entre les administrateurs de votre SGH (y compris le domaine auquel est joint SGH) et les environnements Hyper-V.
En vous assurant qu'aucun un administrateur n’a accès aux deux systèmes, l’attaquant doit compromettre 2 différents comptes de 2 personnes pour terminer sa mission pour modifier les stratégies de SGH.
Cela signifie également que les administrateurs de domaine et d’entreprise pour les deux environnements Active Directory ne doivent pas être la même personne, ni SGH doit utiliser la même forêt Active Directory en tant que vos hôtes Hyper-V.
Toute personne qui peuvent s’octroyer l’accès à davantage de ressources présente un risque de sécurité.

### <a name="using-just-enough-administration"></a>À l’aide de Just Enough Administration
SGH est fourni avec [Just Enough Administration](https://aka.ms/JEAdocs) rôles (JEA) intégrées à vous aider à gérer de manière plus sécurisée.
JEA vous aide à en vous permettant de déléguer des tâches d’administration pour les utilisateurs non-administrateurs, ce qui signifie que les personnes qui gèrent les stratégies SGH ne sont pas réellement nécessairement admins du domaine ou ensemble de la machine.
JEA fonctionne en limitant les commandes un utilisateur peut exécuter dans une session PowerShell et à l’aide d’un compte local temporaire dans les coulisses (uniques pour chaque session utilisateur) pour exécuter les commandes qui nécessitent normalement une élévation.

SGH est livré avec 2 rôles JEA préconfigurées :
- **Les administrateurs de service SGH** qui permet aux utilisateurs de gérer toutes les stratégies de SGH, y compris autoriser de nouveaux ordinateurs hôtes pour exécuter des machines virtuelles protégées.
- **Les réviseurs SGH** qui autorise uniquement les utilisateurs le droit d’auditer les stratégies existantes. Ils ne peuvent pas apporter toutes les modifications à la configuration de SGH.

Pour utiliser JEA, vous devez d’abord créer un utilisateur standard et de les rendre membre du groupe de relecteurs SGH ou SGH administrateurs.
Si vous avez utilisé `Install-HgsServer` pour configurer une nouvelle forêt pour SGH, ces groupes sont nommés «*servicename*administrateurs » et «*servicename*réviseurs », respectivement, où *servicename*  est le nom réseau du cluster SGH.
Si vous avez rejoint SGH à un domaine existant, vous devez consulter les noms de groupe que vous avez spécifié dans `Initialize-HgsServer`.

**Créer des utilisateurs standards pour les rôles d’administrateur et réviseur SGH**

```powershell
$hgsServiceName = (Get-ClusterResource HgsClusterResource | Get-ClusterParameter DnsName).Value
$adminGroup = $hgsServiceName + "Administrators"
$reviewerGroup = $hgsServiceName + "Reviewers"

New-ADUser -Name 'hgsadmin01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Admin Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $adminGroup -Members 'hgsadmin01'

New-ADUser -Name 'hgsreviewer01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Reviewer Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $reviewerGroup -Members 'hgsreviewer01'
```

**Stratégies d’audit avec le rôle de réviseur**

Sur un ordinateur distant qui dispose d’une connectivité réseau à SGH, exécutez les commandes suivantes dans PowerShell pour entrer dans la session JEA avec les informations d’identification du réviseur.
Il est important de noter que, dans la mesure où le compte de réviseur est simplement un utilisateur standard, il ne peut pas être utilisé pour régulière accès distant Windows PowerShell, accès Bureau à distance à SGH, etc.

```powershell
Enter-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsreviewer01' -ConfigurationName 'microsoft.windows.hgs'
```

Vous pouvez ensuite vérifier les commandes qui sont autorisées dans la session avec `Get-Command` et exécuter des commandes autorisées pour la configuration de l’audit.
Dans l’exemple ci-dessous, nous vérifions les stratégies qui sont activés sur SGH.

```powershell
Get-Command

Get-HgsAttestationPolicy
```

Tapez la commande `Exit-PSSession` ou son alias, `exit`, lorsque vous avez terminé l’utilisation de la session JEA. 

**Ajouter une nouvelle stratégie à SGH à l’aide du rôle d’administrateur**

Pour réellement modifier une stratégie, vous devez vous connecter au point de terminaison JEA avec une identité qui appartient au groupe « hgsAdministrators ».
Dans l’exemple ci-dessous, nous montrons comment vous pouvez copier une nouvelle stratégie d’intégrité de code à SGH et inscrire à l’aide de JEA.
La syntaxe peut être différente de ce que vous êtes habitué.
Il s’agit pour prendre en charge certaines des restrictions dans JEA comme n’ayant ne pas accès au système de fichiers complète.

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

## <a name="monitoring-hgs"></a>Surveillance SGH
### <a name="event-sources-and-forwarding"></a>Transfert et sources d’événements
Événements à partir de SGH s’affichent dans le journal des événements Windows sous 2 sources :
- **HostGuardianService-Attestation**
- **HostGuardianService-KeyProtection**

Vous pouvez afficher ces événements en ouvrant l’Observateur d’événements et en accédant à Microsoft-Windows-HostGuardianService-d’Attestation et Microsoft-Windows-HostGuardianService-KeyProtection.

Dans un environnement de grande taille, il est souvent préférable de transférer les événements à un collecteur d’événements Windows central pour faciliter l’analyse des événements.
Pour plus d’informations, consultez le [documentation Windows Event Forwarding](https://msdn.microsoft.com/library/windows/desktop/bb427443.aspx).

### <a name="using-system-center-operations-manager"></a>À l’aide de System Center Operations Manager
Vous pouvez également utiliser System Center 2016 - Operations Manager pour surveiller vos hôtes service Guardian et les SGH.
Le pack d’administration de structure protégée a des analyses des événements pour rechercher les erreurs de configuration courantes pouvant conduire à des temps d’arrêt de centre de données, y compris les hôtes ne passe ne pas d’attestation et serveurs SGH signalent des erreurs.

Prise en main, [installer et configurer SCOM 2016](https://technet.microsoft.com/system-center-docs/om/welcome-to-operations-manager) et [télécharger le pack d’administration de structure protégée](https://www.microsoft.com/download/details.aspx?id=52764).
Le guide du pack d’administration inclus explique comment configurer le pack d’administration et de comprendre la portée de ses analyses.

## <a name="backing-up-and-restoring-hgs"></a>Sauvegarde et restauration SGH
### <a name="disaster-recovery-planning"></a>Planification de la récupération d’urgence
Lorsque vous élaborez vos plans de récupération d’urgence, il est important de tenir compte des besoins uniques du Service Guardian hôte dans votre infrastructure protégée.
Si vous perdez tout ou partie de vos nœuds SGH, vous risquez de rencontrer des problèmes de disponibilité immédiate qui empêchent les utilisateurs de démarrer leurs machines virtuelles protégées.
Dans un scénario où vous perdez votre cluster SGH, vous devez effectuer des sauvegardes complètes de la configuration de SGH quant à la restauration de votre cluster SGH et reprendre les opérations normales.
Cette section décrit les étapes nécessaires pour préparer pour un tel scénario.

Tout d’abord, il est important de comprendre qu’en est-il SGH est important de sauvegarder.
SGH conserve plusieurs éléments d’information qui lui permettront déterminent quels sont les hôtes sont autorisés à exécuter des machines virtuelles protégées.
Cela comprend les éléments suivants :
1. Identificateurs de sécurité actives Directory pour les groupes contenant approuvé hôtes (lorsque vous utilisez l’attestation Active Directory) ;
2. Identificateurs de module de plateforme sécurisée uniques pour chaque hôte dans votre environnement ;
3. Stratégies de module de plateforme sécurisée pour chaque configuration unique de l’hôte ; et
4. Stratégies d’intégrité du code qui déterminent quels logiciels sont autorisé à exécuter sur vos ordinateurs hôtes.

Ces artefacts de l’attestation nécessitent une coordination avec les administrateurs de votre infrastructure d’hébergement pour obtenir, potentiellement rend difficile d’obtenir des informations à nouveau après un sinistre.

En outre, SGH requiert l’accès à 2 ou plusieurs certificats utilisés pour chiffrer et signer les informations requises pour démarrer une machine virtuelle protégée (le protecteur de clé).
Ces certificats sont bien connus (utilisé par les propriétaires des machines virtuelles protégées pour autoriser votre infrastructure à exécuter leurs machines virtuelles) et doivent être restaurés après un sinistre pour une expérience transparente de récupération.
Devez vous pas restaurer SGH avec les certificats mêmes après un sinistre, chaque machine virtuelle doit être mis à jour pour autoriser vos nouvelles clés pour déchiffrer leurs informations.
Pour des raisons de sécurité, seul le propriétaire de la machine virtuelle peut mettre à jour la configuration de machine virtuelle pour autoriser ces nouvelles clés, d’un échec de signification pour restaurer vos clés après qu’un incident entraîne dans chaque propriétaire de machine virtuelle devoir prendre des mesures pour obtenir leurs machines virtuelles en cours d’exécution à nouveau.

#### <a name="preparing-for-the-worst"></a>Préparation au pire ?
Pour préparer une perte complète de SGH, il existe 2 étapes à suivre :
1. Sauvegarder les stratégies de l’attestation SGH
2. Sauvegarder les clés HGS

Obtenir des conseils sur la façon d’effectuer ces deux étapes sont fournies dans le [sauvegarde SGH](#backing-up-hgs) section.

Il est en outre recommandé, mais pas obligatoire, que vous sauvegardiez la liste des utilisateurs autorisés à gérer SGH dans son domaine Active Directory ou Active Directory proprement dit.

Les sauvegardes doivent veiller régulièrement à ce que les informations sont à jour et stockées en toute sécurité afin d’éviter toute falsification ou le vol.

Il est **déconseillé** doit être sauvegardé ou tentez de restaurer une image de l’ensemble du système d’un nœud SGH.
Dans l’événement vous avez perdu votre cluster, la meilleure pratique consiste à configurer un tout nouveau nœud SGH et restaurer simplement l’état de SGH, et pas l’ensemble du serveur du système d’exploitation.

#### <a name="recovering-from-the-loss-of-one-node"></a>Récupération de la perte d’un nœud
Si vous perdez un ou plusieurs nœuds (mais pas tous les nœuds) dans votre cluster SGH, vous pouvez simplement [ajouter des nœuds à votre cluster](guarded-fabric-configure-additional-hgs-nodes.md) suivant les instructions dans le guide de déploiement.
Les stratégies de l’attestation seront synchronisent automatiquement, ainsi que tous les certificats qui ont été fournies comme des fichiers PFX à SGH avec accompagnant les mots de passe.
Pour les certificats ajoutés à SGH à l’aide d’une empreinte numérique (non exportable et soutenu par le matériel, les certificats couramment), vous devez vérifier chaque nouveau nœud a accès à la clé privée de chaque certificat.

#### <a name="recovering-from-the-loss-of-the-entire-cluster"></a>Récupération de la perte de l’ensemble du cluster
Si votre cluster SGH tombe en panne et que vous ne parvenez pas à mettre en ligne, vous devez restaurer une sauvegarde SGH.
Restauration HGS à partir d’une sauvegarde implique la première configuration d’un nouveau cluster SGH par le [conseils dans le guide de déploiement](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).
Il est vivement recommandé, mais pas obligatoire, à utiliser le même nom de cluster lors de la configuration de l’environnement de service SGH de récupération afin de faciliter la résolution de noms à partir d’hôtes.
En utilisant le même nom évite d’avoir à reconfigurer les ordinateurs hôtes dont l’attestation de nouveau et les URL de la protection de clé.
Si vous avez restauré des objets au domaine Active Directory sauvegarde SGH, il est recommandé de supprimer les objets qui représentent les cluster SGH, ordinateurs, compte du service et les groupes JEA avant d’initialiser le serveur SGH.

Une fois que vous avez configuré votre premier nœud SGH (par exemple, il a été installé et initialisé), vous allez suivre les procédures sous [SGH de restauration à partir d’une sauvegarde](#restoring-hgs-from-a-backup) pour restaurer les stratégies d’attestation et réduit de moitié publique de la protection des clés certificats.
Vous devez restaurer les clés privées pour vos certificats manuellement en fonction de l’aide de votre fournisseur de certificat (par exemple, importer le certificat dans Windows, ou configurer l’accès aux certificats reposant sur les HSM).
Après avoir configuré le premier nœud, vous pouvez continuer à [installer des nœuds supplémentaires au cluster](guarded-fabric-configure-additional-hgs-nodes.md) jusqu'à ce que vous avez atteint la capacité et la résilience de votre choix.

### <a name="backing-up-hgs"></a>Sauvegarde SGH
L’administrateur SGH doit être chargé de sauvegarder SGH régulièrement.
Une sauvegarde complète contient les clés confidentielles doivent être correctement sécurisée.
Doit une entité non approuvée accéder à ces clés, ils peuvent utiliser material à configurer un environnement SGH malveillant à des fins de compromettre des machines virtuelles protégées.

**Sauvegarder les stratégies d’attestation** pour sauvegarder des stratégies d’attestation SGH, exécutez la commande suivante sur n’importe quel nœud du serveur SGH travail.
Vous êtes invité à fournir un mot de passe.
Ce mot de passe est utilisé pour chiffrer tous les certificats ajoutés à SGH à l’aide d’un fichier PFX (au lieu d’un empreinte numérique du certificat).

```powershell
Export-HgsServerState -Path C:\temp\HGSBackup.xml
```

> [!NOTE]
> Si vous utilisez l’attestation approuvée par l’administrateur, vous devez sauvegarder séparément les différents groupes de sécurité utilisé par SGH pour autoriser des hôtes service Guardian.
> SGH sauvegardera uniquement le SID des groupes de sécurité, pas l’appartenance au sein de celles-ci.
> En cas de ces groupes sont perdues lors d’un sinistre, vous devez recréer l’ou les groupes et ajoutez chaque hôte service Guardian leur à nouveau.

**Sauvegarder des certificats**

Le `Export-HgsServerState` commande sauvegardera les PFX certificats basés sur ajouté à SGH au moment où la commande est exécutée.
Si vous avez ajouté des certificats à SGH à l’aide d’une empreinte numérique (standard pour les certificats non exportable et reposant sur le matériel), vous devez sauvegarder manuellement les clés privées de vos certificats.
Pour identifier les certificats qui sont inscrits à SGH et doivent être sauvegardées manuellement, exécutez la commande PowerShell suivante sur n’importe quel nœud du serveur SGH travail.

```powershell
Get-HgsKeyProtectionCertificate | Where-Object { $_.CertificateData.GetType().Name -eq 'CertificateReference' } | Format-Table Thumbprint, @{ Label = 'Subject'; Expression = { $_.CertificateData.Certificate.Subject } }
```

Pour chacun des certificats répertoriés, vous devrez sauvegarder manuellement la clé privée.
Si vous utilisez un certificat basé sur logiciel qui n’est pas exportable, vous devez contacter votre autorité de certification pour s’assurer qu’ils ont une sauvegarde de votre certificat peuvent soumettre de nouveau à la demande.
Pour les certificats créés et stockés dans des modules de sécurité matériels, vous devez consulter la documentation de votre appareil pour obtenir des conseils sur la planification de la récupération d’urgence.

Vous devez stocker les sauvegardes de certificat en même temps que vos sauvegardes de la stratégie d’attestation dans un emplacement sécurisé afin que les deux parties peuvent être restaurées ensemble.

**Configuration supplémentaire pour sauvegarder**

La sauvegarde des SGH état du serveur n’inclut pas le nom de votre cluster SGH, toutes les informations à partir d’Active Directory, ou tous les certificats SSL utilisé pour sécuriser les communications avec les API SGH.
Ces paramètres sont importants pour la cohérence mais non essentielle pour obtenir votre cluster SGH en ligne après un sinistre.

Pour capturer le nom du service SGH, exécutez `Get-HgsServer` et notez le nom plat dans les URL de Protection de clé et de l’Attestation.
Par exemple, si l’URL d’Attestation est «<http://hgs.contoso.com/Attestation>», « sgh » est le nom du service SGH.

Le domaine Active Directory utilisé par SGH doit être géré comme tout autre domaine Active Directory.
Lorsque vous restaurez SGH après un sinistre, vous pas nécessairement devrez recréer les objets spécifiques qui sont présents dans le domaine actuel.
Toutefois, cela facilite la récupération si vous sauvegardez Active Directory et conservez une liste des utilisateurs JEA autorisé à gérer le système, ainsi que l’appartenance au groupe de sécurité utilisées par attestation approuvée par l’administrateur pour autoriser des hôtes service Guardian.

Pour identifier l’empreinte numérique des certificats SSL configuré pour le service SGH, exécutez la commande suivante dans PowerShell.
Vous pouvez ensuite sauvegarder ces certificats SSL en suivant les instructions du fournisseur de votre certificat.

```powershell
Get-WebBinding -Protocol https | Select-Object certificateHash
```

### <a name="restoring-hgs-from-a-backup"></a>SGH restauration à partir d’une sauvegarde
Les étapes suivantes décrivent comment restaurer des paramètres SGH à partir d’une sauvegarde.
Les étapes sont pertinentes pour les deux situations où vous essayez d’annuler les modifications apportées à vos instances de service SGH en cours d’exécution et lorsque vous créez un tout nouveau cluster SGH après une perte complète de votre précédente.

#### <a name="set-up-a-replacement-hgs-cluster"></a>Configuration d’un cluster de service SGH de remplacement
Avant de pouvoir restaurer SGH, vous devez disposez d’un cluster SGH initialisé à laquelle vous pouvez restaurer la configuration.
Si vous importez simplement les paramètres qui ont été supprimées accidentellement à un cluster (en cours d’exécution) existant, vous pouvez ignorer cette étape.
Si vous récupérez à partir d’une perte complète de SGH, vous devez installer et initialiser au moins un nœud qui suit SGH le [conseils dans le guide de déploiement](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

Plus précisément, vous devez :
1. [Configurer le domaine SGH](guarded-fabric-choose-where-to-install-hgs.md) ou joindre SGH à un domaine existant
2. [Initialiser le serveur SGH](guarded-fabric-initialize-hgs.md) à l’aide de vos clés existantes *ou* un ensemble de clés temporaires. Vous pouvez [supprimer les clés temporaires](#renewing-or-replacing-keys) après l’importation de vos clés réelles à partir de la SGH des fichiers de sauvegarde.
3. [Importer les paramètres SGH](#import-settings-from-a-backup) à partir de votre sauvegarde à restaurer les groupes hôtes approuvés, stratégies d’intégrité du code, configurations de référence de module de plateforme sécurisée et les identificateurs de module de plateforme sécurisée

> [!TIP]
> Le nouveau cluster SGH n’a pas besoin d’utiliser des certificats, de nom de service ou de domaine en tant que l’instance de service HGS à partir de laquelle votre fichier de sauvegarde a été exporté.

#### <a name="import-settings-from-a-backup"></a>Importer les paramètres d’une sauvegarde

Pour restaurer l’attestation de stratégies, les certificats PFX et les clés publiques des certificats non PFX à votre nœud HGS à partir d’un fichier de sauvegarde, exécutez la commande suivante sur un nœud du serveur SGH initialisé.
Vous êtes invité à entrer le mot de passe que vous avez spécifié lors de la création de la sauvegarde.

```powershell
Import-HgsServerState -Path C:\Temp\HGSBackup.xml
```

Si vous souhaitez uniquement importer des stratégies de l’attestation approuvée par l’administrateur ou attestation approuvée par le module de plateforme sécurisée, vous pouvez le faire en spécifiant le `-ImportActiveDirectoryModeState` ou `-ImportTpmModeState` indicateurs à [Import-HgsServerState](https://technet.microsoft.com/library/mt652168.aspx).

Vérifiez la dernière mise à jour cumulative pour Windows Server 2016 est installé avant d’exécuter `Import-HgsServerState`.
Cela peut entraîner une erreur d’importation.

> [!NOTE]
> Si vous restaurez des stratégies sur un nœud SGH existant qui a déjà un ou plusieurs de ces stratégies installés, la commande d’importation affichera une erreur pour chaque stratégie en double.
> Ce comportement est normal et peut être ignoré en toute sécurité dans la plupart des cas.

#### <a name="reinstall-private-keys-for-certificates"></a>Réinstallez les clés privées des certificats
Si un des certificats utilisés sur le service HGS à partir de laquelle la sauvegarde a été créée ont été ajouté à l’aide d’empreintes numériques, seule la clé publique de ces certificats est incluse dans le fichier de sauvegarde.
Cela signifie que vous deviez installer manuellement et/ou d’accorder l’accès aux clés privées pour chacun de ces certificats avant SGH peut traiter les demandes des hôtes Hyper-V.
Les actions nécessaires pour effectuer cette étape varie en fonction de la façon dont votre certificat a été émis.
Pour les certificats reposant sur le logiciel émis par une autorité de certification, vous devez contacter votre autorité de certification pour obtenir la clé privée et installez-le sur **chaque** nœud SGH par leurs instructions.
De même, si vos certificats sont sur le matériel, vous devez consulter la documentation de votre sécurité module du fabricant de matériel pour installer les pilotes nécessaires sur chaque nœud SGH pour vous connecter pour le module HSM et chaque ordinateur d’accéder à la clé privée.

En guise de rappel, ajoutés à SGH à l’aide d’empreintes numériques de certificats nécessitent une réplication manuelle des clés privées pour chaque nœud.
Vous devez répéter cette étape sur chaque nœud supplémentaire que vous ajoutez au cluster SGH restauré.

#### <a name="review-imported-attestation-policies"></a>Passez en revue les stratégies importées d’attestation
Une fois que vous avez importé vos paramètres à partir d’une sauvegarde, il est recommandé d’en étroite collaboration passez en revue toutes les stratégies importées à l’aide de `Get-HgsAttestationPolicy` pour vous assurer que les hôtes que vous faites confiance pour exécuter des machines virtuelles protégées seront en mesure d’attester avec succès.
Si vous trouvez des stratégies qui ne correspondent pas à votre posture de sécurité, vous pouvez [désactivez ou supprimez les](#review-attestation-policies).

#### <a name="run-diagnostics-to-check-system-state"></a>Exécutez les diagnostics pour vérifier l’état du système
Une fois que vous avez terminé la configuration et de restauration de l’état de votre nœud SGH, vous devez exécuter l’outil de diagnostic SGH pour vérifier l’état du système.
Pour ce faire, exécutez la commande suivante sur le nœud SGH où vous avez restauré la configuration :

```powershell
Get-HgsTrace -RunDiagnostics
```

Si le résultat « global » n’est pas « réussite », les étapes supplémentaires sont nécessaires pour terminer la configuration du système.
Vérifiez les messages signalés dans le subtest(s) ayant échoué pour plus d’informations.

## <a name="patching-hgs"></a>Mise à jour corrective SGH
Il est important de garder vos nœuds Service Guardian hôte à jour en installant la dernière mise à jour cumulative lorsqu’il s’agit de. Si vous configurez un tout nouveau nœud SGH, il est recommandé d’installer les mises à jour disponibles avant d’installer le rôle de SGH ou de sa configuration.
Cela garantira un nouveau ou fonctionnalité modifiée prend effet immédiatement.

Lors de la mise à jour corrective de votre infrastructure protégée, il est fortement recommandé que vous tout d’abord mettre à niveau *tous les* hôtes Hyper-V **avant la mise à niveau SGH**.
Cela consiste à s’assurer que toutes les stratégies d’attestation SGH est modifiée *après* les hôtes Hyper-V ont été mis à jour pour fournir les informations nécessaires pour eux.
Si une mise à jour va changer le comportement de stratégies, ils ne seront pas automatiquement activés pour éviter d’interrompre votre structure.
Ces mises à jour nécessitent que vous suivez les instructions dans la section suivante pour activer les stratégies de l’attestation de nouveaux ou modifiés.
Nous vous encourageons à lire les notes de publication pour Windows Server et les mises à jour cumulatives, que vous installez pour vérifier si les mises à jour de stratégie sont requis.

### <a name="updates-requiring-policy-activation"></a>Mises à jour nécessitant une activation de la stratégie
Si une mise à jour pour SGH introduit ou modifie le comportement d’une stratégie de l’attestation de manière significative, une étape supplémentaire est nécessaire pour activer la stratégie modifiée.
Modifications de stratégie sont uniquement mises en œuvre après l’exportation et importation de l’état SGH.
Vous devez uniquement activer les stratégies nouvelles ou modifiées une fois que vous avez appliqué la mise à jour cumulative pour tous les ordinateurs hôtes et tous les nœuds de SGH dans votre environnement.
Une fois que chaque machine a été mis à jour, exécutez les commandes suivantes sur n’importe quel nœud SGH pour déclencher le processus de mise à niveau :

```powershell
$password = Read-Host -AsSecureString -Prompt "Enter a temporary password"
Export-HgsServerState -Path .\temporaryExport.xml -Password $password
Import-HgsServerState -Path .\temporaryExport.xml -Password $password
```

Si une nouvelle stratégie a été introduite, il sera désactivé par défaut.
Pour activer la nouvelle stratégie, tout d’abord le trouver dans la liste des stratégies de Microsoft (précédés de « HGS_ »), puis l’activer à l’aide des commandes suivantes :

```powershell
Get-HgsAttestationPolicy

Enable-HgsAttestationPolicy -Name <Hgs_NewPolicyName>
```

## <a name="managing-attestation-policies"></a>La gestion des stratégies de l’attestation
SGH gère plusieurs stratégies d’attestation qui définissent l’ensemble minimal d’exigences qu'un ordinateur hôte doit remplir pour être considéré comme « sain » et autorisée à s’exécuter des machines virtuelles protégées.
Certaines de ces stratégies sont définies par Microsoft, d’autres utilisateurs sont ajoutés en vous permettant de définir les stratégies d’intégrité du code autorisée, les lignes de base de module de plateforme sécurisée et les hôtes dans votre environnement.
Une maintenance régulière de ces stratégies est nécessaire pour garantir hôtes peuvent continuer attestant correctement que vous mettez à jour et les remplacez, pour garantir des hôtes non approuvés ou des configurations sont bloquées à partir de l’attestation avec succès.

Pour l’attestation approuvée par l’administrateur, il n'existe qu’une seule stratégie qui détermine si un ordinateur hôte est sain : l’appartenance à un groupe de sécurité connues et approuvées.
L’attestation TPM est plus complexe et implique des différentes stratégies pour mesurer le code et la configuration d’un système avant de déterminer s’il est sain.

Un service SGH unique peut être configuré avec les stratégies Active Directory et le module de plateforme sécurisée à la fois, mais le service vérifie uniquement les stratégies pour le mode actuel qui elle est configurée pour lorsqu’un hôte essaie d’attestation.
Pour vérifier le mode de votre serveur SGH, exécutez `Get-HgsServer`.

### <a name="default-policies"></a>Stratégies par défaut
Pour l’attestation approuvée par le module de plateforme sécurisée, il existe plusieurs stratégies intégrées configurés sur SGH.
Certaines de ces stratégies sont « verrouillés », ce qui signifie qu’ils ne peut pas être désactivées pour des raisons de sécurité.
Le tableau ci-dessous explique l’objectif de chaque stratégie par défaut.

Nom de la stratégie                    | Objectif
-------------------------------|-----------------------------------------------------
Hgs_SecureBootEnabled          | Nécessite des hôtes pour que le démarrage sécurisé est activé. Cela est nécessaire pour mesurer les fichiers binaires de démarrage et d’autres paramètres de verrouillage UEFI.
Hgs_UefiDebugDisabled          | Garantit que les hôtes n’ont pas d’un débogueur du noyau est activé. Les débogueurs en mode utilisateur sont bloquées avec les stratégies d’intégrité du code.
Hgs_SecureBootSettings         | La stratégie négatif pour assurer des hôtes correspond au moins une base de module de plateforme sécurisée (définie par l’administrateur).
Hgs_CiPolicy                   | Négatif la stratégie pour assurer les hôtes sont à l’aide de l’une des stratégies d’élément de configuration définie par l’administrateur.
Hgs_HypervisorEnforcedCiPolicy | Nécessite la stratégie d’intégrité du code pour être appliqué par l’hyperviseur. La désactivation de cette stratégie réduit votre protection contre les attaques de stratégie de l’intégrité de code en mode noyau.
Hgs_FullBoot                   | Garantit que l’hôte ne pas reprendre en mode veille ou veille prolongée. Ordinateurs hôtes doivent être correctement redémarrés ou arrêtés à passer cette stratégie.
Hgs_VsmIdkPresent              | Nécessite la sécurité en fonction de la virtualisation pour être en cours d’exécution sur l’ordinateur hôte. Le Kit de déploiement Iexpress représente la clé nécessaire pour chiffrer les informations renvoyées à l’espace mémoire sécurisé de l’hôte.
Hgs_PageFileEncryptionEnabled  | Nécessite des fichiers d’échange soient chiffrés sur l’ordinateur hôte. La désactivation de cette stratégie peut entraîner l’exposition des informations si un fichier d’échange non chiffré est inspectée pour les clés secrètes de client.
Hgs_BitLockerEnabled           | Nécessite BitLocker doit être activée sur l’hôte Hyper-V. Cette stratégie est désactivée par défaut pour des raisons de performances et n’est pas recommandée doit être activé. Cette stratégie n’a aucune incidence sur le chiffrement des machines virtuelles protégées eux-mêmes.
Hgs_IommuEnabled               | Nécessite que l’hôte dispose d’un appareil IOMMU en cours d’utilisation pour empêcher des attaques d’accès direct à la mémoire. La désactivation de cette stratégie et l’utilisation des ordinateurs hôtes sans un IOMMU activé peuvent exposer les secrets de machine virtuelle de locataire pour diriger des attaques de mémoire.
Hgs_NoHibernation              | Nécessite la mise en veille prolongée doit être désactivée sur l’ordinateur hôte Hyper-V. La désactivation de cette stratégie pourrait permettre des hôtes enregistrer la mémoire de machine virtuelle protégée dans un fichier de mise en veille prolongée non chiffré.
Hgs_NoDumps                    | Nécessite les vidages de mémoire doit être désactivée sur l’ordinateur hôte Hyper-V. Si vous désactivez cette stratégie, il est recommandé de configurer le chiffrement de vidage pour empêcher la mémoire de machine virtuelle protégée ne sont pas enregistrées pour les fichiers de vidage sur incident non chiffré.
Hgs_DumpEncryption             | Nécessite les vidages de mémoire, si activé sur l’hôte Hyper-V, est chiffré avec une clé de chiffrement approuvée par SGH. Cette stratégie ne s’applique pas si les images ne sont pas activés sur l’ordinateur hôte. Si cette stratégie et *Sgh\_NoDumps* sont toutes les deux désactivées, mémoire de machine virtuelle protégée pu être enregistré dans un fichier de vidage non chiffré.
Hgs_DumpEncryptionKey          | Stratégie de négative pour assurer les hôtes configurés pour autoriser des dumps utilisent une clé de chiffrement de fichier de vidage définie par l’administrateur SGH connue de mémoire. Cette stratégie ne s’applique pas quand *Sgh\_DumpEncryption* est désactivé.

### <a name="authorizing-new-guarded-hosts"></a>Autorisation de nouveau des hôtes service Guardian
Pour autoriser un nouvel hôte à hôte service Guardian (par exemple, attester avec succès), SGH doit approuver l’ordinateur hôte et (quand il est configuré pour utiliser l’attestation approuvée par le module de plateforme sécurisée) le logiciel en cours d’exécution sur ce dernier.
Les étapes pour autoriser un nouvel hôte diffèrent selon le mode d’attestation pour lequel SGH est configuré actuellement.
Pour vérifier le mode d’attestation pour votre infrastructure protégée, exécutez `Get-HgsServer` sur n’importe quel nœud SGH.

#### <a name="software-configuration"></a>Configuration logicielle
Sur le nouvel hôte Hyper-V, assurez-vous que Windows Server 2016 Datacenter edition est installée.
Windows Server 2016 Standard ne peut pas exécuter des machines virtuelles protégées dans une infrastructure protégée.
L’hôte peut être installé expérience ou Server Core.

Sur le serveur avec expérience utilisateur et Server Core, vous devez installer les rôles de serveur Hyper-V et de Support de Hyper-V Guardian hôte :

```powershell
Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
```

#### <a name="admin-trusted-attestation"></a>Attestation approuvée par l’administrateur
Pour inscrire un nouvel hôte dans SGH lors de l’utilisation de l’attestation approuvée par l’administrateur, vous devez d’abord ajouter l’hôte à un groupe de sécurité dans le domaine auquel il est joint.
En règle générale, chaque domaine auront un groupe de sécurité pour les hôtes service Guardian.
Si vous avez déjà inscrit ce groupe avec SGH, la seule action que vous devez effectuer consiste à redémarrer l’ordinateur hôte pour actualiser son appartenance au groupe.

Vous pouvez vérifier quels groupes de sécurité sont approuvés par SGH en exécutant la commande suivante :

```powershell
Get-HgsAttestationHostGroup
```

Pour inscrire un nouveau groupe de sécurité avec SGH, tout d’abord de capturer l’identificateur de sécurité (SID) du groupe dans le domaine de l’hôte et d’inscrire le SID avec SGH.

```powershell
Add-HgsAttestationHostGroup -Name "Contoso Guarded Hosts" -Identifier "S-1-5-21-3623811015-3361044348-30300820-1013"
```

Obtenir des instructions sur la configuration de l’approbation entre le domaine de l’hôte et le SGH sont disponibles dans le guide de déploiement.

#### <a name="tpm-trusted-attestation"></a>Attestation approuvée par le module de plateforme sécurisée
Quand SGH est configuré en mode de module de plateforme sécurisée, les hôtes doivent passer toutes les stratégies de « activé » le préfixe « Hgs_ », mais aussi au moins une ligne de base de module de plateforme sécurisée, identificateur de module de plateforme sécurisée et stratégie d’intégrité du code et les stratégies verrouillés.
Chaque fois que vous ajoutez un nouvel hôte, vous devez inscrire le nouvel identificateur de module de plateforme sécurisée avec SGH.
Tant que l’hôte exécute les mêmes logiciels (et a la même stratégie d’intégrité de code appliqué) et de la ligne de base du module de plateforme sécurisée en tant qu’un autre hôte dans votre environnement, vous ne devrez pas ajouter de nouvelles stratégies de l’élément de configuration ou des lignes de base de.

**Ajout de l’identificateur de module de plateforme sécurisée pour un nouvel hôte** sur le nouvel hôte, exécutez la commande suivante pour capturer l’identificateur de module de plateforme sécurisée.
Veillez à spécifier un nom unique pour l’hôte qui vous aideront à le chercher sur SGH.
Ces informations seront nécessaires si vous retirez de l’ordinateur hôte ou que vous souhaitez empêcher d’en cours d’exécution des machines virtuelles protégées dans SGH.

```powershell
(Get-PlatformIdentifier -Name "Host01").InnerXml | Out-File C:\temp\host01.xml -Encoding UTF8
```

Copiez ce fichier à votre serveur SGH, puis exécutez la commande suivante pour inscrire l’hôte avec SGH.

```powershell
Add-HgsAttestationTpmHost -Name 'Host01' -Path C:\temp\host01.xml
```

**Ajout d’une nouvelle ligne de base du module de plateforme sécurisée** si le nouvel ordinateur hôte exécute un nouveau matériel ou la configuration du microprogramme pour votre environnement, vous devrez peut-être prendre une nouvelle ligne de base du module de plateforme sécurisée.
Pour ce faire, exécutez la commande suivante sur l’ordinateur hôte.

```powershell
Get-HgsAttestationBaselinePolicy -Path 'C:\temp\hardwareConfig01.tcglog'
```

> [!NOTE]
> Si vous recevez une erreur indiquant que votre hôte de validation a échoué et ne sera pas correctement attester, ne vous inquiétez pas.
> Ceci est une vérification des prérequis pour vous assurer que votre hôte peut exécuter des machines virtuelles protégées et probablement signifie que vous n'avez pas encore appliqué une stratégie d’intégrité du code ou un autre paramètre requis.
> Lire le message d’erreur, apportez les modifications suggérées par celui-ci, puis réessayez.
> Ou bien, vous pouvez ignorer la validation pour l’instant en ajoutant le `-SkipValidation` indicateur à la commande.

Copiez la ligne de base du module de plateforme sécurisée à votre serveur SGH, puis inscrivez-le avec la commande suivante.
Nous vous encourageons à utiliser une convention d’affectation de noms qui vous aidera à comprendre la configuration matérielle et du microprogramme de cette classe de l’hôte Hyper-V.

```powershell
Add-HgsAttestationTpmPolicy -Name 'HardwareConfig01' -Path 'C:\temp\hardwareConfig01.tcglog'
```

**Ajouter une nouvelle stratégie d’intégrité de code** si vous avez modifié la stratégie d’intégrité du code en cours d’exécution sur vos ordinateurs hôtes Hyper-V, vous devez inscrire la nouvelle stratégie avec SGH avant que ces ordinateurs hôtes peuvent correctement attester.
Sur un hôte de référence, qui sert d’une image principale pour les machines Hyper-V approuvés dans votre environnement, capturer une nouvelle stratégie CI à l’aide du `New-CIPolicy` commande.
Nous vous encourageons à utiliser le **FilePublisher** niveau et **hachage** secours pour les stratégies d’élément de configuration d’hôte Hyper-V.
Vous devez tout d’abord créer une stratégie d’intégrité en mode audit pour vous assurer que tout fonctionne comme prévu.
Après avoir validé une charge de travail d’exemple sur le système, vous pouvez appliquer la stratégie et copiez la version imposée à SGH.
Pour obtenir une liste complète des options de configuration de stratégie de l’intégrité du code, consultez le [documentation de Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies).

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

Une fois que vous avez votre stratégie créée, testées et appliquées, copiez le fichier binaire (.p7b) sur votre serveur SGH et enregistrer la stratégie.

```powershell
Add-HgsAttestationCiPolicy -Name 'WS2016-Hardware01' -Path 'C:\temp\ws2016-hardware01-ci.p7b'
```

**Ajout d’une clé de chiffrement de vidage de mémoire**

Lorsque le *Sgh\_NoDumps* stratégie est désactivée et *Sgh\_DumpEncryption* stratégie est activée, les hôtes service Guardian sont autorisés à avoir des vidages de mémoire (y compris les vidages sur incident) pour être activé tant que ces images mémoire sont chiffrées. Hôtes service Guardian passera uniquement l’attestation s’ils ont les vidages de mémoire désactivés ou sont le chiffrant avec une clé connue à SGH. Par défaut, aucune clé de chiffrement de vidage n’est configurés sur SGH.

Pour ajouter une clé de chiffrement de vidage à SGH, utilisez le `Add-HgsAttestationDumpPolicy` fournir SGH avec le hachage de votre clé de chiffrement de vidage de l’applet de commande.
Si vous capturez une ligne de base du module de plateforme sécurisée sur un hôte Hyper-V configuré avec le chiffrement d’image mémoire, le hachage est inclus dans le tcglog et peut être fourni pour le `Add-HgsAttestationDumpPolicy` applet de commande.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey01' -Path 'C:\temp\TpmBaselineWithDumpEncryptionKey.tcglog'
```

Vous pouvez également fournir directement la représentation sous forme de chaîne de hachage à l’applet de commande.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey02' -PublicKeyHash '<paste your hash here>'
```

Veillez à ajouter chaque clé de chiffrement unique vidage à SGH si vous choisissez d’utiliser des clés différentes entre votre infrastructure protégée.
Les hôtes qui cryptez les vidages de mémoire avec une clé ne pas connue à SGH ne passera pas d’attestation.

Consultez la documentation de Hyper-V pour plus d’informations sur [configuration de vider le chiffrement sur les ordinateurs hôtes](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).

#### <a name="check-if-the-system-passed-attestation"></a>Vérifiez si le système passé une attestation
Après avoir inscrit les informations nécessaires avec SGH, vous devez vérifier si l’hôte obtient une attestation.
Sur l’hôte Hyper-V qui vient d’être ajouté, exécutez `Set-HgsClientConfiguration` et fournir les URL correctes pour votre cluster SGH.
Ces URL peut être obtenu en exécutant `Get-HgsServer` sur n’importe quel nœud SGH.

```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'http://hgs.bastion.local/KeyProtection' -AttestationServerUrl 'http://hgs.bastion.local/Attestation'
```

Si l’état résultant n’indique pas « IsHostGuarded : Valeur True » vous devez résoudre les problèmes de la configuration.
Sur l’hôte que l’attestation a échoué, exécutez la commande suivante pour obtenir un rapport détaillé sur les problèmes qui peuvent vous aider à résoudre l’attestation a échoué.

```powershell
Get-HgsTrace -RunDiagnostics -Detailed
```

> [!IMPORTANT]
> Si vous utilisez Windows Server 2019 ou Windows 10, version 1809 et sont à l’aide de stratégies d’intégrité du code, `Get-HgsTrace` peut retourner un échec pour le **Code intégrité stratégie Active** diagnostic.
> Vous pouvez ignorer ce résultat lorsqu’il est l’uniquement Échec de diagnostic.

### <a name="review-attestation-policies"></a>Passez en revue les stratégies de l’attestation
Pour consulter l’état actuel des stratégies configurées sur SGH, exécutez les commandes suivantes sur n’importe quel nœud SGH :

```powershell
# List all trusted security groups for admin-trusted attestation
Get-HgsAttestationHostGroup

# List all policies configured for TPM-trusted attestation
Get-HgsAttestationPolicy
```

Si vous trouvez une stratégie est activée, ce qui ne répondait plus à vos besoins de sécurité (par exemple, une ancien code stratégie d’intégrité qui est désormais considéré comme non sécurisée), vous pouvez le désactiver en remplaçant le nom de la stratégie dans la commande suivante :

```powershell
Disable-HgsAttestationPolicy -Name 'PolicyName'
```

De même, vous pouvez utiliser `Enable-HgsAttestationPolicy` pour réactiver une stratégie.

Si vous n’avez plus besoin une stratégie et qui souhaitent utiliser pour le supprimer de tous les nœuds de SGH, exécutez `Remove-HgsAttestationPolicy -Name 'PolicyName'` pour supprimer définitivement la stratégie.

## <a name="changing-attestation-modes"></a>Modification des modes d’attestation
Si vous avez démarré votre infrastructure protégée à l’aide de l’attestation approuvée par l’administrateur, vous pouvez mettre à niveau vers le mode d’attestation TPM plus fort de très dès que vous avez suffisamment hôtes 2.0 compatible TPM dans votre environnement.
Lorsque vous êtes prêt à basculer, vous pouvez précharger tous les artefacts d’attestation (élément de configuration des stratégies, lignes de base de module de plateforme sécurisée et identificateurs de module de plateforme sécurisée) dans SGH tout en continuant à exécuter SGH avec attestation approuvée par l’administrateur.
Pour ce faire, il suffit de suivre les instructions de la [autorisant un nouvel hôte service Guardian](#authorizing-new-guarded-hosts) section.

Une fois que vous avez ajouté toutes vos stratégies à SGH, l’étape suivante consiste à exécuter une tentative d’attestation synthétique sur vos hôtes pour voir si elles transmettriez attestation en mode de module de plateforme sécurisée.
Cela n’affecte pas l’état opérationnel actuel de SGH.
Les commandes ci-dessous doivent être exécutées sur un ordinateur qui a accès à tous les hôtes dans l’environnement et au moins un nœud SGH.
Si votre pare-feu ou autres stratégies de sécurité éviter ce problème, vous pouvez ignorer cette étape.
Dans la mesure du possible, nous vous recommandons de l’attestation synthétique afin de vous donner une bonne indication de si « marche » au mode de module de plateforme sécurisée entraîne temps d’arrêt pour vos machines virtuelles en cours d’exécution. 

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

Après les tests de diagnostic terminée, passez en revue les informations de ces résultats pour déterminer si tous les hôtes aurait échoué attestation en mode de module de plateforme sécurisée.
Exécutez les diagnostics de nouveau jusqu'à ce que vous obtenez un « pass » à partir de chaque hôte, puis procéder à la modification SGH au mode de module de plateforme sécurisée.

**Passage au mode de module de plateforme sécurisée** prend quelques instants pour terminer.
Exécutez la commande suivante sur n’importe quel nœud SGH pour mettre à jour le mode d’attestation.

```powershell
Set-HgsServer -TrustTpm
```

Si vous rencontrez des problèmes et devez revenir au mode d’Active Directory, vous pouvez le faire en exécutant `Set-HgsServer -TrustActiveDirectory`.

Une fois que vous avez confirmé que tout fonctionne comme prévu, vous devez supprimer tous les groupes hôtes de Active Directory approuvés de SGH et supprimer l’approbation entre les domaines SGH et fabric.
Si vous laissez l’approbation Active Directory en place, vous risquez de quelqu'un réactiver l’approbation et le basculement SGH en mode Active Directory, ce qui pourrait permettre l’exécution désactivée sur vos hôtes service Guardian du code non approuvé.

## <a name="key-management"></a>Gestion des clés
La solution d’infrastructure protégée utilise plusieurs paires de clés publique/privée pour valider l’intégrité des différents composants de la solution et chiffrer les secrets des locataires.
Le Service Guardian hôte est configuré au moins deux certificats (avec les clés publiques et privées), qui sont utilisés pour signer et chiffrer les clés utilisées pour démarrer des machines virtuelles protégées.
Ces clés doivent être gérées soigneusement.
Si la clé privée est acquis par un adversaire, ils pourrez unshield toutes les machines virtuelles en cours d’exécution sur votre structure ou de configurer un cluster de service SGH imposteur qui utilise des stratégies de l’attestation de contourner les protections de que vous mettre en place.
Doit perdre les clés privées lors d’un sinistre et introuvable dans une sauvegarde, vous devrez de définir une nouvelle paire de clés de chaque machine virtuelle nouveau indexé pour autoriser les nouveaux certificats.

Cette section traite des rubriques générales de gestion de clés pour vous aider à configurer vos clés, afin qu’ils soient fonctionnelle et sécurisée.

### <a name="adding-new-keys"></a>Ajout de nouvelles clés
Bien que SGH doit être initialisé avec un jeu de clés, vous pouvez ajouter plus d’un chiffrement et la clé de signature à SGH.
Les deux principales raisons pourquoi vous devez ajouter les nouvelles clés à SGH sont :
1. Pour prendre en charge « apportez votre propre clé » les scénarios où les locataires copier leurs clés privées dans votre module de sécurité matériel et autorisent uniquement leurs clés pour démarrent leurs machines virtuelles protégées.
2. Pour remplacer les clés existantes pour SGH en ajoutant d’abord les nouvelles clés et conserver les deux jeux de clés jusqu'à ce que chaque machine virtuelle configuration a été mis à jour pour utiliser les nouvelles clés.

Le processus pour ajouter vos nouvelles clés diffère en fonction du type de certificat que vous utilisez.

**Option 1 : Ajout d’un certificat stocké dans un module HSM**

Notre approche recommandée pour la sécurisation des clés de SGH consiste à utiliser des certificats créés dans un module de sécurité matériel (HSM).
Modules de sécurité matériels Vérifiez l’utilisation de vos clés est liée à l’accès physique à un appareil de respect de la sécurité dans votre centre de données.
Chaque module de sécurité matériel est différent et dispose d’un processus unique pour créer des certificats et les inscrire avec SGH.
Les étapes ci-dessous sont conçues pour fournir l’aide approximative pour l’utilisation de HSM soutenu de certificats.
La documentation du fabricant de votre HSM pour les fonctionnalités et les étapes exactes à suivre.

1. Installez le logiciel HSM sur chaque nœud de SGH dans votre cluster. Selon que vous avez un réseau ou un appareil HSM local, vous devrez peut-être configurer le module HSM pour accorder l’accès de votre ordinateur à son magasin de clés.
2. Créer 2 certificats dans le module HSM avec **clés RSA de 2 048 bits** pour le chiffrement et signature
    1. Créer un certificat de chiffrement avec le **chiffrement des données** la clé de propriété d’utilisation dans votre module HSM
    2. Créer un certificat de signature avec la **Signature numérique** la clé de propriété d’utilisation dans votre module HSM
3. Installer les certificats dans le magasin de certificats local de chaque nœud SGH selon les recommandations du votre fournisseur HSM.
4. Si votre module HSM utilise des autorisations granulaires pour autoriser des applications ou des utilisateurs à utiliser la clé privée, vous devrez accorder votre compte service SGH géré de groupe accès au certificat. Vous trouverez le nom du compte de service administré de groupe SGH en exécutant `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
5. Ajoutez les certificats de signature et le chiffrement à SGH en remplaçant les empreintes numériques avec ceux de vos certificats dans les commandes suivantes :

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA"
    ```

**Option 2 : Ajout de certificats logiciels non exportable**

Si vous disposez d’un certificat reposant sur le logiciel émis par votre société ou une autorité de certification publique qui a une clé privée non exportable, vous devez ajouter votre certificat à SGH à l’aide de son empreinte numérique.
1. Installez le certificat sur votre ordinateur en suivant les instructions de votre autorité de certification.
2. Accordez le SGH géré de groupe service compte en lecture seule pour la clé privée du certificat. Vous trouverez le nom du compte de service administré de groupe SGH en exécutant `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
3. Inscrire le certificat avec SGH à l’aide de la commande suivante et en entrant dans l’empreinte numérique de votre certificat (modifier *chiffrement* à *signature* pour les certificats de signature) :

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    ```

> [!IMPORTANT]
> Vous devez installer la clé privée et accorder l’accès en lecture au compte de service administré de groupe sur chaque nœud SGH manuellement.
> SGH ne peut pas répliquer automatiquement les clés privées pour *n’importe quel* certificat enregistré par son empreinte numérique.

**Option 3 : Ajout de certificats stockés dans des fichiers PFX**

Si vous avez un certificat reposant sur le logiciel avec une clé privée exportable qui peut être stockée dans le format de fichier PFX et sécurisé avec un mot de passe, SGH peut gérer automatiquement vos certificats pour vous.
Certificats ajoutés avec les fichiers PFX sont automatiquement répliquées vers tous les nœuds de votre cluster SGH et SGH sécurise l’accès aux clés privées.
Pour ajouter un nouveau certificat à l’aide d’un fichier PFX, exécutez les commandes suivantes sur n’importe quel nœud SGH (modifier *chiffrement* à *signature* pour les certificats de signature) :

```powershell
$certPassword = Read-Host -AsSecureString -Prompt "Provide the PFX file password"
Add-HgsKeyProtectionCertificate -CertificateType Encryption -CertificatePath "C:\temp\encryptionCert.pfx" -CertificatePassword $certPassword
```

**Identifiant et en modifiant les certificats primaires** tandis que SGH peut prendre en charge plusieurs certificats de signature et le chiffrement, il utilise une paire en tant que ses certificats « principales ».
Il s’agit des certificats qui seront utilisées si un utilisateur télécharge les métadonnées pour ce cluster SGH.
Pour vérifier les certificats qui sont actuellement marquées comme vos certificats principales, exécutez la commande suivante :

```powershell
Get-HgsKeyProtectionCertificate -IsPrimary $true
```

Pour définir un nouveau chiffrement principal ou un certificat de signature, trouver l’empreinte numérique du certificat souhaité et la marquer comme primaire à l’aide des commandes suivantes :

```powershell
Get-HgsKeyProtectionCertificate
Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899" -IsPrimary
Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA" -IsPrimary
```

### <a name="renewing-or-replacing-keys"></a>Renouveler ou remplacer des clés
Lorsque vous créez les certificats utilisés par SGH, les certificats seront affectées à une date d’expiration en fonction de la stratégie de votre autorité de certification et les informations de votre demande.
En règle générale, dans les scénarios où la validité du certificat est importante, comme la sécurisation des communications HTTP, les certificats doivent être renouvelés avant leur expiration pour éviter une interruption de service ou d’un message d’erreur préoccupant.
SGH n’utilise pas de certificats dans ce sens.
SGH est simplement l’utilisation de certificats comme un moyen pratique pour créer et stocker une paire de clés asymétriques.
Un chiffrement a expiré ou le certificat de signature sur SGH n’indique pas une faiblesse ou une perte de la protection des machines virtuelles protégées.
En outre, les vérifications de révocation de certificat ne sont pas effectuées par SGH.
Si un certificat de service SGH ou l’autorité de certification émettrice est révoquée, il n’affecte pas l’utilisation SHG du certificat.

Le seul moment où que vous avez besoin à vous soucier d’un certificat de service SGH est le si vous avez des raisons de croire que sa clé privée a été volée.
Dans ce cas, l’intégrité de vos machines virtuelles protégées est menacée, car la possession de la moitié privée de chiffrement SGH et paire de clés de signature est suffisant pour supprimer les protections de protection sur une machine virtuelle ou de mettre en place un serveur SGH factice qui a des stratégies d’attestation.

Si vous trouvez dans cette situation, ou sont requis par les normes de conformité pour actualiser régulièrement les clés de certificat, les étapes suivantes décrivent le processus pour modifier les clés sur un serveur SGH.
Veuillez noter que les conseils suivants représentent une entreprise significative qui entraîne une interruption du service à chaque machine virtuelle pris en charge par le cluster SGH.
Une planification appropriée pour la modification des clés de SGH est nécessaire pour réduire les interruptions de service et garantir la sécurité des machines virtuelles du locataire.

Sur un nœud SGH, procédez comme suit pour inscrire une nouvelle paire de chiffrement et les certificats de signature.
Consultez la section sur [Ajout de nouvelles clés](#adding-new-keys) pour des informations détaillées les différentes façons pour ajouter de nouvelles clés à SGH.
1. Créer une nouvelle paire de chiffrement et les certificats de signature pour votre serveur SGH. Dans l’idéal, il seront créées dans un module de sécurité matériel.
2. Inscrire le nouveau chiffrement et la signature des certificats avec **HgsKeyProtectionCertificate-ajouter**

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>
    ```
3. Si vous avez utilisé des empreintes numériques, vous devez accéder à chaque nœud du cluster pour installer la clé privée et accorder l’accès de service administré de groupe de SGH à la clé.
4. Rendre les nouveaux certificats par défaut dans SGH

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsPrimary
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsPrimary
    ```

À ce stade, les données créées avec les métadonnées obtenues à partir du nœud de service SGH de protection utilisera les nouveaux certificats, mais les machines virtuelles existantes continueront de fonctionner, car les anciens certificats sont toujours disponibles.
Afin de garantir que toutes les machines virtuelles existantes fonctionneront avec les nouvelles clés, vous devez mettre à jour le protecteur de clé sur chaque machine virtuelle.
Il s’agit d’une action qui nécessite que le propriétaire de la machine virtuelle (personne ou entité en possession de la surveillance de « propriétaire ») soit impliqué.
Pour chaque machine virtuelle protégée, procédez comme suit :
5. Arrêtez la machine virtuelle. La machine virtuelle ne peut pas être désactivée jusqu'à ce que les autres étapes sont terminées, sans quoi vous devrez recommencer le processus à nouveau.
6. Enregistrer le protecteur de clé actuels dans un fichier : `Get-VMKeyProtector -VMName 'VM001' | Out-File '.\VM001.kp'`
7. Transférer le KP au propriétaire de la machine virtuelle
8. Avoir le téléchargement de propriétaire les informations de mise à jour guardian de SGH et importez-le sur son système local
9. Lire le KP actuelle en mémoire, la nouvelle guardian autorisés à accéder la KP et enregistrez-le dans un nouveau fichier en exécutant les commandes suivantes :

    ```powershell
    $kpraw = Get-Content -Path .\VM001.kp
    $kp = ConvertTo-HgsKeyProtector -Bytes $kpraw
    $newGuardian = Get-HgsGuardian -Name 'UpdatedHgsGuardian'
    $updatedKP = Grant-HgsKeyProtectorAccess -KeyProtector $kp -Guardian $newGuardian
    $updatedKP.RawData | Out-File .\updatedVM001.kp
    ```
10. Copie le KP mis à jour à l’infrastructure d’hébergement
11. Appliquer le KP à la machine virtuelle d’origine :

   ```powershell
   $updatedKP = Get-Content -Path .\updatedVM001.kp
   Set-VMKeyProtector -VMName VM001 -KeyProtector $updatedKP
   ```
12. Enfin, démarrez la machine virtuelle et vous assurer qu’il s’exécute avec succès.

> [!NOTE]
> Si le propriétaire de la machine virtuelle définit un protecteur de clé incorrect sur la machine virtuelle et n’autorise pas votre infrastructure à exécuter la machine virtuelle, il se peut que vous ne pourrez pas démarrer la machine virtuelle protégée.
> Pour revenir à la dernière protecteur de clé bonne connu, exécutez `Set-VMKeyProtector -RestoreLastKnownGoodKeyProtector`

Une fois que toutes les machines virtuelles ont été mis à jour pour autoriser les nouvelles clés guardian, vous pouvez désactiver et supprimer les anciennes clés.

13. Obtenir les empreintes numériques des certificats à partir d’anciennes `Get-HgsKeyProtectionCertificate -IsPrimary $false`

14. Désactiver chaque certificat en exécutant les commandes suivantes :  

   ```powershell
   Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsEnabled $false
   Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsEnabled $false
   ```

15. Après avoir vérifié que les machines virtuelles sont toujours en mesure de commencer par les certificats désactivés, supprimez les certificats SGH en exécutant les commandes suivantes :

   ```powershell
   Remove-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>`
   Remove-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>`
   ```

> [!IMPORTANT]
> Sauvegardes de machines virtuelles contiendra les anciennes informations de protecteur de clé qui permettent les anciens certificats à utiliser pour démarrer la machine virtuelle.
> Si vous savez que votre clé privée a été compromise, vous devez supposer que les sauvegardes de machines virtuelles peuvent être compromis, trop et agir en conséquence.
> Destruction de la configuration de machine virtuelle à partir des sauvegardes (.vmcx) supprimera les protecteurs de clé, au prix d’avoir besoin d’utiliser le mot de passe de récupération BitLocker pour démarrer la machine virtuelle la prochaine fois.

### <a name="key-replication-between-nodes"></a>Réplication des clés entre les nœuds
Chaque nœud du cluster SGH doit être configuré avec le même chiffrement, signature et (quand il est configuré) certificats SSL.
Cela est nécessaire pour garantir les hôtes Hyper-V avec n’importe quel nœud du cluster peuvent avoir leurs demandes traitées avec succès.

**Si vous avez initialisées serveur SGH avec les certificats PFX** puis SGH répliquera automatiquement la clé publique et privée de ces certificats sur tous les nœuds du cluster.
Vous devez uniquement ajouter les clés sur un nœud.

**Si vous avez initialisé serveur SGH avec les références du certificat** ou les empreintes numériques, puis SGH répliquera uniquement le *public* clé dans le certificat à chaque nœud.
En outre, SGH ne peut pas accorder lui-même l’accès à la clé privée sur n’importe quel nœud dans ce scénario.
Par conséquent, il vous incombe de :
1. Installez la clé privée sur chaque nœud SGH
2. Accorder au compte (gMSA) de service de groupe géré SGH accès à la clé privée sur chaque nœud que ces tâches ajoutent une charge supplémentaire opérationnelle, mais ils sont requis pour les clés HSM et des certificats avec clés privées non exportable.

**Certificats SSL** ne sont jamais répliquées dans n’importe quelle forme.
Il vous incombe initialiser chaque serveur SGH avec le même certificat SSL et mettre à jour de chaque serveur chaque fois que vous souhaitez renouveler ou remplacer le certificat SSL.
Lors du remplacement du certificat SSL, il est recommandé de procéder à l’aide de la [Set-HgsServer](https://technet.microsoft.com/library/mt652180.aspx) applet de commande.

## <a name="unconfiguring-hgs"></a>Annulation de la configuration de SGH

Si vous avez besoin mettre hors service ou considérablement reconfigurer un serveur SGH, vous pouvez le faire avec le [Clear-HgsServer](https://technet.microsoft.com/library/mt652176.aspx) ou [HgsServer de désinstallation](https://technet.microsoft.com/library/mt652182.aspx) applets de commande.

### <a name="clearing-the-hgs-configuration"></a>Effacer la configuration de SGH

Pour supprimer un nœud du cluster SGH, utilisez le [Clear-HgsServer](https://technet.microsoft.com/library/mt652176.aspx) applet de commande.
Cette applet de commande effectue les modifications suivantes sur le serveur où il est exécuté :

- Annule l’inscription des services de protection d’attestation et de clé
- Supprime le point de terminaison « microsoft.windows.hgs » JEA gestion
- Supprime l’ordinateur local à partir du cluster de basculement SGH

Si le serveur est le dernier nœud SGH dans le cluster, le cluster et ses ressources de nom de réseau distribué correspondant seront détruits.

```powershell
# Removes the local computer from the HGS cluster
Clear-HgsServer
```

Une fois l’opération d’effacement terminé, le serveur SGH peut être réinitialisé avec [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx).
Si vous avez utilisé [Install-HgsServer](https://technet.microsoft.com/library/mt652169.aspx) pour configurer un domaine Active Directory Domain Services, ce domaine reste configurée et opérationnelle après l’opération d’effacement.

### <a name="uninstalling-hgs"></a>Désinstallation de SGH

Si vous souhaitez supprimer un nœud du cluster SGH **et** rétrograder le contrôleur de domaine Active Directory en cours d’exécution sur ce dernier, utilisez le [HgsServer de désinstallation](https://technet.microsoft.com/library/mt652182.aspx) applet de commande.
Cette applet de commande effectue les modifications suivantes sur le serveur où il est exécuté :

- Annule l’inscription des services de protection d’attestation et de clé
- Supprime le point de terminaison « microsoft.windows.hgs » JEA gestion
- Supprime l’ordinateur local à partir du cluster de basculement SGH
- Rétrograde le contrôleur de domaine Active Directory, si configuré

Si le serveur est le dernier nœud SGH dans le cluster, le domaine, le cluster de basculement, et la ressource du cluster nom du réseau distribué sera détruite.

```powershell
# Removes the local computer from the HGS cluster and demotes the ADDC (restart required)
$newLocalAdminPassword = Read-Host -AsSecureString -Prompt "Enter a new password for the local administrator account"
Uninstall-HgsServer -LocalAdministratorPassword $newLocalAdminPassword -Restart
```

Une fois l’opération de désinstallation est terminée et que l’ordinateur a été redémarré, vous pouvez réinstaller le contrôleur et à l’aide de SGH [Install-HgsServer](https://technet.microsoft.com/library/mt652169.aspx) ou joindre l’ordinateur à un domaine et initialiser le serveur SGH dans ce domaine avec [ Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx).

Si vous souhaitez n’est plus utiliser l’ordinateur comme un nœud SGH, vous pouvez supprimer le rôle à partir de Windows.

```powershell
Uninstall-WindowsFeature HostGuardianServiceRole
```
