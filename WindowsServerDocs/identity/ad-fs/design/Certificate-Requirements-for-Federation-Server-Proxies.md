---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: Certificats requis pour les serveurs proxy de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: dab77c3e3226e89eb3ac9b74e7db9b6df8f181bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408148"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Certificats requis pour les serveurs proxy de fédération

Les serveurs qui s’exécutent dans le rôle de serveur proxy de Fédération dans Services ADFS \(AD FS @ no__t-1 sont requis pour utiliser protocole SSL certificats d’authentification de serveur \(SSL @ no__t-3. Les serveurs proxy de fédération utilisent les certificats d’authentification serveur SSL pour sécuriser les communications du trafic sur le serveur Web avec les clients web.  
  
Les serveurs proxys de Fédération sont généralement exposés à des ordinateurs sur Internet qui ne sont pas inclus dans votre infrastructure de clé publique d’entreprise \(PKI @ no__t-1. Par conséquent, utilisez un certificat d’authentification serveur émis par une autorité de certification public \(third @ no__t-1party @ no__t-2 \(CA @ no__t-4, par exemple, VeriSign.  
  
Lorsque vous avez une batterie de serveurs proxy de Fédération, tous les ordinateurs proxy du serveur de Fédération doivent utiliser le même certificat d’authentification serveur. Pour plus d'informations, voir [Quand créer une batterie de serveurs proxy de fédération](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
Il est important de vérifier que le nom d’objet dans le certificat d’authentification serveur correspond à la valeur de nom d’service FS (Federation Service) spécifiée dans le composant logiciel enfichable de gestion AD FS @ no__t-0in. Pour rechercher cette valeur, ouvrez le service Snap @ no__t-0in, Right @ no__t-1Cliquez **service**, cliquez sur modifier les **Propriétés du service FS (Federation Service)** , puis recherchez la valeur dans la zone de texte nom de **service FS (Federation Service)** .  
  
Pour obtenir des informations générales sur l’utilisation des certificats SSL, consultez Configuration de protocole SSL dans IIS 7,0 \([http : @no__t -2\/go.microsoft.com @ no__t-4fwlink @ no__t-5 ? LinkId @ no__t-6108544](https://go.microsoft.com/fwlink/?LinkID=108544)\) et configuration des certificats de serveur dans IIS 7,0 \([http : @no__t -101go.microsoft.com @ no__t-12fwlink @ no__t-13 ? LinkId @ no__t-14108545](https://go.microsoft.com/fwlink/?LinkID=108545)5.  
  
> [!NOTE]  
> Les certificats d’authentification client ne sont pas requis pour AD FS serveurs proxy de Fédération.  
  
Si un certificat que vous utilisez comporte des listes de révocation de certificats \(CRLs @ no__t-1, le serveur avec le certificat configuré doit être en mesure de contacter le serveur qui distribue les listes de révocation de certificats. Le type de CRL détermine les ports qui sont utilisés.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
