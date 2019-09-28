---
title: Étape 2 planifier le déploiement de l’accès à distance
description: Cette rubrique fait partie du guide gérer des clients DirectAccess à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc9f02b9-8ddd-4cae-b397-a832996144dd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 131520f567da6529e342229a0f6965d3223f928b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404574"
---
# <a name="step-2-plan-the-remote-access-deployment"></a>Étape 2 planifier le déploiement de l’accès à distance

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Après avoir planifié l’infrastructure que vous prévoyez d’utiliser pour configurer votre serveur d’accès à distance unique pour la gestion à distance des clients DirectAccess, vous êtes prêt à planifier les paramètres que l’Assistant Configuration de l’accès à distance utilisera.  
  
> [!NOTE]  
> Avant de poursuivre ces tâches, voir [Step 1 : Planifier l’infrastructure d’accès à distance @ no__t-0.  
  
|Tâche|Description|  
|----|--------|  
|[Planifier une stratégie de déploiement de client](#plan-a-client-deployment-strategy)|Identifiez les ordinateurs gérés qui seront configurés en tant que clients DirectAccess.|  
|[Planifier une stratégie de déploiement de serveur d’accès à distance](#plan-a-remote-access-server-deployment-strategy)|Planifiez le déploiement du serveur d'accès à distance.|  
|[Planifier les configurations des serveurs d’infrastructure](#plan-the-infrastructure-servers-configurations)|Planifiez les serveurs d’infrastructure dans votre déploiement de l’accès à distance, y compris le serveur d’emplacement réseau DirectAccess, les serveurs DNS et les serveurs d’administration DirectAccess.|  
  
## <a name="plan-a-client-deployment-strategy"></a>Planifier une stratégie de déploiement de client  
Vous devez prendre trois décisions lors de la planification du déploiement de vos clients :  
  
1.  DirectAccess sera-t-il disponible uniquement pour les ordinateurs portables ou pour chaque ordinateur d’un groupe de sécurité spécifié ?  
  
    Quand vous configurez des clients DirectAccess dans l’Assistant Installation du client DirectAccess, vous pouvez choisir d’autoriser uniquement les ordinateurs portables dans les groupes de sécurité spécifiés à se connecter au serveur à l’aide de DirectAccess. Si vous voulez restreindre l'accès aux ordinateurs portables, le service Accès à distance configure automatiquement un filtre WMI pour garantir que l'objet de stratégie de groupe Client DirectAccess est appliqué uniquement aux ordinateurs portables des groupes de sécurité spécifiés. Pour activer ce paramètre, l'administrateur de l'accès à distance doit disposer d'autorisations de création ou de modification des filtres WMI de stratégie de groupe.  
  
2.  Quels groupes de sécurité contiendront les ordinateurs clients DirectAccess ?  
  
    Les paramètres DirectAccess sont contenus dans l’objet stratégie de groupe du client DirectAccess. L'objet de stratégie de groupe est appliqué aux ordinateurs figurant dans les groupes de sécurité que vous spécifiez dans l'Assistant Installation des clients DirectAccess. Vous pouvez spécifier des groupes de sécurité contenus dans tout domaine pris en charge.
  
    Avant de configurer l’accès à distance, vous devez créer les groupes de sécurité. Vous pouvez ajouter des ordinateurs au groupe de sécurité après avoir effectué le déploiement de l’accès à distance. Toutefois, si vous ajoutez des ordinateurs clients qui résident dans un domaine différent de celui du groupe de sécurité, l’objet de stratégie de groupe client ne sera pas appliqué à ces clients. Par exemple, si vous avez créé SG1 dans le domaine A pour les clients DirectAccess et que vous ajoutez par la suite des clients du domaine B à ce groupe, l’objet de stratégie de groupe client ne sera pas appliqué aux clients du domaine B.  
  
    Pour éviter ce problème, créez un nouveau groupe de sécurité client pour chaque domaine contenant des ordinateurs clients. Sinon, si vous ne souhaitez pas créer un nouveau groupe de sécurité, exécutez l’applet de commande **Add-daclient** Windows PowerShell avec le nom du nouvel objet de stratégie de groupe pour le nouveau domaine.  
  
3.  Quels paramètres allez-vous configurer pour l’Assistant connectivité réseau DirectAccess ?  
  
    L’Assistant connectivité réseau DirectAccess s’exécute sur les ordinateurs clients et fournit des informations supplémentaires sur la connexion DirectAccess aux utilisateurs finaux. Dans l'Assistant Installation des clients DirectAccess, vous pouvez configurer les éléments suivants :  
  
    -   **Vérificateurs de connectivité**  
  
        Une sonde web par défaut est créée et elle est utilisée par les clients pour valider la connectivité au réseau interne. Le nom par défaut `https://directaccess-WebProbeHost.<domain_name>`est. Ce nom doit être enregistré manuellement dans DNS. Vous pouvez créer d’autres vérificateurs de connectivité utilisant d’autres adresses Web via HTTP ou PING. Pour chaque vérificateur de connectivité, une entrée DNS doit exister.  
  
    -   **Adresse de messagerie du support technique**  
  
        Si les utilisateurs finaux rencontrent des problèmes de connectivité DirectAccess, ils peuvent envoyer un e-mail contenant des informations de diagnostic à l’administrateur de l’accès à distance, qui peut résoudre le problème.  
  
    -   **Nom de la connexion DirectAccess**  
  
        Vous pouvez spécifier un nom de connexion DirectAccess pour aider les utilisateurs finaux à identifier la connexion DirectAccess sur leur ordinateur.  
  
    -   **Autoriser les clients DirectAccess à utiliser la résolution de noms locale**  
  
        Les clients requièrent un moyen de résoudre les noms localement. Si vous autorisez les clients DirectAccess à utiliser la résolution de noms locale, les utilisateurs finaux peuvent utiliser les serveurs DNS locaux pour résoudre les noms. Lorsque les utilisateurs finaux choisissent d’utiliser des serveurs DNS locaux pour la résolution de noms, DirectAccess n’envoie pas de demandes de résolution pour les noms d’étiquette uniques au serveur DNS d’entreprise interne. Il utilise la résolution de noms locale à la place (à l’aide de la résolution de noms LLMNR (Link-local multicast Name Resolution) et NetBios sur les protocoles TCP/IP).  
  
## <a name="plan-a-remote-access-server-deployment-strategy"></a>Planifier une stratégie de déploiement de serveur d’accès à distance  
Les décisions que vous devez prendre lorsque vous envisagez de déployer votre serveur d’accès à distance sont les suivantes :  
  
-   **Topologie du réseau**  
  
    Deux topologies sont disponibles lors du déploiement d’un serveur d’accès à distance :  
  
    -   **Deux adaptateurs**: Avec deux cartes réseau, l’accès à distance peut être configuré avec une seule carte réseau connectée directement à Internet et l’autre connectée au réseau interne. Ou bien, le serveur est installé derrière un périphérique de périmètre, tel qu’un pare-feu ou un routeur. Dans cette configuration, une carte réseau est connectée au réseau de périmètre et l’autre est connectée au réseau interne.  
  
    -   **Carte réseau unique**: Dans cette configuration, le serveur d’accès à distance est installé derrière un périphérique de périmètre, tel qu’un pare-feu ou un routeur. La carte réseau est connectée au réseau interne.  

-   **Cartes réseau**  
  
    L’Assistant Installation du serveur d’accès à distance détecte automatiquement les cartes réseau qui sont configurées sur le serveur d’accès à distance. Vous devez vous assurer que les cartes appropriées sont sélectionnées.  
  
-   **Certificat IP-HTTPs**  
  
    L'Assistant Installation du serveur d'accès à distance détecte automatiquement un certificat adapté à la connexion IP-HTTPS. Le nom de sujet du certificat que vous sélectionnez doit correspondre à l'adresse ConnectTo. Si vous utilisez des certificats auto-signés, vous pouvez choisir d’utiliser un certificat créé automatiquement par le serveur d’accès à distance.  
  
-   **Préfixes IPv6**  
  
    Si l'Assistant Installation du serveur d'accès à distance détecte qu'IPv6 a été déployé sur les cartes réseau, il renseigne automatiquement les préfixes IPv6 pour le réseau interne, un préfixe IPv6 à affecter aux ordinateurs clients DirectAccess et un préfixe IPv6 à affecter aux ordinateurs clients VPN. Si les préfixes générés automatiquement ne sont pas corrects pour votre infrastructure IPv6 ou ISATAP native, vous devez les modifier manuellement.  
  
-   **Authentification**  
  
    Vous pouvez choisir l’une des méthodes suivantes pour authentifier les clients DirectAccess sur le serveur d’accès à distance :  
  
    -   **Authentification de l’utilisateur**: Vous pouvez permettre aux utilisateurs de s'authentifier à l'aide des informations d'identification Active Directory ou d'une authentification à deux facteurs.  
  
    -   **Authentification de l’ordinateur**: Vous pouvez configurer l’authentification de l’ordinateur pour utiliser des certificats. Ou le serveur d’accès à distance peut agir en tant que proxy pour l’authentification Kerberos sans nécessiter de certificats. 
  
    -   **Clients Windows 7** Par défaut, les ordinateurs clients qui exécutent Windows 7 ne peuvent pas se connecter à un déploiement de l’accès à distance exécutant Windows Server 2012. Si vous avez des clients exécutant Windows 7 dans votre organisation qui requièrent un accès à distance aux ressources internes, vous pouvez les autoriser à se connecter. Tout ordinateur client que vous voulez autoriser à accéder aux ressources internes doit être membre d'un groupe de sécurité que vous spécifiez dans l'Assistant Installation des clients DirectAccess.  
  
        > [!NOTE]  
        > Pour permettre aux clients exécutant Windows 7 de se connecter à l’aide de DirectAccess, vous devez utiliser l’authentification par certificat d’ordinateur.  
  
-   **Configuration VPN**  
  
    Avant de configurer l’accès à distance, déterminez si vous allez fournir un accès VPN aux clients distants. Vous devez fournir un accès VPN si des ordinateurs clients de votre organisation ne prennent pas en charge la connectivité DirectAccess (par exemple, s’ils ne sont pas gérés ou s’ils exécutent un système d’exploitation pour lequel DirectAccess n’est pas pris en charge). L’Assistant Installation du serveur d’accès à distance vous permet de configurer la façon dont les adresses IP sont attribuées (à l’aide de DHCP ou d’un pool d’adresses statiques) et de la façon dont les clients VPN sont authentifiés (à l’aide d’Active Directory ou d’un serveur RADIUS).  
  
## <a name="plan-the-infrastructure-servers-configurations"></a>Planifier les configurations des serveurs d’infrastructure  
L’accès à distance requiert trois types de serveurs d’infrastructure :  
  
-   **Serveur emplacement réseau**  
  
-   **Serveurs DNS** 
  
-   **Serveurs d’administration** 
  
## <a name="see-also"></a>Voir aussi  
  
-   [Étape 1 : Planifier l’infrastructure d’accès à distance](Step-1-Plan-the-Remote-Access-Infrastructure.md)  
  


