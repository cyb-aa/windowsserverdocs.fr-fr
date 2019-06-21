---
title: Conditions préalables pour le déploiement de DirectAccess
description: Cette rubrique fournit des conditions préalables pour le déploiement de DirectAccess dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f77df438ba2b282101a031b4ff4d145286cf3e94
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283623"
---
# <a name="prerequisites-for-deploying-directaccess"></a>Conditions préalables pour le déploiement de DirectAccess

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le tableau suivant répertorie les conditions préalables nécessaires à l’aide des Assistants configuration pour déployer DirectAccess.  
  
|||  
|-|-|  
|Scénario|Prérequis|  
|[Déployer un serveur DirectAccess unique à l’aide de l’Assistant Mise en route](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-Windows pare-feu doit être activé sur tous les profils<br /><br />-Uniquement pris en charge pour les clients qui exécutent Windows 10&reg;, <br />              Windows&reg; 8 et Windows&reg; 8.1 entreprise.<br /><br />-Une infrastructure à clé publique n’est pas nécessaire.<br /><br />-Non pris en charge pour le déploiement de l’authentification à deux facteurs. Les informations d’identification de domaine sont requises pour l’authentification.<br /><br />-Déploie automatiquement DirectAccess sur tous les ordinateurs portables dans le domaine actuel.<br /><br />-Le trafic vers Internet ne traverse pas DirectAccess. La configuration de tunneling forcé n’est pas prise en charge.<br /><br />-DirectAccess server est le serveur emplacement réseau.<br /><br />-Network Access Protection (NAP) n’est pas pris en charge.<br /><br />-La modification des stratégies à l’aide d’une fonctionnalité autre que la console de gestion DirectAccess ou les applets de commande Windows PowerShell n’est pas pris en charge.<br /><br />-Pour une configuration multisite, maintenant ou à l’avenir, suivez tout d’abord les instructions de [déployer un serveur DirectAccess unique avec des paramètres avancés](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).|  
|[Déployer un serveur DirectAccess unique avec des paramètres avancés](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-Une infrastructure à clé publique doit être déployée.<br />    Pour plus d’informations, consultez [mini-module de Test Lab Guide : Base PKI pour Windows Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx).<br /><br />-Windows pare-feu doit être activé sur tous les profils.<br /><br />Les systèmes d’exploitation de serveur suivants prennent en charge DirectAccess.<br /><br />-Vous pouvez déployer toutes les versions de Windows Server 2016 comme un client DirectAccess ou un serveur DirectAccess.<br />-Vous pouvez déployer toutes les versions de Windows Server 2012 R2 en tant qu’un client DirectAccess ou un serveur DirectAccess.<br />-Vous pouvez déployer toutes les versions de Windows Server 2012 en tant qu’un client DirectAccess ou un serveur DirectAccess.<br />-Vous pouvez déployer toutes les versions de Windows Server 2008 R2 en tant qu’un client DirectAccess ou un serveur DirectAccess.<br /><br />Les systèmes d’exploitation de client suivants prennent en charge DirectAccess.<br /><br />-Windows 10&reg; Enterprise<br />-Windows 10&reg; Enterprise 2015 Long Term Servicing Branch dans (LTSB)<br />-Windows&reg; 8 et 8.1 Enterprise<br />-Windows&reg; 7 Édition intégrale<br />-Windows&reg; 7 entreprise<br /><br />-Configuration de tunnel force n’est pas prise en charge avec l’authentification KerbProxy.<br /><br />-La modification des stratégies à l’aide d’une fonctionnalité autre que la console de gestion DirectAccess ou les applets de commande Windows PowerShell n’est pas pris en charge.<br /><br />-Séparation NAT64/DNS64 et les rôles de serveur IP-HTTPS sur un autre serveur n’est pas pris en charge.|  
  


