---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: Certificat de signature de jetons
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d31b7d60021e72f80c762df5b4e1a4199ea3909c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867686"
---
# <a name="token-signing-certificates"></a>Certificat de signature de jetons

Les serveurs de Fédération\-requièrent des certificats de signature de jetons pour empêcher les attaquants de modifier ou de contrefaire les jetons de sécurité en vue d’obtenir un accès non autorisé aux ressources fédérées. Le couplage\/de clé publique privée utilisé avec les certificats de\-signature de jetons est le mécanisme de validation le plus important de tout partenariat fédéré, car ces clés vérifient qu’un jeton de sécurité a été émis par un partenaire valide serveur de Fédération et que le jeton n’a pas été modifié lors du transit.  
  
## <a name="token-signing-certificate-requirements"></a>Conditions\-requises pour les certificats de signature de jetons  
Un certificat\-de signature de jetons doit remplir les conditions suivantes pour fonctionner avec AD FS :  
  
-   Pour qu’un\-certificat de signature de jetons signe correctement un jeton de sécurité\-, le certificat de signature de jetons doit contenir une clé privée.  
  
-   Le compte de service AD FS doit avoir accès à la\-clé privée du certificat de signature de jetons dans le magasin personnel de l’ordinateur local. Ce processus est pris en charge par le programme d’installation de. Vous pouvez également utiliser le composant logiciel enfichable\-gestion de la AD FS pour garantir cet accès si vous modifiez par la suite le certificat de signature de jetons.\-  
  
> [!NOTE]  
> Il s’agit d’une pratique \(recommandée\) pour l’infrastructure PKI de l’infrastructure de clé publique de ne pas partager la clé privée à des fins multiples. Par conséquent, n’utilisez pas le certificat de communication du service que vous avez installé sur le serveur\-de Fédération en tant que certificat de signature de jetons.  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>Utilisation des\-certificats de signature de jetons pour les partenaires  
Chaque certificat\-de signature de jetons contient des clés privées de chiffrement et des clés publiques utilisées \(pour signer numériquement par le biais\) de la clé privée un jeton de sécurité. Plus tard, une fois qu’ils sont reçus par un serveur de Fédération partenaire, ces \(clés valident l’authenticité au\) moyen de la clé publique du jeton de sécurité chiffré.  
  
Étant donné que chaque jeton de sécurité est signé numériquement par le partenaire de compte, le partenaire de ressource peut vérifier que le jeton de sécurité a été émis par le partenaire de compte et qu’il n’a pas été modifié. Les signatures numériques sont vérifiées par la partie clé publique du certificat de\-signature de jetons d’un partenaire. Une fois la signature vérifiée, le serveur de Fédération de ressources génère son propre jeton de sécurité pour son organisation et signe le jeton de sécurité\-avec son propre certificat de signature de jetons.  
  
Pour les environnements de partenaires de Fédération,\-lorsque le certificat de signature de jetons a été émis par une autorité de certification, assurez-vous que :  
  
1.  La révocation\) de certificat \(répertorie les listes de révocation de certificats du certificat sont accessibles aux parties de confiance et aux serveurs Web qui approuvent le serveur de Fédération.  
  
2.  Le certificat d’autorité de certification racine est approuvé par les parties de confiance et les serveurs Web qui approuvent le serveur de Fédération.  
  
Le serveur Web du partenaire de ressource utilise la clé publique du certificat de\-signature de jetons pour vérifier que le jeton de sécurité est signé par le serveur de Fédération de ressources. Le serveur Web autorise alors l’accès approprié au client.  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>Considérations relatives au déploiement\-pour les certificats de signature de jetons  
Lorsque vous déployez le premier serveur de Fédération dans une nouvelle installation de AD FS, vous devez\-obtenir un certificat de signature de jetons et l’installer dans le magasin de certificats personnels de l’ordinateur local sur ce serveur de Fédération. Vous pouvez obtenir un certificat\-de signature de jetons en demandant un certificat auprès d’une autorité de certification d’entreprise ou d'\-une autorité de certification publique ou en créant un certificat auto-signé.  
  
Lorsque vous déployez une batterie de serveurs\-AD FS, les certificats de signature de jetons sont installés différemment, en fonction de la façon dont vous créez la batterie de serveurs.  
  
Il existe deux options de batterie de serveurs que vous pouvez prendre en compte\-lorsque vous obtenez des certificats de signature de jetons pour votre déploiement :  
  
-   Une clé privée d’un certificat\-de signature de jetons est partagée entre tous les serveurs de Fédération d’une batterie de serveurs.  
  
    Dans un environnement de batterie de serveurs de Fédération, nous recommandons que tous \(les serveurs\) de fédération partagent\-ou réutilisent le même certificat de signature de jetons. Vous pouvez installer un certificat de\-signature de jetons unique à partir d’une autorité de certification sur un serveur de Fédération, puis exporter la clé privée, tant que le certificat émis est marqué comme étant exportable.  
  
    Comme indiqué dans l’illustration suivante, la clé privée d’un certificat de\-signature de jetons unique peut être partagée sur tous les serveurs de Fédération d’une batterie de serveurs. Cette option, comparée à l’option « certificat\-de signature de jeton unique » ci-dessous, réduit les coûts si\-vous prévoyez d’obtenir un certificat de signature de jetons auprès d’une autorité de certification publique.  
  
![signature des jetons](media/adfs2_fedserver_certstory_3.gif)  
  
-   Il existe un certificat de\-signature de jetons unique pour chaque serveur de Fédération d’une batterie de serveurs.  
  
    Lorsque vous utilisez plusieurs certificats uniques dans votre batterie de serveurs, chaque serveur de cette batterie signe des jetons avec sa propre clé privée unique.  
  
    Comme indiqué dans l’illustration suivante, vous pouvez obtenir un certificat de\-signature de jetons distinct pour chaque serveur de Fédération d’une batterie de serveurs. Cette option est plus coûteuse si vous prévoyez d’obtenir vos certificats\-de signature de jetons auprès d’une autorité de certification publique.  
  
![signature des jetons](media/adfs2_fedserver_certstory_4.gif)  
  
Pour plus d’informations sur l’installation d’un certificat quand vous utilisez les services de certificats Microsoft [en tant qu’autorité de certification d’entreprise, consultez IIS 7,0 : Créez un certificat de serveur de domaine dans](https://go.microsoft.com/fwlink/?LinkId=108548)IIS 7,0.  
  
Pour plus d’informations sur l’installation d’un certificat à partir d’une autorité de certification publique, voir [Demander un certificat de serveur Internet (IIS7)](https://go.microsoft.com/fwlink/?LinkId=108549).  
  
Pour plus d’informations sur l'\-installation d’un certificat [auto-signé, consultez IIS 7,0 : Créez un certificat\-de serveur auto-signé dans](https://go.microsoft.com/fwlink/?LinkID=108271)IIS 7,0.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
