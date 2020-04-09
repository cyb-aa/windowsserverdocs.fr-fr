---
title: Installer SGH dans une forêt bastion existante
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 20e0d5e73713c0d6280e95d51ec8de8fde612350
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856582"
---
# <a name="install-hgs-in-an-existing-bastion-forest"></a>Installer SGH dans une forêt bastion existante 

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016


## <a name="join-the-hgs-server-to-the-existing-domain"></a>Joindre le serveur SGH au domaine existant

Dans une forêt bastion existante, SGH doit être ajouté au domaine racine. Utilisez Gestionnaire de serveur ou [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) pour joindre votre serveur SGH au domaine racine.

## <a name="add-the-hgs-server-role"></a>Ajouter le rôle de serveur SGH

Exécuter toutes les commandes de cette rubrique dans une session PowerShell avec élévation de privilèges.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

Si votre centre de centres dispose d’une forêt bastion sécurisée dans laquelle vous souhaitez joindre des nœuds SGH, procédez comme suit.
Vous pouvez également utiliser ces étapes pour configurer 2 ou plusieurs clusters SGH indépendants qui sont joints au même domaine.

## <a name="join-the-hgs-server-to-the-existing-domain"></a>Joindre le serveur SGH au domaine existant

Utilisez Gestionnaire de serveur ou [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) pour joindre les serveurs SGH au domaine souhaité.

## <a name="prepare-active-directory-objects"></a>Préparer les objets Active Directory

Créez un compte de service géré de groupe et 2 groupes de sécurité.
Vous pouvez également prédéfinir les objets du cluster si le compte que vous initialisez avec n’a pas l’autorisation de créer des objets ordinateur dans le domaine.

## <a name="group-managed-service-account"></a>Compte de service administré de groupe

Le compte de service administré de groupe (gMSA) est l’identité utilisée par SGH pour récupérer et utiliser ses certificats. Utilisez [New-ADServiceAccount](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adserviceaccount) pour créer un gMSA.
S’il s’agit du premier gMSA dans le domaine, vous devez ajouter une clé racine du service de distribution de clés.

Chaque nœud SGH doit être autorisé à accéder au mot de passe gMSA.
Le moyen le plus simple de configurer cela consiste à créer un groupe de sécurité qui contient tous vos nœuds SGH et à accorder à ce groupe de sécurité l’accès pour récupérer le mot de passe gMSA.

Vous devez redémarrer votre serveur SGH après l’avoir ajouté à un groupe de sécurité pour vous assurer qu’il obtient sa nouvelle appartenance à un groupe.

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

Le gMSA nécessite le droit de générer des événements dans le journal de sécurité sur chaque serveur SGH.
Si vous utilisez stratégie de groupe pour configurer l’attribution des droits utilisateur, assurez-vous que le compte gMSA reçoit le [privilège générer des événements d’audit](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn221956%28v=ws.11%29) sur vos serveurs SGH.

> [!NOTE]
> Les comptes de service administrés de groupe sont disponibles à partir du schéma de Active Directory Windows Server 2012.
> Pour plus d’informations, consultez [conditions requises pour le compte de service administré de groupe](https://technet.microsoft.com/library/jj128431.aspx).

## <a name="jea-security-groups"></a>Groupes de sécurité JEA

Quand vous configurez SGH, un point de terminaison PowerShell d' [administration juste assez (JEA)](https://aka.ms/JEAdocs) est configuré pour permettre aux administrateurs de gérer SGH sans avoir besoin de privilèges d’administrateur local complets.
Vous n’êtes pas obligé d’utiliser JEA pour gérer SGH, mais il doit toujours être configuré lors de l’exécution de Initialize-HgsServer.
La configuration du point de terminaison JEA consiste à désigner 2 groupes de sécurité qui contiennent vos administrateurs SGH et les réviseurs SGH.
Les utilisateurs qui appartiennent au groupe d’administrateurs peuvent ajouter, modifier ou supprimer des stratégies sur SGH. les réviseurs peuvent uniquement afficher la configuration actuelle.

Créez 2 groupes de sécurité pour ces groupes JEA à l’aide des outils d’administration de Active Directory ou [de New-](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adgroup)adgroup.

```powershell
New-ADGroup -Name 'HgsJeaReviewers' -GroupScope DomainLocal
New-ADGroup -Name 'HgsJeaAdmins' -GroupScope DomainLocal
```

## <a name="cluster-objects"></a>Objets de cluster

Si le compte que vous utilisez pour configurer SGH n’a pas l’autorisation de créer des objets ordinateur dans le domaine, vous devez prédéfinir les objets du cluster.
Ces étapes sont expliquées dans [préparer des objets d’ordinateur de cluster dans Active Directory Domain Services](https://technet.microsoft.com/library/dn466519(v=ws.11).aspx).

Pour configurer votre premier nœud SGH, vous devrez créer un objet nom de cluster (CNO) et un objet ordinateur virtuel (VCO).
Le CNO représente le nom du cluster et est principalement utilisé en interne par le clustering de basculement.
Le VCO représente le service SGH qui réside au-dessus du cluster et qui correspond au nom inscrit auprès du serveur DNS.

> [!IMPORTANT]
> L’utilisateur qui exécutera `Initialize-HgsServer` a besoin d’un **contrôle total** sur les objets CNO et VCO dans Active Directory.

Pour préparer rapidement votre CNO et votre VCO, vous devez disposer d’un administrateur Active Directory exécuter les commandes PowerShell suivantes :

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

## <a name="security-baseline-exceptions"></a>Exceptions de la ligne de base de sécurité

Si vous déployez le SGH dans un environnement hautement verrouillé, certains paramètres de stratégie de groupe peuvent empêcher le fonctionnement normal de SGH.
Vérifiez les paramètres suivants dans vos objets stratégie de groupe et suivez les instructions si vous en êtes affecté :

### <a name="network-logon"></a>Ouverture de session réseau

**Chemin de la stratégie :** Ordinateur \ paramètres de configuration \ stratégies Locales\attribution des droits d’ordinateur

**Nom de la stratégie :** Refuser l’accès à cet ordinateur à partir du réseau

**Valeur requise :** Vérifiez que la valeur ne bloque pas les ouvertures de session réseau pour tous les comptes locaux. Vous pouvez toutefois bloquer en toute sécurité les comptes d’administrateur local.

**Raison :** Le clustering de basculement s’appuie sur un compte local non-administrateur appelé CLIUSR pour gérer les nœuds de cluster. Le blocage de l’ouverture de session réseau pour cet utilisateur empêche le cluster de fonctionner correctement.

### <a name="kerberos-encryption"></a>Chiffrement Kerberos

**Chemin de la stratégie :** Paramètres ordinateur \ paramètres de configuration \ stratégies

**Nom de la stratégie :** Sécurité réseau : configurer les types de chiffrement autorisés pour Kerberos

**Action**: si cette stratégie est configurée, vous devez mettre à jour le compte GMSA avec [Set-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/set-adserviceaccount?view=win10-ps) pour utiliser uniquement les types de chiffrement pris en charge dans cette stratégie. Par exemple, si votre stratégie autorise uniquement AES128\_HMAC\_SHA1 et AES256\_HMAC\_SHA1, vous devez exécuter `Set-ADServiceAccount -Identity HGSgMSA -KerberosEncryptionType AES128,AES256`.



## <a name="next-steps"></a>Étapes suivantes :

- Pour connaître les étapes à suivre pour configurer l’attestation basée sur le module de plateforme sécurisée, consultez [initialiser le cluster SGH à l’aide du mode TPM dans une forêt bastion existante](guarded-fabric-initialize-hgs-tpm-mode-bastion.md).
- Pour connaître les étapes suivantes pour configurer l’attestation de clé hôte, consultez [initialiser le cluster SGH à l’aide du mode clé dans une forêt bastion existante](guarded-fabric-initialize-hgs-key-mode-bastion.md).
- Pour connaître les étapes à suivre pour configurer l’attestation basée sur l’administration (déconseillée dans Windows Server 2019), consultez [initialiser le cluster SGH à l’aide du mode ad dans une forêt bastion existante](guarded-fabric-initialize-hgs-ad-mode-bastion.md).

