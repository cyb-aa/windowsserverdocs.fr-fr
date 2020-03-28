---
title: Environnement réseau (Netsh)
description: Cette rubrique fournit une vue d’ensemble de l’utilitaire en ligne de commande NetSh (Network Shell) dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: d9103585d1868f586f169f01096c4d37961e7033
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316686"
---
# <a name="network-shell-netsh"></a>\(Netsh\) (Network Shell)

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

NetSh (Network Shell) est un utilitaire en ligne de commande qui vous permet de configurer et d’afficher l’état de différents composants et rôles serveurs de communication réseau une fois ceux-ci installés sur les ordinateurs exécutant Windows Server 2016.

Certaines technologies clientes, telles que le client \(DHCP\) (Dynamic Host Configuration Protocol) et BranchCache, fournissent également des commandes netsh avec lesquelles vous pouvez configurer les ordinateurs clients exécutant Windows 10.

Dans la plupart des cas, les commandes netsh fournissent les mêmes fonctionnalités que celles disponibles quand vous utilisez le composant logiciel enfichable \(MMC\) (Microsoft Management Console) pour chaque rôle de serveur réseau ou fonctionnalité réseau. Par exemple, vous pouvez configurer le serveur \(NPS\) (Network Policy Server) en utilisant le composant logiciel enfichable MMC NPS ou les commandes netsh dans le contexte de **netsh nps**.

En outre, il existe des commandes netsh pour les technologies réseau, par exemple pour IPv6, le pont réseau et \(RPC\) (appel de procédure distante), qui ne sont pas disponibles dans Windows Server en tant que composant logiciel enfichable MMC.

>[!IMPORTANT]
>Nous vous recommandons d’utiliser Windows PowerShell pour gérer les technologies réseau dans [Windows Server 2016 et Windows 10](https://technet.microsoft.com/library/mt156917.aspx) plutôt que Network Shell. Network Shell est toutefois inclus à des fins de compatibilité avec vos scripts, et son utilisation est prise en charge.

## <a name="network-shell-netsh-technical-reference"></a>Informations techniques de référence sur Netsh (Network Shell)

Les informations techniques de référence sur Netsh fournissent une référence complète des commandes netsh, notamment la syntaxe, les paramètres et des exemples de commandes netsh. Vous pouvez utiliser les informations techniques de référence sur Netsh afin de créer des scripts et des fichiers de commandes à l’aide de commandes netsh pour la gestion locale ou à distance des technologies réseau sur les ordinateurs exécutant Windows Server 2016 et Windows 10.  
  
### <a name="content-availability"></a>Disponibilité du contenu  
  
Vous pouvez télécharger les informations techniques de référence sur Network Shell au format de l’Aide Windows \(*.chm\) à partir de la galerie TechNet : [Informations techniques de référence sur Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---
