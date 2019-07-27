---
title: BranchCache
description: Cette rubrique fournit une vue d’ensemble de BranchCache dans Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-bc
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4587cff-c086-49f1-a0bf-cd74b8a44440
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ba334e4aee0232d939a52f1173885a5f457adbc8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883860"
---
# <a name="branchcache"></a>BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique, qui s'adresse aux professionnels de l'informatique, présente des informations générales sur BranchCache, notamment les modes BranchCache, les caractéristiques et les capacités, ainsi que les fonctionnalités BranchCache disponibles dans différents systèmes d'exploitation.

> [!NOTE]
> Outre cette rubrique, la documentation BranchCache suivante est disponible.
> 
> - [Environnement réseau de BranchCache et des commandes de Windows PowerShell](../branchcache/BranchCache-Network-Shell-and-Windows-PowerShell-Commands.md)
> -   [Guide de déploiement de BranchCache](../branchcache/deploy/BranchCache-Deployment-Guide.md)

**Qui sera intéressé par BranchCache ?**

Si vous êtes administrateur système, architecte de solutions réseau ou de stockage, ou tout autre professionnel de l’informatique, BranchCache peut vous intéresser dans les cas suivants :

- Vous assurez la conception ou la maintenance de l’infrastructure informatique d’une organisation disposant de deux emplacements physiques, ou plus, et d’une connexion de réseau étendu (WAN, Wide Area Network) des filiales vers le siège social.

- Vous assurez la conception ou la maintenance de l’infrastructure informatique d’une organisation qui a déployé des technologies de nuage, et une connexion WAN est utilisée par les travailleurs pour accéder aux données et aux applications se trouvant dans des emplacements distants.

- Vous voulez optimiser l’utilisation de la bande passante du réseau étendu (WAN) en réduisant la quantité de trafic réseau entre les filiales et le siège social.

- Vous avez déployé ou prévoyez de déployer, à votre siège social, des serveurs de contenu qui correspondent aux configurations décrites dans cette rubrique.

- Les ordinateurs clients dans vos filiales exécutent Windows 10, Windows 8.1, Windows 8 ou Windows 7.

Cette rubrique contient les sections suivantes :

-   [Nouveautés de BranchCache ?](#bkmk_what)

-   [Modes de BranchCache](#BKMK_2)
  
-   [Serveurs de contenu BranchCache](#BKMK_3)
  
-   [BranchCache et le cloud](#BKMK_3a)
  
-   [Versions des informations de contenu](#bkmk_version)  
  
-   [Comment BranchCache gère les mises à jour de contenu dans les fichiers](#bkmk_handles)  
  
-   [Guide d’installation de BranchCache](#BKMK_4)  
  
-   [Versions de système d’exploitation pour BranchCache](#bkmk_os)  
  
-   [Sécurité de BranchCache](#bkmk_security)  
  
-   [Flux de contenu et de processus](#bkmk_flow)  
  
-   [Sécurité du cache](#bkmk_cache)  
  
## <a name="bkmk_what"></a>Nouveautés de BranchCache ?

BranchCache est une technologie d’optimisation étendu réseaux (étendus WAN) de la bande passante qui est incluse dans certaines éditions des systèmes d’exploitation Windows 10 et Windows Server 2016, ainsi que dans certaines éditions de Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8 , Windows Server 2008 R2 et Windows 7. Pour optimiser la bande passante d’un réseau étendu lorsque des utilisateurs accèdent à du contenu sur des serveurs distants, BranchCache extrait le contenu des serveurs de contenu de votre siège social ou du cloud hébergé et le met en cache sur les systèmes des filiales, permettant ainsi aux ordinateurs clients des filiales d’accéder localement au contenu au lieu de passer par le réseau étendu.
  
Dans les succursales, le contenu est stocké sur les serveurs configurés pour héberger le cache ou, si aucun serveur n’est disponible dans la succursale, sur les ordinateurs clients qui exécutent Windows 10, Windows 8.1, Windows 8 ou Windows 7. Lorsqu’un ordinateur client demande et reçoit du contenu à partir du siège social et que le contenu est mis en cache dans la filiale, les autres ordinateurs de la même filiale peuvent obtenir le contenu localement au lieu de télécharger le contenu à partir du serveur de contenu via une liaison de réseau étendu (WAN).

Lorsque des demandes ultérieures pour le même contenu sont effectuées par des ordinateurs clients, ceux-ci téléchargent  les *informations de contenu* à partir du serveur au lieu du contenu proprement dit. Les informations de contenu se composent de hachages qui sont calculés à l’aide de segments du contenu d’origine et qui sont extrêmement petits par rapport au contenu des données d’origine. Les ordinateurs clients utilisent alors les informations de contenu pour localiser le contenu dans un cache de la filiale, que le cache se trouve sur un ordinateur client ou sur un serveur. Les ordinateurs clients et les serveurs utilisent également les informations de contenu pour sécuriser le contenu mis en cache afin qu’il ne soit pas accessible par des utilisateurs non autorisés.

BranchCache augmente la productivité de l’utilisateur final en améliorant les délais de réponse aux requêtes des clients et serveurs des filiales ; il peut également participer à l’amélioration des performances du réseau en réduisant le trafic sur les liaisons de réseau étendu (WAN).

## <a name="BKMK_2"></a>Modes de BranchCache
BranchCache a deux modes de fonctionnement : le mode de cache distribué et le mode de cache hébergé.

Lorsque vous déployez BranchCache en mode de cache distribué, le cache de contenu dans la filiale est distribué entre les ordinateurs clients.

Lorsque vous déployez BranchCache en mode de cache hébergé, le cache de contenu dans la filiale est hébergé sur un ou plusieurs ordinateurs serveurs, lesquels sont appelés « serveurs de cache hébergé ».

> [!NOTE]
> Vous pouvez déployer BranchCache à l’aide des deux modes. Cependant, un seul mode peut être utilisé par filiale. Par exemple, si vous avez deux filiales, une disposant d’un serveur et l’autre pas, vous pouvez déployer BranchCache en mode de cache hébergé dans le bureau ayant le serveur, tout en déployant BranchCache en mode de cache distribué dans le bureau disposant uniquement d’ordinateurs clients.

Dans l’illustration suivante, BranchCache est déployé dans les deux modes.  

![Modes de BranchCache](../media/BranchCache/bc_modes.jpg)

Le mode de cache distribué convient mieux aux petites filiales ne disposant pas d’un serveur local à utiliser en tant que serveur de cache hébergé. Le mode de cache distribué vous permet de déployer BranchCache sans matériel supplémentaire dans les filiales.

Si la filiale où vous voulez déployer BranchCache comprend une infrastructure supplémentaire, telle qu’un ou plusieurs serveurs exécutant d’autres charges de travail, le déploiement de BranchCache en mode de cache hébergé est avantageux pour les raisons suivantes :

### <a name="increased-cache-availability"></a>Disponibilité améliorée du cache

Le mode de cache hébergé augmente l’efficacité du cache, car le contenu est disponible même si le client qui avait demandé et mis en cache les données à l’origine n’est pas connecté. Étant donné que le serveur de cache hébergé est toujours disponible, davantage de contenu est mis en cache, ce qui économise davantage la bande passante du réseau étendu et améliore l’efficacité de BranchCache.

### <a name="centralized-caching-for-multiple-subnet-branch-offices"></a>Mise en cache centralisée pour les filiales à plusieurs sous-réseaux


Le mode de cache distribué ne fonctionne que sur un seul sous-réseau. Dans une filiale comportant plusieurs sous-réseaux et qui est configurée pour le mode de cache distribué, un fichier téléchargé sur un sous-réseau ne peut pas être partagé avec des ordinateurs clients se trouvant sur d’autres sous-réseaux. 

Par conséquent, les clients sur d’autres sous-réseaux, qui sont dans l’impossibilité de détecter que le fichier a déjà été téléchargé, obtiennent le fichier auprès du serveur de contenu du siège social, en utilisant la bande passante du réseau étendu dans le processus.

Ce n’est cependant pas le cas lorsque vous effectuez le déploiement en mode de cache hébergé : tous les clients d’une filiale comportant plusieurs sous-réseaux peuvent accéder à un seul et même cache, qui est stocké sur le serveur de cache hébergé, même si les clients se trouvent sur des sous-réseaux différents. De plus, BranchCache dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 offre la possibilité de déployer plusieurs serveurs de cache hébergé par filiale.

> [!CAUTION]
> Si vous utilisez BranchCache pour la mise en cache SMB des fichiers et dossiers, ne désactivez pas la fonctionnalité Fichiers hors connexion. Si vous la désactivez, la mise en cache SMB de BranchCache ne fonctionne pas correctement.

## <a name="BKMK_3"></a>Serveurs de contenu BranchCache

Lorsque vous déployez BranchCache, le contenu de la source est stocké sur des serveurs contenus prenant en charge BranchCache dans votre siège social ou dans un centre de données cloud. Les types de serveurs de contenu suivants sont pris en charge par BranchCache :

> [!NOTE]
> Seul le contenu source - autrement dit, contenu que les ordinateurs clients obtiennent initialement à partir d’un serveur de contenu compatible BranchCache - est accéléré par BranchCache. Le contenu que les ordinateurs clients obtiennent directement auprès d’autres sources, telles que des serveurs Web sur Internet ou Windows Update, n’est pas mis en cache par les ordinateurs clients ou les serveurs de cache hébergé puis partagé avec d’autres ordinateurs de la filiale. Toutefois, si vous souhaitez accélérer le contenu de la mise à jour de Windows, vous pouvez installer un serveur d’applications Windows Server Update Services (WSUS) dans votre siège social ou du centre de données cloud et configurez-le comme un serveur de contenu BranchCache.

### <a name="web-servers"></a>Serveurs Web

Les serveurs Web pris en charge incluent les ordinateurs qui exécutent Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 qui ont le rôle serveur serveur Web (IIS) installé et qui utilisent le protocole HTTP (Hypertext Transfer) ou Secure HTTP ( (HTTPS).

De plus, la fonctionnalité BranchCache doit être installée sur le serveur Web.

### <a name="file-servers"></a>Serveurs de fichiers

Serveurs de fichiers pris en charge incluent les ordinateurs qui exécutent Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 et ayant le rôle de serveur Services de fichiers et le BranchCache pour fichiers réseau service de rôle installé. 

Ces serveurs de fichiers utilisent le protocole SMB (Server Message Block) pour échanger des informations entre les ordinateurs. Une fois l’installation de votre serveur de fichiers terminée, vous devez également partager des dossiers et permettre la génération de hachage pour les dossiers partagés en utilisant une stratégie de groupe ou une stratégie de l’ordinateur local pour activer BranchCache.

### <a name="application-servers"></a>Serveurs d’applications

Serveurs d’applications pris en charge incluent les ordinateurs qui exécutent Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 avec Background Intelligent Transfer Service (BITS) installé et activé. 

De plus, la fonctionnalité BranchCache doit être installée sur le serveur d’applications. Par exemple des serveurs d’applications, vous pouvez déployer des serveurs Microsoft Windows Server Update Services (WSUS) et le Point de Distribution branche Microsoft System Center Configuration Manager comme serveurs de contenu BranchCache.

## <a name="BKMK_3a"></a>BranchCache et le cloud

Avec le nuage, le potentiel de réduire les frais de fonctionnement et de prendre une nouvelle ampleur est énorme. Toutefois, séparer les charges de travail des personnes qui en dépendent peut augmenter les coûts de mise en réseau et affecter la productivité. Les utilisateurs souhaitent des performances élevées et peu importe où leurs applications et les données sont hébergées. 

BranchCache peut améliorer les performances d’applications en réseau et réduire la consommation de bande passante à l’aide d’un cache de données partagé.  Cette fonctionnalité améliore la productivité dans les filiales et dans les sièges sociaux, où les travailleurs utilisent des serveurs qui sont déployés dans le nuage.

Étant donné que BranchCache ne requiert pas de nouveau matériel ou de modifications de topologie de réseau, c’est une excellente solution pour améliorer la communication entre des bureaux et des nuages aussi bien publics que privés.

> [!NOTE]
> Étant donné que certains proxys Web ne peut pas traiter les en-têtes Content-Encoding non standards, il est recommandé que vous utilisez BranchCache avec Hyper texte Transfer protocole sécurisé (HTTPS) et non HTTP.
  
=== Pour plus d’informations sur les technologies de cloud dans Windows Server 2016, consultez [Sdn &#40;SDN&#41;](../sdn/Software-Defined-Networking--SDN-.md).
  
## <a name="bkmk_version"></a>Versions des informations de contenu

Il existe deux versions des informations de contenu :

- Les informations de contenu qui sont compatibles avec les ordinateurs exécutant Windows Server 2008 R2 et Windows 7 sont appelées version 1, ou V1. Avec la segmentation de fichiers BranchCache V1, les segments de fichiers sont plus grands que dans V2 et ont une taille fixe. En raison des importantes tailles de segments fixes, lorsqu’un utilisateur apporte une modification qui change la longueur du fichier, non seulement le segment avec la modification est invalidé, mais tous les segments jusqu’à la fin du fichier le sont aussi. L’appel suivant pour le fichier modifié par un autre utilisateur de la filiale aboutit donc à des économies de bande passante du réseau étendu réduites, car le contenu modifié et l’intégralité du contenu après la modification sont envoyés par la liaison de réseau étendu.

- Les informations de contenu qui sont compatibles avec les ordinateurs exécutant Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012 et Windows 8 sont appelées version 2, ou V2. Les informations de contenu V2 utilisent des segments plus petits de taille variable qui sont plus tolérants aux modifications dans un fichier. Cela augmente la probabilité que des segments d’une version plus ancienne du fichier puissent être réutilisés lorsque les utilisateurs accèdent à une version mise à jour ; ils récupèrent ainsi uniquement la partie modifiée du fichier à partir du serveur de contenu et utilisent moins de bande passante du réseau étendu.

Le tableau suivant fournit des informations sur la version des informations de contenu qui est utilisée selon les systèmes d’exploitation du client, du serveur de contenu et du serveur de cache hébergé que vous utilisez dans votre déploiement BranchCache.

> [!NOTE]
> Dans le tableau ci-dessous, l’acronyme « Se » signifie système d’exploitation.

|SE du client|SE du serveur de contenu|SE du serveur de cache hébergé|Version des informations de contenu|
|-------------|---------------------|--------------------------|-------------------------------|
|Windows Server 2008 R2 et Windows 7 |Windows Server 2012 ou version ultérieure|Windows Server 2012 ou version ultérieure ; Aucun pour le mode cache distribué|V1|
|Windows Server 2012 ou version ultérieure ; Windows 8 ou version ultérieure|Windows Server 2008 R2 |Windows Server 2012 ou version ultérieure ; Aucun pour le mode cache distribué|V1|
|Windows Server 2012 ou version ultérieure ; Windows 8 ou version ultérieure| Windows Server 2012 ou version ultérieure| Windows Server 2008 R2 |V1|
|Windows Server 2012 ou version ultérieure ; Windows 8 ou version ultérieure|Windows Server 2012 ou version ultérieure|Windows Server 2012 ou version ultérieure ; Aucun pour le mode cache distribué|V2|

Lorsque vous avez des serveurs de contenu et les serveurs de cache hébergé qui exécutent Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012, ils utilisent la version des informations de contenu qui est appropriée en fonction du système d’exploitation du client BranchCache qui demande des informations. 

Quand les ordinateurs exécutant Windows Server 2012 et Windows 8 ou des systèmes d’exploitation ultérieurs demandent du contenu, les serveurs de cache de contenu et hébergé utilisent les informations de contenu V2 ; Quand les ordinateurs exécutant Windows Server 2008 R2 et Windows 7 demandent du contenu, les serveurs de cache de contenu et hébergé utilisent les informations de contenu V1.

>[!IMPORTANT]
>Lorsque vous déployez BranchCache en mode de cache distribué, les clients qui utilisent différentes versions des informations de contenu ne partagent pas le contenu entre eux. Par exemple, un ordinateur client exécutant Windows 7 et un ordinateur client exécutant Windows 10 qui sont installés dans la même filiale ne partagent pas contenu entre eux.
  
## <a name="bkmk_handles"></a>Comment BranchCache gère les mises à jour de contenu dans les fichiers

Lorsque les utilisateurs des filiales modifient ou mettre à jour le contenu des documents, leurs modifications sont écrites directement sur le serveur de contenu du siège social sans implication de BranchCache. Cela est vrai que l'utilisateur ait téléchargé le document à partir du serveur de contenu ou qu'il l'ait obtenu à partir d'un cache hébergé ou distribué dans la filiale.

Quand le fichier modifié est demandé par un autre client dans une filiale, les nouveaux segments du fichier sont téléchargés depuis le serveur du siège social et ajoutés au cache distribué ou hébergé dans cette filiale. Pour cette raison, les utilisateurs des filiales reçoivent toujours les dernières versions du contenu mis en cache.

## <a name="BKMK_4"></a>Guide d’installation de BranchCache

Vous pouvez utiliser le Gestionnaire de serveur dans Windows Server 2016 pour installer la fonctionnalité BranchCache ou le BranchCache pour le service de rôle fichiers réseau du rôle serveur Services de fichiers. Le tableau suivant vous permet de déterminer si vous devez installer le service de rôle ou la fonctionnalité.

|Fonctionnalité|Emplacement de l’ordinateur|Installer cet élément BranchCache|
|-----------------|---------------------|------------------------------------|
|Serveur de contenu \(serveur d’applications basé sur les BITS\)|Siège social ou centre de données en nuage|Fonctionnalité BranchCache|
|Serveur de contenu \(serveur Web\)|Siège social ou centre de données en nuage|Fonctionnalité BranchCache|
|Serveur de contenu \(serveur de fichiers utilisant le protocole SMB\)|Siège social ou centre de données en nuage|Service de rôle BranchCache pour fichiers réseau du rôle serveur Services de fichiers|
|Serveur de cache hébergé|Filiale|Fonctionnalité BranchCache avec le mode de serveur de cache hébergé activé|
|Ordinateur client prenant en charge BranchCache|Filiale|Aucune installation nécessaire ; Activer simplement BranchCache et un mode BranchCache \(distribué ou hébergé\) sur le client|

Pour installer le service de rôle ou la fonctionnalité, ouvrez le Gestionnaire de serveur et sélectionnez les ordinateurs sur lesquels vous voulez activer la fonctionnalité BranchCache. Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant **Ajout de rôles et de fonctionnalités** s’ouvre. Lorsque vous exécutez l’Assistant, effectuez les sélections suivantes :

- Dans la page **Sélectionner le type d’installation** de l’Assistant, sélectionnez **Installation basée sur un rôle ou une fonctionnalité**.

- Dans la page Assistant **sélectionner des rôles de serveur**, si vous installez un serveur de fichiers prenant en charge BranchCache, développez **File and Storage Services** et **Services de fichiers et iSCSI**, et puis sélectionnez **BranchCache pour fichiers réseau**.  Pour économiser l’espace disque, vous pouvez également sélectionner le **la déduplication des données** rôle de service, puis suivez les instructions de l’Assistant installation et de saisie semi-automatique. Si vous ne souhaitez pas installer un serveur de fichiers prenant en charge BranchCache, n’installez pas le rôle Services de fichiers et stockage avec le BranchCache pour fichiers réseau.

- Dans la page Assistant **sélectionner des fonctionnalités**, si vous installez un serveur de contenu qui n’est pas un serveur de fichiers ou si vous installez un serveur de cache hébergé, sélectionnez **BranchCache**et puis suivez les instructions de l’Assistant pour l’installation et la saisie semi-automatique. Si vous ne voulez pas installer un serveur de contenu autre qu’un serveur de fichiers ou un serveur de cache hébergé, n’installez pas la fonctionnalité BranchCache.
  
## <a name="bkmk_os"></a>Versions de système d’exploitation pour BranchCache

Voici la liste des systèmes d’exploitation qui prennent en charge les différents types de fonctionnalités BranchCache.

### <a name="operating-systems-for-branchcache-client-computer-functionality"></a>Systèmes d’exploitation pour les fonctionnalités des ordinateurs clients BranchCache

Les systèmes d’exploitation suivants fournissent BranchCache avec prise en charge pour le Service de transfert Intelligent en arrière-plan (BITS), Hyper texte HTTP (Transfer Protocol) et bloc de Message serveur (SMB).

- Windows 10 Entreprise

- Windows 10 Éducation

- Windows 8.1 Entreprise

- Windows 8 Entreprise

- Windows 7 Entreprise

- Windows 7 Édition Intégrale

Dans les systèmes d’exploitation suivants, BranchCache ne prend pas en charge les fonctionnalités HTTP et SMB, mais prend en charge les fonctionnalités de BITS de BranchCache.

-   Windows 10 Professionnel, BITS prennent uniquement en charge

-   Windows 8.1 Pro, BITS prennent uniquement en charge

-   Windows 8 Professionnel, BITS prennent uniquement en charge

-   Windows 7 Professionnel, BITS prennent uniquement en charge

> [!NOTE]
> BranchCache n’est pas disponible par défaut dans les systèmes d’exploitation Windows Server 2008 ou Windows Vista. Sur ces systèmes d’exploitation, toutefois, si vous téléchargez et installez la mise à jour Windows Management Framework, les fonctionnalités BranchCache est disponible pour le protocole de Service de transfert Intelligent en arrière-plan (BITS) uniquement. Pour plus d’informations et pour télécharger Windows Management Framework, consultez [Windows Management Framework (Windows PowerShell 2.0, WinRM 2.0 et BITS 4.0)](https://go.microsoft.com/fwlink/?LinkId=188677) à https://go.microsoft.com/fwlink/?LinkId=188677.
  
### <a name="operating-systems-for-branchcache-content-server-functionality"></a>Systèmes d’exploitation pour les fonctionnalités du serveur de contenu BranchCache

Vous pouvez utiliser les familles de systèmes d’exploitation de Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 en tant que serveurs de contenu BranchCache.

En outre, la famille Windows Server 2008 R2 de systèmes d’exploitation peut être utilisée en tant que serveurs de contenu BranchCache, avec les exceptions suivantes :

- BranchCache n’est pas pris en charge dans les installations Server Core de Windows Server 2008 R2 Enterprise avec Hyper-V.

- BranchCache n’est pas pris en charge dans les installations Server Core de Windows Server 2008 R2 Datacenter avec Hyper-V.

### <a name="operating-systems-for-branchcache-hosted-cache-server-functionality"></a>Systèmes d’exploitation pour les fonctionnalités de serveur de cache hébergé de BranchCache

Vous pouvez utiliser les familles de systèmes d’exploitation de Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 comme serveurs de cache hébergé de BranchCache.

En outre, les systèmes d’exploitation Windows Server 2008 R2 suivants peuvent être utilisés comme serveurs de cache hébergé de BranchCache :

- Windows Server 2008 R2 Entreprise

- Windows Server 2008 R2 Enterprise avec Hyper-V

- Installation minimale de Windows Server 2008 R2 Enterprise Server

- Windows Server 2008 R2 Enterprise Server Installation minimale avec Hyper-V

- Windows Server 2008 R2 pour les systèmes Itanium

- Windows Server 2008 R2 Datacenter

- Centre de données de Windows Server 2008 R2 avec Hyper-V

- Windows Server 2008 R2 Datacenter Server Installation minimale avec Hyper-V

## <a name="bkmk_security"></a>Sécurité de BranchCache

BranchCache implémente une approche de sécurisation de par sa conception, qui fonctionne de façon transparente avec vos architectures de sécurité réseau existantes, sans qu’un équipement supplémentaire ou une configuration supplémentaire complexe de la sécurité ne soit nécessaire.
  
BranchCache est non invasif et ne modifie aucun processus d’authentification ou d’autorisation Windows. Une fois que vous avez déployé BranchCache, l’authentification continue à être effectuée à l’aide d’informations d’identification de domaine, et le fonctionnement de l’autorisation avec des listes de contrôles d’accès reste inchangé. De plus, les autres configurations continuent à fonctionner exactement de la même façon qu’avant le déploiement de BranchCache.

Le modèle de sécurité BranchCache est basé sur la création de métadonnées, qui prennent la forme d’une série de hachages. Ces hachages sont également appelés « informations de contenu ».

Une fois les informations de contenu créées, elles sont utilisées dans des échanges de messages BranchCache à la place des données réelles, et elles sont échangées à l’aide des protocoles pris en charge (HTTP, HTTPS et SMB).

Les données mises en cache sont maintenues chiffrées et ne sont pas accessibles par des clients qui ne disposent pas d’une autorisation pour accéder au contenu de la source d’origine.  Les clients doivent être authentifiés et autorisés par la source de contenu d’origine pour pouvoir récupérer les métadonnées du contenu. De plus, ils doivent posséder les métadonnées du contenu pour accéder au cache dans le bureau local.

### <a name="how-branchcache-generates-content-information"></a>Comment BranchCache génère-t-il les informations de contenu ?

Étant donné que les informations de contenu sont créées à partir de plusieurs éléments, la valeur des informations de contenu est toujours unique. Ces éléments sont les suivants :

- Le contenu réel (tel que des pages Web ou des fichiers partagés) duquel les hachages sont dérivés.  

- Des paramètres de configuration, tels que l’algorithme de hachage et la taille de bloc. Pour générer les informations de contenu, le serveur de contenu divise le contenu en segments, puis subdivise ces segments en blocs. BranchCache utilise des hachages de chiffrement sécurisés pour identifier et vérifier chaque bloc et chaque segment, en prenant en charge l’algorithme de hachage SHA256.

- Un secret de serveur. Tous les serveurs de contenu doivent être configurés avec un secret de serveur, qui est une valeur binaire de longueur arbitraire.

> [!NOTE]
> L’utilisation d’un secret de serveur garantit que les ordinateurs clients ne peuvent pas générer eux-mêmes les informations de contenu. Cela empêche les utilisateurs malveillants d’utiliser des attaques en force brute avec les ordinateurs clients prenant en charge BranchCache pour deviner des modifications mineures entre les versions dans les cas où le client avait accès à la version précédente mais n’a pas accès à la version actuelle.

### <a name="content-information-details"></a>Détails sur les informations de contenu

BranchCache utilise le secret de serveur comme clé afin de dériver un hachage spécifique au contenu qui est envoyé aux clients autorisés. L’application d’un algorithme de hachage au secret de serveur et au hachage de données combinés génère ce hachage.

Ce hachage est appelé « secret de segment ». BranchCache utilise des secrets de segment pour sécuriser les communications. De plus, BranchCache crée une liste des hachages de blocs, qui est la liste des blocs de données hachés, et le hachage de données, qui est généré en hachant la liste des hachages de blocs.

Les informations de contenu incluent les éléments suivants :

- La liste des hachages de blocs :

    `BlockHashi = Hash(dataBlocki)   1<=i<=n`

- Le hachage de données (HoD) :

    `HoD = Hash(BlockHashList)`

- Le secret de segment (Kp) :

    `Kp = HMAC(Ks, HoD)`

BranchCache utilise le protocole de mise en cache de contenu des homologues (Peer Content Caching) et le protocole de récupération (Retrieval) Framework pour implémenter les processus requis pour garantir la mise en cache et la récupération sécurisées de données entre des caches de contenu.

De plus, BranchCache gère les informations de contenu avec le même niveau de sécurité que lors de la gestion et de la transmission du contenu réel lui-même.

## <a name="bkmk_flow"></a>Flux de contenu et de processus

Le flux d’informations de contenu et de contenu réel est divisé en quatre phases :

1.  [Processus de BranchCache : Contenu de la demande](#BKMK_8)

2.  [Processus de BranchCache : Localiser du contenu](#BKMK_9)

3.  [Processus de BranchCache : Récupérer le contenu](#BKMK_10)

4.  [Processus de BranchCache : Contenu du cache](#BKMK_11)

Les sections suivantes décrivent ces différentes phases.

## <a name="BKMK_8"></a>Processus de BranchCache : Demander du contenu

Au cours de la première phase, l’ordinateur client de la filiale demande du contenu, tel qu’un fichier ou une page Web, d’un serveur de contenu situé dans un emplacement distant, tel qu’un siège social. Le serveur de contenu vérifie que l’ordinateur client est autorisé à recevoir le contenu demandé. Si l’ordinateur client est autorisé et les clients et serveur de contenu BranchCache\-activé, le serveur de contenu génère des informations de contenu.

Le serveur de contenu envoie alors les informations de contenu à l’ordinateur client en utilisant le même protocole que celui qui aurait été utilisé pour le contenu réel. 

Par exemple, si l’ordinateur client a demandé une page Web via HTTP, le serveur de contenu envoie les informations de contenu en utilisant HTTP. C’est pourquoi les garanties de sécurité du contenu au niveau du réseau et les informations de contenu sont identiques.

Une fois la partie initiale des informations de contenu (hachage de données + secret de segment) reçue, l’ordinateur client effectue les actions suivantes :

- Il utilise le secret de segment (Kp) comme clé de chiffrement (Ke).

- Il génère l’ID de segment (HoHoDk) à partir des valeurs HoD et Kp :

    `HoHoDk = HMAC(Kp, HoD + C), where C is the ASCII string "MS_P2P_CACHING" with NUL terminator.`

À ce stade, la principale menace est le risque encouru par le secret de serveur. Toutefois, BranchCache chiffre les blocs de données de contenu pour protéger le secret de segment. Pour cela, cette fonctionnalité utilise la clé de chiffrement qui est dérivée du secret de segment du segment de contenu dans lequel se trouvent les blocs de contenu.

Cette approche garantit qu’une entité qui n’est pas en possession du secret de serveur ne peut pas découvrir le contenu réel d’un bloc de données. Le secret de segment est traité avec le même niveau de sécurité que le segment en texte brut lui-même, car le fait de connaître le secret de segment d’un segment donné permet à une entité d’obtenir le segment auprès d’homologues, puis de le déchiffrer. La connaissance du secret de serveur ne produit pas immédiatement un texte brut particulier, mais permet de dériver certains types de données du texte chiffré, puis d’exposer éventuellement certaines données partiellement connues à une attaque de déchiffrement en force brute. Par conséquent, le secret de serveur doit être maintenu confidentiel.
  
## <a name="BKMK_9"></a>Processus de BranchCache : Localiser du contenu

Une fois les informations de contenu reçues par l’ordinateur client, le client utilise l’ID de segment pour localiser le contenu demandé dans le cache de la filiale locale, que le cache soit distribué entre les ordinateurs clients ou situé sur un serveur de cache hébergé.

Si l’ordinateur client est configuré pour le mode de cache hébergé, il est configuré avec le nom d’ordinateur du serveur de cache hébergé et contacte ce serveur pour récupérer le contenu.

Toutefois, si l’ordinateur client est configuré pour le mode de cache distribué, il est possible que le contenu soit stocké dans plusieurs caches de plusieurs ordinateurs de la filiale. Avant que le contenu soit récupéré, l’ordinateur client doit découvrir où il se trouve.

Lorsque les ordinateurs clients sont configurés pour le mode de cache distribué, ils localisent le contenu en utilisant un protocole de découverte basé sur le protocole WS-Discovery (Web Services Dynamic Discovery). Les clients envoient des messages d’exploration (Probe) de multidiffusion WS-Discovery pour découvrir le contenu mis en cache sur le réseau. Les messages d’exploration incluent l’ID de segment, ce qui permet aux clients de vérifier si le contenu demandé correspond au contenu stocké dans leur cache. Les clients qui reçoivent le message d’exploration initial répondent au client effectuant la demande avec des messages de type Probe-Match de monodiffusion si l’ID de segment correspond au contenu mis en cache localement.  

La réussite du processus WS-Discovery dépend du fait que le client qui effectue la découverte dispose des informations de contenu correctes, lesquelles ont été fournies par le serveur de contenu, pour le contenu qu’il demande.

La principale menace pour les données pendant la phase de demande de contenu est la divulgation d’informations, car l’accès aux informations de contenu implique un accès autorisé au contenu. Pour réduire ce risque, le processus de découverte ne divulgue pas les informations de contenu, hormis l’ID de segment, ce qui ne révèle rien sur le segment en texte brut dans lequel se trouve le contenu

En outre, un autre ordinateur client exécuté par un utilisateur malveillant sur le même sous-réseau de réseau peut voir le trafic de découverte BranchCache vers la source de contenu d’origine passant par le routeur.

Si le contenu demandé est introuvable dans la filiale, le client demande le contenu directement à partir du serveur de contenu via la liaison de réseau étendu (WAN).

Une fois le contenu reçu, il est ajouté au cache local, soit sur l’ordinateur client, soit sur un serveur de cache hébergé. Dans ce cas, les informations de contenu empêchent un client ou un serveur de cache hébergé d’ajouter au cache local du contenu qui ne correspond pas aux hachages. Le processus de vérification du contenu en faisant correspondre les hachages garantit que seul le contenu valide est ajouté au cache, protégeant ainsi l’intégrité du cache local.

## <a name="BKMK_10"></a>Processus de BranchCache : Récupérer du contenu

Une fois qu’un ordinateur client a localisé le contenu souhaité sur l’hôte de contenu, lequel est soit un serveur de cache hébergé soit un ordinateur client en mode de cache distribué, il commence le processus de récupération du contenu.

L’ordinateur client commence par envoyer une demande à l’hôte de contenu pour le premier bloc dont il a besoin. La demande contient l’ID de segment et la plage de blocs qui identifient le contenu souhaité. Étant donné qu’un seul bloc est retourné, la plage de blocs contient un seul bloc. (Les demandes de plusieurs blocs ne sont actuellement pas prises en charge.) Le client stocke également la demande dans sa liste des demandes non traitées locale.  

Lorsqu’il reçoit un message de demande valide à partir d’un client, l’hôte de contenu vérifie si le bloc spécifié dans la demande existe dans le cache de contenu de l’hôte de contenu.

Si l’hôte de contenu est en possession du bloc de contenu, il envoie une réponse contenant l’ID de segment, l’ID de bloc, le bloc de données chiffré et le vecteur d’initialisation utilisé pour le chiffrement du bloc.

Si l’hôte de contenu n’est pas en possession du bloc de contenu, il envoie un message de réponse vide. Cela informe l’ordinateur client que l’hôte de contenu ne dispose pas du bloc demandé. Un message de réponse vide contient l’ID de segment et l’ID de bloc du bloc demandé, ainsi qu’un bloc de données de taille zéro.

Lorsque l’ordinateur client reçoit la réponse de l’hôte de contenu, le client vérifie que le message correspond à un message de demande figurant dans sa liste des demandes non traitées. (L’ID de segment et l’index de bloc doivent correspondre à ceux d’une demande non traitée.)

Si ce processus de vérification échoue et que l’ordinateur client n’a pas de message de demande correspondant dans sa liste de demandes non traitées, l’ordinateur client ignore le message.

Si ce processus de vérification réussit et que l’ordinateur client a un message de demande correspondant dans sa liste de demandes non traitées, l’ordinateur client déchiffre le bloc. Le client valide alors le bloc déchiffré par rapport au hachage de bloc approprié présent dans les informations de contenu initialement obtenues par le client auprès du serveur de contenu d’origine.

Si la validation du bloc réussit, le bloc déchiffré est stocké dans le cache.

Ce processus est répété jusqu’à ce que le client ait tous les blocs requis.

> [!NOTE]
> Si les segments de contenu complets n’existent pas sur un ordinateur, le protocole de récupération récupère et assemble le contenu à partir d’une combinaison de sources : un ensemble d’ordinateurs clients en mode de cache distribué, un serveur de cache hébergé et, si les caches des filiales ne contiennent pas le contenu complet, le serveur de contenu d’origine installé au siège social.

Avant que BranchCache envoie les informations de contenu ou le contenu, les données sont chiffrées. BranchCache chiffre le bloc dans le message de réponse. Dans Windows 7, l’algorithme de chiffrement par défaut qui utilise BranchCache est AES-128, la clé de chiffrement est Ke et la taille de clé est 128 bits, comme stipulé par l’algorithme de chiffrement. 

BranchCache génère un vecteur d’initialisation qui convient à l’algorithme de chiffrement et utilise la clé de chiffrement pour chiffrer le bloc. BranchCache enregistre alors l’algorithme de chiffrement et le vecteur d’initialisation dans le message. 

Les serveurs et les clients ne s’échangent, ne partagent et ne s’envoient jamais la clé de chiffrement. Le client reçoit la clé de chiffrement du serveur de contenu qui héberge le contenu source. À l’aide de l’algorithme de chiffrement et du vecteur d’initialisation qu’il a reçu du serveur, il déchiffre alors le bloc. Aucune autre authentification ou autorisation explicite n’est intégrée au protocole de téléchargement.

### <a name="security-threats"></a>Menaces pour la sécurité

À ce stade, les principales menaces de sécurité sont les suivantes :

- Falsification des données :

  *Un client fournissant des données à un demandeur falsifie les données*. Le modèle de sécurité BranchCache utilise des hachages pour vérifier que ni le client ni le serveur n’a altéré les données.  

- Divulgation d’informations :  

    *BranchCache envoie du contenu chiffré à tout client qui spécifie l’ID de segment approprié*. Étant donné que les ID de segment sont publics, tous les clients peuvent recevoir du contenu chiffré. Toutefois, si un utilisateur malveillant obtient du contenu chiffré, il doit connaître la clé de chiffrement pour le déchiffrer. Le protocole de couche supérieure effectue l’authentification, puis donne les informations de contenu au client authentifié et autorisé. La sécurité des informations de contenu est équivalente à la sécurité fournie au contenu lui-même, et BranchCache n’expose jamais les informations de contenu.  

    *Un agresseur « écoute » le réseau afin d’obtenir du contenu*.  BranchCache chiffre tous les transferts entre les clients à l’aide du chiffrement AES128, où la clé secrète est Ke, ce qui empêche la détection de données sur le réseau.  Les informations de contenu qui sont téléchargées à partir du serveur de contenu sont protégées exactement de la même manière que les données elles-mêmes l’auraient été. Elles ne sont donc protégées ni plus ni moins contre la divulgation d’informations que si BranchCache n’avait pas du tout été utilisé.

-   Refus de service :

    *Un client est submergé de demandes de données*. Les protocoles BranchCache incorporent des compteurs de gestion des files d’attente et des minuteurs pour éviter que les clients soient surchargés.

## <a name="BKMK_11"></a>Processus de BranchCache : Mettre en cache du contenu

Sur les ordinateurs clients en mode de cache distribué et sur les serveurs de cache hébergé situés dans les filiales, des caches de contenu sont progressivement intégrés à mesure que du contenu est récupéré via des liaisons de réseau étendu (WAN).

Lorsque des ordinateurs clients sont configurés avec le mode de cache hébergé, ils ajoutent le contenu à leur propre cache local et proposent également des données au serveur de cache hébergé. Le protocole de cache hébergé offre un mécanisme permettant aux clients d’informer le serveur de cache hébergé sur la disponibilité du contenu et des segments.

Pour télécharger du contenu vers le serveur de cache hébergé, le client informe le serveur qu’il a un segment disponible. Le serveur de cache hébergé récupère alors toutes les informations de contenu associées au segment proposé, et télécharge les blocs présents dans le segment dont il a réellement besoin. Ce processus est répété jusqu’à ce que le client n’ait plus de segments à proposer au serveur de cache hébergé.

Pour mettre à jour le serveur de cache hébergé en utilisant le protocole de cache hébergé, les exigences suivantes doivent être satisfaites :

- L’ordinateur client doit disposer d’un ensemble de blocs contenus dans un segment qu’il peut proposer au serveur de cache hébergé. Le client doit fournir les informations de contenu correspondant au segment proposé ; elles sont constituées de l’ID de segment, du hachage de données du segment, du secret de segment et de la liste de tous les hachages de blocs contenus dans le segment.

- Cache hébergé pour les serveurs qui exécutent Windows Server 2008 R2, un serveur de cache hébergé de certificat et la clé privée associée sont requis, et l’autorité de certification (CA) qui a émis le certificat doit être approuvée par les ordinateurs clients dans la succursale. Cela permet au client et au serveur de participer correctement à l’authentification de serveur HTTPS.

    > [!IMPORTANT]
    > Les serveurs de cache hébergé qui exécutent Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 ne nécessitent pas un certificat de serveur de cache hébergé et la clé privée associée.  

- L’ordinateur client est configuré avec le nom d’ordinateur du serveur de cache hébergé et le numéro de port TCP (Transmission Control Protocol) sur lequel le serveur de cache hébergé écoute le trafic BranchCache. Certificat du serveur de cache hébergé est lié à ce port. Le nom d’ordinateur du serveur de cache hébergé peut être un nom de domaine complet (FQDN) si le serveur de cache hébergé est un ordinateur membre d’un domaine ; ou ce peut être le nom NetBIOS de l’ordinateur si le serveur de cache hébergé n’est pas membre d’un domaine.

- L’ordinateur client écoute activement les demandes de blocs entrantes. Le port sur lequel il effectue l’écoute est passé en tant que partie des messages d’offre envoyés par le client au serveur de cache hébergé. Cela permet au serveur de cache hébergé d’utiliser les protocoles BranchCache pour se connecter à l’ordinateur client afin de récupérer des blocs de données se trouvant dans le segment.

- Le serveur de cache hébergé commence à écouter les requêtes HTTP entrantes quand il est initialisé.

- Si le serveur de cache hébergé est configuré pour exiger l’authentification de l’ordinateur client, le client et le serveur de cache hébergé doivent tous les deux prendre en charge l’authentification HTTPS.

### <a name="hosted-cache-mode-cache-population"></a>Remplissage du cache en mode cache hébergé

Le processus d’ajout de contenu au cache du serveur de cache hébergé dans une filiale commence lorsque le client envoie un message INITIAL_OFFER_MESSAGE, lequel inclut l’ID de Segment. L’ID de Segment dans la demande INITIAL_OFFER_MESSAGE est utilisé pour récupérer le hachage de données de segment correspondant, la liste des hachages de blocs et le Secret de Segment à partir de blocs du serveur de cache hébergé. Si le serveur de cache hébergé dispose déjà de toutes les informations de contenu pour un segment particulier, la réponse au message INITIAL_OFFER_MESSAGE est OK, et aucune demande de téléchargement de blocs ne se produit.

Si le serveur de cache hébergé ne dispose pas de tous les blocs de données proposés qui sont associés aux hachages du segment, la réponse au message INITIAL_OFFER_MESSAGE est INTERESTED. Le client envoie alors un message SEGMENT_INFO_MESSAGE décrivant l’unique segment qui est proposé. Le serveur de cache hébergé répond par un message OK et commence le téléchargement des blocs manquants à partir de l’ordinateur client émetteur de l’offre.

Le hachage de données du segment, la liste des hachages de blocs et le secret de segment sont utilisés pour garantir que le contenu qui est téléchargé n’a pas été falsifié ou altéré. Les blocs téléchargés sont alors ajoutés au cache de blocs du serveur de cache hébergé.

## <a name="bkmk_cache"></a>Sécurité du cache  
Cette section fournit des informations sur la façon dont BranchCache sécurise les données mises en cache sur les ordinateurs clients et les serveurs de cache hébergé.

### <a name="client-computer-cache-security"></a>Sécurité du cache sur l’ordinateur client
La falsification constitue la plus grande menace pour les données stockées dans le cache BranchCache. Si une personne malveillante peut falsifier le contenu et les informations de contenu stockées dans le cache, il est possible d’utiliser cette faille pour essayer de lancer une attaque contre les ordinateurs qui utilisent BranchCache. Des personnes malveillantes peuvent initier une attaque en insérant un logiciel malveillant à la place d’autres données. BranchCache réduit ce risque en validant tout le contenu à l’aide des hachages de blocs se trouvant dans les informations de contenu. Si une personne malveillante tente d’altérer ces données, elles sont ignorées et sont remplacées par les données valides provenant de la source d’origine.

La divulgation d’informations représente une menace secondaire pour les données stockées dans le cache BranchCache. En mode de cache distribué, le client met en cache uniquement le contenu qu’il a lui-même demandé ; toutefois, les données, qui sont stockées en texte clair, courent un risque. Pour limiter l’accès au cache uniquement au service BranchCache, le cache local est protégé par les autorisations du système de fichiers qui sont spécifiées dans une liste de contrôle d’accès. 

Même si la liste de contrôle d’accès est efficace pour empêcher des utilisateurs non autorisés d’accéder au cache, il est possible qu’un utilisateur disposant de privilèges d’administrateur accède au cache en modifiant manuellement les autorisations spécifiées dans la liste de contrôle d’accès. BranchCache ne protège pas contre une utilisation malveillante d’un compte administratif.

Les données stockées dans le cache de contenu ne sont pas chiffrées ; par conséquent, si la perte de données représente un problème, vous pouvez utiliser des technologies de chiffrement telles que BitLocker ou le système de fichiers EFS (Encrypting File System). Le cache local utilisé par BranchCache n’augmente pas la menace de divulgation d’informations que représente un ordinateur de la filiale ; il contient uniquement des copies de fichiers qui résident non chiffrés ailleurs sur le disque. 

Il est particulièrement important de chiffrer la totalité du disque dans les environnements dans lesquels la sécurité physique des clients est difficile à garantir. Par exemple, le chiffrement de la totalité du disque permet de sécuriser des données sensibles sur des ordinateurs portables qui peuvent être sortis de l’environnement de la filiale.

### <a name="hosted-cache-server-cache-security"></a>Sécurité du cache sur le serveur de cache hébergé

En mode de cache hébergé, la plus grande menace pour la sécurité du serveur de cache hébergé est la divulgation d’informations. Dans un environnement de cache hébergé, BranchCache se comporte de la même manière qu’en mode de cache distribué, avec l’autorisation du système de fichiers qui protège les données mises en cache. La différence est que le serveur de cache hébergé stocke tout le contenu que n’importe quel ordinateur prenant en charge BranchCache dans la filiale demande, et non pas uniquement les données demandées par un seul client. Les conséquences d’une intrusion non autorisée dans ce cache pourraient être beaucoup plus graves, car davantage de données sont menacées.  
  
Dans un environnement de cache hébergé où le serveur de cache hébergé est en cours d’exécution Windows Server 2008 R2, l’utilisation de technologies de chiffrement telles que BitLocker ou EFS est conseillée si tous les clients de la filiale peuvent accéder aux données sensibles sur la liaison WAN. Il est également nécessaire d’empêcher un accès physique au cache hébergé, car le chiffrement de disque fonctionne uniquement lorsque l’ordinateur est éteint au moment où la personne malveillante y accède physiquement.  Si l’ordinateur est allumé ou en mode veille, la protection offerte par le chiffrement de disque est faible.

> [!NOTE]
> Les serveurs de cache hébergé qui exécutent Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 chiffrent toutes les données dans le cache par défaut, l’utilisation de technologies de chiffrement supplémentaire n’est pas requise.

Même si un client est configuré en mode de cache hébergé, il met toujours en cache les données localement, et vous pouvez prendre des mesures pour protéger le cache local en plus du cache situé sur le serveur de cache hébergé.
