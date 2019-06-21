---
title: Étape 3 configurer le serveur d’accès à distance pour OTP
description: Cette rubrique fait partie du guide de déploiement des accès à distance avec authentification OTP dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df1e87f2-6a0f-433b-8e42-816ae75395f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 093877657f19006bba2b80c10b92db1fb3b40fde
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280863"
---
# <a name="step-3-configure-the-remote-access-server-for-otp"></a>Étape 3 configurer le serveur d’accès à distance pour OTP

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Une fois que le serveur RADIUS a été configuré avec des jetons de distribution de logiciels, les ports de communication sont ouverts, un secret partagé a été créé, les comptes d’utilisateur correspondant à Active Directory ont été créées sur le serveur RADIUS et le serveur d’accès à distance a été configurée comme un agent d’authentification RADIUS, le serveur d’accès à distance doit être configuré pour prendre en charge le secret à usage unique.  
  
|Tâche|Description|  
|----|--------|  
|[3.1 exempter des utilisateurs à partir de l’authentification OTP (facultative)](#BKMK_Exempt)|Si des utilisateurs spécifiques seront exemptés de DirectAccess avec l’authentification OTP, puis suivez ces étapes préliminaires.|  
|[3.2 configurer le serveur d’accès à distance pour prendre en charge le secret à usage unique](#BKMK_Config)|Sur le serveur d’accès à distance mettre à jour la configuration de l’accès à distance pour prendre en charge l’authentification à deux facteurs OTP.|  
|[3.3 les cartes à puce pour une autorisation supplémentaire](#BKMK_Smartcard)|Obtenir des informations supplémentaires concernant l’utilisation de cartes à puce.|  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Exempt"></a>3.1 exempter des utilisateurs à partir de l’authentification OTP (facultative)  
Si des utilisateurs spécifiques doivent être exemptés de l’authentification OTP, ces étapes doivent être prises avant la configuration de l’accès à distance :  
  
> [!NOTE]  
> Vous devez attendre la réplication entre les domaines à effectuer lors de la configuration du groupe d’exemption OTP.  
  
#### <a name="create-user-exemption-security-group"></a>Créer le groupe de sécurité d’exemption utilisateur  
  
1.  Créer un groupe de sécurité dans Active Directory dans le but exemption du secret à usage unique.  
  
2.  Ajouter tous les utilisateurs à exempter de l’authentification OTP au groupe de sécurité.  
  
    > [!NOTE]  
    > Veillez à inclure uniquement les comptes d’utilisateur et pas les comptes d’ordinateur, dans le groupe de sécurité d’exemption OTP.  
  
## <a name="BKMK_Config"></a>3.2 configurer le serveur d’accès à distance pour prendre en charge le secret à usage unique  
Pour configurer l’accès à distance à utiliser l’authentification à deux facteurs et le secret à usage unique avec le serveur RADIUS et le déploiement des certificats dans les sections précédentes, procédez comme suit :  
  
#### <a name="configure-remote-access-for-otp"></a>Configurer l’accès à distance pour OTP  
  
1.  Ouvrez **gestion de l’accès à distance** et cliquez sur **Configuration**.  
  
2.  Dans le **le programme d’installation de DirectAccess** fenêtre, sous **étape 2 : serveur d’accès à distance**, cliquez sur **modifier**.  
  
3.  Cliquez sur **suivant** trois fois, puis, dans le **authentification** section Sélectionnez à la fois **authentification à deux facteurs** et **utilisez OTP**et vous assurer qui **utilisent des certificats d’ordinateur** est activée.  
  
    > [!NOTE]  
    > Une fois que le secret à usage unique a été activée sur le serveur d’accès à distance, si vous désactivez OTP en désélectionnant **utilisez OTP**, les extensions ISAPI et CGI seront désinstallées sur le serveur.  
  
4.  Si la prise en charge de Windows 7 est nécessaire, sélectionnez le **autoriser Windows client les ordinateurs 7 à se connecter via DirectAccess** case à cocher. Remarque: Comme indiqué dans la section de planification, les clients Windows 7 doivent avoir DCA 2.0 est installé pour prendre en charge de DirectAccess avec secret à usage unique.  
  
5.  Cliquez sur **Suivant**.  
  
6.  Dans le **OTP RADIUS Server** section, double-cliquez sur l’essai à blanc **nom du serveur** champ.  
  
7.  Dans le **ajouter un serveur RADIUS** boîte de dialogue, tapez le nom du serveur RADIUS dans le **nom du serveur** champ. Cliquez sur **modification** à côté du **secret partagé** champ, puis tapez le mot de passe que vous avez utilisé lors de la configuration de serveur RADIUS dans le **nouveau secret** et  **Confirmer le nouveau secret** champs. Cliquez sur **OK** à deux reprises, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > Si le serveur RADIUS est dans un domaine qui est différent de celle du serveur d’accès à distance, puis le **nom du serveur** le champ doit spécifier le nom de domaine complet du serveur RADIUS.  
  
8.  Dans le **serveurs d’autorité de certification OTP** section Sélectionner les serveurs d’autorité de certification à utiliser pour l’inscription des certificats d’authentification OTP client, puis cliquez sur **ajouter**. Cliquez sur **Suivant**.  
  
9. Dans le **les modèles de certificats OTP** section cliquez **Parcourir** pour sélectionner le modèle de certificat utilisé pour l’inscription des certificats émis pour l’authentification OTP.  
  
    > [!NOTE]  
    > Le modèle de certificat pour les certificats OTP émis par l’autorité de certification d’entreprise doit être configuré sans l’option « N’incluent pas les informations de révocation de certificats émis ». Si cette option est sélectionnée lors de la création de modèle de certificat, les ordinateurs clients OTP échouera se connecter correctement.  
  
    Cliquez sur **Parcourir** pour sélectionner un modèle de certificat utilisé pour inscrire le certificat utilisé par le serveur d’accès à distance pour signer les demandes d’inscription de certificat OTP. Cliquez sur **OK**. Cliquez sur **Suivant**.  
  
10. Si l’exemption d’utilisateurs spécifiques à partir de DirectAccess avec l’OTP est nécessaire, puis dans le **OTP Exemptions** section Sélectionnez **ne nécessitent pas d’utilisateurs dans le groupe de sécurité spécifié pour s’authentifier à l’aide de l’authentification à deux facteurs** . Cliquez sur **groupe de sécurité** et sélectionnez le groupe de sécurité qui a été créé pour les exemptions de secret à usage unique.  
  
11. Sur le **le programme d’installation du serveur d’accès à distance** page, cliquez sur **Terminer**.  
  
12. Dans le **le programme d’installation de DirectAccess** fenêtre, sous **étape 3 : serveurs d’Infrastructure**, cliquez sur **modifier**.  
  
13. Cliquez sur **suivant** deux fois, puis, dans le **gestion** double-clic de la section la **serveurs d’administration** champ.  
  
14. Entrez le **nom de l’ordinateur** ou **adresse** du serveur d’autorité de certification qui est configuré pour émettre des certificats de l’OTP, puis cliquez sur **OK**.  
  
15. Dans le **configuration de l’accès à distance** cliquez sur windows **Terminer**.  
  
16. Cliquez sur **Terminer** sur le **DirectAccess Expert Assistant**.  
  
17. Sur le **révision d’accès à distance** boîte dialogue, cliquez sur **appliquer**, attendez que la stratégie de DirectAccess à mettre à jour, puis cliquez sur **fermer**.  
  
18. Sur le **Démarrer** , tapez**powershell.exe**, avec le bouton droit **powershell**, cliquez sur **avancé**, puis cliquez sur **d’identification administrateur**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
19. Dans la fenêtre Windows PowerShell, tapez **gpupdate /force** et appuyez sur ENTRÉE.  
  
Pour configurer l’accès à distance pour le secret à usage unique à l’aide des commandes PowerShell :  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Remote-Access-Server-for-OTP/PowerShellLogoSmall.gif)**commandes Windows PowerShell équivalentes**  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Pour configurer l’accès à distance pour utiliser l’authentification à deux facteurs sur un déploiement qui utilise actuellement une authentification de certificat d’ordinateur :  
  
```  
Set-DAServer -UserAuthentication TwoFactor  
```  
  
Pour configurer l’accès à distance pour utiliser l’authentification OTP en utilisant les paramètres suivants :  
  
-   Un serveur de secret à usage unique nommé OTP.corp.contoso.com.  
  
-   Un serveur d’autorité de certification nommé APP1.corp.contoso.com\corp-APP1-CA1.  
  
-   Un modèle de certificat nommé DAOTPLogon utilisé pour l’inscription des certificats émis pour l’authentification unique.  
  
-   Un modèle de certificat nommé DAOTPRA utilisé pour inscrire le certificat d’autorité d’inscription permet de signer des demandes d’inscription de certificat OTP par le serveur d’accès à distance.  
  
```  
Enable-DAOtpAuthentication -CertificateTemplateName 'DAOTPLogon' -SigningCertificateTemplateName 'DAOTPRA' -CAServer @('APP1.corp.contoso.com\corp-APP1-CA1') -RadiusServer OTP.corp.contoso.com -SharedSecret Abcd123$  
```  
  
Après l’exécution des commandes PowerShell terminer les étapes 12-19 à partir de la procédure précédente pour configurer le serveur d’accès à distance pour prendre en charge le secret à usage unique.  
  
> [!NOTE]  
> Veillez à vérifier que vous avez appliqué les paramètres OTP sur le serveur d’accès à distance avant d’ajouter un point d’entrée.  
  
## <a name="BKMK_Smartcard"></a>3.3 les cartes à puce pour une autorisation supplémentaire  
Sur la page d’authentification de l’étape 2 dans l’Assistant Installation de l’accès à distance, vous pouvez exiger l’utilisation de cartes à puce pour l’accès au réseau interne. Lorsque cette option est sélectionnée, l’Assistant Installation de l’accès à distance configure la règle de sécurité de connexion IPsec pour le tunnel intranet sur le serveur DirectAccess pour exiger l’autorisation en mode de tunnel avec cartes à puce. Autorisation en mode tunnel permet de spécifier que seuls les ordinateurs autorisés ou les utilisateurs peuvent établir un tunnel entrant.  
  
Pour utiliser des cartes à puce avec l'autorisation en mode tunnel IPsec pour le tunnel intranet, vous devez déployer une infrastructure à clé publique (PKI) avec l'infrastructure à cartes à puce.  
  
Étant donné que vos clients DirectAccess sont à l’aide de cartes à puce pour l’accès à l’intranet, vous pouvez également utiliser l’assurance du mécanisme d’authentification, une fonctionnalité de Windows Server 2008 R2, pour contrôler l’accès aux ressources, telles que des fichiers, dossiers et des imprimantes, selon que le utilisateur connecté avec un certificat basé sur une carte à puce. Assurance du mécanisme d’authentification requiert un niveau fonctionnel du domaine de Windows Server 2008 R2.  
  
### <a name="allowing-access-for-users-with-unusable-smart-cards"></a>Autorisation de l'accès aux utilisateurs munis de cartes à puce inutilisables  
Pour autoriser l'accès temporaire aux utilisateurs munis de cartes à puce qui sont inutilisables, procédez comme suit :  
  
1.  Créez un groupe de sécurité Active Directory pour contenir les comptes des utilisateurs qui ne peuvent pas utiliser temporairement leurs cartes à puce.  
  
2.  Pour l’objet stratégie de groupe du serveur DirectAccess, configurez les paramètres IPsec globaux pour l’autorisation de tunnel IPsec et ajoutez le groupe de sécurité Active Directory à la liste des utilisateurs autorisés.  
  
Pour accorder l'accès à un utilisateur qui ne peut pas utiliser sa carte à puce, ajoutez temporairement son compte d'utilisateur au groupe de sécurité Active Directory. Supprimez le compte d'utilisateur du groupe lorsque la carte à puce est utilisable.  
  
### <a name="under-the-covers-smart-card-authorization"></a>En coulisse : Autorisation par carte à puce  
L'autorisation par carte à puce fonctionne grâce à l'activation de l'autorisation en mode tunnel sur la règle de sécurité de connexion du tunnel intranet du serveur DirectAccess pour un identificateur de sécurité (SID) Kerberos spécifique. Pour l'autorisation par carte à puce, c'est le SID bien connu (S-1-5-65), qui effectue les mappages aux ouvertures de session à base de cartes à puce. Ce SID est présent dans le jeton Kerberos d’un client DirectAccess et est appelé pour les paramètres du mode d’autorisation du tunnel en tant que « Ce certificat d’organisation » configuré dans l’IPsec global.  
  
Lorsque vous activez l’autorisation de carte à puce à l’étape 2 de l’Assistant Installation DirectAccess, l’Assistant Installation DirectAccess configure le paramètre d’autorisation du mode de tunnel IPsec global avec ce SID pour l’objet stratégie de groupe du serveur DirectAccess. Pour afficher cette configuration dans le pare-feu Windows avec fonctions avancées de sécurité, composant logiciel enfichable pour l’objet stratégie de groupe du serveur DirectAccess, procédez comme suit :  
  
1.  Cliquez avec le bouton droit sur le pare-feu Windows avec fonctions avancées de sécurité, puis cliquez sur Propriétés.  
  
2.  Sous l’onglet Paramètres IPsec, en matière d’autorisation de tunnel IPsec, cliquez sur Personnaliser.  
  
3.  Cliquez sur l’onglet utilisateurs. Vous devez voir le « NT authority\ce certificat d’organisation » en tant qu’un utilisateur autorisé.  
  


