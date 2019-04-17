---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: "Exporter la partie clé privée d’un certificat d’authentification serveur"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c968f0702d56b56d0a80459e5cf0c9e658c56741
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>Exporter la partie clé privée d’un certificat d’authentification serveur

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Chaque serveur de fédération dans une batterie de serveurs ActiveDirectory Federation Services \(ADFS\) doit avoir accès à la clé privée du certificat d’authentification serveur. Si vous implémentez une batterie de serveurs Web ou serveurs de fédération, vous devez disposer d’un certificat d’authentification unique. Ce certificat doit être émis par une autorité de certification d’entreprise \(CA\) et il doivent avoir une clé privée exportable. La clé privée du certificat d’authentification serveur doit être exportable afin qu’il peut être rendu disponible pour tous les serveurs de la batterie de serveurs.  
  
Ce même concept s’applique des batteries de serveurs de fédération serveur proxy en ce sens que tous les serveurs proxy de fédération dans une batterie de serveurs doit partager la partie clé privée du même certificat d’authentification serveur.  
  
> [!NOTE]  
> La gestion ADFS enfichable fait référence aux certificats d’authentification serveur pour les serveurs de fédération en tant que certificats de communication du service.  
  
En fonction du rôle cet ordinateur, procédez comme suit sur l’ordinateur du serveur de fédération ou d’un ordinateur du serveur proxy de fédération où vous avez installé le certificat d’authentification serveur avec la clé privée. Lorsque vous avez terminé la procédure, vous pouvez ensuite importer ce certificat sur le site Web par défaut de chaque serveur de la batterie de serveurs. Pour plus d’informations, voir [importer un certificat d’authentification serveur sur le site Web par défaut](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>Pour exporter la partie clé privée d’un certificat d’authentification serveur  
  
1.  Sur le **Démarrer**, tapez**\(IIS\) Internet Information Services Manager**, puis appuyez sur ENTRÉE.  
  
2.  Dans l’arborescence de la console, cliquez sur **Nom_Ordinateur**.  
  
3.  Dans le volet central, double-cliquant sur **certificats de serveur**.  
  
4.  Dans le volet central, cliquez droit sur le certificat que vous voulez exporter, puis cliquez sur **exporter**.  
  
5.  Dans le **exporter un certificat** boîte de dialogue, cliquez sur le **...** bouton.  
  
6.  Dans **nom de fichier**, type **C:\\***Nom_du_certificat*, puis cliquez sur **ouvrir**.  
  
7.  Tapez un mot de passe pour le certificat, confirmez-le, puis cliquez sur **OK**.  
  
8.  Valider la réussite de l’exportation en confirmant que le fichier spécifié est créé à l’emplacement spécifié.  
  
    > [!IMPORTANT]  
    > Afin que ce certificat peut être importé vers le magasin de certificats local sur le nouveau serveur, vous devez transférer le fichier sur un média physique et protéger sa sécurité pendant le transfert sur le nouveau serveur. Il est extrêmement important de maintenir la sécurité de la clé privée. Si cette clé est compromise, la sécurité de votre déploiement ADFS tout entier \ (y compris les ressources au sein de votre organisation et organizations\ de partenaire de ressource) sont compromis.  
  
9. Importer le certificat d’authentification serveur exporté dans le magasin de certificats sur le nouveau serveur avant d’installer le Service de fédération. Pour plus d’informations sur la façon d’importer le certificat, voir Importer un certificat de serveur \ ([http:///\/go.microsoft.com\/fwlink\/? LinkId\ = 108283](https://go.microsoft.com/fwlink/?LinkId=108283)\).  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Liste de vérification: Configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Certificats requis pour les serveurs proxy de fédération](https://technet.microsoft.com/library/dd807054.aspx)  
  

