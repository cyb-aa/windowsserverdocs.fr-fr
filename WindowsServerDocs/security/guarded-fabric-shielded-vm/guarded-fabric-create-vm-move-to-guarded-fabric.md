---
redirect_url: guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md
title: 'Machines virtuelles protégées pour les locataires : création d’une machine virtuelle protégée localement et déplacement de celle-ci vers une infrastructure protégée'
ms.prod: windows-server
ms.topic: article
ms.assetid: 0ca1efa0-01f9-4b6f-87d4-c66db00d7d70
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: a5ca3ab29b83d0cb6cb2d55507471790f65800a2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856722"
---
# <a name="shielded-vms-for-tenants---creating-a-new-shielded-vm-on-premises-and-moving-it-to-a-guarded-fabric"></a>Machines virtuelles protégées pour les locataires : création d’une machine virtuelle protégée localement et déplacement de celle-ci vers une infrastructure protégée

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit les étapes de création d’une machine virtuelle protégée à l’aide d’Hyper-V uniquement. autrement dit, sans Virtual Machine Manager, les disques de modèle ou un fichier de données de protection. Il s’agit d’un scénario rare pour la plupart des environnements d’hébergement de cloud public, mais il peut être utile lors du test d’une structure protégée ou dans des scénarios d’entreprise où une machine virtuelle est déplacée d’une infrastructure départementale vers une infrastructure informatique partagée et doit être chiffrée avant la migration.

Pour comprendre le fonctionnement de cette rubrique dans le processus global de déploiement de machines virtuelles protégées, consultez [étapes de configuration du fournisseur de services d’hébergement pour les hôtes service Guardian et les machines virtuelles](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)protégées.

## <a name="import-the-guardian-configuration-on-the-tenant-hyper-v-server"></a>Importer la configuration Guardian sur le serveur Hyper-V du locataire

1.  Avant de commencer la procédure, assurez-vous que vous êtes sur un ordinateur hôte Hyper-V exécutant Windows Server 2016 avec les rôles et fonctionnalités suivants installés :

    - Role

        - Hyper-V

    - Fonctionnalités

        - Outils d’administration de fonctionnalités de Outils d’administration de serveur distant\\\\outils de machine virtuelle protégée

    > [!NOTE]
    > L’hôte utilisé ici ne doit *pas* être un hôte dans l’infrastructure protégée. Il s’agit d’un hôte distinct où les machines virtuelles sont préparées avant d’être déplacées vers l’infrastructure protégée.

2.  Avant de pouvoir exécuter une machine virtuelle protégée sur cet ordinateur, vous devez vous assurer que la sécurité basée sur la virtualisation est activée et en cours d’exécution. Exécutez la commande suivante dans une fenêtre Windows PowerShell avec élévation de privilèges pour installer la fonctionnalité de prise en charge d’Hyper-V Guardian hôte. Cela permet de configurer tous les paramètres nécessaires sur votre ordinateur pour pouvoir exécuter des machines virtuelles protégées.

        Install-WindowsFeature HostGuardian

3.  Vous devez acquérir les métadonnées Guardian pour l’infrastructure protégée dans laquelle votre machine virtuelle s’exécutera. Ces métadonnées sont utilisées pour autoriser cette infrastructure à exécuter votre machine virtuelle protégée. La façon dont vous obtenez ces informations sera différente pour chaque fournisseur de services d’hébergement ou entreprise. L’hébergeur (ou vous, si vous avez accès au réseau de l’infrastructure protégée) peut obtenir ces informations en exécutant la commande Windows PowerShell suivante :

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

    Dans l’exemple ci-dessus, « SGH » est le nom de réseau distribué du cluster SGH et « bastion. local » est le nom du domaine SGH.

4.  Pour importer la clé Guardian dont vous aurez besoin dans une procédure ultérieure, exécutez la commande suivante.

    Pour &lt;chemin d’accès&gt;&lt;nom de fichier&gt;, remplacez le chemin d’accès et le nom de fichier du fichier XML que vous avez enregistré à l’étape précédente, par exemple : **C :\\temp\\GuardianKey. xml**

    Pour &lt;&gt;GuardianName, spécifiez un nom pour le fournisseur d’hébergement ou le centre de vos entreprises, par exemple, **HostingProvider1**. Notez le nom de la procédure suivante.

    Include **-AllowUntrustedRoot** uniquement si le serveur SGH a été configuré avec des certificats auto-signés. (Ces certificats font partie du service de protection de clé dans SGH.)

        Import-HgsGuardian -Path '<Path><Filename>' -Name '<GuardianName>' -AllowUntrustedRoot

## <a name="create-a-new-shielded-virtual-machine-on-the-host"></a>Créer un nouvel ordinateur virtuel protégé sur l’ordinateur hôte

Dans cette procédure, vous allez créer un ordinateur virtuel sur l’hôte Hyper-V et le préparer pour l’exportation vers votre fournisseur d’hébergement ou votre administrateur de centre de clients, qui peut l’exécuter sur un hôte service Guardian.

Dans le cadre de la procédure, vous allez créer un protecteur de clé qui contient deux éléments importants :

-   **Propriétaire**: dans le protecteur de clé, vous-même ou plus probable, le groupe dans lequel vous travaillez, qui partage des éléments de sécurité tels que des certificats, est identifié comme « propriétaire » de la machine virtuelle. Votre identité en tant que propriétaire est représentée par un certificat qui, si vous exécutez les commandes comme indiqué, est généré sous la forme d’un certificat auto-signé. Si vous le souhaitez, vous pouvez utiliser à la place un certificat sauvegardé par l’infrastructure de l’infrastructure à clé publique et omettre le paramètre **-AllowUntrustedRoot** dans les commandes.

-   **Gardiens**: dans le protecteur de clé, votre fournisseur d’hébergement ou centre de code d’entreprise (qui exécute les hôtes SGH et service Guardian) est identifié en tant que « gardien ». Le gardien est représenté par la clé Guardian que vous avez importée dans la procédure précédente, puis vous [importez la configuration Guardian sur le serveur Hyper-V du locataire](#import-the-guardian-configuration-on-the-tenant-hyper-v-server).

Pour obtenir une illustration illustrant le protecteur de clé, qui est un élément dans un fichier de données de protection, consultez [qu’est-ce que les données de protection et pourquoi est-il nécessaire ?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

1. Sur un ordinateur hôte Hyper-V client, pour créer un ordinateur virtuel de 2e génération, exécutez la commande suivante.

   Pour &lt;&gt;ShieldedVMname, spécifiez un nom pour la machine virtuelle, par exemple : **ShieldVM1**
    
   Pour &lt;&gt;VHDPath, spécifiez un emplacement pour stocker le VHDX de la machine virtuelle, par exemple : **C :\\machines virtuelles\\ShieldVM1\\ShieldVM1. VHDX**
    
   Pour &lt;&gt;nnGB, spécifiez une taille pour le VHDX, par exemple : **60 Go**

       New-VM -Generation 2 -Name "<ShieldedVMname>" -NewVHDPath <VHDPath>.vhdx -NewVHDSizeBytes <nnGB>

2. Installez un système d’exploitation pris en charge (Windows Server 2012 ou version ultérieure, Windows 8 client ou version ultérieure) sur la machine virtuelle et activez la connexion Bureau à distance et la règle de pare-feu correspondante. Enregistrez l’adresse IP et/ou le nom DNS de la machine virtuelle. vous en aurez besoin pour vous y connecter à distance.

3. Utilisez le protocole RDP pour vous connecter à distance à la machine virtuelle et vérifier que le protocole RDP et le pare-feu sont configurés correctement. Dans le cadre du processus de protection, l’accès de la console à la machine virtuelle via Hyper-V est désactivé. il est donc important de s’assurer que vous êtes en mesure de gérer à distance le système sur le réseau.

4. Pour créer un protecteur de clé (décrit au début de cette section), exécutez la commande suivante.

   Pour &lt;&gt;GuardianName, utilisez le nom que vous avez spécifié dans la procédure précédente, par exemple : **HostingProvider1**

   Include **-AllowUntrustedRoot** pour autoriser les certificats auto-signés.

       $Guardian = Get-HgsGuardian -Name '<GuardianName>'

       $Owner = New-HgsGuardian -Name 'Owner' -GenerateCertificates

       $KP = New-HgsKeyProtector -Owner $Owner -Guardian $Guardian -AllowUntrustedRoot

   Si vous souhaitez que plusieurs centres de informations puissent exécuter votre machine virtuelle protégée (par exemple, un site de récupération d’urgence et un fournisseur de cloud public), vous pouvez fournir une liste de gardiens au paramètre **-Guardian** . Pour plus d’informations, consultez [New-HgsKeyProtector] (https://docs.microsoft.com/powershell/module/hgsclient/new-hgskeyprotector?view=win10-ps.

5. Pour activer vTPM à l’aide du protecteur de clé, exécutez la commande suivante. Pour &lt;&gt;ShieldedVMname, utilisez le même nom de machine virtuelle que celui utilisé dans les étapes précédentes.

       $VMName="<ShieldedVMname>"

       Stop-VM -Name $VMName -Force

       Set-VMKeyProtector -VMName $VMName -KeyProtector $KP.RawData

       Set-VMSecurityPolicy -VMName $VMName -Shielded $true

       Enable-VMTPM -VMName $VMName

6. Pour démarrer la machine virtuelle afin de vérifier que le protecteur de clé fonctionne avec les certificats de propriétaire local, exécutez la commande suivante.

       Start-VM -Name $VMName

7. Vérifiez que la machine virtuelle a démarré dans la console Hyper-V.

8. Utilisez le protocole RDP pour vous connecter à distance à la machine virtuelle et activer BitLocker sur toutes les partitions de tous les fichiers vhdx attachés à la machine virtuelle protégée.

   > [!IMPORTANT]
   > Avant de passer à l’étape suivante, attendez que le chiffrement BitLocker se termine sur toutes les partitions où vous l’avez activé.

9. Arrêtez la machine virtuelle lorsque vous êtes prêt à la déplacer vers l’infrastructure protégée.

10. Sur le serveur Hyper-V du locataire, exportez la machine virtuelle à l’aide de l’outil de votre choix (Windows PowerShell ou Hyper-V Manager). Organisez ensuite les fichiers à copier sur un hôte service Guardian géré par votre fournisseur d’hébergement ou votre centre de fichiers d’entreprise.

11. **Pour le fournisseur d’hébergement ou le centre de connaissances de l’entreprise**:

    Importez la machine virtuelle protégée à l’aide du Gestionnaire Hyper-V ou de Windows PowerShell. Vous devez importer le fichier de configuration de machine virtuelle à partir du propriétaire de la machine virtuelle afin de démarrer la machine virtuelle. Cela est dû au fait que le protecteur de clé et le module de plateforme sécurisée virtuel de la machine virtuelle sont stockés dans le fichier de configuration. Si la machine virtuelle est configurée pour s’exécuter sur l’infrastructure protégée, elle doit pouvoir démarrer correctement.

## <a name="see-also"></a>Voir aussi

- [Étapes de configuration du fournisseur de services d’hébergement pour les hôtes service Guardian et les machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)
