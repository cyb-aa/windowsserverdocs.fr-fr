---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: Exporter la partie clé privée d'un certificat d'authentification serveur
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8e1bbeddc4bae1c420b6cc78b52d6b873320ae8f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359578"
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>Exporter la partie clé privée d'un certificat d'authentification serveur

Chaque serveur de Fédération d’une batterie de serveurs Services ADFS \(AD FS @ no__t-1 doit avoir accès à la clé privée du certificat d’authentification serveur. Si vous implémentez une batterie de serveurs de serveurs de Fédération ou de serveurs Web, vous devez disposer d’un certificat d’authentification unique. Ce certificat doit être émis par une autorité de certification d’entreprise @no__t 0CA @ no__t-1, et il doit avoir une clé privée exportable. La clé privée du certificat d'authentification serveur doit être exportable pour être mise à la disposition de tous les serveurs de la batterie.  
  
Ce même concept est vrai pour les batteries de serveurs proxy de Fédération dans le sens où tous les serveurs proxys de Fédération d’une batterie de serveurs doivent partager la partie clé privée du même certificat d’authentification serveur.  
  
> [!NOTE]  
> Le composant logiciel enfichable de gestion AD FS @ no__t-0in fait référence aux certificats d’authentification serveur pour les serveurs de Fédération en tant que certificats de communication de service.  
  
En fonction du rôle joué par cet ordinateur, procédez comme suit sur l’ordinateur du serveur de Fédération ou le serveur proxy de Fédération sur lequel vous avez installé le certificat d’authentification serveur avec la clé privée. Une fois la procédure terminée, importez ce certificat sur le site web par défaut de chaque serveur de la batterie de serveurs. Pour plus d’informations, consultez [Importer un certificat d’authentification serveur sur le site Web par défaut](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>Pour exporter la partie clé privée d'un certificat d'authentification serveur  
  
1. Dans l’écran d' **Accueil** , tapez**Internet Information Services \(IIS @ No__t-3 Manager**, puis appuyez sur entrée.  
  
2. Dans l'arborescence de la console, cliquez sur **Nom_Ordinateur**.  
  
3. Dans le volet central, doublez les certificats de **serveur**@ no__t-0click.  
  
4. Dans le volet central, cliquez avec le bouton droit sur @ no__t-0click le certificat que vous souhaitez exporter, puis cliquez sur **Exporter**.  
  
5. Dans la boîte de dialogue **exporter un certificat** , cliquez sur **...** .  
  
6. Dans **nom de fichier**, tapez **C : \\** <em>NameofCertificate</em>, puis cliquez sur **ouvrir**.  
  
7. Tapez le mot de passe du certificat, confirmez-le, puis cliquez sur **OK**.  
  
8. Validez l'exportation en confirmant que le fichier spécifié a été créé à l'emplacement approprié.  
  
   > [!IMPORTANT]  
   > Pour pouvoir importer ce certificat dans le magasin de certificats local sur le nouveau serveur, vous devez transférer le fichier sur un média physique et assurer son transport en toute sécurité vers le nouveau serveur. Il est essentiel de garantir la sécurité de la clé privée. Si cette clé est compromise, la sécurité de la totalité de votre AD FS déploiement @no__t des ressources 0including au sein de votre organisation et dans les organisations partenaires de ressources @ no__t-1 est compromise.  
  
9. Importez le certificat d'authentification serveur exporté dans le magasin de certificats sur le nouveau serveur avant d'installer le service de fédération. Pour plus d’informations sur l’importation du certificat, voir Importer un certificat de serveur \([http : @no__t -2\/go.microsoft.com @ no__t-4fwlink @ no__t-5 ? LinkId @ no__t-6108283](https://go.microsoft.com/fwlink/?LinkId=108283)\).  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Liste de vérification : configuration d’un serveur de fédération proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Certificats requis pour les serveurs proxy de fédération](https://technet.microsoft.com/library/dd807054.aspx)  
  

