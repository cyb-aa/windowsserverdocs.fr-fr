---
title: Environnement réseau (Netsh)
description: Cette rubrique fournit une vue d’ensemble de l’utilitaire de ligne de commande Network Shell (netsh) dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 038b21783ef1d27619657ec3f9a472cf3caba68e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849360"
---
# <a name="network-shell-netsh"></a>Réseau Shell \(Netsh\)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Environnement réseau (netsh) est un utilitaire de ligne de commande qui vous permet de configurer et afficher l’état de différents composants et les rôles de serveur des communications réseau après leur installation sur les ordinateurs exécutant Windows Server 2016.

Certaines technologies de client, telles que Dynamic Host Configuration Protocol \(DHCP\) client et BranchCache, fournissent également des commandes netsh qui vous permettent de configurer des ordinateurs clients qui exécutent Windows 10.

Dans la plupart des cas, les commandes netsh fournissent les mêmes fonctionnalités que celui qui est disponible lorsque vous utilisez la Console de gestion Microsoft \(MMC\) aligner\-dans pour chaque fonctionnalité de rôle ou de mise en réseau server mise en réseau. Par exemple, vous pouvez configurer le serveur de stratégie réseau \(NPS\) en utilisant le composant logiciel enfichable MMC NPS ou les commandes netsh dans le **netsh nps** contexte.

En outre, voici les commandes netsh pour les technologies de réseau, comme pour IPv6, pont réseau et Remote Procedure Call \(RPC\), qui ne sont pas disponibles dans Windows Server comme un composant logiciel enfichable MMC.

>[!IMPORTANT]
>Il est recommandé d’utiliser Windows PowerShell pour gérer des technologies de mise en réseau dans [Windows Server 2016 et Windows 10](https://technet.microsoft.com/library/mt156917.aspx) plutôt que de l’environnement réseau. Environnement réseau est inclus pour la compatibilité avec vos scripts, toutefois, et son utilisation est prise en charge.

## <a name="network-shell-netsh-technical-reference"></a>Network Shell (Netsh) – référence technique

La référence technique Netsh fournit une référence de commande netsh complète, y compris la syntaxe, des paramètres et des exemples de commandes netsh. Vous pouvez utiliser la référence technique de Netsh pour générer des scripts et fichiers de commandes à l’aide des commandes netsh pour la gestion locale ou distante de technologies de réseau sur les ordinateurs exécutant Windows Server 2016 et Windows 10.  
  
### <a name="content-availability"></a>Disponibilité du contenu  
  
Le réseau Shell – référence technique est disponible au téléchargement dans l’aide de Windows \(*.chm\) format à partir de la Galerie TechNet : [Référence technique de netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---
