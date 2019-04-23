---
title: Installation minimale de Windows Server
description: Comment obtenir et installer une installation Server Core sur Windows Server 2019, Windows Server 2016 ou Windows Server (canal semi-annuel).
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 1/04/2019
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d22818c-fbb7-487a-bb82-81ef0a3f7ede
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: d99cd0b028d08d5c3247541ce3a868676b60693d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869020"
---
# <a name="install-server-core"></a>Installation minimale de Windows Server

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel)
  
Lorsque vous installez Windows Server pour la première fois, vous avez les options d’installation suivantes :

>[!NOTE]
> Dans la liste suivante, les éditions sans « Expérience de bureau » sont les options d’installation minimale

-   Windows Server Standard
-   Windows Server Standard avec Expérience de bureau
-   Windows Server Datacenter
-   Windows Server Datacenter avec Expérience de bureau

Lorsque vous installez Windows Server (canal semi-annuel), y compris les versions 1709, 1803 et 1809, vous disposez des options d’installation suivantes :

-   Windows Server Standard 
-   Windows Server Datacenter

L’option d’installation minimale requiert moins d’espace disque et réduit la surface exposée aux attaques. Pour toutes ces raisons, nous vous recommandons de choisir l’installation minimale si vous n’avez pas besoin d’utiliser les éléments d’interface utilisateur et les outils de gestion graphiques que l’option Serveur avec Expérience de bureau fournit en plus. Si vous pensez avoir besoin des éléments d’interface utilisateur supplémentaires, voir [Installer un serveur avec Expérience de bureau](Getting-Started-with-Server-with-Desktop-Experience.md). 

Avec l’option d’installation minimale, l’interface utilisateur standard (c’est-à-dire l’Expérience de bureau) n’est pas installée. Vous gérez donc le serveur à l’aide de la ligne de commande, de Windows PowerShell ou des méthodes de gestion à distance.

>[!NOTE]
>
>À la différence des versions précédentes de Windows Server, vous ne pouvez pas basculer entre l’installation minimale et l’installation avec Expérience utilisateur après l’installation. Si vous procédez à l’installation minimale et décidez ultérieurement d’utiliser l’installation avec Expérience utilisateur, vous devez procéder à une nouvelle installation.

**Interface utilisateur :** invite de commandes

**Installation, configuration et désinstallation des rôles serveur en local :** à une invite de commandes avec Windows PowerShell.

**Installation, configuration et désinstallation des rôles de serveur à distance à partir d’un ordinateur client de Windows (ou un serveur avec la fonctionnalité Expérience Bureau installée) :** avec le Gestionnaire de serveur, outils d’Administration de serveur distant (RSAT), Windows PowerShell ou Windows Admin Center .

>[!NOTE]
>
>Pour les outils d’administration de serveur distant, vous devez utiliser la version Windows 10.
>La console MMC (Microsoft Management Console) n’est pas disponible en local.

**Exemples de rôles serveur disponibles :**

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
- Services WSUS (Windows Server Update Services)
- Serveur AD RMS (Active Directory Rights Management Server)
- Serveur de routage et d’accès à distance et les sous-rôles suivants :
- Service Broker pour les connexions des services Bureau à distance
- Licences
- Virtualisation
- Services d'activation en volume

Pour les rôles ne figurant ne pas Server Core, consultez [rôles, Services de rôle et fonctionnalités pas dans Windows Server - Server Core](../administration/server-core/server-core-removed-roles.md).

## <a name="installing-on-windows-server-2019-or-windows-server-2016"></a>L’installation sur Windows Server 2019 ou Windows Server 2016

Pour les étapes d’installation générale et les options pour Windows Server (canal de maintenance Long terme), consultez [Windows Server Installation and Upgrade](installation-and-upgrade.md).

## <a name="installing-on-windows-server-semi-annual-channel"></a>Installation sur Windows Server (canal semi-annuel)

Étapes d’installation de Windows Server (canal semi-annuel) sont les mêmes que pour l’installation des versions précédentes de Windows Server (à partir d’un. Image ISO), avec les exceptions suivantes :
- Les mises à niveau à partir de versions antérieures de Windows Server vers Windows Server, version 1709 ne sont pas prises en charge. Une nouvelle installation est toujours nécessaire.
   Cela signifie que lorsque vous exécutez setup.exe à partir du bureau d’un ordinateur Windows, l’expérience d’installation n’autorise pas l’option de mise à niveau (il est grisé).
- Il n’existe aucune version d’évaluation pour Windows Server (canal semi-annuel)
- Il n’existe pas de version OEM ou commerciale. Windows Server (canal semi-annuel) peut uniquement être sous licence Software Assurance ou la fidélité des programmes.

Pour obtenir Windows Server, version 1709, voir [Présentation de Windows Server, version 1709](get-started-with-1709.md).

Pour obtenir Windows Server version 1803, consultez [présentation de Windows Server, version 1803](get-started-with-1803.md).

Pour voir quelles sont les nouveautés dans Windows Server, version 1809, consultez [What ' s New in Windows Server version 1809](whats-new-in-windows-server-1809.md)
