---
title: Migrer les déploiements des services Bureau à distance vers Windows Server 2016
description: Cet article décrit comment migrer votre déploiement des services Bureau à distance vers de nouveaux serveurs Windows Server 2016.
ms.custom: na
ms.prod: windows-server
msreviewer: ''
nams.suite: ''
nams.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b1fa833-4325-48a8-bf34-46265f40c001
author: christianmontoya
manager: scottman
ms.openlocfilehash: e8471d67c8b9978d8b3c08fd3ef497e4e329d84b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387604"
---
# <a name="migrate-your-remote-desktop-services-deployment-to-windows-server-2016"></a>Migrer les déploiements des services Bureau à distance vers Windows Server 2016

Si vous utilisez actuellement des services Bureau à distance dans Windows Server 2012 R2, vous pouvez passer à Windows Server 2016 pour tirer parti des nouvelles fonctionnalités comme la prise en charge de SQL Azure et des espaces de stockage Direct.

La migration pour un déploiement des services Bureau à distance est prise en charge à partir de serveurs source exécutant Windows Server 2016 vers des serveurs de destination exécutant Windows Server 2016. En d’autres termes, il n’existe aucune migration directe à partir des services Bureau à distance exécutés sur Windows Server 2012 R2 vers Windows Server 2016. Pour la plupart des composants des services Bureau à distance, vous devez d’abord effectuer une mise à niveau vers Windows Server 2016 et migrer les données et les licences. Les seuls composants pour lesquels une migration directe est possible sont l'accès par le web aux services Bureau à distance, la passerelle des services Bureau à distance et le serveur de licences.

Pour plus d’informations sur la mise à niveau et la configuration requise, consultez [Mise à niveau de vos déploiements de services Bureau à distance vers Windows Server 2016](upgrade-to-rds-2016.md).

Utilisez les étapes suivantes pour migrer votre déploiement des services Bureau à distance :

- [Migrer des serveurs du service Broker pour les connexions Bureau à distance](#migrate-rdconnection-broker-servers)

- [Migrer les collections de sessions](#migrate-session-collections)

- [Migrer les collections de bureaux virtuels](#migrate-virtual-desktop-collections)

- [Migrer les serveurs d’accès Bureau à distance par le Web](#migrate-rdweb-access-servers)

- [Migrer les serveurs de passerelle Bureau à distance](#migrate-rdgateway-servers)

- [Migrer les serveurs de licences des services Bureau à distance](#migrate-rdgateway-servers)

- [Migrer les certificats](#migrate-certificates)

## <a name="migrate-rdconnection-broker-servers"></a>Migrer des serveurs du service Broker pour les connexions Bureau à distance

Il s’agit de la première de la migration et également de la plus importante : migration de vos agents de connexion Bureau à distance vers des serveurs de destination exécutant Windows Server 2016.

> [!IMPORTANT]
> Les serveurs sources Service Broker pour les connexions Bureau à distance doivent être configurés pour la haute disponibilité afin de prendre en charge la migration. Pour plus d’informations, consultez [Déployer un cluster du service Broker pour les connexions Bureau à distance](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md).

1. Si plusieurs serveurs Service Broker pour les connexions Bureau à distance sont présents dans la configuration de haute disponibilité, supprimez tous ces serveurs à l’exception du serveur actif.

2. [Mettez à niveau](upgrade-to-rds-2016.md) le serveur Service Broker pour les connexions Bureau à distance restant vers Windows Server 2016.

3. Ajoutez des serveurs Broker de connexion Bureau à distance sous Windows Server 2016 dans le déploiement de haute disponibilité.

> [!NOTE] 
> Une configuration de haute disponibilité mixte avec Windows Server 2016 et Windows Server 2012 R2 n’est pas prise en charge pour les serveurs Service Broker pour les connexions Bureau à distance. Un serveur Service Broker pour les connexions Bureau à distance exécutant Windows Server 2016 peut utiliser des collections de sessions avec les serveurs Hôte de session Bureau à distance exécutant Windows Server 2012 R2, et il peut utiliser des collections de bureaux virtuels avec des serveurs Hôte de virtualisation des services Bureau à distance exécutant Windows Server 2012 R2.

## <a name="migrate-session-collections"></a>Migrer les collections de sessions

Procédez comme suit pour migrer une collection de sessions dans Windows Server 2012 R2 vers une collection de sessions dans Windows Server 2016.

> [!IMPORTANT]
> Migrez les collections de sessions uniquement après avoir effectué l’étape précédente, [Migrer des serveurs Service Broker pour les connexions Bureau à distance](#migrate-rdconnection-broker-servers).

1. [Mettez à niveau la collection de sessions](Upgrade-to-RDSH-2016.md) à partir de Windows Server 2012 R2 vers Windows Server 2016.

2. Ajoutez le nouveau serveur Hôte de session Bureau à distance exécutant Windows Server 2016 à la collection de sessions.

3. Déconnectez-vous de toutes les sessions sur les serveurs Hôte de session Bureau à distance et supprimez les serveurs qui vont être migrés de la collection de sessions.

   > [!NOTE]
   > Si le modèle UVHD (UVHD-template.vhdx) est activé dans la collection de sessions et si le serveur de fichiers a été migré sur un nouveau serveur, mettez à jour la propriété de collection Disques de profil utilisateur : Emplacement avec le nouveau chemin. Les disques de profil utilisateur doivent être disponibles au même chemin relatif sur le serveur source et au nouvel emplacement.
   >
   > Une collection de sessions de serveurs Hôte de session Bureau à distance avec un mélange de serveurs exécutant Windows Server 2012 R2 et Windows Server 2016 n’est pas prise en charge.

## <a name="migrate-virtual-desktop-collections"></a>Migrer les collections de bureaux virtuels

Procédez comme suit pour migrer une collection de bureaux virtuels d’un serveur source exécutant Windows Server 2012 R2 vers un serveur de destination exécutant Windows Server 2016.

> [!IMPORTANT]
> Migrez les collections de bureaux virtuels uniquement après avoir effectué l’étape précédente, [Migrer des serveurs Service Broker pour les connexions Bureau à distance](#migrate-rdconnection-broker-servers).

1. [Mettez à jour la collection de bureaux virtuels](Upgrade-to-RDVH-2016.md) à partir du serveur exécutant Windows Server 2012 R2 vers Windows Server 2016.

2. Ajoutez les nouveaux serveurs Hôte de virtualisation des services Bureau à distance Windows Server 2016 à la collection de bureaux virtuels.

3. Migrez tous les ordinateurs virtuels dans la collection de bureaux virtuels actuelle qui s’exécutent sur les serveurs Hôte de virtualisation des services Bureau à distance sur les nouveaux serveurs.

4. Supprimez tous les serveurs Hôte de virtualisation des services Bureau à distance qui doivent être migrés de la collection de bureaux virtuels sur le serveur source.

> [!NOTE]
> Si le modèle UVHD (UVHD-template.vhdx) est activé dans la collection de sessions et si le serveur de fichiers a été migré sur un nouveau serveur, mettez à jour la propriété de collection Disques de profil utilisateur : Emplacement avec le nouveau chemin. Les disques de profil utilisateur doivent être disponibles au même chemin relatif sur le serveur source et au nouvel emplacement.
>
> Une collection de bureaux virtuels de serveurs Hôte de virtualisation Bureau à distance avec un mélange de serveurs exécutant Windows Server 2012 R2 et Windows Server 2016 n’est pas prise en charge.

## <a name="migrate-rdweb-access-servers"></a>Migrer les serveurs d’accès Bureau à distance par le Web

Suivez ces étapes pour migrer des serveurs d’accès Bureau à distance par le web :

- Joindre les serveurs de destination exécutant Windows Server 2016 au déploiement des services Bureau à distance et installer le rôle RD web

- Utilisez [l’outil IIS Web Deploy](https://www.iis.net/) pour migrer les paramètres de site web RD à partir des serveurs d’accès RD Web actuels vers les serveurs de destination exécutant Windows Server 2016.

- [Migrez des certificats](#migrate-certificates) vers les serveurs de destination exécutant Windows Server 2016.

- Supprimez les serveurs sources du déploiement des services Bureau à distance.

## <a name="migrate-rdgateway-servers"></a>Migrer les serveurs de passerelle Bureau à distance

Suivez ces étapes pour migrer des serveurs de passerelle Bureau à distance :

- Joindre les serveurs de destination exécutant Windows Server 2016 au déploiement des services Bureau à distance et installer le rôle RD Gateway

- Utilisez [l’outil IIS Web Deploy](https://www.iis.net/) pour migrer les paramètres de point de terminaison RD Gateway à partir des serveurs d’accès RD Gateway actuels vers les serveurs de destination exécutant Windows Server 2016.

- [Migrez des certificats](#migrate-certificates) vers les serveurs de destination exécutant Windows Server 2016.

- Supprimez les serveurs sources du déploiement des services Bureau à distance.

## <a name="migrate-rdlicensing-servers"></a>Migrer les serveurs de licences des services Bureau à distance

Procédez comme suit pour migrer un serveur de licences des services Bureau à distance à partir d’un serveur source exécutant Windows Server 2012 ou Windows Server 2012 R2 vers un serveur de destination exécutant Windows Server 2016.

1. [Migrez les licences RDS CA](migrate-rds-cals.md) du serveur source vers le serveur de destination.

2. Modifiez les **propriétés de déploiement** dans le **Gestionnaire de serveur** sur le serveur d’administration Bureau à distance (qui est généralement exécuté sur le premier serveur Service Broker pour les connexions Bureau à distance) pour inclure uniquement les nouveaux serveurs de licences des services Bureau à distance exécutant Windows Server 2016.

3. Désactiver le serveur de licences des services Bureau à distance source : Dans le **Gestionnaire de licences bureau à distance**, faites un clic droit sur le serveur approprié, pointez **Avancé** pour sélectionner **Désactiver le serveur**, puis suivez les étapes décrites dans l’assistant.

4. Supprimez du déploiement les serveurs de licences des services Bureau à distance sources dans le **Gestionnaire de serveur** sur le serveur d’administration Bureau à distance.

## <a name="migrate-certificates"></a>Migrer les certificats

Une migration réussie des certificats nécessite à la fois le processus de migration des certificats et la mise à jour des informations des certificats dans les propriétés du déploiement des services Bureau à distance.

Une migration de certificats classique comprend les étapes suivantes :

- Exporter le certificat vers un fichier PFX avec la clé privée.

- Importer le certificat à partir d’un fichier PFX.

Après avoir migré les certificats appropriés, mettez à jour les certificats requis suivants pour le déploiement des services Bureau à distance dans le gestionnaire de serveur ou PowerShell :

- Serveur Broker pour les connexions Bureau à distance - authentification unique

- Service Broker pour les connexions Bureau à distance - publication du fichier RDP

- Passerelle Bureau à distance - connexion HTTPS

- Accès Web de bureau à distance - connexion HTTPS et abonnement de connexion RemoteApp/bureau
