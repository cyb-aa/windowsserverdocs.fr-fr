---
title: Plan a Multi-Forest Deployment
description: Cette rubrique fait partie du guide déployer l’accès à distance dans un environnement à plusieurs forêts dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8acc260f-d6d1-4d32-9e3a-1fd0b2a71586
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a0f04a3ff7797d18f7647416dc99319860c7030
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404514"
---
# <a name="plan-a-multi-forest-deployment"></a>Plan a Multi-Forest Deployment

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit les étapes de planification requises pour configurer l’accès à distance dans un déploiement à forêts multiples.  
  
## <a name="prerequisites"></a>Conditions préalables  
Avant de déployer ce scénario, prenez connaissance des conditions requises suivantes qui ont leur importance :  
  
-   Une approbation bidirectionnelle est requise.  
  
## <a name="plan-trust-between-forests"></a>Prévoir l’approbation entre les forêts  
Lorsque vous décidez d’autoriser l’accès aux ressources depuis une nouvelle forêt, d’autoriser les clients de cette nouvelle forêt à utiliser DirectAccess ou d’ajouter des serveurs d’accès à distance depuis la nouvelle forêt en tant que points d’entrée vers le déploiement de l’accès à distance, vous devez vous assurer qu’une approbation complète, c’est-à-dire une approbation transitive bidirectionnelle, est configurée entre les deux forêts ; voir [Types d’approbations](https://technet.microsoft.com/library/cc775736.aspx). Une approbation complète entre les forêts constitue un prérequis pour un déploiement à forêts multiples afin d’autoriser les administrateurs à effectuer des opérations telles que la modification des objets de stratégie de groupe dans la nouvelle forêt, l’utilisation de groupes de sécurité depuis la nouvelle forêt en tant que groupe de sécurité client, l’émission d’appels distants (WinRM, RPC) vers des ordinateurs inclus dans la nouvelle forêt et l’authentification des clients distants de la nouvelle forêt.  
  
## <a name="plan-remote-access-administrator-permissions"></a>Prévoir les autorisations des administrateurs de l’accès à distance  
Lorsque vous configurez l’accès à distance, des objets de stratégie de groupe sont mis à jour et parfois créés dans chacun des domaines contenant des serveurs ou clients d’accès à distance. Dans un environnement à forêts multiples, comme dans les environnements à forêt unique, l’administrateur de l’accès à distance doit posséder des autorisations d’écriture et de modification des objets de stratégie de groupe DirectAccess et de leurs filtres de sécurité, ainsi que des autorisations de création de liens pour les objets de stratégie de groupe DirectAccess dans toutes les forêts impliquées. Ces autorisations sont requises quelle que soit la forêt à laquelle l’administrateur de l’accès à distance appartient.  
  
En outre, l’administrateur de l’accès à distance doit être administrateur local sur tous les serveurs d’accès à distance, notamment ceux de la nouvelle forêt qui sont ajoutés en tant que points d’entrée vers le déploiement de l’accès à distance d’origine.  
  
## <a name="ClientSG"></a>Planifier des groupes de sécurité client  
Vous devez configurer au moins un groupe de sécurité dans la nouvelle forêt pour les ordinateurs clients DirectAccess inclus dans cette nouvelle forêt. Cela est dû au fait qu’un groupe de sécurité ne peut pas contenir des comptes provenant de plusieurs forêts.  
  
> [!NOTE]  
> -   DirectAccess requiert au moins un groupe de sécurité Windows 10&reg; ou Windows&reg; 8 client pour chaque forêt. Toutefois, il est recommandé de disposer d’un groupe de sécurité client Windows 10 ou Windows 8 pour chaque domaine contenant des clients Windows 10 ou Windows 8.  
> -   Lorsque le multisite est activé, DirectAccess requiert au moins un groupe de sécurité client Windows 7&reg; par forêt pour chaque point d’entrée DirectAccess dans lequel les ordinateurs clients Windows 7 sont pris en charge. Toutefois, il est recommandé de disposer d’un groupe de sécurité client Windows 7 distinct pour chaque point d’entrée de chaque domaine contenant des clients Windows 7.  
>   
> Pour que DirectAccess soit appliqué aux ordinateurs clients situés dans d’autres domaines, il est nécessaire de créer des objets de stratégie de groupe dans ces domaines. L’ajout de groupes de sécurité déclenche l’écriture de nouveaux objets de stratégie de groupe clients pour les nouveaux domaines ; par conséquent, si vous ajoutez un nouveau groupe de sécurité à partir d’un nouveau domaine à la liste des groupes de sécurité clients DirectAccess, un objet de stratégie de groupe client est automatiquement créé sur le nouveau domaine et les ordinateurs clients du nouveau domaine obtiennent les paramètres DirectAccess via cet objet de stratégie de groupe client.  
>   
> Notez que si vous ajoutez un client depuis un nouveau domaine à un groupe de sécurité existant qui est déjà configuré en tant que groupe de sécurité client DirectAccess, l’objet de stratégie de groupe client n’est pas créé automatiquement par DirectAccess sur le nouveau domaine. Le client situé dans le nouveau domaine ne reçoit pas les paramètres DirectAccess et n’est pas en mesure de se connecter à l’aide de DirectAccess.  
  
## <a name="plan-certification-authorities"></a>Planifier les autorités de certification  
Si le déploiement DirectAccess est configuré de sorte à utiliser une authentification par mot de passe à usage unique, chaque forêt contient les mêmes modèles de certificat de signature mais avec des valeurs OID différentes. Par conséquent, les forêts ne peuvent pas être configurées en tant qu’unité de configuration unique. Pour résoudre ce problème et configurer un mot de passe à usage unique dans un environnement à plusieurs forêts, consultez la section « configurer un mot de passe à usage unique dans un déploiement à forêts multiples » dans la rubrique [configurer un déploiement à forêts multiples](Configure-a-Multi-Forest-Deployment.md).  
  
Dans le cadre de l’utilisation d’une authentification par certificat d’ordinateur IPsec, tous les ordinateurs clients et serveurs doivent posséder un certificat d’ordinateur émis par la même autorité de certification racine ou intermédiaire, quelle que soit la forêt à laquelle ils appartiennent.  
  
## <a name="plan-otp-exemptions"></a>Planifier les exemptions de mot de passe à usage unique  
Si vous utilisez l’authentification par mot de passe à usage unique DirectAccess, notez que le groupe de sécurité des exemptions de mot de passe à usage unique se limite aux utilisateurs d’une seule forêt. Ceci est dû au fait que chaque groupe de sécurité peut contenir uniquement les utilisateurs d’une seule forêt et qu’il est possible de configurer un seul groupe de sécurité de ce type.  
  


