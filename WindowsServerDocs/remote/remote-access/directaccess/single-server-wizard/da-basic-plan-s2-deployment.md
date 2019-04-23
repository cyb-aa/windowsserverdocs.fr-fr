---
title: Étape 2 planifier le déploiement de DirectAccess de base
description: Cette rubrique fait partie du guide de déployer un serveur DirectAccess unique à l’aide de la prise en main Assistant pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ddcb162-dd92-406c-acab-d3de7239c644
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8c21b7fa62246170caeb07cb5865c1ff311e0f09
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848750"
---
# <a name="step-2-plan-the-basic-directaccess-deployment"></a>Étape 2 planifier le déploiement de DirectAccess de base

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après la planification de l’infrastructure DirectAccess, l’étape suivante dans le déploiement de DirectAccess sur un serveur unique avec les paramètres de base consiste à planifier les paramètres de l’Assistant Mise en route.  
  
|Tâche|Description|  
|----|--------|  
|Planification du déploiement de clients|Par défaut, l’Assistant Mise en route déploie DirectAccess sur tous les ordinateurs portables et ordinateurs portables dans le domaine en appliquant un filtre WMI à la stratégie de groupe de paramètres client|  
|Planification du déploiement du serveur DirectAccess|Planifiez la façon de déployer le serveur DirectAccess.|  
  
## <a name="bkmk_2_1_client"></a>Planification du déploiement du client  
Il y a deux décisions à prendre lors de la planification du déploiement de vos clients :  
  
1.  DirectAccess sera-t-il disponible uniquement pour les ordinateurs portables ou pour n'importe quel ordinateur ?  
  
    Lorsque vous configurez les clients DirectAccess dans l’Assistant Mise en route, vous pouvez choisir d’autoriser des ordinateurs portables uniquement dans les groupes de sécurité spécifié pour se connecter à l’aide de DirectAccess. Si vous limitez l’accès sur les ordinateurs portables, DirectAccess configure automatiquement un filtre WMI pour vous assurer que le client DirectAccess qu'est appliqué uniquement aux ordinateurs portables des groupes de sécurité spécifiés. L’administrateur DirectAccess requiert des autorisations pour créer ou modifier des filtres WMI de stratégie de groupe pour activer ce paramètre.  
  
2.  Quels groupes de sécurité contiendront les ordinateurs clients DirectAccess ?  
  
    Les paramètres DirectAccess sont contenus dans l'objet de stratégie de groupe Client DirectAccess. L’objet de stratégie de groupe est appliqué aux ordinateurs qui font partie des groupes de sécurité que vous spécifiez dans l’Assistant Mise en route. Vous pouvez spécifier des groupes de sécurité contenus dans tout domaine pris en charge. Avant de configurer DirectAccess, les groupes de sécurité doivent être créées. Vous pouvez ajouter des ordinateurs au groupe de sécurité après avoir effectué le déploiement de DirectAccess, mais notez que si vous ajoutez des ordinateurs clients qui résident dans un autre domaine au groupe de sécurité, le client GPO ne sera pas appliqué à ces clients. Par exemple, si vous avez créé le groupe de sécurité GS1 dans un domaine A pour les clients DirectAccess, puis que vous ajoutez ensuite des clients d'un domaine B à ce groupe, l'objet de stratégie de groupe client ne sera pas appliqué aux clients du domaine B. Pour éviter ce problème, créez un nouveau groupe de sécurité du client pour chaque domaine contenant des ordinateurs clients. Ou, si vous ne voulez pas créer un nouveau groupe de sécurité, exécutez l'applet de commande Add-DAClient avec le nom du nouvel objet de stratégie de groupe pour le nouveau domaine.  
  
## <a name="bkmk_2_2_server"></a>Planification du déploiement du serveur DirectAccess  
Il existe un nombre de décisions à prendre lors de la planification du déploiement de votre serveur DirectAccess :  
  
-   **Topologie du réseau** -deux topologies sont disponibles lorsque vous déployez un serveur DirectAccess :  
  
    -   **Deux cartes** -avec deux cartes réseau, DirectAccess peut être configurés avec une carte réseau connectée directement à Internet, et l’autre est connectée au réseau interne. Le serveur peut aussi être installé derrière un périphérique de périmètre tel qu'un pare-feu ou un routeur. Dans cette configuration, une carte réseau est connectée au réseau de périmètre et l'autre est connectée au réseau interne.  
  
    -   **Carte réseau unique** -dans cette configuration DirectAccess server est installé derrière un périphérique de périmètre tel qu’un pare-feu ou un routeur. La carte réseau est connectée au réseau interne.  
  
-   **Cartes réseau** -l’Assistant DirectAccess détecte automatiquement les cartes réseau configurées sur le serveur DirectAccess. Vous pouvez vous assurer que les cartes appropriées sont sélectionnées à partir de la **révision** page.  
  
-   **Certificat IP-HTTPS** -dans la mesure où il n’existe aucune infrastructure à clé publique requis dans ce déploiement, l’Assistant configure automatiquement les certificats auto-signés pour IP-HTTPS et le serveur d’emplacement réseau (si aucun certificat n’est présent) et active automatiquement Proxy Kerberos. L’Assistant Active également NAT64 et DNS64 pour la traduction de protocole dans l’environnement IPv4 uniquement. Une fois que l’Assistant a fini d’appliquer la configuration, cliquez sur **Fermer**.  
  
-   **Les clients Windows 7** -vous ne pouvez pas activer la prise en charge pour les clients Windows 7 à partir de l’Assistant Mise en route. Cela peut être activée à partir de l’Assistant Installation avancée. Pour plus d’informations, consultez [déployer un serveur DirectAccess unique avec des paramètres avancés](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
-   **Configuration de VPN** -avant de configurer DirectAccess, décidez si vous vous apprêtez à fournir l’accès VPN à des clients distants. Vous devez fournir l’accès VPN si vous avez des ordinateurs clients dans votre organisation qui ne prennent pas en charge la connectivité DirectAccess (soit parce qu’elles ne sont pas gérés ou exécutent un système d’exploitation pour lequel DirectAccess n’est pas pris en charge). L’Assistant Mise en route configure l’attribution d’adresses IP du VPN à l’aide de DHCP et configure les clients VPN pour être authentifié à l’aide de Active Directory.  
  
-   **Forcer le Tunneling** -si vous envisagez d’utiliser le tunneling forcé ou que vous pouvez ajouter à l’avenir, vous devez utiliser [déployer un serveur DirectAccess unique avec des paramètres avancés](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) pour déployer une configuration à deux tunnels. Pour des raisons de sécurité, le tunneling forcé dans une configuration de tunnel unique n’est pas pris en charge.  
  
## <a name="BKMK_Links"></a>Étape précédente  
  
-   [Étape 1 : Planifier l’Infrastructure DirectAccess de base](da-basic-plan-s1-infrastructure.md)  
  


