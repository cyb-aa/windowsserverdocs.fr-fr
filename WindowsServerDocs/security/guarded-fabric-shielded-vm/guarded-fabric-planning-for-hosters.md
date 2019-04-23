---
title: Structure protégée et machines virtuelles protégées Planning Guide pour les hébergeurs
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 854defc8-99f8-4573-82c0-f484e0785859
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f280fbe682ebf706ce6ea5b53ea8af5e6f39d75d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857810"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-hosters"></a>Structure protégée et machines virtuelles protégées Planning Guide pour les hébergeurs

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique couvre les décisions de planification qui devront être apportées à autorisez protégée des ordinateurs virtuels pour s’exécuter sur votre structure. Si vous mettez à niveau une infrastructure Hyper-V existante ou créez une nouvelle structure, en cours d’exécution protégées machines virtuelles se compose de deux composants principaux :

- Le Service Guardian hôte (SGH) fournit une protection d’attestation et de clé afin que vous pouvez vous assurer que les machines virtuelles protégées seront exécutera uniquement sur les ordinateurs hôtes Hyper-V approuvés et sains. 
- Approuvé et intègres des hôtes Hyper-V sur lequel les machines virtuelles protégées (et ceux des machines virtuelles) peuvent s’exécuter, ceux-ci sont connus comme hôtes service Guardian.

![SGH et un hôte service Guardian](../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-plus-host-diagram-basic.png)

## <a name="decision-1-trust-level-in-the-fabric"></a>Décision #1 : Niveau dans l’infrastructure de confiance

Comment implémenter le Service Guardian hôte et les hôtes Hyper-V dépend principalement force l’approbation que vous souhaitez pour obtenir dans votre infrastructure. La force de confiance est régie par le mode d’attestation. Il existe deux options mutuellement exclusives :

1. Attestation approuvée par le module de plateforme sécurisée

    Si votre objectif est de vous aider à protéger les machines virtuelles à partir des administrateurs malveillants ou d’une structure compromise, vous allez utiliser l’attestation approuvée par le module de plateforme sécurisée. Cette option fonctionne bien pour plusieurs locataires scénarios d’hébergement, ainsi que pour les ressources de valeur élevée dans les environnements d’entreprise, telles que les contrôleurs de domaine ou serveurs de contenu telles que SQL ou SharePoint.
    Stratégies d’intégrité (HVCI) du code protégé par l’hyperviseur sont mesurées et leur validité appliquée par SGH avant que l’hôte est autorisé à exécuter des machines virtuelles protégées. 

2. Attestation de clé hôte

    Si vos besoins sont essentiellement motivées par conformité nécessite virtual machines chiffrée à la fois au repos et en cours, puis vous allez utiliser l’attestation de clé hôte. Cette option fonctionne bien pour les centres de données à usage général dans lequel vous êtes familiarisé avec Hyper-V hôte et fabric les administrateurs ayant un accès pour les systèmes d’exploitation invités d’ordinateurs virtuels pour les opérations et maintenance quotidienne. 

    Dans ce mode, l’administrateur de l’infrastructure est uniquement responsable de l’intégrité des hôtes Hyper-V. 
    Étant donné que SGH joue aucun rôle afin de décider ce qui est ou n’est pas autorisé à exécuter, les logiciels malveillants et les débogueurs fonctionnera comme prévu. 
    
    Toutefois, les débogueurs qui tentent de s’attacher directement à un processus (par exemple, WinDbg.exe) sont bloquées pour les machines virtuelles protégées, car le processus de travail de la machine virtuelle (VMWP.exe) est une lumière de processus protégé (PPL). 
    Autres techniques de débogage, tels que ceux utilisés par LiveKd.exe, ne sont pas bloqués. 
    Contrairement aux machines virtuelles protégées, le processus de travail pour les machines virtuelles de chiffrement pris en charge ne s’exécute pas comme une bibliothèque de modèles parallèles pour les débogueurs traditionnels comme WinDbg.exe continueront à fonctionner normalement.

    Un mode d’attestation similaire nommée attestation approuvée par l’administrateur (basée sur Active Directory) est déconseillée à compter de Windows Server 2019. 

Le niveau de confiance que vous choisissez détermine la configuration matérielle requise pour vos hôtes Hyper-V, ainsi que les stratégies que vous appliquez sur l’infrastructure. Si nécessaire, vous pouvez déployer votre infrastructure protégée à l’aide de matériel existant et attestation approuvée par l’administrateur et convertir à l’attestation approuvée par le module de plateforme sécurisée lorsque le matériel a été mis à niveau et que vous avez besoin renforcer la sécurité de l’infrastructure.

## <a name="decision-2-existing-hyper-v-fabric-versus-a-new-separate-hyper-v-fabric"></a>Décision #2 : Fabric Hyper-V existant par rapport à une autre infrastructure Hyper-V nouveau

Si vous avez une infrastructure existante (Hyper-V ou autre), il est très probable que vous pouvez l’utiliser pour exécuter des machines virtuelles protégées, ainsi que ceux des machines virtuelles. Certains clients choisissent d’intégrer des machines virtuelles protégées leurs outils existants et les infrastructures tandis que d’autres séparent l’infrastructure pour des raisons professionnelles.

## <a name="hgs-admin-planning-for-the-host-guardian-service"></a>Administrateur SGH planification pour le Service Guardian hôte

Déployer le Service Guardian hôte (SGH) dans un environnement hautement sécurisé, que ce soit sur un serveur physique dédié, une machine virtuelle protégée, une machine virtuelle sur un hôte Hyper-V isolé (séparés à partir de l’infrastructure qu’il protège), ou séparées logiquement à l’aide d’un autre Azure abonnement.   

| Domaine | Détails |
|------|---------|
| Configuration requise pour l’installation | <ul><li>Un seul serveur (cluster à trois nœuds recommandé pour la haute disponibilité)</li><li>De secours, au moins deux serveurs SGH sont requis</li><li>Serveurs peuvent être virtuels ou physiques (serveurs physiques avec TPM 2.0 recommandé ; Module de plateforme sécurisée 1.2 également pris en charge)</li><li>Installation Server Core de Windows Server 2016 ou version ultérieure</li><li>Réseau d’une visibilité à l’infrastructure autorisant HTTP ou [configuration de secours](guarded-fabric-manage-branch-office.md#fallback-configuration)</li><li>Certificat HTTPS recommandé pour la validation de l’accès</li></ul> |
| Dimensionnement | Chaque nœud de serveur SGH de taille moyenne (8 core/4 Go) peut gérer 1 000 hôtes Hyper-V. |
| Gestion | Spécifier les personnes qui géreront SGH. Ils doivent être distincts à partir des administrateurs de structure. Pour la comparaison, SGH clusters peuvent être considérés de la même manière en tant qu’une autorité de certification (CA) en termes d’isolation d’administration, déploiement physique et niveau de sensibilité de sécurité global. |
| Répertoire actif du Service Guardian hôte | Par défaut, SGH installe son propre Active Directory pour la gestion interne. Ceci est une forêt autonome autogérée et la configuration recommandée pour contribuer à isoler HGS à partir de votre infrastructure.<br><br>Si vous disposez déjà d’une forêt Active Directory disposant de privilèges élevés que vous utilisez pour l’isolation, vous pouvez utiliser cette forêt au lieu de la forêt par défaut de SGH. Il est important que SGH n’est pas joint à un domaine dans la même forêt que les hôtes Hyper-V ou des outils de gestion de votre infrastructure. Cela pourrait permettre à un administrateur de structure à mieux contrôler le SGH. |
| Récupération d'urgence | Il existe trois options :<br><ol><li>Installer un cluster séparé des SGH dans chaque centre de données et autoriser les machines virtuelles protégées à exécuter dans le réplica principal et les centres de données de sauvegarde. Cela évite d’avoir au cluster étendu sur un réseau WAN et vous permet d’isoler les machines virtuelles telles qu’elles s’exécutent uniquement dans leur site désigné.</li><li>Installez SGH sur un cluster étendu entre les centres de données deux (ou plus). Cela fournit la résilience si le réseau étendu tombe en panne, mais repousse les limites du clustering de basculement. Vous ne pouvez pas isoler les charges de travail à un seul site ; une machine virtuelle autorisée à s’exécuter dans un site peut exécuter sur n’importe quel autre.</li><li>Inscrire votre hôte Hyper-V avec un autre service HGS sous forme de basculement.</li></ol>Vous devez également sauvegarder chaque SGH en exportant sa configuration afin que vous puissiez toujours récupérer localement. Pour plus d’informations, consultez [Export-HgsServerState](https://technet.microsoft.com/library/mt652164.aspx) et [Import-HgsServerState](https://technet.microsoft.com/library/mt652168.aspx). |
| Clés d’hôte Service Guardian | Un Service de Guardian hôte utilise deux paires de clés asymétriques : une clé de chiffrement et une clé de signature, chacun représenté par un certificat SSL. Il existe deux options pour générer ces clés :<br><ol><li>Autorité de certification interne : vous pouvez générer ces clés à l’aide de votre infrastructure PKI interne. Cela est adapté à un environnement de centre de données.</li><li>Autorités de certification approuvée publiquement : utiliser un jeu de clés obtenues à partir d’une autorité de certification approuvée publiquement. Il s’agit de l’option que les hébergeurs doivent utiliser.</li></ol>Notez que bien qu’il soit possible d’utiliser des certificats auto-signés, il est déconseillé pour les scénarios de déploiement autres que des laboratoires de preuve de concept.<br><br>En plus des clés de SGH, un hébergeur peut utiliser « apportez votre propre clé, » où locataires peuvent fournir leurs propres clés afin que certains (ou l’ensemble) des clients puissent avoir leur propre clé SGH spécifique. Cette option est adaptée pour les hébergeurs qui peuvent fournir un processus hors-bande pour les clients de télécharger leurs clés. |
| Stockage de clés de Service Guardian hôte | Pour une sécurité maximale possible, nous recommandons que les clés de SGH sont créées et stockées exclusivement dans un HSM Hardware Security Module (). Si vous n’utilisez pas de modules de sécurité matériels, application de BitLocker sur les serveurs SGH est fortement recommandé. |

## <a name="fabric-admin-planning-for-guarded-hosts"></a>Planification d’un administrateur de fabric pour les hôtes service Guardian

| Domaine | Détails |
|------|---------|
| Matériel | <ul><li>Attestation de clé hôte : Vous pouvez utiliser n’importe quel matériel existant comme hôte service Guardian. Il existe quelques exceptions près (pour vous assurer que votre hôte peut utiliser les nouveaux mécanismes de sécurité à partir de Windows Server 2016, consultez [matériel Compatible avec la protection d’intégrité du Code basé sur la virtualisation Windows Server 2016](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).</li><li>Attestation approuvée par le module de plateforme sécurisée : Vous pouvez utiliser n’importe quel matériel qui a le [Qualification supplémentaire de matériel Assurance](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#system-server-assurance) tant qu’il est correctement configuré (voir [des configurations de serveur qui sont compatibles avec les machines virtuelles protégées et basés sur la virtualisation protection de l’intégrité du code](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md) pour la configuration spécifique). Cela inclut le module de plateforme sécurisée, versions 2.0 et UEFI 2.3.1c et versions ultérieures.</li></ul> |
| Système d’exploitation | Nous vous recommandons d’utiliser l’option Server Core pour la système d’exploitation de l’hôte Hyper-V. |
| Impact sur les performances | Selon les tests de performances, nous prévoyons une environ 5 % densité-différence entre en cours d’exécution protégées des machines virtuelles et des machines virtuelles non protégées. Cela signifie que si un hôte Hyper-V donné peut exécuter 20 machines virtuelles de la non protégé, nous prévoyons qu’il peut exécuter des machines virtuelles protégées 19.<br><br>Veillez à vérifier le dimensionnement avec vos charges de travail classiques. Par exemple, il peut y avoir des valeurs hors norme avec orientée sur l’écriture d’e/s charges de travail intensives qui affecteront plus la différence de densité. |
| Éléments à prendre en compte en matière de filiale | À compter de Windows Server version 1709, vous pouvez spécifier une URL de secours pour un serveur SGH virtualisé en cours d’exécution localement en tant qu’une machine virtuelle protégée dans la succursale. L’URL de secours peut être utilisée lors de la succursale perd la connectivité aux serveurs SGH dans le centre de données. Sur les versions précédentes de Windows Server, un hôte Hyper-V en cours d’exécution dans une branche besoins de connectivité au Service Guardian hôte tension ou à la migration dynamique des machines virtuelles protégées. Pour plus d’informations, consultez [branche considérations sur office](guarded-fabric-manage-branch-office.md). |
