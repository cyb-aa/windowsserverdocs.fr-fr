---
title: Étape 3 planifier le déploiement de certificats avec mot de passe à usage unique
description: Cette rubrique fait partie du guide déployer l’accès à distance avec l’authentification par mot de passe à usage unique dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d34630b4faa8012eee73967a99bc0541f1305a09
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313521"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>Étape 3 planifier le déploiement de certificats avec mot de passe à usage unique

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Une fois le serveur RADIUS planifié, vous devez planifier les exigences en matière d’autorité de certification, y compris l’autorité de certification qui émet les certificats de mot de passe à usage unique, le modèle de certificat OTP et le certificat d’autorité d’inscription utilisé par l’instance distante. Accédez au serveur pour signer toutes les demandes de certificat à mot de passe à usage unique DirectAccess. Ces certificats sont utilisés comme suit :  
  
1.  Le client DirectAccess demande un certificat à mot de passe à usage unique et le serveur d’accès à distance reçoit la demande.  
  
2.  Le serveur d’accès à distance vérifie les informations d’identification par mot de passe à usage unique et, s’ils sont valides, le serveur joue le rôle d’autorité d’inscription et signe la demande d’inscription de certificat par mot de passe à usage unique à l’aide d’un certificat de signature de courte durée.  
  
3.  Le serveur d’accès à distance renvoie la demande d’inscription de certificat signée au client DirectAccess.  
  
4.  Le client inscrit ensuite le certificat de mot de passe à usage unique auprès de l’autorité de certification à l’aide des demandes d’inscription de certificat signées par le serveur.  
  
5.  L’autorité de certification vérifie les informations d’identification et la demande.  
  
|Tâche|Description|  
|----|--------|  
|[3,1 planifier l’AC avec mot de passe à usage unique](#bkmk_3_1_CA)|Planifiez l’autorité de certification à utiliser pour émettre des certificats aux clients DirectAccess pour l’authentification par mot de passe à usage unique.|  
|[3,2 planifier le modèle de certificat avec mot de passe à usage unique](#bkmk_3_2_OTP_Cert)|Planifier le modèle de certificat avec mot de passe à usage unique.|
|[3,3 planifier le certificat d’autorité d’inscription](#bkmk_33RACert)|Planifier le certificat d’autorité d’inscription pour signer toutes les demandes de certificat d’authentification par mot de passe à usage unique.|

## <a name="31-plan-the-otp-ca"></a><a name="bkmk_3_1_CA"></a>3,1 planifier l’AC avec mot de passe à usage unique  
Pour déployer DirectAccess à l’aide de l’authentification par mot de passe à usage unique, vous avez besoin d’une autorité de certification interne pour émettre les certificats d’authentification par mot de passe à usage unique sur les ordinateurs clients DirectAccess. À cet effet, vous pouvez utiliser la même autorité de certification interne que celle utilisée pour émettre les certificats utilisés pour l’authentification de l’ordinateur IPsec standard.  
  
## <a name="32-plan-the-otp-certificate-template"></a><a name="bkmk_3_2_OTP_Cert"></a>3,2 planifier le modèle de certificat avec mot de passe à usage unique  
Chaque client DirectAccess requiert un certificat d’authentification par mot de passe à usage unique afin d’accéder au réseau interne. Vous devez configurer un modèle sur votre autorité de certification interne pour le certificat OTP. Notez les points suivants lors de la configuration du modèle de certificat à mot de passe à usage unique :  
  
-   Tous les utilisateurs qui doivent effectuer une authentification par mot de passe à usage unique doivent disposer d’autorisations de lecture et d’inscription pour ce modèle.  
  
-   Le nom du sujet doit être créé à partir d’informations de Active Directory, pour s’assurer que le nom de l’objet correspond au nom d’utilisateur du mot de passe à usage unique, et non au nom du serveur d’accès à distance qui effectue la demande de certificat. Le nom du sujet doit être au format de nom unique, et l’autre nom de l’objet doit être au format UPN. Cela garantit que le certificat de mot de passe à usage unique inscrit est valide pour l’authentification Kerberos par carte à puce.  
  
-   Le rôle prévu du certificat doit être ouverture de session par carte à puce  
  
-   L’émission doit nécessiter une signature autorisée. La signature doit être configurée avec la stratégie d’application de mot de passe à usage unique DirectAccess prédéfinie définie dans le modèle de certificat de signature de l’autorité d’inscription.  
  
-   La période de validité doit être définie sur une heure.  
  
    > [!NOTE]  
    > Dans les cas où le serveur de l’autorité de certification est un ordinateur Windows Server 2003, le modèle doit être configuré sur un autre ordinateur. Cela est dû au fait que la définition de la **période de validité** en heures n’est pas possible lors de l’exécution de versions de Windows antérieures à 2008/Vista. Si le rôle de service de certification de l’ordinateur que vous utilisez pour configurer le modèle n’est pas installé ou s’il s’agit d’un ordinateur client, vous devrez peut-être installer le composant logiciel enfichable modèles de certificats. Pour plus d’informations sur ce sujet, cliquez [ici](https://technet.microsoft.com/library/cc732445.aspx).  
  
-   La période de renouvellement doit être définie sur 0.  
  
-   Facultatif Les certificats et les demandes ne doivent pas être stockés dans la base de données de l’autorité de certification.  
  
-   Le paramètre utilisation améliorée de la clé du certificat doit être défini correctement, comme suit :  
  
    -   Pour le modèle de certificat de signature d’inscription DirectAccess, utilisez la clé 1.3.6.1.4.1.311.81.1.1.  
  
    -   Pour le modèle de certificat d’authentification par mot de passe à usage unique, utilisez la clé key 1.3.6.1.4.1.311.20.2.2.  
  
## <a name="33-plan-the-registration-authority-certificate"></a><a name="bkmk_33RACert"></a>3,3 planifier le certificat d’autorité d’inscription  
Lorsque les clients DirectAccess demandent un certificat à mot de passe à usage unique, le serveur d’accès à distance reçoit la requête du client. Le serveur d’accès à distance signe toutes les demandes de certificat de mot de passe à usage unique des clients utilisant le certificat d’autorité d’inscription. L’autorité de certification émet des certificats uniquement si la demande est signée par le certificat de l’autorité d’inscription sur le serveur d’accès à distance. Le certificat doit être émis par une autorité de certification interne, le certificat ne peut pas être auto-signé. Il ne doit pas être émis par l’autorité de certification qui a émis les certificats à usage unique, mais l’autorité de certification qui émet les certificats à usage unique doit approuver l’autorité de certification qui émet le certificat de signature de l’autorité d’inscription.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Voir aussi  
  
-   [Étape 4 : planifier le mot de passe à usage unique pour le serveur d’accès à distance](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  


