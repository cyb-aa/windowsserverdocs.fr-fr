---
title: Déploiement du service Guardian hôte
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 310b63d9-5ac7-4961-98ef-103af45d706a
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/14/2020
ms.openlocfilehash: e66e7f365553f3aa106abbebf372492e0cc08386
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265941"
---
# <a name="deploying-the-host-guardian-service"></a>Déploiement du service Guardian hôte 

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

L’un des objectifs les plus importants de la fourniture d’un environnement hébergé est de garantir la sécurité des ordinateurs virtuels en cours d’exécution dans l’environnement. En tant que fournisseur de services cloud ou administrateur d’un cloud privé d’entreprise, vous pouvez utiliser une structure protégée pour offrir un environnement plus sécurisé pour les machines virtuelles. Une structure protégée se compose d’un Service Guardian hôte (HGS), généralement, un cluster de trois nœuds, d’un ou de plusieurs hôtes protégés et d’un ensemble d’ordinateurs virtuels.

## <a name="video-deploying-a-guarded-fabric"></a>Vidéo : déploiement d’une infrastructure protégée 

> [!VIDEO https://www.microsoft.com/videoplayer/embed/dcd8e99f-36f1-4bc8-b3d2-9576da38d9f1?autoplay=false]

## <a name="deployment-tasks-for-guarded-fabrics-and-shielded-vms"></a>Tâches de déploiement pour les infrastructures protégées et les machines virtuelles protégées

Le tableau suivant détaille les tâches de déploiement d’une infrastructure protégée et crée des machines virtuelles protégées en fonction de différents rôles d’administrateur. Notez que lorsque l’administrateur SGH configure SGH avec des ordinateurs hôtes Hyper-V autorisés, un administrateur d’infrastructure collecte et fournit des informations d’identification sur les ordinateurs hôtes en même temps.    

|<img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-administrator-tasks.png" alt="Host Guardian Service administrator tasks" width="238" height="62" align="left" /> | <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-fabric-administrator-tasks.png" alt="Fabric administrator tasks" width="300" height="62" align="left" /> | <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-tenant-administrator-tasks.png" alt="Tenant administrator tasks" width="184" height="66" align="left" /> |
|-------------------------------------|--------------------------------|-----------------------------------------|
|(1) [vérifier les conditions préalables du SGH](guarded-fabric-prepare-for-hgs.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png" alt="Step 1" hspace="8" align="right" />| | |
|(2) [configurer le premier nœud de&nbsp;sgh](guarded-fabric-choose-where-to-install-hgs.md)&nbsp;<img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-first-hgs-node.png" alt="Step 2" hspace="8" align="right" />| | |
|(3) [configurer des nœuds de&nbsp;SGH supplémentaires](guarded-fabric-configure-additional-hgs-nodes.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-secondary-hgs-nodes.png" alt="Step 3" hspace="8" align="right" />| | |
| &nbsp; |(4) [configurer le système DNS](guarded-fabric-configuring-fabric-dns.md) de l’infrastructure <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-fabric-dns.png" alt="Step 4" hspace="8" align="right" />| |
| &nbsp; |(5) [vérifier les conditions préalables de l’hôte&nbsp;(clé)](guarded-fabric-guarded-host-prerequisites.md#host-key-attestation)<br>[Vérifier la configuration requise de l’hôte&nbsp;(TPM)](guarded-fabric-guarded-host-prerequisites.md#tpm-trusted-attestation)<img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png" alt="Step 5" hspace="8" align="right" />| |
|(7) [configurer SGH avec les informations de l’hôte](guarded-fabric-add-host-information-to-hgs.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-hgs-with-host-info.png" alt="Step 7" hspace="8" align="right" />|(6) [créer une clé d’hôte (clé)](guarded-fabric-create-host-key.md)<br>[Collecter les informations de l’hôte (TPM)](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-collect-info-from-hosts.png" alt="Step 6" hspace="8" align="right" />| |
| &nbsp; |(8) [confirmer que les hôtes peuvent attester](guarded-fabric-confirm-hosts-can-attest-successfully.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-confirm-hosts-attest.png" alt="Step 8" hspace="8" align="right" />| |
| &nbsp; |(9) [configurer VMM (facultatif)](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-vmm.png" alt="Step 9" hspace="8" align="right" />| |
| &nbsp; |(10) [créer des disques de modèle](guarded-fabric-create-a-shielded-vm-template.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-create-template-disk.png" alt="Step 10" hspace="8" align="right" />| |
| &nbsp; |(11) [créer un disque d’assistance de protection de machine virtuelle pour VMM (facultatif)](guarded-fabric-vm-shielding-helper-vhd.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-create-helper-disk.png" alt="Step 11" hspace="8" align="right" />| |
| &nbsp; |(12) [configurer Windows Azure Pack (facultatif)](guarded-fabric-shielded-vm-windows-azure-pack.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-windows-azure-pack.png" alt="Step 12" hspace="8" align="right" />| |
| &nbsp; | &nbsp; |(13) [créer un fichier de données de protection](guarded-fabric-tenant-creates-shielding-data.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-file.png" alt="Step 13" hspace="8" align="right" />|
| &nbsp; | &nbsp; |(14) [créer des machines virtuelles dotées d’une protection maximale à l’aide de Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png" alt="Step 14" hspace="8" align="right" /><br>[Créer des machines virtuelles protégées à l’aide de VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-vms) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png" alt="Step 15" hspace="8" align="right" />|


## <a name="see-also"></a>Articles associés

- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)
