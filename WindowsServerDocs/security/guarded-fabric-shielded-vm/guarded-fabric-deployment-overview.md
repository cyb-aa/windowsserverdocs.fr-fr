---
title: Démarrage rapide pour le déploiement d’une infrastructure protégée
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: e060e052-39a0-4154-90bb-b97cc6dde68e
manager: dongill
author: justinha
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: e2b8400fc7b7f0e01e000fcb2f6472bdb4059ac8
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949804"
---
# <a name="quick-start-for-guarded-fabric-deployment"></a>Démarrage rapide pour le déploiement d’une infrastructure protégée

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique explique ce qu’est une structure protégée, ses spécifications et un résumé du processus de déploiement. Pour connaître les étapes de déploiement détaillées, consultez [déploiement du service Guardian hôte pour les hôtes service Guardian et les machines virtuelles](https://technet.microsoft.com/windows-server-docs/security/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview)protégées.

Vous préférez une vidéo ? Reportez-vous au cours Microsoft Virtual Academy déploiement de machines virtuelles protégées [et d’une structure protégée avec Windows Server 2016](https://mva.microsoft.com/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474).

## <a name="what-is-a-guarded-fabric"></a>Qu’est-ce qu’une infrastructure protégée ?

Une _infrastructure protégée_ est une infrastructure Hyper-V Windows Server 2016 qui permet de protéger les charges de travail des locataires contre l’inspection, le vol et la falsification des logiciels malveillants en cours d’exécution sur l’ordinateur hôte, ainsi que des administrateurs système. Ces charges de travail de locataire virtualisées, protégées au repos et en transit, sont appelées _machines virtuelles_protégées. 

## <a name="what-are-the-requirements-for-a-guarded-fabric"></a>Quelles sont les conditions requises pour une infrastructure protégée

La configuration requise pour une infrastructure protégée est la suivante :

- **Emplacement permettant d’exécuter des machines virtuelles protégées qui sont exemptes de logiciels malveillants.**

    Il s’agit des _hôtes service Guardian_. 
    Les hôtes service Guardian sont des ordinateurs hôtes Hyper-V Windows Server 2016 Datacenter Edition qui peuvent exécuter des machines virtuelles protégées uniquement s’ils peuvent prouver qu’ils s’exécutent dans un État approuvé connu à une autorité externe appelée service Guardian hôte (SGH). 
    Le SGH est un nouveau rôle serveur dans Windows Server 2016 et il est généralement déployé en tant que cluster à trois nœuds. 

- **Un moyen de vérifier un ordinateur hôte est dans un état sain.**

    Le SGH effectue l' _attestation_, où il mesure l’intégrité des hôtes service Guardian.

- **Processus permettant de libérer en toute sécurité des clés sur des hôtes sains.**

    Le SGH exécute la _protection de clé et la version de clé_, où il libère les clés sur les hôtes sains.

- **Outils de gestion pour automatiser l’approvisionnement et l’hébergement sécurisés des machines virtuelles protégées.**

    Si vous le souhaitez, vous pouvez ajouter ces outils de gestion à une infrastructure protégée :

    - System Center 2016 Virtual Machine Manager (VMM). VMM est recommandé, car il fournit des outils de gestion supplémentaires au-delà de ce que vous pouvez faire en utilisant uniquement les applets de commande PowerShell fournies avec Hyper-V et les charges de travail de l’infrastructure protégée.
    - SPF (System Center 2016 Service Provider Foundation). Il s’agit d’une couche d’API entre Windows Azure Pack et VMM, et d’une condition préalable à l’utilisation de Windows Azure Pack.
    - Windows Azure Pack fournit une bonne interface Web graphique pour gérer une infrastructure protégée et des machines virtuelles dotées d’une protection maximale. 

Dans la pratique, une décision doit être prise à l’avance : le [mode d’attestation](guarded-fabric-and-shielded-vms.md#attestation-modes-in-the-guarded-fabric-solution) utilisé par l’infrastructure protégée. Il existe deux moyens : deux modes s’excluant mutuellement, par lesquels SGH peut mesurer qu’un ordinateur hôte Hyper-V est sain. Lorsque vous initialisez SGH, vous devez choisir le mode :  

- L’attestation de clé hôte, ou le mode clé, est moins sécurisée, mais plus facile à adopter  
- L’attestation basée sur le module de plateforme sécurisée (TPM), ou le mode TPM, est plus sécurisée, mais nécessite davantage de configuration et de matériel spécifique

Si nécessaire, vous pouvez déployer en mode clé à l’aide d’ordinateurs hôtes Hyper-V existants qui ont été mis à niveau vers Windows Server 2019 Datacenter Edition, puis passer au mode TPM le plus sécurisé lors de la prise en charge du matériel serveur (y compris TPM 2,0). 

Maintenant que vous savez ce que sont les éléments, examinons un exemple du modèle de déploiement.

## <a name="how-to-get-from-a-current-hyper-v-fabric-to-a-guarded-fabric"></a>Comment passer d’une infrastructure Hyper-V actuelle à une infrastructure protégée

Imaginez ce scénario : vous disposez d’une infrastructure Hyper-V existante, comme Contoso.com dans l’image suivante, et vous souhaitez créer une infrastructure Windows Server 2016 protégée.

![Infrastructure Hyper-V existante](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-existing-hyper-v.png)

## <a name="step-1-deploy-the-hyper-v-hosts-running-windows-server-2016"></a>Étape 1 : déployer les ordinateurs hôtes Hyper-V exécutant Windows Server 2016 

Les hôtes Hyper-V doivent exécuter Windows Server 2016 Datacenter Edition ou une version ultérieure. Si vous mettez à niveau des ordinateurs hôtes, vous pouvez [effectuer la mise à niveau](https://technet.microsoft.com/windowsserver/dn527667.aspx) de l’édition standard vers Datacenter Edition.

![Mettre à niveau les ordinateurs hôtes Hyper-V](../../security/media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-one-upgrade-hyper-v.png)

## <a name="step-2-deploy-the-host-guardian-service-hgs"></a>Étape 2 : déployer le service Guardian hôte (SGH)

Installez ensuite le rôle serveur SGH et déployez-le en tant que cluster à trois nœuds, comme l’exemple relecloud.com dans l’image suivante. Cela requiert trois applets de commande PowerShell :

- Pour ajouter le rôle SGH, utilisez `Install-WindowsFeature` 
- Pour installer le SGH, utilisez `Install-HgsServer` 
- Pour initialiser le SGH avec le mode d’attestation choisi, utilisez `Initialize-HgsServer` 

Si vos serveurs Hyper-V existants ne remplissent pas les conditions préalables pour le mode TPM (par exemple, ils ne disposent pas de TPM 2,0), vous pouvez initialiser SGH à l’aide d’une attestation basée sur l’administrateur (mode AD), qui nécessite une approbation Active Directory avec le domaine de l’infrastructure. 

Dans notre exemple, supposons que contoso déploie initialement en mode AD pour répondre immédiatement aux exigences de conformité et envisage de passer à l’attestation TPM plus sécurisée après l’achat d’un matériel serveur approprié. 

![Installer SGH](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-two-deploy-hgs.png)

## <a name="step-3-extract-identities-hardware-baselines-and-code-integrity-policies"></a>Étape 3 : extraire les identités, les bases de référence du matériel et les stratégies d’intégrité du code

Le processus d’extraction des identités à partir d’ordinateurs hôtes Hyper-V dépend du mode d’attestation utilisé.

Pour le mode AD, l’ID de l’hôte est son compte d’ordinateur joint au domaine, qui doit être membre d’un groupe de sécurité désigné dans le domaine de la structure.
L’appartenance au groupe désigné est la seule détermination du fait que l’ordinateur hôte est sain ou non. 

Dans ce mode, l’administrateur de l’infrastructure est uniquement responsable de la vérification de l’intégrité des ordinateurs hôtes Hyper-V. Dans la mesure où SGH ne joue aucun rôle dans le choix de ce qui est ou non autorisé à s’exécuter, les programmes malveillants et les débogueurs fonctionneront comme prévu. 

Toutefois, les débogueurs qui tentent de s’attacher directement à un processus (par exemple, WinDbg. exe) sont bloqués pour les machines virtuelles protégées, car le processus de travail de la machine virtuelle (VMWP. exe) est un voyant de processus protégé (PPL).
D’autres techniques de débogage, telles que celles utilisées par LiveKd. exe, ne sont pas bloquées. Contrairement aux machines virtuelles dotées d’une protection maximale, le processus de travail pour les machines virtuelles prises en charge par le chiffrement ne s’exécute pas en tant que PPL. ainsi, les débogueurs traditionnels comme WinDbg. exe continuent à fonctionner normalement

Autrement dit, les étapes de validation rigoureuses utilisées pour le mode TPM ne sont pas utilisées en mode AD.

Pour le mode TPM, trois éléments sont nécessaires : 

1.  Une _clé de type EK public_ (ou _EKpub_) du module de plateforme sécurisée 2,0 sur chaque hôte Hyper-V. Pour capturer le EKpub, utilisez `Get-PlatformIdentifier`. 
2.  Une _ligne de base matérielle_. Si chacun de vos ordinateurs hôtes Hyper-V est identique, une seule ligne de base suffit. Si ce n’est pas le cas, vous en aurez besoin pour chaque classe de matériel. La ligne de base se présente sous la forme d’un fichier journal de groupe Trustworthy Computing ou TCGlog. Le TCGlog contient tout ce que l’hôte a fait à partir du microprogramme UEFI, via le noyau, jusqu’à l’endroit où l’hôte est entièrement amorcé. Pour capturer la ligne de base matérielle, installez le rôle Hyper-V et la fonctionnalité de prise en charge d’Hyper-V Guardian hôte et utilisez `Get-HgsAttestationBaselinePolicy`. 
3.  _Stratégie d’intégrité du code_. Si chacun de vos ordinateurs hôtes Hyper-V est identique, il vous suffit d’une seule stratégie CI. Si ce n’est pas le cas, vous en aurez besoin pour chaque classe de matériel. Windows Server 2016 et Windows 10 disposent tous les deux d’une nouvelle forme de mise en application pour les stratégies d’élément de configuration, appelées _intégrité du code stratégie hvci (hyperviseur)_ . STRATÉGIE HVCI fournit une mise en œuvre forte et garantit qu’un hôte est autorisé uniquement à exécuter des fichiers binaires qu’un administrateur approuvé lui a autorisé à exécuter. Ces instructions sont encapsulées dans une stratégie CI qui est ajoutée à SGH. SGH mesure chaque stratégie CI de l’hôte avant qu’il ne soit autorisé à exécuter des machines virtuelles protégées. Pour capturer une stratégie CI, utilisez `New-CIPolicy`. La stratégie doit ensuite être convertie au format binaire à l’aide de `ConvertFrom-CIPolicy`.

![Extraire les identités, la ligne de base et la stratégie CI](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-three-extract-identity-baseline-ci-policy.png)

C’est tout : l’infrastructure protégée est créée, en termes d’infrastructure pour l’exécuter.  
Vous pouvez maintenant créer un disque de modèle de machine virtuelle protégé et un fichier de données de protection afin que les machines virtuelles protégées puissent être approvisionnées simplement et en toute sécurité. 

## <a name="step-4-create-a-template-for-shielded-vms"></a>Étape 4 : créer un modèle pour les machines virtuelles protégées

Un modèle de machine virtuelle protégée protège les disques de modèle en créant une signature du disque à un point de confiance connu dans le temps. 
Si le disque de modèle est par la suite infecté par un programme malveillant, sa signature sera différente de celle du modèle d’origine, qui sera détectée par le processus d’approvisionnement de la machine virtuelle protégée. 
Pour créer des disques de modèle protégés, exécutez l' **Assistant de création de disque de modèle protégé** ou `Protect-TemplateDisk` sur un disque de modèle normal. 

Chaque est inclus avec la fonctionnalité **outils de machine virtuelle protégée** dans le [Outils d’administration de serveur distant pour Windows 10](https://www.microsoft.com/download/details.aspx?id=45520).
Une fois que vous avez téléchargé les outils d’installation de la machine virtuelle, exécutez la commande suivante pour installer la fonctionnalité **outils de machines virtuelles protégées** :

```powershell
Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
```

Un administrateur Trustworthy, tel que l’administrateur de l’infrastructure ou le propriétaire de la machine virtuelle, aura besoin d’un certificat (souvent fourni par un fournisseur de services d’hébergement) pour signer le disque de modèle VHDX. 

![Assistant disque de modèle protégé](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-shielded-template-wizard.png)

La signature de disque est calculée sur la partition du système d’exploitation du disque virtuel.
Si une modification est apportée à la partition du système d’exploitation, la signature change également.
Cela permet aux utilisateurs d’identifier fortement les disques qu’ils approuvent en spécifiant la signature appropriée.

Avant de commencer, passez en revue les [conditions requises en matière de disque de modèle](guarded-fabric-create-a-shielded-vm-template.md) . 

## <a name="step-5-create-a-shielding-data-file"></a>Étape 5 : créer un fichier de données de protection 

Un fichier de données de protection, également connu sous le nom de fichier. PDK, capture des informations sensibles sur l’ordinateur virtuel, telles que le mot de passe de l’administrateur. 

![Données de protection](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-five-create-shielding-data.png)

Le fichier de données de protection comprend également le paramètre de stratégie de sécurité pour la machine virtuelle protégée. Vous devez choisir l’une des deux stratégies de sécurité lorsque vous créez un fichier de données de protection :

- Protégées
   
    L’option la plus sécurisée, qui élimine de nombreux vecteurs d’attaque administratifs.

- Chiffrement pris en charge

    Un niveau de protection inférieur qui offre toujours les avantages de la conformité pour pouvoir chiffrer une machine virtuelle, mais permet aux administrateurs Hyper-V d’effectuer des opérations telles que la connexion à la console de machine virtuelle et PowerShell direct. 

    ![Nouvelle machine virtuelle prise en charge par le chiffrement](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-shielded-vm.png)

Vous pouvez ajouter des éléments de gestion facultatifs tels que VMM ou Windows Azure Pack. Si vous souhaitez créer une machine virtuelle sans installer ces éléments, consultez [étape par étape : création de machines virtuelles protégées sans VMM](https://blogs.technet.microsoft.com/datacentersecurity/2016/06/06/step-by-step-creating-shielded-vms-without-vmm/).

## <a name="step-6-create-a-shielded-vm"></a>Étape 6 : créer une machine virtuelle dotée d’une protection maximale

La création d’ordinateurs virtuels protégés diffère très peu des machines virtuelles standard. Dans Windows Azure Pack, l’expérience est encore plus simple que la création d’une machine virtuelle normale, car il vous suffit de fournir un nom, un fichier de données de protection (contenant le reste des informations de spécialisation) et le réseau d’ordinateurs virtuels. 

![Nouvelle machine virtuelle protégée dans Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-vm-config.png)

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Conditions préalables du SGH](guarded-fabric-prepare-for-hgs.md)
