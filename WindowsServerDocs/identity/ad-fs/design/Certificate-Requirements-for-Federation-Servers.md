---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: Certificats requis pour les serveurs de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ce301f6320ed3347b1ee802f57c2b2ebd4394970
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191641"
---
# <a name="certificate-requirements-for-federation-servers"></a>Certificats requis pour les serveurs de fédération

Dans les Services de fédération Active Directory \(AD FS\) conception, différents certificats doivent être utilisés pour sécuriser les communications et faciliter les authentifications utilisateur entre les clients Internet et les serveurs de fédération. Chaque serveur de fédération doit posséder un certificat de communication de service et un jeton\-avant de pouvoir participer aux communications AD FS le certificat de signature. Le tableau suivant décrit les types de certificats qui sont associés au serveur de fédération.  
  
|Type de certificat|Description|  
|--------------------|---------------|  
|Jeton\-certificat de signature|Un jeton\-certificat de signature est un X509 certificat. Serveurs de fédération utilisent public associé\/des paires de clés privées pour signer numériquement tous les jetons de sécurité qu’ils génèrent. Cela inclut la signature des demandes de métadonnées de fédération et de résolution d'artefacts publiées.<br /><br />Vous pouvez avoir plusieurs jeton\-signature de certificats configurés dans le composant logiciel enfichable Gestion AD FS\-pour autoriser la substitution de certificat de quand un certificat est point d’expirer. Par défaut, tous les certificats dans la liste sont publiés, mais uniquement le jeton principal\-certificat de signature est utilisé par AD FS pour la signature des jetons. À chaque certificat que vous sélectionnez doit correspondre une clé privée.<br /><br />Pour plus d'informations, consultez [Token-Signing Certificates](Token-Signing-Certificates.md) et [Add a Token-Signing Certificate](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificat de communication du service|Serveurs de fédération utilisent un certificat d’authentification serveur, également appelé une communication de service pour Windows Communication Foundation \(WCF\) sécurité de Message. Par défaut, il s’agit du même certificat qu’un serveur de fédération utilise comme Secure Sockets Layer \(SSL\) certificat dans Internet Information Services \(IIS\). **Remarque :** Le composant logiciel enfichable Gestion AD FS\-dans fait référence aux certificats d’authentification serveur pour les serveurs de fédération en tant que certificats de communication de service.<br /><br />Pour plus d’informations, consultez [certificats de Communications de Service](Service-Communications-Certificates.md) et [définir un certificat de Communications de Service](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<br /><br />Étant donné que le certificat de communication de service doit être approuvé par les ordinateurs clients, nous vous recommandons d’utiliser un certificat signé par une autorité de certification approuvée \(autorité de certification\). À chaque certificat que vous sélectionnez doit correspondre une clé privée.|  
|Secure Sockets Layer \(SSL\) certificat|Les serveurs de fédération utilisent un certificat SSL pour sécuriser le trafic des services web pour la communication SSL avec les clients web et avec les serveurs proxy de fédération.<br /><br />Comme le certificat SSL doit être approuvé par les ordinateurs clients, nous vous recommandons d'utiliser un certificat signé par une autorité de certification de confiance. À chaque certificat que vous sélectionnez doit correspondre une clé privée.|  
|Jeton\-certificat de déchiffrement|Ce certificat est utilisé pour déchiffrer les jetons qui sont reçus par ce serveur de fédération.<br /><br />Vous pouvez avoir plusieurs certificats de déchiffrement. Cela rend possible pour un serveur de fédération de ressources être en mesure de déchiffrer les jetons sont émis avec un ancien après qu’un nouveau certificat est défini en tant que le certificat de déchiffrement principal. Tous les certificats peuvent être utilisés pour le déchiffrement, mais uniquement le jeton principal\-certificat de déchiffrement est publié dans les métadonnées de fédération. À chaque certificat que vous sélectionnez doit correspondre une clé privée.<br /><br />Pour plus d’informations, consultez [ajouter un certificat de déchiffrement de jetons](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
Vous pouvez demander et installer un certificat SSL ou un certificat de communication du service en demandant un certificat de communication de service via la Console de gestion Microsoft \(MMC\) aligner\-dans IIS. Pour plus d'informations générales sur l'utilisation des certificats SSL, consultez la page [IIS 7.0 : Configuration sécurisée du protocole SSL dans IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108544) et [IIS 7.0 : Configuration des certificats de serveur dans IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108545) .  
  
> [!NOTE]  
> Dans AD FS, vous pouvez modifier l’algorithme de hachage sécurisé \(SHA\) niveau qui est utilisé pour les signatures numériques pour soit SHA\-1 ou SHA\-256 \(plus sécurisées\). AD FSdoes prend pas en charge l’utilisation de certificats avec d’autres méthodes de hachage, telles que MD5 \(l’algorithme de hachage par défaut qui est utilisé avec la commande Makecert.exe\-outil en ligne\). Pour des raisons de sécurité, nous vous recommandons d’utiliser SHA\-256 \(qui a la valeur par défaut\) pour toutes les signatures. SHA\-1 est recommandé d’utiliser uniquement dans les scénarios où vous devez interagir avec un produit qui ne prend pas en charge les communications à l’aide de SHA\-256, par exemple un non\-produit Microsoft ou AD FS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Détermination de votre stratégie d'autorité de certification  
AD FS ne requiert pas qu’une autorité de certification émette des certificats. Toutefois, le certificat SSL \(le certificat qui est également utilisé par défaut comme certificat de communication du service\) doit être approuvé par les clients AD FS. Nous vous conseillons pas self\-certificats auto-signés pour ces types de certificats.  
  
> [!IMPORTANT]  
> Utilisation de lui-même\-signé, les certificats SSL dans un environnement de production peuvent autoriser un utilisateur malveillant dans une organisation partenaire de compte pour prendre le contrôle des serveurs de fédération dans une organisation partenaire de ressource. Ce risque de sécurité existe car self\-certificats auto-signés sont des certificats racines. Ils doivent être ajoutés au magasin racine approuvé d’un autre serveur de fédération \(, par exemple, le serveur de fédération de ressources\), ce qui ce serveur vulnérable aux attaques.  
  
Après avoir reçu un certificat d'une autorité de certification, assurez-vous que tous les certificats sont importés dans le magasin de certificats personnels de l'ordinateur local. Vous pouvez importer des certificats dans le magasin personnel avec le composant logiciel enfichable MMC Certificats\-dans.  
  
Comme alternative à l’aide des certificats d’alignement\-dans, vous pouvez également importer le certificat SSL avec le composant logiciel enfichable Gestionnaire des services Internet\-au moment que vous attribuez le protocole SSL de certificat au site Web par défaut. Pour plus d’informations, consultez [importer un certificat d’authentification serveur pour le Site Web par défaut](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Avant d’installer le logiciel AD FS sur l’ordinateur qui deviendra le serveur de fédération, assurez-vous que les deux certificats se trouvent dans le magasin de certificats personnel d’ordinateur Local et que le certificat SSL est affecté au Site Web par défaut. Pour plus d’informations sur l’ordre des tâches qui sont requises pour configurer un serveur de fédération, consultez [liste de vérification : Configuration d’un serveur de fédération](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
Suivant vos contraintes en termes de sécurité et de budget, déterminez attentivement, parmi vos certificats, ceux qui seront obtenus par une autorité de certification publique ou par une autorité de certification d'entreprise. La figure suivante montre les autorités de certification émettrices recommandées pour un type de certificat donné. Cette recommandation reflète une meilleure\-pratique approche concernant la sécurité et les coûts.  
  
![spécifications du certificat](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Listes de révocation de certificats  
Si des listes de révocation de certificats sont associées à l'un des certificats que vous utilisez, le serveur détenant le certificat configuré doit pouvoir contacter le serveur qui distribue les listes de révocation de certificats.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
