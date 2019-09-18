---
title: Installation minimale de Windows Server
description: Comment obtenir et effectuer une installation minimale sur Windows Server 2019, Windows Server 2016 ou Windows Server (canal semi-annuel).
ms.prod: windows-server-threshold
ms.date: 05/21/2019
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d22818c-fbb7-487a-bb82-81ef0a3f7ede
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 542eeea4bfae53defc2624b3b56a7db5ae454c2c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868445"
---
# <a name="install-server-core"></a>Installation minimale de Windows Server

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (Canal semi-annuel)
  
Lorsque vous installez Windows Server pour la première fois, vous disposez des options d’installation suivantes :

>[!NOTE]
> Dans la liste suivante, les éditions sans « Expérience de bureau » sont les options d’installation minimale

-   Windows Server Standard
-   Windows Server Standard avec Expérience de bureau
-   Windows Server Datacenter
-   Windows Server Datacenter avec Expérience de bureau

Lorsque vous installez Windows Server (canal semi-annuel), vous disposez des options d’installation suivantes :

-   Windows Server Standard 
-   Windows Server Datacenter

L’option d’installation minimale requiert moins d’espace disque et réduit la surface exposée aux attaques. Pour toutes ces raisons, nous vous recommandons de choisir l’installation minimale si vous n’avez pas besoin d’utiliser les éléments d’interface utilisateur et les outils de gestion graphiques que l’option Serveur avec Expérience de bureau fournit en plus. Si vous pensez avoir besoin des éléments d’interface utilisateur supplémentaires, voir [Installer un serveur avec Expérience de bureau](Getting-Started-with-Server-with-Desktop-Experience.md). 

Avec l’option d’installation minimale, l’interface utilisateur standard (c’est-à-dire l’Expérience de bureau) n’est pas installée. Vous gérez donc le serveur à l’aide de la ligne de commande, de Windows PowerShell ou des méthodes de gestion à distance.

>[!NOTE]
>
>À la différence des versions précédentes de Windows Server, vous ne pouvez pas basculer entre l’installation minimale et l’installation avec Expérience utilisateur après l’installation. Si vous procédez à l’installation minimale et décidez ultérieurement d’utiliser l’installation avec Expérience utilisateur, vous devez procéder à une nouvelle installation.

**Interface utilisateur :** invite de commandes

**Installation, configuration et désinstallation des rôles serveur en local :** à une invite de commandes avec Windows PowerShell.

**Installation, configuration et désinstallation des rôles serveur à distance à partir d’un ordinateur client Windows (ou un serveur avec Expérience de bureau installé :** les Outils d’administration de serveur distant (RSAT), Windows PowerShell ou Windows Admin Center.

>[!NOTE]
>
>Pour les outils d’administration de serveur distant, vous devez utiliser la version Windows 10.
>La console MMC (Microsoft Management Console) n’est pas disponible en local.

**Exemple de rôles serveur disponibles :**

- Services de certificats Active Directory
- Services de domaine Active Directory
- Serveur DHCP
- Serveur DNS
- Services de fichiers (dont le Gestionnaire de ressources du serveur de fichiers)
- Services AD LDS (Active Directory Lightweight Directory Services)
- Hyper-V
- Services d'impression et de numérisation de document
- Services de diffusion multimédia en continu
- Serveur Web (dont une partie d’ASP.NET)
- Services WSUS (Windows Server Update Services)
- Serveur AD RMS (Active Directory Rights Management Server)
- Serveur de routage et d’accès à distance et les sous-rôles suivants :
   - Service Broker pour les connexions des services Bureau à distance
   - Licences
   - Virtualisation
   - Services d'activation en volume

Pour les rôles ne figurant pas dans l’installation minimale, consultez [Rôles, services de rôle et fonctionnalités non inclus dans Windows Server - Installation minimale](../administration/server-core/server-core-removed-roles.md).

## <a name="installing-on-windows-server-2019-or-windows-server-2016"></a>Installation sur Windows Server 2019 ou Windows Server 2016

Pour les étapes et les options d’installation générales pour Windows Server (canal de maintenance à long terme), consultez [Installation et mise à niveau de Windows Server](installation-and-upgrade.md).

## <a name="installing-on-windows-server-semi-annual-channel"></a>Installation sur Windows Server (canal semi-annuel)

Les étapes d’installation de Windows Server (canal semi-annuel) sont identiques à celles des versions précédentes de Windows Server (à partir d’une image .ISO), avec toutefois les exceptions suivantes :

- Les mises à niveau à partir de versions antérieures de Windows Server vers Windows Server, version 1709 ne sont pas prises en charge. Une nouvelle installation est toujours nécessaire.
   Cela signifie que lorsque vous exécutez le fichier setup.exe à partir du bureau d’un ordinateur Windows, le programme d’installation n’autorise pas l’option de mise à niveau (grisée).
- Il n’existe pas de version d’évaluation de Windows Server (canal semi-annuel).
- Il n’existe pas de version OEM ou commerciale. Windows Server (canal semi-annuel) peut uniquement être fourni sous licence par le biais de Software Assurance ou de programmes de fidélité.

Pour plus d’informations sur le canal semi-annuel, consultez la [comparaison des canaux de maintenance](../get-started-19/servicing-channels-19.md).

Pour découvrir les nouveautés du canal semi-annuel de Windows Server, consultez [Nouveautés de Windows Server](whats-new-in-windows-server.md).
