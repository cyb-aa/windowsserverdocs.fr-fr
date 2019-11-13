---
title: Conditions préalables pour le déploiement de DirectAccess
description: Cette rubrique présente les conditions préalables pour le déploiement de DirectAccess dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6e04dbf1576277493ec849c8de82aeab51e97649
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388814"
---
# <a name="prerequisites-for-deploying-directaccess"></a>Conditions préalables pour le déploiement de DirectAccess

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le tableau suivant répertorie les conditions préalables nécessaires à l’utilisation des assistants de configuration pour déployer DirectAccess.  
  
|||  
|-|-|  
|Scénario|Conditions préalables|  
|[Déployer un serveur DirectAccess unique à l’aide de l’Assistant Prise en main](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-Le pare-feu Windows doit être activé sur tous les profils<br /><br />-Uniquement pris en charge pour les clients exécutant Windows 10&reg;, <br />              Windows&reg; 8 et Windows&reg; 8,1 Enterprise.<br /><br />-Une infrastructure à clé publique n’est pas obligatoire.<br /><br />-Non pris en charge pour le déploiement de l’authentification à deux facteurs. Les informations d’identification de domaine sont requises pour l’authentification.<br /><br />-Déploie automatiquement DirectAccess sur tous les ordinateurs portables du domaine actuel.<br /><br />-Le trafic vers Internet ne passe pas par DirectAccess. La configuration de tunneling forcé n’est pas prise en charge.<br /><br />-Le serveur DirectAccess est le serveur emplacement réseau.<br /><br />-La protection d’accès réseau (NAP) n’est pas prise en charge.<br /><br />-La modification de stratégies à l’aide d’une fonctionnalité autre que la console de gestion DirectAccess ou les applets de commande Windows PowerShell n’est pas prise en charge.<br /><br />-Pour une configuration multisite, maintenant ou à l’avenir, suivez d’abord les instructions de la [section déployer un serveur DirectAccess unique avec des paramètres avancés](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).|  
|[Déployer un serveur DirectAccess unique avec des paramètres avancés](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-Une infrastructure à clé publique doit être déployée.<br />    Pour plus d’informations, consultez [mini-module du Guide de laboratoire de test : infrastructure à clé publique de base pour Windows Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx).<br /><br />-Le pare-feu Windows doit être activé sur tous les profils.<br /><br />Les systèmes d’exploitation serveur suivants prennent en charge DirectAccess.<br /><br />-Vous pouvez déployer toutes les versions de Windows Server 2016 en tant que client DirectAccess ou serveur DirectAccess.<br />-Vous pouvez déployer toutes les versions de Windows Server 2012 R2 comme un client DirectAccess ou un serveur DirectAccess.<br />-Vous pouvez déployer toutes les versions de Windows Server 2012 en tant que client DirectAccess ou serveur DirectAccess.<br />-Vous pouvez déployer toutes les versions de Windows Server 2008 R2 comme un client DirectAccess ou un serveur DirectAccess.<br /><br />Les systèmes d’exploitation clients suivants prennent en charge DirectAccess.<br /><br />-Windows 10&reg; entreprise<br />-Windows 10&reg; Enterprise 2015 Long Term Servicing Branch (LTSB)<br />-Windows&reg; 8 et 8,1 entreprise<br />-Windows&reg; 7 édition intégrale<br />-Windows&reg; 7 entreprise<br /><br />-La configuration du tunnel forcé n’est pas prise en charge avec l’authentification KerbProxy.<br /><br />-La modification de stratégies à l’aide d’une fonctionnalité autre que la console de gestion DirectAccess ou les applets de commande Windows PowerShell n’est pas prise en charge.<br /><br />La séparation des rôles de serveur NAT64/DNS64 et IPHTTPS sur un autre serveur n’est pas prise en charge.|  
  


