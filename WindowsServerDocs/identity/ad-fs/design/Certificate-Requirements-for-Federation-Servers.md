---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: Certificats requis pour les serveurs de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7eeff82ef3311f18c8252c44c96310fcf3c18217
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359185"
---
# <a name="certificate-requirements-for-federation-servers"></a>Certificats requis pour les serveurs de fédération

Dans n’importe quelle Services ADFS \(AD FS @ no__t-1, divers certificats doivent être utilisés pour sécuriser les communications et faciliter les authentifications des utilisateurs entre les clients Internet et les serveurs de Fédération. Chaque serveur de Fédération doit avoir un certificat de communication du service et un certificat @ no__t-0signing pour pouvoir participer à AD FS communications. Le tableau suivant décrit les types de certificats associés au serveur de Fédération.  
  
|Type de certificat|Description|  
|--------------------|---------------|  
|Jeton @ no__t-0signing|Un certificat @ no__t-0signing est un certificat x509. Les serveurs de Fédération utilisent des paires de clés publiques @ no__t-0private associées pour signer numériquement tous les jetons de sécurité qu’ils produisent. Cela inclut la signature des demandes de métadonnées de fédération et de résolution d'artefacts publiées.<br /><br />Vous pouvez avoir plusieurs certificats Token @ no__t-0signing configurés dans le composant logiciel enfichable de gestion AD FS @ no__t-1Dans pour permettre la substitution de certificat lorsqu’un certificat est proche de l’expiration. Par défaut, tous les certificats de la liste sont publiés, mais seul le certificat principal @ no__t-0signing est utilisé par AD FS pour signer les jetons. À chaque certificat que vous sélectionnez doit correspondre une clé privée.<br /><br />Pour plus d'informations, consultez [Token-Signing Certificates](Token-Signing-Certificates.md) et [Add a Token-Signing Certificate](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificat de communication du service|Les serveurs de Fédération utilisent un certificat d’authentification serveur, également appelé communication de service pour Windows Communication Foundation la sécurité des messages \(WCF @ no__t-1. Par défaut, il s’agit du même certificat que celui utilisé par un serveur de Fédération en tant que protocole SSL \(SSL @ no__t-1 dans Internet Information Services \(IIS @ no__t-3. **Remarque :** Le composant logiciel enfichable de gestion AD FS @ no__t-0in fait référence aux certificats d’authentification serveur pour les serveurs de Fédération en tant que certificats de communication de service.<br /><br />Pour plus d’informations, consultez [certificats de communication de service](Service-Communications-Certificates.md) et [définir un certificat de communication de service](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<br /><br />Étant donné que le certificat de communication du service doit être approuvé par les ordinateurs clients, nous vous recommandons d’utiliser un certificat signé par une autorité de certification approuvée \(CA @ no__t-1. À chaque certificat que vous sélectionnez doit correspondre une clé privée.|  
|Protocole SSL le certificat \(SSL @ no__t-1|Les serveurs de fédération utilisent un certificat SSL pour sécuriser le trafic des services web pour la communication SSL avec les clients web et avec les serveurs proxy de fédération.<br /><br />Comme le certificat SSL doit être approuvé par les ordinateurs clients, nous vous recommandons d'utiliser un certificat signé par une autorité de certification de confiance. À chaque certificat que vous sélectionnez doit correspondre une clé privée.|  
|Jeton @ no__t-0decryption|Ce certificat est utilisé pour déchiffrer les jetons reçus par ce serveur de Fédération.<br /><br />Vous pouvez avoir plusieurs certificats de déchiffrement. Cela permet à un serveur de Fédération de ressources d’être en mesure de déchiffrer les jetons émis avec un ancien certificat une fois qu’un nouveau certificat est défini en tant que certificat de déchiffrement principal. Tous les certificats peuvent être utilisés pour le déchiffrement, mais seul le certificat principal @ no__t-0decrypting est effectivement publié dans les métadonnées de Fédération. À chaque certificat que vous sélectionnez doit correspondre une clé privée.<br /><br />Pour plus d’informations, consultez [Ajouter un certificat de déchiffrement de jetons](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
Vous pouvez demander et installer un certificat SSL ou un certificat de communication de service en demandant un certificat de communication de service via la console de gestion Microsoft \(MMC @ no__t-1 Snap @ no__t-2in pour IIS. Pour plus d'informations générales sur l'utilisation des certificats SSL, consultez la page [IIS 7.0 : Configuration de protocole SSL dans IIS 7.0 @ no__t-0 et [IIS 7,0 : Configuration des certificats de serveur dans IIS 7.0 @ no__t-0.  
  
> [!NOTE]  
> Dans AD FS vous pouvez modifier le \(niveau SHA\) de l’algorithme de hachage sécurisé utilisé pour les signatures numériques\-pour SHA 1\-ou \(SHA 256\)plus sécurisé. AD FSdoes ne prend pas en charge l’utilisation de certificats avec d’autres méthodes de hachage, telles que MD5 \(the algorithme de hachage par défaut utilisé avec la commande Makecert. exe @ no__t-1line Tool @ no__t-2. En guise de meilleure pratique de sécurité, nous vous recommandons\-d' \(utiliser l’algorithme SHA\) 256 qui est défini par défaut pour toutes les signatures. SHA @ no__t-01 est recommandé pour une utilisation uniquement dans les scénarios où vous devez interagir avec un produit qui ne prend pas en charge les communications à l’aide de SHA @ no__t-1256, tel qu’un produit non-no__t-2Microsoft ou AD FS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Détermination de votre stratégie d'autorité de certification  
AD FS ne requiert pas que les certificats soient émis par une autorité de certification. Toutefois, le certificat SSL @no__t 0the qui est également utilisé par défaut en tant que certificat de communication du service @ no__t-1 doit être approuvé par les clients AD FS. Nous vous recommandons de ne pas utiliser de certificats auto @ no__t-0signed pour ces types de certificat.  
  
> [!IMPORTANT]  
> L’utilisation de self @ no__t-0signed, les certificats SSL dans un environnement de production peuvent permettre à un utilisateur malveillant d’une organisation partenaire de compte de prendre le contrôle des serveurs de Fédération dans une organisation partenaire de ressource. Ce risque de sécurité est dû au fait que les certificats auto-no__t-0signed sont des certificats racines. Ils doivent être ajoutés au magasin racine approuvé d’un autre serveur de Fédération @no__t 0for, le serveur de Fédération de ressources @ no__t-1, ce qui peut rendre ce serveur vulnérable aux attaques.  
  
Après avoir reçu un certificat d'une autorité de certification, assurez-vous que tous les certificats sont importés dans le magasin de certificats personnels de l'ordinateur local. Vous pouvez importer des certificats dans le magasin personnel avec le composant logiciel\-enfichable MMC Certificats.  
  
En guise d’alternative à l’utilisation des certificats, vous pouvez également importer le certificat SSL avec le composant logiciel enfichable Gestionnaire des services Internet @ no__t-1Dans au moment où vous affectez le certificat SSL au site Web par défaut. Pour plus d’informations, consultez [Importer un certificat d’authentification serveur sur le site Web par défaut](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Avant d’installer le logiciel AD FS sur l’ordinateur qui deviendra le serveur de Fédération, assurez-vous que les deux certificats se trouvent dans le magasin de certificats personnels de l’ordinateur local et que le certificat SSL est affecté au site Web par défaut. Pour plus d’informations sur l’ordre des tâches requises pour configurer un serveur de Fédération, voir [Checklist : Configuration d’un serveur de Fédération @ no__t-0.  
  
Suivant vos contraintes en termes de sécurité et de budget, déterminez attentivement, parmi vos certificats, ceux qui seront obtenus par une autorité de certification publique ou par une autorité de certification d'entreprise. La figure suivante montre les autorités de certification émettrices recommandées pour un type de certificat donné. Cette recommandation reflète une meilleure approche @ no__t-0practice en matière de sécurité et de coût.  
  
![conditions requises pour le certificat](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Listes de révocation de certificats  
Si des listes de révocation de certificats sont associées à l'un des certificats que vous utilisez, le serveur détenant le certificat configuré doit pouvoir contacter le serveur qui distribue les listes de révocation de certificats.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
