---
title: Étape 3 planifier un déploiement de Cluster à charge équilibrée
description: Cette rubrique fait partie du guide de déploiement des accès à distance dans un Cluster dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7540c17b-81de-47de-a04f-3247afa26f70
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 12485ddb9cbb70766c018e8f99caa91cfa6ee9da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861330"
---
# <a name="step-3-plan-a-load-balanced-cluster-deployment"></a>Étape 3 planifier un déploiement de Cluster à charge équilibrée

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

L’étape suivante consiste à planifier la configuration d’équilibrage de charge et le déploiement de cluster.  
  
|Tâche|Description|  
|----|--------|  
|3.1 planifier l’équilibrage de charge|Décider s’il faut utiliser Windows réseau équilibrage de charge (NLB) ou un équilibreur de charge externe (ELB).|  
|3.2 plan IP-HTTPS|Si un certificat auto-signé n’est pas utilisé, le serveur d’accès à distance a besoin d’un certificat SSL sur chaque serveur dans le cluster, afin d’authentifier les connexions IP-HTTPS.|  
|3.3 plan pour les connexions de client VPN|Notez la configuration requise pour les connexions de client VPN.|  
|3.4 planifier le serveur emplacement réseau|Si le site de serveur d’emplacement réseau est hébergé sur le serveur d’accès à distance, et un certificat auto-signé n’est pas utilisé, assurez-vous que chaque serveur dans le cluster possède un certificat de serveur pour authentifier la connexion au site Web.|  
  
## <a name="bkmk_2_1_Plan_LB"></a>3.1 planifier l’équilibrage de charge  
Accès à distance peut être déployé sur un serveur unique, ou sur un cluster de serveurs d’accès à distance. Le trafic vers le cluster peut être équilibrée pour fournir une haute disponibilité et évolutivité pour les clients DirectAccess. Il existe deux options d’équilibrage de charge :  
  
-   **Équilibrage de charge réseau Windows**-NLB de Windows est une fonctionnalité de serveur Windows. Pour l’utiliser, vous n’avez pas besoin matériel supplémentaire, car tous les serveurs du cluster sont chargés de gérer la charge du trafic. NLB de Windows prend en charge un maximum de huit serveurs dans un cluster de l’accès à distance.  
  
-   **Équilibreur de charge externe**-utilisation d’un équilibreur de charge externe nécessite un matériel externe pour gérer la charge du trafic entre les serveurs de cluster d’accès à distance. En outre, à l’aide d’un équilibreur de charge externe prend en charge un maximum de 32 serveurs d’accès à distance dans un cluster. Voici quelques points à prendre en compte lors de la configuration de l’équilibrage de charge externe sont :  
  
    -   L’administrateur doit s’assurer que les adresses IP virtuelles configurée via l’Assistant équilibrage de charge de l’accès à distance sont utilisés sur les équilibreurs de charge externe (par exemple, F5 Big-Ip Local Traffic Manager système). Lors de l’équilibrage de charge externe est activée, les adresses IP sur les interfaces internes et externes seront promus en adresses IP virtuelles et doivent être montées sur les équilibreurs de charge. Cela afin que l’administrateur n’a pas modifier l’entrée DNS pour le nom public du déploiement du cluster. En outre, les points de terminaison de tunnel IPsec sont dérivées du adresses IP de serveur. Si l’administrateur fournit les adresses IP virtuelles distinctes, le client ne sera pas en mesure de se connecter au serveur. Consultez l’exemple pour configurer DirectAccess avec équilibrage de charge externe dans 3.1.1 exemple de configuration d’équilibreur de charge externe.  
  
    -   Nombreux équilibreurs de charge externe (y compris F5) ne gèrent pas l’équilibrage de charge de 6to4 et ISATAP. Si le serveur d’accès à distance est un routeur ISATAP, la fonction ISATAP doit être déplacée vers un autre ordinateur. En outre, lors de la fonction ISATAP est sur un autre ordinateur, les serveurs DirectAccess doivent avoir une connectivité IPv6 native avec le routeur ISATAP. Notez que cette connectivité doit figurer avant de configurer DirectAccess.  
  
    -   Pour l’équilibrage de charge externe, si Teredo doit être utilisé, tous les serveurs d’accès à distance doivent avoir deux adresses IPv4 publiques consécutives que des adresses IP dédiées. Les adresses IP virtuelles du cluster doit également disposer de deux adresses IPv4 publiques consécutives. Cela n’est pas vrai pour NLB Windows où uniquement les adresses IP virtuelles du cluster doit avoir deux adresses IPv4 publiques consécutives. Si Teredo n’est pas utilisé, les deux adresses IP consécutives ne sont pas requis.  
  
    -   L’administrateur peut passer de Windows NLB pour l’équilibreur de charge externe et vice versa. Notez que l’administrateur ne peut pas basculer à partir de l’équilibreur de charge externe à NLB de Windows s’il a plus de 8 serveurs dans le déploiement d’équilibreur de charge externe.  
  
### <a name="ELBConfigEx"></a>3.1.1 exemple de configuration externe équilibreur de charge  
Cette section décrit les étapes de configuration pour l’activation d’un équilibreur de charge externe sur un nouveau déploiement de l’accès à distance. Lorsque vous utilisez un équilibreur de charge externe, le cluster de l’accès à distance peut ressembler à la figure ci-dessous, où les serveurs d’accès à distance sont connectés au réseau d’entreprise via un équilibreur de charge sur le réseau interne et à Internet via un équilibreur de charge connecté au réseau externe :  
  
![Exemple de configuration équilibreur de charge externe](../../../../media/Step-3-Plan-a-Load-Balanced-Cluster-Deployment/ELBDiagram-URA_Enterprise_NLB-.png)  
  
##### <a name="planning-information"></a>Informations sur la planification  
  
1.  Adresses IP virtuelles externes (adresses IP que le client utilisera pour se connecter à l’accès à distance) ont été décidés d’être 131.107.0.102, 131.107.0.103  
  
2.  Sur le réseau externe self-adresses IP - 131.107.0.245 (Internet), 131.107.1.245 l’équilibrage de charge  
  
    Il est le réseau de périmètre (également appelé zone démilitarisée et DMZ) entre l’équilibreur de charge sur le réseau externe et le serveur d’accès à distance.  
  
3.  Adresses IP pour le serveur d’accès à distance sur le réseau de périmètre - 131.107.1.102, 131.107.1.103  
  
4.  Des adresses IP pour le serveur d’accès à distance sur le réseau ELB (par exemple, entre le serveur d’accès à distance et l’équilibrage de charge sur le réseau interne) - 30.11.1.101, 2006:2005:11:1::101  
  
5.  Sur le réseau interne self-IPs - 30.11.1.245 l’équilibrage de charge 2006:2005:11:1::245 (ELB), 30.1.1.245 2006:2005:1:1::245 (Corpnet)  
  
6.  Adresses IP virtuelles internes (adresses IP utilisés pour la sonde de web client et serveur emplacement réseau, si installé sur les serveurs d’accès à distance) ont été décidés d’être 30.1.1.10, 2006:2005:1:1::10  
  
##### <a name="steps"></a>Étapes  
  
1.  Configurez la carte de réseau externe du serveur d’accès à distance (ce qui est connectée au réseau de périmètre) avec des adresses 131.107.0.102, 131.107.0.103. Cette étape est requise pour la configuration de DirectAccess détecter les points de terminaison de tunnel IPsec corrects.  
  
2.  Configurer l’adaptateur de réseau interne du serveur d’accès à distance (ce qui est connecté au réseau ELB) avec les adresses IP du serveur web-sonde/du réseau (30.1.1.10, 2006:2005:1:1::10). Cette étape est requise pour autoriser les clients à accéder à l’adresse IP de sonde web, pour l’assistant de connectivité réseau indique correctement l’état de connexion à DirectAccess. Cette étape autorise également l’accès au serveur d’emplacement réseau, s’il est configuré sur le serveur DirectAccess.  
  
    > [!NOTE]  
    > Assurez-vous que le contrôleur de domaine est accessible à partir du serveur d’accès à distance avec cette configuration.  
  
3.  Configurer le serveur unique de DirectAccess sur le serveur d’accès à distance.  
  
4.  Activer externe équilibrage de charge dans la configuration de DirectAccess. Utiliser 131.107.1.102 comme l’adresse IP dédiée (DIP) externe (131.107.1.103 seront sélectionnées automatiquement), utilisez 30.11.1.101, 2006:2005:11:1::101 en tant que les adresses IP dynamiques internes.  
  
5.  Configurer les adresses IP virtuelle externe (VIP) de l’équilibreur de charge externe avec les adresses 131.107.0.102 et 131.107.0.103. En outre, configurez les adresses IP virtuelles internes sur l’équilibreur de charge externe avec des adresses 30.1.1.10 2006:2005:1:1::10.  
  
6.  Le serveur d’accès à distance sera désormais être configuré avec les adresses IP planifiées et les adresses IP internes et externes pour le cluster seront configurés selon les adresses IP planifiés.  
  
## <a name="bkmk_2_2_NLB"></a>3.2 plan IP-HTTPS  
  
1.  **Spécifications relatives aux certificats**-au cours du déploiement du serveur d’accès à distance unique que vous avez sélectionné à utiliser un certificat IP-HTTPS émis par une autorité de certification publique ou interne (CA) ou un certificat auto-signé. Pour le déploiement du cluster, vous devez utiliser le même type de certificat sur chaque membre du cluster de l’accès à distance. Autrement dit, si vous avez utilisé un certificat émis par une autorité de certification publique (recommandée), vous devez installer un certificat émis par une autorité de certification publique sur chaque membre du cluster. Le nom du sujet du nouveau certificat doit être identique au nom du sujet du certificat IP-HTTPS actuellement utilisé dans votre déploiement. Notez que si vous utilisez des certificats auto-signés ces seront être configurés automatiquement sur chaque serveur lors du déploiement de cluster.  
  
2.  **Configuration requise du préfixe**-accès à distance permet l’équilibrage de charge du trafic SSL et le trafic DirectAccess. Pour équilibrer la charge de toutes les adresses IPv6 en fonction du trafic DirectAccess, l’accès à distance doit examiner le tunneling de IPv4 pour toutes les technologies de transition. Étant donné que le trafic IP-HTTPS est chiffré, examiner le contenu du tunnel IPv4 n’est pas possible. Pour activer le trafic IP-HTTPS équilibrer la charge, vous devez allouer un préfixe IPv6 suffisamment large afin qu’un préfixe IPv6 égales à/64 différent peut être affecté à chaque membre du cluster. Vous pouvez configurer un maximum de 32 serveurs dans un cluster à charge équilibrée de charge ; Par conséquent, vous devez spécifier un par/59 préfixe. Ce préfixe doit être routable vers l’adresse IPv6 interne du cluster d’accès à distance et est configuré dans l’Assistant Installation de serveur accès à distance.  
  
    > [!NOTE]  
    > Les exigences de préfixe sont pertinentes que dans un réseau interne IPv6 est activé (IPv6 uniquement, ou IPV4 + IPv6). Dans un réseau d’entreprise uniquement IPv4, le préfixe de client est configuré automatiquement et l’administrateur ne peut pas le modifier.  
  
## <a name="BKMK_3.3"></a>3.3 plan pour les connexions de client VPN  
Il existe un certain nombre de considérations pour les connexions de client VPN :  
  
-   Le trafic de client VPN ne peut pas être à charge équilibrée si les adresses de client VPN sont allouées à l’aide de DHCP. Un pool d’adresses statiques est requis.  
  
-   RRAS peut être activée sur un cluster d’équilibrage de charge qui a été déployé pour DirectAccess uniquement, à l’aide de **activer VPN** dans le volet de tâches de la console de gestion de l’accès à distance.  
  
-   Toutes les modifications VPN terminées dans la console Routage et de gestion de l’accès à distance (rrasmgmt.msc) aura doivent être répliquées manuellement sur tous les serveurs d’accès à distance dans le cluster.  
  
-   Pour activer le trafic de client VPN IPv6 à charge équilibrée, vous devez spécifier un préfixe IPv6 de 59 bits.  
  
## <a name="BKMK_nls"></a>3.4 planifier le serveur emplacement réseau  
Si vous exécutez le site de serveur d’emplacement réseau sur le serveur d’accès à distance unique, vous avez sélectionné pour utiliser un certificat émis par une autorité de certification interne (CA) ou un certificat auto-signé lors du déploiement.  Notez les points suivants :  
  
1.  Chaque membre du cluster de l’accès à distance doit avoir un certificat pour le serveur emplacement réseau qui correspond à l’entrée DNS pour le site de serveur d’emplacement réseau.  
  
2.  Le certificat pour chaque serveur en cluster doit être émis dans la même façon que le certificat sur le seul accès à distance serveur actuel certificat serveur emplacement réseau. Par exemple, si vous avez utilisé un certificat émis par une autorité de certification interne, vous devez installer un certificat émis par l’autorité de certification interne sur chaque membre du cluster.  
  
3.  Si vous avez utilisé un certificat auto-signé, un certificat auto-signé sera être configuré automatiquement pour chaque serveur lors du déploiement de cluster.  
  
4.  Le nom du sujet du certificat ne doit pas être identique au nom de tous les serveurs dans le déploiement de l’accès à distance.  
