---
ms.assetid: 4cc9c16c-1928-4dce-a3a8-6229be28eb65
title: "Concepts de réplication ActiveDirectory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 45753c048f9bb1cb174daade1d408b88b3686b4b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-replication-concepts"></a>Concepts de réplication ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Avant de concevoir la topologie de site, familiarisez-vous avec certains concepts de réplication ActiveDirectory.  
  
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
Un objet de connexion est un objet ActiveDirectory qui représente une connexion de réplication à partir d’un contrôleur de domaine source à un contrôleur de domaine de destination. Un contrôleur de domaine est un membre d’un site unique et est représenté par un objet serveur dans les Services de domaine ActiveDirectory (ADDS) dans le site. Chaque objet serveur possède un enfant d’objet Paramètres NTDS représentant le contrôleur de domaine répliqués dans le site.  
  
L’objet de connexion est un enfant de l’objet Paramètres NTDS sur le serveur de destination. Pour la réplication entre deux contrôleurs de domaine, l’objet serveur de l’un doit avoir un objet de connexion représentant la réplication entrante à partir de l’autre. Toutes les connexions de réplication pour un contrôleur de domaine sont stockées sous la forme d’objets de connexion sous l’objet Paramètres NTDS. L’objet de connexion identifie le serveur source de réplication, contienne une planification de réplication et spécifie un transport de réplication.  
  
Le vérificateur de cohérence des connaissances (KCC) crée automatiquement des objets de connexion, mais elles peuvent également être créées manuellement. Les objets de connexion créés par le processus KCC apparaissent dans les Sites ActiveDirectory et le composant logiciel enfichable Services en tant que **<automatically generated>** et sont considérés comme adéquates dans des conditions normales. Les objets de connexion créés par un administrateur sont créés manuellement les objets de connexion. Un objet de connexion créés manuellement est identifié par le nom attribué par l’administrateur lors de sa création. Lorsque vous modifiez une **<automatically generated>** des objets de connexion, vous le convertir en un objet de connexion administrativement modifié et l’objet s’affiche sous la forme d’un GUID. KCC n’apporte pas de modification aux objets de connexion manuelle ou modifiés.  
  
## <a name="BKMK_2"></a>KCC  
Le processus KCC est un processus intégré qui s’exécute sur tous les contrôleurs de domaine et génère la topologie de réplication pour la forêt ActiveDirectory. Le processus KCC crée des topologies de réplication distincts selon si la réplication s’exécute sur un site (intrasite) ou entre les sites (intersite). Le processus KCC ajuste dynamiquement la topologie pour prendre en charge l’ajout de nouveaux contrôleurs de domaine, le mouvement de contrôleurs de domaine vers et depuis des sites, modifier les coûts et les calendriers et les contrôleurs de domaine qui ne sont temporairement pas disponibles ou la suppression des contrôleurs de domaine existants dans un état d’erreur.  
  
Au sein d’un site, les connexions entre les contrôleurs de domaine accessibles en écriture sont toujours organisées dans un anneau bidirectionnel, avec des connexions de raccourci supplémentaires pour réduire la latence pour les sites importants. En revanche, la topologie intersite est une superposition de fractionnement des arborescences, ce qui signifie qu’une connexion intersite existe entre les deux sites pour chaque partition d’annuaire et généralement ne contient-elle pas de connexions de raccourci. Pour plus d’informations sur le fractionnement des arborescences et la topologie de réplication ActiveDirectory, voir ActiveDirectory Replication topologie informations techniques de référence ([https://go.microsoft.com/fwlink/?LinkID=93578](https://go.microsoft.com/fwlink/?LinkID=93578)).  
  
Sur chaque contrôleur de domaine, le processus KCC crée des itinéraires de réplication en créant des objets de connexion entrante à sens unique qui définissent les connexions à partir d’autres contrôleurs de domaine. Pour les contrôleurs de domaine dans le même site, le KCC crée des objets de connexion automatiquement sans intervention de l’administrateur. Lorsque vous disposez de plusieurs sites, vous configurez des liens de sites entre les sites, et un seul KCC dans chaque site crée automatiquement des connexions entre les sites également.  
  
### <a name="kcc-improvements-for-windows-server-2008-rodcs"></a>Améliorations apportées à des données pour Windows Server2008 RODC  

Il existe une série d’améliorations des données pour prendre en charge le contrôleur disponible qui vient d’être domaine en lecture seule (RODC) dans Windows Server2008. Un scénario de déploiement classique de RODC est la filiale. La topologie de réplication ActiveDirectory la plus couramment déployée dans ce scénario est basée sur une conception de hub and spoke, où les contrôleurs de domaine de branche dans plusieurs sites répliquent avec un petit nombre de serveurs de tête de pont dans un site hub.  
  
Un des avantages du déploiement RODC dans ce scénario est la réplication unidirectionnelle. Serveurs tête de pont ne sont pas requis pour répliquer depuis le RODC, ce qui réduit d’administration et de l’utilisation du réseau.  
  
Toutefois, un des défis d’administration mis en surbrillance par la topologie hub spoke sur les versions précédentes du système d’exploitation Windows Server sont qu’après avoir ajouté un nouveau contrôleur de domaine de tête de pont dans le hub, il n’existe aucun mécanisme pour redistribuer les connexions de réplication entre les contrôleurs de domaine de branche et les contrôleurs de domaine afin de tirer parti du nouveau contrôleur de domaine hub automatique.  
  
Pour les contrôleurs de domaine Windows Server2003, vous pouvez rééquilibrer la charge de travail à l’aide d’un outil tel Adlb.exe du Guide de déploiement Windows Server2003 branche Office ([https://go.microsoft.com/fwlink/?LinkID=28523](https://go.microsoft.com/fwlink/?LinkID=28523)).  
  
Pour Windows Server2008 RODC, fonctionnement normal du KCC fournit certaines rééquilibrage, qui élimine le besoin d’utiliser un outil comme Adlb.exe supplémentaire. La nouvelle fonctionnalité est activée par défaut. Vous pouvez le désactiver en ajoutant la clé de Registre suivante définie sur le RODC:  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters**  
  
**«Loadbalancing BH aléatoire autorisé»**  
**1 = activé (valeur par défaut), 0 = désactivé**  
  
Pour plus d’informations sur le fonctionnement de ces améliorations des données, consultez Planification et déploiement des Services de domaine ActiveDirectory pour les filiales ([https://go.microsoft.com/fwlink/?LinkId=107114](https://go.microsoft.com/fwlink/?LinkId=107114)).  
  
## <a name="BKMK_3"></a>Fonctionnalité de basculement  
Sites vous assurer que la réplication est routée autour des défaillances réseau et contrôleurs de domaine hors connexion. Le processus KCC s’exécute à intervalles réguliers pour ajuster la topologie de réplication des modifications qui se produisent dans les services ADDS, par exemple, lorsque des nouveaux contrôleurs de domaine sont ajoutés et nouveaux sites sont créés. Le processus KCC examine l’état de réplication des connexions existantes pour déterminer si les connexions ne fonctionnent pas. Si une connexion ne fonctionne pas en raison d’un contrôleur de domaine a échoué, le processus KCC crée automatiquement des connexions temporaires à d’autres partenaires de réplication (si disponible) pour vous assurer que la réplication se produit. Si tous les contrôleurs de domaine dans un site ne sont pas disponibles, le processus KCC crée automatiquement des connexions de réplication entre les contrôleurs de domaine à partir d’un autre site.  
  
## <a name="BKMK_4"></a>Sous-réseau  
Un sous-réseau est un segment de réseau TCP/IP auquel un ensemble d’adresses IP logiques sont affectés. Sous-réseaux regroupement les ordinateurs d’une façon qui identifie leur proximité physique sur le réseau. Dans ADDS, les objets de sous-réseaux identifient les adresses de réseau qui sont utilisés pour mapper les ordinateurs à des sites.  
  
## <a name="BKMK_5"></a>Site  
Les sites sont des objets ActiveDirectory qui représentent un ou plusieurs sous-réseaux TCP/IP avec des connexions réseau hautement fiable et rapide. Informations de site permettent aux administrateurs de configurer l’accès à ActiveDirectory et la réplication d’optimiser l’utilisation du réseau physique. Les objets de sites sont associés à un ensemble de sous-réseaux, et chaque contrôleur de domaine dans une forêt est associé à un site ActiveDirectory en fonction de son adresse IP. Les sites peuvent héberger des contrôleurs de domaine à partir de plusieurs domaines et un domaine peut être représenté dans plusieurs sites.  
  
## <a name="BKMK_6"></a>Lien de sites  
Liens de sites sont des objets ActiveDirectory qui représentent des chemins d’accès logiques que le KCC utilise pour établir une connexion à la réplication ActiveDirectory. Un objet de lien de site représente un ensemble de sites qui peuvent communiquer à un coût uniforme via un transport intersite spécifié.  
  
Tous les sites contenus dans le lien de sites sont considérées comme être connecté au moyen du même type de réseau. Sites doivent être liés manuellement à d’autres sites à l’aide des liens de sites afin que les contrôleurs de domaine dans un site peuvent répliquer les modifications de l’annuaire à partir de contrôleurs de domaine dans un autre site. Étant donné que les liens de sites ne correspondent pas au chemin emprunté par les paquets réseau sur le réseau physique lors de la réplication, vous n’avez pas besoin créer des liens de sites redondants pour améliorer l’efficacité de la réplication ActiveDirectory.  
  
Lorsque deux sites sont connectés par un lien de sites, le système de réplication crée automatiquement des connexions entre les contrôleurs de domaine spécifique dans chaque site qui sont appelés serveurs de tête de pont. Dans Windows Server2008, tous les contrôleurs de domaine dans un site qui hébergent la même partition d’annuaire sont des candidats à être sélectionné en tant que serveurs de tête de pont. Les connexions de réplication créées par le KCC sont distribuées au hasard parmi tous les serveurs de tête de pont candidat dans un site pour partager la charge de travail de réplication. Par défaut, la sélection aléatoire processus intervient uniquement une fois que, lorsque les objets de connexion sont tout d’abord ajoutés au site.  
  
## <a name="BKMK_7"></a>Pont lien de sites  
Un pont entre liens de site est un objet ActiveDirectory qui représente un ensemble de liens de sites, tous les dont les sites peuvent communiquer à l’aide d’un transport courants. Ponts entre liens de site permettent de contrôleurs de domaine qui ne sont pas connectés directement au moyen d’un lien de communication à se répliquer entre eux. En règle générale, un pont entre liens de site correspond à un routeur (ou un ensemble de routeurs) sur un réseau IP.  
  
Par défaut, le processus KCC peut constituer une route transitive via les liens de tous les sites qui ont en commun de certains sites. Si ce comportement est désactivé, chaque lien de site représente son propre réseau distinct et isolé. Ensembles de liens de sites qui peuvent être traités comme un seul itinéraire sont exprimées par le biais d’un pont entre liens de site. Chaque pont représente un environnement isolé de communication pour le trafic réseau.  
  
Ponts entre liens de site sont un mécanisme pour représenter logiquement transitive connectivité physique entre les sites. Un pont entre liens de site permet le KCC à utiliser n’importe quelle combinaison de liens de sites inclus à déterminer l’itinéraire le moins coûteux pour interconnecter des partitions d’annuaire contenues dans ces sites. Le pont lien de site ne fournit pas de connectivité réelle sur les contrôleurs de domaine. Si le pont lien de site est supprimé, la réplication sur les liens de sites combinée continue jusqu'à ce que le processus KCC supprime les liens.  
  
Ponts entre liens de site sont nécessaires uniquement si un site contient un contrôleur de domaine qui héberge une partition d’annuaire qui n’est pas aussi hébergée sur un contrôleur de domaine dans un site adjacent, mais un contrôleur de domaine qui héberge la partition de répertoire se trouve dans un ou plusieurs sites dans la forêt. Les sites adjacents sont définis en tant que tous les deux ou plusieurs sites inclus dans un lien de site unique.  
  
Un pont entre liens de site crée une connexion logique entre deux liens de sites, en fournissant un chemin d’accès transitive entre deux sites déconnectés à l’aide d’un site intermédiaire. Dans le cadre du Générateur de topologie intersite (ISTG), le pont implique la connectivité physique à l’aide du site intermédiaire. Le pont n’implique pas que le contrôleur de domaine dans le site intermédiaire fournira le chemin de réplication. Toutefois, cela aurait être le cas si le site intermédiaire contenue un contrôleur de domaine hébergeant la partition d’annuaire à répliquer, auquel cas un pont entre liens de site n’est pas requis.  
  
Le coût de chaque lien de site est ajouté, création d’un coût somme pour le chemin d’accès résultant. Le pont lien de site doit être utilisé si le site intermédiaire ne contient pas un contrôleur de domaine qui héberge la partition d’annuaire et un lien de coût inférieur n’existe pas. Si le site intermédiaire contenue un contrôleur de domaine qui héberge la partition d’annuaire, deux sites déconnectés seraient d’établir des connexions de réplication pour le contrôleur de domaine temporaire et n’utilisez pas le pont.  
  
## <a name="BKMK_8"></a>Transitivité de lien de site  
Par défaut, tous les liens de sites sont transitives ou «relais». Lorsque des liens de sites sont affectés d’un pont et les planifications se chevauchent, le processus KCC crée des connexions de réplication qui déterminent les partenaires de réplication de contrôleur de domaine entre les sites, où les sites ne sont pas directement connectés par des liens de sites, mais sont connectés de manière transitive via un ensemble de sites courants. Cela signifie que n’importe quel site peut se connecter à un autre site via une combinaison de liens de sites.  
  
En règle générale, pour un réseau routé complètement, vous n’avez pas besoin créer des ponts entre liens de n’importe quel site, sauf si vous voulez contrôler le flux des modifications de réplication. Si votre réseau n’est pas entièrement routé, les ponts entre liens de site doivent être créés pour éviter les tentatives de réplication impossible. Tous les liens de sites pour un transport spécifique appartiennent implicitement à un pont entre liens de site unique pour ce transport. Le pontage par défaut pour les liens de sites se produit automatiquement, et aucun objet ActiveDirectory ne représente ce pont. Le **relier tous les liens de sites** paramètre, dans les propriétés des conteneurs transport intersite IP et de transférer protocole SMTP (Simple Mail), met en œuvre de pont lien de site automatique.  
  
> [!NOTE]  
> La réplication SMTP n’est pas pris en charge dans les futures versions des services ADDS; par conséquent, la création d’objets de liens de site dans le conteneur SMTP n’est pas recommandée.  
  
## <a name="BKMK_9"></a>Serveur de catalogue global  
Un serveur de catalogue global est un contrôleur de domaine qui stocke des informations sur tous les objets de la forêt, afin que les applications puissent rechercher des services ADDS sans faire référence aux contrôleurs de domaine spécifiques qui stockent les données demandées. Comme tous les contrôleurs de domaine, un serveur de catalogue global stocke complètes, accessible en écriture les réplicas des partitions d’annuaire de schéma et de configuration et un réplica complet, accessible en écriture de la partition d’annuaire de domaine pour le domaine qu’il héberge. En outre, un serveur de catalogue global stocke un réplica partiel en lecture seule de tous les autres domaines dans la forêt. Chaque objet dans le domaine, mais uniquement un sous-ensemble des attributs (ces attributs sont généralement utilisés pour la recherche de l’objet) contiennent des réplicas du domaine partielles en lecture seule.  
  
## <a name="BKMK_10"></a>La mise en cache de l’appartenance au groupe universel  
La mise en cache de l’appartenance au groupe universel permet le contrôleur de domaine en cache les informations de l’appartenance au groupe universel pour les utilisateurs. Vous pouvez activer des contrôleurs de domaine qui exécutent Windows Server2008 pour les appartenances au groupe universel de cache à l’aide de Sites ActiveDirectory et le composant logiciel enfichable Services.  
  
L’activation de la mise en cache de l’appartenance au groupe universel élimine la nécessité d’un serveur de catalogue global sur chaque site dans un domaine, ce qui réduit l’utilisation de la bande passante réseau, car un contrôleur de domaine n’a pas besoin de répliquer tous les objets situés dans la forêt. Cela réduit également les ouvertures de session dans la mesure où les contrôleurs de domaine authentification ne sont pas toujours nécessaires accéder à un catalogue global pour obtenir des informations d’appartenance au groupe universel. Pour plus d’informations sur l’utilisation de la mise en cache de l’appartenance au groupe universel, voir [planification emplacement serveur de catalogue Global](../../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md).  
  


