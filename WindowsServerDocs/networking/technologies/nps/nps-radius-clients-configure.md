---
title: Configurer des clients RADIUS
description: Cette rubrique fournit des informations sur la configuration des Clients RADIUS pour le serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 851bdb0677a17567f81a2c331baad595fe40436a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839490"
---
# <a name="configure-radius-clients"></a>Configurer des clients RADIUS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour configurer les serveurs d’accès réseau en tant que Clients RADIUS dans NPS.

Lorsque vous ajoutez un nouveau serveur d’accès réseau \(serveur VPN, point d’accès sans fil, commutateur d’authentification ou serveur à distance\) à votre réseau, vous devez ajouter le serveur en tant que client RADIUS dans NPS et puis configurez le client RADIUS pour communiquer avec le serveur NPS.

>[!IMPORTANT]
>Les ordinateurs clients et périphériques, tels que les ordinateurs portables, tablettes, téléphones et autres ordinateurs exécutant les systèmes d’exploitation clients, ne sont pas des clients RADIUS. Les clients RADIUS sont des serveurs d’accès réseau - tels que les points d’accès sans fil, commutateurs compatibles X 802. 1, virtual private network serveurs (VPN) et les serveurs d’accès à distance - car ils utilisent le protocole RADIUS pour communiquer avec les serveurs RADIUS, tels que la stratégie de réseau Serveur \(NPS\) serveurs.

Cette étape est également nécessaire lorsque le serveur NPS est un membre d’un groupe de serveurs RADIUS distant qui est configuré sur un proxy de serveur NPS. Dans ce cas, en plus de suivre les étapes de cette tâche sur le proxy NPS, vous devez procédez comme suit :

- Sur le proxy NPS, configurez un groupe de serveurs RADIUS distants qui contient le serveur NPS.
- Sur le serveur NPS à distance, configurez le proxy de serveur NPS en tant que client RADIUS.

Pour effectuer les procédures décrites dans cette rubrique, vous devez disposer d’au moins un serveur d’accès réseau \(serveur VPN, point d’accès sans fil, commutateur d’authentification ou serveur à distance\) ou proxy NPS installée physiquement sur votre réseau.

## <a name="configure-the-network-access-server"></a>Configurer le serveur d’accès réseau

Cette procédure permet de configurer des serveurs d’accès réseau pour une utilisation avec le serveur NPS. Lorsque vous déployez des serveurs d’accès réseau (NAS) en tant que clients RADIUS, vous devez configurer les clients pour communiquer avec les NPSs où les serveurs NAS sont configurés en tant que clients.

Cette procédure fournit des recommandations générales sur les paramètres que vous devez utiliser pour configurer votre NAS ; Pour obtenir des instructions spécifiques sur la façon de configurer l’appareil que vous déployez sur votre réseau, consultez la documentation de votre produit NAS.

### <a name="to-configure-the-network-access-server"></a>Pour configurer le serveur d’accès réseau

1. Sur le NAS, dans **paramètres RADIUS**, sélectionnez **l’authentification RADIUS** sur le port de protocole UDP (User Datagram) **1812** et **gestion des comptes RADIUS**sur le port UDP **1813**.
2. Dans **serveur d’authentification** ou **serveur RADIUS**, spécifiez le serveur NPS par adresse IP ou nom de domaine complet (FQDN), selon les besoins du NAS. 
3. Dans **Secret** ou **secret partagé**, tapez un mot de passe fort. Lorsque vous configurez le NAS en tant que client RADIUS dans NPS, vous allez utiliser le même mot de passe, afin de ne pas l’oublier.
4. Si vous utilisez PEAP ou EAP comme méthode d’authentification, configurez le serveur NAS pour utiliser l’authentification EAP.
5. Si vous configurez un point d’accès sans fil dans **SSID**, spécifiez un Service Set Identifier \(SSID\), qui est une chaîne alphanumérique qui sert de nom de réseau. Ce nom est diffusé par des points d’accès pour les clients sans fil et est visible pour les utilisateurs de votre fidélité sans fil \(Wi-Fi\) zones réactives.
6. Si vous configurez un point d’accès sans fil dans **802. 1 X et WPA**, activer **l’authentification IEEE 802. 1 X** si vous souhaitez déployer PEAP-MS-CHAP v2, PEAP-TLS ou EAP-TLS.

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>Ajouter le serveur d’accès réseau en tant que Client RADIUS dans NPS

Utilisez cette procédure pour ajouter un serveur d’accès réseau en tant que client RADIUS dans NPS. Vous pouvez utiliser cette procédure pour configurer un NAS comme client RADIUS à l’aide de la console NPS.

Pour réaliser cette procédure, vous devez être membre du groupe **Administrateurs**.

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Pour ajouter un serveur d’accès réseau en tant que client RADIUS dans NPS

1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. La console NPS s’ouvre.
2. Dans la console NPS, double-cliquez sur **Clients et serveurs RADIUS**. Avec le bouton droit **Clients RADIUS**, puis cliquez sur **nouveau Client RADIUS**. 
3. Dans **nouveau Client RADIUS**, vérifiez que le **activer ce client RADIUS** case à cocher est activée.
4. Dans **nouveau Client RADIUS**, dans **nom convivial**, tapez un nom complet pour le serveur NAS. Dans **adresse (IP ou DNS)**, tapez le nom de domaine complet (FQDN) ou l’adresse IP du NAS. Si vous entrez le nom de domaine complet, cliquez sur **Vérifiez** si vous souhaitez vérifier que le nom est correct et qu’il est mappé à une adresse IP valide. 
5. Dans **nouveau Client RADIUS**, dans **fournisseur**, spécifiez le nom de fabricant NAS. Si vous n’êtes pas sûr du nom de fabricant NAS, sélectionnez **norme RADIUS**.
6. Dans **nouveau Client RADIUS**, dans **secret partagé**, effectuez l’une des opérations suivantes :
    - Vérifiez que **manuel** est sélectionnée, puis dans **secret partagé**, tapez le mot de passe fort qui est aussi entré sur le NAS. Retapez le secret partagé dans **confirmer le secret partagé**.
    - Sélectionnez **générer**, puis cliquez sur **générer** pour générer automatiquement un secret partagé. Enregistrer le secret partagé généré pour la configuration sur le NAS, afin qu’il puisse communiquer avec le serveur NPS.
7. Dans **nouveau Client RADIUS**, dans **des Options supplémentaires**, si vous utilisez des méthodes d’authentification autre que EAP et PEAP et si votre NAS prend en charge l’utilisation de l’attribut de l’authentificateur de message, sélectionnez **Messages de demande d’accès doivent contenir l’attribut de l’authentificateur de Message**.
8. Cliquez sur **OK**. Votre NAS s’affiche dans la liste des clients RADIUS configuré sur le serveur NPS.

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>Configurer des Clients RADIUS par plage d’adresses IP dans Windows Server 2016 Datacenter

Si vous exécutez Windows Server 2016 Datacenter, vous pouvez configurer des clients RADIUS dans NPS par plage d’adresses IP. Ainsi, vous pouvez ajouter un grand nombre de clients RADIUS (tels que les points d’accès sans fil) la console NPS en même temps, au lieu d’ajouter chaque client RADIUS individuellement.

Vous ne pouvez pas configurer les clients RADIUS par plage d’adresses IP si vous utilisez NPS sur Windows Server 2016 Standard.

Utilisez cette procédure pour ajouter un groupe de serveurs d’accès réseau (NAS) en tant que clients RADIUS qui sont configurés avec des adresses IP à partir de la même plage d’adresses IP.

Tous les clients RADIUS dans la plage doivent utiliser la même configuration et le secret partagé.

Pour réaliser cette procédure, vous devez être membre du groupe **Administrateurs**.

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>Pour configurer les clients RADIUS en plage d’adresses IP

1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. La console NPS s’ouvre.
2. Dans la console NPS, double-cliquez sur **Clients et serveurs RADIUS**. Avec le bouton droit **Clients RADIUS**, puis cliquez sur **nouveau Client RADIUS**.
3. Dans **nouveau Client RADIUS**, dans **nom convivial**, tapez un nom d’affichage pour la collection de serveurs NAS.
4. Dans **adresse \(IP ou DNS\)**, tapez la plage d’adresses IP pour les clients RADIUS à l’aide de Classless Interdomain Routing \(CIDR\) notation. Par exemple, si la plage d’adresses IP pour les serveurs NAS est 10.10.0.0, tapez **10.10.0.0/16**.
5. Dans **nouveau Client RADIUS**, dans **fournisseur**, spécifiez le nom de fabricant NAS. Si vous n’êtes pas sûr du nom de fabricant NAS, sélectionnez **norme RADIUS**.
6. Dans **nouveau Client RADIUS**, dans **secret partagé**, effectuez l’une des opérations suivantes :
    - Vérifiez que **manuel** est sélectionnée, puis dans **secret partagé**, tapez le mot de passe fort qui est aussi entré sur le NAS. Retapez le secret partagé dans **confirmer le secret partagé**.
    - Sélectionnez **générer**, puis cliquez sur **générer** pour générer automatiquement un secret partagé. Enregistrer le secret partagé généré pour la configuration sur le NAS, afin qu’il puisse communiquer avec le serveur NPS.
7. Dans **nouveau Client RADIUS**, dans **des Options supplémentaires**, si vous utilisez des méthodes d’authentification autre que EAP et PEAP et si tous vos serveurs NAS prennent en charge l’utilisation de l’attribut de l’authentificateur de message, sélectionnez  **Messages de demande d’accès doivent contenir l’attribut de l’authentificateur de Message**.
8. Cliquez sur **OK**. Votre NAS s’affichent dans la liste des clients RADIUS configuré sur le serveur NPS.

Pour plus d’informations, consultez [Clients RADIUS](nps-radius-clients.md).

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).