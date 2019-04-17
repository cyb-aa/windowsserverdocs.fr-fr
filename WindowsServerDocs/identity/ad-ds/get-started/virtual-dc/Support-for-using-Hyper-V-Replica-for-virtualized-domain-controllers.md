---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: "Prise en charge pour l’utilisation de la réplication Hyper-V pour les contrôleurs de domaine virtualisés"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0444198196ed08a22aba92a0f59cc6e7a2518a2e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>Prise en charge pour l’utilisation de la réplication Hyper-V pour les contrôleurs de domaine virtualisés

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique décrit la prise en charge de l’utilisation de la réplication Hyper-V pour répliquer un ordinateur virtuel (VM) qui s’exécute comme un contrôleur de domaine (DC). Réplica Hyper-V est une nouveauté de Windows Server2012 qui fournit un mécanisme de réplication intégrée à un niveau de l’ordinateur virtuel à compter de Hyper-V.  
  
Réplica Hyper-V réplique de façon asynchrone ordinateurs virtuels sélectionnés à partir d’un hôte Hyper-V principal vers un ordinateur hôte Hyper-V de réplica sur le réseau local ou sur des liaisons WAN. Une fois la réplication initiale terminée, les modifications suivantes sont répliquées selon un intervalle défini par l’administrateur.  
  
Basculement peut être planifié ou non. Un basculement planifié est lancé par un administrateur sur l’ordinateur virtuel principal et toutes les modifications non répliquées sont copiées sur l’ordinateur virtuel afin d’éviter toute perte de données de réplication. Un basculement non planifié est lancé sur l’ordinateur virtuel de réplication en réponse à une défaillance inattendue de l’ordinateur virtuel principal. Perte de données est possible car il n’existe aucune possibilité de transmettre les changements de l’ordinateur virtuel principal qui peut n’a pas été répliqué encore.  
  
Pour plus d’informations sur la réplication Hyper-V, voir [vue d’ensemble du réplica Hyper-V](https://technet.microsoft.com/library/jj134172.aspx) et [déployer la réplication Hyper-V](https://technet.microsoft.com/library/jj134207.aspx).  
  
> [!NOTE]  
> Réplica Hyper-V peut être exécuté uniquement sur WindowsServerHyper-V, pas la version d’Hyper-V qui s’exécute sur Windows8.  
  
## <a name="windows-server-2012-domain-controllers-required"></a>Contrôleurs de domaine Windows Server2012requis  
Windows Server2012 Hyper-V introduit également VM-GenerationID (VMGenID). VMGenID permet à l’hyperviseur de communiquer avec la système d’exploitation invité lors de changements importants ont lieu. Par exemple, l’hyperviseur peut communiquer à un contrôleur de domaine virtualisé qu’une restauration à partir d’une capture instantanée a eu lieu (technologie Hyper-V snapshot restauration, non restauration de sauvegarde). Les services ADDS dans Windows Server2012 prend en charge de la technologie de VMGenID VM et l’utilise pour détecter les opérations de l’hyperviseur sont effectuées, par exemple, la restauration de capture instantanée, ce qui lui permet de mieux se protéger.  
  
> [!NOTE]  
> Pour renforcer ce point, seul ADDS sur des contrôleurs de domaine Windows Server2012 fournit ces mesures de sécurité liées à VMGenID; contrôleurs de domaine qui exécutent les versions antérieures de Windows Server sont sujets à des problèmes tels que la restauration USN qui peuvent se produire lorsqu’un contrôleur de domaine virtualisé est restauré à l’aide d’un mécanisme non pris en charge, par exemple, la restauration de capture instantanée. Pour plus d’informations sur ces dispositifs de protection et leur déclenchement, voir [Architecture des contrôleurs de domaine virtualisés](https://technet.microsoft.com/library/jj574118.aspx).  
  
Lorsqu’un basculement du réplica Hyper-V se produit (planifié ou non), Windows Server2012 virtualisée du contrôleur de domaine détecte une réinitialisation de VMGenID, qui déclenche les fonctionnalités de sécurité indiqués ci-dessus. Opérations ActiveDirectory puis poursuivez normalement. Le réplica de la machine virtuelle s’exécute à la place de l’ordinateur virtuel principal.  
  
> [!NOTE]  
> Étant donné qu’il existe désormais maintenant deux instances de la même identité du contrôleur de domaine, il est possible pour l’instance principale et l’instance répliquée à exécuter. Alors que le réplica Hyper-V dispose de mécanismes de contrôle en place pour garantir le serveur principal et le réplica ne s’exécutent pas simultanément, il est possible pour qu’ils puissent s’exécuter en même temps, en cas de défaillance de la liaison entre eux après la réplication de l’ordinateur virtuel. En cas de cette peu probable, des contrôleurs de domaine virtualisés qui exécutent Windows Server2012 ont des dispositifs de protection pour protéger les services ADDS, contrairement aux contrôleurs de domaine qui exécutent des versions antérieures de Windows Server ne le faites pas.  
  
Lorsque vous utilisez la réplication Hyper-V, veillez à suivre les meilleures pratiques pour [contrôleurs de domaine virtuel en cours d’exécution sur Hyper-V](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=WS.10).aspx). Traite, par exemple, les recommandations pour le stockage des fichiers ActiveDirectory sur les disques SCSI virtuels, ce qui permet de durabilité de données.  
  
## <a name="supported-and-unsupported-scenarios"></a>Scénarios pris en charge et non pris en charge  
Seuls les ordinateurs virtuels qui exécutent Windows Server2012 sont pris en charge pour un basculement non planifié et test de basculement. Même pour le basculement planifié, Windows Server2012 est recommandé pour le contrôleur de domaine virtualisé afin d’atténuer les risques au cas où l’administrateur commence par inadvertance l’ordinateur virtuel principal et l’ordinateur virtuel répliqué en même temps.  
  
Les ordinateurs virtuels qui exécutent des versions antérieures de Windows Server sont prises en charge pour le basculement planifié mais non pris en charge pour le basculement non planifié en raison des risques pour la restauration USN. Pour plus d’informations sur la restauration USN, voir [USN et restauration USN](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10)).  
  
> [!NOTE]  
> Il n’existe aucun niveau fonctionnel du domaine ou de forêt; il existe uniquement de système d’exploitation requis pour les contrôleurs de domaine qui s’exécutent en tant qu’ordinateurs virtuels qui sont répliqués à l’aide de la réplication Hyper-V. Les ordinateurs virtuels peuvent être déployés dans une forêt qui contient d’autres contrôleurs de domaine physiques ou virtuels qui exécutent des versions antérieures de Windows Server et peuvent ou peuvent être également répliqués à l’aide de la réplication Hyper-V.  
  
Cette déclaration de prise en charge est basée sur les tests qui ont été effectuées dans une forêt à domaine unique, même si les configurations de la forêt à domaines multiples font également. Pour ces tests, les contrôleurs de domaine virtualisés DC1 et DC2 sont des partenaires de réplication ActiveDirectory dans le même site, hébergés sur un serveur qui exécute Hyper-V sur Windows Server2012. L’ordinateur virtuel invité qui s’exécute DC2 a réplica Hyper-V est activé. Le serveur de réplication est hébergé dans un autre centre de données géographiquement distant. Pour expliquer les processus de cas de test indiquées ci-dessous, l’ordinateur virtuel en cours d’exécution sur le serveur de réplication est appelé DC2-Rec (bien que dans la pratique, il conserve le même nom que l’ordinateur virtuel d’origine).  
  
### <a name="windows-server-2012"></a>Windows Server2012  
Le tableau suivant explique la prise en charge des contrôleurs de domaine virtualisés qui exécutent Windows Server2012 et les cas de test.  
  
|||  
|-|-|  
|Basculement planifié|Basculement non planifié|  
|Prise en charge|Prise en charge|  
|Cas de test:<br /><br />-DC1 et DC2 exécutent Windows Server2012.<br /><br />-DC2 est arrêté et un basculement est effectué sur DC2-Rec. Le basculement peut être planifié ou non.<br /><br />-Une fois que DC2-Rec démarre, il vérifie si la valeur de VMGenID qu’il contient dans sa base de données est identique à la valeur du pilote d’ordinateur virtuel enregistrée par le serveur de réplication Hyper-V.<br /><br />-Ainsi, DC2-Rec déclenche les dispositifs de protection; en d’autres termes, il réinitialise son InvocationID, abandonne son pool RID et définit une exigence de synchronisation initiale avant d’endosser un rôle de maître d’opérations. Pour plus d’informations sur la spécification de la synchronisation initiale, voir.<br /><br />-Ensuite, DC2-Rec enregistre la nouvelle valeur de VMGenID dans sa base de données et valide toutes les mises à jour ultérieures dans le contexte du nouvel InvocationID.<br /><br />-Comme résultat de la réinitialisation d’InvocationID, DC1 fait converger tous les changements ActiveDirectory introduits par DC2-Rec, même si elle a été restauré à temps, ce qui signifie que les mises à jour ActiveDirectory effectuées sur DC2-Rec après que le basculement fait converger en toute sécurité|Le cas de test est la même que pour un basculement planifié, à ces exceptions près:<br /><br />-Toute AD mises à jour reçues sur DC2 mais pas encore répliquées par ActiveDirectory pour un partenaire de réplication avant que l’événement de basculement seront perdue.<br /><br />-La mise à jour AD reçue sur DC2 après que l’heure du point de récupération qui ont été répliquées par ActiveDirectory sur DC1 est répliquée à partir de DC1 vers DC2-Rec.|  
  
### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server2008R2 et versions antérieures  
Le tableau suivant explique la prise en charge des contrôleurs de domaine virtualisés qui exécutent Windows Server2008R2 et versions antérieures.  
  
|||  
|-|-|  
|Basculement planifié|Basculement non planifié|  
|Prise en charge mais non recommandé, car les contrôleurs de domaine qui exécutent ces versions de Windows Server ne pas prendre en charge VMGenID ou utilisez des dispositifs de protection associé. Cela les expose aux risques liés à la restauration USN. Pour plus d’informations, voir [USN et restauration USN](https://technet.microsoft.com/en-us/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10)).|Non pris en charge **Remarque:** basculement non planifié est pris en charge lorsque où la restauration USN n’est pas un risque, par exemple, un seul contrôleur de domaine dans la forêt (configuration non recommandée).|  
|Cas de test:<br /><br />-DC1 et DC2 exécutent Windows Server2008R2.<br /><br />-DC2 est arrêté et un basculement planifié est effectué sur DC2-Rec. Toutes les données sur DC2 sont répliquées sur DC2-Rec avant l’arrêt complet.<br /><br />-Une fois que DC2-Rec démarre, il reprend la réplication avec DC1 en utilisant le même invocationID que DC2.|NON APPLICABLE|  
  


