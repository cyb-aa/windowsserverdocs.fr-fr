---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: Certificats de communication du service
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 624d2e26bc0277129e44eee3fdce7c7396b735a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407905"
---
# <a name="service-communications-certificates"></a>Certificats de communication du service

Un serveur de Fédération requiert l’utilisation de certificats de communication de service pour les scénarios dans lesquels la sécurité des messages WCF est utilisée.  
  
## <a name="service-communication-certificate-requirements"></a>Conditions requises pour les certificats de communication de service  
Les certificats de communication de service doivent remplir les conditions suivantes pour fonctionner avec AD FS :  
  
-   Le certificat de communication du service doit inclure l’extension de l’utilisation améliorée de la clé d’authentification du serveur \(EKU @ no__t-1.  
  
-   Les listes de révocation de certificats \(CRLs @ no__t-1 doivent être accessibles pour tous les certificats de la chaîne du certificat de communication du service au certificat d’autorité de certification racine. L’autorité de certification racine doit également être approuvée par les serveurs proxy de Fédération et les serveurs Web qui approuvent ce serveur de Fédération.  
  
-   Le nom du sujet utilisé dans le certificat de communication du service doit correspondre au nom de service FS (Federation Service) dans les propriétés de l’service FS (Federation Service).  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Considérations relatives au déploiement des certificats de communication de service  
Configurez des certificats de communication de service afin que tous les serveurs de Fédération utilisent le même certificat. Si vous déployez la conception no__t-0Sign @ no__t-1On \(SSO @ no__t-3 fédérée Web fédéré, nous vous recommandons d’émettre votre certificat de communication de service par une autorité de certification publique. Vous pouvez demander et installer ces certificats via le composant logiciel enfichable Gestionnaire des services Internet @ no__t-0in.  
  
Vous pouvez utiliser Self @ no__t-0signed, les certificats de communication de service avec succès sur les serveurs de Fédération dans un environnement de laboratoire de test. Toutefois, pour un environnement de production, nous vous recommandons d’obtenir des certificats de communication de service auprès d’une autorité de certification publique. Voici les raisons pour lesquelles vous ne devez pas utiliser Self @ no__t-0signed, les certificats de communication de service pour un déploiement en direct :  
  
-   Un certificat SSL Self @ no__t-0signed doit être ajouté au magasin racine approuvé sur chacun des serveurs de Fédération de l’organisation partenaire de ressource. Bien que ce seul ne permette pas à une personne malveillante de compromettre un serveur de Fédération de ressources, l’approbation des certificats Self @ no__t-0signed augmente la surface d’attaque d’un ordinateur et peut entraîner des failles de sécurité si le signataire de certificat n’est pas sûr.  
  
-   Il crée une expérience utilisateur incorrecte. Les clients recevront des invites d’alerte de sécurité lorsqu’ils tenteront d’accéder aux ressources fédérées qui affichent le message suivant : « Le certificat de sécurité a été émis par une société à laquelle vous n’avez pas choisi de faire confiance ». Ce comportement est normal, car le certificat Self @ no__t-0signed n’est pas approuvé.  
  
    > [!NOTE]  
    > Si nécessaire, vous pouvez contourner cette condition en utilisant stratégie de groupe pour pousser manuellement le certificat Self @ no__t-0signed vers le magasin racine approuvé sur chaque ordinateur client qui tente d’accéder à un site AD FS.  
  
-   Les autorités de certification fournissent des fonctionnalités de certificat supplémentaires @ no__t-0based, telles que l’archive de clé privée, le renouvellement et la révocation, qui ne sont pas fournies par les certificats Self @ no__t-1signed.  
  

