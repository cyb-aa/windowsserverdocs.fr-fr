---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: "Certificats requis pour les serveurs proxy de fédération"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7fb8e71afed1c0eb6b55857835d95f2dd0ec9d5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Certificats requis pour les serveurs proxy de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Serveurs qui exécutent le rôle de proxy de serveur de fédération dans ActiveDirectory Federation Services \(ADFS\) sont requis pour utiliser des certificats d’authentification serveur SSL \(SSL\). Serveurs proxy de fédération utilisent des certificats d’authentification serveur SSL pour sécuriser les communications de trafic de serveur Web avec les clients Web.  
  
Serveurs proxy de fédération est généralement exposés à des ordinateurs sur Internet qui ne sont pas inclus dans votre infrastructure à clé publique entreprise \(PKI\). Par conséquent, utilisez un certificat d’authentification serveur émis par une autorité de certification publique \(third\-party\) \(CA\), par exemple, VeriSign.  
  
Lorsque vous disposez d’une batterie de serveurs de fédération proxy, tous les serveurs proxy de fédération doit utiliser le même certificat d’authentification serveur. Pour plus d’informations, voir [When to Create a Federation Server Proxy Farm](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
Il est important de vérifier que le nom du sujet dans le certificat d’authentification serveur correspond à la valeur du nom de Service de fédération qui est spécifiée dans la gestion ADFS de composants. Pour rechercher cette valeur, ouvrez le composant logiciel enfichable, droit clic **Service**, cliquez sur **modifier les propriétés du Service de fédération**et recherchez la valeur dans **nom du Service de fédération** zone de texte.  
  
Pour obtenir des informations générales sur l’utilisation de certificats SSL, consultez la configuration de SSL dans IIS 7.0 \ ([http:///\/go.microsoft.com\/fwlink\/? LinkID\ = 108544](https://go.microsoft.com/fwlink/?LinkID=108544)\) et la configuration des certificats de serveur dans IIS 7.0 \ ([http:///\/go.microsoft.com\/fwlink\/? LinkID\ = 108545](https://go.microsoft.com/fwlink/?LinkID=108545)\).  
  
> [!NOTE]  
> Certificats d’authentification client ne sont pas requis pour les serveurs proxy de fédération ADFS.  
  
Si un certificat que vous utilisez a \(CRLs\) des listes de révocation de certificats, le serveur avec le certificat configuré doit être en mesure de contacter le serveur qui distribue les CRL. Le type de CRL détermine les ports qui sont utilisés.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
