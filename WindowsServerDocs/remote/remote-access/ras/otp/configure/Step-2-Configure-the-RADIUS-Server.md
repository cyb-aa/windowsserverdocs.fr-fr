---
title: Étape 2 configurer le serveur RADIUS
description: Cette rubrique fait partie du guide déployer l’accès à distance avec l’authentification par mot de passe à usage unique dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0326818f-9144-496c-b946-f82be4eefbd3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 83c63e8d11b4f93b87e1f49f487342d3955f6372
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313642"
---
# <a name="step-2-configure-the-radius-server"></a>Étape 2 configurer le serveur RADIUS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Avant de configurer le serveur d’accès à distance pour prendre en charge DirectAccess avec prise en charge du mot de passe à usage unique, vous configurez le serveur RADIUS.  
  
|Tâche|Description|  
|----|--------|  
|[2,1. Configuration des jetons de distribution de logiciels RADIUS](#BKMK_1.1)|Sur le serveur RADIUS, configurez les jetons de distribution de logiciels.|  
|[2,2. Configuration des informations de sécurité RADIUS](#BKMK_1.2)|Sur le serveur RADIUS, configurez les ports et le secret partagé à utiliser.|  
|[2,3 Ajout d’un compte d’utilisateur pour la détection de mot de passe à usage unique](#BKMK_Probe)|Sur le serveur RADIUS, créez un nouveau compte d’utilisateur pour la détection de mot de passe à usage unique.|  
|[2,4 synchroniser avec Active Directory](#BKMK_Active)|Sur le serveur RADIUS, créez des comptes d’utilisateur synchronisés avec les comptes de Active Directory.|  
|[2,5 Configuration de l’agent d’authentification RADIUS](#BKMK_AuthAgent)|Configurez le serveur d’accès à distance en tant qu’agent d’authentification RADIUS.|  
  
## <a name="21-configure-the-radius-software-distribution-tokens"></a><a name="BKMK_1.1"></a>2,1 configurer les jetons de distribution de logiciels RADIUS  
Le serveur RADIUS doit être configuré avec la licence requise et les jetons de distribution de matériel et/ou matériel à utiliser par DirectAccess avec le mot de passe à usage unique. Ce processus sera spécifique à chaque implémentation de fournisseur RADIUS.  
  
## <a name="22-configure-the-radius-security-information"></a><a name="BKMK_1.2"></a>2,2 configurer les informations de sécurité RADIUS  
Le serveur RADIUS utilise des ports UDP à des fins de communication, et chaque fournisseur RADIUS a ses propres ports UDP par défaut pour les communications entrantes et sortantes. Pour que le serveur RADIUS fonctionne avec le serveur d’accès à distance, assurez-vous que tous les pare-feu de l’environnement sont configurés pour autoriser le trafic UDP entre les serveurs DirectAccess et OTP sur les ports requis en fonction des besoins.  
  
Le serveur RADIUS utilise un secret partagé à des fins d’authentification. Configurez le serveur RADIUS avec un mot de passe fort pour le secret partagé, et Notez qu’il sera utilisé lors de la configuration de la configuration de l’ordinateur client du serveur DirectAccess pour une utilisation avec DirectAccess avec un mot de passe à usage unique.  
  
## <a name="23-adding-user-account-for-otp-probing"></a><a name="BKMK_Probe"></a>2,3 Ajout d’un compte d’utilisateur pour la détection de mot de passe à usage unique  
Sur le serveur RADIUS, créez un nouveau compte d’utilisateur appelé **DAProbeUser** et donnez-lui le mot de passe **DAProbePass**.  
  
## <a name="24-synchronize-with-active-directory"></a><a name="BKMK_Active"></a>2,4 synchroniser avec Active Directory  
Le serveur RADIUS doit avoir des comptes d’utilisateur qui correspondent aux utilisateurs de Active Directory qui utiliseront DirectAccess avec un mot de passe à usage unique.  
  
#### <a name="to-synchronize-the-radius-and-active-directory-users"></a>Pour synchroniser le RADIUS et les utilisateurs Active Directory  
  
1.  Enregistrez les informations utilisateur à partir de Active Directory pour tous les utilisateurs DirectAccess avec mot de passe à usage unique.  
  
2.  Utilisez la procédure spécifique du fournisseur pour créer des comptes utilisateur **domaine\nom_utilisateur** identiques dans le serveur RADIUS qui ont été enregistrés.  
  
## <a name="25-configure-the-radius-authentication-agent"></a><a name="BKMK_AuthAgent"></a>2,5 Configuration de l’agent d’authentification RADIUS  
Le serveur d’accès à distance doit être configuré en tant qu’agent d’authentification RADIUS pour l’implémentation DirectAccess avec mot de passe à usage unique. Suivez les instructions du fournisseur RADIUS pour configurer le serveur d’accès à distance en tant qu’agent d’authentification RADIUS.  
  


