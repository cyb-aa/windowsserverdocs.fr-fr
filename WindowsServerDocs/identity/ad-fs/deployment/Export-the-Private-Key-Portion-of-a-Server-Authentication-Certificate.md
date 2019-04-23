---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: Exporter la partie clé privée d'un certificat d'authentification serveur
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c968f0702d56b56d0a80459e5cf0c9e658c56741
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857190"
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>Exporter la partie clé privée d'un certificat d'authentification serveur

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Chaque serveur de fédération dans un Active Directory Federation Services \(AD FS\) batterie de serveurs doit avoir accès à la clé privée du certificat d’authentification serveur. Si vous implémentez une batterie de serveurs de fédération ou de serveurs Web, vous devez disposer d’un certificat d’authentification unique. Ce certificat doit être émis par une autorité de certification d’entreprise \(autorité de certification\), et il doit avoir une clé privée exportable. La clé privée du certificat d'authentification serveur doit être exportable pour être mise à la disposition de tous les serveurs de la batterie.  
  
Ce même concept s’applique des batteries de serveurs de fédération serveur proxy en ce sens que tous les serveurs proxy de fédération dans une batterie de serveurs doivent partager la partie clé privée du certificat d’authentification serveur même.  
  
> [!NOTE]  
> Le composant logiciel enfichable Gestion AD FS\-dans fait référence aux certificats d’authentification serveur pour les serveurs de fédération en tant que certificats de communication de service.  
  
En fonction du rôle cet ordinateur, procédez comme suit sur le serveur de fédération ou le serveur proxy de fédération où vous avez installé le certificat d’authentification serveur avec la clé privée. Une fois la procédure terminée, importez ce certificat sur le site web par défaut de chaque serveur de la batterie de serveurs. Pour plus d’informations, consultez [importer un certificat d’authentification serveur pour le Site Web par défaut](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>Pour exporter la partie clé privée d'un certificat d'authentification serveur  
  
1.  Sur le **Démarrer** , tapez**Internet Information Services \(IIS\) Manager**, puis appuyez sur ENTRÉE.  
  
2.  Dans l'arborescence de la console, cliquez sur **Nom_Ordinateur**.  
  
3.  Dans le volet central, double-cliquez\-cliquez sur **certificats de serveur**.  
  
4.  Dans le volet central, avec le bouton droit\-cliquez sur le certificat que vous souhaitez exporter, puis cliquez sur **exporter**.  
  
5.  Dans le **exporter le certificat** boîte de dialogue, cliquez sur le **...** .  
  
6.  Dans **nom de fichier**, type **C:\\*** Nom_du_certificat*, puis cliquez sur **Open**.  
  
7.  Tapez le mot de passe du certificat, confirmez-le, puis cliquez sur **OK**.  
  
8.  Validez l'exportation en confirmant que le fichier spécifié a été créé à l'emplacement approprié.  
  
    > [!IMPORTANT]  
    > Pour pouvoir importer ce certificat dans le magasin de certificats local sur le nouveau serveur, vous devez transférer le fichier sur un média physique et assurer son transport en toute sécurité vers le nouveau serveur. Il est essentiel de garantir la sécurité de la clé privée. Si cette clé est compromise, la sécurité de tout votre déploiement AD FS \(, y compris des ressources au sein de votre organisation et dans les organisations partenaires de ressource\) est compromis.  
  
9. Importez le certificat d'authentification serveur exporté dans le magasin de certificats sur le nouveau serveur avant d'installer le service de fédération. Pour plus d’informations sur la façon d’importer le certificat, voir Importer un certificat de serveur \( [http :\/\/go.microsoft.com\/fwlink\/? LinkId\=108283](https://go.microsoft.com/fwlink/?LinkId=108283)\).  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Liste de vérification : Configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Configuration requise des certificats pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Configuration requise des certificats pour les serveurs proxy de fédération](https://technet.microsoft.com/library/dd807054.aspx)  
  

