---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: Certificat de signature de jetons
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 24b095827ad074dbe249a9d4decd8edeecbb3603
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858782"
---
# <a name="token-signing-certificates"></a>Certificat de signature de jetons

Les serveurs de Fédération requièrent des certificats de signature\-de jeton pour empêcher des personnes malveillantes de modifier ou de contrefaire des jetons de sécurité en tentant d’accéder de façon non autorisée aux ressources fédérées. Le couplage de clé publique\/privé utilisé avec les certificats de signature\-de jeton est le mécanisme de validation le plus important de tout partenariat fédéré, car ces clés vérifient qu’un jeton de sécurité a été émis par un serveur de Fédération partenaire valide et que le jeton n’a pas été modifié lors du transit.  
  
## <a name="token-signing-certificate-requirements"></a>Conditions requises pour le certificat de signature\-de jeton  
Un jeton\-certificat de signature doit remplir les conditions suivantes pour fonctionner avec AD FS :  
  
-   Pour qu’un jeton\-certificat de signature pour signer correctement un jeton de sécurité, le jeton\-certificat de signature doit contenir une clé privée.  
  
-   Le compte de service AD FS doit avoir accès au jeton\-clé privée du certificat de signature dans le magasin personnel de l’ordinateur local. Ce processus est pris en charge par le programme d’installation de. Vous pouvez également utiliser le\-du composant logiciel enfichable de gestion AD FS dans pour garantir cet accès si vous modifiez par la suite le jeton\-certificat de signature.  
  
> [!NOTE]  
> Il s’agit d’une infrastructure à clé publique \(PKI\) meilleure pratique pour ne pas partager la clé privée à plusieurs fins. Par conséquent, n’utilisez pas le certificat de communication du service que vous avez installé sur le serveur de Fédération en tant que jeton\-certificat de signature.  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>Utilisation des certificats de signature\-des jetons pour les partenaires  
Chaque jeton\-certificat de signature contient des clés privées de chiffrement et des clés publiques qui sont utilisées pour signer numériquement \(au moyen de la clé privée\) un jeton de sécurité. Plus tard, une fois qu’ils sont reçus par un serveur de Fédération partenaire, ces clés valident l’authenticité \(au moyen de la clé publique\) du jeton de sécurité chiffré.  
  
Étant donné que chaque jeton de sécurité est signé numériquement par le partenaire de compte, le partenaire de ressource peut vérifier que le jeton de sécurité a été émis par le partenaire de compte et qu’il n’a pas été modifié. Les signatures numériques sont vérifiées par la partie clé publique du jeton\-certificat de signature d’un partenaire. Une fois la signature vérifiée, le serveur de Fédération de ressources génère son propre jeton de sécurité pour son organisation et signe le jeton de sécurité avec son propre jeton\-certificat de signature.  
  
Pour les environnements de partenaires de Fédération, lorsque le jeton\-certificat de signature a été émis par une autorité de certification, assurez-vous que :  
  
1.  Les listes de révocation de certificats \(\) CRL du certificat sont accessibles aux parties de confiance et aux serveurs Web qui approuvent le serveur de Fédération.  
  
2.  Le certificat d’autorité de certification racine est approuvé par les parties de confiance et les serveurs Web qui approuvent le serveur de Fédération.  
  
Le serveur Web du partenaire de ressource utilise la clé publique du jeton\-certificat de signature pour vérifier que le jeton de sécurité est signé par le serveur de Fédération de ressources. Le serveur Web autorise alors l’accès approprié au client.  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>Considérations relatives au déploiement des certificats de signature\-de jeton  
Lorsque vous déployez le premier serveur de Fédération dans une nouvelle installation de AD FS, vous devez obtenir un jeton\-certificat de signature et l’installer dans le magasin de certificats personnels de l’ordinateur local sur ce serveur de Fédération. Vous pouvez obtenir un jeton\-certificat de signature en demandant un certificat auprès d’une autorité de certification d’entreprise ou d’une autorité de certification publique ou en créant un certificat auto\-signé.  
  
Lorsque vous déployez une batterie de serveurs AD FS, les certificats de signature\-de jeton sont installés différemment, en fonction de la façon dont vous créez la batterie de serveurs.  
  
Il existe deux options de batterie de serveurs que vous pouvez prendre en compte quand vous obtenez des certificats de signature\-de jeton pour votre déploiement :  
  
-   Une clé privée d’un jeton\-certificat de signature est partagée entre tous les serveurs de Fédération d’une batterie de serveurs.  
  
    Dans un environnement de batterie de serveurs de Fédération, nous recommandons que tous les serveurs de fédération partagent \(ou réutilise\) le même jeton\-certificat de signature. Vous pouvez installer un jeton unique\-certificat de signature à partir d’une autorité de certification sur un serveur de Fédération, puis exporter la clé privée, tant que le certificat émis est marqué comme étant exportable.  
  
    Comme indiqué dans l’illustration suivante, la clé privée d’un jeton unique\-certificat de signature peut être partagée avec tous les serveurs de Fédération d’une batterie de serveurs. Cette option, comparée à l’option « jeton unique\-certificat de signature » ci-dessous, réduit les coûts si vous envisagez d’obtenir un jeton\-certificat de signature d’une autorité de certification publique.  
  
![signature des jetons](media/adfs2_fedserver_certstory_3.gif)  
  
-   Il existe un jeton unique\-certificat de signature pour chaque serveur de Fédération d’une batterie de serveurs.  
  
    Lorsque vous utilisez plusieurs certificats uniques dans votre batterie de serveurs, chaque serveur de cette batterie signe des jetons avec sa propre clé privée unique.  
  
    Comme indiqué dans l’illustration suivante, vous pouvez obtenir un jeton distinct\-certificat de signature pour chaque serveur de Fédération d’une batterie de serveurs. Cette option est plus coûteuse si vous prévoyez d’obtenir votre jeton\-des certificats de signature à partir d’une autorité de certification publique.  
  
![signature des jetons](media/adfs2_fedserver_certstory_4.gif)  
  
Pour plus d’informations sur l’installation d’un certificat quand vous utilisez les services de certificats Microsoft en tant qu’autorité de certification d’entreprise, consultez [iis 7,0 : créer un certificat de serveur de domaine dans iis 7,0](https://go.microsoft.com/fwlink/?LinkId=108548).  
  
Pour plus d’informations sur l’installation d’un certificat à partir d’une autorité de certification publique, consultez [IIS 7,0 : demander un certificat de serveur Internet](https://go.microsoft.com/fwlink/?LinkId=108549).  
  
Pour plus d’informations sur l’installation d’un certificat auto\-signé, voir [iis 7,0 : créer un certificat de serveur auto\-signé dans IIS 7,0](https://go.microsoft.com/fwlink/?LinkID=108271).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
