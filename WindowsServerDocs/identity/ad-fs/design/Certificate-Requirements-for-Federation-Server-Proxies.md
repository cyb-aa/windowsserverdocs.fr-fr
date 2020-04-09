---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: Certificats requis pour les serveurs proxy de fédération
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cc32288d01d7e1386f146716f45f0e49ced3d48e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858122"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Certificats requis pour les serveurs proxy de fédération

Les serveurs qui s’exécutent dans le rôle de serveur proxy de Fédération dans Services ADFS \(AD FS\) sont requis pour utiliser protocole SSL certificats d’authentification de serveur \(SSL.\) Les serveurs proxy de fédération utilisent les certificats d’authentification serveur SSL pour sécuriser les communications du trafic sur le serveur Web avec les clients web.  
  
Les serveurs proxys de Fédération sont généralement exposés à des ordinateurs sur Internet qui ne sont pas inclus dans votre infrastructure de clé publique d’entreprise \(PKI\). Par conséquent, utilisez un certificat d’authentification serveur émis par un \(public tiers\-tiers\) autorité de certification \(\)autorité de certification, par exemple, VeriSign.  
  
Lorsque vous avez une batterie de serveurs proxy de Fédération, tous les ordinateurs proxy du serveur de Fédération doivent utiliser le même certificat d’authentification serveur. Pour plus d'informations, voir [When to Create a Federation Server Proxy Farm](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
Il est important de vérifier que le nom d’objet dans le certificat d’authentification serveur correspond à la valeur de nom d’service FS (Federation Service) spécifiée dans le\-AD FS de gestion de la gestion des. Pour rechercher cette valeur, ouvrez le\-d’alignement dans,\-cliquez avec le bouton droit sur **service**, cliquez sur **modifier les propriétés du service FS (Federation Service)** , puis recherchez la valeur dans la zone de texte nom de la **service FS (Federation Service)** .  
  
Pour obtenir des informations générales sur l’utilisation des certificats SSL, consultez Configuration de protocole SSL dans IIS 7,0 \([http :\/\/go.microsoft.com\/fwlink\/? LinkID\=108544](https://go.microsoft.com/fwlink/?LinkID=108544)\) et configuration des certificats de serveur dans IIS 7,0 \([http :\/\/Go.Microsoft.com\/fwlink\/? LinkID\=108545](https://go.microsoft.com/fwlink/?LinkID=108545)\).  
  
> [!NOTE]  
> Les certificats d’authentification client ne sont pas requis pour AD FS serveurs proxy de Fédération.  
  
Si un certificat que vous utilisez comporte des listes de révocation de certificats \(les CRL\), le serveur avec le certificat configuré doit être en mesure de contacter le serveur qui distribue les listes de révocation de certificats. Le type de CRL détermine les ports qui sont utilisés.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
