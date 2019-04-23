---
title: Étape 2 configurer le serveur RADIUS
description: Cette rubrique fait partie du guide de déploiement des accès à distance avec authentification OTP dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0326818f-9144-496c-b946-f82be4eefbd3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 219c333745d28bdedb9027c1dd46148ac80998f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840870"
---
# <a name="step-2-configure-the-radius-server"></a>Étape 2 configurer le serveur RADIUS

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Avant de configurer le serveur d’accès à distance pour prendre en charge DirectAccess avec l’OTP prennent en charge, vous configurez le serveur RADIUS.  
  
|Tâche|Description|  
|----|--------|  
|[2.1. Configurer les jetons de distribution de logiciels RADIUS](#BKMK_1.1)|Sur le serveur RADIUS configurer des jetons de distribution de logiciels.|  
|[2.2. Configurer les informations de sécurité RADIUS](#BKMK_1.2)|Sur le serveur RADIUS, configurez les ports et le secret partagé à utiliser.|  
|[2.3 Ajout de compte d’utilisateur pour la détection de secret à usage unique](#BKMK_Probe)|Sur le serveur RADIUS, créez un nouveau compte d’utilisateur pour la détection de secret à usage unique.|  
|[2.4 effectuer une synchronisation avec Active Directory](#BKMK_Active)|Sur le serveur RADIUS créer des comptes d’utilisateur synchronisés avec les comptes Active Directory.|  
|[2.5 configurer l’agent d’authentification RADIUS](#BKMK_AuthAgent)|Configurer le serveur d’accès à distance comme un agent d’authentification RADIUS.|  
  
## <a name="BKMK_1.1"></a>2.1 configurer les jetons de distribution de logiciels RADIUS  
Le serveur RADIUS doit être configuré avec la licence nécessaire et les jetons de distribution de logiciel et/ou matériel utilisable par DirectAccess avec secret à usage unique. Ce processus sera spécifique à chaque implémentation de fournisseur RADIUS.  
  
## <a name="BKMK_1.2"></a>2.2 configurer les informations de sécurité RADIUS  
Le serveur RADIUS utilise des ports UDP à des fins de communication, et chaque fournisseur RADIUS a ses propres ports UDP par défaut pour les communications entrantes et sortantes. Pour le serveur RADIUS travailler avec le serveur d’accès à distance, assurez-vous que tous les pare-feu dans l’environnement sont configurés pour autoriser le trafic UDP entre les serveurs DirectAccess et le secret à usage unique via les ports requis en fonction des besoins.  
  
Le serveur RADIUS utilise un secret partagé pour l’authentification. Configurer le serveur RADIUS avec mot de passe fort pour le secret partagé et notez qu’il sera utilisé lors de la configuration de configuration d’ordinateur client du serveur DirectAccess pour une utilisation avec DirectAccess avec secret à usage unique.  
  
## <a name="BKMK_Probe"></a>2.3 Ajout de compte d’utilisateur pour la détection de secret à usage unique  
Sur le serveur RADIUS, créez un nouveau compte d’utilisateur appelé **DAProbeUser** et lui donner le mot de passe **DAProbePass**.  
  
## <a name="BKMK_Active"></a>2.4 effectuer une synchronisation avec Active Directory  
Le serveur RADIUS doit avoir des comptes d’utilisateur qui correspondent aux utilisateurs dans Active Directory qui l’utiliserez DirectAccess avec secret à usage unique.  
  
#### <a name="to-synchronize-the-radius-and-active-directory-users"></a>Pour synchroniser les utilisateurs RADIUS et Active Directory  
  
1.  Enregistrer les informations utilisateur à partir d’Active Directory pour tous les DirectAccess avec les utilisateurs à usage.  
  
2.  Utilisez la procédure spécifique du fournisseur pour créer le même utilisateur **domaine\nom d’utilisateur** comptes qui ont été enregistrés dans le serveur RADIUS.  
  
## <a name="BKMK_AuthAgent"></a>2.5 configurer l’agent d’authentification RADIUS  
Le serveur d’accès à distance doit être configuré comme un agent d’authentification RADIUS pour DirectAccess avec l’implémentation de secret à usage unique. Suivez les instructions du fournisseur RADIUS pour configurer le serveur d’accès à distance comme un agent d’authentification RADIUS.  
  


