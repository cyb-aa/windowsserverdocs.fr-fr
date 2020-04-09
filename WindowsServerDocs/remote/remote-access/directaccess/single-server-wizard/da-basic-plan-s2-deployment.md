---
title: Étape 2 planifier le déploiement de base de DirectAccess
description: Cette rubrique fait partie du guide déployer un serveur DirectAccess unique à l’aide de l’Assistant Prise en main pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 7ddcb162-dd92-406c-acab-d3de7239c644
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 546d722ed5e69f8c157e67c5eee728f5f5156abb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819332"
---
# <a name="step-2-plan-the-basic-directaccess-deployment"></a>Étape 2 planifier le déploiement de base de DirectAccess

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après la planification de l’infrastructure DirectAccess, l’étape suivante du déploiement de DirectAccess sur un serveur unique avec les paramètres de base consiste à planifier les paramètres de l’Assistant Prise en main.  
  
|Tâche|Description|  
|----|--------|  
|Planification du déploiement de clients|Par défaut, l’Assistant Prise en main déploie DirectAccess sur tous les ordinateurs portables et les ordinateurs portables du domaine en appliquant un filtre WMI à l’objet de stratégie de groupe des paramètres client.|  
|Planification du déploiement du serveur DirectAccess|Planifiez la façon de déployer le serveur DirectAccess.|  
  
## <a name="planning-for-client-deployment"></a><a name="bkmk_2_1_client"></a>Planification du déploiement du client  
Il y a deux décisions à prendre lors de la planification du déploiement de vos clients :  
  
1.  DirectAccess sera-t-il disponible uniquement pour les ordinateurs portables ou pour n'importe quel ordinateur ?  
  
    Quand vous configurez des clients DirectAccess dans l’Assistant Prise en main, vous pouvez choisir d’autoriser uniquement les ordinateurs portables des groupes de sécurité spécifiés à se connecter à l’aide de DirectAccess. Si vous restreignez l’accès aux ordinateurs portables, DirectAccess configure automatiquement un filtre WMI pour s’assurer que l’objet de stratégie de groupe du client DirectAccess est appliqué uniquement aux ordinateurs portables des groupes de sécurité spécifiés. L’administrateur DirectAccess requiert des autorisations pour créer ou modifier des filtres WMI de stratégie de groupe pour activer ce paramètre.  
  
2.  Quels groupes de sécurité contiendront les ordinateurs clients DirectAccess ?  
  
    Les paramètres DirectAccess sont contenus dans l'objet de stratégie de groupe Client DirectAccess. L’objet de stratégie de groupe est appliqué aux ordinateurs qui font partie des groupes de sécurité que vous spécifiez dans l’Assistant Prise en main. Vous pouvez spécifier des groupes de sécurité contenus dans tout domaine pris en charge. Avant de configurer DirectAccess, les groupes de sécurité doivent être créés. Vous pouvez ajouter des ordinateurs au groupe de sécurité après avoir effectué le déploiement de DirectAccess, mais notez que si vous ajoutez des ordinateurs clients qui résident dans un domaine différent du groupe de sécurité, l’objet de stratégie de groupe client ne sera pas appliqué à ces clients. Par exemple, si vous avez créé le groupe de sécurité GS1 dans un domaine A pour les clients DirectAccess, puis que vous ajoutez ensuite des clients d'un domaine B à ce groupe, l'objet de stratégie de groupe client ne sera pas appliqué aux clients du domaine B. Pour éviter ce problème, créez un nouveau groupe de sécurité du client pour chaque domaine contenant des ordinateurs clients. Ou, si vous ne voulez pas créer un nouveau groupe de sécurité, exécutez l'applet de commande Add-DAClient avec le nom du nouvel objet de stratégie de groupe pour le nouveau domaine.  
  
## <a name="planning-for-directaccess-server-deployment"></a><a name="bkmk_2_2_server"></a>Planification du déploiement du serveur DirectAccess  
Il existe un certain nombre de décisions à prendre lors de la planification du déploiement de votre serveur DirectAccess :  
  
-   **Topologie de réseau** : deux topologies sont disponibles lors du déploiement d’un serveur DirectAccess :  
  
    -   **Deux adaptateurs** : avec deux cartes réseau, DirectAccess peut être configuré avec une seule carte réseau connectée directement à Internet, tandis que l’autre est connectée au réseau interne. Le serveur peut aussi être installé derrière un périphérique de périmètre tel qu'un pare-feu ou un routeur. Dans cette configuration, une carte réseau est connectée au réseau de périmètre et l'autre est connectée au réseau interne.  
  
    -   **Carte réseau unique** : dans cette configuration, le serveur DirectAccess est installé derrière un périphérique de périmètre tel qu’un pare-feu ou un routeur. La carte réseau est connectée au réseau interne.  
  
-   **Cartes réseau** : l’Assistant DirectAccess détecte automatiquement les cartes réseau configurées sur le serveur DirectAccess. Vous pouvez vous assurer que les adaptateurs corrects sont sélectionnés dans la page **vérifier** .  
  
-   **Certificat IP-HTTPS** : dans la mesure où aucune PKI n’est requise dans ce déploiement, l’Assistant configure automatiquement les certificats auto-signés pour IP-HTTPS et le serveur emplacement réseau (si aucun certificat n’est présent) et active automatiquement le proxy Kerberos. L’assistant active également NAT64 et DNS64 pour la traduction de protocole dans l’environnement IPv4 uniquement. Une fois que l’Assistant a fini d’appliquer la configuration, cliquez sur **Fermer**.  
  
-   **Clients Windows 7** : vous ne pouvez pas activer la prise en charge des clients Windows 7 à partir de l’Assistant prise en main. Vous pouvez l’activer à partir de l’Assistant Installation avancée. Pour plus d’informations, consultez [déployer un serveur DirectAccess unique avec des paramètres avancés](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
-   **Configuration VPN** : avant de configurer DirectAccess, déterminez si vous allez fournir un accès VPN aux clients distants. Vous devez fournir un accès VPN si vous avez des ordinateurs clients de votre organisation qui ne prennent pas en charge la connectivité DirectAccess (parce qu’ils ne sont pas gérés ou qu’ils exécutent un système d’exploitation pour lequel DirectAccess n’est pas pris en charge). L’Assistant Prise en main configure l’attribution d’adresses IP VPN à l’aide de DHCP et configure les clients VPN à authentifier à l’aide de Active Directory.  
  
-   **Forcer le tunneling** : Si vous envisagez d’utiliser le tunneling forcé, ou si vous pouvez l’ajouter ultérieurement, vous devez utiliser [déployer un serveur DirectAccess unique avec des paramètres avancés](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) pour déployer une configuration à deux tunnels. En raison de considérations de sécurité, le tunneling forcé dans une configuration de tunnel unique n’est pas pris en charge.  
  
## <a name="previous-step"></a><a name="BKMK_Links"></a>Étape précédente  
  
-   [Étape 1 : planifier l’infrastructure DirectAccess de base](da-basic-plan-s1-infrastructure.md)  
  


