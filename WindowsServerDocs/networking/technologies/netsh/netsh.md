---
title: Environnement réseau (Netsh)
description: Cette rubrique fournit une vue d’ensemble de l’utilitaire de ligne de commande de l’environnement réseau (Netsh) dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: ac440c8187424733c0636cf2013342458f08d2f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405551"
---
# <a name="network-shell-netsh"></a>@No__t de l’interpréteur de commandes réseau-0Netsh @ no__t-1

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Network Shell (Netsh) est un utilitaire de ligne de commande qui vous permet de configurer et d’afficher l’état de différents rôles et composants de serveur de communications réseau après leur installation sur des ordinateurs exécutant Windows Server 2016.

Certaines technologies clientes, telles que Dynamic Host Configuration Protocol \(DHCP @ no__t-1 client et BranchCache, fournissent également des commandes netsh qui vous permettent de configurer des ordinateurs clients qui exécutent Windows 10.

Dans la plupart des cas, les commandes netsh fournissent la même fonctionnalité que celle disponible lorsque vous utilisez la console MMC (Microsoft Management Console) \(MMC @ no__t-1 Snap @ no__t-2in pour chaque rôle de serveur réseau ou fonctionnalité de mise en réseau. Par exemple, vous pouvez configurer le serveur de stratégie réseau \(NPS @ no__t-1 en utilisant le composant logiciel enfichable MMC NPS ou les commandes netsh dans le contexte **netsh nps** .

En outre, il existe des commandes netsh pour les technologies réseau, telles que pour IPv6, le pont réseau et l’appel de procédure distante \(RPC @ no__t-1, qui ne sont pas disponibles dans Windows Server en tant que composant logiciel enfichable MMC.

>[!IMPORTANT]
>Il est recommandé d’utiliser Windows PowerShell pour gérer les technologies de mise en réseau dans [Windows Server 2016 et Windows 10](https://technet.microsoft.com/library/mt156917.aspx) plutôt que Network Shell. Network Shell est toutefois inclus pour la compatibilité avec vos scripts, et son utilisation est prise en charge.

## <a name="network-shell-netsh-technical-reference"></a>Informations techniques de référence sur l’environnement réseau (Netsh)

La référence technique netsh fournit une référence de commande netsh complète, y compris la syntaxe, les paramètres et des exemples pour les commandes netsh. Vous pouvez utiliser la référence technique netsh pour créer des scripts et des fichiers de commandes à l’aide des commandes netsh pour la gestion locale ou à distance des technologies réseau sur les ordinateurs exécutant Windows Server 2016 et Windows 10.  
  
### <a name="content-availability"></a>Disponibilité du contenu  
  
La référence technique de l’interpréteur de commandes réseau est disponible au téléchargement dans l’aide de Windows \( *. chm @ no__t-1 format de la Galerie TechNet : [Référence technique netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---
