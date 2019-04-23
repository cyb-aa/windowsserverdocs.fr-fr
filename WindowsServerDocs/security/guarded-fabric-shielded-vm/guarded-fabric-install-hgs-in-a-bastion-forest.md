---
title: Installer le service SGH dans une forêt bastion existante
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 147610d9dcb36dfedab3aca11ee1a64731715519
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842220"
---
# <a name="install-hgs-in-an-existing-bastion-forest"></a>Installer le service SGH dans une forêt bastion existante 

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016


## <a name="join-the-hgs-server-to-the-existing-domain"></a>Joindre le serveur SGH au domaine existant

Dans une forêt bastion existante, SGH doit être ajouté au domaine racine. Utilisez le Gestionnaire de serveur ou [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) pour joindre votre serveur SGH pour le domaine racine.

## <a name="add-the-hgs-server-role"></a>Ajouter le rôle de serveur SGH

Dans une session PowerShell avec élévation de privilèges, exécutez toutes les commandes dans cette rubrique.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

Si votre centre de données possède une forêt bastion sécurisé où vous souhaitez joindre des nœuds de SGH, procédez comme suit.
Vous pouvez également utiliser ces étapes pour configurer des clusters SGH 2 ou plus indépendantes qui sont joints au même domaine.

## <a name="join-the-hgs-server-to-the-existing-domain"></a>Joindre le serveur SGH au domaine existant

Utilisez le Gestionnaire de serveur ou [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) pour joindre les serveurs de SGH pour le domaine souhaité.

## <a name="prepare-active-directory-objects"></a>Préparer des objets Active Directory

Créer un compte de service administré de groupe et de 2 groupes de sécurité.
Vous pouvez également préconfigurer les objets de cluster si le compte de que service SGH avec sont en cours d’initialisation n’a pas autorisé à créer des objets ordinateur dans le domaine.

## <a name="group-managed-service-account"></a>Compte de service administré de groupe

Le compte de service administré de groupe (gMSA) est l’identité utilisée par SGH pour extraire et utiliser ses certificats. Utilisez [New-ADServiceAccount](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adserviceaccount) pour créer un compte gMSA.
Si c’est le premier service administré de groupe dans le domaine, vous devez ajouter une clé de racine de Service de Distribution de clés.

Chaque nœud SGH devez être autorisé à accéder au mot de passe de compte gMSA.
Pour configurer ce paramètre le plus simple consiste à créer un groupe de sécurité qui contient tous vos nœuds SGH et accorder cet accès de groupe de sécurité pour récupérer le mot de passe du service administré de groupe.

Vous devez redémarrer votre serveur SGH après l’avoir ajoutée à un groupe de sécurité pour vous assurer qu’il obtient sa nouvelle appartenance de groupe.

```powershell
# Check if the KDS root key has been set up
if (-not (Get-KdsRootKey)) {
    # Adds a KDS root key effective immediately (ignores normal 10 hour waiting period)
    Add-KdsRootKey -EffectiveTime ((Get-Date).AddHours(-10))
}

# Create a security group for HGS nodes
$hgsNodes = New-ADGroup -Name 'HgsServers' -GroupScope DomainLocal -PassThru

# Add your HGS nodes to this group
# If your HGS server object is under an organizational unit, provide the full distinguished name instead of "HGS01"
Add-ADGroupMember -Identity $hgsNodes -Members "HGS01"

# Create the gMSA
New-ADServiceAccount -Name 'HGSgMSA' -DnsHostName 'HGSgMSA.yourdomain.com' -PrincipalsAllowedToRetrieveManagedPassword $hgsNodes
```

Le compte gMSA nécessitera le droit Générer des événements dans le journal de sécurité sur chaque serveur SGH.
Si vous utilisez la stratégie de groupe pour configurer l’attribution des droits utilisateur, vérifiez que le compte de service administré de groupe possède le [générer le privilège d’événements d’audit](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn221956%28v=ws.11%29) sur vos serveurs SGH.

> [!NOTE]
> Comptes de service administrés de groupe sont disponibles depuis le schéma Active Directory de Windows Server 2012.
> Pour plus d’informations, consultez [exigences relatives aux comptes de service administré de groupe](https://technet.microsoft.com/library/jj128431.aspx).

## <a name="jea-security-groups"></a>Groupes de sécurité JEA

Lorsque vous configurez SGH, un [Just Enough Administration (JEA)](https://aka.ms/JEAdocs) PowerShell de point de terminaison est configuré pour permettre aux administrateurs de gérer SGH sans avoir besoin des privilèges d’administrateur local complet.
Vous n’êtes pas obligé de JEA permet de gérer SGH, mais il toujours doit être configuré lors de l’exécution de Initialize-HgsServer.
La configuration du point de terminaison JEA se compose de désigner des 2 groupes de sécurité qui contiennent votre SGH administrateurs et les réviseurs de SGH.
Les utilisateurs qui appartiennent au groupe administrateur peuvent ajouter, modifier ou supprimer des stratégies sur SGH ; réviseurs peuvent uniquement afficher la configuration actuelle.

Créer des groupes de sécurité 2 pour ces groupes JEA à l’aide des outils d’administration Active Directory ou [New-ADGroup](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adgroup).

```powershell
New-ADGroup -Name 'HgsJeaReviewers' -GroupScope DomainLocal
New-ADGroup -Name 'HgsJeaAdmins' -GroupScope DomainLocal
```

## <a name="cluster-objects"></a>Objets de cluster

Si le compte que vous utilisez pour configurer le service SGH n’a pas autorisé à créer des objets d’ordinateur dans le domaine, vous devez prédéfinir les objets de cluster.
Ces étapes sont expliquées dans [Prestage Cluster Computer Objects in Active Directory Domain Services](https://technet.microsoft.com/library/dn466519(v=ws.11).aspx).

Pour configurer votre premier nœud SGH, vous devrez créer un objet de nom de Cluster (CNO) et un objet virtuel d’ordinateur (VCO).
Le CNO représente le nom du cluster et est principalement utilisé en interne par le Clustering de basculement.
L’objet ordinateur virtuel représente le service SGH qui réside sur le cluster et sera le nom inscrit auprès du serveur DNS.

> [!IMPORTANT]
> L’utilisateur qui exécutera `Initialize-HgsServer` requiert **contrôle total** sur les objets CNO et VCO dans Active Directory.

Pour préparer rapidement votre objet nom de cluster et les VCO, ont un administrateur Active Directory à exécuter les commandes PowerShell suivantes :

```powershell
# Create the CNO
$cno = New-ADComputer -Name 'HgsCluster' -Description 'HGS CNO' -Enabled $false -Passthru

# Create the VCO
$vco = New-ADComputer -Name 'HgsService' -Description 'HGS VCO' -Passthru

# Give the CNO full control over the VCO
$vcoPath = Join-Path "AD:\" $vco.DistinguishedName
$acl = Get-Acl $vcoPath
$ace = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $cno.SID, "GenericAll", "Allow"
$acl.AddAccessRule($ace)
Set-Acl -Path $vcoPath -AclObject $acl

# Allow time for your new CNO and VCO to replicate to your other Domain Controllers before continuing
```

## <a name="security-baseline-exceptions"></a>Exceptions de sécurité de base

Si vous déployez SGH dans un environnement hautement verrouillé, certains paramètres de stratégie de groupe peuvent empêcher le service SGH de fonctionner normalement.
Vérifiez vos objets de stratégie de groupe pour les paramètres suivants et suivez les instructions si vous êtes concerné :

### <a name="network-logon"></a>Ouverture de session réseau

**Chemin d’accès de la stratégie :** Ordinateur Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignments

**Nom de la stratégie :** Interdire l’accès à cet ordinateur à partir du réseau

**Valeur requise :** Vérifiez que la valeur ne bloque pas les ouvertures de session réseau pour tous les comptes locaux. Vous pouvez en toute sécurité bloquer les comptes d’administrateur local, toutefois.

**Raison :** Le Clustering de basculement s’appuie sur un compte local non-administrateur appelé CLIUSR pour gérer les nœuds de cluster. Ouverture de session réseau blocage pour cet utilisateur empêchera le cluster fonctionne correctement.

### <a name="kerberos-encryption"></a>Chiffrement Kerberos

**Chemin d’accès de la stratégie :** Configuration ordinateur\Paramètres Windows\Paramètres de sécurité\Stratégies locales\Options de sécurité

**Nom de la stratégie :** Sécurité réseau : Configurer les types de chiffrement autorisés pour Kerberos

**Action**: Si cette stratégie est configurée, vous devez mettre à jour le compte de service administré de groupe avec [Set-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/set-adserviceaccount?view=win10-ps) à utiliser uniquement les types de chiffrement pris en charge dans cette stratégie. Par exemple, si votre stratégie autorise uniquement AES128\_HMAC\_SHA1 et AES256\_HMAC\_SHA1, vous devez exécuter `Set-ADServiceAccount -Identity HGSgMSA -KerberosEncryptionType AES128,AES256`.



## <a name="next-steps"></a>Étapes suivantes

- Pour les étapes suivantes configurer l’attestation basée sur le module de plateforme sécurisée, consultez [initialiser le cluster SGH à l’aide du mode de module de plateforme sécurisée dans une forêt bastion existante](guarded-fabric-initialize-hgs-tpm-mode-bastion.md).
- Pour les étapes suivantes configurer l’attestation de clé hôte, consultez [initialiser le cluster SGH à l’aide du mode clé dans une forêt bastion existante](guarded-fabric-initialize-hgs-key-mode-bastion.md).
- Pour la prochaine procédure de configuration de l’administrateur d’une attestation (déconseillée dans Windows Server 2019), consultez [initialiser le cluster SGH à l’aide du mode AD dans une forêt bastion existante](guarded-fabric-initialize-hgs-ad-mode-bastion.md).

