---
title: ÉTAPE 4 installer et configurer RSA et EDGE1
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
ms.assetid: d46ede6f-1a21-414d-b8c3-6b5c87344b9d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5bb28ff6131c371e4b2f668fd20ec0a6133a0099
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860000"
---
# <a name="step-4-install-and-configure-rsa-and-edge1"></a>ÉTAPE 4 installer et configurer RSA et EDGE1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

RSA est le serveur RADIUS et le secret à usage unique et est installé avant de configurer le protocole RADIUS et le secret à usage unique.  
  
Vous allez effectuer les étapes suivantes pour configurer le déploiement de RSA :  
  
1. Installer le système d’exploitation sur le serveur RSA. Installez Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 sur le serveur RSA.  
  
2. Configurer TCP/IP sur RSA. Configurer les paramètres TCP/IP sur le serveur RSA.  
  
3. Copiez les fichiers d’installation de gestionnaire d’authentification sur le serveur RSA. Après avoir installé le système d’exploitation sur RSA, copiez les fichiers du Gestionnaire d’authentification sur l’ordinateur RSA.  
  
4. Joindre le serveur RSA au domaine CORP. Joindre RSA au domaine CORP.  
  
5. Désactiver le pare-feu Windows sur RSA. Désactiver le pare-feu Windows sur le serveur RSA.  
  
6. Installer le Gestionnaire d’authentification RSA sur le serveur RSA. Installer le Gestionnaire d’authentification RSA.  
  
7. Configurer le Gestionnaire d’authentification RSA. Configurer le Gestionnaire d’authentification.  
  
8. Créer DAProbeUser. Créer un compte d’utilisateur pour des raisons de détection.  
  
9. Installer le jeton de logiciel RSA SecurID sur CLIENT1. Installer le jeton de logiciel RSA SecurID sur CLIENT1.  
  
10. Configurer EDGE1 comme un Agent d’authentification RSA. Configurer l’Agent d’authentification RSA sur EDGE1.  
  
11. Configurer la prise en charge l’authentification OTP EDGE1. Configurer OTP pour DirectAccess et vérifiez la configuration.  
  
## <a name="InstallOS"></a>Installer le système d’exploitation sur le serveur RSA  
  
1.  RSA, démarrez l’installation de Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Suivez les instructions pour terminer l’installation, en spécifiant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (Installation complète) et un mot de passe fort pour le compte d’administrateur local. Ouvrez une session à l’aide du compte Administrateur local.  
  
3.  Connecter RSA à un réseau qui a accès à Internet et exécutez la mise à jour de Windows pour installer les dernières mises à jour pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 et vous déconnecter puis à partir d’Internet.  
  
4.  Se connecter à RSA au sous-réseau Corpnet.  
  
## <a name="TCP"></a>Configurer TCP/IP sur RSA  
  
1.  Dans les tâches de Configuration initiales, cliquez sur **configurer le réseau**.  
  
2.  Dans **connexions réseau**, avec le bouton droit **connexion au réseau Local**, puis cliquez sur **propriétés**.  
  
3.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)**, puis sur **Propriétés**.  
  
4.  Cliquez sur **Utiliser l’adresse IP suivante**. Dans la zone **Adresse IP**, tapez **10.0.0.5**. Dans la zone **Masque de sous-réseau**, tapez **255.255.255.0**. Dans **passerelle par défaut**, type **10.0.0.2**. Cliquez sur **utiliser l’adresse de serveur DNS suivante**, dans **serveur DNS préféré**, type **10.0.0.1**.  
  
5.  Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
6.  Dans **suffixe DNS pour cette connexion**, type **corp.contoso.com**, puis cliquez sur **OK** à deux reprises.  
  
7.  Sur le **propriétés de connexion de réseau Local** boîte de dialogue, cliquez sur **fermer**.  
  
8.  Fermez la fenêtre **Connexions réseau**.  
  
## <a name="copyinstfiles"></a>Copiez les fichiers d’installation de gestionnaire d’authentification sur le serveur RSA  
  
1.  Sur le serveur RSA créez le dossier C:\RSA Installation.  
  
2.  Copiez le contenu du média RSA Authentication Manager 7.1 SP4 dans le dossier d’Installation de C:\RSA.  
  
3.  Créez le sous-dossier C:\RSA Installation\License et le jeton.  
  
4.  Copiez les fichiers de licence RSA à C:\RSA Installation\License et le jeton.  
  
## <a name="JoinDomain"></a>Joindre le serveur RSA au domaine CORP  
  
1.  Avec le bouton droit **poste de travail**, puis cliquez sur **propriétés**.  
  
2.  Dans la boîte de dialogue **Propriétés système** , sous l’onglet **Nom de l’ordinateur** , cliquez sur **Modifier**.  
  
3.  Dans **nom de l’ordinateur**, type **RSA**. Dans **membre**, cliquez sur **domaine**, type **corp.contoso.com**, puis cliquez sur **OK**.  
  
4.  Lorsque vous êtes invité à un nom d’utilisateur et le mot de passe, tapez **User1** et son mot de passe, puis cliquez sur **OK**.  
  
5.  Sur le domaine de la boîte de dialogue de bienvenue, cliquez sur **OK**.  
  
6.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
9. Après avoir redémarré l’ordinateur, tapez **User1** et le mot de passe, sélectionnez CORP dans le **ouvrir une session sur :** liste déroulante, puis cliquez sur **OK**.  
  
## <a name="BKMK_Firewall"></a>Désactiver le pare-feu Windows sur RSA  
  
1.  Cliquez sur **Démarrer**, cliquez sur **le panneau de configuration**, cliquez sur **système et sécurité**, puis cliquez sur **Windows Firewall**.  
  
2.  Cliquez sur **activer ou désactiver le pare-feu Windows**.  
  
3.  **Désactiver le pare-feu de Windows** pour tous les paramètres.  
  
4.  Cliquez sur **OK** et fermer le pare-feu de Windows.  
  
## <a name="install"></a>Installer le Gestionnaire d’authentification RSA sur le serveur RSA  
  
1.  Si le message d’avertissement de sécurité s’affiche à tout moment pendant ce processus, cliquez sur **exécuter** pour continuer.  
  
2.  Ouvrez le dossier d’Installation de C:\RSA et double-cliquez sur **autorun.exe**.  
  
3.  Cliquez sur **installer maintenant**, cliquez sur **suivant**, sélectionnez l’option top pour l’Amérique, puis cliquez sur **suivant**.  
  
4.  Sélectionnez **J’accepte les termes du contrat de licence**, puis cliquez sur **suivant**.  
  
5.  Sélectionnez **Instance principale**, puis cliquez sur **suivant**.  
  
6.  Dans le **nom de répertoire :** type de champ **C:\RSA**, puis cliquez sur **suivant**.  
  
7.  Vérifiez que l’adresse IP et le nom du serveur (RSA.corp.contoso.com) sont corrects, puis cliquez sur **suivant**.  
  
8.  Accédez à C:\RSA Installation\License et le jeton, puis cliquez sur **suivant**.  
  
9. Sur le **fichier de licence Vérifiez** , cliquez sur **suivant**.  
  
10. Dans le **ID utilisateur** type de champ **administrateur**, puis, dans le **mot de passe** et **confirmer le mot de passe** champs type d’un mot de passe fort. Cliquez sur **Suivant**.  
  
11. Dans l’écran de sélection de journal, acceptez les valeurs par défaut et cliquez sur **suivant**.  
  
12. Dans l’écran Résumé, cliquez sur **installer**.  
  
13. Une fois l’installation terminée, cliquez sur **Terminer**.  
  
## <a name="confiauthmgr"></a>Configurer le Gestionnaire d’authentification RSA  
  
1.  Si la Console de sécurité RSA ne s’ouvre pas automatiquement, double-cliquez ensuite sur le bureau de l’ordinateur RSA sur « Console de sécurité RSA ».  
  
2.  Si la sécurité avertissement de certificat / l’alerte de sécurité s’affiche, cliquez sur **poursuivre sur ce site Web** ou cliquez sur **Oui** pour continuer et ajouter ce site aux sites approuvés, si nécessaire.  
  
3.  Dans le **ID utilisateur** type de champ **administrateur** et cliquez sur **OK**.  
  
4.  Dans le **mot de passe** le mot de passe pour le compte d’administrateur de type de champ et cliquez sur **ouverture de session**.  
  
5.  Insérer des informations de jeton.  
  
    1.  Dans le **RSA Security Console** cliquez sur **authentification** et cliquez sur **SecurID jetons**.  
  
    2.  Cliquez sur **travail d’importation jetons**, puis cliquez sur **Ajouter nouveau**.  
  
    3.  Dans le **Options d’importation** section cliquez **Parcourir**. Recherchez et sélectionnez le fichier XML de jetons dans le C:\ RSA Installation\License et jeton puis cliquez sur **Open**.  
  
    4.  Cliquez sur **Submit Job** au bas de la page.  
  
6.  Créer un nouvel utilisateur de secret à usage unique.  
  
    1.  Dans le **RSA Security Console** cliquez sur le **identité** , cliquez sur **utilisateurs**, puis cliquez sur **Ajouter nouveau**.  
  
    2.  Dans le **nom :** section type **utilisateur**, puis, dans le **ID d’utilisateur :** section type **User1** (UserID doit être le même que le nom d’utilisateur AD utilisé pour Ce laboratoire).  Dans le **mot de passe :** et **confirmer le mot de passe :** sections tapez un mot de passe fort. Effacer la **« Exiger l’utilisateur à modifier le mot de passe à la prochaine ouverture de session »** case à cocher et cliquez sur **enregistrer**.  
  
7.  Affecter User1 à un des jetons importés.  
  
    1.  Sur le **utilisateurs** page, cliquez sur **User1** et cliquez sur **SecurID jetons**.  
  
    2.  Cliquez sur **SecurID jetons** et cliquez sur **affecter jeton**.  
  
    3.  Sous le **numéro de série** en-tête cliquez sur le premier numéro indiqué, puis cliquez sur **affecter**.  
  
    4.  Cliquez sur le jeton attribué, puis cliquez sur **modifier**. Dans le **gestion du code confidentiel SecurID** section pour **exigence d’authentification utilisateur**, sélectionnez **ne nécessitent pas de code confidentiel (uniquement le code de jeton)**.  
  
    5.  Cliquez sur **enregistrer et distribuer le jeton**.  
  
    6.  Sur le **distribuer les logiciels jeton** page dans le **notions de base** , cliquez sur **fichier de jeton du problème (SDTID)**.  
  
    7.  Sur le **distribuer les logiciels jeton** page dans le **Options du fichier de jeton** section, désactivez le **activer la protection contre la copie** case à cocher. Cliquez sur **aucun mot de passe** et **suivant**.  
  
    8.  Sur le **distribuer les logiciels jeton** page dans le **télécharger le fichier** , cliquez sur **Télécharger maintenant**. Cliquez sur **Enregistrer**. Accédez à l’Installation de C:\RSA et cliquez sur **enregistrer** et **fermer**.  
  
    9. Réduire le **RSA Security Console** pour une utilisation ultérieure.  
  
8.  Configurer le Gestionnaire d’authentification en tant que serveur RADIUS.  
  
    1.  Sur le double-clic RSA ordinateur Bureau **« RSA Security Operations Console »**.  
  
    2.  Si la sécurité avertissement de certificat / l’alerte de sécurité s’affiche, cliquez sur **poursuivre sur ce site Web** ou cliquez sur **Oui** pour continuer et ajouter ce site aux sites approuvés si nécessaire.  
  
    3.  Entrez l’ID utilisateur et le mot de passe et cliquez sur **ouverture de session**.  
  
    4.  Cliquez sur **Configuration de déploiement - RADIUS - configurer le serveur**.  
  
    5.  Sur le **supplémentaire des informations d’identification requises** page, entrez l’ID d’utilisateur d’administrateur et le mot de passe et cliquez sur **OK**.  
  
    6.  Sur le **configurer le serveur RADIUS** page Entrez le mot de passe utilisé pour l’utilisateur administrateur pour le **Secrets** et **mot de passe principal**. Entrez l’ID d’utilisateur administrateur et le mot de passe, puis cliquez sur **configurer**.  
  
    7.  Vérifiez que le message **'serveur RADIUS a été configuré'** s’affiche. Cliquez sur **Terminé**. Fermer le **RSA Operations Console**.  
  
    8.  Revenez à la **« Console de sécurité RSA »**.  
  
    9. Sur le **RADIUS** onglet, cliquez sur **serveurs RADIUS**. Vérifiez que rsa.corp.contoso.com est répertorié.  
  
9. Configurer le serveur RSA en tant que Client d’authentification RSA.  
  
    1.  Sur le **RADIUS** , cliquez sur **Clients RADIUS** et **Ajouter nouveau**.  
  
    2.  Cliquez sur le **ANY Client RADIUS** case à cocher.  
  
    3.  Tapez un mot de passe de votre choix dans le **Secret partagé** champ. Vous allez utiliser ultérieurement ce même mot de passe lors de la configuration EDGE1 pour OTP.  
  
    4.  Laissez le **adresse IP** champ vide et le **rendre / modèle** entrée en tant que **RADIUS Standard**.  
  
    5.  Cliquez sur **enregistrer sans Agent RSA**.  
  
10. Créer des fichiers requis pour la configuration EDGE1 comme un Agent d’authentification RSA.  
  
    1.  Sur le **accès** onglet, mettez en surbrillance **Agents d’authentification**, puis cliquez sur **Ajouter nouveau**.  
  
    2.  Type **EDGE1** dans le **nom d’hôte** champ, puis cliquez sur **IP résoudre**.  
  
    3.  Notez que l’adresse IP pour EDGE1 est maintenant affichée dans le **adresse IP** champ. Cliquez sur **Enregistrer**.  
  
11. Générer un fichier de configuration pour le serveur de EDGE1 (AM_Config.zip).  
  
    1.  Sur le **accès** onglet, mettez en surbrillance **Agents d’authentification**, puis cliquez sur **générer un fichier de Configuration**.  
  
    2.  Sur le **générer un fichier de Configuration** page, cliquez sur **générer un fichier de configuration**, puis cliquez sur **Télécharger maintenant**.  
  
    3.  Cliquez sur **enregistrer**, accédez à C:\ Installation de RSA, puis cliquez sur **enregistrer**.  
  
    4.  Cliquez sur **fermer** sur le **téléchargement terminé** boîte de dialogue.  
  
12. Générer un fichier de secret de nœud pour le serveur de EDGE1 (EDGE1_NodeSecret.zip).  
  
    1.  Sur le **accès** onglet, mettez en surbrillance **Agents d’authentification**, puis cliquez sur **gérer existant**.  
  
    2.  Cliquez sur le nœud actuel configuré EDGE1, puis cliquez sur **gérer le Secret nœud**.  
  
    3.  Vérifier le **créer un nouveau secret aléatoire de nœud et exporter le secret de nœud dans un fichier** case à cocher.  
  
    4.  Entrez le mot de passe utilisé pour l’utilisateur administrateur dans le **mot de passe de chiffrement** et **confirmer le mot de passe chiffrement** champs, puis cliquez sur **enregistrer**.  
  
    5.  Sur le **nœud : fichier de Secret généré** page, cliquez sur **Télécharger maintenant**.  
  
    6.  Sur le **téléchargement de fichier** dialogue, cliquez sur **enregistrer**, accédez à l’Installation de C:\RSA, puis cliquez sur **enregistrer**. Cliquez sur **fermer** sur le **téléchargement terminé** boîte de dialogue.  
  
    7.  À partir du Gestionnaire d’authentification RSA media copie \auth_mgr\windows-x86_64\am\rsa-ace_nsload\win32-5.0-x86\agent_nsload.exe à C:\RSA Installation.  
  
## <a name="BKMK_DAProbeUser"></a>Créer DAProbeUser  
  
1.  Dans le **RSA Security Console** cliquez sur le **identité** , cliquez sur **utilisateurs**, puis cliquez sur **Ajouter nouveau**.  
  
2.  Dans le **nom :** section type **sonde**, puis, dans le **ID d’utilisateur :** section type **DAProbeUser**. Dans le **mot de passe :** et **confirmer le mot de passe :** sections tapez un mot de passe fort. Effacer la **« Exiger l’utilisateur à modifier le mot de passe à la prochaine ouverture de session »** case à cocher et cliquez sur **enregistrer**.  
  
## <a name="InstToken"></a>Installer le jeton de logiciel RSA SecurID sur CLIENT1  
Utilisez cette procédure pour installer le jeton de logiciel SecurID sur CLIENT1.  
  
#### <a name="install-securid-software-token"></a>Installer le jeton de logiciel SecurID  
  
1.  Sur l’ordinateur CLIENT1, créez le dossier C:\RSA fichiers. Copiez le fichier Software_Tokens.zip C:\RSA Installation sur l’ordinateur RSA pour les fichiers C:\RSA. Extrayez le fichier User1_000031701832.SDTID aux fichiers C:\RSA sur CLIENT1.  
  
2.  Accéder à la source de jeton de support logiciel RSA SecurID et double-cliquez sur RSASECURIDTOKEN410 dans le **SecurID SoftwareToken client application** dossier pour démarrer l’installation de RSA SecurID. Si le **ouvrir un fichier - Avertissement de sécurité** message s’affiche, puis cliquez sur **exécuter**.  
  
3.  Sur le **logiciel RSA SecurID - Assistant InstallShield** dialogue, cliquez sur **suivant** à deux reprises.  
  
4.  Acceptez le contrat de licence, puis cliquez sur **suivant**.  
  
5.  Sur le **le Type d’installation** boîte de dialogue Sélectionnez **standard**, cliquez sur **suivant**, puis cliquez sur **installer**.  
  
6.  Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
7.  Sélectionnez le **lancer logiciel RSA SecurID** case à cocher, puis cliquez sur **Terminer**.  
  
8.  Cliquez sur **importer à partir du fichier**.  
  
9. Cliquez sur **Parcourir**, C:\RSA Files\User1_000031701832.SDTID, puis cliquez sur **Open**.  
  
10. Cliquez deux fois sur **OK**.  
  
## <a name="configAuthAgt"></a>Configurer EDGE1 comme un Agent d’authentification RSA  
Utilisez cette procédure pour configurer EDGE1 pour effectuer l’authentification RSA.  
  
#### <a name="configure-the-rsa-authentication-agent"></a>Configurer l’Agent d’authentification RSA  
  
1.  Sur EDGE1 ouvrez l’Explorateur Windows et créez le dossier C:\RSA fichiers. Accédez au support d’Installation d’ACE RSA.  
  
2.  Copie les fichiers agent_nsload.exe, AM_Config.zip et EDGE1_NodeSecret.zip à partir du support RSA pour les fichiers C:\RSA.  
  
3.  Extrayez le contenu des fichiers zip aux emplacements suivants :  
  
    1.  C:\Windows\system32\  
  
    2.  C:\Windows\SysWOW64\  
  
4.  Copiez agent_nsload.exe à C:\Windows\SysWOW64\\.  
  
5.  Ouvrez une invite de commandes avec élévation de privilèges et accédez à C:\Windows\SysWOW64.  
  
6.  Type **agent_nsload.exe -f nodesecret.rec -p <password>**  où <password> est le mot de passe fort que vous avez créé lors de la configuration initiale de RSA. Appuyez sur Entrée.  
  
7.  Copy C:\Windows\SysWOW64\securid to C:\Windows\System32.  
  
## <a name="configOTP"></a>Configurer EDGE1 à prise en charge l’authentification OTP  
Utilisez cette procédure pour configurer OTP pour DirectAccess et vérifiez la configuration.  
  
#### <a name="configure-otp-for-directaccess"></a>Configurer OTP pour DirectAccess  
  
1.  Sur EDGE1, ouvrez le Gestionnaire de serveur, puis cliquez sur **accès à distance** dans le volet gauche.  
  
2.  Avec le bouton droit **EDGE1** dans le volet de serveurs, puis sélectionnez **gestion de l’accès à distance**.  
  
3.  Cliquez sur **Configuration**.  
  
4.  Dans le **le programme d’installation de DirectAccess** fenêtre, sous **étape 2 : serveur d’accès à distance**, cliquez sur **modifier**.  
  
5.  Cliquez sur **suivant** trois fois, puis, dans le **authentification** section Sélectionnez **authentification à deux facteurs** et **utilisez OTP**et vérifiez que **Utilisent des certificats d’ordinateur** est activée. Vérifiez que l’autorité de certification racine est définie sur **CN = corp-APP1-CA**. Cliquez sur **Suivant**.  
  
6.  Dans le **OTP RADIUS Server** section, double-cliquez sur l’essai à blanc **nom du serveur** champ.  
  
7.  Dans le **ajouter un serveur RADIUS** boîte de dialogue, tapez **RSA** dans le **nom du serveur** champ. Cliquez sur **modification** à côté du **secret partagé** champ, puis tapez le mot de passe que vous avez utilisé lors de la configuration des clients RADIUS sur le serveur RSA dans le **nouveau secret** et  **Confirmer le nouveau secret** champs. Cliquez sur **OK** à deux reprises, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > Si le serveur RADIUS est dans un domaine qui est différent de celle du serveur d’accès à distance, puis le **nom du serveur** le champ doit spécifier le nom de domaine complet du serveur RADIUS.  
  
8.  Dans le **serveurs d’autorité de certification OTP** section APP1.corp.contoso.com sélectionnez, puis cliquez sur **ajouter**. Cliquez sur **Suivant**.  
  
9. Sur le **les modèles de certificats OTP** page, cliquez sur **Parcourir** pour sélectionner un modèle de certificat utilisé pour l’inscription des certificats émis pour l’authentification OTP et, sur le  **Les modèles de certificats** sélectionnez boîte de dialogue **DAOTPLogon**. Cliquez sur **OK**. Cliquez sur **Parcourir** pour sélectionner un modèle de certificat utilisé pour inscrire le certificat utilisé par le serveur d’accès à distance pour les demandes de connexion OTP certificat d’inscription et sur le **modèles de certificats** boîte de dialogue Sélectionnez **DAOTPRA**. Cliquez sur **OK**. Cliquez sur **Suivant**.  
  
10. Sur le **le programme d’installation du serveur d’accès à distance** page, cliquez sur **Terminer**, puis cliquez sur **Terminer** sur le **DirectAccess Expert Assistant**.  
  
11. Sur le **révision d’accès à distance** boîte dialogue, cliquez sur **appliquer**, attendez que la stratégie de DirectAccess à mettre à jour, puis cliquez sur **fermer**.  
  
12. Sur le **Démarrer** , tapez**powershell.exe**, avec le bouton droit **powershell**, cliquez sur **avancé**, puis cliquez sur **d’identification administrateur**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
13. Dans la fenêtre Windows PowerShell, tapez **gpupdate /force** et appuyez sur ENTRÉE.  
  
14. Fermez et rouvrez la Console de gestion d’accès à distance et vérifiez que tous les paramètres de secret à usage unique sont corrects.  
  


