---
title: Migrer votre déploiement de Services Bureau à distance vers Windows Server 2016
description: Cet article décrit comment migrer votre déploiement des Services Bureau à distance vers de nouveaux serveurs Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 63037dd7e32320b6e640396e20344e5678ed91dd
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034433"
---
# <a name="migrate-your-remote-desktop-services-deployment-to-windows-server-2016"></a>Migrer votre déploiement de Services Bureau à distance vers Windows Server 2016

Si vous utilisez actuellement des Services Bureau à distance dans Windows Server 2012 R2, vous pouvez déplacer vers Windows Server 2016 pour tirer parti des nouvelles fonctionnalités telles que la prise en charge pour SQL Azure et les espaces de stockage Direct.

Migration d’un déploiement Services Bureau à distance est prise en charge à partir des serveurs sources exécutant Windows Server 2016 à des serveurs de destination exécutant Windows Server 2016. En d’autres termes, il n’existe aucune migration sur place directe à partir des services Bureau à distance dans Windows Server 2012 R2 vers Windows Server 2016. Au lieu de cela, pour la plupart des composants des services Bureau à distance, tout d’abord mettre à niveau vers Windows Server 2016 et migrer des données et licences. Les seuls composants avec une migration directe sont Web Bureau à distance, passerelle Bureau à distance et le serveur de licences.

Pour plus d’informations sur la mise à niveau et la configuration requise, consultez [mise à niveau de vos déploiements de Services Bureau à distance vers Windows Server 2016](upgrade-to-rds-2016.md).

Utilisez les étapes suivantes pour migrer votre déploiement des Services Bureau à distance :
- [Migrer les serveurs de service Broker pour les connexions Bureau à distance](#migrate-rd-connection-broker-servers)
- [Migrer les collections de sessions](#migrate-session-collections)
- [Migrer les collections de bureaux virtuels](#migrate-virtual-desktop-collections)
- [Migrer des serveurs d’accès Web de bureau à distance](#migrate-rd-web-access-servers)
- [Migrer les serveurs de passerelle Bureau à distance](#migrate-rd-gateway-servers)
- [Migrer des serveurs de licences des services Bureau à distance](#migrate-rd-licensing-servers)
- [Migrer des certificats](#migrate-certificates)

## <a name="migrate-rdconnection-broker-servers"></a>Migrer les serveurs de service Broker pour les connexions Bureau à distance

Ceci est la première et la plus importante pour la migration : migration de vos agents de connexion Bureau à distance vers les serveurs de destination exécutant Windows Server 2016.
> [!IMPORTANT] 
> Les serveurs de source de Broker de connexion Bureau à distance (RD Connection Broker) doivent être configurés pour la haute disponibilité prendre en charge la migration. Pour plus d’informations, consultez [déployer un cluster de service Broker pour les connexions Bureau à distance](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md).

1. Si plusieurs serveurs Service Broker pour les connexions Bureau à distance sont présents dans la configuration de haute disponibilité, supprimez tous ces serveurs à l’exception du serveur actif.
2. [Mettre à niveau](upgrade-to-rds-2016.md) serveur RD Connection Broker restant dans le déploiement vers Windows Server 2016.
3. Ajouter des serveurs de Broker de connexion Bureau à distance de Windows Server 2016 dans le déploiement de haute disponibilité.

> [!NOTE] 
> Une configuration haute disponibilité mixte avec Windows Server 2016 et Windows Server 2012 R2 n’est pas pris en charge pour les serveurs de service Broker pour les connexions Bureau à distance. Un Broker pour les connexions Bureau à distance exécutant Windows Server 2016 peut utiliser des collections de sessions avec les serveurs hôte de Session Bureau à distance exécutant Windows Server 2012 R2, et il peut utiliser des collections de bureaux virtuels avec les serveurs hôte de virtualisation Bureau à distance exécutant Windows Server 2012 R2.

## <a name="migrate-session-collections"></a>Migrer les collections de sessions

Suivez ces étapes pour migrer une collection de sessions dans Windows Server 2012 R2 vers une collection de sessions dans Windows Server 2016.
> [!IMPORTANT] 
> Migrer des collections de sessions uniquement après avoir effectué l’étape précédente, [serveurs de migrer RD Connection Broker](#migrate-rd-connection-broker-servers).

1. [Mise à niveau de la collection de sessions](Upgrade-to-RDSH-2016.md) à partir de Windows Server 2012 R2 vers Windows Server 2016.
2. Ajoutez le nouveau serveur hôte de Session Bureau à distance exécutant Windows Server 2016 à la collection de sessions.
3. Déconnectez-vous de toutes les sessions sur les serveurs Hôte de session Bureau à distance et supprimez les serveurs qui vont être migrés de la collection de sessions. 
> [!NOTE]
> Si le modèle UVHD (UVHD-Template.vhdx) est activé dans la collection de sessions et le serveur de fichiers a été migré vers un nouveau serveur, mettez à jour les disques de profil utilisateur : Propriété de collection d’emplacement avec le nouveau chemin. Les disques de profil utilisateur doivent être disponibles au même chemin relatif sur le serveur source et au nouvel emplacement.
>
> Une collection de sessions de serveurs hôtes de Session Bureau à distance avec un mélange de serveurs qui exécutent Windows Server 2012 R2 et Windows Server 2016 n’est pas pris en charge.

## <a name="migrate-virtual-desktop-collections"></a>Migrer les collections de bureaux virtuels

Suivez ces étapes pour migrer une collection de bureaux virtuels à partir d’un serveur source exécutant Windows Server 2012 R2 vers un serveur de destination exécutant Windows Server 2016.

> [!IMPORTANT] 
> Migrer des collections de bureaux virtuels uniquement après avoir effectué l’étape précédente, [serveurs de migrer RD Connection Broker](#migrate-rd-connection-broker-servers).

1. [Mise à niveau de la collection de bureaux virtuels](Upgrade-to-RDVH-2016.md) à partir du serveur exécutant Windows Server 2012 R2 vers Windows Server 2016.
2. Ajouter les nouveaux serveurs hôte de virtualisation Bureau à distance de Windows Server 2016 à la collection de bureaux virtuels.
3. Migrez tous les ordinateurs virtuels dans la collection de bureaux virtuels actuelle qui s’exécutent sur les serveurs Hôte de virtualisation des services Bureau à distance sur les nouveaux serveurs. 
4. Supprimez tous les serveurs Hôte de virtualisation des services Bureau à distance qui doivent être migrés de la collection de bureaux virtuels sur le serveur source.

> [!NOTE] 
> Si le modèle UVHD (UVHD-Template.vhdx) est activé dans la collection de sessions et le serveur de fichiers a été migré vers un nouveau serveur, mettez à jour les disques de profil utilisateur : Propriété de collection d’emplacement avec le nouveau chemin. Les disques de profil utilisateur doivent être disponibles au même chemin relatif sur le serveur source et au nouvel emplacement.
>
> Une collection de bureaux virtuelle de serveurs hôtes de virtualisation des services Bureau à distance avec un mélange de serveurs qui exécutent Windows Server 2012 R2 et Windows Server 2016 n’est pas pris en charge.

## <a name="migrate-rdweb-access-servers"></a>Migrer les serveurs d’accès Bureau à distance par le Web
Suivez ces étapes pour migrer des serveurs d’accès Web de bureau à distance :
- Joindre les serveurs de destination exécutant Windows Server 2016 pour le déploiement des Services Bureau à distance et d’installer le rôle Web de bureau à distance
- Utilisez [outil IIS Web Deploy](https://www.iis.net/) pour migrer les paramètres de site Web Bureau à distance à partir des serveurs d’accès Web de bureau à distance en cours pour les serveurs de destination exécutant Windows Server 2016.
- [Migrer des certificats](#migrate-certificates) pour les serveurs de destination exécutant Windows Server 2016.
- Supprimez les serveurs source du déploiement des Services Bureau à distance  

## <a name="migrate-rdgateway-servers"></a>Migrer les serveurs de passerelle Bureau à distance
Suivez ces étapes pour migrer les serveurs de passerelle Bureau à distance :
- Joindre les serveurs de destination exécutant Windows Server 2016 pour le déploiement des Services Bureau à distance et d’installer le rôle de passerelle Bureau à distance
- Utilisez [outil IIS Web Deploy](https://www.iis.net/) pour migrer les paramètres de point de terminaison de passerelle Bureau à distance à partir des serveurs de passerelle Bureau à distance en cours pour les serveurs de destination exécutant Windows Server 2016.
- [Migrer des certificats](#migrate-certificates) pour les serveurs de destination exécutant Windows Server 2016.
- Supprimez les serveurs source le déploiement des Services Bureau à distance  

## <a name="migrate-rdlicensing-servers"></a>Migrer les serveurs de licences des services Bureau à distance

Suivez ces étapes pour migrer un serveur de licences des services Bureau à distance à partir d’un serveur source exécutant Windows Server 2012 ou Windows Server 2012 R2 vers un serveur de destination exécutant Windows Server 2016.

1. [Migrer les licences d’accès client des Services Bureau à distance (RDS CAL)](migrate-rds-cals.md) à partir du serveur source vers le serveur de destination.
2. Modifier le **propriétés de déploiement** dans **le Gestionnaire de serveur** sur le serveur d’administration Bureau à distance (qui est généralement en cours d’exécution sur le premier serveur du service Broker pour les connexions Bureau à distance) pour inclure uniquement les nouvelles licences des services Bureau serveurs exécutant Windows Server 2016.
3. Désactiver le serveur de licences des services Bureau à distance source : Dans **Gestionnaire de licences bureau à distance**, cliquez sur le serveur approprié, pointez sur **avancé** pour sélectionner **désactiver le serveur**, puis suivez les étapes décrites dans l’Assistant .
4. Supprimer les serveurs de licences des services Bureau à distance source du déploiement dans **le Gestionnaire de serveur** sur le serveur d’administration Bureau à distance.

## <a name="migrate-certificates"></a>Migrer les certificats

Migration des certificats réussie nécessite à la fois le processus de migration des certificats et de mise à jour des informations de certificat dans les propriétés de déploiement de Services Bureau à distance.

Migration de certificats classique comprend les étapes suivantes :
- Exportez le certificat vers un fichier PFX avec la clé privée.
- Importez le certificat à partir d’un fichier PFX.

Après avoir migré les certificats appropriés, mettez à jour les certificats requis suivants pour le déploiement des Services Bureau à distance dans le Gestionnaire de serveur ou de PowerShell : 
- Service Broker pour les connexions Bureau à distance - l’authentification unique
- Service Broker pour les connexions Bureau à distance - publication du fichier RDP
- Passerelle Bureau à distance - connexion HTTPS
- Accès Web de bureau à distance - connexion HTTPS et abonnement de connexion de RemoteApp/de bureau
