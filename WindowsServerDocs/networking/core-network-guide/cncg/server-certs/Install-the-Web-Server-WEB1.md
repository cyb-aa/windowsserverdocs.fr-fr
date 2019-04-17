---
title: Installer le WEB1 de serveur Web
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e818eac49719b394a2c73cc125a2e7ba9ea80c82
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-web-server-web1"></a>Installer le WEB1 de serveur Web

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Le rôle de serveur Web (IIS) dans Windows Server2016 fournit une plateforme sécurisée, facile à gérer, modulaire et extensible héberger de manière fiable des sites Web, services et applications. Avec IIS, vous pouvez partager des informations avec des utilisateurs sur Internet, un intranet ou un extranet. IIS est une plateforme web unifiée qui intègre IIS, ASP.NET, services FTP, PHP et WindowsCommunicationFoundation (WCF).  

Lorsque vous déployez des certificats de serveur, votre serveur Web vous offre un emplacement où vous pouvez publier la liste de révocation de certificats (CRL) pour votre autorité de certification (CA). Après la publication, la liste de révocation est accessible à tous les ordinateurs sur votre réseau afin qu’ils peuvent utiliser cette liste pendant le processus d’authentification pour vérifier que les certificats présentés par d’autres ordinateurs ne sont pas révoqués.   

Si un certificat est sur la révocation de certificats comme révoqué, l’effort de l’authentification échoue et que votre ordinateur est protégé d’approuver une entité qui possède un certificat qui n’est plus valide.  

Avant d’installer le rôle de serveur Web (IIS), vérifiez que vous avez configuré le nom du serveur et l’adresse IP et que vous avez joint l’ordinateur au domaine.  

## <a name="to-install-the-web-server-iis-server-role"></a>Pour installer le rôle serveur serveur Web (IIS)  
Pour effectuer cette procédure, vous devez être un membre de la **administrateurs** groupe.  

>[!NOTE]  
>Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell, tapez la commande suivante et appuyez sur ENTRÉE.  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  Dans le Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**. L’Assistant Ajouter des rôles et fonctionnalités s’ouvre.  
2.  Dans **avant de commencer**, cliquez sur **suivant**.  

**Remarque**   
Le **avant de commencer** page de l’ajout de rôles et fonctionnalités Assistant n’apparaît pas si vous avez sélectionné et que vous avez déjà exécuté l’ajout de rôles et fonctionnalités Assistant **ignorer cette page par défaut** à ce moment-là.  

3.  Sur le **Type d’Installation**, cliquez sur **suivant**.  
4.  Sur le **sélection du serveur**, cliquez sur **suivant**.  
5.  Sur le **des rôles de serveurs** page, sélectionnez **serveur Web (IIS)**, puis cliquez sur **suivant**.  
6.  Cliquez sur **suivant** jusqu'à ce que vous avez accepté la valeur par défaut tous les paramètres du serveur web, puis cliquez sur **installer**.  
7.  Vérifiez que toutes les installations ont réussi, puis cliquez sur **fermer**.
