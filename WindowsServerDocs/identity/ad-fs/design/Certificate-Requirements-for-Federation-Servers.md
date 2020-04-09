---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: Certificats requis pour les serveurs de fédération
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9154e168fa03b08177dd0125fcbc1707d8e5594f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853212"
---
# <a name="certificate-requirements-for-federation-servers"></a>Certificats requis pour les serveurs de fédération

Dans les Services ADFS \(AD FS de la conception de\), divers certificats doivent être utilisés pour sécuriser les communications et faciliter les authentifications des utilisateurs entre les clients Internet et les serveurs de Fédération. Chaque serveur de Fédération doit avoir un certificat de communication du service et un jeton\-certificat de signature avant de pouvoir participer aux communications AD FS. Le tableau suivant décrit les types de certificats associés au serveur de Fédération.  
  
|Type de certificat|Description|  
|--------------------|---------------|  
|Jeton\-certificat de signature|Un jeton\-certificat de signature est un certificat x509. Les serveurs de Fédération utilisent des paires de clés publiques\/privées associées pour signer numériquement tous les jetons de sécurité qu’ils produisent. Cela inclut la signature des demandes de métadonnées de fédération et de résolution d'artefacts publiées.<p>Vous pouvez avoir plusieurs certificats de signature de\-de jeton configurés dans le composant logiciel enfichable Gestion des AD FS\-dans pour permettre la substitution de certificat lorsqu’un certificat est proche de l’expiration. Par défaut, tous les certificats de la liste sont publiés, mais seul le jeton principal\-certificat de signature est utilisé par AD FS pour signer des jetons. À chaque certificat que vous sélectionnez doit correspondre une clé privée.<p>Pour plus d'informations, consultez [Certificat de signature de jetons](Token-Signing-Certificates.md) et [Ajouter un certificat de signature de jetons](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificat de communication du service|Les serveurs de Fédération utilisent un certificat d’authentification serveur, également appelé communication de service pour Windows Communication Foundation \(WCF\) la sécurité des messages. Par défaut, il s’agit du même certificat que celui utilisé par un serveur de Fédération en tant que protocole SSL \(SSL\) certificat dans Internet Information Services \(IIS\). **Remarque :** Le\-du composant logiciel enfichable Gestion de AD FS dans fait référence aux certificats d’authentification serveur pour les serveurs de Fédération en tant que certificats de communication de service.<p>Pour plus d’informations, consultez [certificats de communication de service](Service-Communications-Certificates.md) et [définir un certificat de communication de service](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<p>Étant donné que le certificat de communication du service doit être approuvé par les ordinateurs clients, nous vous recommandons d’utiliser un certificat signé par une autorité de certification approuvée \(\)d’autorité de certification. À chaque certificat que vous sélectionnez doit correspondre une clé privée.|  
|Protocole SSL \(SSL\) certificat|Les serveurs de fédération utilisent un certificat SSL pour sécuriser le trafic des services web pour la communication SSL avec les clients web et avec les serveurs proxy de fédération.<p>Comme le certificat SSL doit être approuvé par les ordinateurs clients, nous vous recommandons d'utiliser un certificat signé par une autorité de certification de confiance. À chaque certificat que vous sélectionnez doit correspondre une clé privée.|  
|Certificat de déchiffrement de jeton\-|Ce certificat est utilisé pour déchiffrer les jetons reçus par ce serveur de Fédération.<p>Vous pouvez avoir plusieurs certificats de déchiffrement. Cela permet à un serveur de Fédération de ressources d’être en mesure de déchiffrer les jetons émis avec un ancien certificat une fois qu’un nouveau certificat est défini en tant que certificat de déchiffrement principal. Tous les certificats peuvent être utilisés pour le déchiffrement, mais seul le jeton principal\-le déchiffrement du certificat est réellement publié dans les métadonnées de Fédération. À chaque certificat que vous sélectionnez doit correspondre une clé privée.<p>Pour plus d’informations, consultez [Ajouter un certificat de déchiffrement de jetons](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
Vous pouvez demander et installer un certificat SSL ou un certificat de communication de service en demandant un certificat de communication de service via la console MMC (Microsoft Management Console) \(MMC\) Snap\-dans pour IIS. Pour plus d’informations générales sur l’utilisation des certificats SSL, consultez [iis 7,0 : configuration de protocole SSL dans iis 7,0](https://go.microsoft.com/fwlink/?LinkID=108544) et [IIS 7,0 : configuration des certificats de serveur dans IIS 7,0](https://go.microsoft.com/fwlink/?LinkID=108545) .  
  
> [!NOTE]  
> Dans AD FS vous pouvez modifier l’algorithme de hachage sécurisé \(niveau de\) SHA utilisé pour les signatures numériques pour SHA\-1 ou SHA\-256 \(plus sécurisé\). AD FSdoes ne prend pas en charge l’utilisation de certificats avec d’autres méthodes de hachage, telles que MD5 \(l’algorithme de hachage par défaut utilisé avec la commande Makecert. exe\-\)de l’outil de ligne. Pour des raisons de sécurité, nous vous recommandons d’utiliser l’algorithme SHA\-256 \(qui est défini par défaut\) pour toutes les signatures. L’utilisation de l’algorithme SHA\-1 est recommandée dans les scénarios où vous devez interagir avec un produit qui ne prend pas en charge les communications utilisant l’algorithme SHA\-256, tel qu’un produit Microsoft non\-ou un AD FS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Détermination de votre stratégie d'autorité de certification  
AD FS ne requiert pas que les certificats soient émis par une autorité de certification. Toutefois, le certificat SSL \(le certificat qui est également utilisé par défaut en tant que certificat de communication du service\) doit être approuvé par les clients AD FS. Nous vous recommandons de ne pas utiliser de certificats auto\-signés pour ces types de certificat.  
  
> [!IMPORTANT]  
> L’utilisation de certificats SSL auto\-signés dans un environnement de production peut permettre à un utilisateur malveillant d’une organisation partenaire de compte de prendre le contrôle des serveurs de Fédération dans une organisation partenaire de ressource. Ce risque de sécurité est dû au fait que les certificats auto\-signés sont des certificats racines. Ils doivent être ajoutés au magasin racine approuvé d’un autre serveur de Fédération \(par exemple, le serveur de Fédération de ressources\), ce qui peut rendre ce serveur vulnérable aux attaques.  
  
Après avoir reçu un certificat d'une autorité de certification, assurez-vous que tous les certificats sont importés dans le magasin de certificats personnels de l'ordinateur local. Vous pouvez importer des certificats dans le magasin personnel avec le composant logiciel enfichable MMC Certificats\-dans.  
  
Au lieu d’utiliser le composant logiciel enfichable Certificats\-dans, vous pouvez également importer le certificat SSL à l’aide du composant logiciel enfichable Gestionnaire des services Internet\-dans au moment où vous affectez le certificat SSL au site Web par défaut. Pour plus d’informations, consultez [Importer un certificat d’authentification serveur sur le site Web par défaut](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Avant d’installer le logiciel AD FS sur l’ordinateur qui deviendra le serveur de Fédération, assurez-vous que les deux certificats se trouvent dans le magasin de certificats personnels de l’ordinateur local et que le certificat SSL est affecté au site Web par défaut. Pour plus d’informations sur l’ordre des tâches requises pour configurer un serveur de Fédération, consultez Liste de [vérification : configuration d’un serveur de Fédération](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
Suivant vos contraintes en termes de sécurité et de budget, déterminez attentivement, parmi vos certificats, ceux qui seront obtenus par une autorité de certification publique ou par une autorité de certification d'entreprise. La figure suivante montre les autorités de certification émettrices recommandées pour un type de certificat donné. Cette recommandation reflète une meilleure approche en matière de\-pour la sécurité et les coûts.  
  
![conditions requises pour le certificat](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Listes de révocation de certificats  
Si des listes de révocation de certificats sont associées à l'un des certificats que vous utilisez, le serveur détenant le certificat configuré doit pouvoir contacter le serveur qui distribue les listes de révocation de certificats.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
