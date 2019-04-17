---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: Certificats de signature de jetons
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 84e106ec87861960d7de651b4aaa0e88c952aa79
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="token-signing-certificates"></a>Certificats de signature de jetons

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Serveurs de fédération requièrent des certificats de signature de token\ pour empêcher les intrus de modifier ou de contrefaçon de jetons de sécurité dans une tentative d’accès non autorisé aux ressources fédérées. La clé publique/private\ couplage qui est utilisé avec des certificats de signature de token\ est le mécanisme de validation plus important de n’importe quel partenariat fédérée dans la mesure où ces clés de vérifier qu’un jeton de sécurité a été émis par un serveur de fédération partenaire valide et que le jeton n’a pas été modifié au cours du transit.  
  
## <a name="token-signing-certificate-requirements"></a>Signature Token\ certificats requis  
Un certificat de signature token\ doit répondre aux exigences suivantes pour travailler avec ADFS:  
  
-   Pour un certificat de signature token\ signer un jeton de sécurité, le certificat de signature token\ doit contenir une clé privée.  
  
-   Le compte de service ADFS doit avoir accès à la clé privée du certificat signature token\ dans le magasin personnel de l’ordinateur local. Cela est prise en charge par le programme d’installation. Vous pouvez également utiliser la gestion AD FS enfichable pour vous assurer de cet accès si vous modifiez par la suite le certificat de signature token\.  
  
> [!NOTE]  
> Il est recommandé \(PKI\) infrastructure à clé publique à ne pas partager la clé privée pour couvrir plusieurs. Par conséquent, n’utilisez pas le certificat de communication du service que vous avez installé sur le serveur de fédération en tant que le certificat de signature token\.  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>Utilisation des certificats de signature de token\ entre les partenaires  
Chaque certificat de signature de token\ contient des clés privées de chiffrement et des clés publiques qui sont utilisés pour signer numériquement \ (au moyen de la clé privée) un jeton de sécurité. Plus tard, après leur réception par un serveur de fédération du partenaire, ces clés de valider l’authenticité \ (au moyen de la clé publique) du jeton de sécurité chiffré.  
  
Étant donné que chaque jeton de sécurité est signé numériquement par le partenaire de compte, le partenaire de ressource peut vérifier que le jeton de sécurité a été émis en fait par le partenaire de compte et qu’il n’a pas été modifié. Signatures numériques sont vérifiées par la partie clé publique du certificat de token\-qui chante d’un partenaire. Une fois la signature est vérifiée, le serveur de fédération de ressources génère son propre jeton de sécurité pour son organisation et il connecte le jeton de sécurité avec son propre certificat de signature token\.  
  
Pour les environnements de partenaire de fédération, lorsque le certificat de signature token\ a été émis par une autorité de certification, assurez-vous que:  
  
1.  Le certificat révocation listes \(CRLs\) du certificat sont accessibles aux parties de confiance et des serveurs Web qui approuvent le serveur de fédération.  
  
2.  Le certificat d’autorité de certification racine est approuvé par les parties de confiance et les serveurs Web qui approuvent le serveur de fédération.  
  
Le serveur Web du partenaire de ressource utilise la clé publique du certificat de signature de token\ pour vérifier que le jeton de sécurité est signé par le serveur de fédération de ressources. Le serveur Web permet ensuite de l’accès approprié au client.  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>Considérations relatives au déploiement de certificats de signature de token\  
Lorsque vous déployez le premier serveur de fédération dans une nouvelle installation d’ADFS, vous devez obtenir un certificat de signature token\ et l’installer dans le magasin de certificats personnel d’ordinateur local sur ce serveur de fédération. Vous pouvez obtenir une token\ signature de certificat en demandant un à partir d’une entreprise autorité de certification ou d’une autorité de certification publique ou en créant un certificat signé intégrée.  
  
Lorsque vous déployez une batterie ADFS, les certificats de signature de token\ sont installés différemment, en fonction de la création de la batterie de serveurs.  
  
Il existe deux options de la batterie de serveur que vous pouvez prendre en compte lorsque vous obtenez des certificats de signature de token\ pour votre déploiement:  
  
-   Une clé privée à partir d’un certificat de signature de token\ est partagée entre tous les serveurs de fédération dans une batterie de serveurs.  
  
    Dans un environnement de batterie de serveurs de fédération, nous recommandons que tous les serveurs de fédération partagent \(or reuse\) le même certificat de signature de token\. Vous pouvez installer un certificat de signature token\ unique à partir d’une autorité de certification sur un serveur de fédération et puis exporter la clé privée, tant que le certificat émis est marqué comme étant exportable.  
  
    Comme indiqué dans l’illustration suivante, la clé privée à partir d’un certificat de signature token\ unique peut être partagée à tous les serveurs de fédération dans une batterie de serveurs. Cette option, par rapport à la suivant «unique token\ certificat de signature de» option: réduit les coûts si vous prévoyez d’obtenir un certificat de signature token\ à partir d’une autorité de certification publique.  
  
![Signature de jetons](media/adfs2_fedserver_certstory_3.gif)  
  
-   Il existe un certificat de signature token\ unique pour chaque serveur de fédération dans une batterie de serveurs.  
  
    Lorsque vous utilisez plusieurs, les certificats unique tout au long de votre batterie de serveurs, chaque serveur de cette batterie se connecte jetons avec sa propre clé privée unique.  
  
    Comme indiqué dans l’illustration suivante, vous pouvez obtenir un certificat de signature token\ distinct pour chaque serveur de fédération unique dans la batterie de serveurs. Cette option n’est plus onéreuse, si vous prévoyez d’obtenir vos certificats de signature de token\ à partir d’une autorité de certification publique.  
  
![Signature de jetons](media/adfs2_fedserver_certstory_4.gif)  
  
Pour plus d’informations sur l’installation d’un certificat lorsque vous utilisez des Services de certificats Microsoft comme votre autorité de certification d’entreprise, consultez [IIS 7.0: créer un certificat de serveur de domaine dans IIS 7.0](https://go.microsoft.com/fwlink/?LinkId=108548).  
  
Pour plus d’informations sur l’installation d’un certificat à partir d’une autorité de certification publique, consultez [IIS 7.0: demander un certificat de serveur Internet](https://go.microsoft.com/fwlink/?LinkId=108549).  
  
Pour plus d’informations sur l’installation d’un certificat signé intégrée, consultez [IIS 7.0: créer un certificat de serveur Self\-Signed dans IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108271).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
