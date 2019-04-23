---
title: Configurations non prises en charge DirectAccess
description: Cette rubrique fournit une liste des configurations non prises en charge de DirectAccess dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-da
ms.topic: article
ms.assetid: 23d05e61-95c3-4e70-aa83-b9a8cae92304
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 49fa86883e6a590ac8f7dcf0c724f87af419f88e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828850"
---
# <a name="directaccess-unsupported-configurations"></a>Configurations non prises en charge DirectAccess

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Avant de commencer votre déploiement pour éviter d’avoir à démarrer à nouveau votre déploiement, passez en revue la liste suivante des configurations non prises en charge de DirectAccess.  

## <a name="bkmk_frs"></a>Distribution de Service de réplication de fichiers d’objets de stratégie de groupe (réplications SYSVOL)  
Ne déployez pas de DirectAccess dans les environnements où vos contrôleurs de domaine sont en cours d’exécution du Service de réplication de fichiers (FRS) pour la distribution d’objets de stratégie de groupe (réplications SYSVOL). Déploiement de DirectAccess n’est pas pris en charge lorsque vous utilisez FRS.  
  
Vous utilisez FRS si vous avez des contrôleurs de domaine qui exécutent Windows Server 2003 ou Windows Server 2003 R2. En outre, vous utilisez peut-être FRS si vous avez utilisé précédemment Windows 2000 Server ou les contrôleurs de domaine Windows Server 2003 et que vous avez migré jamais la réplication SYSVOL de FRS pour Distributed File System Replication (DFS-R).  
  
Si vous déployez DirectAccess avec la réplication FRS SYSVOL, vous risquez de la suppression involontaire de stratégie de groupe DirectAccess des objets qui contiennent le serveur DirectAccess et les informations de configuration de client. Si ces objets sont supprimés, votre déploiement DirectAccess subira une panne, et les ordinateurs clients qui utilisent DirectAccess ne sera pas en mesure de se connecter à votre réseau.  
  
Si vous envisagez de déployer DirectAccess, vous devez utiliser des contrôleurs de domaine qui exécutent les systèmes d’exploitation Windows Server 2003 R2 au plus tard, et vous devez utiliser DFS-R.  
  
Pour plus d’informations sur la migration de FRS vers DFS-R, consultez le [Guide de Migration de réplication SYSVOL : Réplication FRS vers DFS](https://technet.microsoft.com/library/dd640019(v=ws.10).aspx).  
  
## <a name="bkmk_nap"></a>Protection d’accès réseau pour les clients DirectAccess  
Protection d’accès réseau (NAP) est utilisé pour déterminer si les ordinateurs clients distants répondent aux stratégies informatiques avant de pouvoir accéder au réseau d’entreprise. NAP a été déconseillée dans Windows Server 2012 R2 et n’est pas inclus dans Windows Server 2016. Pour cette raison, à partir d’un nouveau déploiement de DirectAccess avec NAP n’est pas recommandée. Une autre méthode de contrôle de point de terminaison pour la sécurité des clients DirectAccess est recommandée.  
  
## <a name="bkmk_multi"></a>Prise en charge multisite pour les clients Windows 7  
Lorsque DirectAccess est configuré dans un déploiement multisite, Windows 10&reg;, Windows&reg; 8.1 et Windows&reg; 8 clients ont la capacité de se connecter au site le plus proche.  Windows 7&reg; ordinateurs clients n’ont pas la même fonctionnalité. Sélection d’un site pour les clients Windows 7 est définie pour un site particulier au moment de la configuration de la stratégie, et ces clients seront connecte toujours à ce site désigné, quel que soit leur emplacement.  
  
## <a name="bkmk_user"></a>Contrôle d’accès utilisateur  
Les stratégies de DirectAccess sont ordinateur en fonction, pas en fonction de l’utilisateur. Spécification des stratégies d’utilisateur DirectAccess pour contrôler l’accès au réseau d’entreprise n’est pas prise en charge.  
  
## <a name="bkmk_policy"></a>Personnalisation de la stratégie de DirectAccess  
DirectAccess peut être configuré à l’aide de l’Assistant Installation DirectAccess, la console de gestion de l’accès à distance ou les applets de commande PowerShell pour Windows l’accès à distance. À l’aide de n’importe quel moyen autre que l’Assistant Installation DirectAccess pour configurer DirectAccess, telles que modifier directement les objets de stratégie de groupe DirectAccess ou modifier manuellement les paramètres de stratégie par défaut sur le serveur ou client, n’est pas pris en charge. Ces modifications peuvent entraîner une configuration inutilisable.  
  
## <a name="bkmk_kerb"></a>Authentification KerbProxy  
Lorsque vous configurez un serveur DirectAccess avec l’Assistant Mise en route, le serveur DirectAccess est automatiquement configuré pour utiliser l’authentification KerbProxy pour ordinateur et utilisateur. Pour cette raison, vous devez uniquement utiliser l’Assistant Mise en route pour les déploiements de site unique où Windows 10&reg;, Windows 8.1, ou si les clients Windows 8 sont déployés.  
  
En outre, les fonctionnalités suivantes ne doivent pas être utilisées avec l’authentification KerbProxy :  
  
-   L’équilibrage à l’aide d’un équilibreur de charge externe ou d’une charge de Windows   
    Équilibrage  
  
-   Authentification à deux facteurs où les cartes à puce ou un mot de passe à usage unique (OTP) sont requises  
  
Les plans de déploiement suivants ne sont pas pris en charge si vous activez l’authentification KerbProxy :  
  
-   Multisite.  
  
-   DirectAccess prend en charge pour les clients Windows 7.  
  
-   Le tunneling forcé. Pour vous assurer que l’authentification KerbProxy n’est pas activée lorsque vous utilisez le tunneling forcé, configurez les éléments suivants lors de l’exécution de l’Assistant :  
  
    -   Activer Forcer le mode tunneling  
  
    -   Permettre aux clients DirectAccess pour Windows 7  
  
> [!NOTE]  
> Pour les déploiements précédents, vous devez utiliser l’Assistant de Configuration avancée, qui utilise une configuration de deux tunnels avec une ordinateur et utilisateur authentification par certificat. Pour plus d’informations, consultez [déployer un serveur DirectAccess unique avec des paramètres avancés](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
## <a name="bkmk_isa"></a>Utilisation d’ISATAP  
ISATAP est une technologie de transition qui fournit une connectivité IPv6 dans IPv4 uniquement des réseaux d’entreprise. Il est limité aux petites et moyennes entreprises un déploiement de serveur DirectAccess unique, et elle permet la gestion à distance des clients DirectAccess. Si ISATAP est deployedin multisite, l’équilibrage de charge ou un environnement à plusieurs domaines, vous devez le supprimer ou déplacez-le vers un déploiement IPv6 natif avant de configurer DirectAccess.  
  
## <a name="bkmk_iphttps"></a>IPHTTPS et configuration de point de terminaison de mot de passe à usage unique (OTP)  
Lorsque vous utilisez IPHTTPS, la connexion IP-HTTPS doit se terminer sur le serveur DirectAccess, pas sur un autre appareil, par exemple un équilibreur de charge. De même, la connexion de couche SSL (Secure Sockets) hors-bande qui est créée lors de l’authentification de mot de passe à usage unique (OTP) doit se terminer sur le serveur DirectAccess. Tous les appareils entre les points de terminaison de ces connexions doivent être configurés en mode Pass-through.  
  
## <a name="bkmk_ft"></a>Forcer le Tunnel avec l’authentification OTP  
Ne déployez pas un serveur DirectAccess avec l’authentification à deux facteurs avec l’OTP et le tunneling forcé, ou l’authentification OTP échouera. Une connexion de couche SSL (Secure Sockets) out-of-band est requise entre le serveur DirectAccess et le client DirectAccess. Cette connexion requiert une exemption pour envoyer le trafic en dehors du tunnel DirectAccess. Dans une configuration de forcer le Tunnel, tout le trafic doit passer par un tunnel DirectAccess, et aucune exemption n’est autorisée une fois le tunnel établi. Pour cette raison, il n'est pas prise en charge l’authentification unique dans une configuration de tunneling forcé.  
  
## <a name="bkmk_rodc"></a>Déploiement de DirectAccess avec un contrôleur de domaine en lecture seule  
Les serveurs DirectAccess doivent avoir accès à un contrôleur de domaine de lecture-écriture et ne fonctionnent pas correctement avec un contrôleur de domaine en lecture seule (RODC).  
  
Un contrôleur de domaine de lecture-écriture est requis pour de nombreuses raisons, notamment les suivantes :  
  
-   Sur le serveur DirectAccess, un contrôleur de domaine de lecture-écriture est nécessaire pour ouvrir la distant accès Console (MMC).  
  
-   Le serveur DirectAccess doit à la fois lire et écrire dans le client DirectAccess et le serveur DirectAccess objets de stratégie de groupe (GPO).  
  
-   Le serveur DirectAccess lit et écrit dans le client GPO en particulier à partir de l’émulateur de contrôleur de domaine principal (PDCe).  
  
En raison de ces exigences, ne déployez pas de DirectAccess avec un RODC.  
  


