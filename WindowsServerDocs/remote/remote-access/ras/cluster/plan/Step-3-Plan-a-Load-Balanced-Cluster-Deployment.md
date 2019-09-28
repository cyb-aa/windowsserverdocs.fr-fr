---
title: 'Étape 3 : planifier un déploiement de cluster à charge équilibrée'
description: Cette rubrique fait partie du guide déployer l’accès à distance dans un cluster dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7540c17b-81de-47de-a04f-3247afa26f70
ms.author: pashort
author: shortpatti
ms.openlocfilehash: beb2f5ce27115bf328917e38910198794f523547
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404605"
---
# <a name="step-3-plan-a-load-balanced-cluster-deployment"></a>Étape 3 : planifier un déploiement de cluster à charge équilibrée

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

L’étape suivante consiste à planifier la configuration de l’équilibrage de charge et le déploiement du cluster.  
  
|Tâche|Description|  
|----|--------|  
|3,1 planifier l’équilibrage de charge|Décidez s’il faut utiliser l’équilibrage de charge réseau (NLB) Windows ou un équilibreur de charge externe (ELB).|  
|3,2 planifier IP-HTTPs|Si un certificat auto-signé n’est pas utilisé, le serveur d’accès à distance a besoin d’un certificat SSL sur chaque serveur du cluster, afin d’authentifier les connexions IP-HTTPs.|  
|3,3 planifier des connexions client VPN|Notez la configuration requise pour les connexions de clients VPN.|  
|3,4 planifier le serveur emplacement réseau|Si le site Web du serveur d’emplacement réseau est hébergé sur le serveur d’accès à distance et qu’un certificat auto-signé n’est pas utilisé, assurez-vous que chaque serveur du cluster dispose d’un certificat de serveur pour authentifier la connexion au site Web.|  
  
## <a name="bkmk_2_1_Plan_LB"></a>3,1 planifier l’équilibrage de charge  
L’accès à distance peut être déployé sur un serveur unique ou sur un cluster de serveurs d’accès à distance. Le trafic vers le cluster peut être équilibré en charge pour fournir une haute disponibilité et une évolutivité aux clients DirectAccess. Il existe deux options d’équilibrage de charge :  
  
-   **Windows NLB**-Windows NLB est une fonctionnalité de Windows Server. Pour l’utiliser, vous n’avez pas besoin de matériel supplémentaire, car tous les serveurs du cluster sont responsables de la gestion de la charge du trafic. L’équilibrage de la charge réseau Windows prend en charge un maximum de huit serveurs dans un cluster d’accès à distance.  
  
-   Équilibrage de **charge externe**-l’utilisation d’un équilibreur de charge externe nécessite un matériel externe pour gérer la charge de trafic entre les serveurs de cluster d’accès à distance. En outre, l’utilisation d’un équilibreur de charge externe prend en charge un maximum de 32 serveurs d’accès à distance dans un cluster. Voici quelques points à prendre en compte lors de la configuration de l’équilibrage de charge externe :  
  
    -   L’administrateur doit s’assurer que les adresses IP virtuelles configurées via l’Assistant équilibrage de charge d’accès à distance sont utilisées sur les équilibrages de charge externes (par exemple, F5 BIG-IP local Traffic Manager System). Lorsque l’équilibrage de charge externe est activé, les adresses IP sur les interfaces externes et internes sont promues en adresses IP virtuelles et doivent être montées sur les équilibrages de charge. Cela permet à l’administrateur de ne pas avoir à modifier l’entrée DNS pour le nom public du déploiement de cluster. En outre, les points de terminaison de tunnel IPsec sont dérivés des adresses IP de serveur. Si l’administrateur fournit des adresses IP virtuelles distinctes, le client ne sera pas en mesure de se connecter au serveur. Consultez l’exemple de configuration de DirectAccess avec équilibrage de charge externe en 3.1.1 exemple de configuration de Load Balancer externe.  
  
    -   De nombreux équilibrages de charge externes (y compris F5) ne prennent pas en charge l’équilibrage de charge des 6to4 et ISATAP. Si le serveur d’accès à distance est un routeur ISATAP, la fonction ISATAP doit être déplacée vers un autre ordinateur. En outre, quand la fonction ISATAP se trouve sur un autre ordinateur, les serveurs DirectAccess doivent disposer d’une connectivité IPv6 native avec le routeur ISATAP. Notez que cette connectivité doit être présente avant la configuration de DirectAccess.  
  
    -   Pour l’équilibrage de charge externe, si Teredo doit être utilisé, tous les serveurs d’accès à distance doivent avoir deux adresses IPv4 publiques consécutives comme adresses IP dédiées. Les adresses IP virtuelles du cluster doivent également avoir deux adresses IPv4 publiques consécutives. Cela n’est pas vrai pour Windows NLB où seules les adresses IP virtuelles du cluster doivent avoir deux adresses IPv4 publiques consécutives. Si Teredo n’est pas utilisé, deux adresses IP consécutives ne sont pas requises.  
  
    -   L’administrateur peut passer de Windows NLB à l’équilibreur de charge externe et vice versa. Notez que l’administrateur ne peut pas passer de l’équilibreur de charge externe à l’équilibrage de charge Windows s’il compte plus de 8 serveurs dans le déploiement de l’équilibrage de charge externe.  
  
### <a name="ELBConfigEx"></a>3.1.1 exemple de configuration de Load Balancer externe  
Cette section décrit les étapes de configuration permettant d’activer un équilibreur de charge externe sur un nouveau déploiement de l’accès à distance. Lorsque vous utilisez un équilibreur de charge externe, le cluster d’accès à distance peut ressembler à la figure ci-dessous, où les serveurs d’accès à distance sont connectés au réseau d’entreprise via un équilibreur de charge sur le réseau interne et à Internet via un équilibreur de charge. connecté au réseau externe :  
  
![Exemple de configuration de Load Balancer externe](../../../../media/Step-3-Plan-a-Load-Balanced-Cluster-Deployment/ELBDiagram-URA_Enterprise_NLB-.png)  
  
##### <a name="planning-information"></a>Informations sur la planification  
  
1.  Les adresses IP virtuelles externes (adresses IP que le client utilisera pour se connecter à l’accès à distance) ont été décidées comme 131.107.0.102, 131.107.0.103  
  
2.  Équilibreur de charge sur des réseaux externes auto-IPs-131.107.0.245 (Internet), 131.107.1.245  
  
    Le réseau de périmètre (également appelé zone démilitarisée et DMZ) se trouve entre l’équilibreur de charge sur le réseau externe et le serveur d’accès à distance.  
  
3.  Adresses IP du serveur d’accès à distance sur le réseau de périmètre-131.107.1.102, 131.107.1.103  
  
4.  Adresses IP du serveur d’accès à distance sur le réseau ELB (par exemple, entre le serveur d’accès à distance et l’équilibreur de charge sur le réseau interne)-30.11.1.101, 2006:2005:11:1 ::101  
  
5.  Équilibreur de charge sur des réseaux internes auto-IPs-30.11.1.245 2006:2005:11:1 ::245 (ELB), 30.1.1.245 2006:2005:1:1 ::245 (corpnet)  
  
6.  Les adresses IP virtuelles internes (adresses IP utilisées pour la sonde Web cliente et pour le serveur emplacement réseau, si elles sont installées sur les serveurs d’accès à distance) ont été considérées comme étant 30.1.1.10, 2006:2005:1:1 ::10  
  
##### <a name="steps"></a>Étapes  
  
1.  Configurez la carte réseau externe du serveur d’accès à distance (qui est connectée au réseau de périmètre) avec les adresses 131.107.0.102, 131.107.0.103. Cette étape est requise pour que la configuration DirectAccess détecte les points de terminaison de tunnel IPsec appropriés.  
  
2.  Configurez la carte réseau interne du serveur d’accès à distance (qui est connectée au réseau ELB) avec les adresses IP du serveur d’emplacement réseau/sonde Web (30.1.1.10, 2006:2005:1:1 ::10). Cette étape est nécessaire pour permettre aux clients d’accéder à l’adresse IP de la sonde Web, de sorte que l’Assistant connectivité réseau indique correctement l’état de la connexion à DirectAccess. Cette étape permet également d’accéder au serveur emplacement réseau, s’il est configuré sur le serveur DirectAccess.  
  
    > [!NOTE]  
    > Assurez-vous que le contrôleur de domaine est accessible à partir du serveur d’accès à distance avec cette configuration.  
  
3.  Configurez un serveur unique DirectAccess sur le serveur d’accès à distance.  
  
4.  Activez l’équilibrage de charge externe dans la configuration DirectAccess. Utilisez 131.107.1.102 comme adresse IP dédiée externe (DIP) (131.107.1.103 sera sélectionné automatiquement), utilisez 30.11.1.101, 2006:2005:11:1 ::101 comme DIP internes.  
  
5.  Configurez l’adresse IP virtuelle (VIP) externe sur l’équilibreur de charge externe avec les adresses 131.107.0.102 et 131.107.0.103. En outre, configurez les adresses IP virtuelles internes sur l’équilibreur de charge externe avec les adresses 30.1.1.10 et 2006:2005:1:1 ::10.  
  
6.  Le serveur d’accès à distance sera désormais configuré avec les adresses IP planifiées, et les adresses IP externes et internes du cluster seront configurées en fonction des adresses IP planifiées.  
  
## <a name="bkmk_2_2_NLB"></a>3,2 planifier IP-HTTPs  
  
1.  **Exigences relatives aux certificats**: lors du déploiement du serveur d’accès à distance unique, vous avez choisi d’utiliser un certificat IP-HTTPS émis par une autorité de certification publique ou interne, ou un certificat auto-signé. Pour le déploiement de cluster, vous devez utiliser le même type de certificat sur chaque membre du cluster d’accès à distance. Autrement dit, si vous avez utilisé un certificat émis par une autorité de certification publique (recommandé), vous devez installer un certificat émis par une autorité de certification publique sur chaque membre du cluster. Le nom du sujet du nouveau certificat doit être identique au nom d’objet du certificat IP-HTTPs actuellement utilisé dans votre déploiement. Notez que si vous utilisez des certificats auto-signés, ceux-ci sont configurés automatiquement sur chaque serveur lors du déploiement du cluster.  
  
2.  **Conditions requises pour le préfixe**: l’accès à distance permet d’équilibrer la charge du trafic SSL et du trafic DirectAccess. Pour équilibrer la charge de tous les trafics DirectAccess basés sur IPv6, l’accès à distance doit examiner le tunnel IPv4 pour toutes les technologies de transition. Étant donné que le trafic IP-HTTPs est chiffré, l’examen du contenu du tunnel IPv4 n’est pas possible. Pour permettre l’équilibrage de charge du trafic IP-HTTPs, vous devez allouer un préfixe IPv6 suffisamment grand pour qu’un autre préfixe IPv6/64 puisse être attribué à chaque membre du cluster. Vous pouvez configurer un maximum de 32 serveurs dans un cluster à charge équilibrée. par conséquent, vous devez spécifier un préfixe/59. Ce préfixe doit être routable vers l’adresse IPv6 interne du cluster d’accès à distance et est configuré dans l’Assistant Installation du serveur d’accès à distance.  
  
    > [!NOTE]  
    > Les exigences en matière de préfixe ne sont pertinentes que dans un réseau interne compatible IPv6 (IPv6 uniquement ou IPV4 + IPv6). Dans un réseau d’entreprise IPv4 uniquement, le préfixe du client est configuré automatiquement et l’administrateur ne peut pas le modifier.  
  
## <a name="BKMK_3.3"></a>3,3 planifier des connexions client VPN  
Il existe plusieurs éléments à prendre en compte pour les connexions de clients VPN :  
  
-   Le trafic du client VPN ne peut pas faire l’équilibrage de charge si les adresses des clients VPN sont allouées à l’aide de DHCP. Un pool d’adresses statiques est requis.  
  
-   Le service RRAS peut être activé sur un cluster à charge équilibrée qui a été déployé pour DirectAccess uniquement, à l’aide de l’option **activer le VPN** sur le volet des tâches de la console de gestion de l’accès à distance.  
  
-   Toutes les modifications de VPN effectuées dans la console de gestion du routage et de l’accès à distance (rrasmgmt. msc) devront être répliquées manuellement sur tous les serveurs d’accès à distance du cluster.  
  
-   Pour activer l’équilibrage de charge du trafic du client IPv6 VPN, vous devez spécifier un préfixe IPv6 59 bits.  
  
## <a name="BKMK_nls"></a>3,4 planifier le serveur emplacement réseau  
Si vous exécutez le site Web du serveur emplacement réseau sur le serveur d’accès à distance unique, pendant le déploiement, vous avez choisi d’utiliser un certificat émis par une autorité de certification interne ou un certificat auto-signé.  Notez les points suivants :  
  
1.  Chaque membre du cluster d’accès à distance doit avoir un certificat pour le serveur emplacement réseau qui correspond à l’entrée DNS pour le site Web du serveur emplacement réseau.  
  
2.  Le certificat pour chaque serveur de cluster doit être émis de la même façon que le certificat sur le serveur d’accès à distance unique, le certificat de serveur d’emplacement réseau actuel. Par exemple, si vous avez utilisé un certificat émis par une autorité de certification interne, vous devez installer un certificat émis par l’autorité de certification interne sur chaque membre du cluster.  
  
3.  Si vous avez utilisé un certificat auto-signé, un certificat auto-signé est configuré automatiquement pour chaque serveur lors du déploiement du cluster.  
  
4.  Le nom du sujet du certificat ne doit pas être identique au nom de l’un des serveurs dans le déploiement de l’accès à distance.  
