---
ms.assetid: 4cc9c16c-1928-4dce-a3a8-6229be28eb65
title: Concepts de réplication Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d426b9923b569c8475862c1426a9dd310dc0b798
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390550"
---
# <a name="active-directory-replication-concepts"></a>Concepts de réplication Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avant de concevoir la topologie de site, familiarisez-vous avec certains concepts de réplication Active Directory.  
  
-   [Objet Connection](#BKMK_1)  
  
-   [KCC](#BKMK_2)  
  
-   [Fonctionnalité de basculement](#BKMK_3)  
  
-   [Subnet](#BKMK_4)  
  
-   [Site](#BKMK_5)  
  
-   [Lien de site](#BKMK_6)  
  
-   [Pont entre liens de sites](#BKMK_7)  
  
-   [Transitivité des liaisons de sites](#BKMK_8)  
  
-   [Serveur de catalogue global](#BKMK_9)  
  
-   [Mise en cache de l’appartenance au groupe universel](#BKMK_10)  
  
## <a name="BKMK_1"></a>Objet Connection  
Un objet de connexion est un objet Active Directory qui représente une connexion de réplication entre un contrôleur de domaine source et un contrôleur de domaine de destination. Un contrôleur de domaine est membre d’un site unique et est représenté dans le site par un objet serveur dans Active Directory Domain Services (AD DS). Chaque objet serveur possède un objet Paramètres NTDS enfant qui représente le contrôleur de domaine de réplication dans le site.  
  
L’objet de connexion est un enfant de l’objet Paramètres NTDS sur le serveur de destination. Pour que la réplication ait lieu entre deux contrôleurs de domaine, l’objet serveur d’un doit avoir un objet de connexion qui représente la réplication entrante de l’autre. Toutes les connexions de réplication d’un contrôleur de domaine sont stockées en tant qu’objets de connexion sous l’objet Paramètres NTDS. L’objet de connexion identifie le serveur source de réplication, contient une planification de réplication et spécifie un transport de réplication.  
  
Le vérificateur de cohérence des connaissances (KCC) crée automatiquement des objets de connexion, mais ils peuvent également être créés manuellement. Les objets de connexion créés par le KCC apparaissent dans le composant logiciel enfichable sites et services Active Directory en tant que **<automatically generated>** et sont considérés comme adéquats dans des conditions de fonctionnement normales. Les objets de connexion créés par un administrateur sont des objets de connexion créés manuellement. Un objet de connexion créé manuellement est identifié par le nom attribué par l’administrateur lors de sa création. Lorsque vous modifiez un objet **de connexion <automatically generated>** , vous le convertissez en un objet de connexion modifié administrativement et l’objet apparaît sous la forme d’un GUID. Le KCC n’apporte aucune modification aux objets de connexion manuels ou modifiés.  
  
## <a name="BKMK_2"></a>KCC  
Le KCC est un processus intégré qui s’exécute sur tous les contrôleurs de domaine et génère une topologie de réplication pour la forêt Active Directory. Le KCC crée des topologies de réplication distinctes selon que la réplication se produit au sein d’un site (intrasite) ou entre sites (intersite). Le KCC ajuste également dynamiquement la topologie pour s’adapter à l’ajout de nouveaux contrôleurs de domaine, à la suppression des contrôleurs de domaine existants, au déplacement de contrôleurs de domaine vers et à partir de sites, à la modification des coûts et des planifications et aux contrôleurs de domaine temporairement indisponible ou dans un état d’erreur.  
  
Au sein d’un site, les connexions entre les contrôleurs de domaine accessibles en écriture sont toujours organisées en anneau bidirectionnel, avec des connexions de raccourcis supplémentaires pour réduire la latence dans les sites de grande taille. En revanche, la topologie intersite est une superposition d’arborescences, ce qui signifie qu’une connexion intersite existe entre deux sites quelconques pour chaque partition d’annuaire et ne contient généralement pas de connexions de raccourci. Pour plus d’informations sur la répartition des arborescences et la topologie de réplication Active Directory, consultez Active Directory référence technique sur la topologie de réplication ([https://go.microsoft.com/fwlink/?LinkID=93578](https://go.microsoft.com/fwlink/?LinkID=93578)).  
  
Sur chaque contrôleur de domaine, le KCC crée des itinéraires de réplication en créant des objets de connexion entrante unidirectionnels qui définissent les connexions à partir d’autres contrôleurs de domaine. Pour les contrôleurs de domaine du même site, le KCC crée automatiquement des objets de connexion sans intervention de l’administrateur. Lorsque vous avez plusieurs sites, vous configurez des liaisons de sites entre les sites, et un seul KCC dans chaque site crée automatiquement des connexions entre les sites.  
  
### <a name="kcc-improvements-for-windows-server-2008-rodcs"></a>Améliorations du KCC pour Windows Server 2008 RODC  

Il existe un certain nombre d’améliorations du KCC pour prendre en charge le contrôleur de domaine en lecture seule (RODC) qui vient d’être disponible dans Windows Server 2008. Un scénario de déploiement type pour RODC est la filiale. La Active Directory topologie de réplication la plus couramment déployée dans ce scénario est basée sur une conception Hub-and-spoke, où les contrôleurs de domaine de succursale dans plusieurs sites répliquent avec un petit nombre de serveurs tête de pont dans un site hub.  
  
L’un des avantages du déploiement de RODC dans ce scénario est la réplication unidirectionnelle. Les serveurs tête de pont ne sont pas tenus de répliquer à partir du RODC, ce qui réduit l’administration et l’utilisation du réseau.  
  
Toutefois, l’un des défis administratifs mis en évidence par la topologie Hub-and-spoke sur les versions précédentes du système d’exploitation Windows Server est qu’après l’ajout d’un nouveau contrôleur de domaine tête de pont dans le Hub, il n’existe aucun mécanisme automatique pour redistribuer le connexions de réplication entre les contrôleurs de domaine de la succursale et les contrôleurs de domaine du concentrateur pour tirer parti du nouveau contrôleur de domaine Hub.  
  
Pour les contrôleurs de domaine Windows Server 2003, vous pouvez rééquilibrer la charge de travail à l’aide d’un outil tel que adlb. exe du Guide de déploiement de la succursale Windows Server 2003 ([https://go.microsoft.com/fwlink/?LinkID=28523](https://go.microsoft.com/fwlink/?LinkID=28523)).  
  
Pour Windows Server 2008 RODC, le fonctionnement normal du KCC fournit un certain rééquilibrage, ce qui élimine la nécessité d’utiliser un outil supplémentaire tel que adlb. exe. La nouvelle fonctionnalité est activée par défaut. Vous pouvez la désactiver en ajoutant la clé de Registre suivante définie sur le contrôleur de domaine en lecture seule :  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters**  
  
**« LoadBalancing BH Random allowed »**  
**1 = activé (valeur par défaut), 0 = désactivé**  
  
Pour plus d’informations sur le fonctionnement de ces améliorations du KCC, consultez Planification et déploiement d’Active Directory Domain Services pour les filiales ([https://go.microsoft.com/fwlink/?LinkId=107114](https://go.microsoft.com/fwlink/?LinkId=107114)).  
  
## <a name="BKMK_3"></a>Fonctionnalité de basculement  
Les sites garantissent que la réplication est acheminée autour des pannes réseau et des contrôleurs de domaine hors connexion. Le KCC s’exécute à des intervalles spécifiés pour ajuster la topologie de réplication pour les modifications apportées dans AD DS, par exemple quand de nouveaux contrôleurs de domaine sont ajoutés et que de nouveaux sites sont créés. Le KCC examine l’état de réplication des connexions existantes pour déterminer si des connexions ne fonctionnent pas. Si une connexion ne fonctionne pas en raison d’un contrôleur de domaine défaillant, le KCC crée automatiquement des connexions temporaires à d’autres partenaires de réplication (si disponibles) pour s’assurer que la réplication a lieu. Si tous les contrôleurs de domaine d’un site ne sont pas disponibles, le KCC crée automatiquement des connexions de réplication entre les contrôleurs de domaine d’un autre site.  
  
## <a name="BKMK_4"></a>Subnet  
Un sous-réseau est un segment d’un réseau TCP/IP auquel un ensemble d’adresses IP logiques sont attribuées. Les sous-réseaux groupent les ordinateurs d’une manière qui identifie leur proximité physique sur le réseau. Les objets de sous-réseau dans AD DS identifient les adresses réseau utilisées pour mapper les ordinateurs aux sites.  
  
## <a name="BKMK_5"></a>Site  
Les sites sont des objets Active Directory qui représentent un ou plusieurs sous-réseaux TCP/IP avec des connexions réseau très fiables et rapides. Les informations de site permettent aux administrateurs de configurer l’accès et la réplication Active Directory pour optimiser l’utilisation du réseau physique. Les objets de site sont associés à un ensemble de sous-réseaux, et chaque contrôleur de domaine d’une forêt est associé à un site Active Directory en fonction de son adresse IP. Les sites peuvent héberger des contrôleurs de domaine à partir de plusieurs domaines, et un domaine peut être représenté dans plusieurs sites.  
  
## <a name="BKMK_6"></a>Lien de site  
Les liens de sites sont des objets Active Directory qui représentent les chemins logiques que le KCC utilise pour établir une connexion pour la réplication Active Directory. Un objet lien de sites représente un ensemble de sites qui peuvent communiquer à un coût uniforme via un transport intersite spécifié.  
  
Tous les sites contenus dans le lien de sites sont considérés comme étant connectés par le biais du même type de réseau. Les sites doivent être liés manuellement à d’autres sites à l’aide de liens de sites, de sorte que les contrôleurs de domaine d’un site peuvent répliquer les modifications d’annuaire à partir des contrôleurs de domaine d’un autre site. Étant donné que les liens de sites ne correspondent pas au chemin d’accès réel pris par les paquets réseau sur le réseau physique lors de la réplication, vous n’avez pas besoin de créer des liens de site redondants pour améliorer Active Directory efficacité de réplication.  
  
Lorsque deux sites sont connectés par un lien de sites, le système de réplication crée automatiquement des connexions entre des contrôleurs de domaine spécifiques de chaque site appelés serveurs tête de pont. Dans Windows Server 2008, tous les contrôleurs de domaine d’un site qui hébergent la même partition d’annuaire peuvent être sélectionnés en tant que serveurs tête de pont. Les connexions de réplication créées par le KCC sont distribuées de façon aléatoire parmi tous les serveurs tête de pont candidats d’un site pour partager la charge de travail de réplication. Par défaut, le processus de sélection aléatoire n’a lieu qu’une seule fois, lorsque les objets de connexion sont ajoutés pour la première fois au site.  
  
## <a name="BKMK_7"></a>Pont entre liens de sites  
Un pont de liaison de site est un objet Active Directory qui représente un ensemble de liens de sites, tous les sites dont les sites peuvent communiquer à l’aide d’un transport commun. Les ponts entre liens de sites activent les contrôleurs de domaine qui ne sont pas directement connectés au moyen d’une liaison de communication pour se répliquer entre eux. En règle générale, un pont entre liens de sites correspond à un routeur (ou à un ensemble de routeurs) sur un réseau IP.  
  
Par défaut, le KCC peut former un itinéraire transitif via tous les liens de sites qui ont certains sites en commun. Si ce comportement est désactivé, chaque lien de sites représente son propre réseau distinct et isolé. Les ensembles de liens de sites qui peuvent être traités comme un seul itinéraire sont exprimés à l’aide d’un pont entre liens de sites. Chaque pont représente un environnement de communication isolé pour le trafic réseau.  
  
Les ponts entre liens de sites sont un mécanisme de représentation logique de la connectivité physique transitive entre les sites. Un pont entre liens de sites permet au KCC d’utiliser n’importe quelle combinaison des liens de sites inclus pour déterminer l’itinéraire le moins coûteux pour interconnecter les partitions d’annuaire contenues dans ces sites. Le pont lien de sites ne fournit pas de connectivité réelle aux contrôleurs de domaine. Si le pont lien de sites est supprimé, la réplication sur les liens de sites combinés se poursuit jusqu’à ce que le KCC supprime les liens.  
  
Les ponts entre liens de sites ne sont nécessaires que si un site contient un contrôleur de domaine qui héberge une partition d’annuaire qui n’est pas également hébergée sur un contrôleur de domaine dans un site adjacent, mais un contrôleur de domaine qui héberge cette partition d’annuaire se trouve dans un ou plusieurs autres sites dans forêt. Les sites adjacents sont définis en tant qu’un ou plusieurs sites inclus dans un lien de site unique.  
  
Un pont entre liens de sites crée une connexion logique entre deux liens de sites, fournissant un chemin transitif entre deux sites déconnectés à l’aide d’un site intermédiaire. Dans le cadre du générateur de topologie intersite (ISTG), le pont implique une connectivité physique à l’aide du site intermédiaire. Le pont ne signifie pas qu’un contrôleur de domaine du site intermédiaire fournira le chemin de réplication. Toutefois, ce serait le cas si le site intermédiaire contenait un contrôleur de domaine qui hébergeait la partition d’annuaire à répliquer, auquel cas un pont de liaison de site n’est pas requis.  
  
Le coût de chaque lien de sites est ajouté, ce qui crée un coût total pour le chemin d’accès résultant. Le pont lien de sites est utilisé si le site temporaire ne contient pas de contrôleur de domaine qui héberge la partition d’annuaire et qu’il n’existe pas de lien de coût inférieur. Si le site temporaire contient un contrôleur de domaine qui héberge la partition d’annuaire, deux sites déconnectés configurent des connexions de réplication vers le contrôleur de domaine temporaire et n’utilisent pas le pont.  
  
## <a name="BKMK_8"></a>Transitivité des liaisons de sites  
Par défaut, tous les liens de sites sont transitifs, ou « pontés ». Lorsque les liens de sites sont reliés et que les planifications se chevauchent, le KCC crée des connexions de réplication qui déterminent les partenaires de réplication de contrôleur de domaine entre les sites, où les sites ne sont pas directement connectés par des liens de sites, mais qui sont connectés de manière transitive via ensemble de sites communs. Cela signifie que vous pouvez connecter n’importe quel site à n’importe quel autre site par le biais d’une combinaison de liens de sites.  
  
En général, pour un réseau entièrement routé, vous n’avez pas besoin de créer de ponts entre liens de sites, sauf si vous souhaitez contrôler le déroulement des modifications de réplication. Si votre réseau n’est pas entièrement routé, des ponts entre liens de sites doivent être créés pour éviter des tentatives de réplication impossibles. Tous les liens de sites pour un transport spécifique appartiennent implicitement à un seul pont de liaison de site pour ce transport. Le pontage par défaut pour les liens de sites se produit automatiquement, et aucun objet Active Directory représente ce pont. Le paramètre **relier tous les liens de sites** , qui se trouve dans les propriétés des conteneurs de transport intersite SMTP et SMTP (Simple Mail Transfer Protocol), implémente le pontage automatique des liaisons de sites.  
  
> [!NOTE]  
> La réplication SMTP ne sera pas prise en charge dans les futures versions de AD DS ; par conséquent, il n’est pas recommandé de créer des objets de liens de sites dans le conteneur SMTP.  
  
## <a name="BKMK_9"></a>Serveur de catalogue global  
Un serveur de catalogue global est un contrôleur de domaine qui stocke des informations sur tous les objets de la forêt, afin que les applications puissent effectuer des recherches AD DS sans faire référence à des contrôleurs de domaine spécifiques qui stockent les données demandées. Comme tous les contrôleurs de domaine, un serveur de catalogue global stocke des réplicas complets et accessibles en écriture du schéma et des partitions d’annuaire de configuration, ainsi qu’un réplica complet et accessible en écriture de la partition d’annuaire du domaine pour le domaine qu’il héberge. En outre, un serveur de catalogue global stocke un réplica partiel en lecture seule de tous les autres domaines de la forêt. Les réplicas de domaine en lecture seule partiels contiennent chaque objet dans le domaine, mais seulement un sous-ensemble des attributs (les attributs les plus couramment utilisés pour la recherche de l’objet).  
  
## <a name="BKMK_10"></a>Mise en cache de l’appartenance au groupe universel  
La mise en cache de l’appartenance au groupe universel permet au contrôleur de domaine de mettre en cache les informations d’appartenance au groupe universel pour les utilisateurs. Vous pouvez activer les contrôleurs de domaine qui exécutent Windows Server 2008 pour mettre en cache les appartenances aux groupes universels à l’aide du composant logiciel enfichable sites et services Active Directory.  
  
L’activation de la mise en cache de l’appartenance aux groupes universels élimine la nécessité de disposer d’un serveur de catalogue global sur chaque site d’un domaine, ce qui réduit l’utilisation de la bande passante réseau, car un contrôleur de domaine n’a pas besoin de répliquer tous les objets situés dans la forêt. Cela réduit également les délais d’ouverture de session, car les contrôleurs de domaine d’authentification n’ont pas toujours besoin d’accéder à un catalogue global pour obtenir des informations sur l’appartenance au groupe universel. Pour plus d’informations sur l’utilisation de la mise en cache de l’appartenance au groupe universel, consultez Planification de l' [emplacement du serveur de catalogue global](../../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md).  
  


