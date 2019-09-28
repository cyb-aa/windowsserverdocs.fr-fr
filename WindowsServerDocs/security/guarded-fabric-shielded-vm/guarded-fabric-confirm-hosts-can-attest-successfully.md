---
title: Confirmer que les hôtes protégés peuvent attester
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 7485796b-b840-4678-9b33-89e9710fbbc7
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 02/05/2019
ms.openlocfilehash: bc047bbc23f4edfbbd77fb3e2d7258241d1f19d0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386696"
---
# <a name="confirm-guarded-hosts-can-attest"></a>Confirmer que les hôtes protégés peuvent attester 

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016


Un administrateur d’infrastructure doit confirmer que les hôtes Hyper-V peuvent s’exécuter en tant qu’hôtes service Guardian. Effectuez les étapes suivantes sur au moins un hôte service Guardian :

1.  Si vous n’avez pas encore installé le rôle Hyper-V et la fonctionnalité de prise en charge d’Hyper-V Guardian hôte, installez-les à l’aide de la commande suivante :

        Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart

2.  Assurez-vous que l’hôte Hyper-V peut résoudre le nom DNS SGH et qu’il dispose d’une connectivité réseau pour atteindre le port 80 (ou 443 si vous configurez HTTPs) sur le serveur SGH.

2.  Configurez les URL de protection et d’attestation des clés de l’hôte :

    - **Via Windows PowerShell**: Vous pouvez configurer les URL de protection de clé et d’attestation en exécutant la commande suivante dans une console Windows PowerShell avec élévation de privilèges. Par &lt;FQDN @ no__t-1, utilisez le nom de domaine complet (FQDN) de votre cluster SGH (par exemple, SGH. bastion. local) ou demandez à l’administrateur SGH d’exécuter l’applet de commande **« obtenir-HgsServer »** sur le serveur SGH pour récupérer les URL).

        `Set-HgsClientConfiguration -AttestationServerUrl 'http://<FQDN>/Attestation' -KeyProtectionServerUrl 'http://<FQDN>/KeyProtection'`

        Pour configurer un serveur SGH de secours, répétez cette commande et spécifiez les URL de secours pour les services de protection de clé et d’attestation. Pour plus d’informations, consultez [configuration de secours](guarded-fabric-manage-branch-office.md#fallback-configuration). 

    - **Via VMM**: Si vous utilisez System Center 2016-Virtual Machine Manager (VMM), vous pouvez configurer les URL d’attestation et de protection de clé dans VMM. Pour plus d’informations, consultez [configurer les paramètres globaux SGH](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts#configure-global-hgs-settings) dans **approvisionner des hôtes service Guardian dans VMM**.
    
    >**Notes**
    > - Si l’administrateur SGH a [activé le protocole HTTPS sur le serveur SGH](guarded-fabric-configure-hgs-https.md), commencez les url par `https://`.
    > - Si l’administrateur SGH a activé le protocole HTTPs sur le serveur SGH et utilisé un certificat auto-signé, vous devez importer le certificat dans le magasin d’autorités de certification racines de confiance sur chaque ordinateur hôte. Pour ce faire, exécutez la commande suivante sur chaque hôte :<br>
        `Import-Certificate -FilePath "C:\temp\HttpsCertificate.cer" -CertStoreLocation Cert:\LocalMachine\Root`
    
3.  Pour lancer une tentative d’attestation sur l’hôte et afficher l’état de l’attestation, exécutez la commande suivante :

        Get-HgsClientConfiguration

    La sortie de la commande indique si l’hôte a passé l’attestation et est maintenant protégé. Si `IsHostGuarded` ne retourne pas la **valeur true**, vous pouvez exécuter l’outil de diagnostics SGH, [HgsTrace](https://technet.microsoft.com/library/mt718831.aspx), pour examiner. Pour exécuter les Diagnostics, entrez la commande suivante dans une invite Windows PowerShell avec élévation de privilèges sur l’ordinateur hôte :

        Get-HgsTrace -RunDiagnostics -Detailed

    > [!IMPORTANT]
    > Si vous utilisez Windows Server 2019 ou Windows 10, version 1809 et que vous utilisez des stratégies d’intégrité du code, `Get-HgsTrace` retourne un échec pour la **stratégie d’intégrité du code active** diagnostic.
    > Vous pouvez ignorer ce résultat en toute sécurité lorsqu’il s’agit du seul diagnostic qui échoue.

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Déployer des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

## <a name="see-also"></a>Voir aussi

- [Déployer le service Guardian hôte (SGH)](guarded-fabric-deploying-hgs-overview.md)
- [Déployer des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

