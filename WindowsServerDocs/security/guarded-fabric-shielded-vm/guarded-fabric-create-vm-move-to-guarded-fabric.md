---
redirect_url: guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md
title: Protégées de machines virtuelles protégées pour les locataires - création d’une nouvelle machine virtuelle en local et en la déplaçant vers une structure protégée
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 0ca1efa0-01f9-4b6f-87d4-c66db00d7d70
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9601145048b8798cfb102757384da49bed16a538
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469627"
---
# <a name="shielded-vms-for-tenants---creating-a-new-shielded-vm-on-premises-and-moving-it-to-a-guarded-fabric"></a>Protégées de machines virtuelles protégées pour les locataires - création d’une nouvelle machine virtuelle en local et en la déplaçant vers une structure protégée

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit les étapes pour créer une machine virtuelle protégée, à l’aide de Hyper-V uniquement ; Autrement dit, sans Virtual Machine Manager, les disques de modèle ou un fichier de données de protection. Ce scénario est rare pour le cloud public plus environnements d’hébergement, mais peut être utile lorsque vous testez une structure protégée ou dans l’entreprise scénarios où une machine virtuelle est déplacée d’un service fabric à partagé infrastructure informatique et doivent être chiffrés avant la migration.

Pour comprendre comment cette rubrique s’intègre dans le processus de déploiement de machines virtuelles protégées, consultez [hébergeant des étapes de configuration de fournisseur de service pour les hôtes service Guardian et des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="import-the-guardian-configuration-on-the-tenant-hyper-v-server"></a>Importer la configuration de surveillance sur le serveur client Hyper-V

1.  Avant de commencer la procédure, vérifiez que vous êtes sur un hôte Hyper-V exécutant Windows Server 2016 avec les fonctionnalités installées et les rôles suivants :

    - Rôle

        - Hyper-V

    - Fonctionnalités

        - Outils d’Administration de serveur distant\\fonctionnalité des outils d’Administration\\protégées des outils de machine virtuelle

    > [!NOTE]
    > L’hôte utilisé ici doit *pas* être un ordinateur hôte dans la structure protégée. Il s’agit d’un hôte séparé, où les machines virtuelles sont préparées avant d’être transférées à l’infrastructure service Guardian.

2.  Avant de pouvoir exécuter une machine virtuelle protégée sur cet ordinateur, vous devez garantir la que sécurité en fonction de la virtualisation est activé et en cours d’exécution. Exécutez la commande suivante dans une fenêtre Windows PowerShell avec élévation de privilèges pour installer la fonctionnalité de Support de Hyper-V Guardian hôte. Cette opération configure tous les paramètres nécessaires sur votre ordinateur pour être en mesure d’exécuter des machines virtuelles protégées.

        Install-WindowsFeature HostGuardian

3.  Vous devez acquérir les métadonnées pour l’infrastructure protégée où votre machine virtuelle s’exécutera. Ces métadonnées sont utilisées pour autoriser cette infrastructure à exécuter votre machine virtuelle protégée. Comment vous procurer ce sera différente pour chaque fournisseur de services ou d’une entreprise. L’hébergeur (ou, si vous avez accès au réseau de l’infrastructure protégée) peut acquérir ces informations en exécutant la commande Windows PowerShell suivante :

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

    Dans l’exemple ci-dessus, « sgh » est le nom de réseau distribué du cluster SGH et « bastion.local » est le nom du domaine SGH.

4.  Pour importer la clé de gardien, vous aurez besoin dans une procédure ultérieure, exécutez la commande suivante.

    Pour &lt;chemin d’accès&gt;&lt;Filename&gt;, remplacez le chemin d’accès et nom de fichier du fichier XML du fichier vous enregistré à l’étape précédente, par exemple : **C:\\temp\\GuardianKey.xml**

    Pour &lt;GuardianName&gt;, spécifiez un nom pour votre fournisseur d’hébergement ou d’une entreprise de centre de données, par exemple, **HostingProvider1**. Notez le nom de la procédure suivante.

    Inclure **- AllowUntrustedRoot** uniquement si le serveur SGH a été configuré avec des certificats auto-signés. (Ces certificats font partie du Service de Protection de clé dans SGH.)

        Import-HgsGuardian -Path '<Path><Filename>' -Name '<GuardianName>' -AllowUntrustedRoot

## <a name="create-a-new-shielded-virtual-machine-on-the-host"></a>Créez une nouvelle machine virtuelle protégée sur l’hôte

Dans cette procédure, vous créez une machine virtuelle sur l’ordinateur hôte Hyper-V et préparer pour l’exportation vers votre fournisseur d’hébergement ou d’un administrateur de centre de données, ce qui peut l’exécuter sur un ordinateur hôte service Guardian.

Dans le cadre de la procédure, vous allez créer un protecteur de clé qui contient deux éléments importants :

-   **Propriétaire**: Dans le protecteur de clé, vous - ou plus vraisemblablement, le groupe de que participer, qui partage des éléments tels que les certificats de sécurité - est identifié comme « propriétaire » de la machine virtuelle. Votre identité en tant que propriétaire est représentée par un certificat qui, si vous exécutez les commandes comme indiqué, est généré comme un certificat auto-signé. Si vous le souhaitez, vous pouvez utiliser un certificat soutenu par une infrastructure à clé publique et omettez la **- AllowUntrustedRoot** paramètre dans les commandes.

-   **Gardiens**: Également dans le protecteur de clé, votre fournisseur d’hébergement ou d’un centre de données enterprise (qui exécute le service SGH et les hôtes service Guardian) est identifié comme un « surveillance ». La surveillance est représenté par la clé de gardien que vous avez importé dans la procédure précédente, [importer la configuration de surveillance sur le serveur client Hyper-V](#import-the-guardian-configuration-on-the-tenant-hyper-v-server).

Pour obtenir une illustration montrant le protecteur de clé, qui est un élément dans un fichier de données de protection, consultez [ce qui est des données de protection et pourquoi est-il nécessaire ?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

1. Sur un hôte Hyper-V, de locataire pour créer une machine virtuelle de nouvelle génération 2, exécutez la commande suivante.

   Pour &lt;ShieldedVMname&gt;, spécifiez un nom pour la machine virtuelle, par exemple : **ShieldVM1**
    
   Pour &lt;VHDPath&gt;, spécifiez un emplacement pour stocker le VHDX de la machine virtuelle, par exemple : **C:\\VMs\\ShieldVM1\\ShieldVM1.vhdx**
    
   Pour &lt;nnGB&gt;, spécifiez une taille pour VHDX, par exemple : **60 GO**

       New-VM -Generation 2 -Name "<ShieldedVMname>" -NewVHDPath <VHDPath>.vhdx -NewVHDSizeBytes <nnGB>

2. Installer un système d’exploitation pris en charge (client de Windows Server 2012 ou version ultérieure, de Windows 8 ou version ultérieure) sur la machine virtuelle et activer la connexion Bureau à distance et la règle de pare-feu correspondante. Enregistrer l’adresse IP et/ou le nom DNS ; de la machine virtuelle vous en aurez besoin pour la connexion à distance.

3. Utilisez RDP pour se connecter à la machine virtuelle à distance et de vérifier que RDP et le pare-feu sont correctement configurés. Dans le cadre du processus de protection, console à la machine virtuelle via Hyper-V est désactivé, il est donc important que vous êtes en mesure de gérer à distance le système sur le réseau.

4. Pour créer un nouveau protecteur de clé (décrite au début de cette section), exécutez la commande suivante.

   Pour &lt;GuardianName&gt;, utilisez le nom spécifié dans la procédure précédente, par exemple : **HostingProvider1**

   Inclure **- AllowUntrustedRoot** pour autoriser les certificats auto-signés.

       $Guardian = Get-HgsGuardian -Name '<GuardianName>'

       $Owner = New-HgsGuardian -Name 'Owner' -GenerateCertificates

       $KP = New-HgsKeyProtector -Owner $Owner -Guardian $Guardian -AllowUntrustedRoot

   Si vous le souhaitez pour plus d’un centre de données être en mesure d’exécuter votre machine virtuelle protégée (par exemple, un site de récupération d’urgence et un fournisseur de cloud public), vous pouvez fournir une liste des gardiens à la **-Guardian** paramètre. Pour plus d’informations, consultez [New-HgsKeyProtector] (https://docs.microsoft.com/powershell/module/hgsclient/new-hgskeyprotector?view=win10-ps.

5. Pour activer la vTPM à l’aide du protecteur de clé, exécutez la commande suivante. Pour &lt;ShieldedVMname&gt;, utilisez le même nom de machine virtuelle utilisé dans les étapes précédentes.

       $VMName="<ShieldedVMname>"

       Stop-VM -Name $VMName -Force

       Set-VMKeyProtector -VMName $VMName -KeyProtector $KP.RawData

       Set-VMSecurityPolicy -VMName $VMName -Shielded $true

       Enable-VMTPM -VMName $VMName

6. Pour démarrer la machine virtuelle pour vérifier que le protecteur de clé fonctionne avec les certificats d’un propriétaire local, exécutez la commande suivante.

       Start-VM -Name $VMName

7. Vérifiez que la machine virtuelle a démarré dans la console Hyper-V.

8. Utilisez RDP pour se connecter à la machine virtuelle à distance et d’activer BitLocker sur toutes les partitions sur tous les fichiers vhdx partagés qui sont attachés à la machine virtuelle protégée.

   > [!IMPORTANT]
   > Avant de passer à l’étape suivante, attendez que le chiffrement BitLocker se termine sur toutes les partitions où vous avez activé il.

9. Arrêtez la machine virtuelle lorsque vous êtes prêt pour le déplacer vers la structure protégée.

10. Sur le serveur client Hyper-V, exportez la machine virtuelle à l’aide de l’outil de votre choix (Windows PowerShell ou le Gestionnaire Hyper-V). Puis réorganisez pour les fichiers à copier sur un hôte service Guardian géré par votre fournisseur d’hébergement ou d’une entreprise de centre de données.

11. **Pour le centre de données fournisseur ou enterprise hébergement**:

    Importer une machine virtuelle protégée à l’aide du Gestionnaire Hyper-V ou Windows PowerShell. Vous devez importer le fichier de configuration de machine virtuelle au propriétaire de la machine virtuelle afin de démarrer la machine virtuelle. Il s’agit, car le protecteur de clé et du module TPM virtuel de la machine virtuelle sont stockés dans le fichier de configuration. Si la machine virtuelle est configurée pour s’exécuter sur la structure protégée, elle doit être en mesure de démarrer correctement.

## <a name="see-also"></a>Voir aussi

- [Hébergement des étapes de configuration de fournisseur de service pour les hôtes service Guardian et des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)
