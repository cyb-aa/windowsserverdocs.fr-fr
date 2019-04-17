---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: Namespace disjoint
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac231dbfbaaeafa39199e29a1744d84d5cec23e8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="disjoint-namespace"></a>Namespace disjoint

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Un espace de noms disjoint se produit lorsqu’un ou plusieurs ordinateurs membres du domaine ont un suffixe principal de Service DNS (Domain Name) qui ne correspond pas le nom DNS du domaine ActiveDirectory auquel appartiennent les ordinateurs. Par exemple, un ordinateur membre qui utilise un suffixe DNS principal de corp.fabrikam.com dans un domaine ActiveDirectory nommé na.corp.fabrikam.com est à l’aide d’un espace de noms disjoint.  
  
Un espace de noms disjoint est plus complexe à administrer, tenir à jour et résoudre les problèmes à un espace de noms contigu. Dans un espace de noms contigu, le suffixe DNS principal correspond au nom de domaine ActiveDirectory. Les applications de réseau qui sont conçues pour supposer que l’espace de noms ActiveDirectory est identique au suffixe DNS principal pour tous les ordinateurs membres du domaine ne fonctionnent pas correctement dans un espace de noms disjoint.  
  
## <a name="support-for-disjoint-namespaces"></a>Prise en charge des espaces de noms disjoints  
Ordinateurs membres du domaine, y compris les contrôleurs de domaine, peuvent fonctionner dans un espace de noms disjoint. Ordinateurs membres du domaine peuvent inscrire leur hôte (A) enregistrement de ressource et le protocole IP version6 (IPv6) hôte enregistrement de ressource (AAAA) dans un espace de noms DNS disjoint. Lorsque des ordinateurs membres du domaine s’inscrivent leurs enregistrements de ressources de cette façon, les contrôleurs de domaine continuent à inscrire des enregistrements de ressource globaux et spécifiques de service (SRV) dans la zone DNS est identique au nom de domaine ActiveDirectory.  
  
Par exemple, supposons qu’un contrôleur de domaine pour le domaine ActiveDirectory nommé na.corp.fabrikam.com qui utilise un suffixe DNS principal de corp.fabrikam.com inscrit hôte (A) et les enregistrements de ressource hôte (AAAA) IPv6 dans la zone DNS corp.fabrikam.com. Le contrôleur de domaine continue à inscrire des enregistrements de ressource globaux et spécifiques de service (SRV) dans les zone _msdcs. na.corp.fabrikam.com et na.corp.fabrikam.com zones DNS, ce qui permet l’emplacement du service.  
  
> [!IMPORTANT]  
> Bien que les systèmes d’exploitation Windows peuvent prendre en charge un espace de noms disjoint, les applications qui sont conçues pour supposer que le suffixe DNS principal est le même que le suffixe de domaine ActiveDirectory peut ne pas fonctionnent dans un environnement de ce type. Pour cette raison, vous devez tester toutes les applications et leurs systèmes d’exploitation respectifs avec soin avant de déployer un espace de noms disjoint.  
  
Un espace de noms disjoint doit fonctionner (et est pris en charge) dans les situations suivantes:  
  
-   Lorsqu’une forêt avec plusieurs domaines ActiveDirectory utilise un espace de noms DNS unique, qui est également appelé une zone DNS  
  
    Ceci est une société qui utilise des domaines régionaux portant des noms comme na.corp.fabrikam.com, sa.corp.fabrikam.com et asia.corp.fabrikam.com et utilise un espace de noms DNS unique, par exemple, corp.fabrikam.com.  
  
-   Quand un seul domaine ActiveDirectory est divisé en espaces de noms distincts  
  
    Ceci est une société avec un domaine ActiveDirectory corp.contoso.com qui utilise des zones DNS comme hr.corp.contoso.com, production.corp.contoso.com et it.corp.contoso.com.  
  
Un espace de noms disjoint ne fonctionne pas correctement (et n’est pas pris en charge) dans les situations suivantes:  
  
-   Un suffixe disjoint utilisé par les membres du domaine correspond à un nom de domaine ActiveDirectory dans la forêt ou dans un autre. Cela s’arrête le routage du suffixe de nom de Kerberos.  
  
-   Le même suffixe disjoint est utilisé dans une autre forêt. Cela empêche le routage de manière unique ces suffixes entre forêts.  
  
-   Lorsqu’une autorité de certification de membre de domaine (CA) les modifications de serveur qu’il est entièrement domaine nom complet (FQDN) afin qu’il ne plus utiliser le même suffixe DNS principal qui est utilisé par les contrôleurs de domaine du domaine auquel le serveur d’autorité de certification est un membre. Dans ce cas, vous devrez peut-être des problèmes de validation des certificats du serveur d’autorité de certification a émis, selon les noms DNS sont utilisés dans les Points de Distribution CRL. Mais si vous placez un serveur d’autorité de certification dans un espace de noms disjoint stable, il fonctionne correctement et est pris en charge.  
  
## <a name="considerations-for-disjoint-namespaces"></a>Considérations pour les espaces de noms disjoints  
Les considérations suivantes peuvent vous aider à décider si vous devez utiliser un espace de noms disjoint.  
  
### <a name="application-compatibility"></a>Compatibilité des applications  
Comme indiqué précédemment, un espace de noms disjoint peut entraîner des problèmes pour les applications et services qui sont conçues pour supposer que le suffixe DNS principal ordinateur est identique au nom du nom de domaine dont il est membre. Avant de déployer un espace de noms disjoint, vous devez vérifier les applications pour des problèmes de compatibilité. En outre, veillez à vérifier la compatibilité de toutes les applications que vous utilisez lorsque vous effectuez votre analyse. Cela inclut les applications à partir de Microsoft et d’autres développeurs de logiciels.  
  
### <a name="advantages-of-disjoint-namespaces"></a>Avantages des espaces de noms disjoint  
À l’aide d’un espace de noms disjoint peut avoir les avantages suivants:  
  
-   Étant donné que le suffixe DNS principal d’un ordinateur peut indiquer des informations différentes, vous pouvez gérer l’espace de noms DNS séparément à partir du nom de domaine ActiveDirectory.  
  
-   Vous pouvez séparer l’espace de noms DNS basée sur la structure de l’entreprise ou l’emplacement géographique. Par exemple, vous pouvez séparer l’espace de noms basé sur les noms des unités ou emplacement physique comme continent, pays/région ou création.  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>Inconvénients des espaces de noms disjoints  
À l’aide d’un espace de noms disjoint présentent les inconvénients suivants:  
  
-   Vous devez créer et gérer des zones DNS distincts pour chaque domaine ActiveDirectory dans la forêt qui a des ordinateurs membres qui utilisent un espace de noms disjoint. (Autrement dit, il nécessite une configuration supplémentaire et plus complexe.)  
  
-   Vous devez effectuer les étapes manuelles pour modifier et gérer l’attribut ActiveDirectory qui permet aux membres du domaine à utiliser les suffixes DNS spécifiés, principales.  
  
-   Pour optimiser la résolution de noms, vous devez effectuer les étapes manuelles pour modifier et mettre à jour la stratégie de groupe pour configurer des ordinateurs membres avec d’autres suffixes DNS principaux.  
  
    > [!NOTE]  
    > Le Service de nom Internet Windows (WINS) permet de décalage cet inconvénient à la résolution de noms en une partie. Pour plus d’informations sur WINS, consultez la référence technique WINS ([https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303)).  
  
-   Lorsque votre environnement nécessite plusieurs suffixes DNS principaux, vous devez configurer correctement l’ordre de recherche de suffixe DNS pour tous les domaines ActiveDirectory dans la forêt.  
  
    Pour définir l’ordre de recherche de suffixe DNS, vous pouvez utiliser des objets de stratégie de groupe ou les paramètres du service serveur DHCP Dynamic Host Configuration Protocol (). Vous pouvez également modifier le Registre.  
  
-   Vous devez tester soigneusement toutes les applications pour des problèmes de compatibilité.  
  
Pour plus d’informations sur les étapes que vous pouvez prendre pour résoudre ces inconvénients, voir créer un Namespace Disjoint ([https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638)).  
  
### <a name="planning-a-namespace-transition"></a>Planification d’une transition de l’espace de noms  
Avant de modifier un espace de noms, passez en revue les considérations suivantes, qui s’appliquent aux transitions des espaces de noms contigu à disjoints des espaces de noms (ou l’inverse):  
  
-   Configuré manuellement de noms principaux de Service (SPN) peut ne correspondent plus aux noms DNS après une modification de l’espace de noms. Cela peut entraîner des échecs d’authentification.  
  
    Pour plus d’informations, voir Service ouvertures de session Échec dû à définir correctement des SPN ([https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304)).  
  
    -   Si vous utilisez des ordinateurs basés sur Windows Server2003 avec la délégation contrainte, ces ordinateurs peuvent nécessiter une configuration supplémentaire pour modifier les noms principaux de service. Pour plus d’informations, consultez l’article 936628dans la Base de connaissances Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306)).  
  
    -   Si vous souhaitez déléguer des autorisations pour modifier le SPN subordonnées administrateurs, voir Délégation de l’autorité pour modifier le SPN ([https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639)).  
  
-   Si vous utilisez Active accès protocole LDAP (Lightweight) via SSL Secure Sockets Layer () (appelé LDAPS) avec une autorité de certification dans un déploiement qui possède des contrôleurs de domaine qui sont configurés dans un espace de noms disjoint, vous devez utiliser le nom de domaine ActiveDirectory approprié et le suffixe DNS principal lorsque vous configurez les certificats LDAPS.  
  
    Pour plus d’informations sur les exigences de certificat de contrôleur de domaine, voir l’article 321051dans la Base de connaissances Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307)).  
  
    > [!NOTE]  
    > Les contrôleurs de domaine qui utilisent des certificats pour LDAPS pouvant vous obliger à redéployer leurs certificats. Lorsque vous procédez ainsi, contrôleurs de domaine ne peuvent pas sélectionner un certificat approprié jusqu'à ce qu’ils sont redémarrés. Pour plus d’informations sur l’authentification LDAPS et une mise à jour associée pour Windows Server2003, consultez l’article 932834dans la Base de connaissances Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308)).  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>Planification de déploiements de l’espace de noms disjoint  
Prenez les précautions suivantes si vous déployez des ordinateurs dans un environnement qui possède un espace de noms disjoint:  
  
1.  Notifier tous les éditeurs de logiciels avec lesquels vous travaillez qu’ils doivent tester et prendre en charge un espace de noms disjoint. Demandez-lui de vérifier qu’ils prennent en charge leurs applications dans les environnements qui utilisent des espaces de noms disjoint.  
  
2.  Testez toutes les versions des systèmes d’exploitation et des applications dans les environnements de laboratoire d’espace de noms disjoint. Lorsque vous le faites, suivez ces recommandations:  
  
    1.  Résoudre tous les problèmes de logiciel avant de déployer le logiciel dans votre environnement.  
  
    2.  Lorsque cela est possible, participer à des tests bêta des systèmes d’exploitation et des applications que vous envisagez de déployer des espaces de noms disjoint.  
  
3.  Assurez-vous que les administrateurs et le personnel du support technique est informé de l’espace de noms disjoint et son impact.  
  
4.  Créer un plan qui permet à vous pour effectuer la transition à partir d’un espace de noms disjoint à un espace de noms contigu, si nécessaire.  
  
5.  Sensibilisation l’importance de la prise en charge de l’espace de noms disjoint avec le système d’exploitation et aux fournisseurs d’applications.  
  


