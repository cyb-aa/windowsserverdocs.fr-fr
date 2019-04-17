---
title: Déploiement de certificats de serveur
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4a10a9bafa6a8c9fddecac799ec8e837bf339d0e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment"></a>Déploiement de certificats de serveur

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Suivez ces étapes pour installer une autorité de certification racine entreprise (CA) et déployer des certificats de serveur à utiliser avec PEAP et EAP.  
  
> [!IMPORTANT]  
> Avant d’installer les Services de certificats ActiveDirectory, vous devez nommer l’ordinateur, configurez l’ordinateur avec une adresse IP statique et joindre l’ordinateur au domaine. Après avoir installé les services ADCS, vous ne pouvez pas modifier le nom d’ordinateur ou l’appartenance au domaine de l’ordinateur, mais vous pouvez modifier l’adresse IP si nécessaire. Pour plus d’informations sur comment effectuer ces tâches, consultez Windows Server&reg; 2016 [Guide réseau de base](../../Core-Network-Guide.md).  

  
-   [Installer le WEB1 de serveur Web](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [Créer un enregistrement d’alias (CNAME) dans DNS pour WEB1](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [Configurer WEB1 pour distribuer des listes de révocation de certificats (CRL)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [Préparer le fichier inf Décla_ac](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [Installer l’autorité de Certification](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [Configurer les extensions CDP et AIA sur CA1](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [Copiez le certificat d’autorité de certification et la liste CRL dans le répertoire virtuel](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [Configurer le modèle de certificat de serveur](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [Configurer l’inscription automatique du certificat de serveur](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [Stratégie de groupe d’actualisation](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [Vérifiez le serveur d’inscription d’un certificat de serveur](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> Les procédures décrites dans ce guide n’incluent pas d’instructions pour les cas dans lesquels le **contrôle de compte d’utilisateur** boîte de dialogue s’ouvre pour demander votre autorisation pour continuer. Si cette boîte de dialogue s’ouvre pendant que vous effectuez les procédures décrites dans ce guide et si la boîte de dialogue a été ouverte en réponse à vos actions, cliquez sur **continuer**.  
  


