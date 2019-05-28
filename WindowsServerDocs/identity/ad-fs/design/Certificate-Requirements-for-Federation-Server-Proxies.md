---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: Certificats requis pour les serveurs proxy de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ca0b25480eedfc6471837ab8ae83b0d1d522e61e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191665"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Certificats requis pour les serveurs proxy de fédération

Les serveurs qui sont exécutent dans le rôle du serveur proxy de fédération dans Active Directory Federation Services \(AD FS\) sont requises pour utiliser le protocole SSL (Secure Sockets Layer) \(SSL\) certificats d’authentification serveur. Les serveurs proxy de fédération utilisent les certificats d’authentification serveur SSL pour sécuriser les communications du trafic sur le serveur Web avec les clients web.  
  
Serveurs proxy de fédération est généralement exposés à des ordinateurs sur Internet qui ne sont pas inclus dans votre infrastructure à clé publique entreprise \(PKI\). Par conséquent, utilisez un certificat d’authentification serveur émis par un public \(troisième\-tiers\) autorité de certification \(autorité de certification\), par exemple, VeriSign.  
  
Lorsque vous avez une batterie de serveurs du proxy du serveur de fédération, tous les serveurs proxy de fédération doit utiliser le même certificat d’authentification serveur. Pour plus d'informations, voir [When to Create a Federation Server Proxy Farm](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
Il est important de vérifier que le nom du sujet dans les correspondances de certificat d’authentification serveur le nom du Service de fédération valeur qui est spécifié dans le composant logiciel enfichable Gestion AD FS\-dans. Pour rechercher cette valeur, ouvrez le composant logiciel enfichable\-, avec le bouton droit\-cliquez sur **Service**, cliquez sur **modifier les propriétés de Service de fédération**, puis recherchez la valeur dans **fédération Nom du service** zone de texte.  
  
Pour obtenir des informations générales sur l’utilisation de certificats SSL, consultez Configuration de Secure Sockets Layer dans IIS 7.0 \( [http :\/\/go.microsoft.com\/fwlink\/? LinkID\=108544](https://go.microsoft.com/fwlink/?LinkID=108544) \) et configuration des certificats de serveur dans IIS 7.0 \( [http :\/\/go.microsoft.com\/fwlink\/? LinkID\=108545](https://go.microsoft.com/fwlink/?LinkID=108545)\).  
  
> [!NOTE]  
> Certificats d’authentification client ne sont pas requis pour les serveurs proxy de fédération AD FS.  
  
Si un certificat que vous utilisez a des listes de révocation de certificats \(CRL\), le serveur avec le certificat configuré doit être en mesure de contacter le serveur qui distribue les CRL. Le type de CRL détermine les ports qui sont utilisés.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
