---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: Définir un certificat de communication du service
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 140e8e4204148dd8862385054554d7b8336856ec
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192010"
---
# <a name="set-a-service-communications-certificate"></a>Définir un certificat de communication du service


Serveurs de fédération dans Active Directory Federation Services \(AD FS\) utiliser le certificat de communications de service pour sécuriser le trafic des services Web pour Secure Sockets Layer \(SSL\) communication avec le Web les clients ou avec des serveurs proxy de fédération.

> [!NOTE]  
> Le certificat de Communications de Service n’est pas un certificat SSL. Pour modifier le certificat SSL AD FS, vous devrez utiliser Powershell. Suivez les instructions dans cette [article](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap).


Vous pouvez utiliser la procédure suivante pour modifier le certificat de communications de service avec le composant logiciel enfichable Gestion AD FS\-dans.  

> [!NOTE]  
> Le composant logiciel enfichable Gestion AD FS\-dans fait référence aux certificats d’authentification serveur pour les serveurs de fédération en tant que certificats de communication de service.  

Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [locaux et les groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   

### <a name="to-set-a-service-communications-certificate"></a>Pour définir un certificat de communications de service  

1.  Sur le **Démarrer** , tapez**gestion AD FS**, puis appuyez sur ENTRÉE.  

2.  Dans l’arborescence de la console, double-cliquez\-cliquez sur **Service**, puis cliquez sur **certificats**.  

3.  Dans le **Actions** volet, cliquez sur le **définir le certificat de communication du Service** lien.  

4.  Dans le **sélectionner un certificat de communications de service** boîte de dialogue, accédez au fichier de certificat que vous souhaitez définir en tant que le certificat de communications de service, sélectionnez le fichier de certificat, puis cliquez sur **ouvrir**.  

## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  

[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
