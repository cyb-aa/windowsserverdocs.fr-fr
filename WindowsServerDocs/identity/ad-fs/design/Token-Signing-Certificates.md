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
ms.openlocfilehash: 9db69cfb2eb42af90b392433a6e05eaab9978160
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190809"
---
# <a name="token-signing-certificates"></a>Certificat de signature de jetons

Serveurs de fédération nécessitent le jeton\-certificats de signature pour empêcher les attaquants de modifier ou de contrefaçon des jetons de sécurité dans une tentative d’accéder aux ressources fédérés. Privé\/appariements de clé publique est utilisée avec le jeton\-certificats de signature sont le mécanisme de validation plus importantes de n’importe quel partenariat fédérée, car ces clés vérifier qu’un jeton de sécurité a été émis par un partenaire valid serveur de fédération et que le jeton n’a pas été modifié pendant le transit.  
  
## <a name="token-signing-certificate-requirements"></a>Jeton\-les exigences de certificat de signature  
Un jeton\-certificat de signature doit remplir les conditions suivantes pour travailler avec AD FS :  
  
-   Pour obtenir un jeton\-certificat pour signer un jeton de sécurité, le jeton de signature\-certificat de signature doit contenir une clé privée.  
  
-   Le compte de service AD FS doit avoir accès au jeton\-signature de clé privée du certificat dans le magasin personnel de l’ordinateur local. Cela est pris en charge par le programme d’installation. Vous pouvez également utiliser le composant logiciel enfichable Gestion AD FS\-pour vous assurer de cet accès si vous modifiez ultérieurement le jeton\-certificat de signature.  
  
> [!NOTE]  
> Il est une infrastructure à clé publique \(PKI\) méthode recommandée pour le partage pas la clé privée à plusieurs fins. Par conséquent, n’utilisez pas le certificat de communication de service que vous avez installé sur le serveur de fédération en tant que le jeton\-certificat de signature.  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>Comment le jeton\-certificats de signature sont utilisés entre les partenaires  
Chaque jeton\-certificat de signature contient des clés privées de chiffrement et des clés publiques qui sont utilisés pour signer numériquement \(au moyen de la clé privée\) un jeton de sécurité. Plus tard, lorsqu’ils sont reçus par un serveur de fédération du partenaire, ces clés de valider l’authenticité \(au moyen de la clé publique\) du jeton de sécurité chiffré.  
  
Étant donné que chaque jeton de sécurité est signé numériquement par le partenaire de compte, le partenaire de ressource peut vérifier que le jeton de sécurité a été en fait émis par le partenaire de compte et qu’il n’a pas été modifié. Signatures numériques sont vérifiées par la partie clé publique du jeton d’un partenaire\-certificat de signature. Une fois la signature est vérifiée, le serveur de fédération de ressources génère son propre jeton de sécurité pour son organisation et il signe le jeton de sécurité avec son propre jeton\-certificat de signature.  
  
Pour les environnements de partenaire de fédération, lorsque le jeton\-certificat de signature a été émis par une autorité de certification, vérifiez que :  
  
1.  Les listes de révocation de certificat \(CRL\) du certificat sont accessibles aux parties de confiance et de serveurs Web qui approuvent le serveur de fédération.  
  
2.  Le certificat d’autorité de certification racine est approuvé par les parties de confiance et les serveurs Web qui approuvent le serveur de fédération.  
  
Le serveur Web dans le partenaire de ressource utilise la clé publique du jeton\-certificat de signature pour vérifier que le jeton de sécurité est signé par le serveur de fédération de ressources. Le serveur Web permet ensuite à l’accès approprié au client.  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>Considérations relatives au déploiement pour le jeton\-certificats de signature  
Lorsque vous déployez le premier serveur de fédération dans une nouvelle installation d’AD FS, vous devez obtenir un jeton\-de signature de certificat et l’installer dans le magasin de certificats personnel d’ordinateur local sur ce serveur de fédération. Vous pouvez obtenir un jeton\-le certificat de signature en demandant un à partir d’une entreprise autorité de certification ou d’une autorité de certification publique ou en créant un libre-service\-certificat signé.  
  
Lorsque vous déployez une batterie de serveurs AD FS, le jeton\-certificats de signature sont installés différemment, selon la façon dont vous créez la batterie de serveurs.  
  
Il existe deux options de batterie de serveurs de serveur que vous pouvez envisager lorsque vous obtenez jeton\-certificats de signature à votre déploiement :  
  
-   Une clé privée à partir d’un seul jeton\-certificat de signature est partagé entre tous les serveurs de fédération dans une batterie de serveurs.  
  
    Dans un environnement de batterie de serveurs de fédération, nous recommandons que tous les serveurs de fédération partagent \(ou réutiliser\) le même jeton\-certificat de signature. Vous pouvez installer un seul jeton\-de signature de certificat à partir d’une autorité de certification sur un serveur de fédération et puis exporter la clé privée, tant que le certificat émis est marqué comme étant exportable.  
  
    Comme indiqué dans l’illustration suivante, la clé privée à partir d’un seul jeton\-certificat de signature peut être partagé à tous les serveurs de fédération dans une batterie de serveurs. Cette option, par rapport à ce qui suit « jeton unique\-certificat de signature « option : réduit les coûts si vous souhaitez obtenir un jeton\-signature de certificat à partir d’une autorité de certification publique.  
  
![signature de jetons](media/adfs2_fedserver_certstory_3.gif)  
  
-   Il existe un jeton unique\-signature de certificat pour chaque serveur de fédération dans une batterie de serveurs.  
  
    Lorsque vous utilisez plusieurs, les certificats uniques tout au long de votre batterie de serveurs, chaque serveur de cette batterie signe les jetons avec leur propre clé privée.  
  
    Comme indiqué dans l’illustration suivante, vous pouvez obtenir un jeton\-signature de certificat pour chaque serveur de fédération unique dans la batterie de serveurs. Cette option est plus coûteuse si vous souhaitez obtenir votre jeton\-signature des certificats à partir d’une autorité de certification publique.  
  
![signature de jetons](media/adfs2_fedserver_certstory_4.gif)  
  
Pour plus d’informations sur l’installation d’un certificat lorsque vous utilisez les Services de certificats Microsoft comme autorité de certification de votre entreprise, consultez [IIS 7.0 : Créer un certificat de serveur de domaine dans IIS 7.0](https://go.microsoft.com/fwlink/?LinkId=108548).  
  
Pour plus d’informations sur l’installation d’un certificat à partir d’une autorité de certification publique, voir [Demander un certificat de serveur Internet (IIS7)](https://go.microsoft.com/fwlink/?LinkId=108549).  
  
Pour plus d’informations sur l’installation d’un libre-service\-signature de certificat, consultez [IIS 7.0 : Créer un\-signé le certificat de serveur dans IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108271).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
