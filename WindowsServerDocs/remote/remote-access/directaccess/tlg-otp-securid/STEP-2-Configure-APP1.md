---
title: ÉTAPE 2 configurer APP1
description: Cette rubrique fait partie du Guide de laboratoire de Test - démontrer DirectAccess avec l’authentification OTP et RSA SecurID pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19a7a4a6-9a04-42ea-a5d0-ecb28a34dbaa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 612d3f4e9be71f60811b4499d9bdb8ae3bd217e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847490"
---
# <a name="step-2-configure-app1"></a>ÉTAPE 2 configurer APP1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Utilisez les étapes suivantes pour préparer la prise en charge de l’OTP APP1 :  
  
1. Pour créer et déployer un modèle de certificat utilisé pour signer des demandes de certificat OTP. Configurer un modèle de certificat utilisé pour signer des demandes de certificat OTP.  
  
2. Pour créer et déployer un modèle de certificat pour les certificats OTP émis par l’autorité de certification d’entreprise. Configurer un modèle de certificat pour les certificats OTP émis par l’autorité de certification d’entreprise.  
  
> [!WARNING]  
> La conception de ce guide de laboratoire de test inclut des serveurs d’infrastructure, comme un contrôleur de domaine et une autorité de certification (CA) qui exécutent Windows Server 2012 R2 ou Windows Server 2012. À l’aide de ce guide de laboratoire de test pour configurer les serveurs d’infrastructure qui exécutent d’autres systèmes d’exploitation n’a pas été testée et des instructions pour configurer d’autres systèmes d’exploitation ne sont pas incluses dans ce guide.  
  
## <a name="DAOTPRA"></a>Pour créer et déployer un modèle de certificat utilisé pour signer des demandes de certificat OTP  
  
1.  Exécutez **certtmpl.msc**, puis appuyez sur ENTRÉE.  
  
2.  Dans la Console Modèles de certificats, dans le volet de détails, cliquez sur le **ordinateur** modèle, puis cliquez sur **dupliquer le modèle**.  
  
3.  Sur le **propriétés du nouveau modèle** boîte de dialogue le **compatibilité** sous l’onglet le **autorité de Certification** , sélectionnez le système d’exploitation que vous le souhaitez et dans le  **Les modifications résultant** boîte dialogue, cliquez sur **OK**. Dans le **destinataire du certificat** liste, sélectionnez le système d’exploitation souhaité, puis, dans le **modifications résultantes** boîte dialogue, cliquez sur **OK**.  
  
4.  Sur le **propriétés du nouveau modèle** boîte de dialogue, cliquez sur le **général** onglet.  
  
5.  Sur le **général** sous l’onglet **nom complet du modèle**, type **DAOTPRA**. Définir le **période de validité** avec 2 jours et définissez le **période de renouvellement** à 1 jour. Si le **modèles de certificats** avertissement s’affiche, cliquez sur **OK**.  
  
6.  Cliquez sur l’onglet **Sécurité** , puis sur **Ajouter**.  
  
7.  Sur le **sélectionner des utilisateurs, les ordinateurs, les comptes de Service ou les groupes** boîte de dialogue, cliquez sur **Types d’objets**. Sur le **Types d’objets** boîte de dialogue, sélectionnez **ordinateurs**, puis cliquez sur **OK**. Dans le **Entrez les noms des objets à sélectionner** , tapez **EDGE1**, cliquez sur **OK**, puis, dans le **autoriser** colonne, sélectionnez le **en lecture** , **Inscription**, et **inscription automatique** cases à cocher. Cliquez sur **utilisateurs authentifiés**, sélectionnez le **en lecture** case à cocher sous la **autoriser** colonne et désactivez toutes les autres cases à cocher. Cliquez sur **ordinateurs du domaine**, puis décochez **inscription** sous le **autoriser** colonne. Cliquez sur **Admins du domaine** et **administrateurs de l’entreprise** et cliquez sur **contrôle total** sous le **autoriser** colonne pour les deux. Cliquez sur **Appliquer**.  
  
8.  Cliquez sur le **nom de l’objet** onglet, puis cliquez sur **construire à partir de ces informations Active Directory**. Dans le **format du nom du sujet :** liste select **nom DNS**, assurez-vous que le **nom DNS** case est cochée, puis cliquez sur **appliquer**.  
  
9. Cliquez sur le **Extensions** onglet, sélectionnez **stratégies d’Application** puis cliquez sur **modifier**. Supprimer toutes les stratégies d’application existant. Cliquez sur **ajouter**, puis, dans le **ajouter une stratégie Application** boîte de dialogue, cliquez sur **New**, entrez **DA OTP RA** dans le **nom :** champ et **1.3.6.1.4.1.311.81.1.1** dans le **identificateur d’objet :** champ, puis cliquez sur **OK**. Sur le **ajouter une stratégie Application** boîte de dialogue, cliquez sur **OK**. Sur le **modifier l’Extension des stratégies d’Application**, cliquez sur **OK**. Sur le **propriétés du nouveau modèle** boîte de dialogue, cliquez sur **OK**.  
  
## <a name="DAOTPLogon"></a>Pour créer et déployer un modèle de certificat pour les certificats OTP émis par l’autorité de certification d’entreprise  
  
1.  Dans la Console Modèles de certificats, dans le volet de détails, cliquez sur le **ouverture de session par carte à puce** modèle, puis cliquez sur **dupliquer le modèle**.  
  
2.  Sur le **propriétés du nouveau modèle** boîte de dialogue le **compatibilité** onglet dans le **autorité de Certification** , cliquez sur le système d’exploitation que vous souhaitez utiliser, puis, dans le **Modifications résultantes** boîte de dialogue, cliquez sur **OK**. Dans le **destinataire du certificat** liste, sélectionnez le système d’exploitation que vous souhaitez utiliser, puis, dans le **modifications résultantes** boîte dialogue, cliquez sur **OK**.  
  
3.  Sur le **propriétés du nouveau modèle** boîte de dialogue, cliquez sur le **général** onglet.  
  
4.  Sur le **général** sous l’onglet **nom complet du modèle**, type **DAOTPLogon**. Dans **période de validité**, dans la liste déroulante, cliquez sur **heures**, dans le **modèles de certificats** boîte de dialogue, cliquez sur **OK**et assurez-vous que que le nombre d’heures est défini sur 1. Dans **période de renouvellement**, type **0**.  
  
    > [!IMPORTANT]  
    > **Autorité de certification Windows Server 2003**. Dans les situations où l’autorité de certification (CA) est sur un ordinateur qui exécute Windows Server 2003, puis le modèle de certificat doit être configuré sur un autre ordinateur. Cela est nécessaire car la définition du **période de validité** heures n’est pas possible lors de l’exécution des versions de Windows antérieures à Windows Server 2008 et Windows Vista. Si l’ordinateur que vous utilisez pour configurer le modèle n’a pas le rôle de serveur Services de certificats Active Directory installé, ou s’il s’agit d’un ordinateur client, vous devez installer le composant logiciel enfichable modèles de certificats. Pour plus d’informations, consultez [installer le composant logiciel enfichable modèles de certificat](https://technet.microsoft.com/library/cc732445.aspx).  
    >   
    > **Autorité de certification Windows Server 2008 R2**. Si vous avez déjà déployé une autorité de certification (CA) qui exécute Windows Server 2008 R2, vous devez configurer le modèle de certificat **période de renouvellement** à 1 ou 2 heures et le **période de validité** à dépasser les **période de renouvellement**, mais pas plus de 4 heures. Si vous configurez un modèle de certificat **période de validité** de plus de 4 heures avec une autorité de certification Windows Server 2008 R2 est en cours d’exécution, l’Assistant installation DirectAccess ne peut pas détecter le modèle de certificat et DirectAccess Échec de l’installation.  
  
5.  Cliquez sur le **sécurité** onglet, sélectionnez **utilisateurs authentifiés**, dans le **autoriser** colonne, puis sélectionnez le **en lecture** et **inscription**  cases à cocher. Cliquez sur **OK**. Cliquez sur **Admins du domaine** et **administrateurs de l’entreprise**, puis cliquez sur **contrôle total** sous le **autoriser** colonne pour les deux. Cliquez sur **Appliquer**.  
  
6.  Cliquez sur le **nom de l’objet** onglet, puis cliquez sur **construire à partir de ces informations Active Directory**. Dans le **format du nom du sujet :** liste select **nom distinctif complet**, assurez-vous que le **nom d’utilisateur principal (UPN)** case est cochée, puis cliquez sur **appliquer** .  
  
7.  Cliquez sur le **Server** onglet, sélectionnez le **ne stockez pas de certificats et les demandes dans la base de données de l’autorité de certification** case à cocher, le **n’incluent pas les informations de révocation de certificats émis** case à cocher, puis, dans le **propriétés du nouveau modèle** boîte de dialogue, cliquez sur **appliquer**.  
  
8.  Cliquez sur le **conditions d’émission** onglet, sélectionnez le **ce nombre de signatures autorisées :** case à cocher, définissez la valeur sur 1. Dans le **nécessaire dans la signature de type de stratégie :** liste select **stratégie d’Application**, puis, dans le **stratégie d’Application** liste select **DA OTP RA**. Sur le **propriétés du nouveau modèle** boîte de dialogue, cliquez sur **OK**.  
  
9. Cliquez sur le **Extensions** onglet, puis, dans **stratégies d’Application** cliquez sur **modifier**. Supprimer **l’authentification du Client**, conserver **SmartCardLogon**, puis cliquez sur **OK** à deux reprises.  
  
10. Fermez la Console Modèles de certificat.  
  
11. Sur le **Démarrer** , tapez**certsrv.msc**, puis appuyez sur ENTRÉE.  
  
12. Dans l’arborescence de la console Autorité de Certification, développez **corp-APP1-CA-1**, cliquez sur **modèles de certificats**, avec le bouton droit **modèles de certificats**, pointez sur **New**, puis cliquez sur **modèle de certificat à délivrer**.  
  
13. Dans la liste des modèles de certificats, cliquez sur **DAOTPRA** et **DAOTPLogon**, puis cliquez sur **OK**.  
  
14. Dans le volet de détails de la console, vous devez voir le **DAOTPRA** modèle de certificat avec un **rôle prévu** de **DA OTP RA** et **DAOTPLogon** modèle de certificat avec un **rôle prévu** de **ouverture de session de carte à puce**.  
  
15. Redémarrez les services.  
  
16. Fermez la console Autorité de certification.  
  
17. Ouvrez une invite de commandes avec élévation de privilèges. Type **CertUtil.exe - SetReg DBFlags + DBFLAGS_ENABLEVOLATILEREQUESTS**, puis appuyez sur ENTRÉE.  
  
18. Laissez la fenêtre d’invite de commandes ouverte pour l’étape suivante.  
  


