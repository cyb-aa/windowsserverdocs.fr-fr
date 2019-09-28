---
title: Étape 4 vérifier DirectAccess avec un mot de passe à usage unique
description: Cette rubrique fait partie du guide déployer l’accès à distance avec l’authentification par mot de passe à usage unique dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed49a0a3-1c45-42e5-8f13-cad20c1c1d68
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83ea3c4e4feefacde3e1ed7be6b605d8c0e644a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366972"
---
# <a name="step-4-verify-directaccess-with-otp"></a>Étape 4 vérifier DirectAccess avec un mot de passe à usage unique

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment vérifier que vous avez correctement configuré votre DirectAccess avec un déploiement à mot de passe à usage unique.
  
### <a name="to-verify-otp-health-on-the-remote-access-server"></a>Pour vérifier l’intégrité du mot de passe à usage unique sur le serveur d’accès à distance

1. Sur le serveur d’accès à distance, ouvrez la console de gestion de l' **accès à distance** .  

2. Sous **serveurs d’accès à distance** , cliquez sur le serveur d’accès à distance configuré pour la prise en charge du mot de passe à usage unique.  

3. Cliquez sur **État des opérations**.  

4. Vérifiez que l’état du mot de passe à usage unique affiche l’icône verte et fonctionne.  
  
    > [!NOTE]  
    > L’intervalle de mise à jour de l’état d’intégrité correspond au maximum de la somme des valeurs de la clé de Registre HKLM\SYSTEM\CCS\Services\Ramgmtsvc\parameters\HealthRefreshTimeout et de l' **intervalle de temps pour l’activité du serveur** qui a été défini dans l’accès à distance. configuré.  
  
### <a name="to-verify-access-to-internal-resources-using-otp-authentication"></a>Pour vérifier l’accès aux ressources internes avec l’authentification par mot de passe à usage unique  
  
1.  Connectez un ordinateur client DirectAccess au réseau d’entreprise et exécutez **gpupdate/force** à partir de l’invite de commandes pour obtenir la stratégie de groupe.  
  
2.  Déconnectez l’ordinateur client du réseau d’entreprise, connectez-vous au réseau externe et tentez d’accéder aux ressources internes. Vous ne devez pas avoir accès aux ressources internes.  
  
3.  Dans le cas d’un jeton logiciel, accédez au jeton du client OTP à l’aide des instructions du fournisseur et notez le code de jeton actuel. Lorsqu’un jeton matériel est utilisé, suivez les instructions du fournisseur pour l’authentification.  
  
4.  Cliquez sur le**connexions réseau**icône dans la zone de notification pour accéder au Gestionnaire de médias DA.  
  
5.  Cliquez sur la **connexion DirectAccess**, puis sur **Continuer**.  
  
6.  Entrez le code de jeton mentionné précédemment, puis cliquez sur **OK**. Attendez la fin de l’authentification. L’état de la connexion à l’espace de travail DirectAccess sera maintenant **connecté**.  
  
7.  Essayez d’accéder aux ressources internes. Vous devriez pouvoir accéder à toutes les ressources d’entreprise.  
  


