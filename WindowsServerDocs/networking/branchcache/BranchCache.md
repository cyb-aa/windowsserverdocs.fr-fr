---
title: BranchCache
description: Cette rubrique fournit une vue d’ensemble de BranchCache dans Windows Server2016
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
ms.openlocfilehash: 5540d89827b80a21bf23f6a2aa8f54f09dfde67a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache"></a>BranchCache

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique, qui s’adresse aux professionnels de l’Information informatique, fournit des informations générales sur BranchCache, notamment les modes BranchCache, les fonctionnalités, les fonctionnalités et les fonctionnalités BranchCache disponibles dans différents systèmes d’exploitation.

> [!NOTE]
> Outre cette rubrique, la documentation BranchCache suivante est disponible.
> 
> - [Environnement réseau BranchCache et les commandes Windows PowerShell](../branchcache/BranchCache-Network-Shell-and-Windows-PowerShell-Commands.md)
> -   [Guide de déploiement de BranchCache](../branchcache/deploy/BranchCache-Deployment-Guide.md)

**S’adresse-t-il dans BranchCache?**

Si vous êtes un administrateur système, réseau ou architecte de solutions de stockage ou tout autre professionnel de l’informatique, BranchCache peut vous intéresser dans les circonstances suivantes:

- Vous concevez ou prendre en charge l’infrastructure informatique d’une organisation qui a deux ou plusieurs emplacements physiques et une connexion réseau (étendu WAN) à partir de filiales au siège social.

- Conception ou de la prise en charge de l’infrastructure informatique d’une organisation qui a déployé des technologies de cloud, et une connexion WAN est utilisée par les travailleurs pour accéder aux données et des applications à des emplacements distants.

- Vous voulez optimiser l’utilisation de la bande passante réseau étendu en réduisant la quantité de trafic réseau entre les filiales et le siège social.

- Vous avez déployé ou prévoyez de déployer des serveurs de contenu au siège social qui correspondent aux configurations décrites dans cette rubrique.

- Les ordinateurs clients dans vos filiales exécutent Windows10, Windows8.1, Windows8 ou Windows7.

Cette rubrique contient les sections suivantes:

-   [Nouveautés de BranchCache?](#bkmk_what)

-   [Modes BranchCache](#BKMK_2)
  
-   [Serveurs de contenu BranchCache](#BKMK_3)
  
-   [BranchCache et le cloud](#BKMK_3a)
  
-   [Versions des informations de contenu](#bkmk_version)  
  
-   [Comment BranchCache gère les mises à jour dans les fichiers de contenu](#bkmk_handles)  
  
-   [Guide d’installation de BranchCache](#BKMK_4)  
  
-   [Versions de système d’exploitation pour BranchCache](#bkmk_os)  
  
-   [Sécurité BranchCache](#bkmk_security)  
  
-   [Flux de contenu et de processus](#bkmk_flow)  
  
-   [Sécurité du cache](#bkmk_cache)  
  
## <a name="bkmk_what"></a>Nouveautés de BranchCache?

BranchCache est une technologie d’optimisation étendu réseau la bande passante (WAN) incluse dans certaines éditions des systèmes d’exploitation Windows Server2016 et Windows10, ainsi que dans certaines éditions de Windows Server2012R2, Windows8.1, Windows Server2012, Windows8, Windows Server2008R2 et Windows7. Pour optimiser la bande passante réseau étendue lorsque des utilisateurs accéder au contenu sur des serveurs distants, BranchCache extrait le contenu de votre siège social ou les serveurs de contenu cloud hébergé et met en cache dans les filiales, permettant ainsi aux clients les ordinateurs dans des filiales d’accéder au contenu localement plutôt que via le réseau étendu.
  
Dans les filiales, le contenu est stocké sur les serveurs qui sont configurés pour héberger le cache ou, si aucun serveur n’est disponible dans la filiale, sur les ordinateurs clients qui exécutent Windows10, Windows8.1, Windows8 ou Windows7. Une fois un ordinateur client demande et reçoit du contenu à partir du siège social et le contenu est mis en cache dans la filiale, les autres ordinateurs de la même filiale peuvent obtenir le contenu localement plutôt que de télécharger le contenu à partir du serveur de contenu sur la liaison WAN.

Lorsque des demandes ultérieures pour le même contenu sont effectuées par les ordinateurs clients, les clients téléchargent *informations de contenu* à partir du serveur au lieu du contenu réel. Informations de contenu sont composent de hachages qui sont calculés à l’aide de blocs du contenu d’origine et sont extrêmement petits par rapport au contenu dans les données d’origine. Ordinateurs clients utilisent ensuite les informations de contenu pour localiser le contenu à partir d’un cache de la filiale, si le cache se trouve sur un ordinateur client ou sur un serveur. Serveurs et ordinateurs clients utilisent également des informations de contenu pour sécuriser le contenu mis en cache afin qu’il ne peut pas être accessible par les utilisateurs non autorisés.

BranchCache augmente la productivité de l’utilisateur final en améliorant les délais de réponse aux requêtes des clients et serveurs dans les filiales et peut également aider à améliorer les performances du réseau en réduisant le trafic sur les liaisons WAN.

## <a name="BKMK_2"></a>Modes BranchCache
BranchCache a deux modes de fonctionnement: le mode de cache et le mode de cache hébergé distribuées.

Lorsque vous déployez BranchCache en mode de cache distribué, le cache de contenu dans une filiale est distribué entre les ordinateurs clients.

Lorsque vous déployez BranchCache en mode de cache hébergé, le cache de contenu dans une filiale est hébergé sur un ou plusieurs ordinateurs serveurs, qui sont appelés des serveurs de cache hébergé.

> [!NOTE]
> Vous pouvez déployer BranchCache à l’aide de ces deux modes, toutefois qu’un seul mode peut être utilisé par filiale. Par exemple, si vous avez deux filiales, une disposant d’un serveur et l’autre pas, vous pouvez déployer BranchCache en mode de cache hébergé dans le bureau qui contient un serveur, tout en déployant BranchCache en mode de cache distribué dans le bureau qui contient uniquement les ordinateurs clients.

Dans l’illustration suivante, BranchCache est déployé dans les deux modes.  

![Modes BranchCache](../media/BranchCache/bc_modes.jpg)

Mode de cache distribué convient mieux aux petites filiales qui ne contiennent pas d’un serveur local à utiliser en tant que serveur de cache hébergé. Mode de cache distribué vous permet de déployer BranchCache sans matériel supplémentaire dans les filiales.

Si la filiale où vous voulez déployer BranchCache comprend une infrastructure supplémentaire, par exemple, un ou plusieurs serveurs qui exécutent d’autres charges de travail, déploiement de BranchCache en mode de cache hébergé est avantageux pour les raisons suivantes:

### <a name="increased-cache-availability"></a>Disponibilité améliorée du cache

Mode de cache hébergé augmente l’efficacité du cache, car le contenu est disponible même si le client qui avait demandé et mis en cache les données est hors connexion. Étant donné que le serveur de cache hébergé est toujours disponible, davantage de contenu est mis en cache, en fournissant des économies de bande passante réseau étendu supérieures, et l’efficacité de BranchCache est améliorée.

### <a name="centralized-caching-for-multiple-subnet-branch-offices"></a>Mise en cache pour les succursales à plusieurs sous-réseaux centralisée


Mode de cache distribué fonctionne sur un seul sous-réseau. Dans la plusieurs sous-réseaux filiale qui est configurée pour le mode de cache distribué, un fichier téléchargé sur un sous-réseau ne peuvent pas être partagé avec les ordinateurs clients sur d’autres sous-réseaux. 

Pour cette raison, les clients sur d’autres sous-réseaux, ne peut pas détecter que le fichier a déjà été téléchargé, obtient le fichier à partir du serveur de contenu du siège social, à l’aide de la bande passante réseau étendue dans le processus.

Lorsque vous déployez le mode de cache hébergé, toutefois, cela n’est pas le cas, tous les clients dans une succursale de plusieurs sous-réseaux peuvent accéder à un seul et même cache, qui est stocké sur le serveur de cache hébergé, même si les clients se trouvent sur des sous-réseaux différents. De plus, BranchCache dans Windows Server2016, Windows Server2012R2 et Windows Server2012 offre la possibilité de déployer plusieurs serveurs de cache hébergé par filiale.

> [!CAUTION]
> Si vous utilisez BranchCache pour la mise en cache SMB des fichiers et dossiers, ne désactivez pas la fonctionnalité fichiers hors connexion. Si vous désactivez la fonctionnalité fichiers hors connexion, BranchCache SMB la mise en cache ne fonctionne pas correctement.

## <a name="BKMK_3"></a>Serveurs de contenu BranchCache

Lorsque vous déployez BranchCache, le contenu source est stocké sur des serveurs contenus compatible BranchCache dans votre siège social ou dans un centre de données cloud. Les types de serveurs de contenu suivants sont pris en charge par BranchCache:

> [!NOTE]
> Seul le contenu source - autrement dit, contenu que les ordinateurs clients obtiennent initialement à partir d’un serveur de contenu compatible BranchCache - est accéléré par BranchCache. Contenu que les ordinateurs clients obtiennent directement à partir d’autres sources, telles que des serveurs Web sur Internet ou de la mise à jour de Windows, n’est pas mis en cache par les ordinateurs clients ou serveurs de cache hébergé et alors partagé avec d’autres ordinateurs de la filiale. Toutefois, si vous souhaitez accélérer le contenu de la mise à jour de Windows, vous pouvez installer un serveur d’applications WindowsServerUpdateServices (WSUS) à votre siège social ou du centre de données cloud et configurez-le en tant que serveur de contenu BranchCache.

### <a name="web-servers"></a>Serveurs Web

Les serveurs Web pris en charge incluent les ordinateurs qui exécutent Windows Server2016, Windows Server2012R2, Windows Server2012 ou Windows Server2008R2 sur lesquels le rôle de serveur serveur Web (IIS) installé et qui utilisent le protocole HTTP (Hypertext Transfer) ou HTTPS (HTTP Secure).

En outre, le serveur Web doit avoir la fonctionnalité BranchCache est installée.

### <a name="file-servers"></a>Serveurs de fichiers

Serveurs de fichiers pris en charge incluent les ordinateurs qui exécutent Windows Server2016, Windows Server2012R2, Windows Server2012 ou Windows Server2008R2 et ayant le rôle de serveur Services de fichiers et BranchCache pour fichiers réseau installé. 

Ces serveurs de fichiers utilisent le bloc de Message serveur (SMB) pour échanger des informations entre les ordinateurs. Après avoir terminé l’installation de votre serveur de fichiers, vous devez également partager des dossiers et activer la génération de hachage pour les dossiers partagés à l’aide de la stratégie de groupe ou stratégie ordinateur Local pour activer BranchCache.

### <a name="application-servers"></a>Serveurs d’applications

Serveurs d’applications pris en charge incluent les ordinateurs qui exécutent Windows Server2016, Windows Server2012R2, Windows Server2012 ou Windows Server2008R2 avec BITS Background Intelligent Transfer Service () installé et activé. 

En outre, le serveur d’applications doit disposer de la fonctionnalité BranchCache est installée. Exemples de serveurs d’applications, vous pouvez déployer des serveurs MicrosoftWindowsServerUpdateServices (WSUS) et le Point de Distribution branche MicrosoftSystemCenter ConfigurationManager en tant que serveurs de contenu BranchCache.

## <a name="BKMK_3a"></a>BranchCache et le cloud

Le cloud a énorme potentiel de réduire les frais de fonctionnement et atteindre de nouveaux niveaux de montée en puissance parallèle, mais les charges de travail des personnes qui en dépendent peut augmenter les coûts de mise en réseau et affecter la productivité. Les utilisateurs souhaitent des performances élevées et ne soucient pas où leurs applications et les données sont hébergées. 

BranchCache peut améliorer les performances des applications en réseau et réduire la consommation de bande passante avec un cache de données partagé.  Il améliore la productivité dans les filiales et dans le siège social, où les travailleurs utilisent les serveurs qui sont déployés dans le cloud.

Étant donné que BranchCache ne requiert pas de nouveau matériel ou modifications de topologie de réseau, il est une excellente solution pour améliorer la communication entre les sites et des nuages publics et privés.

> [!NOTE]
> Étant donné que certains proxys Web ne peut pas traiter les en-têtes de codage de contenu non standard, il est recommandé que vous utilisez BranchCache avec Hyper texte sécurisé HTTPS (Transfer Protocol) et non HTTP.
  
=== Pour plus d’informations sur les technologies de cloud dans Windows Server2016, voir [logiciel défini de mise en réseau et #40; sDN et #41; ](../sdn/Software-Defined-Networking--SDN-.md).
  
## <a name="bkmk_version"></a>Versions des informations de contenu

Il existe deux versions des informations de contenu:

- Informations de contenu qui sont compatibles avec les ordinateurs exécutant Windows Server2008R2 et Windows7 sont appelées version1, ou V1. Avec la segmentation de fichiers BranchCache V1 segments de fichiers sont plus volumineuse que dans V2 et de taille fixe. En raison des tailles de grands segments fixes, lorsqu’un utilisateur apporte une modification qui change la longueur du fichier, non seulement le segment avec la modification n’est invalidé, mais tous les segments jusqu'à la fin du fichier sont invalidés. L’appel suivant pour le fichier modifié par un autre utilisateur dans la filiale aboutit donc à économies de bande passante réseau étendu réduites, car le contenu modifié et tout le contenu après la modification sont envoyées via la liaison WAN.

- Informations de contenu qui sont compatibles avec les ordinateurs exécutant Windows Server2016, Windows10, Windows Server2012R2, Windows8.1, Windows Server2012 et Windows8 sont appelées version2, ou V2. Informations de contenu v2 utilisent des segments plus petits et de taille variable qui sont plus tolérants aux modifications dans un fichier. Cela augmente la probabilité que des segments d’une version antérieure du fichier peut être réutilisée lorsque les utilisateurs accèdent à une version mise à jour, ils récupèrent uniquement la partie modifiée du fichier à partir du serveur de contenu et en utilisant moins de bande passante réseau étendu.

Le tableau suivant fournit des informations sur la version des informations de contenu qui est utilisée selon le client, le serveur de contenu, et hébergé des systèmes d’exploitation de cache server que vous utilisez dans votre déploiement BranchCache.

> [!NOTE]
> Dans le tableau ci-dessous, l’acronyme «Se» signifie système d’exploitation.

|Système d’exploitation client|Serveur de contenu du système d’exploitation|Système d’exploitation du serveur de Cache hébergé|Version des informations de contenu|
|-------------|---------------------|--------------------------|-------------------------------|
|Windows Server2008R2 et Windows7 |Windows Server2012 ou version ultérieure|Windows Server2012 ou version ultérieure; aucun pour le mode de cache distribué|V1|
|Windows Server2012 ou version ultérieure; Windows8 ou version ultérieure|Windows Server2008R2 |Windows Server2012 ou version ultérieure; aucun pour le mode de cache distribué|V1|
|Windows Server2012 ou version ultérieure; Windows8 ou version ultérieure| Windows Server2012 ou version ultérieure| Windows Server2008R2 |V1|
|Windows Server2012 ou version ultérieure; Windows8 ou version ultérieure|Windows Server2012 ou version ultérieure|Windows Server2012 ou version ultérieure; aucun pour le mode de cache distribué|V2|

Lorsque vous avez des serveurs de contenu et serveurs de cache qui exécutent Windows Server2016, Windows Server2012R2 et Windows Server2012 hébergé, ils utilisent la version des informations de contenu qui est appropriée en fonction du système d’exploitation du client BranchCache qui demande des informations. 

Lorsque les ordinateurs exécutant Windows Server2012 et Windows8 ou des systèmes d’exploitation ultérieurs demandent du contenu, les serveurs de cache hébergé et de contenu utilisent les informations de contenu V2; lorsque les ordinateurs exécutant Windows Server2008R2 et Windows7 demandent du contenu, les serveurs de cache hébergé et de contenu utilisent les informations de contenu V1.

>[!IMPORTANT]
>Lorsque vous déployez BranchCache en mode de cache distribué, les clients qui utilisent des versions différentes informations de contenu ne partagent pas de contenu entre eux. Par exemple, un ordinateur client exécutant Windows7 et un ordinateur client exécutant Windows10 qui sont installés dans la même filiale ne partagent pas le contenu entre eux.
  
## <a name="bkmk_handles"></a>Comment BranchCache gère les mises à jour dans les fichiers de contenu

Lorsque les utilisateurs des filiales, modifient ou mettre à jour le contenu des documents, leurs modifications sont écrites directement sur le serveur de contenu au siège social sans implication de BranchCache. Cela est vrai que l’utilisateur ait téléchargé le document à partir du serveur de contenu ou obtenu à partir de l’un cache hébergé ou distribué dans la filiale.

Lorsque le fichier modifié est demandé par un autre client dans une filiale, les nouveaux segments du fichier sont téléchargés à partir du serveur du siège social et ajoutés au cache distribué ou hébergé dans cette filiale. Pour cette raison, les utilisateurs des filiales reçoivent toujours les versions les plus récentes du contenu mis en cache.

## <a name="BKMK_4"></a>Guide d’installation de BranchCache

Vous pouvez utiliser le Gestionnaire de serveur dans Windows Server2016 pour installer la fonctionnalité BranchCache ou le BranchCache pour fichiers réseau du rôle de serveur Services de fichiers. Vous pouvez utiliser le tableau suivant pour déterminer si vous souhaitez installer le service de rôle ou la fonctionnalité.

|Fonctionnalités|Emplacement de l’ordinateur|Installer cet élément BranchCache|
|-----------------|---------------------|------------------------------------|
|Serveur de contenu \ (serveur d’applications BITS)|Centre de données office ou cloud principal|Fonctionnalité BranchCache|
|Serveur de contenu \(Web server\)|Centre de données office ou cloud principal|Fonctionnalité BranchCache|
|Serveur de contenu \ (serveur de fichiers à l’aide de la protocol\ SMB)|Centre de données office ou cloud principal|BranchCache pour fichiers réseau du rôle de serveur Services de fichiers|
|Serveur de cache hébergé|Filiale|Fonctionnalité BranchCache avec le mode de serveur de cache hébergé activé|
|Ordinateur client compatible BranchCache|Filiale|Aucune installation nécessaire; activer simplement BranchCache et un mode BranchCache \(distributed or hosted\) sur le client|

Pour installer le service de rôle ou la fonctionnalité, ouvrez le Gestionnaire de serveur et sélectionnez les ordinateurs où vous souhaitez activer la fonctionnalité BranchCache. Dans le Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**. Le **Ajout de rôles et fonctionnalités** Assistant s’ouvre. Lorsque vous exécutez l’Assistant, effectuez les sélections suivantes:

- Sur la page Assistant **sélectionner le Type d’Installation**, sélectionnez **Installation basée sur un rôle ou une fonctionnalité**.

- Sur la page Assistant **sélectionner des rôles de serveur**, si vous installez un serveur de fichiers BranchCache, développez **Services de fichiers et stockage** et **des Services de fichiers et iSCSI**, puis sélectionnez **BranchCache pour fichiers réseau**.  Pour économiser de l’espace disque, vous pouvez également sélectionner le **la déduplication des données** rôle de service et poursuivez par le biais de l’Assistant installation et l’achèvement. Si vous ne souhaitez pas installer un serveur de fichiers BranchCache, n’installez pas le rôle Services de fichiers et stockage avec BranchCache pour fichiers réseau.

- Sur la page Assistant **sélectionner des fonctionnalités**, si vous installez un serveur de contenu qui n’est pas un serveur de fichiers ou si vous installez un serveur de cache hébergé, sélectionnez **BranchCache**, puis passez par le biais de l’Assistant installation et l’achèvement. Si vous ne souhaitez pas installer un serveur de contenu autre que le serveur de fichiers ou un serveur de cache hébergé, n’installez pas la fonctionnalité BranchCache.
  
## <a name="bkmk_os"></a>Versions de système d’exploitation pour BranchCache

Voici une liste des systèmes d’exploitation qui prennent en charge différents types de fonctionnalités BranchCache.

### <a name="operating-systems-for-branchcache-client-computer-functionality"></a>Systèmes d’exploitation pour les fonctionnalités des ordinateurs clients BranchCache

Les systèmes d’exploitation suivants fournissent BranchCache avec prise en charge pour le Service de transfert Intelligent en arrière-plan (BITS), Hyper texte HTTP (Transfer Protocol) et bloc de Message serveur (SMB).

- Windows10 entreprise

- Windows10 éducation

- Windows8.1 entreprise

- Windows8 entreprise

- Windows7 entreprise

- Windows7 Édition intégrale

Dans les systèmes d’exploitation suivants, BranchCache ne prend pas en charge les fonctionnalités HTTP et SMB, mais ne prend pas en charge la fonctionnalité BranchCache BITS.

-   Windows10 Professionnel, BITS prise en charge uniquement

-   Windows8.1Professionnel, BITS prise en charge uniquement

-   Windows8 Professionnel, BITS prise en charge uniquement

-   Windows7 Professionnel, BITS prise en charge uniquement

> [!NOTE]
> BranchCache n’est pas disponible par défaut dans les systèmes d’exploitation Windows Server2008 ou WindowsVista. Sur ces systèmes d’exploitation, toutefois, si vous téléchargez et installez la mise à jour WindowsManagementFramework, les fonctionnalités BranchCache sont disponible pour le protocole de Service de transfert Intelligent en arrière-plan (BITS) uniquement. Pour plus d’informations et pour télécharger WindowsManagementFramework, voir [WindowsManagementFramework (WindowsPowerShell2.0, WinRM 2.0 et BITS 4.0)](https://go.microsoft.com/fwlink/?LinkId=188677) à https://go.microsoft.com/fwlink/?LinkId=188677.
  
### <a name="operating-systems-for-branchcache-content-server-functionality"></a>Les fonctionnalités de serveur de contenu de systèmes d’exploitation pour BranchCache

Vous pouvez utiliser les familles de systèmes d’exploitation de Windows Server2016, Windows Server2012R2 et Windows Server2012 en tant que serveurs de contenu BranchCache.

En outre, la famille Windows Server2008R2 de systèmes d’exploitation peut être utilisée comme serveurs de contenu BranchCache, avec les exceptions suivantes:

- BranchCache n’est pas pris en charge dans les installations Server Core de Windows Server2008R2 Enterprise avec Hyper-V.

- BranchCache n’est pas pris en charge dans les installations Server Core de Windows Server2008R2 Datacenter avec Hyper-V.

### <a name="operating-systems-for-branchcache-hosted-cache-server-functionality"></a>Fonctionnalités de serveur de cache hébergé de systèmes d’exploitation pour BranchCache

Vous pouvez utiliser les familles de systèmes d’exploitation de Windows Server2016, Windows Server2012R2 et Windows Server2012 comme serveurs de cache hébergé de BranchCache.

En outre, les systèmes d’exploitation Windows Server2008R2 suivants peuvent être utilisés comme serveurs de cache hébergé de BranchCache:

- Windows Server2008R2 entreprise

- Windows Server2008R2 Enterprise avec Hyper-V

- Windows Server2008R2 entreprise Installation minimale

- Windows Server2008R2 entreprise Installation minimale avec Hyper-V

- Windows Server2008R2 pour systèmes Itanium

- Windows Server2008R2 Datacenter

- Windows Server2008R2 Datacenter avec Hyper-V

- Windows Server2008R2 Datacenter Installation minimale avec Hyper-V

## <a name="bkmk_security"></a>Sécurité BranchCache

BranchCache implémente une approche de conception sécurisée fonctionne de façon transparente avec vos architectures de sécurité réseau existantes, sans nécessité de l’équipement supplémentaire ou la configuration de sécurité plus complexes.
  
BranchCache est non invasif et ne modifie pas les processus d’authentification ou d’autorisation Windows. Une fois que vous déployez BranchCache, l’authentification est effectuée toujours à l’aide des informations d’identification de domaine et la manière de l’autorisation avec des listes de contrôle d’accès (ACL) reste inchangé. En outre, les autres configurations continuent à fonctionner comme avant le déploiement de BranchCache.

Le modèle de sécurité BranchCache est basé sur la création de métadonnées, qui prennent la forme d’une série de hachages. Ces hachages sont également appelées informations de contenu.

Une fois les informations de contenu sont créées, il est utilisé dans les échanges de messages BranchCache plutôt que les données réelles, et elles sont échangées à l’aide des protocoles pris en charge (HTTP, HTTPS et SMB).

Données mises en cache sont maintenues chiffrées et ne sont pas accessibles par les clients qui n’ont pas d’autorisation d’accéder au contenu à partir de la source d’origine.  Les clients doivent être authentifiés et autorisés par la source de contenu d’origine avant qu’ils peuvent récupérer les métadonnées de contenu et doivent posséder les métadonnées du contenu pour accéder au cache dans la filiale.

### <a name="how-branchcache-generates-content-information"></a>Comment BranchCache génère des informations de contenu

Étant donné que les informations de contenu sont créées à partir de plusieurs éléments, la valeur des informations de contenu est toujours unique. Ces éléments sont:

- Le contenu réel (par exemple, les pages Web ou des fichiers partagés) à partir duquel les hachages sont dérivés.  

- Paramètres de configuration, telles que la taille d’algorithme et bloc de hachage. Pour générer des informations de contenu, le serveur de contenu divise le contenu en segments et puis subdivise ces segments en blocs. BranchCache utilise des hachages de chiffrement sécurisés pour identifier et vérifier chaque bloc et chaque segment, la prise en charge l’algorithme de hachage SHA256.

- Un secret de serveur. Tous les serveurs de contenu doivent être configurés avec un secret de serveur, qui est une valeur binaire de longueur arbitraire.

> [!NOTE]
> L’utilisation d’un secret de serveur garantit que les ordinateurs clients ne sont pas en mesure de générer les informations de contenu eux-mêmes. Cela empêche les utilisateurs malveillants d’utiliser des attaques en force brute avec les ordinateurs clients prenant en charge BranchCache pour deviner des modifications mineures entre les versions dans des situations dans lesquelles le client avait accès à une version précédente, mais n’a pas accès à la version actuelle.

### <a name="content-information-details"></a>Détails des informations de contenu

BranchCache utilise le secret de serveur en tant que clé afin de dériver un hachage spécifique au contenu qui est envoyé aux clients autorisés. Appliquer un algorithme de hachage au secret de serveur combinée et le hachage de données génère ce hachage.

Ce hachage est appelé le secret de segment. BranchCache utilise des secrets de segment pour sécuriser les communications. De plus, BranchCache crée une liste des hachages de bloc, qui est la liste des blocs de données hachés, et le hachage de données, qui est généré en hachant la liste rouge de hachage.

Les informations de contenu comprennent les éléments suivants:

- La liste des hachages de blocs:

    `BlockHashi = Hash(dataBlocki)   1<=i<=n`

- Le hachage de données (HoD):

    `HoD = Hash(BlockHashList)`

- Secret de segment (Kp):

    `Kp = HMAC(Ks, HoD)`

BranchCache utilise le protocole de mise en cache le contenu des homologues et le protocole de récupération Framework pour implémenter les processus qui sont nécessaires pour assurer la mise en cache et la récupération des données entre les caches de contenu sécurisé.

De plus, BranchCache gère informations de contenu avec le même niveau de sécurité qu’il utilise pour la gestion et de la transmission du contenu réel lui-même.

## <a name="bkmk_flow"></a>Flux de contenu et de processus

Le flux des informations de contenu et le contenu réel est divisé en quatre phases:

1.  [Processus BranchCache: demander du contenu](#BKMK_8)

2.  [Processus BranchCache: localiser du contenu](#BKMK_9)

3.  [Processus BranchCache: récupérer du contenu](#BKMK_10)

4.  [Processus BranchCache: mettre en Cache le contenu](#BKMK_11)

Les sections suivantes décrivent ces phases.

## <a name="BKMK_8"></a>Processus BranchCache: demander du contenu

Dans la première phase, l’ordinateur client dans la filiale demande du contenu, tel qu’un fichier ou une page Web, à partir d’un serveur de contenu dans un emplacement distant, comme un siège social. Le serveur de contenu vérifie que l’ordinateur client est autorisé à recevoir le contenu demandé. Si l’ordinateur client est autorisé et serveur de contenu et de client sont compatibles BranchCache\, le serveur de contenu génère des informations de contenu.

Le serveur de contenu envoie ensuite les informations de contenu à l’ordinateur client utilisant le même protocole qu’aurait été utilisée pour le contenu réel. 

Par exemple, si l’ordinateur client a demandé une page Web via HTTP, le serveur de contenu envoie les informations de contenu à l’aide de HTTP. Pour cette raison, le contenu de garantir la sécurité au niveau du réseau et les informations de contenu sont identiques.

Une fois la partie initiale des informations de contenu (hachage de données + Secret de Segment) reçue, l’ordinateur client effectue les actions suivantes:

- Utilise le Secret de Segment (Kp) comme clé de chiffrement (Ke).

- Génère l’ID de Segment (HoHoDk) à partir des valeurs HoD et Kp:

    `HoHoDk = HMAC(Kp, HoD + C), where C is the ASCII string "MS_P2P_CACHING" with NUL terminator.`

La principale menace à ce stade est le risque le Secret de Segment, toutefois, BranchCache chiffre les blocs de données de contenu pour protéger le Secret de Segment. Pour cela, BranchCache à l’aide de la clé de chiffrement qui est dérivée le Secret de Segment du segment de contenu dans lequel se trouvent les blocs de contenu.

Cette approche garantit qu’une entité qui n’est pas en possession du secret de serveur ne peut pas découvrir le contenu réel dans un bloc de données. Le Secret de Segment est traité avec le même niveau de sécurité comme texte en clair de segment lui-même, car la connaissance du Secret de Segment pour un segment donné permet à une entité d’obtenir le segment à partir d’homologues et puis de le déchiffrer. La connaissance du Secret de serveur ne produit pas immédiatement un texte brut particulier, mais peut être utilisée pour dériver certains types de données à partir du texte chiffré, puis d’exposer éventuellement certaines données partiellement connues à une attaque de déchiffrement en force brute. Par conséquent, le secret de serveur doit rester confidentiel.
  
## <a name="BKMK_9"></a>Processus BranchCache: localiser du contenu

Une fois les informations de contenu sont reçues par l’ordinateur client, le client utilise l’ID de Segment pour localiser le contenu demandé dans le cache de la filiale locale, soit que le cache est distribué entre les ordinateurs clients se trouve sur un serveur de cache hébergé.

Si l’ordinateur client est configuré pour le mode de cache hébergé, il est configuré avec le nom d’ordinateur du serveur de cache hébergé et contacte ce serveur pour récupérer le contenu.

Si l’ordinateur client est configuré pour le mode de cache distribué, toutefois, le contenu peut être stocké dans plusieurs caches de plusieurs ordinateurs dans la filiale. L’ordinateur client doit découvrir où le contenu se trouve avant que le contenu est récupéré.

Lorsqu’ils sont configurés pour le mode de cache distribué, les ordinateurs clients localisent du contenu à l’aide d’un protocole de découverte qui est basé sur le protocole Web Services Dynamic Discovery (WS-Discovery). Les clients envoient WS-Discovery multidiffusion les messages d’exploration de découvrir le contenu mis en cache sur le réseau. Messages d’exploration incluent l’ID de Segment, ce qui permet aux clients de vérifier si le contenu demandé correspond au contenu stocké dans leur cache. Clients qui reçoivent la réponse de message sonde initiale au client effectuant demande avec les messages Probe-Match de monodiffusion si l’ID de Segment correspond au contenu est mis en cache localement.  

La réussite du processus WS-Discovery dépend du fait que le client qui effectue la découverte dispose des informations de contenu correctes, ce qui a été fournies par le serveur de contenu pour le contenu qu’il demande.

La principale menace pour les données pendant la phase de demande de contenu est la divulgation d’informations, car l’accès aux informations de contenu implique un accès autorisé au contenu. Pour atténuer ce risque, le processus de découverte ne divulgue pas les informations de contenu, autre que l’ID de Segment, ce qui ne révèle pas quoi que ce soit sur le segment en texte brut qui contient le contenu.

En outre, un autre ordinateur client exécuté par un utilisateur malveillant sur le même sous-réseau de réseau peut voir le trafic de découverte BranchCache vers la source de contenu d’origine passant par le routeur.

Si le contenu demandé est introuvable dans la filiale, le client demande le contenu directement à partir du serveur de contenu sur la liaison WAN.

Une fois que le contenu est reçu, il est ajouté au cache local, sur l’ordinateur client ou sur un serveur de cache hébergé. Dans ce cas, les informations de contenu empêche un client ou serveur d’ajouter dans le cache local tout contenu qui ne correspond pas aux hachages de cache hébergé. Le processus de vérification du contenu en faisant correspondre les hachages garantit que seul le contenu valide est ajouté au cache, et l’intégrité du cache local est protégée.

## <a name="BKMK_10"></a>Processus BranchCache: récupérer du contenu

Une fois un ordinateur client localise le contenu souhaité sur l’hôte de contenu, qui est un serveur de cache hébergé ou un ordinateur de client en mode cache distribué, l’ordinateur client commence le processus de récupération du contenu.

Tout d’abord l’ordinateur client envoie une demande à l’hôte de contenu pour le premier bloc dont il a besoin. La demande contient la plage d’ID de Segment et les blocs qui identifient le contenu souhaité. Étant donné que seul un bloc est retourné, la plage de blocs contient un seul bloc. (Demandes de plusieurs blocs sont actuellement pas pris en charge.) Le client stocke également la demande dans sa liste de demandes non traitées locale.  

Lorsqu’il reçoit un message de demande valide d’un client, l’hôte de contenu vérifie si le bloc spécifié dans la demande existe dans le cache de contenu de l’hôte de contenu.

Si l’hôte de contenu est en possession du bloc de contenu, l’hôte de contenu envoie une réponse qui contient l’ID de Segment, l’ID de bloc, le bloc de données chiffré et le vecteur d’initialisation qui est utilisé pour chiffrer le bloc.

Si l’hôte de contenu n’est pas en possession du bloc de contenu, l’hôte de contenu envoie un message de réponse vide. Cela informe l’ordinateur client que l’hôte de contenu ne dispose pas du bloc demandé. Un message de réponse vide contient l’ID de Segment et l’ID de bloc du bloc demandé, ainsi que d’un bloc de données de taille zéro.

Lorsque l’ordinateur client reçoit la réponse de l’hôte de contenu, le client vérifie que le message correspond à un message de demande dans la liste des demandes en attente de. (L’index de l’ID de Segment et bloc doit correspondre à celui d’une demande non traitée.)

Si ce processus de vérification échoue et que l’ordinateur client n’a pas d’un message de demande correspondant dans liste des demandes en attente, l’ordinateur client ignore le message.

Si ce processus de vérification réussit et que l’ordinateur client dispose d’un message de demande correspondant liste des demandes en attente, l’ordinateur client déchiffre le bloc. Le client valide alors le bloc déchiffré contre le hachage de bloc approprié à partir des informations de contenu que le client est initialement obtenu à partir du serveur de contenu d’origine.

Si la validation du bloc réussit, le bloc déchiffré est stocké dans le cache.

Ce processus est répété jusqu'à ce que le client ait tous les blocs requis.

> [!NOTE]
> Si les segments de contenu complets n’existent pas sur un seul ordinateur, le protocole de récupération récupère et assemble le contenu à partir d’une combinaison de sources: un ensemble de distribué les ordinateurs clients de cache en mode, un serveur de cache hébergé et - si met en cache de la filiale ne contiennent pas le contenu complet, le serveur de contenu d’origine au siège social.

Avant que BranchCache envoie des informations de contenu ou le contenu, les données sont chiffrées. BranchCache chiffre le bloc dans le message de réponse. Dans Windows7, l’algorithme de chiffrement par défaut par BranchCache est AES-128, la clé de chiffrement est Ke et la taille de clé est 128bits, comme stipulé par l’algorithme de chiffrement. 

BranchCache génère un vecteur d’initialisation qui convient à l’algorithme de chiffrement et utilise la clé de chiffrement pour chiffrer le bloc. BranchCache enregistre alors l’algorithme de chiffrement et le vecteur d’initialisation dans le message. 

Serveurs et clients jamais exchange, partagent ou envoient la clé de chiffrement. Le client reçoit la clé de chiffrement du serveur qui héberge le contenu de la source de contenu. Ensuite, à l’aide du chiffrement algorithme et vecteur d’initialisation qu’il a reçu à partir du serveur, il déchiffre le bloc. Il n’existe aucune autre authentification ou autorisation explicite intégré au protocole de téléchargement.

### <a name="security-threats"></a>Menaces de sécurité

Les principales menaces de sécurité à ce stade sont les suivantes:

- Falsification des données:

  *Un client servant de données à un demandeur falsifie les données*. Le modèle de sécurité BranchCache utilise des hachages pour confirmer que le client ni le serveur n’a altéré les données.  

- Divulgation d’informations:  

    *BranchCache envoie du contenu chiffré à tout client qui spécifie l’ID de Segment approprié*. Les ID de segment sont publics, les clients peuvent recevoir du contenu chiffré. Toutefois, si un utilisateur malveillant obtienne le contenu chiffré, il doit connaître la clé de chiffrement pour déchiffrer le contenu. Le protocole de couche supérieure effectue l’authentification, puis donne les informations de contenu au client authentifié et autorisé. La sécurité des informations de contenu est équivalente à la sécurité fournie au contenu lui-même, et BranchCache n’expose jamais les informations de contenu.  

    *Un agresseur le réseau afin d’obtenir le contenu*.  BranchCache chiffre tous les transferts entre les clients à l’aide de AES128, où la clé secrète est Ke, empêche données surveillés provenant du câble.  Informations de contenu qui sont téléchargées à partir du serveur de contenu sont protégées de la même façon que les données elles-mêmes aurait été et sont donc ne plus ou moins protégées divulgation d’informations que si BranchCache n’avait pas du tout été utilisé.

-   Attaque par déni de Service:

    *Un client est submergé de demandes de données*. Les protocoles BranchCache incorporent des compteurs de gestion de la file d’attente et les minuteurs pour éviter que les clients soient surchargés.

## <a name="BKMK_11"></a>Processus BranchCache: mettre en Cache le contenu

Sur les ordinateurs client en mode cache distribué et serveurs de cache hébergé qui sont trouvent dans les filiales, caches de contenu sont progressivement intégrés à comme le contenu est récupéré via des liaisons WAN.

Lorsque les ordinateurs clients sont configurés avec le mode de cache hébergé, ils ajouter du contenu à leur propre cache local et proposent également des données sur le serveur de cache hébergé. Le protocole de Cache hébergé fournit un mécanisme pour les clients informer le serveur de cache hébergé sur la disponibilité du contenu et des segments.

Pour télécharger le contenu sur le serveur de cache hébergé, le client informe le serveur qu’il dispose d’un segment qui est disponible. Le serveur de cache hébergé récupère alors toutes les informations de contenu qui sont associé au segment proposé et téléchargement les blocs dans le segment dont il a réellement besoin. Ce processus est répété jusqu'à ce que le client ne dispose d’aucune plus de segments à proposer le serveur de cache hébergé.

Pour mettre à jour le serveur de cache hébergé à l’aide du protocole de Cache hébergé, les conditions suivantes doivent être remplies:

- L’ordinateur client doit posséder un ensemble de blocs contenus dans un segment qu’il peut proposer au serveur de cache hébergé. Le client doit fournir les informations de contenu correspondant au segment proposé; cela se compose de l’ID de Segment, le hachage de données du segment, le Secret de Segment et une liste de tous les hachages de blocs contenus dans le segment.

- Cache hébergé pour les serveurs qui exécutent Windows Server2008R2, un serveur de cache hébergé de certificat et la clé privée associée sont requis, et l’autorité de certification (CA) qui a émis le certificat doit être approuvée par les ordinateurs clients dans la filiale. Cela permet au client et serveur de participer correctement à l’authentification de serveur HTTPS.

    > [!IMPORTANT]
    > Serveurs de cache hébergé qui exécutent Windows Server2016, Windows Server2012R2 ou Windows Server2012 ne nécessitent pas un certificat de serveur de cache hébergé et une clé privée associée.  

- L’ordinateur client est configuré avec le nom d’ordinateur du serveur de cache hébergé et le numéro de port de protocole TCP (Transmission Control) sur lequel le serveur de cache hébergé écoute le trafic BranchCache. Certificat du serveur de cache hébergé est lié à ce port. Le nom d’ordinateur du serveur de cache hébergé peut être un nom de domaine complet (FQDN), si le serveur de cache hébergé est un ordinateur membre du domaine; ou il peut être le nom NetBIOS de l’ordinateur si le serveur de cache hébergé n’est pas membre d’un domaine.

- L’ordinateur client écoute activement les bloquer les demandes entrantes. Le port sur lequel il est à l’écoute est passé en tant que partie des messages d’offre à partir du client vers le serveur de cache hébergé. Cela permet au serveur de cache hébergé à utiliser les protocoles BranchCache pour se connecter à l’ordinateur client pour récupérer des blocs de données dans le segment.

- Le serveur de cache hébergé commence à écouter les requêtes HTTP entrantes quand il est initialisé.

- Si le serveur de cache hébergé est configuré pour exiger l’authentification de l’ordinateur client, le client et le serveur de cache hébergé sont requis pour la prise en charge l’authentification HTTPS.

### <a name="hosted-cache-mode-cache-population"></a>Population de cache de mode de cache hébergé

Le processus d’ajout de contenu au cache du serveur de cache hébergé dans une filiale commence quand le client envoie un message INITIAL_OFFER_MESSAGE, lequel inclut l’ID de Segment. L’ID de Segment dans la demande INITIAL_OFFER_MESSAGE est utilisé pour récupérer le hachage de données du segment correspondant, la liste des hachages de blocs et le Secret de Segment à partir du cache de blocs du serveur de cache hébergé. Si le serveur de cache hébergé dispose déjà de toutes les informations de contenu pour un segment particulier, la réponse au message INITIAL_OFFER_MESSAGE est OK, et aucune demande de téléchargement de blocs se produit.

Si le serveur de cache hébergé ne dispose pas de tous les blocs de données proposés qui sont associés les hachages du segment, la réponse au message INITIAL_OFFER_MESSAGE est INTERESTED. Le client envoie alors un message SEGMENT_INFO_MESSAGE décrivant l’unique segment qui est proposé. Le serveur de cache hébergé répond avec un message OK et commence le téléchargement des blocs manquants à partir de l’offre ordinateur client.

Le hachage de données du segment, la liste des hachages de blocs et le secret de segment sont utilisés pour vous assurer que le contenu qui est téléchargé n’a pas été falsifié ou altéré. Les blocs téléchargés sont alors ajoutés au cache de blocs du serveur de cache hébergé.

## <a name="bkmk_cache"></a>Sécurité du cache  
Cette section fournit des informations sur la façon dont BranchCache sécurise les données mises en cache sur les ordinateurs clients et sur les serveurs de cache hébergé.

### <a name="client-computer-cache-security"></a>Sécurité du cache de l’ordinateur client
La falsification de la plus grande menace pour les données stockées dans le cache BranchCache. Si une personne malveillante peut falsifier le contenu et informations stockées dans le cache, puis il est possible d’utiliser cette pour essayer de lancer une attaque contre les ordinateurs qui utilisent BranchCache. Les pirates peuvent initier une attaque en insérant un logiciel malveillant à la place d’autres données. BranchCache réduit ce risque en validant tout le contenu à l’aide des hachages de blocs se trouvés dans les informations de contenu. Si un attaquant tente d’altérer ces données, il est ignoré et est remplacée par les données valides à partir de la source d’origine.

Une menace secondaire pour les données stockées dans le cache BranchCache est la divulgation d’informations. En mode de cache distribué, le client met en cache uniquement le contenu qu’il a demandé à lui-même; toutefois, ces données sont stockées en texte clair et peuvent être en danger. Pour limiter l’accès du cache sur le BranchCache Service uniquement, le cache local est protégé par des autorisations de système de fichiers qui sont spécifiées dans une liste ACL. 

Bien que la liste ACL est efficace pour empêcher les utilisateurs non autorisés d’accéder à la mémoire cache, il est possible pour un utilisateur disposant de privilèges administratifs d’accéder au cache en modifiant manuellement les autorisations qui sont spécifiées dans la liste ACL. BranchCache ne protège pas contre l’utilisation malveillante d’un compte administratif.

Les données stockées dans le cache de contenu ne sont pas chiffrées, si les fuites de données sont un problème, vous pouvez utiliser les technologies de chiffrement telles que BitLocker ou le système EFS (ENCRYPTING File System). Le cache local est utilisé par BranchCache n’augmente pas la menace de divulgation d’informations que représente un ordinateur dans la filiale; le cache contient uniquement des copies de fichiers qui résident non chiffrés ailleurs sur le disque. 

Le chiffrement de la totalité du disque est particulièrement important dans les environnements dans lesquels la sécurité physique des clients est difficile de garantir. Par exemple, le chiffrement de la totalité du disque permet pour sécuriser les données sensibles sur des ordinateurs portables qui peuvent être supprimés de l’environnement de filiales.

### <a name="hosted-cache-server-cache-security"></a>Sécurité de cache de serveur de cache hébergé

En mode de cache hébergé, la plus grande menace pour la sécurité du serveur de cache hébergé est la divulgation d’informations. BranchCache dans un environnement de cache hébergé se comporte de manière similaire pour le mode de cache distribué, avec les autorisations de système de fichiers protège les données mises en cache. La différence est que le serveur de cache hébergé stocke tout le contenu que n’importe quel ordinateur prenant en charge BranchCache dans la filiale demande, plutôt que simplement les données qui demande un seul client. Les conséquences d’une intrusion non autorisée dans ce cache peuvent être beaucoup plus graves, car davantage de données sont exposés.  
  
Dans un environnement de cache hébergé où le serveur de cache hébergé exécute Windows Server2008R2, l’utilisation de technologies de chiffrement telles que BitLocker ou EFS est conseillée si un des clients de la filiale peut accéder à des données sensibles sur la liaison WAN. Il est également nécessaire empêcher l’accès physique au cache hébergé, étant donné que le chiffrement de disque fonctionne uniquement lorsque l’ordinateur est éteint lorsque la personne malveillante y accède physiquement.  Si l’ordinateur est allumé ou en mode veille, le chiffrement des disques offre une protection minimale.

> [!NOTE]
> Serveurs de cache hébergé qui exécutent Windows Server2016, Windows Server2012R2 ou Windows Server2012 crypter toutes les données dans le cache par défaut, l’utilisation de technologies de chiffrement supplémentaire n’est pas requise.

Même si un client est configuré en mode de cache hébergé, il met toujours en cache les données localement, et vous souhaitez peut-être prendre des mesures pour protéger le cache local en plus du cache sur le serveur de cache hébergé.
