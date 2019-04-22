---
ms.assetid: 4cc9c16c-1928-4dce-a3a8-6229be28eb65
title: Concepts de réplication Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 087dee7c914b81d97c6cf3a2ea985d809cb57fe6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812350"
---
# <a name="active-directory-replication-concepts"></a>Concepts de réplication Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avant de concevoir la topologie de site, vous devez vous familiariser avec certains concepts de réplication Active Directory.  
  
-   [Objet de connexion](#BKMK_1)  
  
-   [KCC](#BKMK_2)  
  
-   [Fonctionnalité de basculement](#BKMK_3)  
  
-   [Sous-réseau](#BKMK_4)  
  
-   [Site](#BKMK_5)  
  
-   [Lien de sites](#BKMK_6)  
  
-   [Pont lien de sites](#BKMK_7)  
  
-   [Transitivité de lien de site](#BKMK_8)  
  
-   [Serveur de catalogue global](#BKMK_9)  
  
-   [La mise en cache de l’appartenance au groupe universel](#BKMK_10)  
  
## <a name="BKMK_1"></a>Objet de connexion  
Un objet de connexion est un objet Active Directory qui représente une connexion de réplication à partir d’un contrôleur de domaine source à un contrôleur de domaine de destination. Un contrôleur de domaine est membre d’un site unique et est représenté dans le site par un objet serveur dans les Services de domaine Active Directory (AD DS). Chaque objet de serveur possède un enfant de l’objet Paramètres NTDS qui représente le contrôleur de domaine de réplication dans le site.  
  
L’objet de connexion est un enfant de l’objet Paramètres NTDS sur le serveur de destination. Pour la réplication entre deux contrôleurs de domaine, l’objet de serveur d’un doit avoir un objet de connexion représentant la réplication entrante à partir de l’autre. Toutes les connexions de réplication pour un contrôleur de domaine sont stockées en tant qu’objets de connexion sous l’objet Paramètres NTDS. L’objet de connexion identifie le serveur source de réplication contienne une planification de réplication et spécifie un transport de réplication.  
  
Le vérificateur de cohérence des connaissances (KCC) crée automatiquement des objets de connexion, mais ils peuvent également être créés manuellement. Les objets de connexion créés par le KCC apparaissent dans les Sites Active Directory et le composant logiciel enfichable Services en tant que **<automatically generated>** et sont considérés comme adéquates dans des conditions de fonctionnement normales. Les objets de connexion créés par un administrateur sont créés manuellement les objets de connexion. Un objet de connexion créés manuellement est identifié par le nom attribué par l’administrateur lors de sa création. Lorsque vous modifiez un **<automatically generated>** des objets de connexion, vous le convertir en un objet de connexion modifiée au niveau administratif et de l’objet s’affiche sous la forme d’un GUID. Le KCC ne pas apporte des modifications aux objets de connexion manuelle ou modifiés.  
  
## <a name="BKMK_2"></a>KCC  
Le KCC est un processus intégré qui s’exécute sur tous les contrôleurs de domaine et génère la topologie de réplication pour la forêt Active Directory. Le KCC crée des topologies de réplication distinctes selon que la réplication a lieu au sein d’un site (intrasite) ou entre des sites (intersite). Le KCC ajuste également de façon dynamique la topologie pour permettre l’ajout de nouveaux contrôleurs de domaine, la suppression des contrôleurs de domaine existants, le déplacement des contrôleurs de domaine vers et à partir de sites, en modifiant les coûts et les planifications et les contrôleurs de domaine qui sont temporairement indisponible ou dans un état d’erreur.  
  
Au sein d’un site, les connexions entre les contrôleurs de domaine accessible en écriture sont toujours disposées dans un anneau bidirectionnel, avec des connexions supplémentaires raccourci pour réduire la latence dans les sites de grande taille. En revanche, la topologie intersite est une superposition de répartition des arborescences, qui signifie qu’une connexion intersite existe entre les deux sites pour chaque partition d’annuaire et ne contient-elle généralement pas de raccourcis de connexion. Pour plus d’informations sur le fractionnement des arborescences et la topologie de réplication Active Directory, voir Active Directory Replication topologie informations techniques de référence ([https://go.microsoft.com/fwlink/?LinkID=93578](https://go.microsoft.com/fwlink/?LinkID=93578)).  
  
Sur chaque contrôleur de domaine, le KCC crée des itinéraires de réplication en créant des objets de connexion entrante à sens unique qui définissent les connexions à partir d’autres contrôleurs de domaine. Pour les contrôleurs de domaine dans le même site, le KCC crée des objets de connexion automatiquement sans intervention de l’administrateur. Lorsque vous avez plusieurs sites, vous configurez des liens de sites entre les sites, et un KCC unique dans chaque site crée automatiquement des connexions entre les sites également.  
  
### <a name="kcc-improvements-for-windows-server-2008-rodcs"></a>Améliorations des données pour les RODC de Windows Server 2008  

Il existe un certain nombre d’améliorations de KCC pour prendre en charge le contrôleur qui vient d’être disponible domaine en lecture seule (RODC) dans Windows Server 2008. Un scénario de déploiement classique de RODC est la succursale. La topologie de réplication Active Directory couramment déployée dans ce scénario repose sur une conception hub-and-spoke, où les contrôleurs de domaine de succursale dans plusieurs sites répliqueront avec un petit nombre de serveurs de tête de pont dans un site concentrateur.  
  
Un des avantages du déploiement de RODC dans ce scénario est la réplication unidirectionnelle. Serveurs tête de pont n’êtes pas obligés de répliquer à partir de RODC, ce qui réduit d’administration et de l’utilisation du réseau.  
  
Toutefois, l’une des difficultés d’administration mis en surbrillance par la topologie hub-and-spoke dans les versions précédentes du système d’exploitation Windows Server sont qu’après avoir ajouté un nouveau contrôleur de domaine de tête de pont dans le hub, il n’existe aucun mécanisme automatique pour redistribuer le connexions de réplication entre les contrôleurs de domaine des filiales et les contrôleurs de domaine pour tirer parti du nouveau contrôleur de domaine du concentrateur.  
  
Pour les contrôleurs de domaine Windows Server 2003, vous pouvez rééquilibrer la charge de travail à l’aide d’un outil tel que Adlb.exe à partir de Windows Server 2003 Branch Office Deployment Guide ([https://go.microsoft.com/fwlink/?LinkID=28523](https://go.microsoft.com/fwlink/?LinkID=28523)).  
  
Pour les RODC de Windows Server 2008, le fonctionnement normal du KCC fournit certains rééquilibrage, ce qui élimine la nécessité d’utiliser un outil tel que Adlb.exe supplémentaire. La nouvelle fonctionnalité est activée par défaut. Vous pouvez le désactiver en ajoutant la clé de Registre suivante définie sur le RODC :  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters**  
  
**« BH aléatoire l’équilibrage de charge autorisée »**  
**1 = activé (valeur par défaut), 0 = désactivé**  
  
Pour plus d’informations sur le fonctionnement de ces améliorations des données, consultez Planification et déploiement d’Active Directory Domain Services pour les succursales ([https://go.microsoft.com/fwlink/?LinkId=107114](https://go.microsoft.com/fwlink/?LinkId=107114)).  
  
## <a name="BKMK_3"></a>Fonctionnalité de basculement  
Sites garantissent que la réplication est routée autour des défaillances du réseau et contrôleurs de domaine hors connexion. Le KCC s’exécute à des intervalles spécifiés pour ajuster la topologie de réplication pour les modifications qui se produisent dans les services AD DS, telles que lorsque les nouveaux contrôleurs de domaine sont ajoutés et les nouveaux sites sont créés. Le KCC passe en revue l’état de réplication des connexions existantes pour déterminer si toutes les connexions ne fonctionnent pas. Si une connexion ne fonctionne pas en raison d’un contrôleur de domaine ayant échoué, le KCC génère automatiquement des connexions temporaires à d’autres partenaires de réplication (si disponible) pour vous assurer que la réplication se produit. Si tous les contrôleurs de domaine dans un site ne sont pas disponibles, le KCC crée automatiquement les connexions de réplication entre les contrôleurs de domaine à partir d’un autre site.  
  
## <a name="BKMK_4"></a>Sous-réseau  
Un sous-réseau est un segment d’un réseau TCP/IP à laquelle un ensemble d’adresses IP logiques sont affectés. Sous-réseaux regroupement les ordinateurs d’une manière qui identifie leur proximité physique sur le réseau. Dans AD DS, les objets de sous-réseaux identifient les adresses de réseau qui sont utilisés pour mapper les ordinateurs à des sites.  
  
## <a name="BKMK_5"></a>Site  
Les sites sont des objets Active Directory qui représentent un ou plusieurs sous-réseaux TCP/IP avec des connexions réseau hautement fiables et rapides. Informations de site permettent aux administrateurs de configurer l’accès à Active Directory et la réplication pour optimiser l’utilisation du réseau physique. Objets de site sont associés à un ensemble de sous-réseaux, et chaque contrôleur de domaine dans une forêt est associé à un site Active Directory en fonction de son adresse IP. Sites peuvent héberger des contrôleurs de domaine à partir de plusieurs domaines, et un domaine peut être représenté dans plusieurs sites.  
  
## <a name="BKMK_6"></a>Lien de sites  
Liens de sites sont des objets Active Directory qui représentent les chemins d’accès logiques qui le KCC utilise pour établir une connexion pour la réplication Active Directory. Un objet de lien de site représente un ensemble de sites qui peuvent communiquer à un coût uniforme via un transport intersite spécifié.  
  
Tous les sites contenus dans le lien de site sont considérés comme être reliés par le même type de réseau. Sites doivent être liées manuellement vers d’autres sites à l’aide des liens de sites afin que les contrôleurs de domaine dans un site peuvent répliquer les modifications d’annuaire à partir de contrôleurs de domaine dans un autre site. Étant donné que les liens de sites ne correspondent pas au chemin d’accès réel effectuée par les paquets réseau sur le réseau physique au cours de la réplication, il est inutile de créer des liens de site redondant pour améliorer l’efficacité de la réplication Active Directory.  
  
Quand deux sites sont connectés par un lien de site, le système de réplication crée automatiquement les connexions entre les contrôleurs de domaine spécifique dans chaque site qui sont appelés serveurs tête de pont. Dans Windows Server 2008, tous les contrôleurs de domaine dans un site qui hébergent la même partition d’annuaire sont des candidats pour être sélectionné en tant que serveurs de tête de pont. Les connexions de réplication créées par le KCC sont distribuées de manière aléatoire parmi tous les serveurs de tête de pont de candidat dans un site pour partager la charge de travail de réplication. Par défaut, la sélection aléatoire processus intervient une seule fois, lorsque les objets de connexion sont tout d’abord ajoutés au site.  
  
## <a name="BKMK_7"></a>Pont lien de sites  
Un pont lien de sites est un objet Active Directory qui représente un ensemble de liens de site, tous dont sites peuvent communiquer à l’aide d’un transport couramment. Ponts permettent des contrôleurs de domaine qui ne sont pas directement connectées au moyen d’un lien de communication pour répliquer entre eux. En règle générale, un pont entre liens de site correspond à un routeur (ou un ensemble de routeurs) sur un réseau IP.  
  
Par défaut, le KCC peut former un itinéraire transitif via les liens de tous les sites qui ont en commun de certains sites. Si ce comportement est désactivé, chaque lien de site représente son propre réseau distinct et isolé. Ensembles de liens de sites qui peuvent être traités comme un seul itinéraire sont exprimées via un pont lien de sites. Chaque pont représente un environnement isolé de communication pour le trafic réseau.  
  
Ponts entre liens de site sont un mécanisme pour représenter logiquement transitive connectivité physique entre les sites. Un pont lien de sites permet le KCC à utiliser n’importe quelle combinaison des liaisons de sites inclus pour déterminer l’itinéraire le moins coûteux pour interconnecter des partitions d’annuaire contenues dans ces sites. Le pont lien de sites ne fournit pas de connectivité réelle sur les contrôleurs de domaine. Si le pont de liaison de site est supprimé, la réplication sur les liens de sites combiné continuera jusqu'à ce que le KCC supprime les liens.  
  
Ponts entre liens de site sont nécessaires uniquement si un site contient un contrôleur de domaine qui héberge une partition d’annuaire qui n’est pas également hébergée sur un contrôleur de domaine dans un site adjacent, mais un contrôleur de domaine qui héberge la partition de répertoire se trouve dans un ou plusieurs sites dans la forêt. Les sites adjacents sont définis en tant que tous les deux ou plusieurs sites inclus dans un lien de sites unique.  
  
Un pont entre liens de site crée une connexion logique entre deux liens de sites, en fournissant un chemin d’accès transitive entre deux sites déconnectés à l’aide d’un site temporaire. Dans le cadre du Générateur de topologie intersite (ISTG), le pont implique la connectivité physique à l’aide du site intermédiaire. Le pont n’implique pas qu’un contrôleur de domaine dans le site intermédiaire fournira le chemin d’accès de la réplication. Toutefois, cela serait être le cas si le site intermédiaire contenait un contrôleur de domaine hébergeant la partition d’annuaire à répliquer, auquel cas un pont lien de sites n’est pas requis.  
  
Le coût de chaque liaison de site est ajouté, la création d’un coût additionné pour le chemin d’accès résultant. Le pont lien de sites doit être utilisé si le site intermédiaire ne contient pas un contrôleur de domaine qui héberge la partition d’annuaire et un lien de coût inférieur n’existe pas. Si le site intermédiaire contenait un contrôleur de domaine qui héberge la partition d’annuaire, deux sites déconnectés configuriez des connexions de réplication pour le contrôleur de domaine intermédiaire et n’utilisez pas le pont.  
  
## <a name="BKMK_8"></a>Transitivité de lien de site  
Par défaut, tous les liens de site sont transitives ou « transition ». Lorsque des liens de sites sont affectés d’un pont et les planifications se chevauchent, le KCC crée des connexions de réplication qui déterminent les partenaires de réplication de contrôleur de domaine entre les sites, où les sites ne sont pas directement connectés via des liens de sites, mais sont connectés de manière transitive via un ensemble de sites courants. Cela signifie que vous pouvez vous connecter n’importe quel site sur un autre site via une combinaison de liens de sites.  
  
En règle générale, pour un réseau entièrement routé, il est inutile créer des ponts entre liens de n’importe quel site, sauf si vous souhaitez contrôler le flux des modifications de réplication. Si votre réseau n’est pas entièrement routé, les ponts entre liens de site doivent être créés pour éviter les tentatives de réplication impossible. Tous les liens de site pour un transport spécifique appartiennent implicitement à un pont de liaison de site unique pour ce transport. Le pontage par défaut pour les liens de sites se produit automatiquement, et aucun objet Active Directory ne représente ce pont. Le **relier tous les liens de site** paramètre, disponible dans les propriétés de l’adresse IP et le transfert de protocole SMTP (Simple Mail) conteneurs transport intersite, implémente de pont lien de site automatique.  
  
> [!NOTE]  
> La réplication SMTP n’est pas pris en charge dans les futures versions des services AD DS ; Par conséquent, la création d’objets de liens de site dans le conteneur SMTP n’est pas recommandée.  
  
## <a name="BKMK_9"></a>Serveur de catalogue global  
Un serveur de catalogue global est un contrôleur de domaine qui stocke des informations sur tous les objets dans la forêt, afin que les applications puissent rechercher des services AD DS sans faire référence à des contrôleurs de domaine spécifiques qui stockent les données demandées. Comme tous les contrôleurs de domaine, un serveur de catalogue global stocke des réplicas complètes et accessible en écriture, la configuration et schéma de partition d’annuaire et un réplica complet, accessible en écriture de la partition d’annuaire de domaine pour le domaine qu’elle héberge. En outre, un serveur de catalogue global stocke un réplica partiel en lecture seule de tous les autres domaines dans la forêt. Réplicas de domaine en lecture seule, partielle contiennent chaque objet dans le domaine, mais uniquement un sous-ensemble des attributs (attributs qui sont couramment utilisés pour la recherche de l’objet).  
  
## <a name="BKMK_10"></a>La mise en cache de l’appartenance au groupe universel  
La mise en cache de l’appartenance au groupe universel permet le contrôleur de domaine en cache les informations de l’appartenance au groupe universel pour les utilisateurs. Vous pouvez activer les contrôleurs de domaine qui exécutent Windows Server 2008 pour les appartenances au groupe universel de cache à l’aide de Sites Active Directory et le composant logiciel enfichable Services.  
  
L’activation de la mise en cache de l’appartenance au groupe universel élimine la nécessité d’un serveur de catalogue global sur chaque site dans un domaine, ce qui réduit l’utilisation de la bande passante réseau, car un contrôleur de domaine n’a pas besoin répliquer tous les objets situés dans la forêt. Elle réduit également les ouvertures de session, car les contrôleurs de domaine authentification ne sont pas toujours nécessaires accéder à un catalogue global pour obtenir des informations d’appartenance au groupe universel. Pour plus d’informations sur l’utilisation de la mise en cache de l’appartenance au groupe universel, consultez [planification de Placement de serveur catalogue Global](../../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md).  
  


