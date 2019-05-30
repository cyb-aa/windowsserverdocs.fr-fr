---
title: Étape 2 planifier le déploiement de l’accès à distance
description: Cette rubrique fait partie du guide les Clients DirectAccess de gérer à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc9f02b9-8ddd-4cae-b397-a832996144dd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fb940effaae7989dec397e539b64160c87828d5a
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266705"
---
# <a name="step-2-plan-the-remote-access-deployment"></a>Étape 2 planifier le déploiement de l’accès à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Une fois que vous envisagez de l’infrastructure que vous souhaitez utiliser pour configurer votre serveur d’accès à distance unique pour la gestion à distance des clients DirectAccess, vous êtes prêt à planifier les paramètres qui utilise l’Assistant Installation de l’accès à distance.  
  
> [!NOTE]  
> Avant de continuer avec ces tâches, consultez [étape 1 : Planifier l’Infrastructure d’accès à distance](Step-1-Plan-the-Remote-Access-Infrastructure.md).  
  
|Tâche|Description|  
|----|--------|  
|[Planifier une stratégie de déploiement client](#plan-a-client-deployment-strategy)|Identifiez les ordinateurs gérés qui seront configurés en tant que clients DirectAccess.|  
|[Planifier une stratégie de déploiement de serveur accès à distance](#plan-a-remote-access-server-deployment-strategy)|Planifiez le déploiement du serveur d'accès à distance.|  
|[Planifier les configurations de serveurs d’infrastructure](#plan-the-infrastructure-servers-configurations)|Planifier les serveurs d’infrastructure dans votre déploiement de l’accès à distance, y compris le serveur emplacement réseau DirectAccess, les serveurs DNS et les serveurs d’administration DirectAccess.|  
  
## <a name="plan-a-client-deployment-strategy"></a>Planifier une stratégie de déploiement client  
Vous devez prendre trois décisions lors de la planification du déploiement de vos clients :  
  
1.  DirectAccess sera-t-il disponible sur les ordinateurs portables uniquement, ou à tous les ordinateurs dans un groupe de sécurité spécifié ?  
  
    Lorsque vous configurez les clients DirectAccess dans l’Assistant Installation du Client DirectAccess, vous pouvez choisir d’autoriser uniquement les ordinateurs portables dans des groupes de sécurité spécifiés pour se connecter au serveur à l’aide de DirectAccess. Si vous voulez restreindre l'accès aux ordinateurs portables, le service Accès à distance configure automatiquement un filtre WMI pour garantir que l'objet de stratégie de groupe Client DirectAccess est appliqué uniquement aux ordinateurs portables des groupes de sécurité spécifiés. Pour activer ce paramètre, l'administrateur de l'accès à distance doit disposer d'autorisations de création ou de modification des filtres WMI de stratégie de groupe.  
  
2.  Quels groupes de sécurité contiendront les ordinateurs clients DirectAccess ?  
  
    Les paramètres DirectAccess contenus dans l’objet de stratégie de groupe (GPO) du client DirectAccess. L'objet de stratégie de groupe est appliqué aux ordinateurs figurant dans les groupes de sécurité que vous spécifiez dans l'Assistant Installation des clients DirectAccess. Vous pouvez spécifier des groupes de sécurité qui sont contenus dans tout domaine pris en charge.
  
    Avant de configurer l’accès à distance, vous devez créer les groupes de sécurité. Vous pouvez ajouter des ordinateurs au groupe de sécurité après avoir terminé le déploiement de l’accès à distance. Toutefois, si vous ajoutez des ordinateurs clients qui résident dans un autre domaine que le groupe de sécurité, l’objet de stratégie de groupe client n’est pas appliqué à ces clients. Par exemple, si vous avez créé SG1 dans un domaine A pour les clients DirectAccess et vous ajoutez ensuite des clients d’un domaine B à ce groupe, l’objet de stratégie de groupe client ne sera pas appliquée aux clients du domaine B.  
  
    Pour éviter ce problème, créez un nouveau groupe de sécurité client pour chaque domaine qui contient les ordinateurs clients. Ou, si vous ne souhaitez pas créer un nouveau groupe de sécurité, exécutez le **Add-DAClient** applet de commande Windows PowerShell avec le nom de l’objet de stratégie de groupe pour le nouveau domaine.  
  
3.  Les paramètres que vous configurez pour l’Assistant de connectivité de réseau DirectAccess ?  
  
    L’Assistant de connectivité de réseau DirectAccess s’exécute sur les ordinateurs clients et fournit des informations supplémentaires sur la connexion DirectAccess aux utilisateurs finaux. Dans l'Assistant Installation des clients DirectAccess, vous pouvez configurer les éléments suivants :  
  
    -   **Vérificateurs de connectivité**  
  
        Une sonde web par défaut est créée et elle est utilisée par les clients pour valider la connectivité au réseau interne. Le nom par défaut est https://directaccess-WebProbeHost.<domain_name>. Ce nom doit être enregistré manuellement dans DNS. Vous pouvez créer d’autres vérificateurs de connectivité qui utilisent d’autres adresses web via HTTP ou PING. Pour chaque vérificateur de connectivité, une entrée DNS doit exister.  
  
    -   **Aider à adresse électronique du support**  
  
        Si les utilisateurs finaux rencontrent des problèmes de connectivité DirectAccess, ils peuvent envoyer un e-mail contenant des informations de diagnostic à l’administrateur d’accès à distance, ce qui peut résoudre le problème.  
  
    -   **Nom de connexion DirectAccess**  
  
        Vous pouvez spécifier un nom de connexion DirectAccess pour aider les utilisateurs finaux à identifier la connexion DirectAccess sur leur ordinateur.  
  
    -   **Autoriser les clients DirectAccess à utiliser la résolution de noms locale**  
  
        Les clients requièrent un moyen de résolution de noms localement. Si vous autorisez les clients DirectAccess à utiliser la résolution de noms locale, les utilisateurs finaux peuvent utiliser les serveurs DNS locaux pour résoudre les noms. Lorsque les utilisateurs finaux choisir d’utiliser des serveurs DNS locaux pour la résolution de noms, DirectAccess n’envoie pas de demandes de résolution de noms d’étiquette unique vers le serveur DNS d’entreprise interne. Il utilise la résolution de nom local à la place (en utilisant le lien-Local Multicast LLMNR (Name Resolution) et NetBios sur les protocoles TCP/IP).  
  
## <a name="plan-a-remote-access-server-deployment-strategy"></a>Planifier une stratégie de déploiement de serveur accès à distance  
Les décisions que vous devez prendre lorsque vous planifiez de déployer votre serveur d’accès à distance sont les suivantes :  
  
-   **Topologie de réseau**  
  
    Il existe deux topologies disponibles lors du déploiement d’un serveur d’accès à distance :  
  
    -   **Deux cartes**: Avec deux cartes réseau, accès à distance peut être configuré avec une carte réseau connectée directement à Internet et l’autre connectée au réseau interne. Ou vous pouvez également, le serveur est installé derrière un périphérique de périmètre, tel qu’un pare-feu ou un routeur. Dans ce réseau d’une configuration carte est connectée au réseau de périmètre, et l’autre est connectée au réseau interne.  
  
    -   **Carte réseau unique**: Dans cette configuration de l’accès à distance le serveur est installé derrière un périphérique de périmètre, tel qu’un pare-feu ou un routeur. La carte réseau est connectée au réseau interne.  

-   **Cartes réseau**  
  
    Assistant d’installation du serveur d’accès à distance détecte automatiquement les cartes réseau qui sont configurés sur le serveur d’accès à distance. Vous devez vous assurer que les cartes appropriées sont sélectionnées.  
  
-   **Certificat IP-HTTPS**  
  
    L'Assistant Installation du serveur d'accès à distance détecte automatiquement un certificat adapté à la connexion IP-HTTPS. Le nom de sujet du certificat que vous sélectionnez doit correspondre à l'adresse ConnectTo. Si vous utilisez des certificats auto-signés, vous pouvez choisir d’utiliser un certificat qui est créé automatiquement par le serveur d’accès à distance.  
  
-   **Préfixes IPv6**  
  
    Si l'Assistant Installation du serveur d'accès à distance détecte qu'IPv6 a été déployé sur les cartes réseau, il renseigne automatiquement les préfixes IPv6 pour le réseau interne, un préfixe IPv6 à affecter aux ordinateurs clients DirectAccess et un préfixe IPv6 à affecter aux ordinateurs clients VPN. Si les préfixes générés automatiquement ne sont pas corrects pour votre infrastructure IPv6 ou ISATAP native, vous devez les modifier manuellement.  
  
-   **Authentification**  
  
    Vous pouvez choisir une des méthodes suivantes pour authentifier les clients DirectAccess au serveur d’accès à distance :  
  
    -   **Authentification utilisateur**: Vous pouvez permettre aux utilisateurs de s'authentifier à l'aide des informations d'identification Active Directory ou d'une authentification à deux facteurs.  
  
    -   **L’authentification d’ordinateur**: Vous pouvez configurer l’authentification d’ordinateur pour utiliser des certificats. Ou le serveur d’accès à distance peut agir en tant que proxy pour l’authentification Kerberos sans exiger de certificats. 
  
    -   **Les clients Windows 7** par défaut, les ordinateurs clients qui exécutent Windows 7 ne peut pas se connecter à un déploiement de l’accès à distance exécutant Windows Server 2012. Si vous avez des clients exécutant Windows 7 dans votre organisation qui requièrent l’accès à distance aux ressources internes, vous pouvez les autoriser à se connecter. Tout ordinateur client que vous voulez autoriser à accéder aux ressources internes doit être membre d'un groupe de sécurité que vous spécifiez dans l'Assistant Installation des clients DirectAccess.  
  
        > [!NOTE]  
        > Autorisant les clients qui exécutent 7 Windows pour se connecter à l’aide de DirectAccess vous oblige à utiliser l’authentification de certificat d’ordinateur.  
  
-   **Configuration de VPN**  
  
    Avant de configurer l’accès à distance, décidez si vous vous apprêtez à fournir l’accès VPN à des clients distants. Vous devez fournir l’accès VPN si vous avez des ordinateurs clients dans votre organisation qui ne prennent pas en charge la connectivité DirectAccess (par exemple, ils ne sont pas gérés ou ils exécutent un système d’exploitation pour lequel DirectAccess n’est pas pris en charge). Assistant d’installation du serveur d’accès à distance vous permet de configurer la façon dont les adresses IP sont affectées (à l’aide de DHCP ou à partir d’un pool d’adresses statiques) et la manière dont les clients VPN sont authentifiés (en utilisant Active Directory ou un serveur RADIUS).  
  
## <a name="plan-the-infrastructure-servers-configurations"></a>Planifier les configurations de serveurs d’infrastructure  
Accès à distance requiert trois types de serveurs d’infrastructure :  
  
-   **Serveur d’emplacement réseau**  
  
-   **Serveurs DNS** 
  
-   **Serveurs d’administration** 
  
## <a name="see-also"></a>Voir aussi  
  
-   [Étape 1 : Planifier l’infrastructure d’accès à distance](Step-1-Plan-the-Remote-Access-Infrastructure.md)  
  


