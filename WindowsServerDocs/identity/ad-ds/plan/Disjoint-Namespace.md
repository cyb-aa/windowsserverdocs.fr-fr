---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: Espace de noms dissocié
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0a2e911e889343b05a515c94e615d3648289f2df
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883570"
---
# <a name="disjoint-namespace"></a>Espace de noms dissocié

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un espace de noms disjoint se produit lorsqu’un ou plusieurs ordinateurs membres du domaine ont un suffixe principal de Service DNS (Domain Name) qui ne correspond pas au nom DNS du domaine Active Directory auquel appartiennent les ordinateurs. Par exemple, un ordinateur membre qui utilise un suffixe DNS principal corp.fabrikam.com dans un domaine Active Directory appelé na.corp.fabrikam.com utilise un espace de noms disjoint.  
  
Un espace de noms disjoint est plus complexe à administrer, gérer et résoudre les problèmes à un espace de noms contigu. Dans un espace de noms contigu, le suffixe DNS principal correspond au nom de domaine Active Directory. Les applications réseau qui sont écrits de supposer que l’espace de noms Active Directory est identique au suffixe DNS principal pour tous les ordinateurs membres du domaine ne fonctionnent pas correctement dans un espace de noms disjoint.  
  
## <a name="support-for-disjoint-namespaces"></a>Prise en charge des espaces de noms disjoints  
Ordinateurs membres du domaine, y compris les contrôleurs de domaine, peuvent fonctionner dans un espace de noms disjoint. Ordinateurs membres du domaine peuvent inscrire leur hôte (A) enregistrement de ressource et le protocole IP version 6 (IPv6) hébergent l’enregistrement de ressource (AAAA) dans un espace de noms DNS disjoint. Lorsque des ordinateurs membres du domaine s’inscrivent leurs enregistrements de ressources de cette façon, les contrôleurs de domaine continuent à inscrire des enregistrements de ressources globaux et spécifiques de service (SRV) dans la zone DNS est identique au nom de domaine Active Directory.  
  
Par exemple, supposons qu’un contrôleur de domaine pour le domaine Active Directory appelé na.corp.fabrikam.com qui utilise un suffixe DNS principal corp.fabrikam.com s’inscrit à un hôte (A) et les enregistrements de ressource hôte (AAAA) IPv6 dans la zone DNS corp.fabrikam.com. Le contrôleur de domaine continue à inscrire des enregistrements de ressources globaux et spécifiques de service (SRV) dans les zones DNS _msdcs.na.corp.fabrikam.com et na.corp.fabrikam.com, ce qui permet l’emplacement du service.  
  
> [!IMPORTANT]  
> Bien que les systèmes d’exploitation Windows peuvent prendre en charge un espace de noms disjoint, les applications qui sont écrites de supposer que le suffixe DNS principal est le même que le suffixe de domaine Active Directory ne peuvent pas fonctionner dans un tel environnement. Pour cette raison, vous devez tester toutes les applications et leurs systèmes d’exploitation respectifs soigneusement avant de déployer un espace de noms disjoint.  
  
Un espace de noms disjoint devrait fonctionner (et est pris en charge) dans les situations suivantes :  
  
-   Lorsqu’une forêt avec plusieurs domaines Active Directory utilise un espace de noms DNS unique, qui est également appelé une zone DNS  
  
    Un exemple de ceci est une société qui utilise des domaines régionaux avec des noms tels que na.corp.fabrikam.com, sa.corp.fabrikam.com et asia.corp.fabrikam.com et utilise un espace de noms DNS unique, tels que corp.fabrikam.com.  
  
-   Quand un seul domaine Active Directory est divisé en espaces de noms DNS distincts  
  
    Un exemple de ceci est une entreprise avec un domaine Active Directory corp.contoso.com qui utilise les zones DNS, hr.corp.contoso.com production.corp.contoso.com et it.corp.contoso.com.  
  
Un espace de noms disjoint ne fonctionne pas correctement (et n’est pas pris en charge) dans les situations suivantes :  
  
-   Un suffixe disjoint utilisé par les membres du domaine correspond à un nom de domaine Active Directory dans la forêt ou dans un autre. Cela interrompt le routage des suffixes de nom de Kerberos.  
  
-   Le même suffixe disjoint est utilisé dans une autre forêt. Cela empêche le routage de manière unique ces suffixes entre forêts.  
  
-   Lorsque serveur membre de domaine certification authority (CA) modifie son nom de domaine complet (FQDN) afin qu’il ne plus utiliser le même suffixe DNS principal qui est utilisé par les contrôleurs de domaine du domaine auquel le serveur d’autorité de certification est un membre. Dans ce cas, peut avoir des problèmes de validation des certificats le serveur d’autorité de certification émis, selon les noms DNS sont utilisés dans les Points de Distribution CRL. Mais si vous placez un serveur d’autorité de certification dans un espace de noms disjoint stable, il fonctionne correctement et est pris en charge.  
  
## <a name="considerations-for-disjoint-namespaces"></a>Considérations pour les espaces de noms disjoints  
Les considérations suivantes peuvent vous aider à décider si vous devez utiliser un espace de noms disjoint.  
  
### <a name="application-compatibility"></a>Compatibilité des applications  
Comme mentionné précédemment, un espace de noms disjoint peut entraîner des problèmes pour les applications et services qui sont écrits supposent qu’un suffixe DNS principal d’ordinateur est identique au nom du nom de domaine dont il est membre. Avant de déployer un espace de noms disjoint, vous devez vérifier les problèmes de compatibilité des applications. En outre, veillez à vérifier la compatibilité de toutes les applications que vous utilisez lorsque vous effectuez votre analyse. Cela inclut les applications à partir de Microsoft et d’autres développeurs de logiciels.  
  
### <a name="advantages-of-disjoint-namespaces"></a>Avantages des espaces de noms disjoints  
À l’aide d’un espace de noms disjoint peut avoir les avantages suivants :  
  
-   Étant donné que le suffixe DNS principal d’un ordinateur peut indiquer des informations différentes, vous pouvez gérer l’espace de noms DNS séparément à partir du nom de domaine Active Directory.  
  
-   Vous pouvez séparer l’espace de noms DNS en fonction de l’emplacement géographique ou de la structure de l’entreprise. Par exemple, vous pouvez séparer l’espace de noms basé sur les noms d’unité métier ou un emplacement physique comme continent, pays/région ou bâtiment.  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>Inconvénients des espaces de noms disjoints  
À l’aide d’un espace de noms disjoint peut avoir les inconvénients suivants :  
  
-   Vous devez créer et gérer des zones DNS distinctes pour chaque domaine Active Directory dans la forêt qui est membre d’ordinateurs qui utilisent un espace de noms disjoint. (Autrement dit, elle nécessite une configuration supplémentaire et plus complexe).  
  
-   Vous devez effectuer les étapes manuelles pour modifier et gérer l’attribut Active Directory qui permet aux membres du domaine à utiliser les suffixes DNS spécifiés, le principales.  
  
-   Pour optimiser la résolution de noms, vous devez effectuer les étapes manuelles pour modifier et gérer la stratégie de groupe pour configurer des ordinateurs membres avec des suffixes DNS principaux autre.  
  
    > [!NOTE]  
    > Le Service de nom Internet Windows (WINS) peut être utilisée pour décaler cet inconvénient en résolvant les noms en une partie. Pour plus d’informations sur WINS, consultez la référence technique WINS ([https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303)).  
  
-   Lorsque votre environnement requiert plusieurs suffixes DNS principaux, vous devez configurer correctement l’ordre de recherche de suffixe DNS pour tous les domaines Active Directory dans la forêt.  
  
    Pour définir l’ordre de recherche de suffixe DNS, vous pouvez utiliser des objets de stratégie de groupe ou les paramètres du service serveur de Configuration protocole DHCP (Dynamic Host). Vous pouvez également modifier le Registre.  
  
-   Vous devez soigneusement tester toutes les applications des problèmes de compatibilité.  
  
Pour plus d’informations sur les étapes que vous pouvez prendre pour résoudre ces inconvénients, voir créer un Disjoint Namespace ([https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638)).  
  
### <a name="planning-a-namespace-transition"></a>Planification d’une transition de l’espace de noms  
Avant de modifier un espace de noms, passez en revue les considérations suivantes, qui s’appliquent à des transitions à partir des espaces de noms contigus à disjointes des espaces de noms (ou l’inverse) :  
  
-   Configuré manuellement les noms de principal du Service (SPN) ne corresponde plus à des noms DNS après une modification de l’espace de noms. Cela peut entraîner des échecs d’authentification.  
  
    Pour plus d’informations, consultez Service ouvertures de session échouent Due to Incorrectly Set SPNs ([https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304)).  
  
    -   Si vous utilisez des ordinateurs basés sur Windows Server 2003 avec la délégation contrainte, ces ordinateurs peuvent nécessiter une configuration supplémentaire pour modifier les noms principaux de service. Pour plus d’informations, consultez l’article 936628 dans la Base de connaissances Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306)).  
  
    -   Si vous souhaitez déléguer des autorisations pour modifier les noms principaux de service pour d’autres administrateurs subordonnés, voir Délégation de l’autorité pour modifier les noms principaux de service ([https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639)).  
  
-   Si vous utilisez Lightweight Directory Access Protocol (LDAP) sur couche SSL (Secure Sockets) (appelée LDAPS) avec une autorité de certification dans un déploiement comportant des contrôleurs de domaine qui sont configurés dans un espace de noms disjoint, vous devez utiliser le nom de domaine Active Directory approprié et suffixe DNS principal lorsque vous configurez les certificats LDAPS.  
  
    Pour plus d’informations sur les exigences de certificat de contrôleur de domaine, consultez l’article 321051 dans la Base de connaissances Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307)).  
  
    > [!NOTE]  
    > Les contrôleurs de domaine qui utilisent des certificats pour LDAPS peuvent vous obliger à redéployer leurs certificats. Lorsque vous procédez ainsi, contrôleurs de domaine ne peuvent pas sélectionner un certificat approprié jusqu'à ce qu’ils sont redémarrés. Pour plus d’informations sur l’authentification LDAPS et une mise à jour associée pour Windows Server 2003, consultez l’article 932834 dans la Base de connaissances Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308)).  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>Planification de déploiements de l’espace de noms disjoint  
Prenez les précautions suivantes si vous déployez des ordinateurs dans un environnement qui comporte un espace de noms disjoint :  
  
1.  Notifier tous les éditeurs de logiciels avec lesquels vous travaillez qu’ils doivent tester et prendre en charge un espace de noms disjoint. Demandez-lui de vérifier qu’ils prennent en charge leurs applications dans les environnements qui utilisent des espaces de noms disjoints.  
  
2.  Testez toutes les versions des systèmes d’exploitation et des applications dans des environnements de lab d’espace de noms disjoint. Lorsque vous effectuez, suivez ces recommandations :  
  
    1.  Résolvez tous les problèmes de logiciel avant de déployer le logiciel dans votre environnement.  
  
    2.  Dans la mesure du possible, participer à des tests bêta des systèmes d’exploitation et des applications que vous prévoyez de déployer des espaces de noms disjoints.  
  
3.  Assurez-vous que les administrateurs et le personnel du support technique est informé de l’espace de noms disjoint et son impact.  
  
4.  Créer un plan qui rend possible pour vous à la transition à partir d’un espace de noms disjoint à un espace de noms contigu, si nécessaire.  
  
5.  Promouvez l’importance de la prise en charge de l’espace de noms disjoint avec le système d’exploitation et les fournisseurs d’application.  
  


