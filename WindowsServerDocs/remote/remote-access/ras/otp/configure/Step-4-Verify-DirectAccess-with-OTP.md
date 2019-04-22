---
title: Étape 4 vérification DirectAccess avec l’OTP
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
ms.assetid: ed49a0a3-1c45-42e5-8f13-cad20c1c1d68
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bcc152723ee339c8652c9480647bfb8f438c87d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813240"
---
# <a name="step-4-verify-directaccess-with-otp"></a>Étape 4 vérification DirectAccess avec l’OTP

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment vérifier que vous avez correctement configuré votre DirectAccess avec l’OTP deployment.
  
### <a name="to-verify-otp-health-on-the-remote-access-server"></a>Pour vérifier l’intégrité de l’OTP sur le serveur d’accès à distance

1. Sur le serveur d’accès à distance ouvert le **gestion de l’accès à distance** console.  

2. Sous **des serveurs d’accès à distance** cliquez sur le serveur d’accès à distance a été configuré pour prendre en charge du secret à usage unique.  

3. Cliquez sur **état des opérations**.  

4. Vérifiez que l’état de l’OTP affiche l’icône verte et opérationnelle.  
  
    > [!NOTE]  
    > L’intervalle de mise à jour d’état d’intégrité sera un maximum de la somme des valeurs à partir de la clé de Registre HKLM\SYSTEM\CCS\Services\Ramgmtsvc\parameters\HealthRefreshTimeout et **intervalle de temps pour l’activité du serveur** qui a été défini dans le Configuration de l’accès à distance.  
  
### <a name="to-verify-access-to-internal-resources-using-otp-authentication"></a>Pour vérifier l’accès aux ressources internes à l’aide de l’authentification OTP  
  
1.  Connecter un ordinateur client DirectAccess au réseau d’entreprise et exécutez **gpupdate /force** à partir de l’invite de commandes pour obtenir la stratégie de groupe.  
  
2.  Se déconnecter de l’ordinateur client à partir du réseau d’entreprise, se connecter au réseau externe et tentez d’accéder aux ressources internes. Vous ne devez pas accéder aux ressources internes.  
  
3.  Dans le cas d’un jeton logiciel, le jeton client OTP en suivant les instructions du fournisseur d’accès et notez le code de jeton en cours. Lorsqu’un jeton matériel est utilisé, suivez les instructions du fournisseur pour l’authentification.  
  
4.  Cliquez sur le**connexions réseau**icône dans la zone de notification pour accéder au Gestionnaire de médias DA.  
  
5.  Cliquez sur le **connexion DirectAccess**, puis cliquez sur **continuer**.  
  
6.  Entrez le code de jeton indiqué précédemment, puis cliquez sur **OK**. Attendez que l’authentification. L’état de connexion d’espace de travail de DirectAccess seront désormais **connecté**.  
  
7.  Essayez d’accéder aux ressources internes. Vous devriez pouvoir accéder à toutes les ressources d’entreprise.  
  


