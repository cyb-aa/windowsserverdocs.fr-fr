---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: Espace de noms dissocié
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c402e5519bc0e5c37cb6d3818c8def40d98d49e5
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624257"
---
# <a name="disjoint-namespace"></a>Espace de noms dissocié

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un espace de noms disjoint se produit lorsqu’un ou plusieurs ordinateurs membres du domaine ont un suffixe DNS (Domain Name Service) principal qui ne correspond pas au nom DNS du domaine Active Directory dont les ordinateurs sont membres. Par exemple, un ordinateur membre qui utilise un suffixe DNS principal corp.fabrikam.com dans un domaine Active Directory nommé na.corp.fabrikam.com utilise un espace de noms disjoint.

Un espace de noms disjoint est plus complexe à administrer, à gérer et à dépanner qu’un espace de noms contigu. Dans un espace de noms contigu, le suffixe DNS principal correspond au nom de domaine Active Directory. Les applications réseau qui sont écrites pour supposer que l’espace de noms Active Directory est identique au suffixe DNS principal pour tous les ordinateurs membres du domaine ne fonctionnent pas correctement dans un espace de noms disjoint.

## <a name="support-for-disjoint-namespaces"></a>Prise en charge des espaces de noms disjoint

Les ordinateurs membres du domaine, y compris les contrôleurs de domaine, peuvent fonctionner dans un espace de noms disjoint. Les ordinateurs membres du domaine peuvent inscrire leur enregistrement de ressource hôte (A) et l’enregistrement de ressource d’hôte IPv6 (AAAA) dans un espace de noms DNS disjoint. Lorsque les ordinateurs membres du domaine inscrivent leurs enregistrements de ressources de cette manière, les contrôleurs de domaine continuent d’inscrire les enregistrements de ressource de service (SRV) globaux et spécifiques au site dans la zone DNS qui est identique au nom de domaine Active Directory.

Par exemple, supposons qu’un contrôleur de domaine pour le domaine Active Directory nommé na.corp.fabrikam.com qui utilise un suffixe DNS principal de corp.fabrikam.com enregistre les enregistrements de ressource d’hôte (A) et d’hôte IPv6 (AAAA) dans la zone DNS corp.fabrikam.com. Le contrôleur de domaine continue à enregistrer les enregistrements de ressource de service (SRV) globaux et spécifiques aux sites dans les zones DNS _msdcs. na. Corp. fabrikam. com et na.corp.fabrikam.com, ce qui rend possible l’emplacement du service.

> [!IMPORTANT]
> Bien que les systèmes d’exploitation Windows puissent prendre en charge un espace de noms disjoint, les applications qui sont écrites pour supposer que le suffixe DNS principal est le même que le suffixe de domaine Active Directory peut ne pas fonctionner dans un tel environnement. Pour cette raison, vous devez tester soigneusement toutes les applications et leurs systèmes d’exploitation respectifs avant de déployer un espace de noms disjoint.

Un espace de noms disjoint doit fonctionner (et est pris en charge) dans les cas suivants :

- Lorsqu’une forêt avec plusieurs domaines de Active Directory utilise un seul espace de noms DNS, qui est également appelé zone DNS

    Par exemple, une entreprise qui utilise des domaines régionaux avec des noms tels que na.corp.fabrikam.com, sa.corp.fabrikam.com et asia.corp.fabrikam.com et utilise un espace de noms DNS unique, tel que corp.fabrikam.com.

- Lorsqu’un domaine Active Directory unique est fractionné en espaces de noms DNS distincts

    C’est le cas, par exemple, d’une société disposant d’un domaine Active Directory corp.contoso.com qui utilise des zones DNS telles que hr.corp.contoso.com, production.corp.contoso.com et it.corp.contoso.com.

Un espace de noms disjoint ne fonctionne pas correctement (et n’est pas pris en charge) dans les cas suivants :

- Un suffixe disjoint utilisé par les membres du domaine correspond à un nom de domaine Active Directory dans cette forêt ou dans une autre forêt. Cela interrompt le routage du suffixe de nom Kerberos.

- Le même suffixe disjoint est utilisé dans une autre forêt. Cela empêche le routage de ces suffixes de manière unique entre les forêts.

- Lorsqu’un serveur d’autorité de certification de membre de domaine modifie son nom de domaine complet (FQDN) afin qu’il n’utilise plus le même suffixe DNS principal utilisé par les contrôleurs de domaine du domaine auquel le serveur d’autorité de certification est membre. Dans ce cas, vous risquez de rencontrer des problèmes de validation des certificats émis par le serveur de l’autorité de certification, en fonction des noms DNS utilisés dans les points de distribution de la liste de révocation de certificats. Toutefois, si vous placez un serveur d’autorité de certification dans un espace de noms disjoint stable, il fonctionne correctement et est pris en charge.

## <a name="considerations-for-disjoint-namespaces"></a>Considérations relatives aux espaces de noms disjoint

Les considérations suivantes peuvent vous aider à déterminer si vous devez utiliser un espace de noms disjoint.

### <a name="application-compatibility"></a>Compatibilité des applications

Comme mentionné précédemment, un espace de noms disjoint peut provoquer des problèmes pour les applications et services écrits pour supposer qu’un suffixe DNS principal de l’ordinateur est identique au nom du domaine dont il est membre. Avant de déployer un espace de noms disjoint, vous devez vérifier les problèmes de compatibilité des applications. Veillez également à vérifier la compatibilité de toutes les applications que vous utilisez lorsque vous effectuez votre analyse. Cela comprend les applications de Microsoft et d’autres développeurs de logiciels.

### <a name="advantages-of-disjoint-namespaces"></a>Avantages des espaces de noms disjoint

L’utilisation d’un espace de noms disjoint peut présenter les avantages suivants :

- Étant donné que le suffixe DNS principal d’un ordinateur peut indiquer des informations différentes, vous pouvez gérer l’espace de noms DNS séparément du nom de domaine Active Directory.

- Vous pouvez séparer l’espace de noms DNS en fonction de la structure métier ou de l’emplacement géographique. Par exemple, vous pouvez séparer l’espace de noms en fonction des noms de divisions ou de l’emplacement physique, tels que le continent, le pays/la région ou la génération.

### <a name="disadvantages-of-disjoint-namespaces"></a>Inconvénients des espaces de noms disjoint

L’utilisation d’un espace de noms disjoint peut présenter les inconvénients suivants :

- Vous devez créer et gérer des zones DNS distinctes pour chaque Active Directory domaine de la forêt qui contient des ordinateurs membres qui utilisent un espace de noms disjoint. (Autrement dit, il requiert une configuration supplémentaire et plus complexe.)

- Vous devez effectuer les étapes manuelles pour modifier et gérer l’attribut Active Directory qui permet aux membres du domaine d’utiliser des suffixes DNS principaux spécifiés.

- Pour optimiser la résolution de noms, vous devez effectuer des étapes manuelles pour modifier et gérer des stratégie de groupe pour configurer des ordinateurs membres avec d’autres suffixes DNS principaux.

> [!NOTE]
> Le service WINS (Windows Internet Name Service) peut être utilisé pour décaler cet inconvénient en résolvant les noms en une partie. Pour plus d’informations sur WINS, consultez la [référence technique WINS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc736411(v=ws.10)).

- Lorsque votre environnement requiert plusieurs suffixes DNS principaux, vous devez configurer l’ordre de recherche de suffixe DNS pour tous les domaines de Active Directory de la forêt de manière appropriée.

    Pour définir l’ordre de recherche du suffixe DNS, vous pouvez utiliser des objets stratégie de groupe ou des paramètres du service serveur DHCP (Dynamic Host Configuration Protocol). Vous pouvez également modifier le registre.

- Vous devez soigneusement tester les problèmes de compatibilité de toutes les applications.

Pour plus d’informations sur les étapes à suivre pour résoudre ces inconvénients, consultez [créer un espace de noms disjoint](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc755926(v=ws.10)).

### <a name="planning-a-namespace-transition"></a>Planification d’une transition d’espace de noms

Avant de modifier un espace de noms, passez en revue les considérations suivantes, qui s’appliquent aux transitions d’espaces de noms contigus à des espaces de noms disjoints (ou inversement) :

- Les noms de principal du service (SPN) configurés manuellement peuvent ne plus correspondre aux noms DNS après la modification d’un espace de noms. Cela peut entraîner des échecs d’authentification.

    Pour plus d’informations, consultez [échec des ouvertures de session de service en raison de la définition incorrecte de SPN](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc772897(v=ws.10)).

    - Si vous utilisez des ordinateurs Windows Server 2003 avec la délégation avec restriction, ces ordinateurs peuvent nécessiter une configuration supplémentaire pour modifier les noms de principal du service. Pour plus d’informations, voir l’article 936628 de la base de connaissances Microsoft, [le nom de principal du service n’apparaît pas dans la liste des services qui peuvent être délégués à un compte quand vous essayez de configurer la délégation avec restriction sur un ordinateur qui exécute Windows Server 2003](https://support.microsoft.com/help/936628) (404).

    - Si vous souhaitez déléguer des autorisations pour modifier les noms de principal du service (SPN) pour les administrateurs subordonnés, consultez [délégation d’autorité pour modifier les SPN](https://technet.microsoft.com/library/cc772895(WS.10).aspx).

- Si vous utilisez le protocole LDAP (Lightweight Directory Access Protocol) sur protocole SSL (SSL) (appelé LDAPs) avec une autorité de certification dans un déploiement dont les contrôleurs de domaine sont configurés dans un espace de noms disjoint, vous devez utiliser le nom de domaine Active Directory approprié et le suffixe DNS principal lorsque vous configurez les certificats LDAPs.

    Pour plus d’informations sur les exigences relatives aux certificats de contrôleur de domaine, voir l’article 321051 de la base de connaissances Microsoft, [How to Enable LDAP with SSL with a tiers Certification Authority](https://support.microsoft.com/help/321051/).

    > [!NOTE]
    > Les contrôleurs de domaine qui utilisent des certificats pour LDAPs peuvent vous obliger à redéployer leurs certificats. Dans ce cas, les contrôleurs de domaine ne peuvent pas sélectionner un certificat approprié tant qu’ils n’ont pas été redémarrés. Pour plus d’informations sur l’authentification LDAP (Lightweight Directory Access Protocol) sur protocole SSL (SSL) (LDAPs) pour Windows Server 2003, consultez l’article 938703 de la base de connaissances Microsoft, [Comment résoudre les problèmes de connexion LDAP sur SSL](https://support.microsoft.com/help/938703/).

### <a name="planning-for-disjoint-namespace-deployments"></a>Planification des déploiements d’espaces de noms disjoint

Si vous déployez des ordinateurs dans un environnement doté d’un espace de noms disjoint, prenez les précautions suivantes :

1. Informez tous les éditeurs de logiciels avec lesquels vous faites des affaires qu’ils doivent tester et prendre en charge un espace de noms disjoint. Demandez-lui de vérifier qu’ils prennent en charge leurs applications dans des environnements qui utilisent des espaces de noms disjoint.

2. Testez toutes les versions des systèmes d’exploitation et des applications dans des environnements de laboratoire d’espaces de noms disjoints. Dans ce cas, suivez ces recommandations :

    1. Résolvez tous les problèmes logiciels avant de déployer le logiciel dans votre environnement.

    2. Dans la mesure du possible, participez aux tests bêta des systèmes d’exploitation et des applications que vous envisagez de déployer dans des espaces de noms disjoint.

3. Assurez-vous que les administrateurs et le personnel du support technique prennent connaissance de l’espace de noms disjoint et de son impact.

4. Créez un plan qui vous permet de passer d’un espace de noms disjoint à un espace de noms contigu, si nécessaire.

5. Promeut l’importance de la prise en charge des espaces de noms disjoints avec le système d’exploitation et les fournisseurs d’applications.
