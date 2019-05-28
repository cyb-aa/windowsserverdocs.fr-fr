---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: Certificats de communication du service
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b781d6fe99864b13d6e7f8ab65f3a14d205c2aa6
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190804"
---
# <a name="service-communications-certificates"></a>Certificats de communication du service

Un serveur de fédération requiert l’utilisation de certificats de communication de service pour les scénarios dans lesquels la sécurité des messages WCF est utilisée.  
  
## <a name="service-communication-certificate-requirements"></a>Exigences de certificat de communication de service  
Certificats de communication du service doivent répondre aux exigences suivantes pour travailler avec AD FS :  
  
-   Le certificat de communication de service doit inclure l’utilisation de la clé de serveur d’authentification renforcée \(EKU\) extension.  
  
-   Les listes de révocation de certificat \(CRL\) doivent être accessibles pour tous les certificats de la chaîne à partir du certificat de communication de service pour le certificat d’autorité de certification racine. L’autorité de certification racine doit également être approuvé par les serveurs proxy de fédération et les serveurs Web qui approuvent ce serveur de fédération.  
  
-   Le nom d’objet qui est utilisé dans le certificat de communication de service doit correspondre au nom de Service de fédération dans les propriétés du Service de fédération.  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Considérations relatives au déploiement des certificats de communication de service  
Configurer les certificats de communication de service afin que tous les serveurs de fédération utilisent le même certificat. Si vous déployez l’unique Web fédérée\-connexion\-sur \(SSO\) conception, nous recommandons que votre certificat de communication du service être émis par une autorité de certification publique. Vous pouvez demander et installer ces certificats via le composant logiciel enfichable Gestionnaire des services Internet\-dans.  
  
Vous pouvez utiliser self\-signé, des certificats de communication avec succès sur les serveurs de fédération dans un environnement de laboratoire de test de service. Toutefois, pour un environnement de production, nous recommandons que vous obtenez des certificats de communication de service à partir d’une autorité de certification publique. Voici les raisons pour lesquelles vous devez utiliser pas self\-signé, des certificats de communication pour un déploiement de service :  
  
-   Un libre-service\-signé, certificat SSL doit être ajouté au magasin racine approuvé sur chacun des serveurs de fédération de l’organisation partenaire ressource. Alors que seul, cela n’active pas une personne malveillante de compromettre un serveur de fédération de ressources, approbation self\-certificats auto-signés augmente la surface d’attaque d’un ordinateur, et cela peut entraîner des failles de sécurité si le certificat de signataire n’est pas digne de confiance.  
  
-   Il crée une expérience utilisateur incorrects. Les clients reçoivent des invites de l’alerte de sécurité lorsqu’ils tentent d’accéder aux ressources fédérés qui affichent le message suivant : « Le certificat de sécurité a été délivré par une société que vous n’avez pas choisi de faire confiance. » Il s’agit du comportement attendu, étant donné que self\-certificat signé n’est pas approuvé.  
  
    > [!NOTE]  
    > Si nécessaire, vous pouvez contourner cette condition à l’aide de la stratégie de groupe pour envoyer manuellement vers le bas self\-signé le certificat au magasin racine approuvé sur chaque ordinateur client qui tente d’accéder à un site AD FS.  
  
-   Autorités de certification fournissent certificat supplémentaire\-en fonction des fonctionnalités, telles que l’archivage de la clé privée, renouvellement et la révocation, qui ne sont pas fournies par self\-certificats auto-signés.  
  

