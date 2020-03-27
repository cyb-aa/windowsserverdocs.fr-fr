---
title: Configurer des clients RADIUS
description: Cette rubrique fournit des informations sur la configuration des clients RADIUS pour le serveur NPS (Network Policy Server) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b7bc75ea81133c91ad7e9883f03c3e32f085b5eb
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315703"
---
# <a name="configure-radius-clients"></a>Configurer des clients RADIUS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour configurer des serveurs d’accès réseau en tant que clients RADIUS dans NPS.

Lorsque vous ajoutez un nouveau serveur d’accès réseau \(serveur VPN, le point d’accès sans fil, le commutateur d’authentification ou le serveur d’accès à distance\) à votre réseau, vous devez ajouter le serveur en tant que client RADIUS dans NPS, puis configurer le client RADIUS pour qu’il communique avec le serveur NPS.

>[!IMPORTANT]
>Les ordinateurs clients et les périphériques, tels que les ordinateurs portables, les tablettes, les téléphones et les autres ordinateurs exécutant des systèmes d’exploitation clients, ne sont pas des clients RADIUS. Les clients RADIUS sont des serveurs d’accès réseau, tels que les points d’accès sans fil, les commutateurs 802.1 X, les serveurs de réseau privé virtuel (VPN) et les serveurs d’accès à distance, car ils utilisent le protocole RADIUS pour communiquer avec les serveurs RADIUS, tels que le serveur de stratégie réseau \(les serveurs NPS\).

Cette étape est également nécessaire lorsque votre serveur NPS est membre d’un groupe de serveurs RADIUS distants configuré sur un proxy NPS. Dans ce cas, en plus d’effectuer les étapes de cette tâche sur le proxy NPS, vous devez effectuer les opérations suivantes :

- Sur le proxy NPS, configurez un groupe de serveurs RADIUS distants qui contient le serveur NPS.
- Sur le serveur NPS distant, configurez le proxy NPS en tant que client RADIUS.

Pour exécuter les procédures de cette rubrique, vous devez disposer d’au moins un serveur d’accès réseau \(serveur VPN, le point d’accès sans fil, le commutateur d’authentification ou le serveur d’accès à distance\) ou le proxy NPS physiquement installé sur votre réseau.

## <a name="configure-the-network-access-server"></a>Configurer le serveur d’accès réseau

Utilisez cette procédure pour configurer des serveurs d’accès réseau pour une utilisation avec NPS. Lorsque vous déployez des serveurs d’accès réseau (NAS) en tant que clients RADIUS, vous devez configurer les clients pour qu’ils communiquent avec le NPSs où les serveurs NAS sont configurés en tant que clients.

Cette procédure fournit des instructions générales sur les paramètres que vous devez utiliser pour configurer vos serveurs NAS. pour obtenir des instructions spécifiques sur la configuration de l’appareil que vous déployez sur votre réseau, consultez la documentation de votre produit NAS.

### <a name="to-configure-the-network-access-server"></a>Pour configurer le serveur d’accès réseau

1. Sur le serveur NAS, **dans paramètres RADIUS**, sélectionnez **authentification RADIUS** sur le port UDP (User Datagram Protocol) **1812** et **gestion des comptes RADIUS** sur le port UDP **1813**.
2. Dans **serveur d’authentification** ou **serveur RADIUS**, spécifiez votre serveur NPS par adresse IP ou nom de domaine complet (FQDN), en fonction des exigences du NAS. 
3. Dans secret **secret** ou **secret partagé**, tapez un mot de passe fort. Quand vous configurez le NAS en tant que client RADIUS dans NPS, vous utilisez le même mot de passe, donc ne l’oubliez pas.
4. Si vous utilisez PEAP ou EAP comme méthode d’authentification, configurez le NAS pour utiliser l’authentification EAP.
5. Si vous configurez un point d’accès sans fil, dans **SSID**, spécifiez un identificateur de jeu de service \(SSID\), qui est une chaîne alphanumérique qui sert de nom de réseau. Ce nom est diffusé par les points d’accès aux clients sans fil et est visible pour les utilisateurs à la fidélité sans fil \(points d’accès Wi-Fi\).
6. Si vous configurez un point d’accès sans fil, dans **802.1 x et WPA**, activez **l’authentification IEEE 802.1 x** si vous souhaitez déployer PEAP-MS-CHAP v2, PEAP-TLS ou EAP-TLS.

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>Ajouter le serveur d’accès réseau en tant que client RADIUS dans NPS

Utilisez cette procédure pour ajouter un serveur d’accès réseau en tant que client RADIUS dans NPS. Vous pouvez utiliser cette procédure pour configurer un NAS en tant que client RADIUS à l’aide de la console NPS.

Pour réaliser cette procédure, vous devez être membre du groupe **Administrateurs**.

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Pour ajouter un serveur d’accès réseau en tant que client RADIUS dans NPS

1. Sur le serveur NPS, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur serveur NPS ( **Network Policy Server**). La console NPS s’ouvre.
2. Dans la console NPS, double-cliquez sur **clients et serveurs RADIUS**. Cliquez avec le bouton droit sur **clients RADIUS**, puis cliquez sur **nouveau client RADIUS**. 
3. Dans **nouveau client RADIUS**, vérifiez que la case à cocher **activer ce client RADIUS** est activée.
4. Dans **nouveau client RADIUS**, dans **nom convivial**, tapez un nom d’affichage pour le NAS. Dans **adresse (IP ou DNS)** , tapez l’adresse IP NAS ou le nom de domaine complet (FQDN). Si vous entrez le nom de domaine complet, cliquez sur **vérifier** si vous souhaitez vérifier que le nom est correct et qu’il correspond à une adresse IP valide. 
5. Dans **nouveau client RADIUS**, dans **fournisseur**, spécifiez le nom du fabricant NAS. Si vous n’êtes pas sûr du nom du fabricant NAS, sélectionnez **RADIUS standard**.
6. Dans **nouveau client RADIUS**, dans **secret partagé**, effectuez l’une des opérations suivantes :
    - Vérifiez que l’option **Manuel** est sélectionnée, puis, dans **secret partagé**, tapez le mot de passe fort qui est également entré sur le NAS. Retapez le secret partagé dans **confirmer le secret partagé**.
    - Sélectionnez **générer**, puis cliquez sur **générer** pour générer automatiquement un secret partagé. Enregistrez le secret partagé généré pour la configuration sur le NAS afin qu’il puisse communiquer avec le serveur NPS.
7. Dans **nouveau client RADIUS**, dans **options supplémentaires**, si vous utilisez des méthodes d’authentification autres que EAP et PEAP, et si votre NAS prend en charge l’utilisation de l’attribut de l’authentificateur de message, sélectionnez **les messages de demande d’accès doivent contenir l’attribut de l’authentificateur de message**.
8. Cliquez sur **OK**. Votre NAS s’affiche dans la liste des clients RADIUS configurés sur le serveur NPS.

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>Configurer des clients RADIUS par plage d’adresses IP dans Windows Server 2016 Datacenter

Si vous exécutez Windows Server 2016 Datacenter, vous pouvez configurer des clients RADIUS dans NPS par plage d’adresses IP. Cela vous permet d’ajouter un grand nombre de clients RADIUS (tels que des points d’accès sans fil) à la console NPS en même temps, au lieu d’ajouter chaque client RADIUS individuellement.

Vous ne pouvez pas configurer les clients RADIUS par plage d’adresses IP si vous exécutez NPS sur Windows Server 2016 standard.

Utilisez cette procédure pour ajouter un groupe de serveurs d’accès réseau (NAS) en tant que clients RADIUS configurés avec des adresses IP à partir de la même plage d’adresses IP.

Tous les clients RADIUS de la plage doivent utiliser la même configuration et le même secret partagé.

Pour réaliser cette procédure, vous devez être membre du groupe **Administrateurs**.

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>Pour configurer des clients RADIUS par plage d’adresses IP

1. Sur le serveur NPS, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur serveur NPS ( **Network Policy Server**). La console NPS s’ouvre.
2. Dans la console NPS, double-cliquez sur **clients et serveurs RADIUS**. Cliquez avec le bouton droit sur **clients RADIUS**, puis cliquez sur **nouveau client RADIUS**.
3. Dans **nouveau client RADIUS**, dans **nom convivial**, tapez un nom d’affichage pour la collection de serveurs NAS.
4. Dans **adresse \(\)IP ou DNS** , tapez la plage d’adresses IP pour les clients RADIUS en utilisant le routage inter-domaines non classé \(notation de\) CIDR. Par exemple, si la plage d’adresses IP pour le NAS est 10.10.0.0, tapez **10.10.0.0/16**.
5. Dans **nouveau client RADIUS**, dans **fournisseur**, spécifiez le nom du fabricant NAS. Si vous n’êtes pas sûr du nom du fabricant NAS, sélectionnez **RADIUS standard**.
6. Dans **nouveau client RADIUS**, dans **secret partagé**, effectuez l’une des opérations suivantes :
    - Vérifiez que l’option **Manuel** est sélectionnée, puis, dans **secret partagé**, tapez le mot de passe fort qui est également entré sur le NAS. Retapez le secret partagé dans **confirmer le secret partagé**.
    - Sélectionnez **générer**, puis cliquez sur **générer** pour générer automatiquement un secret partagé. Enregistrez le secret partagé généré pour la configuration sur le NAS afin qu’il puisse communiquer avec le serveur NPS.
7. Dans **nouveau client RADIUS**, dans **options supplémentaires**, si vous utilisez des méthodes d’authentification autres que EAP et PEAP, et si tous vos serveurs NAS prennent en charge l’utilisation de l’attribut de l’authentificateur de message, sélectionnez **les messages de demande d’accès doivent contenir l’attribut de l’authentificateur de message**.
8. Cliquez sur **OK**. Vos serveurs d’ordinateurs s’affichent dans la liste des clients RADIUS configurés sur le serveur NPS.

Pour plus d’informations, consultez [clients RADIUS](nps-radius-clients.md).

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).