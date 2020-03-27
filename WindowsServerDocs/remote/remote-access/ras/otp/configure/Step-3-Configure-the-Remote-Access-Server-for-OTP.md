---
title: Étape 3 configurer le serveur d’accès à distance pour le mot de passe à usage unique
description: Cette rubrique fait partie du guide déployer l’accès à distance avec l’authentification par mot de passe à usage unique dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df1e87f2-6a0f-433b-8e42-816ae75395f9
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d588d9b8675dad8bffc9e020032bc66bebf503b0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313678"
---
# <a name="step-3-configure-the-remote-access-server-for-otp"></a>Étape 3 configurer le serveur d’accès à distance pour le mot de passe à usage unique

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Une fois que le serveur RADIUS a été configuré avec des jetons de distribution de logiciels, les ports de communication sont ouverts, un secret partagé a été créé, les comptes d’utilisateurs correspondant à Active Directory ont été créés sur le serveur RADIUS et le serveur d’accès à distance a configuré en tant qu’agent d’authentification RADIUS, le serveur d’accès à distance doit être configuré pour prendre en charge le mot de passe à usage unique.  
  
|Tâche|Description|  
|----|--------|  
|[3,1 exempter les utilisateurs de l’authentification par mot de passe à usage unique (facultatif)](#BKMK_Exempt)|Si des utilisateurs spécifiques sont exemptés de DirectAccess avec l’authentification par mot de passe à usage unique, suivez ces étapes préliminaires.|  
|[3,2 configurer le serveur d’accès à distance pour prendre en charge le mot de passe à usage unique](#BKMK_Config)|Sur le serveur d’accès à distance, mettez à jour la configuration de l’accès à distance pour prendre en charge l’authentification à deux facteurs OTP.|  
|[3,3 cartes à puce pour une autorisation supplémentaire](#BKMK_Smartcard)|Informations supplémentaires concernant l’utilisation des cartes à puce.|  
  
> [!NOTE]  
> Cette rubrique comprend des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="31-exempt-users-from-otp-authentication-optional"></a><a name="BKMK_Exempt"></a>3,1 exempter les utilisateurs de l’authentification par mot de passe à usage unique (facultatif)  
Si des utilisateurs spécifiques doivent être exemptés de l’authentification par mot de passe à usage unique, ces étapes doivent être effectuées avant la configuration de l’accès à distance :  
  
> [!NOTE]  
> Vous devez attendre la fin de la réplication entre les domaines lors de la configuration du groupe d’exemption par mot de passe à usage unique.  
  
#### <a name="create-user-exemption-security-group"></a>Créer un groupe de sécurité d’exemption de l’utilisateur  
  
1.  Créez un groupe de sécurité dans Active Directory pour l’exemption de mot de passe à usage unique.  
  
2.  Ajoutez tous les utilisateurs à exempter de l’authentification par mot de passe à usage unique au groupe de sécurité.  
  
    > [!NOTE]  
    > Veillez à inclure uniquement les comptes d’utilisateur, et non les comptes d’ordinateur, dans le groupe de sécurité exemption par mot de passe à usage unique.  
  
## <a name="32-configure-the-remote-access-server-to-support-otp"></a><a name="BKMK_Config"></a>3,2 configurer le serveur d’accès à distance pour prendre en charge le mot de passe à usage unique  
Pour configurer l’accès à distance pour utiliser l’authentification à deux facteurs et le mot de passe à usage unique avec le serveur RADIUS et le déploiement de certificats des sections précédentes, procédez comme suit :  
  
#### <a name="configure-remote-access-for-otp"></a>Configurer l’accès à distance pour OTP  
  
1.  Ouvrez **gestion de l’accès à distance** , puis cliquez sur **configuration**.  
  
2.  Dans la fenêtre **Installation DirectAccess** , sous **étape 2 : serveur d’accès à distance**, cliquez sur **modifier**.  
  
3.  Cliquez sur **suivant** trois fois. dans la section **authentification** , sélectionnez **authentification à deux facteurs** et **Utilisez le mot de passe à usage unique**et assurez-vous que l’option utiliser des certificats d' **ordinateur** est cochée.  
  
    > [!NOTE]  
    > Une fois que le mot de passe à usage unique a été activé sur le serveur d’accès à distance, si vous désactivez l’utilisation du mot de passe à usage **unique**, les extensions ISAPI et CGI seront désinstallées sur le serveur.  
  
4.  Si la prise en charge de Windows 7 est requise, activez la case à cocher **autoriser les ordinateurs clients Windows 7 à se connecter via DirectAccess** . Remarque : comme indiqué dans la section planification, les clients Windows 7 doivent avoir DCA 2,0 installé pour prendre en charge DirectAccess avec le mot de passe à usage unique.  
  
5.  Cliquez sur **Suivant**.  
  
6.  Dans la section **serveur RADIUS OTP** , double-cliquez sur le champ **nom du serveur** vide.  
  
7.  Dans la boîte de dialogue **Ajouter un serveur RADIUS** , tapez le nom du serveur RADIUS dans le champ **nom du serveur** . Cliquez sur **modifier** en regard du champ **secret partagé** , puis tapez le même mot de passe que celui que vous avez utilisé lors de la configuration du serveur RADIUS dans les champs **nouveau secret** et **confirmer le nouveau secret** . Cliquez deux fois sur **OK** , puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > Si le serveur RADIUS est dans un domaine différent de celui du serveur d’accès à distance, le champ **nom du serveur** doit spécifier le nom de domaine complet du serveur RADIUS.  
  
8.  Dans la section **serveurs d’autorité de certification avec mot de passe à usage unique** , sélectionnez les serveurs d’autorité de certification à utiliser pour l’inscription des certificats d’authentification de client à usage unique, puis cliquez sur **Ajouter**. Cliquez sur **Suivant**.  
  
9. Dans la section **modèles de certificat avec mot de passe à usage unique** , cliquez sur **Parcourir** pour sélectionner le modèle de certificat utilisé pour l’inscription des certificats émis pour l’authentification par mot de passe à usage unique.  
  
    > [!NOTE]  
    > Le modèle de certificat pour les certificats OTP émis par l’autorité de certification d’entreprise doit être configuré sans l’option « ne pas inclure les informations de révocation dans les certificats délivrés ». Si cette option est sélectionnée lors de la création du modèle de certificat, les ordinateurs clients à usage unique ne parviennent pas à se connecter correctement.  
  
    Cliquez sur **Parcourir** pour sélectionner un modèle de certificat utilisé pour inscrire le certificat utilisé par le serveur d’accès à distance afin de signer les demandes d’inscription de certificat avec mot de passe à usage unique. Cliquez sur **OK**. Cliquez sur **Suivant**.  
  
10. S’il est nécessaire d’exempter des utilisateurs spécifiques de DirectAccess avec un mot de passe à usage unique, alors dans la section **exemptions de mot de passe à usage unique** , sélectionnez **ne pas obliger les utilisateurs du groupe de sécurité spécifié à s’authentifier à l’aide**de l’authentification Cliquez sur **groupe de sécurité** et sélectionnez le groupe de sécurité qui a été créé pour les exemptions de mot de passe à usage unique.  
  
11. Sur la page **installation du serveur d’accès à distance** , cliquez sur **Terminer**.  
  
12. Dans la fenêtre **Installation DirectAccess** , sous **étape 3-serveurs d’infrastructure**, cliquez sur **modifier**.  
  
13. Cliquez sur **suivant** à deux moments, puis dans la section **gestion** , double-cliquez sur le champ **serveurs d’administration** .  
  
14. Entrez le **nom** ou l' **adresse** de l’ordinateur du serveur d’autorité de certification configuré pour émettre des certificats à usage unique, puis cliquez sur **OK**.  
  
15. Dans la fenêtre Configuration de l' **accès à distance** , cliquez sur **Terminer**.  
  
16. Cliquez sur **Terminer** dans l' **Assistant expert DirectAccess**.  
  
17. Dans la boîte de dialogue vérification de l' **accès à distance** , cliquez sur **appliquer**, attendez que la Stratégie DirectAccess soit mise à jour, puis cliquez sur **Fermer**.  
  
18. Dans l’écran d' **Accueil** , tapez**PowerShell. exe**, cliquez avec le bouton droit sur **PowerShell**, cliquez sur **avancé**, puis sur **exécuter en tant qu’administrateur**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
19. Dans la fenêtre Windows PowerShell, tapez **gpupdate/force** , puis appuyez sur entrée.  
  
Pour configurer l’accès à distance pour mot de passe à usage unique à l’aide de commandes PowerShell :  
  
![les commandes Windows PowerShell](../../../../media/Step-3-Configure-the-Remote-Access-Server-for-OTP/PowerShellLogoSmall.gif)**équivalentes** Windows PowerShell  
  
La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.  
  
Pour configurer l’accès à distance pour utiliser l’authentification à deux facteurs sur un déploiement qui utilise actuellement l’authentification par certificat d’ordinateur :  
  
```  
Set-DAServer -UserAuthentication TwoFactor  
```  
  
Pour configurer l’accès à distance pour utiliser l’authentification par mot de passe à usage unique à l’aide des paramètres suivants :  
  
-   Un serveur de mot de passe à usage unique nommé OTP.corp.contoso.com.  
  
-   Un serveur d’autorité de certification nommé APP1. Corp. contoso. com\corp-APP1-CA1.  
  
-   Un modèle de certificat nommé DAOTPLogon utilisé pour l’inscription des certificats émis pour l’authentification par mot de passe à usage unique.  
  
-   Un modèle de certificat nommé DAOTPRA utilisé pour inscrire le certificat d’autorité d’inscription utilisé par le serveur d’accès à distance pour signer les demandes d’inscription de certificat avec mot de passe à usage unique.  
  
```  
Enable-DAOtpAuthentication -CertificateTemplateName 'DAOTPLogon' -SigningCertificateTemplateName 'DAOTPRA' -CAServer @('APP1.corp.contoso.com\corp-APP1-CA1') -RadiusServer OTP.corp.contoso.com -SharedSecret Abcd123$  
```  
  
Après l’exécution des commandes PowerShell, effectuez les étapes 12-19 de la procédure précédente pour configurer le serveur d’accès à distance afin qu’il prenne en charge le mot de passe à usage unique.  
  
> [!NOTE]  
> Vérifiez que vous avez appliqué les paramètres de mot de passe à usage unique sur le serveur d’accès à distance avant d’ajouter un point d’entrée.  
  
## <a name="33-smart-cards-for-additional-authorization"></a><a name="BKMK_Smartcard"></a>3,3 cartes à puce pour une autorisation supplémentaire  
Dans la page authentification de l’étape 2 de l’Assistant Configuration de l’accès à distance, vous pouvez exiger l’utilisation de cartes à puce pour accéder au réseau interne. Lorsque cette option est sélectionnée, l’Assistant Configuration de l’accès à distance configure la règle de sécurité de connexion IPsec pour le tunnel intranet sur le serveur DirectAccess afin de requérir l’autorisation en mode tunnel avec les cartes à puce. L’autorisation en mode tunnel vous permet de spécifier que seuls les ordinateurs ou les utilisateurs autorisés peuvent établir un tunnel entrant.  
  
Pour utiliser des cartes à puce avec l'autorisation en mode tunnel IPsec pour le tunnel intranet, vous devez déployer une infrastructure à clé publique (PKI) avec l'infrastructure à cartes à puce.  
  
Étant donné que vos clients DirectAccess utilisent des cartes à puce pour accéder à l’intranet, vous pouvez également utiliser l’assurance du mécanisme d’authentification, une fonctionnalité de Windows Server 2008 R2, pour contrôler l’accès aux ressources, telles que les fichiers, les dossiers et les imprimantes, selon que le l’utilisateur a ouvert une session avec un certificat basé sur une carte à puce. L’assurance du mécanisme d’authentification requiert un niveau fonctionnel de domaine de Windows Server 2008 R2.  
  
### <a name="allowing-access-for-users-with-unusable-smart-cards"></a>Autorisation de l'accès aux utilisateurs munis de cartes à puce inutilisables  
Pour autoriser l'accès temporaire aux utilisateurs munis de cartes à puce qui sont inutilisables, procédez comme suit :  
  
1.  Créez un groupe de sécurité Active Directory pour contenir les comptes des utilisateurs qui ne peuvent pas utiliser temporairement leurs cartes à puce.  
  
2.  Pour l’objet stratégie de groupe serveur DirectAccess, configurez les paramètres IPsec globaux pour l’autorisation de tunnel IPsec et ajoutez le groupe de sécurité Active Directory à la liste des utilisateurs autorisés.  
  
Pour accorder l'accès à un utilisateur qui ne peut pas utiliser sa carte à puce, ajoutez temporairement son compte d'utilisateur au groupe de sécurité Active Directory. Supprimez le compte d'utilisateur du groupe lorsque la carte à puce est utilisable.  
  
### <a name="under-the-covers-smart-card-authorization"></a>En coulisses : autorisation par carte à puce  
L'autorisation par carte à puce fonctionne grâce à l'activation de l'autorisation en mode tunnel sur la règle de sécurité de connexion du tunnel intranet du serveur DirectAccess pour un identificateur de sécurité (SID) Kerberos spécifique. Pour l'autorisation par carte à puce, c'est le SID bien connu (S-1-5-65), qui effectue les mappages aux ouvertures de session à base de cartes à puce. Ce SID est présent dans le jeton Kerberos d’un client DirectAccess et est appelé « ce certificat d’organisation » lorsqu’il est configuré dans les paramètres d’autorisation globaux du mode de tunnel IPsec.  
  
Lorsque vous activez l’autorisation par carte à puce à l’étape 2 de l’Assistant Installation DirectAccess, l’Assistant Installation DirectAccess configure le paramètre d’autorisation du mode de tunnel IPsec global avec ce SID pour l’objet de stratégie de groupe du serveur DirectAccess. Pour afficher cette configuration dans le composant logiciel enfichable pare-feu Windows avec fonctions avancées de sécurité pour l’objet stratégie de groupe du serveur DirectAccess, procédez comme suit :  
  
1.  Cliquez avec le bouton droit sur Pare-feu Windows avec fonctions avancées de sécurité, puis sélectionnez Propriétés.  
  
2.  Sous l’onglet Paramètres IPsec, dans autorisation du tunnel IPsec, cliquez sur Personnaliser.  
  
3.  Cliquez sur l’onglet utilisateurs. Vous devez voir le « certificat d’organisation NT Authority\ce » en tant qu’utilisateur autorisé.  
  


