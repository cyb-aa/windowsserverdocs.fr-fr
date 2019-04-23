---
title: Installer le serveur Web WEB1
description: Cette rubrique fait partie du guide des certificats de serveur de déploiement pour les déploiements de sans fil et câblé à 802.1 X
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ef4f10a6ac1998850758f2c9db86bfd950c1ad70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833280"
---
# <a name="install-the-web-server-web1"></a>Installer le serveur Web WEB1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le rôle de serveur Web (IIS) dans Windows Server 2016 fournit une plate-forme sécurisée, facile à gérer, modulaire et extensible pour héberger des sites Web, services et applications de manière fiable. Avec IIS, vous pouvez partager des informations avec des utilisateurs sur Internet, un intranet ou un extranet. IIS est une plateforme web unifiée qui intègre IIS, ASP.NET, les services FTP, PHP et Windows Communication Foundation (WCF).  

Lorsque vous déployez des certificats de serveur, votre serveur Web vous offre un emplacement où vous pouvez publier la liste de révocation de certificats (CRL) pour votre autorité de certification (CA). Après la publication, la liste de révocation est accessible à tous les ordinateurs sur votre réseau afin qu’ils peuvent utiliser cette liste pendant le processus d’authentification pour vérifier que les certificats présentés par d’autres ordinateurs ne sont pas révoqués.   

Si un certificat est sur la révocation de certificats comme révoqué, l’effort de l’authentification échoue et que votre ordinateur est protégé de l’approbation d’une entité qui possède un certificat qui n’est plus valid.  

Avant d’installer le rôle de serveur Web (IIS), vérifiez que vous avez configuré le nom du serveur et l’adresse IP et que vous avez joint l’ordinateur au domaine.  

## <a name="to-install-the-web-server-iis-server-role"></a>Pour installer le rôle serveur Serveur Web (IIS)  
Pour réaliser cette procédure, vous devez être membre du groupe **Administrateurs**.  

>[!NOTE]  
>Pour effectuer cette procédure à l’aide de Windows PowerShell, ouvrez PowerShell, tapez la commande suivante, puis appuyez sur ENTRÉE.  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.  
2.  Dans **Avant de commencer**, cliquez sur **Suivant**.  

**Remarque**   
Le **avant de commencer** page d’ajout de rôles et fonctionnalités Assistant s’affiche pas si vous avez précédemment exécuté l’ajout de rôles et fonctionnalités Assistant et que vous avez sélectionné **ignorer cette page par défaut** à ce moment-là.  

3.  Dans la page **Type d’installation** , cliquez sur **Suivant**.  
4.  Sur le **sélection du serveur** , cliquez sur **suivant**.  
5.  Sur le **rôles serveur** page, sélectionnez **serveur Web (IIS)**, puis cliquez sur **suivant**.  
6.  Cliquez sur **Suivant** plusieurs fois afin d’accepter tous les paramètres par défaut du serveur Web, puis cliquez sur **Installer**.  
7.  Vérifiez que toutes les installations ont réussi, puis cliquez sur **Fermer**.
