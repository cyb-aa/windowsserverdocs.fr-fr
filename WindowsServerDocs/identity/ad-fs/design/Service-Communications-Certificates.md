---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: Certificats de Communications de service
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 32e60f2c2d9e4fced04061ace44882b792e1a3bc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="service-communications-certificates"></a>Certificats de Communications de service

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Un serveur de fédération requiert l’utilisation de certificats de communication du service pour les scénarios dans lesquels la sécurité de message WCF est utilisée.  
  
## <a name="service-communication-certificate-requirements"></a>Exigences de certificat de communication de service  
Certificats de communication du service doivent répondre aux exigences suivantes pour travailler avec ADFS:  
  
-   Le certificat de communication du service doit inclure l’extension de serveur d’authentification renforcée utilisation de la clé \(EKU\).  
  
-   Le certificat révocation listes \(CRLs\) doivent être accessibles pour tous les certificats de la chaîne du certificat de communication de service pour le certificat d’autorité de certification racine. L’autorité de certification racine doit également être approuvé par les serveurs proxy de fédération et les serveurs Web qui approuvent ce serveur de fédération.  
  
-   Le nom d’objet qui est utilisé dans le certificat de communication du service doit correspondre au nom de Service de fédération dans les propriétés du Service de fédération.  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Considérations relatives au déploiement de certificats de communication du service  
Configurer les certificats de communication du service afin que tous les serveurs de fédération utilisent le même certificat. Si vous déployez la conception \(SSO\) Single\-Sign\-On de Web fédéré, nous recommandons que votre certificat de communication du service être émis par une autorité de certification publique. Vous pouvez demander et installer ces certificats via le Gestionnaire des services Internet enfichable.  
  
Vous pouvez utiliser la signature intégrée, communication du service de certificats avec succès sur les serveurs de fédération dans un environnement de laboratoire de test. Toutefois, pour un environnement de production, nous vous recommandons d’obtenir des certificats de communication du service à partir d’une autorité de certification publique. Raisons pourquoi vous ne devez pas utiliser intégrée, service communication certificats signés pour un déploiement dynamique sont les suivantes:  
  
-   A signé intégrée, certificat SSL doit être ajouté au magasin racine approuvé sur chacun des serveurs de fédération dans l’organisation partenaire de ressource. Alors que seules, cela n’active pas une personne malveillante de compromettre un serveur de fédération de ressources, approuver les certificats signés par intégrée augmente la surface d’attaque d’un ordinateur, et il peut entraîner des failles de sécurité si le signataire de certificat n’est pas digne de confiance.  
  
-   Il crée une expérience utilisateur médiocre. Les clients reçoivent des messages d’alerte de sécurité lorsqu’ils essaient d’accéder aux ressources fédérées qui affichent le message suivant: «le certificat de sécurité a été émis par une société que vous n’avez pas choisi de confiance». Ce comportement est normal, car le certificat signé intégrée n’est pas approuvé.  
  
    > [!NOTE]  
    > Si nécessaire, vous pouvez contourner cette condition à l’aide de la stratégie de groupe pour pousser le certificat de signature intégrée manuellement dans le magasin racine de confiance sur chaque ordinateur client qui tente d’accéder à un site ADFS.  
  
-   Fournissent des autorités de certification basée sur les certificate\ des fonctionnalités supplémentaires, telles que l’archivage des clés privée et révocation, renouvellement qui ne sont pas fournies par des certificats signés par une intégrée.  
  

