---
title: Configurations non prises en charge DirectAccess
description: Cette rubrique fournit une liste des configurations DirectAccess non prises en charge dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 23d05e61-95c3-4e70-aa83-b9a8cae92304
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e652083d4accf90b542a16d51e314299303954f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388840"
---
# <a name="directaccess-unsupported-configurations"></a>Configurations non prises en charge DirectAccess

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Passez en revue la liste suivante de configurations DirectAccess non prises en charge avant de commencer votre déploiement afin d’éviter d’avoir à redémarrer votre déploiement.  

## <a name="bkmk_frs"></a>Distribution du service de réplication de fichiers (FRS) des objets de stratégie de groupe (réplications SYSVOL)  
Ne déployez pas DirectAccess dans les environnements où vos contrôleurs de domaine exécutent le service de réplication de fichiers (FRS) pour la distribution d’objets stratégie de groupe (réplications SYSVOL). Le déploiement de DirectAccess n’est pas pris en charge lorsque vous utilisez FRS.  
  
Vous utilisez le service FRS si vous avez des contrôleurs de domaine qui exécutent Windows Server 2003 ou Windows Server 2003 R2. En outre, vous pouvez utiliser le service FRS si vous avez déjà utilisé des contrôleurs de domaine Windows 2000 Server ou Windows Server 2003 et que vous n’avez jamais migré la réplication SYSVOL de FRS vers système de fichiers DFS réplication (DFS-R).  
  
Si vous déployez DirectAccess avec la réplication SYSVOL FRS, vous risquez la suppression accidentelle des objets DirectAccess stratégie de groupe qui contiennent les informations de configuration du client et du serveur DirectAccess. Si ces objets sont supprimés, votre déploiement DirectAccess subit une panne et les ordinateurs clients qui utilisent DirectAccess ne seront pas en mesure de se connecter à votre réseau.  
  
Si vous envisagez de déployer DirectAccess, vous devez utiliser des contrôleurs de domaine qui exécutent des systèmes d’exploitation ultérieurs à Windows Server 2003 R2, et vous devez utiliser DFS-R.  
  
Pour plus d’informations sur la migration de FRS vers DFS-R, consultez le Guide de migration de la réplication @no__t 0SYSVOL : FRS à réplication DFS](https://technet.microsoft.com/library/dd640019(v=ws.10).aspx).  
  
## <a name="bkmk_nap"></a>Protection d’accès réseau pour les clients DirectAccess  
La protection d’accès réseau (NAP) est utilisée pour déterminer si les ordinateurs clients distants respectent les stratégies informatiques avant de se voir accorder l’accès au réseau d’entreprise. La protection d’accès réseau (NAP) a été dépréciée dans Windows Server 2012 R2 et n’est pas incluse dans Windows Server 2016. Pour cette raison, le démarrage d’un nouveau déploiement de DirectAccess avec NAP n’est pas recommandé. Il est recommandé d’avoir une méthode de contrôle de point de terminaison différente pour la sécurité des clients DirectAccess.  
  
## <a name="bkmk_multi"></a>Prise en charge multisite pour les clients Windows 7  
Lorsque DirectAccess est configuré dans un déploiement multisite, les clients Windows 10 @ no__t-0, Windows @ no__t-1 8,1 et Windows @ no__t-2 8 peuvent se connecter au site le plus proche.  Les ordinateurs clients Windows 7 @ no__t-0 n’ont pas la même fonctionnalité. La sélection de site pour les clients Windows 7 est définie sur un site particulier au moment de la configuration de la stratégie, et ces clients se connectent toujours à ce site désigné, quel que soit leur emplacement.  
  
## <a name="bkmk_user"></a>Contrôle d’accès en fonction de l’utilisateur  
Les stratégies DirectAccess sont basées sur l’ordinateur et non sur l’utilisateur. La spécification de stratégies d’utilisateur DirectAccess pour contrôler l’accès au réseau d’entreprise n’est pas prise en charge.  
  
## <a name="bkmk_policy"></a>Personnalisation de la Stratégie DirectAccess  
DirectAccess peut être configuré à l’aide de l’Assistant Installation DirectAccess, de la console de gestion de l’accès à distance ou des applets de commande Windows PowerShell pour l’accès à distance. N’est pas pris en charge à l’aide de l’Assistant Installation DirectAccess pour configurer DirectAccess, par exemple la modification directe des objets stratégie de groupe DirectAccess ou la modification manuelle des paramètres de stratégie par défaut sur le serveur ou le client. Ces modifications peuvent entraîner une configuration inutilisable.  
  
## <a name="bkmk_kerb"></a>Authentification KerbProxy  
Quand vous configurez un serveur DirectAccess à l’aide de l’Assistant Prise en main, le serveur DirectAccess est automatiquement configuré pour utiliser l’authentification KerbProxy pour l’authentification de l’ordinateur et de l’utilisateur. Pour cette raison, vous devez utiliser uniquement l’Assistant Prise en main pour les déploiements sur un site où seuls les clients Windows 10 @ no__t-0, Windows 8.1 ou Windows 8 sont déployés.  
  
En outre, les fonctionnalités suivantes ne doivent pas être utilisées avec l’authentification KerbProxy :  
  
-   Équilibrage de charge à l’aide d’un équilibrage de charge externe ou d’une charge Windows   
    Équilibreur  
  
-   Authentification à deux facteurs où les cartes à puce ou un mot de passe à usage unique sont requis  
  
Les plans de déploiement suivants ne sont pas pris en charge si vous activez l’authentification KerbProxy :  
  
-   Multisite.  
  
-   Prise en charge DirectAccess pour les clients Windows 7.  
  
-   Forcer le tunneling. Pour vous assurer que l’authentification KerbProxy n’est pas activée lorsque vous utilisez le tunneling forcé, configurez les éléments suivants lors de l’exécution de l’Assistant :  
  
    -   Activer Forcer le mode tunneling  
  
    -   Activer DirectAccess pour les clients Windows 7  
  
> [!NOTE]  
> Pour les déploiements précédents, vous devez utiliser l’Assistant Configuration avancée, qui utilise une configuration à deux tunnels avec une authentification de l’utilisateur et de l’ordinateur basée sur les certificats. Pour plus d’informations, consultez [déployer un serveur DirectAccess unique avec des paramètres avancés](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
## <a name="bkmk_isa"></a>Utilisation d’ISATAP  
ISATAP est une technologie de transition qui fournit une connectivité IPv6 dans des réseaux d’entreprise IPv4 uniquement. Elle est limitée aux petites et moyennes entreprises avec un seul déploiement de serveur DirectAccess et permet la gestion à distance des clients DirectAccess. Si ISATAP est déployé dans un environnement multisite, d’équilibrage de charge ou multidomaine, vous devez le supprimer ou le déplacer vers un déploiement IPv6 natif avant de configurer DirectAccess.  
  
## <a name="bkmk_iphttps"></a>Configuration du point de terminaison de mot de passe à usage unique et IPHTTPS  
Quand vous utilisez IPHTTPS, la connexion IPHTTPS doit se terminer sur le serveur DirectAccess, et non sur un autre appareil, tel qu’un équilibreur de charge. De même, la connexion de protocole SSL hors bande (SSL) créée pendant l’authentification par mot de passe à usage unique doit se terminer sur le serveur DirectAccess. Tous les appareils entre les points de terminaison de ces connexions doivent être configurés en mode relais.  
  
## <a name="bkmk_ft"></a>Forcer le tunnel avec authentification par mot de passe à usage unique  
Ne déployez pas de serveur DirectAccess avec l’authentification à deux facteurs avec un mot de passe à usage unique et le tunneling forcé, ou l’authentification par mot de passe à usage unique échouera. Une connexion de protocole SSL hors bande (SSL) est nécessaire entre le serveur DirectAccess et le client DirectAccess. Cette connexion nécessite une exemption pour envoyer le trafic en dehors du tunnel DirectAccess. Dans une configuration de tunnel forcé, tout le trafic doit transiter via un tunnel DirectAccess, et aucune exemption n’est autorisée une fois le tunnel établi. Pour cette raison, il n’est pas possible d’avoir une authentification par mot de passe à usage unique dans une configuration de tunnel forcé.  
  
## <a name="bkmk_rodc"></a>Déploiement de DirectAccess avec un contrôleur de domaine en lecture seule  
Les serveurs DirectAccess doivent avoir accès à un contrôleur de domaine en lecture-écriture et ne fonctionnent pas correctement avec un contrôleur de domaine en lecture seule (RODC).  
  
Un contrôleur de domaine en lecture-écriture est requis pour de nombreuses raisons, notamment les suivantes :  
  
-   Sur le serveur DirectAccess, un contrôleur de domaine en lecture-écriture est requis pour ouvrir la console MMC (Microsoft Management Console) d’accès à distance.  
  
-   Le serveur DirectAccess doit lire et écrire sur le client DirectAccess et le serveur DirectAccess stratégie de groupe les objets de stratégie de groupe.  
  
-   Le serveur DirectAccess lit et écrit dans l’objet de stratégie de groupe client, spécifiquement à partir de l’émulateur de contrôleur de domaine principal (émulateur).  
  
En raison de ces exigences, ne déployez pas DirectAccess avec un RODC.  
  


