---
title: Installer le serveur Web WEB1
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8871b2b82b30bca5b4efd62a31b52e8dbd7284a9
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318285"
---
# <a name="install-the-web-server-web1"></a>Installer le serveur Web WEB1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le rôle de serveur Web (IIS) dans Windows Server 2016 fournit une plateforme sécurisée, facile à gérer, modulaire et extensible pour héberger de manière fiable des sites Web, des services et des applications. Avec IIS, vous pouvez partager des informations avec des utilisateurs sur Internet, un intranet ou un extranet. IIS est une plateforme Web unifiée qui intègre les services IIS, ASP.NET, FTP, PHP et Windows Communication Foundation (WCF).  

Lorsque vous déployez des certificats de serveur, votre serveur Web vous fournit un emplacement où vous pouvez publier la liste de révocation de certificats (CRL) pour votre autorité de certification. Après la publication, la liste de révocation de certificats est accessible à tous les ordinateurs de votre réseau afin qu’ils puissent utiliser cette liste pendant le processus d’authentification pour vérifier que les certificats présentés par d’autres ordinateurs ne sont pas révoqués.   

Si un certificat se trouve sur la liste de révocation de certificats comme étant révoqué, l’effort d’authentification échoue et votre ordinateur est protégé de l’approbation d’une entité dont le certificat n’est plus valide.  

Avant d’installer le rôle serveur Web (IIS), assurez-vous que vous avez configuré le nom du serveur et l’adresse IP et que vous avez joint l’ordinateur au domaine.  

## <a name="to-install-the-web-server-iis-server-role"></a>Pour installer le rôle serveur Serveur Web (IIS)  
Pour réaliser cette procédure, vous devez être membre du groupe **Administrateurs**.  

>[!NOTE]  
>Pour effectuer cette procédure à l’aide de Windows PowerShell, ouvrez PowerShell, tapez la commande suivante, puis appuyez sur entrée.  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.  
2.  Dans **Avant de commencer**, cliquez sur **Suivant**.  

**Remarque**   
La page **avant de commencer** de l’Assistant Ajout de rôles et de fonctionnalités ne s’affiche pas si vous avez déjà exécuté l’Assistant Ajout de rôles et de fonctionnalités et que vous avez sélectionné **ignorer cette page par défaut** à ce moment-là.  

3. Dans la page **Type d’installation**, cliquez sur **Suivant**.  
4. Sur la page **sélection du serveur** , cliquez sur **suivant**.  
5. Sur la page **rôles du serveur** , sélectionnez **serveur Web (IIS)** , puis cliquez sur **suivant**.  
6. Cliquez sur **Suivant** plusieurs fois afin d’accepter tous les paramètres par défaut du serveur Web, puis cliquez sur **Installer**.  
7. Vérifiez que toutes les installations ont réussi, puis cliquez sur **Fermer**.
