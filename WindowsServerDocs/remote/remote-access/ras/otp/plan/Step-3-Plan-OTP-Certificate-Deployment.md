---
title: Déploiement de certificats de l’étape 3 Plan OTP
description: Cette rubrique fait partie du guide de déploiement des accès à distance avec authentification OTP dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4102fc4f7eacf0b407446717caa83e4e5f70ae57
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282368"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>Déploiement de certificats de l’étape 3 Plan OTP

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après la planification du serveur RADIUS, vous devez planifier pour les conditions d’autorité de certification, y compris l’autorité de certification qui émettre des certificats de mot de passe à usage unique (OTP), le modèle de certificat OTP et le certificat d’autorité d’inscription utilisé par l’instance distante Serveur d’accès pour vous connecter à toutes les demandes de certificat OTP client DirectAccess. Ces certificats sont utilisés comme suit :  
  
1.  Le client DirectAccess demande un certificat à usage, et le serveur d’accès à distance reçoit la demande.  
  
2.  Le serveur d’accès à distance vérifie les informations d’identification de secret à usage unique et si elles sont valides, le serveur agit comme une autorité d’inscription et signe la demande d’inscription de certificat secret à usage unique à l’aide d’un certificat de signature de courte durée de vie.  
  
3.  Le serveur d’accès à distance envoie la demande d’inscription de certificat signé au client DirectAccess  
  
4.  Le client inscrit alors le certificat de secret à usage unique à partir de l’autorité de certification à l’aide des demandes d’inscription de certificats signés par le serveur.  
  
5.  L’autorité de certification vérifie les informations d’identification et de la demande.  
  
|Tâche|Description|  
|----|--------|  
|[3.1 planifier l’autorité de certification OTP](#bkmk_3_1_CA)|Planifier l’autorité de certification (CA) à utiliser pour émettre des certificats pour les clients DirectAccess pour l’authentification unique.|  
|[3.2 planifier le modèle de certificat OTP](#bkmk_3_2_OTP_Cert)|Planifier le modèle de certificat OTP.|
|[3.3 planifier le certificat d’autorité d’inscription](#bkmk_33RACert)|Planifier le certificat d’autorité d’inscription pour vous connecter à toutes les demandes de certificat d’authentification OTP.|

## <a name="bkmk_3_1_CA"></a>3.1 planifier l’autorité de certification OTP  
Pour déployer DirectAccess à l’aide de l’authentification de mot de passe à usage unique (OTP), vous avez besoin d’une autorité de certification interne émettre les certificats d’authentification OTP aux ordinateurs clients DirectAccess. Pour cela, vous pouvez utiliser l’autorité de certification interne même qui vous permet d’émettre les certificats qui sont utilisés pour l’authentification d’ordinateur IPsec standard.  
  
## <a name="bkmk_3_2_OTP_Cert"></a>3.2 planifier le modèle de certificat OTP  
Chaque client DirectAccess nécessite un certificat d’authentification OTP pour obtenir l’accès au réseau interne. Vous devez configurer un modèle sur votre autorité de certification à usage interne. Notez les points suivants lorsque vous configurez le modèle de certificat OTP :  
  
-   Tous les utilisateurs qui ont besoin pour effectuer l’authentification OTP doivent avoir les autorisations pour ce modèle de lecture et inscription.  
  
-   Le nom du sujet doit être généré à partir des informations Active Directory, pour vous assurer que le nom de sujet correspond le nom d’utilisateur et pas le nom du serveur d’accès à distance qui effectue la demande de certificat. Le nom du sujet doit être dans le format de nom unique, et l’autre nom de sujet doit être au format UPN. Cela garantit que le certificat inscrit OTP est valide pour l’authentification Kerberos de Smartcard.  
  
-   Le rôle prévu du certificat doit être l’ouverture de session de carte à puce  
  
-   Émission doit exiger une signature autorisée. La signature doit être configurée avec la stratégie de Application de secret à usage unique DirectAccess prédéfinis définie dans le modèle de certificat d’inscription autorité signature.  
  
-   La période de validité doit être définie sur une heure.  
  
    > [!NOTE]  
    > Dans les situations où le serveur d’autorité de certification est un ordinateur Windows Server 2003, puis le modèle doit être configuré sur un autre ordinateur. Cela est dû au fait que définissant le **période de validité** heures n’est pas possible lors de l’exécution des versions de Windows antérieures à 2008/Vista. Si l’ordinateur que vous utilisez pour configurer le modèle n’a pas le rôle de Service de Certification installé, ou il est un ordinateur client, vous devez installer le composant logiciel enfichable modèles de certificats. Pour plus d’informations sur ce sujet, cliquez sur [ici](https://technet.microsoft.com/library/cc732445.aspx).  
  
-   La période de renouvellement doit être définie sur 0.  
  
-   (Facultatif) Certificats et les demandes ne doivent pas être stockées dans la base de données de l’autorité de certification.  
  
-   Le paramètre d’utilisation améliorée de la clé de certificat doit être défini correctement, comme suit :  
  
    -   Pour le modèle de certificat signature DirectAccess d’inscription, utilisez la clé 1.3.6.1.4.1.311.81.1.1.  
  
    -   Pour le modèle de certificat d’authentification OTP, utilisez la clé 1.3.6.1.4.1.311.20.2.2.  
  
## <a name="bkmk_33RACert"></a>3.3 planifier le certificat d’autorité d’inscription  
Lorsque les clients DirectAccess demandent un certificat de secret à usage unique, le serveur d’accès à distance reçoit la demande du client. Le serveur d’accès à distance connecte toutes les demandes de certificat à usage des clients qui utilisent le certificat d’autorité d’inscription. L’autorité de certification émet des certificats uniquement si la demande est signée par le certificat d’autorité d’inscription sur le serveur d’accès à distance. Le certificat doit être émis par une autorité de certification interne, le certificat ne peut pas être auto-signé. Il ne devra pas être émis par l’autorité de certification qui a émis les certificats OTP, mais l’autorité de certification qui émet les certificats OTP doit approuver l’autorité de certification qui émet le certificat de signature d’autorité d’inscription.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Étape 4 : Planifier le secret à usage unique pour le serveur d’accès à distance](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  


