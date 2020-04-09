---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: Prise en charge de l'utilisation de la réplication Hyper-V pour les contrôleurs de domaine virtualisés
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d882fc5a8e519c461e17a7a82c8abfc6c16fbe9c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824502"
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>Prise en charge de l'utilisation de la réplication Hyper-V pour les contrôleurs de domaine virtualisés

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit la prise en charge de l'utilisation de la réplication Hyper-V pour répliquer un ordinateur virtuel qui s'exécute en tant que contrôleur de domaine. La réplication Hyper-V est une nouvelle fonctionnalité de la technologie Hyper-V qui fait son apparition dans Windows Server 2012 et qui fournit un mécanisme de réplication intégrée au niveau d'un ordinateur virtuel.  
  
La réplication Hyper-V réplique de manière asynchrone les ordinateurs virtuels sélectionnés d'un hôte Hyper-V principal vers l'hôte Hyper-V de réplication sur des liaisons locales (LAN) ou étendues (WAN). Une fois la réplication initiale effectuée, les changements qui surviennent ensuite sont répliqués selon un intervalle défini par l'administrateur.  
  
Le basculement peut être planifié ou non planifié. Un basculement planifié est lancé par un administrateur sur l'ordinateur virtuel principal. Tous les changements non répliqués sont copiés vers l'ordinateur virtuel de réplication pour éviter les pertes de données. Un basculement non planifié est lancé sur l'ordinateur virtuel de réplication en réponse à une défaillance inattendue de l'ordinateur virtuel principal. Il existe un risque de perte de données, car il n'est pas possible de transmettre les changements de l'ordinateur virtuel principal qui n'ont pas encore été répliqués.  
  
Pour plus d’informations sur le réplica Hyper-V, consultez [Vue d’ensemble de la réplication Hyper-V](https://technet.microsoft.com/library/jj134172.aspx) et [Déployer le réplica Hyper-V](https://technet.microsoft.com/library/jj134207.aspx).  
  
> [!NOTE]  
> La réplication Hyper-V ne peut être exécutée que sur Windows Server Hyper-V. Elle n'est pas possible sur la version d'Hyper-V qui s'exécute sur Windows 8.  
  
## <a name="windows-server-2012-or-newer-domain-controllers-required"></a>Contrôleur de domaine Windows Server 2012 ou plus récent requis

Windows Server 2012 Hyper-V a introduit VM-GenerationID (VMGenID). VMGenID permet à l'hyperviseur de communiquer avec le système d'exploitation invité quand des changements importants ont lieu. Par exemple, l'hyperviseur peut communiquer avec un contrôleur de domaine virtualisé en lui indiquant qu'une restauration de capture instantanée a eu lieu (technologie de restauration de capture instantanée Hyper-V et non restauration de sauvegarde). AD DS de Windows Server 2012 et versions ultérieures est conscient de la technologie de machine virtuelle VMGenID et l’utilise pour détecter les opérations d’hyperviseur, telles que la restauration d’instantané, qui lui permet de mieux se protéger.  
  
> [!NOTE]
> Seuls les AD DS sur les contrôleurs de Windows Server 2012 ou plus récents fournissent ces mesures de sécurité résultant de VMGenID ; Les contrôleurs de domaine qui exécutent toutes les versions précédentes de Windows Server sont soumis à des problèmes tels que la restauration USN qui peut se produire lorsqu’un contrôleur de domaine virtualisé est restauré à l’aide d’un mécanisme non pris en charge, tel que la restauration d’instantané. Pour plus d’informations sur ces dispositifs de protection et leur déclenchement, consultez [Architecture des contrôleurs de domaine virtualisés](https://technet.microsoft.com/library/jj574118.aspx).  
  
Lorsqu’un basculement de réplica Hyper-V se produit (planifié ou non), le contrôleur de périphérique virtualisé détecte une réinitialisation VMGenID, en déclenchant les fonctionnalités de sécurité mentionnées ci-dessus. Le déroulement des opérations Active Directory s'effectue ensuite normalement. L'ordinateur virtuel de réplication s'exécute à la place de l'ordinateur virtuel principal.  
  
> [!NOTE]  
> Dans la mesure où il existe maintenant deux instances de la même identité du contrôleur de domaine, il est possible que l'instance principale et l'instance répliquée s'exécutent en même temps. La réplication Hyper-V dispose de mécanismes de contrôle pour s'assurer que l'ordinateur virtuel principal et l'ordinateur virtuel de réplication ne s'exécutent pas simultanément. Toutefois, ils peuvent s'exécuter en même temps en cas de défaillance du lien qui les unit après la réplication de l'ordinateur virtuel. Au cas où une telle éventualité se présenterait (faible probabilité), les contrôleurs de domaine virtuels qui exécutent Windows Server 2012 ont des dispositifs de protection pour AD DS, contrairement aux contrôleurs de domaine virtuels qui exécutent des versions antérieures de Windows Server.  
  
Durant l’utilisation de la réplication Hyper-V, veillez à suivre les meilleures pratiques relatives à l’ [exécution de contrôleurs de domaine virtuels sur Hyper-V](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=WS.10).aspx). Vous y trouverez, par exemple, des recommandations sur le stockage des fichiers Active Directory sur les disques SCSI virtuels, ce qui permet de bénéficier de dispositifs de protection plus robustes pour la pérennité des données.  
  
## <a name="supported-and-unsupported-scenarios"></a>Scénarios pris en charge et non pris en charge

Seules les machines virtuelles qui exécutent Windows Server 2012 ou une version plus récente sont prises en charge pour le basculement non planifié et pour le test de basculement. Même pour le basculement planifié, Windows Server 2012 ou une version plus récente est recommandé pour le contrôleur de réseau virtualisé afin d’atténuer les risques au cas où un administrateur démarrait par inadvertance la machine virtuelle principale et la machine virtuelle répliquée en même temps.  
  
Les ordinateurs virtuels qui utilisent des versions antérieures de Windows Server sont pris en charge pour le basculement planifié mais pas pour le basculement non planifié en raison du risque de restauration USN. Pour plus d’informations sur la restauration USN, consultez [USN et restauration USN](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10)).  
  
> [!NOTE]  
> Il n'y a pas de conditions requises au niveau fonctionnel pour le domaine ou la forêt. Il existe uniquement des conditions requises pour le système d'exploitation des contrôleurs de domaine qui s'exécutent en tant qu'ordinateurs virtuels répliqués à l'aide de la réplication Hyper-V. Les ordinateurs virtuels peuvent être déployés dans une forêt qui contient d'autres contrôleurs de domaine physiques ou virtuels exécutant des versions antérieures de Windows Server et pouvant être également répliqués via la réplication Hyper-V.  
  
Cette déclaration de prise en charge est basée sur les tests effectués dans une forêt à domaine unique. Toutefois, les configurations de forêts à plusieurs domaines sont également prises en charge. Pour ces tests, les contrôleurs de domaine virtualisés DC1 et DC2 sont des partenaires de réplication Active Directory situés dans le même site, hébergés sur un serveur qui exécute Hyper-V sur Windows Server 2012. La réplication Hyper-V est activée sur l'ordinateur virtuel invité qui exécute le contrôleur de domaine DC2. Le serveur de réplication est hébergé dans un autre centre de données géographiquement distant. Pour faciliter la description des processus de cas de test ci-dessous, l'ordinateur virtuel qui s'exécute sur le serveur de réplication est appelé DC2-Rec (bien qu'en pratique, il conserve le même nom que l'ordinateur virtuel d'origine).  
  
### <a name="windows-server-2012"></a>Windows Server 2012

Le tableau suivant donne des explications sur la prise en charge des contrôleurs de domaine virtualisés qui exécutent Windows Server 2012 et les cas de test.  
  
|||  
|-|-|  
|Basculement planifié|Basculement non planifié|  
|Prise en charge|Prise en charge|  
|Cas de test :<p>-DC1 et DC2 exécutent Windows Server 2012.<p>-DC2 est arrêté et un basculement est effectué sur DC2-Rec. Le basculement peut être planifié ou non planifié.<p>-Une fois que DC2-Rec démarre, il vérifie si la valeur de VMGenID qu’il contient dans sa base de données est identique à la valeur du pilote d’ordinateur virtuel enregistré par le serveur de réplication Hyper-V.<p>Par conséquent, DC2-Rec déclenche les dispositifs de protection de la virtualisation ; en d’autres termes, il réinitialise son invocation, abandonne son pool RID et définit une exigence de synchronisation initiale avant de supposer qu’il assume le rôle de maître d’opérations. Pour plus d'informations sur les conditions requises par la synchronisation initiale, voir l'article correspondant.<p>-DC2-Rec enregistre ensuite la nouvelle valeur de VMGenID dans sa base de données et valide toutes les mises à jour ultérieures dans le contexte du nouvel invocation.<p>-Suite à la réinitialisation de l’appel d’invocation, DC1 convergera sur tous les changements AD introduits par DC2-Rec, même s’il a été restauré dans le temps, ce qui signifie que les mises à jour Active Directory effectuées sur DC2-Rec après le basculement convergeront en toute sécurité|Le cas de test est le même que pour un basculement planifié, avec les exceptions suivantes :<p>-Les mises à jour Active Directory reçues sur DC2 mais pas encore répliquées par AD vers un partenaire de réplication avant l’événement de basculement seront perdues.<p>-Les mises à jour AD reçues sur DC2 après l’heure du point de récupération qui ont été répliquées par AD vers DC1 sont répliquées de DC1 vers DC2-Rec.|  
  
### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 et les versions antérieures

Le tableau suivant donne des explications sur la prise en charge des contrôleurs de domaine virtualisés qui exécutent Windows Server 2008 R2 et les versions antérieures.  
  
|||  
|-|-|  
|Basculement planifié|Basculement non planifié|  
|Pris en charge mais non recommandé, car les contrôleurs de domaine qui exécutent ces versions de Windows Server ne prennent pas en charge VMGenID ou n'utilisent pas les dispositifs de protection de virtualisation associés. Cela les expose aux risques liés à la restauration USN. Pour plus d’informations, consultez [USN et restauration USN](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10)).|Remarque non prise en charge **:** le basculement non planifié est pris en charge lorsque la restauration USN n’est pas un risque, par exemple un contrôleur de périphérique unique dans la forêt (configuration non recommandée).|  
|Cas de test :<p>-DC1 et DC2 exécutent Windows Server 2008 R2.<p>-DC2 est arrêté et un basculement planifié est effectué sur DC2-Rec. Toutes les données sur DC2 sont répliquées sur DC2-Rec avant la fin de l’arrêt.<p>-Une fois que DC2-Rec démarre, il reprend la réplication avec DC1 en utilisant le même invocation que DC2.|N/A|  
