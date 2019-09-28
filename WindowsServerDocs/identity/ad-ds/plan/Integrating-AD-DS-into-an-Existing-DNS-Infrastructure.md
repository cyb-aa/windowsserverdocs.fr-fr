---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: Intégration des services AD DS à une infrastructure DNS existante
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f4bb480be4696f15f0a63c20ab47042264584d2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402559"
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>Intégration des services AD DS à une infrastructure DNS existante

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si votre organisation dispose déjà d’un service de serveur DNS (Domain Name System), le propriétaire de Active Directory Domain Services (AD DS) doit travailler avec le propriétaire DNS de votre organisation pour intégrer les AD DS dans l’infrastructure existante. Cela implique la création d’un serveur DNS et d’une configuration de client DNS.  
  
## <a name="creating-a-dns-server-configuration"></a>Création d’une configuration de serveur DNS  
Lors de l’intégration de AD DS avec un espace de noms DNS existant, nous vous recommandons d’effectuer les opérations suivantes :  
  
-   Installez le service serveur DNS sur chaque contrôleur de domaine de la forêt. Cela fournit une tolérance de panne si l’un des serveurs DNS n’est pas disponible. De cette façon, les contrôleurs de domaine n’ont pas besoin de s’appuyer sur d’autres serveurs DNS pour la résolution de noms. Cela simplifie également l’environnement de gestion, car tous les contrôleurs de domaine ont une configuration uniforme.  
  
-   Configurez le contrôleur de domaine racine de la forêt Active Directory pour héberger la zone DNS pour la forêt Active Directory.  
  
-   Configurez les contrôleurs de domaine pour chaque domaine régional afin d’héberger les zones DNS qui correspondent à leurs domaines de Active Directory.  
  
-   Configurez la zone contenant les Active Directory les enregistrements de localisateur à l’ensemble de la forêt (c’est-à-dire, _ msdcs. *forestname* zone) à répliquer sur chaque serveur DNS de la forêt à l’aide de la partition de l’annuaire d’applications DNS à l’ensemble de la forêt.  
  
    > [!NOTE]  
    > Lorsque le service serveur DNS est installé avec le Assistant Installation Active Directory Domain Services (cette option est recommandée), toutes les tâches précédentes sont effectuées automatiquement. Pour plus d’informations, consultez [déploiement d’un domaine racine de forêt Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
    > [!NOTE]  
    > AD DS utilise des enregistrements de localisateur à l’échelle de la forêt pour permettre aux partenaires de réplication de se trouver mutuellement et de permettre aux clients de trouver des serveurs de catalogue global. AD DS stocke les enregistrements de localisateur à l’ensemble de la forêt dans la _ msdcs. zone *forestname* . Étant donné que les informations de la zone doivent être largement disponibles, cette zone est répliquée sur tous les serveurs DNS de la forêt par le biais de la partition de l’annuaire d’applications DNS à l’échelle de la forêt.  
  
La structure DNS existante reste intacte. Vous n’avez pas besoin de déplacer des serveurs ou des zones. Vous devez simplement créer une délégation à vos zones DNS intégrées à Active Directory à partir de votre hiérarchie DNS existante.  
  
## <a name="creating-the-dns-client-configuration"></a>Création de la configuration du client DNS  
Pour configurer DNS sur les ordinateurs clients, le DNS de AD DS propriétaire doit spécifier le schéma de nom de l’ordinateur et la manière dont les clients peuvent localiser les serveurs DNS. Le tableau suivant répertorie nos configurations recommandées pour ces éléments de conception.  
  
|Élément Design|Configuration|  
|------------------|-----------------|  
|Nom de l’ordinateur|Utilisez le nom par défaut. Lorsqu’un ordinateur Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 ou Windows Vista joint un domaine, l’ordinateur s’attribue un nom de domaine complet (FQDN) qui comprend le nom d’hôte de l’ordinateur et le nom de l’ordinateur actif. Domaine de l’annuaire.|  
|Configuration du programme de résolution du client|Configurez les ordinateurs clients pour qu’ils pointent vers n’importe quel serveur DNS du réseau.|  
  
> [!NOTE]  
> Les clients Active Directory et les contrôleurs de domaine peuvent inscrire dynamiquement leurs noms DNS, même s’ils ne pointent pas vers le serveur DNS faisant autorité pour leurs noms.  
  
Un ordinateur peut avoir un nom DNS existant différent si l’organisation avait précédemment inscrit l’ordinateur dans DNS de manière statique ou si l’organisation a déployé précédemment une solution DHCP (Dynamic Host Configuration Protocol) intégrée. Si vos ordinateurs clients ont déjà un nom DNS enregistré, lorsque le domaine auquel ils sont joints est mis à niveau vers Windows Server 2008 AD DS, ils auront deux noms différents :  
  
-   Nom DNS existant  
  
-   Le nouveau nom de domaine complet (FQDN)  
  
Les clients peuvent toujours être localisés par l’un ou l’autre nom. Toute solution DNS, DHCP ou DNS intégrée existante reste intacte. Les nouveaux noms primaires sont créés automatiquement et mis à jour au moyen de la mise à jour dynamique. Elles sont automatiquement nettoyées par le biais du nettoyage.  
  
Si vous souhaitez tirer parti de l’authentification Kerberos lors de la connexion à un serveur exécutant Windows 2000, Windows Server 2003 ou Windows Server 2008, vous devez vous assurer que le client se connecte au serveur à l’aide du nom principal.  
  


