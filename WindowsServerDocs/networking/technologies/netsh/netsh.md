---
title: Environnement réseau (Netsh)
description: Cette rubrique fournit une vue d’ensemble de l’utilitaire de ligne de commande d’environnement réseau (netsh) dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a23d60bc3f181fee62ade105e546bbb7161c133
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="network-shell-netsh"></a>Réseau Shell \(Netsh\)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Environnement réseau (netsh) est un utilitaire de ligne de commande qui vous permet de configurer et d’afficher l’état des divers rôles de serveurs de communication réseau et des composants une fois qu’ils sont installés sur les ordinateurs exécutant Windows Server 2016.

>[!NOTE]
>Outre cette rubrique, le contenu d’interface réseau suivant est disponible.
>
> - [Syntaxe de commande Netsh, contextes et mise en forme](netsh-contexts.md)
> - [Fichier de commandes Network Shell (Netsh) exemple](netsh-wins.md)
> - [Référence technique de netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc) 

Certaines technologies de client, telles que Dynamic Host Configuration Protocol \(DHCP\) client et BranchCache, fournissent également des commandes netsh qui vous permettent de configurer les ordinateurs clients qui exécutent Windows 10.

Dans la plupart des cas, les commandes netsh offrent la même fonctionnalité est disponible lorsque vous utilisez la Console de gestion Microsoft \(MMC\) snap\-dans pour chaque fonctionnalité de rôle ou de mise en réseau server mise en réseau. Par exemple, vous pouvez configurer \(NPS\) serveur NPS à l’aide du composant logiciel enfichable MMC NPS ou les commandes netsh dans le **netsh nps** contexte.

En outre, il existe des commandes netsh pour les technologies de réseau, tels que pour IPv6, \(RPC\) appel de procédure distante et pont réseau, qui ne sont pas disponibles dans Windows Server comme un composant logiciel enfichable MMC.

>[!IMPORTANT]
>Il est recommandé d’utiliser Windows PowerShell pour gérer des technologies de mise en réseau dans [Windows Server 2016 et Windows 10](https://technet.microsoft.com/library/mt156917.aspx) au lieu de l’environnement réseau. Environnement réseau est inclus pour la compatibilité avec vos scripts, toutefois, et son utilisation est pris en charge.

## <a name="network-shell-netsh-technical-reference"></a>Référence technique de Network Shell (Netsh)

La référence technique de Netsh fournit une référence des commandes netsh complète, y compris la syntaxe, les paramètres et les exemples de commandes netsh. Vous pouvez utiliser la référence technique de Netsh pour créer des scripts et fichiers de commandes à l’aide des commandes netsh pour la gestion locale ou distante des technologies de réseau sur les ordinateurs exécutant Windows Server 2016 et Windows 10.  
  
### <a name="content-availability"></a>Disponibilité du contenu  
  
Référence technique interface réseau est disponible en téléchargement dans le format de \(*.chm\) l’aide de Windows à partir de la Galerie TechNet: [référence technique de Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  

