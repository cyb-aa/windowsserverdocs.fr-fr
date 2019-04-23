---
title: Déploiement de certificats de serveur
description: Cette rubrique fait partie du guide des certificats de serveur de déploiement pour les déploiements de sans fil et câblé à 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 751c5c5958b3d06ae0f4b701e4d6e10a7fef19dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858490"
---
# <a name="server-certificate-deployment"></a>Déploiement de certificats de serveur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Suivez ces étapes pour installer une autorité de certification racine entreprise (CA) et déployer des certificats de serveur à utiliser avec PEAP et EAP.  
  
> [!IMPORTANT]  
> Avant d’installer des Services de certificats Active Directory, vous devez nommer l’ordinateur, configurez l’ordinateur avec une adresse IP statique et joindre l’ordinateur au domaine. Après avoir installé les services AD CS, vous ne pouvez pas modifier le nom d’ordinateur ou l’appartenance au domaine de l’ordinateur, mais vous pouvez modifier l’adresse IP si nécessaire. Pour plus d’informations sur la façon d’accomplir ces tâches, consultez le Windows Server&reg; 2016 [Guide du réseau](../../Core-Network-Guide.md).  

  
-   [Installer le WEB1 de serveur Web](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [Créer un enregistrement d’alias (CNAME) dans DNS pour WEB1](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [Configurer WEB1 pour distribuer des listes de révocation de certificats (CRL)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [Préparer le fichier inf de Décla_ac](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [Installer l’autorité de Certification](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [Configurer les extensions CDP et AIA sur CA1](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [Copiez le certificat d’autorité de certification et la révocation de certificats dans le répertoire virtuel](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [Configurer le modèle de certificat de serveur](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [Configurer l’inscription automatique du certificat de serveur](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [Stratégie de groupe de l’actualisation](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [Vérifier l’inscription de serveur d’un certificat de serveur](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> Les procédures décrites dans ce guide n'incluent aucune instruction dans le cas où la boîte de dialogue **Contrôle de compte d'utilisateur** s'ouvre pour demander votre autorisation de continuer. Si cette boîte de dialogue s’ouvre pendant que vous réalisez les procédures de ce guide, et si elle s’ouvre en réponse à vos actions, cliquez sur **Continuer**.  
  


