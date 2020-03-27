---
title: Déploiement de certificats de serveur
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 63ae9ef71b913feeeb28e9838f636b316ea2969f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318200"
---
# <a name="server-certificate-deployment"></a>Déploiement de certificats de serveur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Procédez comme suit pour installer une autorité de certification racine d’entreprise et déployer des certificats de serveur à utiliser avec PEAP et EAP.  
  
> [!IMPORTANT]  
> Avant d’installer les services de certificats Active Directory, vous devez nommer l’ordinateur, configurer l’ordinateur avec une adresse IP statique et joindre l’ordinateur au domaine. Après avoir installé les services AD CS, vous ne pouvez pas modifier le nom de l’ordinateur ou l’appartenance au domaine de l’ordinateur, mais vous pouvez modifier l’adresse IP si nécessaire. Pour plus d’informations sur la façon d’accomplir ces tâches, consultez le Guide du [réseau de base](../../Core-Network-Guide.md)de Windows Server&reg; 2016.  

  
-   [Installer le serveur Web WEB1](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [Créer un enregistrement d’alias (CNAME) dans DNS pour WEB1](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [Configurer WEB1 pour distribuer des listes de révocation de certificats (CRL)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [Préparer le fichier INF de la stratégie](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [Installer l’autorité de certification](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [Configurer les extensions CDP et AIA sur CA1](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [Copier le certificat d’autorité de certification et la liste CRL dans le répertoire virtuel](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [Configurer le modèle de certificat de serveur](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [Configurer l’inscription automatique du certificat de serveur](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [Actualiser stratégie de groupe](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [Vérifier l’inscription du serveur d’un certificat de serveur](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> Les procédures décrites dans ce guide n'incluent aucune instruction dans le cas où la boîte de dialogue **Contrôle de compte d'utilisateur** s'ouvre pour demander votre autorisation de continuer. Si cette boîte de dialogue s’ouvre pendant que vous réalisez les procédures de ce guide, et si elle s’ouvre en réponse à vos actions, cliquez sur **Continuer**.  
  


