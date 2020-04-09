---
title: Guide de planification de l’infrastructure protégée et de la machine virtuelle protégée pour les hébergeurs
ms.prod: windows-server
ms.topic: article
ms.assetid: 854defc8-99f8-4573-82c0-f484e0785859
manager: dongill
author: nirb-ms
ms.author: nirb
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2e64f8a43318f10db3bfcb604adcef4b0bdc9837
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856512"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-hosters"></a>Guide de planification de l’infrastructure protégée et de la machine virtuelle protégée pour les hébergeurs

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit les décisions de planification qui doivent être prises pour permettre l’exécution de machines virtuelles protégées sur votre infrastructure. Que vous procédiez à une mise à niveau d’une infrastructure Hyper-V existante ou que vous créez une nouvelle infrastructure, l’exécution de machines virtuelles protégées se compose de deux composants principaux :

- Le service Guardian hôte (SGH) fournit une attestation et une protection de clé afin que vous puissiez vous assurer que les machines virtuelles protégées s’exécuteront uniquement sur les hôtes Hyper-V approuvés et intègres. 
- Hôtes Hyper-V approuvés et intègres sur lesquels des machines virtuelles protégées (et des machines virtuelles régulières) peuvent s’exécuter ; ce sont des hôtes service Guardian.

![SGH et hôte service Guardian](../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-plus-host-diagram-basic.png)

## <a name="decision-1-trust-level-in-the-fabric"></a>#1 de décision : niveau de confiance dans l’infrastructure

La façon dont vous implémentez le service Guardian hôte et les ordinateurs hôtes Hyper-V protégés dépend principalement de la force de confiance que vous cherchez à atteindre dans votre infrastructure. Le niveau de confiance est régi par le mode attestation. Il existe deux options mutuellement exclusives :

1. Attestation approuvée par le module de plateforme sécurisée

    Si votre objectif est de protéger les machines virtuelles contre les administrateurs malveillants ou une infrastructure compromise, vous utiliserez l’attestation approuvée par le module de plateforme sécurisée (TPM). Cette option fonctionne bien pour les scénarios d’hébergement mutualisés, ainsi que pour les ressources de grande valeur dans les environnements d’entreprise, tels que les contrôleurs de domaine ou les serveurs de contenu tels que SQL ou SharePoint.
    Les stratégies d’intégrité du code protégé par hyperviseur (stratégie HVCI) sont mesurées et leur validité est appliquée par SGH avant que l’hôte soit autorisé à exécuter des machines virtuelles protégées. 

2. Attestation de clé hôte

    Si vos exigences sont principalement pilotées par une conformité nécessitant le chiffrement des machines virtuelles au repos et en cours, vous allez utiliser l’attestation de clé hôte. Cette option fonctionne bien pour les centres de résultats à usage général, dans lesquels vous êtes familiarisé avec les administrateurs de l’infrastructure et de l’hôte Hyper-V qui ont accès aux systèmes d’exploitation invités des machines virtuelles pour la maintenance et les opérations quotidiennes. 

    Dans ce mode, l’administrateur de l’infrastructure est uniquement responsable de la vérification de l’intégrité des ordinateurs hôtes Hyper-V. 
    Dans la mesure où SGH ne joue aucun rôle dans le choix de ce qui est ou non autorisé à s’exécuter, les programmes malveillants et les débogueurs fonctionneront comme prévu. 
    
    Toutefois, les débogueurs qui tentent de s’attacher directement à un processus (par exemple, WinDbg. exe) sont bloqués pour les machines virtuelles protégées, car le processus de travail de la machine virtuelle (VMWP. exe) est un voyant de processus protégé (PPL). 
    D’autres techniques de débogage, telles que celles utilisées par LiveKd. exe, ne sont pas bloquées. 
    Contrairement aux machines virtuelles dotées d’une protection maximale, le processus de travail pour les machines virtuelles prises en charge par le chiffrement ne s’exécute pas en tant que PPL. ainsi, les débogueurs traditionnels comme WinDbg. exe continuent à fonctionner normalement

    Un mode d’attestation similaire nommé attestation approuvée par l’administrateur (basée sur Active Directory) est déconseillé à partir de Windows Server 2019. 

Le niveau de confiance que vous choisissez détermine la configuration matérielle requise pour vos ordinateurs hôtes Hyper-V, ainsi que les stratégies que vous appliquez sur l’infrastructure. Si nécessaire, vous pouvez déployer votre infrastructure protégée à l’aide du matériel existant et de l’attestation approuvée par l’administrateur, puis la convertir en attestation approuvée par le module de plateforme sécurisée (TPM) lorsque le matériel a été mis à niveau et que vous devez renforcer la sécurité de l’infrastructure.

## <a name="decision-2-existing-hyper-v-fabric-versus-a-new-separate-hyper-v-fabric"></a>#2 de décision : structure Hyper-V existante et nouvelle structure Hyper-V distincte

Si vous disposez d’une infrastructure existante (Hyper-V ou autre), il est très probable que vous pouvez l’utiliser pour exécuter des machines virtuelles protégées avec des machines virtuelles standard. Certains clients choisissent d’intégrer des machines virtuelles protégées dans leurs outils et Fabrics existants, tandis que d’autres séparent l’infrastructure pour des raisons commerciales.

## <a name="hgs-admin-planning-for-the-host-guardian-service"></a>Planification de l’administrateur SGH pour le service Guardian hôte

Déployez le service Guardian hôte (SGH) dans un environnement hautement sécurisé, qu’il s’agisse d’un serveur physique dédié, d’une machine virtuelle protégée, d’une machine virtuelle sur un ordinateur hôte Hyper-V isolé (séparé de l’infrastructure qu’il protège) ou d’un autre logiquement séparé à l’aide d’un autre abonnement Azure.   

| Zone | Détails |
|------|---------|
| Configuration requise pour l’installation | <ul><li>Un serveur (cluster à trois nœuds recommandé pour la haute disponibilité)</li><li>Pour le secours, au moins deux serveurs SGH sont requis</li><li>Les serveurs peuvent être de type virtuel ou physique (serveur physique avec module de plateforme sécurisée 2,0 recommandé ; TPM 1,2 également pris en charge)</li><li>Installation Server Core de Windows Server 2016 ou version ultérieure</li><li>Réseau de vue de l’infrastructure autorisant la configuration HTTP ou de [secours](guarded-fabric-manage-branch-office.md#fallback-configuration)</li><li>Certificat HTTPs recommandé pour la validation de l’accès</li></ul> |
| Dimensionnement | Chaque nœud de serveur SGH de taille moyenne (8 cœurs/4 Go) peut gérer des ordinateurs hôtes Hyper-V 1 000. |
| Gestion | Désigner des personnes spécifiques qui géreront le SGH. Ils doivent être distincts des administrateurs de l’infrastructure. À des fins de comparaison, les clusters SGH peuvent être considérés de la même manière qu’une autorité de certification en termes d’isolation administrative, de déploiement physique et de niveau global de sensibilité de sécurité. |
| Active Directory du service Guardian hôte | Par défaut, SGH installe son propre Active Directory interne pour la gestion. Il s’agit d’une forêt autonome et autogérée, qui est la configuration recommandée pour aider à isoler le SGH de votre infrastructure.<br><br>Si vous disposez déjà d’une forêt Active Directory à privilèges élevés que vous utilisez pour l’isolation, vous pouvez utiliser cette forêt au lieu de la forêt par défaut SGH. Il est important que SGH ne soit pas joint à un domaine dans la même forêt que les hôtes Hyper-V ou vos outils de gestion de structure. Cela pourrait permettre à un administrateur d’infrastructure d’obtenir le contrôle de SGH. |
| Récupération d'urgence | Trois options sont disponibles :<br><ol><li>Installez un cluster SGH distinct dans chaque centre de stockage et autorisez l’exécution des machines virtuelles protégées à la fois dans les centres de centres principaux et de sauvegarde. Cela évite d’avoir à étendre le cluster sur un réseau étendu (WAN) et vous permet d’isoler les machines virtuelles de sorte qu’elles s’exécutent uniquement sur leur site désigné.</li><li>Installez SGH sur un cluster étendu entre deux centres de session (ou plus). Cela garantit la résilience en cas de panne du réseau étendu, mais pousse les limites du clustering de basculement. Vous ne pouvez pas isoler les charges de travail sur un seul site. une machine virtuelle autorisée à s’exécuter sur un site peut s’exécuter sur n’importe quel autre.</li><li>Inscrivez votre hôte Hyper-V avec un autre serveur SGH comme basculement.</li></ol>Vous devez également sauvegarder chaque SGH en exportant sa configuration afin de pouvoir toujours récupérer localement. Pour plus d’informations, consultez [Export-HgsServerState](https://docs.microsoft.com/powershell/module/hgsserver/export-hgsserverstate) et [Import-HgsServerState](https://docs.microsoft.com/powershell/module/hgsserver/import-hgsserverstate). |
| Clés du service Guardian hôte | Un service Guardian hôte utilise deux paires de clés asymétriques (une clé de chiffrement et une clé de signature), chacune représentée par un certificat SSL. Il existe deux options pour générer ces clés :<br><ol><li>Autorité de certification interne : vous pouvez générer ces clés à l’aide de votre infrastructure PKI interne. Cela convient pour un environnement de centre de centres.</li><li>Autorités de certification de confiance publique : utilisez un ensemble de clés obtenues auprès d’une autorité de certification approuvée publiquement. Il s’agit de l’option que les hébergeurs doivent utiliser.</li></ol>Notez que bien qu’il soit possible d’utiliser des certificats auto-signés, il n’est pas recommandé pour les scénarios de déploiement autres que les laboratoires de validation technique.<br><br>En plus d’avoir des clés SGH, un hébergeur peut utiliser « apportez votre propre clé », où les locataires peuvent fournir leurs propres clés afin que certains ou l’ensemble des locataires puissent avoir leur propre clé SGH spécifique. Cette option est adaptée aux hébergeurs qui peuvent fournir un processus hors bande permettant aux locataires de charger leurs clés. |
| Stockage de clé du service Guardian hôte | Pour une sécurité maximale, nous recommandons que les clés SGH soient créées et stockées exclusivement dans un module de sécurité matériel (HSM). Si vous n’utilisez pas de modules HSM, il est fortement recommandé d’appliquer BitLocker sur les serveurs SGH. |

## <a name="fabric-admin-planning-for-guarded-hosts"></a>Planification de l’administration d’infrastructure pour les hôtes service Guardian

| Zone | Détails |
|------|---------|
| Matériel | <ul><li>Attestation de clé hôte : vous pouvez utiliser n’importe quel matériel existant comme hôte service Guardian. Il existe quelques exceptions (pour vous assurer que votre hôte peut utiliser de nouveaux mécanismes de sécurité à partir de Windows Server 2016, consultez [matériel compatible avec la protection de l’intégrité du code basée sur la virtualisation Windows server 2016](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).</li><li>Attestation approuvée par le module de plateforme sécurisée : vous pouvez utiliser n’importe quel matériel ayant la [qualification d’assurance matérielle supplémentaire](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#system-server-assurance) tant qu’il est correctement configuré (voir les [configurations de serveur compatibles avec les machines virtuelles protégées et la protection de l’intégrité du code basée sur la virtualisation pour une](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md) configuration spécifique). Cela comprend TPM 2,0 et UEFI version 2.3.1 c et versions ultérieures.</li></ul> |
| Système d’exploitation | Nous vous recommandons d’utiliser l’option Server Core pour le système d’exploitation hôte Hyper-V. |
| Implications sur les performances | En s’appuyant sur les tests de performances, nous prévoyons une différence de densité de 5% environ entre les machines virtuelles protégées et les machines virtuelles non protégées. Cela signifie que si un hôte Hyper-V donné peut exécuter 20 machines virtuelles non protégées, nous pensons qu’il peut exécuter 19 machines virtuelles protégées.<br><br>Veillez à vérifier le dimensionnement avec vos charges de travail standard. Par exemple, il peut y avoir des valeurs hors norme avec des charges de travail d’e/s orientées écriture intensives qui affectent davantage la différence de densité. |
| Éléments à prendre en compte en matière de filiale | À partir de la version 1709 de Windows Server, vous pouvez spécifier une URL de secours pour un serveur SGH virtualisé exécuté localement en tant que machine virtuelle protégée dans la filiale. L’URL de secours peut être utilisée lorsque la succursale perd la connectivité aux serveurs SGH dans le centre de l’entreprise. Dans les versions précédentes de Windows Server, un hôte Hyper-V qui s’exécute dans une filiale a besoin d’une connexion au service Guardian hôte pour mettre à niveau ou pour migrer dynamiquement des machines virtuelles protégées. Pour plus d’informations, consultez [considérations relatives aux succursales](guarded-fabric-manage-branch-office.md). |
