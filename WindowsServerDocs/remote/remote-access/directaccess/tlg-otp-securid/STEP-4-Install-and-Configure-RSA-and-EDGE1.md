---
title: ÉTAPE 4 installer et configurer RSA et EDGE1
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer DirectAccess avec l’authentification par mot de passe à usage unique et RSA SecurID pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d46ede6f-1a21-414d-b8c3-6b5c87344b9d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3519e9d6e89b26a733b6b0178334f86e284bddfb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404758"
---
# <a name="step-4-install-and-configure-rsa-and-edge1"></a>ÉTAPE 4 installer et configurer RSA et EDGE1

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

RSA est le serveur RADIUS et le mot de passe à usage unique, et est installé avant la configuration de RADIUS et du mot de passe à usage unique.  
  
Vous allez effectuer les étapes suivantes pour configurer le déploiement RSA :  
  
1. Installez le système d’exploitation sur le serveur RSA. Installez Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 sur le serveur RSA.  
  
2. Configurez TCP/IP sur RSA. Configurez les paramètres TCP/IP sur le serveur RSA.  
  
3. Copiez les fichiers d’installation du gestionnaire d’authentification sur le serveur RSA. Après l’installation du système d’exploitation sur RSA, copiez les fichiers du gestionnaire d’authentification sur l’ordinateur RSA.  
  
4. Joignez le serveur RSA au domaine CORP. Joindre RSA au domaine CORP.  
  
5. Désactivez le pare-feu Windows sur RSA. Désactivez le pare-feu Windows sur le serveur RSA.  
  
6. Installez le gestionnaire d’authentification RSA sur le serveur RSA. Installez le gestionnaire d’authentification RSA.  
  
7. Configurez le gestionnaire d’authentification RSA. Configurez le gestionnaire d’authentification.  
  
8. Créez DAProbeUser. Créez un compte d’utilisateur à des fins de détection.  
  
9. Installez le jeton logiciel RSA SecurID sur CLIENT1. Installez le jeton logiciel RSA SecurID sur CLIENT1.  
  
10. Configurez EDGE1 en tant qu’agent d’authentification RSA. Configurez l’agent d’authentification RSA sur EDGE1.  
  
11. Configurer EDGE1 pour prendre en charge l’authentification par mot de passe à usage unique. Configurez le mot de passe à usage unique pour DirectAccess et vérifiez la configuration.  
  
## <a name="InstallOS"></a>Installer le système d’exploitation sur le serveur RSA  
  
1.  Sur RSA, démarrez l’installation de Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Suivez les instructions pour terminer l’installation, en spécifiant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (installation complète) et un mot de passe fort pour le compte administrateur local. Ouvrez une session à l’aide du compte Administrateur local.  
  
3.  Connectez RSA à un réseau disposant d’un accès Internet et exécutez Windows Update pour installer les dernières mises à jour pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, puis vous déconnecter d’Internet.  
  
4.  Connectez RSA au sous-réseau Corpnet.  
  
## <a name="TCP"></a>Configurer TCP/IP sur RSA  
  
1.  Dans Tâches de configuration initiales, cliquez sur **configurer la mise en réseau**.  
  
2.  Dans **connexions réseau**, cliquez avec le bouton droit sur connexion au réseau **local**, puis cliquez sur **Propriétés**.  
  
3.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
4.  Cliquez sur **Utiliser l’adresse IP suivante**. Dans la zone **Adresse IP**, tapez **10.0.0.5**. Dans la zone **Masque de sous-réseau**, tapez **255.255.255.0**. Dans **passerelle par défaut**, tapez **10.0.0.2**. Cliquez sur **utiliser l’adresse de serveur DNS suivante**, dans **serveur DNS préféré**, tapez **10.0.0.1**.  
  
5.  Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
6.  Dans **suffixe DNS pour cette connexion**, tapez **Corp.contoso.com**, puis cliquez deux fois sur **OK** .  
  
7.  Dans la boîte de dialogue **Propriétés de connexion au réseau local** , cliquez sur **Fermer**.  
  
8.  Fermez la fenêtre **Connexions réseau**.  
  
## <a name="copyinstfiles"></a>Copier les fichiers d’installation du gestionnaire d’authentification sur le serveur RSA  
  
1.  Sur le serveur RSA, créez le dossier C:\RSA installation.  
  
2.  Copiez le contenu du média RSA Authentication Manager 7,1 SP4 dans le dossier d’installation de C:\RSA.  
  
3.  Créez le sous-dossier C:\RSA Installation\License et Token.  
  
4.  Copiez les fichiers de licence RSA dans C:\RSA Installation\License et Token.  
  
## <a name="JoinDomain"></a>Joindre le serveur RSA au domaine CORP  
  
1.  Cliquez avec le bouton droit sur **poste de travail**, puis cliquez sur **Propriétés**.  
  
2.  Dans la boîte de dialogue **Propriétés système** , sous l’onglet **Nom de l’ordinateur** , cliquez sur **Modifier**.  
  
3.  Dans **nom**de l’ordinateur, tapez **RSA**. Dans **membre de**, cliquez sur **domaine**, tapez **Corp.contoso.com**, puis cliquez sur **OK**.  
  
4.  Lorsque vous êtes invité à entrer un nom d’utilisateur et un mot de passe, tapez **User1** et son mot de passe, puis cliquez sur **OK**.  
  
5.  Dans la boîte de dialogue Accueil du domaine, cliquez sur **OK**.  
  
6.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
9. Une fois l’ordinateur redémarré, tapez **User1** et le mot de passe, sélectionnez Corp dans la liste déroulante **se connecter à :** , puis cliquez sur **OK**.  
  
## <a name="BKMK_Firewall"></a>Désactiver le pare-feu Windows sur RSA  
  
1.  Cliquez sur **Démarrer**, sur **panneau**de configuration, sur **système et sécurité**, puis sur **pare-feu Windows**.  
  
2.  Cliquez sur **activer ou désactiver le pare-feu Windows**.  
  
3.  **Désactivez le pare-feu Windows** pour tous les paramètres.  
  
4.  Cliquez sur **OK** et fermez le pare-feu Windows.  
  
## <a name="install"></a>Installer le gestionnaire d’authentification RSA sur le serveur RSA  
  
1.  Si le message d’avertissement de sécurité s’affiche à tout moment au cours de ce processus, cliquez sur **exécuter** pour continuer.  
  
2.  Ouvrez le dossier d’installation de C:\RSA et double-cliquez sur **Autorun. exe**.  
  
3.  Cliquez sur **Installer maintenant**, cliquez sur **suivant**, sélectionnez l’option Top pour l’Amérique, puis cliquez sur **suivant**.  
  
4.  Sélectionnez **J’accepte les termes du contrat de licence**, puis cliquez sur **suivant**.  
  
5.  Sélectionnez **instance principale**, puis cliquez sur **suivant**.  
  
6.  Dans le champ **nom du répertoire :** , tapez **C:\RSA**, puis cliquez sur **suivant**.  
  
7.  Vérifiez que le nom du serveur (RSA.corp.contoso.com) et l’adresse IP sont corrects, puis cliquez sur **suivant**.  
  
8.  Accédez à C:\RSA Installation\License et Token, puis cliquez sur **suivant**.  
  
9. Dans la page **vérifier le fichier de licence** , cliquez sur **suivant**.  
  
10. Dans le champ **ID d’utilisateur** , tapez **administrateur**, puis dans les champs **mot** de passe et confirmer le **mot de passe** , tapez un mot de passe fort. Cliquez sur **Suivant**.  
  
11. Dans l’écran sélection du journal, acceptez les valeurs par défaut et cliquez sur **suivant**.  
  
12. Dans l’écran Résumé, cliquez sur **installer**.  
  
13. Une fois l’installation terminée, cliquez sur **Terminer**.  
  
## <a name="confiauthmgr"></a>Configurer le gestionnaire d’authentification RSA  
  
1.  Si la console de sécurité RSA ne s’ouvre pas automatiquement, sur le Bureau de l’ordinateur RSA, double-cliquez sur « RSA Security Console ».  
  
2.  Si l’alerte de sécurité/Avertissement de certificat de sécurité s’affiche, cliquez sur **Continuer sur ce site Web** ou cliquez sur **Oui** pour continuer et ajouter ce site aux sites de confiance, si nécessaire.  
  
3.  Dans le champ **ID d’utilisateur** , tapez **administrateur** , puis cliquez sur **OK**.  
  
4.  Dans le champ **mot de passe** , tapez le mot de passe du compte administrateur, puis cliquez sur **ouvrir une session**.  
  
5.  Insérer des informations de jeton.  
  
    1.  Dans la **console de sécurité RSA** , cliquez sur **authentification** , puis sur **jetons SecurID**.  
  
    2.  Cliquez sur **travail d’importation des jetons**, puis sur **Ajouter nouveau**.  
  
    3.  Dans la section **options d’importation** , cliquez sur **Parcourir**. Recherchez et sélectionnez le fichier XML Tokens dans le dossier C:\ RSA Installation\License et le dossier Token, puis cliquez sur **ouvrir**.  
  
    4.  Cliquez sur **Envoyer le travail** en bas de la page.  
  
6.  Créer un utilisateur avec mot de passe à usage unique.  
  
    1.  Dans la **console de sécurité RSA** , cliquez sur l’onglet **identité** , cliquez sur **utilisateurs**, puis cliquez sur **Ajouter nouveau**.  
  
    2.  Dans la section **Last Name :** type **User**, et dans la section **ID utilisateur :** type **User1** (UserID doit être identique au nom d’utilisateur AD utilisé pour ce Lab).  Dans les sections **mot de passe :** et **confirmer le mot de passe :** tapez un mot de passe fort. Désactivez la case à cocher **« demander à l’utilisateur de modifier le mot de passe à la prochaine connexion »** , puis cliquez sur **Enregistrer**.  
  
7.  Affectez User1 à l’un des jetons importés.  
  
    1.  Dans la page **utilisateurs** , cliquez sur **User1** , puis sur **jetons SecurID**.  
  
    2.  Cliquez sur **jetons SecurID** , puis sur **attribuer un jeton**.  
  
    3.  Sous l’en-tête **numéro de série** , cliquez sur le premier numéro figurant dans la liste, puis cliquez sur **attribuer**.  
  
    4.  Cliquez sur le jeton affecté, puis sur **modifier**. Dans la section **gestion des codes confidentiels SecurID** pour la **spécification de l’authentification utilisateur**, sélectionnez **ne pas exiger d’épingler (uniquement code de jeton)** .  
  
    5.  Cliquez sur **enregistrer et distribuer le jeton**.  
  
    6.  Dans la page **distribuer le jeton logiciel** de la section **notions de base** , cliquez sur émettre un **fichier de jeton (SDTID)** .  
  
    7.  Sur la page **distribuer le jeton logiciel** dans la section **Options du fichier de jeton** , désactivez la case à cocher Activer la **protection contre la copie** . Cliquez sur **aucun mot de passe** et sur **suivant**.  
  
    8.  Sur la page **distribuer le jeton logiciel** dans la section **fichier de téléchargement** , cliquez sur **Télécharger maintenant**. Cliquez sur **Enregistrer**. Accédez à installation de C:\RSA, puis cliquez sur **Enregistrer** et **Fermer**.  
  
    9. Réduisez la **console de sécurité RSA** pour une utilisation ultérieure.  
  
8.  Configurez le gestionnaire d’authentification en tant que serveur RADIUS.  
  
    1.  Sur le Bureau de l’ordinateur RSA, double-cliquez sur **« RSA Security Operations console »** .  
  
    2.  Si l’alerte de sécurité/Avertissement de certificat de sécurité s’affiche, cliquez sur **Continuer sur ce site Web** ou cliquez sur **Oui** pour continuer et ajouter ce site aux sites de confiance, le cas échéant.  
  
    3.  Entrez l’ID d’utilisateur et le mot de passe, puis cliquez sur **ouvrir une session**.  
  
    4.  Cliquez sur **configuration du déploiement-RADIUS-configurer le serveur**.  
  
    5.  Dans la page **informations d’identification supplémentaires requises** , entrez l’ID d’utilisateur et le mot de passe de l’administrateur, puis cliquez sur **OK**.  
  
    6.  Dans la page **configurer le serveur RADIUS** , entrez le même mot de passe que celui utilisé pour l’utilisateur administrateur pour les **secrets** et le **mot de passe principal**. Entrez l’ID d’utilisateur et le mot de passe de l’administrateur, puis cliquez sur **configurer**.  
  
    7.  Vérifiez que le message **« serveur RADIUS correctement configuré »** s’affiche. Cliquez sur **Terminé**. Fermez la **console opérateur RSA**.  
  
    8.  Revenez à la **« console de sécurité RSA »** .  
  
    9. Sous l’onglet **RADIUS** , cliquez sur **serveurs RADIUS**. Vérifiez que rsa.corp.contoso.com est listé.  
  
9. Configurez le serveur RSA en tant que client d’authentification RSA.  
  
    1.  Sous l’onglet **RADIUS** , cliquez sur **clients RADIUS** et **Ajouter nouveau**.  
  
    2.  Activez la case à cocher **n’importe quel client RADIUS** .  
  
    3.  Tapez un mot de passe fort de votre choix dans le champ **secret partagé** . Vous utiliserez ce même mot de passe ultérieurement lors de la configuration de EDGE1 pour le mot de passe à usage unique.  
  
    4.  Laissez le champ **adresse IP** vide et l’entrée **Make/Model** comme **rayon standard**.  
  
    5.  Cliquez sur **enregistrer sans l’agent RSA**.  
  
10. Créez les fichiers nécessaires à la configuration de EDGE1 en tant qu’agent d’authentification RSA.  
  
    1.  Sous l’onglet **accès** , mettez en surbrillance **agents d’authentification**, puis cliquez sur **Ajouter nouveau**.  
  
    2.  Tapez **Edge1** dans le champ **nom d’hôte** , puis cliquez sur résoudre l' **adresse IP**.  
  
    3.  Notez que l’adresse IP de EDGE1 s’affiche désormais dans le champ **adresse IP** . Cliquez sur **Enregistrer**.  
  
11. Générez un fichier de configuration pour le serveur EDGE1 (AM_Config. zip).  
  
    1.  Sous l’onglet **accès** , mettez en surbrillance **agents d’authentification**, puis cliquez sur **générer le fichier de configuration**.  
  
    2.  Dans la page **générer un fichier de configuration** , cliquez sur générer un **fichier**de configuration, puis cliquez sur **Télécharger maintenant**.  
  
    3.  Cliquez sur **Enregistrer**, accédez à C:\ Installation RSA, puis cliquez sur **Enregistrer**.  
  
    4.  Cliquez sur **Fermer** dans la boîte de dialogue **Téléchargement terminé** .  
  
12. Générez un fichier de secret de nœud pour le serveur EDGE1 (EDGE1_NodeSecret. zip).  
  
    1.  Sous l’onglet **accès** , mettez en surbrillance **agents d’authentification**, puis cliquez sur **gérer les existants**.  
  
    2.  Cliquez sur le nœud configuré actuel EDGE1, puis sur **gérer le secret du nœud**.  
  
    3.  Cochez la case **créer un nouveau secret de nœud aléatoire et exporter le secret de nœud dans un fichier** .  
  
    4.  Entrez le même mot de passe utilisé pour l’utilisateur administrateur dans les champs **mot de passe de chiffrement** et **confirmer le mot de passe de chiffrement** , puis cliquez sur **Enregistrer**.  
  
    5.  Sur la page **fichier secret de nœud généré** , cliquez sur **Télécharger maintenant**.  
  
    6.  Dans la boîte de dialogue de **téléchargement de fichier** , cliquez sur **Enregistrer**, accédez à installation de C:\RSA, puis cliquez sur **Enregistrer**. Cliquez sur **Fermer** dans la boîte de dialogue **Téléchargement terminé** .  
  
    7.  À partir du support du gestionnaire d’authentification RSA, copiez \auth_mgr\windows-x86_64\am\rsa-ace_nsload\win32-5.0-x86\agent_nsload.exe vers C:\RSA.  
  
## <a name="BKMK_DAProbeUser"></a>Créer DAProbeUser  
  
1.  Dans la **console de sécurité RSA** , cliquez sur l’onglet **identité** , cliquez sur **utilisateurs**, puis cliquez sur **Ajouter nouveau**.  
  
2.  Dans la section **Last Name :** type **section, et**dans la section **User ID :** , tapez **DAProbeUser**. Dans les sections **mot de passe :** et **confirmer le mot de passe :** tapez un mot de passe fort. Désactivez la case à cocher **« demander à l’utilisateur de modifier le mot de passe à la prochaine connexion »** , puis cliquez sur **Enregistrer**.  
  
## <a name="InstToken"></a>Installer le jeton logiciel RSA SecurID sur CLIENT1  
Utilisez cette procédure pour installer le jeton logiciel SecurID sur CLIENT1.  
  
#### <a name="install-securid-software-token"></a>Installer le jeton logiciel SecurID  
  
1.  Sur l’ordinateur CLIENT1, créez le dossier C:\RSA files. Copiez le fichier Software_Tokens. zip à partir de l’installation C:\RSA sur l’ordinateur RSA vers les fichiers C:\RSA. Extrayez le fichier User1_000031701832. SDTID dans les fichiers C:\RSA sur CLIENT1.  
  
2.  Accédez à la source du média du jeton logiciel RSA SecurID, puis double-cliquez sur RSASECURIDTOKEN410 dans le dossier **SecurID SoftwareToken client App** pour démarrer l’installation de RSA SecurID. Si le message **ouverture d’un fichier-Avertissement de sécurité** s’affiche, cliquez sur **exécuter**.  
  
3.  Dans la boîte de dialogue **RSA SecurID Software Token-InstallShield Wizard,** cliquez deux fois sur **suivant** .  
  
4.  Acceptez le contrat de licence, puis cliquez sur **suivant**.  
  
5.  Dans la boîte de dialogue **type d’installation** , sélectionnez **standard**, cliquez sur **suivant**, puis sur **installer**.  
  
6.  Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
7.  Activez la case à cocher **lancer le jeton logiciel RSA SecurID** , puis cliquez sur **Terminer**.  
  
8.  Cliquez sur **Importer à partir d’un fichier**.  
  
9. Cliquez sur **Parcourir**, sélectionnez C:\RSA Files\User1_000031701832.SDTID, puis cliquez sur **ouvrir**.  
  
10. Cliquez deux fois sur **OK**.  
  
## <a name="configAuthAgt"></a>Configurer EDGE1 en tant qu’agent d’authentification RSA  
Utilisez cette procédure pour configurer EDGE1 afin d’effectuer l’authentification RSA.  
  
#### <a name="configure-the-rsa-authentication-agent"></a>Configurer l’agent d’authentification RSA  
  
1. Sur EDGE1, ouvrez l’Explorateur Windows et créez le dossier C:\RSA fichiers. Accédez au support d’installation de RSA ACE.  
  
2. Copiez les fichiers agent_nsload. exe, AM_Config. zip et EDGE1_NodeSecret. zip à partir du média RSA vers les fichiers C:\RSA.  
  
3. Extrayez le contenu des deux fichiers zip aux emplacements suivants :  
  
   1.  C:\Windows\System32  
  
   2.  C:\Windows\SysWOW64  
  
4. Copiez agent_nsload. exe sur C:\Windows\SysWOW64 @ no__t-0.  
  
5. Ouvrez une invite de commandes avec élévation de privilèges et accédez à C:\Windows\SysWOW64.  
  
6. Tapez **agent_nsload. exe-f nodesecret. Rec-p <password>** où <password> est le mot de passe fort que vous avez créé lors de la configuration RSA initiale. Appuyez sur Entrée.  
  
7. Copiez C:\Windows\SysWOW64\securid dans C:\Windows\System32.  
  
## <a name="configOTP"></a>Configurer EDGE1 pour prendre en charge l’authentification OTP  
Utilisez cette procédure pour configurer le mot de passe à usage unique pour DirectAccess et vérifier la configuration.  
  
#### <a name="configure-otp-for-directaccess"></a>Configurer le mot de passe à usage unique pour DirectAccess  
  
1.  Sur EDGE1, ouvrez Gestionnaire de serveur, puis cliquez sur **accès à distance** dans le volet gauche.  
  
2.  Cliquez avec le bouton droit sur **Edge1** dans le volet serveurs, puis sélectionnez **gestion de l’accès à distance**.  
  
3.  Cliquez sur **configuration**.  
  
4.  Dans la fenêtre **Installation DirectAccess** , sous **étape 2 : serveur d’accès à distance**, cliquez sur **modifier**.  
  
5.  Cliquez sur **suivant** trois fois. dans la section **authentification** , sélectionnez **authentification à deux facteurs** et utilisez le mot de passe à **usage unique**et assurez-vous que l’option **utiliser des certificats d’ordinateur** est cochée. Vérifiez que l’autorité de certification racine est définie sur **CN = Corp-App1-ca**. Cliquez sur **Suivant**.  
  
6.  Dans la section **serveur RADIUS OTP** , double-cliquez sur le champ **nom du serveur** vide.  
  
7.  Dans la boîte de dialogue **Ajouter un serveur RADIUS** , tapez **RSA** dans le champ **nom du serveur** . Cliquez sur **modifier** en regard du champ **secret partagé** , puis tapez le même mot de passe que celui que vous avez utilisé lors de la configuration des clients RADIUS sur le serveur RSA dans les champs **nouveau secret** et **confirmer le nouveau secret** . Cliquez deux fois sur **OK** , puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > Si le serveur RADIUS est dans un domaine différent de celui du serveur d’accès à distance, le champ **nom du serveur** doit spécifier le nom de domaine complet du serveur RADIUS.  
  
8.  Dans la section **serveurs d’autorité de certification avec mot de passe à usage unique** , sélectionnez App1.Corp.contoso.com, puis cliquez sur **Ajouter**. Cliquez sur **Suivant**.  
  
9. Sur la page **modèles de certificats avec mot de passe à usage unique** , cliquez sur **Parcourir** pour sélectionner un modèle de certificat utilisé pour l’inscription des certificats émis pour l’authentification par mot de passe à usage unique, puis, dans la boîte de dialogue **modèles de certificat** , sélectionnez **DAOTPLogon** . Cliquez sur **OK**. Cliquez sur **Parcourir** pour sélectionner un modèle de certificat utilisé pour inscrire le certificat utilisé par le serveur d’accès à distance afin de signer les demandes d’inscription de certificat avec mot de passe à usage unique, puis, dans la boîte de dialogue **modèles de certificat** , sélectionnez **DAOTPRA**. Cliquez sur **OK**. Cliquez sur **Suivant**.  
  
10. Sur la page **installation du serveur d’accès à distance** , cliquez sur **Terminer**, puis sur **Terminer** dans l' **Assistant expert DirectAccess**.  
  
11. Dans la boîte de dialogue vérification de l' **accès à distance** , cliquez sur **appliquer**, attendez que la Stratégie DirectAccess soit mise à jour, puis cliquez sur **Fermer**.  
  
12. Dans l’écran d' **Accueil** , tapez**PowerShell. exe**, cliquez avec le bouton droit sur **PowerShell**, cliquez sur **avancé**, puis sur **exécuter en tant qu’administrateur**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
13. Dans la fenêtre Windows PowerShell, tapez **gpupdate/force** , puis appuyez sur entrée.  
  
14. Fermez et rouvrez la console de gestion de l’accès à distance et vérifiez que tous les paramètres de mot de passe à usage unique sont corrects.  
  


