---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: Intégration des services AD DS à une infrastructure DNS existante
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 62405ea9ee38bb3fa457b7731e26fbffb2594797
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891040"
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>Intégration des services AD DS à une infrastructure DNS existante

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si votre organisation possède déjà un service de serveur de système DNS (Domain Name) existant, le serveur DNS pour le propriétaire de Services de domaine Active Directory (AD DS) doit fonctionner avec le propriétaire DNS pour votre organisation d’intégrer les services AD DS dans l’infrastructure existante. Cela implique la création d’un serveur DNS et la configuration du client DNS.  
  
## <a name="creating-a-dns-server-configuration"></a>Création d’une configuration de serveur DNS  
Lors de l’intégration des services AD DS avec un espace de noms DNS existant, nous vous recommandons d’effectuer ce qui suit :  
  
-   Installer le service serveur DNS sur chaque contrôleur de domaine dans la forêt. Cela fournit une tolérance de panne si un des serveurs DNS n’est pas disponible. De cette façon, les contrôleurs de domaine n’avez pas besoin de s’appuyer sur d’autres serveurs DNS pour la résolution de nom. Cela simplifie également l’environnement de gestion, car tous les contrôleurs de domaine ont une configuration uniforme.  
  
-   Configurer le contrôleur de domaine de racine de forêt Active Directory pour héberger la zone DNS pour la forêt Active Directory.  
  
-   Configurer les contrôleurs de domaine pour chaque domaine régional héberger les zones DNS qui correspondent à leurs domaines Active Directory.  
  
-   Configurer la zone contenant les enregistrements de localisateur de forêt Active Directory (autrement dit, la zone _msdcs. *nom_forêt* zone) pour répliquer vers chaque serveur DNS dans la forêt à l’aide de la partition d’annuaire de forêt DNS application.  
  
    > [!NOTE]  
    > Lorsque le service serveur DNS est installé avec l’Assistant domaine Active Directory Services Installation (nous recommandons cette option), toutes les tâches précédentes sont exécutées automatiquement. Pour plus d’informations, consultez [déploiement d’un domaine racine de forêt Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
    > [!NOTE]  
    > Services AD DS utilisent des enregistrements de localisation de la forêt pour permettre aux partenaires de réplication se trouver mutuellement et pour permettre aux clients de trouver des serveurs de catalogue global. Les services AD DS stocke les enregistrements de localisateur de forêt dans la zone _msdcs. *nom_forêt* zone. Étant donné que les informations contenues dans la zone doivent être largement disponibles, cette zone est répliquée sur tous les serveurs DNS dans la forêt au moyen de la partition d’annuaire de forêt DNS application.  
  
La structure DNS existante reste intacte. Il est inutile de déplacer les serveurs ou les zones. Vous devez simplement créer une délégation pour vos zones DNS intégrées à Active Directory à partir de votre hiérarchie DNS existante.  
  
## <a name="creating-the-dns-client-configuration"></a>Création de la configuration du client DNS  
Pour configurer DNS sur les ordinateurs clients, le DNS pour le propriétaire de AD DS doit spécifier l’ordinateur de nommage de schéma et la façon dont les clients localise les serveurs DNS. Le tableau suivant répertorie les nos configurations recommandées pour ces éléments de conception.  
  
|Élément de conception|Configuration|  
|------------------|-----------------|  
|Nom d’ordinateur|Utiliser la dénomination par défaut. Quand un Windows 2000, le Windows XP, Windows Server 2003, Windows Server 2008 ou Windows Vista ordinateur joint un domaine, l’ordinateur s’attribue un nom de domaine complet (FQDN) qui comprend le nom d’hôte de l’ordinateur et le nom de l’actif Domaine de l’annuaire.|  
|Configuration du programme de résolution client|Configurer les ordinateurs clients pour pointer vers n’importe quel serveur DNS sur le réseau.|  
  
> [!NOTE]  
> Les clients Active Directory et les contrôleurs de domaine peuvent enregistrer dynamiquement leurs noms DNS même si elles ne sont pas pointant vers le serveur DNS faisant autorité pour leurs noms.  
  
Un ordinateur peut avoir un autre nom DNS existant si l’organisation précédemment, statiquement inscrit l’ordinateur dans DNS ou si l’organisation a déjà déployé une solution intégrée de la Configuration protocole DHCP (Dynamic Host). Si vos ordinateurs clients ont déjà un nom DNS enregistré, lorsque le domaine auquel ils appartiennent est mis à niveau vers Windows Server 2008 AD DS, ils auront deux noms différents :  
  
-   Le nom DNS existant  
  
-   Le nouveau nom de domaine complet (FQDN)  
  
Les clients peuvent toujours être localisés par le nom. N’importe quel existant DNS, DHCP ou une solution intégrée DNS et DHCP est laissée intacte. Les nouveaux noms de principales sont créés automatiquement et mis à jour au moyen de la mise à jour dynamique. Elles sont automatiquement nettoyées au moyen de nettoyage.  
  
Si vous souhaitez tirer parti de l’authentification Kerberos lors de la connexion à un serveur exécutant Windows 2000, Windows Server 2003 ou Windows Server 2008, il se peut que vous devez vous assurer que le client se connecte au serveur en utilisant le nom de principal.  
  


