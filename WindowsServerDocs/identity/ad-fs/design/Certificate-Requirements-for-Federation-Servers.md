---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: "Certificats requis pour les serveurs de fédération"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 369c0e9e7ab1ef25baee1c35379cc66b886f20d8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="certificate-requirements-for-federation-servers"></a>Certificats requis pour les serveurs de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Dans toute conception \(ADFS\) ActiveDirectory Federation Services, différents certificats doivent être utilisés pour sécuriser les communications et faciliter les authentifications utilisateur entre les clients Internet et les serveurs de fédération. Chaque serveur de fédération doit avoir un certificat de communication du service et un certificat de signature token\ avant de pouvoir participer aux communications ADFS. Le tableau suivant décrit les types de certificats qui sont associés au serveur de fédération.  
  
|Type de certificat|Description|  
|--------------------|---------------|  
|Certificat de signature Token\|Un certificat de signature token\ est une X509 certificat. Serveurs de fédération utilisent des paires de clés associés public\/privée pour signer numériquement tous les jetons de sécurité qu’ils génèrent. Cela inclut la signature des demandes de résolution de métadonnées et des artefacts publiées de fédération.<br /><br />Vous pouvez avoir plusieurs certificats de signature token\ configurés dans la gestion ADFS enfichable pour autoriser la substitution de certificat de quand un seul certificat est sur le point d’expirer. Par défaut, tous les certificats dans la liste sont publiées, mais uniquement le certificat de signature token\ principal est utilisé par ADFS pour la signature des jetons. Tous les certificats que vous sélectionnez doivent avoir une clé privée correspondante.<br /><br />Pour plus d’informations, voir [certificats de signature de jetons](Token-Signing-Certificates.md) et [ajouter un certificat de signature de jetons](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificat de communication du service|Serveurs de fédération utilisent un certificat d’authentification serveur, également appelé communication du service pour WindowsCommunicationFoundation \(WCF\) la sécurité des messages. Par défaut, il s’agit du même certificat par un serveur de fédération en tant que le certificat Secure Sockets Layer \(SSL\) dans Internet Information Services \(IIS\). **Remarque:** la gestion ADFS enfichable fait référence aux certificats d’authentification serveur pour les serveurs de fédération en tant que certificats de communication du service.<br /><br />Pour plus d’informations, voir [certificats de communication du Service](Service-Communications-Certificates.md) et [définir un certificat de Communications de Service](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<br /><br />Étant donné que le certificat de communication du service doit être approuvé par les ordinateurs clients, nous vous recommandons d’utiliser un certificat signé par une autorité de certification approuvée \(CA\). Tous les certificats que vous sélectionnez doivent avoir une clé privée correspondante.|  
|Secure Sockets Layer \(SSL\) certificat|Serveurs de fédération utilisent un certificat SSL pour sécuriser le trafic des services Web pour la communication SSL avec les clients Web et des serveurs proxy de fédération.<br /><br />Étant donné que le certificat SSL doit être approuvé par les ordinateurs clients, nous vous recommandons d’utiliser un certificat signé par une autorité de certification approuvée. Tous les certificats que vous sélectionnez doivent avoir une clé privée correspondante.|  
|Certificat de déchiffrement de Token\|Ce certificat est utilisé pour déchiffrer les jetons qui sont reçues par ce serveur de fédération.<br /><br />Vous pouvez avoir plusieurs certificats de déchiffrement. Cela permet à un serveur de fédération de ressources être en mesure de déchiffrer les jetons émis avec un ancien après qu’un nouveau certificat est défini comme le certificat de déchiffrement principal. Tous les certificats peuvent être utilisés pour le déchiffrement, mais seul le certificat de déchiffrement token\ principal est publié dans les métadonnées de fédération. Tous les certificats que vous sélectionnez doivent avoir une clé privée correspondante.<br /><br />Pour plus d’informations, voir [ajouter un certificat de déchiffrement de jeton](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
Vous pouvez demander et installer un certificat SSL ou le certificat de communication du service en demandant un certificat de communication du service par le biais de la Console de gestion Microsoft \(MMC\) snap\-dans IIS. Pour plus d’informations sur l’utilisation de certificats SSL, consultez [IIS 7.0: configuration de SSL dans IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108544) et [IIS 7.0: configuration des certificats de serveur dans IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108545).  
  
> [!NOTE]  
> Dans ADFS, vous pouvez modifier le niveau \(SHA\) algorithme de hachage sécurisé utilisé pour les signatures numériques \(more secure\) SHA\-1 ou SHA\-256. AD FSdoes prend pas en charge l’utilisation de certificats avec d’autres méthodes de hachage, telles que MD5 \ (par défaut algorithme de hachage qui sont utilisé avec le tool\ de ligne de commande Makecert.exe). Comme meilleure pratique de sécurité, nous vous recommandons d’utiliser SHA\-256 \ (qui est définie par default\) pour toutes les signatures. SHA\-1 est recommandée pour utiliser uniquement dans les scénarios dans lesquels vous devez interagir avec un produit qui ne prend pas en charge les communications utilisant SHA\-256, comme un autre que celle Microsoft produit ou ADFS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Déterminer votre stratégie d’autorité de certification  
ADFS ne nécessite pas que les certificats être émis par une autorité de certification. Toutefois, le certificat SSL \ (le certificat qui est également utilisé par défaut comme le certificate\ de communications de service) doit être approuvé par les clients ADFS. Nous vous conseillons pas certificats signés intégrée pour ces types de certificat.  
  
> [!IMPORTANT]  
> Utilisation de la signature intégrée, les certificats SSL dans un environnement de production peuvent autoriser un utilisateur malveillant dans une organisation partenaire de compte pour prendre le contrôle de serveurs de fédération dans une organisation partenaire de ressource. Ce risque de sécurité existe, car les certificats signés par intégrée sont des certificats racines. Ils doivent être ajoutés au magasin racine approuvé d’un autre serveur de fédération \ (par exemple, le serveur ressource fédération), ce qui peut ce serveur vulnérable aux attaques.  
  
Après avoir reçu un certificat à partir d’une autorité de certification, assurez-vous que tous les certificats sont importés dans le magasin de certificats personnel de l’ordinateur local. Vous pouvez importer des certificats dans le magasin personnel avec le composant enfichable MMC.  
  
Comme alternative à l’aide des certificats de composants, vous pouvez également importer le certificat SSL avec le Gestionnaire des services Internet enfichable au moment que vous affectez le certificat SSL au site Web par défaut. Pour plus d’informations, voir [importer un certificat d’authentification serveur sur le site Web par défaut](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Avant d’installer le logiciel ADFS sur l’ordinateur qui deviendra le serveur de fédération, assurez-vous que les deux certificats se trouvent dans le magasin de certificats personnel d’ordinateur Local et que le certificat SSL est affecté au site Web par défaut. Pour plus d’informations sur l’ordre des tâches qui sont requises pour configurer un serveur de fédération, consultez [liste de vérification: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
En fonction de vos exigences de sécurité et budget, étudiez attentivement parmi vos certificats seront obtenus par un public autorité de certification ou d’une autorité de certification d’entreprise. L’illustration suivante montre les émetteurs d’autorité de certification recommandées pour un type de certificat. Cette recommandation reflète une approche best\ pratiques concernant la sécurité et de coût.  
  
![Configuration requise du certificat](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Listes de révocation de certificats  
Si un certificat que vous utilisez dispose des listes de révocation, le serveur avec le certificat configuré doit être en mesure de contacter le serveur qui distribue les CRL.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
