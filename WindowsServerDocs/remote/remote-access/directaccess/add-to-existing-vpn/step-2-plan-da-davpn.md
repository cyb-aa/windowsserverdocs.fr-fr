---
title: Étape 2 planifier le déploiement de DirectAccess
description: Cette rubrique fait partie du guide ajouter DirectAccess à un déploiement d’accès à distance (VPN) existant pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 72b5b2af-6925-41e0-a3f9-b8809ed711d1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 98fb738f3535845a2117f2f6547856b9081bd7d4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859542"
---
# <a name="step-2-plan-the-directaccess-deployment"></a>Étape 2 planifier le déploiement de DirectAccess

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Une fois l'infrastructure d'accès à distance planifiée, l'étape suivante de l'activation de DirectAccess consiste à planifier les paramètres de l'Assistant Activation de DirectAccess.  
  
|Tâche|Description|  
|----|--------|  
|Planification du déploiement de clients|Planifiez la façon dont les ordinateurs clients pourront se connecter à l'aide de DirectAccess. Identifiez les ordinateurs gérés qui seront configurés en tant que clients DirectAccess.|  
|Planification du déploiement du serveur d'accès à distance|Planifiez le déploiement du serveur d'accès à distance.|  
  
## <a name="planning-for-client-deployment"></a><a name="bkmk_2_1_client"></a>Planification du déploiement du client  
Il y a deux décisions à prendre lors de la planification du déploiement de vos clients :  
  
-   DirectAccess sera-t-il disponible uniquement pour les ordinateurs portables ou pour n'importe quel ordinateur ?  
  
    Quand vous configurez des clients DirectAccess dans l'Assistant Activation de DirectAccess, vous pouvez choisir d'autoriser uniquement les ordinateurs portables des groupes de sécurité spécifiés à se connecter à l'aide de DirectAccess. Si vous voulez restreindre l'accès aux ordinateurs portables, le service Accès à distance configure automatiquement un filtre WMI pour garantir que l'objet de stratégie de groupe Client DirectAccess est appliqué uniquement aux ordinateurs portables des groupes de sécurité spécifiés. Pour activer ce paramètre, l'administrateur de l'accès à distance doit disposer d'autorisations de création ou de modification des filtres WMI de stratégie de groupe.  
  
-   Quels groupes de sécurité contiendront les ordinateurs clients DirectAccess ?  
  
    Les paramètres DirectAccess sont contenus dans l'objet de stratégie de groupe Client DirectAccess. L'objet de stratégie de groupe est appliqué aux ordinateurs faisant partie des groupes de sécurité que vous spécifiez dans l'Assistant Activation de DirectAccess. Vous pouvez spécifier des groupes de sécurité contenus dans tout domaine pris en charge. Les groupes de sécurité doivent être créés avant de configurer l'accès à distance. Vous pouvez ajouter des ordinateurs au groupe de sécurité une fois le déploiement de l'accès à distance terminé. Notez toutefois que si vous ajoutez des ordinateurs clients qui résident dans un autre domaine au groupe de sécurité, l'objet de stratégie de groupe client ne sera pas appliqué à ces clients. Par exemple, si vous avez créé le groupe de sécurité GS1 dans un domaine A pour les clients DirectAccess, puis que vous ajoutez ensuite des clients d'un domaine B à ce groupe, l'objet de stratégie de groupe client ne sera pas appliqué aux clients du domaine B. Pour éviter ce problème, créez un nouveau groupe de sécurité du client pour chaque domaine contenant des ordinateurs clients. Ou, si vous ne voulez pas créer un nouveau groupe de sécurité, exécutez l'applet de commande Add-DAClient avec le nom du nouvel objet de stratégie de groupe pour le nouveau domaine.  
  
## <a name="planning-for-remote-access-server-deployment"></a><a name="bkmk_2_2_server"></a>Planification du déploiement d’un serveur d’accès à distance  
Il y a un certain nombre de décisions à prendre lors de la planification du déploiement de votre serveur d'accès à distance :  
  
-   **Topologie de réseau**: deux topologies sont disponibles lors du déploiement d’un serveur d’accès à distance :  
  
    -   **Deux adaptateurs**: avec deux cartes réseau, l’accès à distance peut être configuré avec une seule carte réseau connectée directement à Internet, tandis que l’autre est connectée au réseau interne. Le serveur peut aussi être installé derrière un périphérique de périmètre tel qu'un pare-feu ou un routeur. Dans cette configuration, une carte réseau est connectée au réseau de périmètre et l'autre est connectée au réseau interne.  
  
    -   **Carte réseau unique**: dans cette configuration, le serveur d’accès à distance est installé derrière un périphérique de périmètre tel qu’un pare-feu ou un routeur. La carte réseau est connectée au réseau interne.  
  
-   **Cartes réseau**: l’Assistant Activation de DirectAccess détecte automatiquement les cartes réseau configurées sur le serveur d’accès à distance, en fonction des interfaces utilisées par le VPN. Vous devez vous assurer que les cartes appropriées sont sélectionnées.  
  
-   **Adresse ConnectTo**-les ordinateurs clients utilisent l’adresse ConnectTo pour se connecter au serveur d’accès à distance. L'adresse que vous choisissez doit correspondre au nom de sujet du certificat IP-HTTPS que vous déployez pour la connexion IP-HTTPS et doit être disponible dans le DNS public. Consultez la rubrique relative à la planification de certificats de site web pour IP-HTTPS.  
  
-   **Certificat IP-HTTPS**: si le VPN SSTP est configuré, l’Assistant Activation de DirectAccess récupère le certificat utilisé par SSTP pour IP-HTTPS. Si le VPN SSTP n'est pas configuré, l'Assistant vérifie si un certificat a été configuré pour IP-HTTPS. Si ce n'est pas le cas, il configure automatiquement des certificats auto-signés pour IP-HTTPS. L'Assistant active automatiquement l'authentification Kerberos. L'Assistant active également NAT64 et DNS64 pour la traduction de protocole dans l'environnement IPv4 uniquement.  
  
-   **Préfixes IPv6**: si l’Assistant détecte que le protocole IPv6 a été déployé sur les cartes réseau, il crée automatiquement des préfixes IPv6 pour le réseau interne, un préfixe IPv6 à attribuer aux ordinateurs clients DirectAccess et un préfixe IPv6 à affecter aux ordinateurs clients VPN. Si les préfixes générés automatiquement ne sont pas corrects pour votre infrastructure IPv6 ou ISATAP native, vous devez les modifier manuellement. Consultez 1.1 Planifier la topologie et les paramètres réseau et serveur.  
  
-   **Clients Windows 7**: par défaut, les ordinateurs clients Windows 7 ne peuvent pas se connecter à un déploiement de l’accès à distance windows server 2012. Si vous avez des ordinateurs clients Windows 7 dans votre organisation qui nécessitent un accès à distance aux ressources internes, vous pouvez les autoriser à se connecter. Tout ordinateur client que vous voulez autoriser à accéder aux ressources internes doit être membre d'un groupe de sécurité que vous spécifiez dans l'Assistant Activation de DirectAccess.  
  
    > [!NOTE]
    > Pour autoriser les ordinateurs clients Windows 7 à se connecter à l’aide de DirectAccess, vous devez utiliser l’authentification par certificat d’ordinateur.
  
-   **Authentification**-l’Assistant Activation de DirectAccess utilise Active Directory pour authentifier les informations d’identification de l’utilisateur. Pour déployer l'authentification à deux facteurs, voir [Déployer l'accès à distance avec l'authentification par mot de passe à usage unique](../../ras/otp/Deploy-RA-OTP.md).  
  

  


