---
title: ÉTAPE 2 configurer APP1
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer DirectAccess avec l’authentification par mot de passe à usage unique et RSA SecurID pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19a7a4a6-9a04-42ea-a5d0-ecb28a34dbaa
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dbeba9f1646cfb13d709cb4f7987802f69708adb
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314422"
---
# <a name="step-2-configure-app1"></a>ÉTAPE 2 configurer APP1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Utilisez les étapes suivantes pour préparer APP1 à la prise en charge du mot de passe à usage unique :  
  
1. Pour créer et déployer un modèle de certificat utilisé pour signer les demandes de certificat avec mot de passe à usage unique. Configurez un modèle de certificat utilisé pour signer les demandes de certificat avec mot de passe à usage unique.  
  
2. Pour créer et déployer un modèle de certificat pour les certificats à mot de passe à usage unique émis par l’autorité de certification d’entreprise. Configurez un modèle de certificat pour les certificats OTP émis par l’autorité de certification d’entreprise.  
  
> [!WARNING]  
> La conception de ce guide de laboratoire de test comprend des serveurs d’infrastructure, tels qu’un contrôleur de domaine et une autorité de certification (CA) qui exécutent Windows Server 2012 R2 ou Windows Server 2012. L’utilisation de ce guide de laboratoire de test pour configurer des serveurs d’infrastructure qui exécutent d’autres systèmes d’exploitation n’a pas été testée et les instructions de configuration d’autres systèmes d’exploitation ne sont pas incluses dans ce guide.  
  
## <a name="to-create-and-deploy-a-certificate-template-used-to-sign-otp-certificate-requests"></a><a name="DAOTPRA"></a>Pour créer et déployer un modèle de certificat utilisé pour signer les demandes de certificat avec mot de passe à usage unique  
  
1.  Exécutez **certtmpl. msc**, puis appuyez sur entrée.  
  
2.  Dans la console modèles de certificats, dans le volet d’informations, cliquez avec le bouton droit sur le modèle **ordinateur** , puis cliquez sur **dupliquer le modèle**.  
  
3.  Dans la **boîte de dialogue Propriétés du nouveau modèle** , sous l’onglet **compatibilité** , dans la liste **autorité de certification** , sélectionnez le système d’exploitation souhaité, puis dans la boîte de dialogue **modifications résultantes** , cliquez sur **OK**. Dans la liste **destinataire du certificat** , sélectionnez le système d’exploitation que vous souhaitez, puis dans la boîte de dialogue **modifications résultantes** , cliquez sur **OK**.  
  
4.  Dans la boîte **de dialogue Propriétés du nouveau modèle** , cliquez sur l’onglet **général** .  
  
5.  Sous l’onglet **général** , dans **nom complet du modèle**, tapez **DAOTPRA**. Définissez la **période de validité** sur 2 jours et définissez la **période de renouvellement** sur 1 jour. Si l’avertissement **modèles de certificats** s’affiche, cliquez sur **OK**.  
  
6.  Cliquez sur l’onglet **Sécurité** , puis sur **Ajouter**.  
  
7.  Dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs, les comptes de service ou les groupes** , cliquez sur **types d’objets**. Dans la boîte de dialogue **types d’objets** , sélectionnez **ordinateurs**, puis cliquez sur **OK**. Dans la zone **Entrez les noms des objets à sélectionner** , tapez **Edge1**, cliquez sur **OK**, puis dans la colonne **autoriser** , activez les cases à cocher **lecture**, **inscription**et **inscription automatique** . Cliquez sur **utilisateurs authentifiés**, activez la case à cocher **lire** sous la colonne **autoriser** et désactivez toutes les autres cases à cocher. Cliquez sur **ordinateurs du domaine**, puis décochez **inscrire** sous la colonne **autoriser** . Cliquez sur **Admins du domaine** et **administrateurs** de l’entreprise, puis cliquez sur **contrôle total** sous la colonne **autoriser** pour les deux. Cliquez sur **Appliquer**.  
  
8.  Cliquez sur l’onglet nom de l' **objet** , puis sur **construire à partir de ces informations de Active Directory**. Dans la liste format du nom de l' **objet :** sélectionnez le **nom DNS**, assurez-vous que la zone **nom DNS** est cochée, puis cliquez sur **appliquer**.  
  
9. Cliquez sur l’onglet **Extensions** , sélectionnez **stratégies d’application** , puis cliquez sur **modifier**. Supprimer toutes les stratégies d’application existantes. Cliquez **sur Ajouter**, puis dans la boîte de dialogue **Ajouter une stratégie d’application** , cliquez sur **nouveau**, entrez **da OTP RA** dans le champ **nom :** et **1.3.6.1.4.1.311.81.1.1** dans le champ **identificateur d’objet :** , puis cliquez sur **OK**. Dans la boîte de dialogue **Ajouter une stratégie d’application** , cliquez sur **OK**. Dans l' **extension modifier les stratégies d’application**, cliquez sur **OK**. Dans la boîte **de dialogue Propriétés du nouveau modèle** , cliquez sur **OK**.  
  
## <a name="to-create-and-deploy-a-certificate-template-for-otp-certificates-issued-by-the-corporate-ca"></a><a name="DAOTPLogon"></a>Pour créer et déployer un modèle de certificat pour les certificats à mot de passe à usage unique émis par l’autorité de certification d’entreprise  
  
1.  Dans la console modèles de certificats, dans le volet d’informations, cliquez avec le bouton droit sur le modèle **connexion par carte à puce** , puis cliquez sur **dupliquer le modèle**.  
  
2.  Dans la **boîte de dialogue Propriétés du nouveau modèle** , sous l’onglet **compatibilité** de la liste **autorité de certification** , cliquez sur le système d’exploitation que vous souhaitez utiliser, puis dans la boîte de dialogue **modifications résultantes** , cliquez sur **OK**. Dans la liste **destinataire du certificat** , sélectionnez le système d’exploitation que vous souhaitez utiliser, puis dans la boîte de dialogue **modifications résultantes** , cliquez sur **OK**.  
  
3.  Dans la boîte **de dialogue Propriétés du nouveau modèle** , cliquez sur l’onglet **général** .  
  
4.  Sous l’onglet **général** , dans **nom complet du modèle**, tapez **DAOTPLogon**. Dans **période de validité**, dans la liste déroulante, cliquez sur **heures**, dans la boîte de dialogue **modèles de certificats** , cliquez sur **OK**, et assurez-vous que le nombre d’heures est défini sur 1. Dans **période de renouvellement**, tapez **0**.  
  
    > [!IMPORTANT]  
    > **Autorité de certification Windows Server 2003**. Dans les situations où l’autorité de certification se trouve sur un ordinateur qui exécute Windows Server 2003, le modèle de certificat doit être configuré sur un autre ordinateur. Ceci est nécessaire car la définition de la **période de validité** en heures n’est pas possible lors de l’exécution de versions de Windows antérieures à windows Server 2008 et Windows Vista. Si le rôle de serveur services de certificats Active Directory n’est pas installé sur l’ordinateur que vous utilisez pour configurer le modèle, ou s’il s’agit d’un ordinateur client, vous devrez peut-être installer le composant logiciel enfichable modèles de certificats. Pour plus d’informations, consultez [installer le composant logiciel enfichable modèles de certificats](https://technet.microsoft.com/library/cc732445.aspx).  
    >   
    > **Autorité de certification Windows Server 2008 R2**. Si vous avez déjà déployé une autorité de certification (CA) qui exécute Windows Server 2008 R2, vous devez configurer la période de **renouvellement** du modèle de certificat sur 1 ou 2 heures, et la **période de validité** sur une période plus longue que la **période de renouvellement**, mais pas plus de 4 heures. Si vous configurez une **période de validité** de modèle de certificat supérieure à 4 heures avec une autorité de certification qui exécute Windows Server 2008 R2, l’Assistant Installation DirectAccess ne peut pas détecter le modèle de certificat et l’installation de DirectAccess échoue.  
  
5.  Cliquez sur l’onglet **sécurité** , sélectionnez **utilisateurs authentifiés**, dans la colonne **autoriser** , puis activez les cases à cocher **lecture** et **inscription** . Cliquez sur **OK**. Cliquez sur **Admins du domaine** et **administrateurs**de l’entreprise, puis cliquez sur **contrôle total** sous la colonne **autoriser** pour les deux. Cliquez sur **Appliquer**.  
  
6.  Cliquez sur l’onglet nom de l' **objet** , puis sur **construire à partir de ces informations de Active Directory**. Dans le champ format du nom de l' **objet :** liste, sélectionnez **nom distinctif complet**, assurez-vous que la case **nom d’utilisateur principal (UPN)** est cochée, puis cliquez sur **appliquer**.  
  
7.  Cliquez sur l’onglet **serveur** , activez la case à cocher **ne pas stocker de certificats et de demandes dans la base de données de l’autorité de certification** , désactivez la case à cocher **ne pas inclure les informations de révocation dans les certificats délivrés** , puis dans la boîte **de dialogue Propriétés du nouveau modèle** , cliquez sur **appliquer**.  
  
8.  Cliquez sur l’onglet **conditions d’émission** , activez la case à cocher **ce nombre de signatures autorisées :** , puis définissez la valeur sur 1. Dans le **type de stratégie requis dans signature :** liste, sélectionnez **stratégie d’application**, puis dans la liste **stratégie d’application** , sélectionnez **da OTP RA**. Dans la boîte **de dialogue Propriétés du nouveau modèle** , cliquez sur **OK**.  
  
9. Cliquez sur l’onglet **Extensions** , puis sur **stratégies d’application** , cliquez sur **modifier**. Supprimez **l’authentification du client**, conservez **SmartCardLogon**, puis cliquez deux fois sur **OK** .  
  
10. Fermez la Console Modèles de certificat.  
  
11. Dans l’écran d' **Accueil** , tapez**certsrv. msc**, puis appuyez sur entrée.  
  
12. Dans l’arborescence de la console autorité de certification, développez **Corp-App1-ca-1**, cliquez sur **modèles de certificats**, cliquez avec le bouton droit sur **modèles de certificats**, pointez sur **nouveau**, puis cliquez sur **modèle de certificat à délivrer**.  
  
13. Dans la liste des modèles de certificats, cliquez sur **DAOTPRA** et **DAOTPLogon**, puis cliquez sur **OK**.  
  
14. Dans le volet d’informations de la console, vous devez voir le modèle de certificat **DAOTPRA** avec un **rôle prévu** de l’autorité de certification de **mot de passe à usage unique da** et le modèle de certificat **DAOTPLogon** avec un **rôle prévu** de l' **ouverture de session par carte à puce**.  
  
15. Redémarrez les services.  
  
16. Fermez la console Autorité de certification.  
  
17. Ouvrez une invite de commandes avec élévation de privilèges. Tapez **certutil. exe-SetReg DBFlags + DBFLAGS_ENABLEVOLATILEREQUESTS**, puis appuyez sur entrée.  
  
18. Laissez la fenêtre d’invite de commandes ouverte pour l’étape suivante.  
  


