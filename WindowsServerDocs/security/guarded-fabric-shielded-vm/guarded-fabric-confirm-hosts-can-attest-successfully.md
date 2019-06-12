---
title: Confirmer le diront hôtes service Guardian
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7485796b-b840-4678-9b33-89e9710fbbc7
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 02/05/2019
ms.openlocfilehash: 87878eba785c0e1cc50454a74b2af4a159e88e12
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443664"
---
# <a name="confirm-guarded-hosts-can-attest"></a>Confirmer le diront hôtes service Guardian 

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016


Un administrateur d’infrastructure doit confirmer que les hôtes Hyper-V peuvent s’exécuter en tant qu’hôtes service Guardian. Procédez comme suit au moins un hôte service Guardian :

1.  Si vous n’avez pas déjà installé le rôle Hyper-V et la fonctionnalité de Support de Hyper-V Guardian hôte, installez-les selon la commande suivante :

        Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart

2.  Assurez-vous que l’hôte Hyper-V peut résoudre le nom DNS de HGS et dispose d’une connectivité réseau pour atteindre le port 80 (ou 443 si vous configurez le protocole HTTPS) sur le serveur SGH.

2.  Configurer la Protection de clé et des URL de l’Attestation de l’hôte :

    - **Via Windows PowerShell**: Vous pouvez configurer la Protection de clé et l’URL d’attestation d’en exécutant la commande suivante dans une console Windows PowerShell avec élévation de privilèges. Pour &lt;nom de domaine complet&gt;, utilisez le nom de domaine complet (FQDN) de votre cluster SGH (par exemple, hgs.bastion.local, ou demandez à l’administrateur SGH pour exécuter le **Get-HgsServer** applet de commande sur le serveur SGH pour récupérer le URL).

        `Set-HgsClientConfiguration -AttestationServerUrl 'http://<FQDN>/Attestation' -KeyProtectionServerUrl 'http://<FQDN>/KeyProtection'`

        Pour configurer un serveur SGH de secours, répétez cette commande et spécifiez l’URL de secours pour les services d’Attestation et de Protection de clé. Pour plus d’informations, consultez [configuration de secours](guarded-fabric-manage-branch-office.md#fallback-configuration). 

    - **Via VMM**: Si vous utilisez System Center 2016 - Virtual Machine Manager (VMM), vous pouvez configurer l’Attestation et URL de Protection de clé dans VMM. Pour plus d’informations, consultez [configurer les paramètres généraux SGH](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts#configure-global-hgs-settings) dans **approvisionner des hôtes dans VMM**.
    
    >**Notes de publication**
    > - Si l’administrateur SGH [activé le protocole HTTPS sur le serveur SGH](guarded-fabric-configure-hgs-https.md), commencer les URL avec `https://`.
    > - Si l’administrateur SGH activé le protocole HTTPS sur le serveur SGH et utilisé un certificat auto-signé, vous devez importer le certificat dans le magasin d’autorités de certification racine approuvée sur chaque hôte. Pour ce faire, exécutez la commande suivante sur chaque ordinateur hôte :<br>
        `Import-Certificate -FilePath "C:\temp\HttpsCertificate.cer" -CertStoreLocation Cert:\LocalMachine\Root`
    
3.  Pour lancer une tentative d’attestation sur l’ordinateur hôte et afficher l’état d’attestation, exécutez la commande suivante :

        Get-HgsClientConfiguration

    La sortie de la commande indique si l’hôte passé une attestation et qu’il est maintenant protégé. Si `IsHostGuarded` ne retourne pas **True**, vous pouvez exécuter l’outil de diagnostic SGH, [Get-HgsTrace](https://technet.microsoft.com/library/mt718831.aspx), à examiner. Pour exécuter les diagnostics, entrez la commande suivante dans une invite de Windows PowerShell avec élévation de privilèges sur l’ordinateur hôte :

        Get-HgsTrace -RunDiagnostics -Detailed

    > [!IMPORTANT]
    > Si vous utilisez Windows Server 2019 ou Windows 10, version 1809 et sont à l’aide de stratégies d’intégrité du code, `Get-HgsTrace` renvoie un échec pour le **Code intégrité stratégie Active** diagnostic.
    > Vous pouvez ignorer ce résultat lorsqu’il est l’uniquement Échec de diagnostic.

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Déployer des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

## <a name="see-also"></a>Voir aussi

- [Déployer le Service de Guardian hôte (SGH)](guarded-fabric-deploying-hgs-overview.md)
- [Déployer des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

