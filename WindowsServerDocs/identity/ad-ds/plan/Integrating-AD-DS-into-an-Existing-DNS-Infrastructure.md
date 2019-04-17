---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: "Intégration des services ADDS dans une Infrastructure DNS existante"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ab8d92a237a6d1fb623d9f4bb7dcc88561edf742
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>Intégration des services ADDS dans une Infrastructure DNS existante

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Si votre organisation possède déjà un service de serveur de système DNS (Domain Name) existant, le serveur DNS pour le propriétaire de Services de domaine ActiveDirectory (ADDS) doit fonctionner avec le propriétaire DNS pour votre organisation pour intégrer les services ADDS dans l’infrastructure existante. Cela implique la création d’un serveur DNS et la configuration du client DNS.  
  
## <a name="creating-a-dns-server-configuration"></a>Création d’une configuration de serveur DNS  
Lors de l’intégration des services ADDS avec un espace de noms DNS, nous vous recommandons d’effectuer les éléments suivants:  
  
-   Installer le service serveur DNS sur chaque contrôleur de domaine dans la forêt. Cela fournit une tolérance de panne si un des serveurs DNS n’est pas disponible. De cette façon, les contrôleurs de domaine est inutile s’appuyer sur les autres serveurs DNS pour la résolution de noms. Cela simplifie également l’environnement de gestion, car tous les contrôleurs de domaine ont une configuration uniforme.  
  
-   Configurer le contrôleur de domaine de racine de forêt ActiveDirectory pour héberger la zone DNS de la forêt ActiveDirectory.  
  
-   Configurer les contrôleurs de domaine pour chaque domaine régional héberger les zones DNS qui correspondent à leurs domaines ActiveDirectory.  
  
-   Configurer la zone contenant les enregistrements de localisateur de la forêt ActiveDirectory (autrement dit, la zone _msdcs.* nom_forêt* zone) à répliquer sur tous les serveurs DNS dans la forêt à l’aide de la partition d’annuaire de forêt DNS application.  
  
    > [!NOTE]  
    > Lorsque le service serveur DNS est installé avec l’Assistant domaine ActiveDirectory Services Installation (nous vous recommandons cette option), toutes les tâches précédentes sont exécutées automatiquement. Pour plus d’informations, voir [déploiement d’un domaine racine de forêt Windows Server2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
    > [!NOTE]  
    > Les services ADDS utilise les enregistrements de localisateur de forêt pour activer des partenaires de réplication pour rechercher d’autres et permettre aux clients de trouver les serveurs de catalogue global. Les services ADDS stocke les enregistrements de localisateur de forêt dans la zone _msdcs. *nom_forêt* zone. Étant donné que les informations contenues dans la zone doivent être largement disponibles, cette zone est répliquée sur tous les serveurs DNS dans la forêt au moyen de la partition d’annuaire de forêt DNS application.  
  
La structure DNS existante reste intacte. Vous n’avez pas besoin déplacer les serveurs ou les zones. Vous devez simplement créer une délégation à vos zones DNS intégrées à ActiveDirectory à partir de votre hiérarchie DNS existante.  
  
## <a name="creating-the-dns-client-configuration"></a>Création de la configuration du client DNS  
Pour configurer DNS sur les ordinateurs clients, le serveur DNS pour le propriétaire du domaine ActiveDirectory doit spécifier l’ordinateur d’attribution de noms schéma et comment les clients utilise pour localiser les serveurs DNS. Le tableau suivant répertorie les nos configurations recommandées pour ces éléments de conception.  
  
|Élément de conception|Configuration|  
|------------------|-----------------|  
|Nom d’ordinateur|Utilisez d’attribution de noms par défaut. Lorsqu’un Windows2000, WindowsXP, Windows Server2003, Windows Server2008 ou WindowsVista-ordinateur joint à un domaine, l’ordinateur s’attribue un nom de domaine complet (FQDN) qui comprend le nom d’hôte de l’ordinateur et le nom de domaine ActiveDirectory.|  
|Configuration du programme de résolution client|Configurer les ordinateurs clients pour pointer vers n’importe quel serveur DNS sur le réseau.|  
  
> [!NOTE]  
> Les clients ActiveDirectory et les contrôleurs de domaine peuvent enregistrer dynamiquement leurs noms DNS même si elles sont ne pointe pas vers le serveur DNS faisant autorité pour leurs noms.  
  
Un ordinateur peut avoir un nom DNS existant différent si l’organisation précédemment, statiquement inscrit l’ordinateur dans DNS ou si l’organisation a précédemment déployé une solution intégrée de Dynamic Host Configuration Protocol (DHCP). Si vos ordinateurs clients ont déjà un nom DNS enregistré, lorsque le domaine auquel ils sont joints est mis à niveau vers Windows Server2008, ADDS, ils aura deux noms différents:  
  
-   Le nom DNS existant  
  
-   Le nouveau nom de domaine complet (FQDN)  
  
Les clients peuvent toujours se trouver par nom. N’importe quel existant DNS, DHCP ou solution intégrée de DNS et DHCP est laissée intacte. Les nouveaux noms de principales sont créés automatiquement et mis à jour au moyen de la mise à jour dynamique. Elles sont automatiquement nettoyées au moyen de nettoyage.  
  
Si vous souhaitez tirer parti de l’authentification Kerberos pour se connecter à un serveur exécutant Windows2000, Windows Server2003 ou Windows Server2008, il se peut que vous devez vous assurer que le client se connecte au serveur en utilisant le nom principal.  
  


