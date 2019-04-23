---
title: Guide de démarrage rapide pour le déploiement de la structure protégée
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: e060e052-39a0-4154-90bb-b97cc6dde68e
manager: dongill
author: justinha
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: d63726e7046896c9ef7aa0c3b3d85237bc3f788d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879060"
---
# <a name="quick-start-for-guarded-fabric-deployment"></a>Guide de démarrage rapide pour le déploiement de la structure protégée

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique explique ce qu’une structure protégée est, ses exigences, ainsi qu’un résumé du processus de déploiement. Pour les étapes de déploiement détaillées, consultez [déployer le Service Guardian hôte pour les hôtes service Guardian et des machines virtuelles protégées](https://technet.microsoft.com/windows-server-docs/security/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview).

Vous préférez une vidéo ? Consultez le cours Microsoft Virtual Academy [déploiement de machines virtuelles protégées et une infrastructure service Guardian avec Windows Server 2016](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474).

## <a name="what-is-a-guarded-fabric"></a>Qu’est une infrastructure protégée

Un _service Guardian fabric_ est une infrastructure Windows Server 2016 Hyper-V capable de protéger les charges de travail clients contre l’inspection, le vol et la falsification contre les logiciels malveillants en cours d’exécution sur l’ordinateur hôte, ainsi que des administrateurs système. Ces charges de travail de locataire de virtualisées — protégés à la fois au repos et en transit, sont appelées _des machines virtuelles protégées_. 

## <a name="what-are-the-requirements-for-a-guarded-fabric"></a>Quelles sont les exigences pour une infrastructure protégée

La configuration requise pour une structure protégée est les suivantes :

- **Un emplacement pour exécuter des machines virtuelles qui est gratuite contre les logiciels malveillants protégées.**

    Ils sont appelés _hôtes service Guardian_. 
    Hôtes service Guardian sont les hôtes Hyper-V Windows Server 2016 Datacenter edition qui peuvent exécuter des machines virtuelles protégées uniquement s’ils peuvent prouver qu’ils sont en cours d’exécution dans un état connu et fiable pour une autorité externe appelée le Service Guardian hôte (SGH). 
    Le service SGH est un nouveau rôle de serveur dans Windows Server 2016 et est généralement déployé comme un cluster à trois nœuds. 

- **Une façon de vérifier un ordinateur hôte est dans un état sain.**

    Exécute le service SGH _attestation_, où il mesure l’intégrité des hôtes service Guardian.

- **Un processus pour libérer en toute sécurité les clés à des hôtes intègres.**

    Exécute le service SGH _la clé de protection et mise en production clé_, où il relâche les touches vers des hôtes intègres.

- **Outils de gestion pour automatiser l’approvisionnement sécurisé et l’hébergement des machines virtuelles protégées.**

    Si vous le souhaitez, vous pouvez ajouter ces outils de gestion à une structure protégée :

    - System Center 2016 Virtual Machine Manager (VMM). VMM est recommandée, car il fournit une gestion supplémentaire des outils au-delà de ce que vous obtenez de l’utilisation de simplement les applets de commande PowerShell qui sont fournis avec Hyper-V et les charges de travail infrastructure protégée).
    - System Center 2016 Service Provider Foundation (SPF). Il s’agit d’une couche d’API entre Windows Azure Pack et VMM et un composant requis pour l’utilisation de Windows Azure Pack.
    - Windows Azure Pack fournit une interface web graphique bonne pour gérer une infrastructure protégée et machines virtuelles protégées. 

Dans la pratique, une décision à l’avance : le [mode d’attestation](guarded-fabric-and-shielded-vms.md#attestation-modes-in-the-guarded-fabric-solution) utilisé par l’infrastructure protégée. Il existe deux moyens, deux modes qui s’excluent mutuellement, par laquelle SGH peut mesurer constituant un hôte Hyper-V intègre. Lorsque vous initialisez SGH, vous devez choisir le mode :  

- L’attestation de clé hôte ou le mode de clé, qui est moins sécurisé, mais plus faciles à adopter  
- L’attestation basée sur le module de plateforme sécurisée ou le mode de module de plateforme sécurisée, est plus sûr mais nécessite plus de configuration et de matériel spécifique

Si nécessaire, vous pouvez déployer en mode de clé à l’aide d’hôtes Hyper-V existants qui ont été mis à niveau vers Windows Server 2019 Datacenter edition et convertir ensuite le mode plus sécurisé de module de plateforme sécurisée lors de la prise en charge du matériel de serveur (y compris le TPM 2.0) est disponible. 

Maintenant que vous savez quelles sont les éléments, nous allons étudier un exemple du modèle de déploiement.

## <a name="how-to-get-from-a-current-hyper-v-fabric-to-a-guarded-fabric"></a>Obtention d’une infrastructure Hyper-V en cours dans une structure protégée

Imaginons que ce scénario, vous avez une infrastructure Hyper-V existant, comme Contoso.com dans l’image suivante, et que vous souhaitez créer une infrastructure Windows Server 2016 service Guardian.

![Fabric Hyper-V existant](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-existing-hyper-v.png)

## <a name="step-1-deploy-the-hyper-v-hosts-running-windows-server-2016"></a>Étape 1 : Déployer les hôtes Hyper-V exécutant Windows Server 2016 

Les hôtes Hyper-V doivent exécuter Windows Server 2016 Datacenter edition ou version ultérieure. Si vous mettez à niveau des ordinateurs hôtes, vous pouvez [mise à niveau](https://technet.microsoft.com/windowsserver/dn527667.aspx) Édition Standard vers Datacenter edition.

![Mettre à niveau les ordinateurs hôtes Hyper-V](../../security/media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-one-upgrade-hyper-v.png)

## <a name="step-2-deploy-the-host-guardian-service-hgs"></a>Étape 2 : Déployer le Service de Guardian hôte (SGH)

Puis installez le rôle de serveur SGH et déployer en tant que d’un cluster à trois nœuds, comme dans l’exemple relecloud.com dans l’image suivante. Cela nécessite trois applets de commande PowerShell :

- Pour ajouter le rôle SGH, utilisez `Install-WindowsFeature` 
- Pour installer le service SGH, utilisez `Install-HgsServer` 
- Pour initialiser le service SGH avec le mode d’attestation choisi, utilisez `Initialize-HgsServer` 

Si vos serveurs Hyper-V existants ne répondent pas à la configuration requise pour le mode de module de plateforme sécurisée (par exemple, ils n’ont pas TPM 2.0), vous pouvez initialiser SGH à l’aide d’administrateur d’une attestation (mode AD), ce qui nécessite une approbation Active Directory avec le domaine de fabric. 

Dans notre exemple, supposons que Contoso initialement se déploie en mode AD afin de répondre immédiatement aux exigences de conformité et des plans convertir l’attestation plus sécurisée en fonction du module de plateforme sécurisée après que le matériel de serveur approprié peut être achetée. 

![Installer SGH](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-two-deploy-hgs.png)

## <a name="step-3-extract-identities-hardware-baselines-and-code-integrity-policies"></a>Étape 3 : Extraire les identités, les lignes de base de matériel et les stratégies d’intégrité du code

Le processus d’extraction des identités à partir d’ordinateurs hôtes Hyper-V dépend du mode d’attestation utilisé.

Pour le mode AD, l’ID de l’hôte est son compte d’ordinateur joint au domaine, qui doit être un membre d’un groupe de sécurité désigné dans le domaine de fabric.
Appartenance au groupe désigné est la seule détermination si l’hôte est intègre ou non. 

Dans ce mode, l’administrateur de l’infrastructure est uniquement responsable de l’intégrité des hôtes Hyper-V. Étant donné que SGH joue aucun rôle afin de décider ce qui est ou n’est pas autorisé à exécuter, les logiciels malveillants et les débogueurs fonctionnera comme prévu. 

Toutefois, les débogueurs qui tentent de s’attacher directement à un processus (par exemple, WinDbg.exe) sont bloquées pour les machines virtuelles protégées, car le processus de travail de la machine virtuelle (VMWP.exe) est une lumière de processus protégé (PPL).
Autres techniques de débogage, tels que ceux utilisés par LiveKd.exe, ne sont pas bloqués. Contrairement aux machines virtuelles protégées, le processus de travail pour les machines virtuelles de chiffrement pris en charge ne s’exécute pas comme une bibliothèque de modèles parallèles pour les débogueurs traditionnels comme WinDbg.exe continueront à fonctionner normalement.

Autrement dit, les étapes de validation rigoureux utilisées pour le mode de module de plateforme sécurisée ne sont pas utilisés pour le mode AD en aucune façon.

Pour le mode de module de plateforme sécurisée, trois choses sont requises : 

1.  Un _publique de type EK_ (ou _EKpub_) à partir de l’interface TPM 2.0 sur chaque hôte Hyper-V. Pour capturer la EKpub, utilisez `Get-PlatformIdentifier`. 
2.  Un _planifié de matériel_. Si chacun de vos hôtes Hyper-V est identique, puis une ligne de base unique est que nécessaire. Si elles ne sont pas, vous aurez besoin un pour chaque classe de matériel. La ligne de base est sous la forme d’un fichier journal Trustworthy Computing Group ou TCGlog. Le TCGlog contient tout ce qui fait de l’hôte, du microprogramme UEFI, via le noyau, jusqu’où l’hôte est entièrement initialisé. Pour capturer la ligne de base du matériel, installez le rôle Hyper-V et la fonctionnalité de Support de Hyper-V Guardian hôte et utilisez `Get-HgsAttestationBaselinePolicy`. 
3.  Un _stratégie d’intégrité du Code_. Si chacun de vos hôtes Hyper-V est identique, puis une seule stratégie d’élément de configuration est que nécessaire. Si elles ne sont pas, vous aurez besoin un pour chaque classe de matériel. Windows Server 2016 et Windows 10 ont une nouvelle forme de mise en œuvre de stratégies d’élément de configuration appelé _appliquée par l’hyperviseur de l’intégrité de Code (HVCI)_. HVCI met en œuvre fort et s’assure qu’un hôte est uniquement autorisé à exécuter des fichiers binaires d’un administrateur approuvé a autorisé à s’exécuter. Ces instructions sont encapsulées dans une stratégie d’intégrité qui est ajoutée à SGH. SGH mesure la stratégie d’intégrité de chaque ordinateur hôte avant qu’ils sont autorisés à s’exécuter des machines virtuelles protégées. Pour capturer une stratégie d’intégrité, utilisez `New-CIPolicy`. La stratégie doit alors être convertie à sa forme binaire à l’aide `ConvertFrom-CIPolicy`.

![Extraire des identités, de base et stratégie d’intégrité](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-three-extract-identity-baseline-ci-policy.png)

C’est tout, la structure protégée est générée, en termes de l’infrastructure pour l’exécuter.  
Vous pouvez désormais créer un disque de modèle de machine virtuelle protégé et un fichier de données de protection protégé par conséquent, les machines virtuelles peuvent être approvisionnées simplement et en toute sécurité. 

## <a name="step-4-create-a-template-for-shielded-vms"></a>Étape 4 : Créer un modèle pour les machines virtuelles protégées

Un modèle de machine virtuelle protégée protège les disques de modèle en créant une signature du disque à un point connu digne de confiance dans le temps. 
Si le disque de modèle est plus tard infecté par un logiciel malveillant, sa signature varient de modèle d’origine qui est détecté par la machine virtuelle sécurisée protégée processus d’approvisionnement. 
Disques de modèle protégés sont créées en exécutant la **Assistant de création du disque de modèle protégée** ou `Protect-TemplateDisk` par rapport à un disque de modèle Normal. 

Chacun est inclus avec le **outils de machine virtuelle protégée** fonctionnalité dans le [distant Server Administration Tools pour Windows 10](https://www.microsoft.com/download/details.aspx?id=45520).
Après avoir téléchargé le serveur distant, exécutez cette commande pour installer le **outils de machine virtuelle protégée** fonctionnalité :

```powershell
Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
```

Un administrateur digne de confiance, telles que l’administrateur de structure ou le propriétaire de la machine virtuelle, doit un certificat (souvent fourni par un fournisseur de services d’hébergement) pour signer le modèle de disque VHDX. 

![Assistant de disque de modèle protégée](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-shielded-template-wizard.png)

La signature de disque est calculée sur la partition du système d’exploitation du disque virtuel.
Si quelque chose change sur la partition du système d’exploitation, la signature vont également changer.
Cela permet aux utilisateurs de fortement identifier quels disques ils font confiance en spécifiant la signature appropriée.

Examinez le [exigences relatives au disque modèle](guarded-fabric-create-a-shielded-vm-template.md) avant de commencer. 

## <a name="step-5-create-a-shielding-data-file"></a>Étape 5 : Créer un fichier de données de protection 

Un fichier de données de protection, également appelé fichier .pdk, capture des informations sensibles sur l’ordinateur virtuel, telles que le mot de passe administrateur. 

![Les données de protection](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-five-create-shielding-data.png)

Le fichier de données de protection inclut également le paramètre de stratégie de sécurité pour la machine virtuelle protégée. Vous devez choisir une des deux stratégies de sécurité lorsque vous créez un fichier de données de protection :

- Protégées
   
    Option la plus sécurisée, ce qui élimine de nombreux vecteurs d’attaque d’administration.

- Chiffrement pris en charge

    Un plus petit niveau de protection qui toujours offre les avantages de la conformité de la possibilité de chiffrer une machine virtuelle, mais autorise les administrateurs Hyper-V effectuer des opérations comme utiliser la connexion de console de machine virtuelle et PowerShell Direct. 

    ![Nouveau chiffrement pris en charge les machines virtuelles](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-shielded-vm.png)

Vous pouvez ajouter les éléments d’administration facultatifs comme VMM ou Windows Azure Pack. Si vous souhaitez créer une machine virtuelle sans installer ces éléments, consultez [étape par étape – Création de groupes de données dotées sans VMM](https://blogs.technet.microsoft.com/datacentersecurity/2016/06/06/step-by-step-creating-shielded-vms-without-vmm/).

## <a name="step-6-create-a-shielded-vm"></a>Étape 6 : Créer une machine virtuelle protégée

Création des machines virtuelles protégées diffère très peu de choses à partir des machines virtuelles régulières. Dans Windows Azure Pack, l’expérience est encore plus facile que la création d’une machine virtuelle standard, car vous devez uniquement fournir un nom de la protection du fichier de données (contenant le reste des informations de spécialisation) et le réseau d’ordinateurs virtuels. 

![Machine virtuelle protégée dans Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-vm-config.png)

## <a name="next-step"></a>Étape suivante

>[!div class="nextstepaction"]
[Conditions préalables SGH](guarded-fabric-prepare-for-hgs.md)
