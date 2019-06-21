---
title: Résolution des problèmes d’authentification
description: Cette rubrique fait partie du guide de déploiement des accès à distance avec authentification OTP dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71307757-f8f4-4f82-b8b3-ffd4fd8c5d6d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 08dd6822cc30135506d82041cfbeab0bc1a058ab
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282354"
---
# <a name="troubleshooting-authentication-issues"></a>Résolution des problèmes d’authentification

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique contient des informations de dépannage des problèmes liés aux problèmes que les utilisateurs devront peut-être lorsque vous tentez de vous connecter à DirectAccess à l’aide de l’authentification unique. DirectAccerss OTP liés événements sont enregistrés sur l’ordinateur client dans l’Observateur d’événements sous **Applications et les journaux de Services/Microsoft/Windows/OtpCredentialProvider**. Assurez-vous que ce journal est activé lors de la résolution des problèmes avec DirectAccess OTP.  
  
## <a name="failed-to-access-the-ca-that-issues-otp-certificates"></a>Échec d’accès de l’autorité de certification qui émet des certificats de secret à usage unique  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (journal des événements du client). L’inscription de certificats OTP pour l’utilisateur <username> a échoué sur le serveur d’autorité de certification < NOM_AUTORITÉ_DE_CERTIFICATION >, demander les raisons ayant échoués, possibles de l’échec : Nom de serveur d’autorité de certification ne peut pas être résolu, serveur d’autorité de certification n’est pas accessible via le premier tunnel DirectAccess ou la connexion au serveur d’autorité de certification ne peut pas être établie.  
  
**Cause**  
  
L’utilisateur a fourni un mot de passe à usage unique et le serveur DirectAccess connecté à la demande de certificat ; Toutefois, l’ordinateur client ne peut pas contacter l’autorité de certification qui émet des certificats OTP pour terminer le processus d’inscription.  
  
**Solution**  
  
Sur le serveur DirectAccess, exécutez les commandes Windows PowerShell suivantes :  
  
1.  Obtenir la liste des OTP configuré AC émettrices et vérifiez la valeur de « ServeurCA » : `Get-DAOtpAuthentication`  
  
2.  Assurez-vous que les autorités de certification sont configurés en tant que serveurs d’administration : `Get-DAMgmtServer -Type All`  
  
3.  Assurez-vous que l’ordinateur client a établi le tunnel d’infrastructure : Dans le pare-feu Windows avec la console de fonctions avancées de sécurité, développez **Associations de sécurité/surveillance**, cliquez sur **Mode principal**et vous assurer que les associations de sécurité IPsec s’affichent à la distance correct adresses de votre configuration de DirectAccess.  
  
## <a name="directaccess-server-connectivity-issues"></a>Problèmes de connectivité du serveur DirectAccess  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (journal des événements du client)  
  
Une des erreurs suivantes :  
  
-   Impossible d’établir une connexion au serveur d’accès à distance < DirectAccess_server_hostname > à l’aide du chemin d’accès de base < OTP_authentication_path > et < OTP_authentication_port > de port. Code d’erreur : < internal_error_code >.  
  
-   Informations d’identification de l’utilisateur ne peut pas être envoyées au serveur d’accès à distance < DirectAccess_server_hostname > à l’aide du chemin d’accès de base < OTP_authentication_path > et < OTP_authentication_port > de port. Code d’erreur : < internal_error_code >.  
  
-   Une réponse a été pas reçue à partir du serveur d’accès à distance < DirectAccess_server_hostname > à l’aide du chemin d’accès de base < OTP_authentication_path > et < OTP_authentication_port > de port. Code d’erreur : < internal_error_code >.  
  
**Cause**  
  
L’ordinateur client ne peut pas accéder au serveur DirectAccess via Internet, en raison des problèmes de réseau ou à un serveur IIS mal configuré sur le serveur DirectAccess.  
  
**Solution**  
  
Vérifiez que l’utilisation de la connexion Internet sur l’ordinateur client et vous assurer que le service DirectAccess est en cours d’exécution et accessible via Internet.  
  
## <a name="failed-to-enroll-for-the-directaccess-otp-logon-certificate"></a>Échec de l’inscription pour obtenir le certificat d’ouverture de session DirectAccess OTP  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (journal des événements du client). Échec de l’inscription de certificats d’autorité de certification < NOM_AUTORITÉ_DE_CERTIFICATION >. La demande n’a pas été signée comme prévu par le certificat de signature du secret à usage unique, ou l’utilisateur n’est pas autorisé à inscrire.  
  
**Cause**  
  
Le mot de passe à usage unique fourni par l’utilisateur est correcte, mais a refusé de l’autorité de certification (CA) émettrice émettre le certificat d’ouverture de session à usage. La demande de certificat n’est pas correctement signée avec l’EKU correcte (stratégie application de l’autorité d’inscription OTP), ou l’utilisateur ne dispose pas de l’autorisation « Enroll » sur le modèle à usage DA.  
  
**Solution**  
  
Assurez-vous que les utilisateurs DirectAccess OTP sont autorisés à inscrire pour le certificat d’ouverture de session de DirectAccess OTP et que la stratégie d’Application « approprié » est incluse dans l’autorité d’inscription de DA OTP modèle de signature. Assurez-vous également que le certificat d’autorité d’inscription DirectAccess sur le serveur d’accès à distance est valide. Consultez le modèle de certificat OTP du Plan de 3.2 et 3.3 Plan le certificat d’autorité d’inscription.  
  
## <a name="missing-or-invalid-computer-account-certificate"></a>Certificat du compte d’ordinateur manquant ou non valide  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (journal des événements du client).  Impossible d’effectuer l’authentification OTP car le certificat d’ordinateur requis pour OTP est introuvable dans le magasin de certificats ordinateur local.  
  
**Cause**  
  
L’authentification DirectAccess OTP nécessite un certificat d’ordinateur client pour établir une connexion SSL avec le serveur DirectAccess ; Toutefois, le certificat d’ordinateur client est introuvable ou n’est pas valide, par exemple, si le certificat a expiré.  
  
**Solution**  
  
Assurez-vous que le certificat d’ordinateur existe et qu’il n’est valide :  
  
1.  Sur l’ordinateur client, dans la console MMC Certificats, pour le compte d’ordinateur Local, ouvrez **personnel/certificats**.  
  
2.  Assurez-vous qu’il existe un certificat émis qui correspond à celui de l’ordinateur et double-cliquez sur le certificat.  
  
3.  Sur le **certificat** boîte de dialogue le **chemin d’accès du certificat** sous l’onglet sous **état du certificat**, assurez-vous qu’elle indique « ce certificat est OK ».  
  
Si aucun certificat valide n’est pas trouvé, supprimer le certificat non valide (le cas échéant) et le nouveau s’inscrire pour le certificat d’ordinateur par soit en cours d’exécution `gpupdate /Force` à partir d’une invite de commandes avec élévation de privilèges ou le redémarrage de l’ordinateur client.  
  
## <a name="missing-ca-that-issues-otp-certificates"></a>Manque d’autorité de certification qui émet des certificats de secret à usage unique  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (journal des événements du client). Impossible d’effectuer l’authentification OTP car le serveur DA n’a pas retourné d’une adresse d’une autorité de certification émettrice.  
  
**Cause**  
  
Il n’y a aucune autorité de certification qui émettent des certificats OTP configurés, soit toutes les autorités de certification configurée qui émettent des certificats de secret à usage unique sont ne répond pas.  
  
**Solution**  
  
1.  Utilisez la commande suivante pour obtenir la liste des autorités de certification qui émettent des certificats OTP (le nom de l’autorité de certification est indiqué dans ServeurCA) : `Get-DAOtpAuthentication`.  
  
2.  Si aucune autorité de certification n’est configurés :  
  
    1.  Utilisez la commande `Set-DAOtpAuthentication` ou la console de gestion de l’accès à distance pour configurer les autorités de certification qui émettent des certificats d’ouverture de session de l’OTP DirectAccess.  
  
    2.  Appliquer la nouvelle configuration et forcer les clients pour actualiser les paramètres de stratégie de groupe DirectAccess en exécutant `gpupdate /Force` à partir d’une invite de commandes avec élévation de privilèges ou le redémarrage de l’ordinateur client.  
  
3.  S’il existe des autorités de certification configurée, assurez-vous qu’ils sont en ligne et répondre aux demandes d’inscription.  
  
## <a name="misconfigured-directaccess-server-address"></a>Adresse du serveur DirectAccess mal configuré  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (journal des événements du client). Impossible de terminer l’authentification OTP comme prévu. Le nom ou l’adresse du serveur d’accès à distance ne peut pas être déterminé.  Code d’erreur : < error_code >. Les paramètres DirectAccess doivent être validés par l’administrateur du serveur.  
  
**Cause**  
  
L’adresse du serveur DirectAccess n’est pas configuré correctement.  
  
**Solution**  
  
Vérification de la configuration DirectAccess server adresse à l’aide `Get-DirectAccess` et corrigez l’adresse si elle est mal configuré.  
  
Assurez-vous que les derniers paramètres sont déployés sur l’ordinateur client en exécutant `gpupdate /force` à partir d’une invite de commandes avec élévation de privilèges ou le redémarrage de l’ordinateur client.  
  
## <a name="failed-to-generate-the-otp-logon-certificate-request"></a>Échec de génération de la demande de certificat d’ouverture de session OTP  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (journal des événements du client). La demande de certificat pour l’authentification OTP ne peut pas être initialisée. Soit une clé privée ne peut pas être générée, ou utilisateur <username> ne peut pas accéder de modèle de certificat < OTP_template_name > sur le contrôleur de domaine.  
  
**Cause**  
  
Il existe deux causes possibles de cette erreur :  
  
-   L’utilisateur n’est pas autorisé à lire le modèle d’ouverture de session de secret à usage unique.  
  
-   Ordinateur de l’utilisateur ne peut pas accéder au contrôleur de domaine en raison de problèmes de réseau.  
  
**Solution**  
  
-   Passez en revue les autorisations sur le modèle d’ouverture de session OTP et assurez-vous que tous les utilisateurs configurés pour DirectAccess OTP ont « autorisation de lecture ».  
  
-   Assurez-vous que le contrôleur de domaine est configuré comme un serveur d’administration et que l’ordinateur client peut atteindre le contrôleur de domaine via le tunnel d’infrastructure. Consultez le Plan de 3,2 le modèle de certificat à usage.  
  
## <a name="no-connection-to-the-domain-controller"></a>Aucune connexion au contrôleur de domaine  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (journal des événements du client). Impossible d’établir une connexion avec le contrôleur de domaine à des fins d’authentification OTP. Code d’erreur : < error_code >.  
  
**Cause**  
  
Il existe deux causes possibles de cette erreur :  
  
-   L’ordinateur de l’utilisateur aucune connectivité réseau.  
  
-   Le contrôleur de domaine n’est pas accessible via le tunnel d’infrastructure.  
  
**Solution**  
  
-   Assurez-vous que le contrôleur de domaine est configuré comme un serveur d’administration en exécutant la commande suivante à partir d’une invite PowerShell : `Get-DAMgmtServer -Type All`.  
  
-   Assurez-vous que l’ordinateur client peut atteindre le contrôleur de domaine via le tunnel d’infrastructure.  
  
## <a name="otp-provider-requires-challengeresponse"></a>Fournisseur de secret à usage unique requiert la stimulation/réponse  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (journal des événements du client). Authentification OTP avec le serveur d’accès à distance (< DirectAccess_server_name >) pour l’utilisateur (<username>) une étape à partir de l’utilisateur est requise.  
  
**Cause**  
  
Le fournisseur de secret à usage unique utilisé oblige l’utilisateur à fournir des informations d’identification supplémentaires sous la forme d’un échange de stimulation/réponse RADIUS, qui n’est pas pris en charge par Windows Server 2012 DirectAccess OTP.  
  
**Solution**  
  
Configurer le fournisseur OTP pour ne pas exiger de stimulation/réponse dans n’importe quel scénario.  
  
## <a name="incorrect-otp-logon-template-used"></a>Modèle de d’ouverture de session incorrecte OTP utilisé  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (journal des événements du client). Le modèle de l’autorité de certification à partir de l’utilisateur qui <username> demandé un certificat n’est pas configuré pour émettre des certificats de secret à usage unique.  
  
**Cause**  
  
Le modèle de connexion DirectAccess OTP a été remplacé et l’ordinateur client tente de s’authentifier à l’aide d’un modèle plus ancien.  
  
**Solution**  
  
Vérifiez que l’ordinateur client est à l’aide de la dernière configuration OTP en effectuant l’une des opérations suivantes :  
  
-   Forcer une mise à jour de la stratégie de groupe en exécutant la commande suivante à partir d’une invite de commandes avec élévation de privilèges : `gpupdate /Force`.  
  
-   Redémarrez l’ordinateur client.  
  
## <a name="missing-otp-signing-certificate"></a>Manquant OTP certificat de signature  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (journal des événements du client). Impossible de trouver un certificat de signature du secret à usage unique. La demande d’inscription de certificat secret à usage unique ne peut pas être signée.  
  
**Cause**  
  
Impossible de trouver le certificat de signature de DirectAccess OTP sur le serveur d’accès à distance ; Par conséquent, la demande de certificat utilisateur ne peut pas être signée par le serveur d’accès à distance. Il n’existe aucun certificat de signature, soit le certificat de signature a expiré et n’a pas été renouvelé.  
  
**Solution**  
  
Effectuez ces étapes sur le serveur d’accès à distance.  
  
1.  Vérifier le configuré OTP signature modèle nom de certificat en exécutant l’applet de commande PowerShell `Get-DAOtpAuthentication` et inspecter la valeur de `SigningCertificateTemplateName`.  
  
2.  Utilisez le composant logiciel enfichable MMC Certificats pour vous assurer qu’un certificat valide est inscrit à partir de ce modèle existe sur l’ordinateur.  
  
3.  Si aucun certificat n’existe, supprimez le certificat arrivé à expiration (le cas échéant) et s’inscrire pour un nouveau certificat basé sur ce modèle.  
  
Pour créer la signature du secret à usage unique modèle de certificat Voir Plan 3.3 le certificat d’autorité d’inscription.  
  
## <a name="missing-or-incorrect-upndn-for-the-user"></a>Manquante ou incorrecte UPN/nom de domaine de l’utilisateur  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (journal des événements du client)  
  
Une des erreurs suivantes :  
  
-   Utilisateur <username> ne peut pas être authentifié avec l’OTP. Assurez-vous qu’un nom UPN est défini pour le nom d’utilisateur dans Active Directory. Code d’erreur : < error_code >.  
  
-   Utilisateur <username> ne peut pas être authentifié avec l’OTP. Assurez-vous qu’un nom unique est défini pour le nom d’utilisateur dans Active Directory. Code d’erreur : < error_code >.  
  
**Erreur reçue** (journal des événements serveur)  
  
Le nom d’utilisateur <username> spécifié pour l’authentification OTP n’existe pas.  
  
**Cause**  
  
L’utilisateur n’a pas le nom Principal utilisateur (UPN) ou les attributs de nom unique (DN) est correctement défini dans le compte d’utilisateur, ces propriétés sont requises pour le bon fonctionnement de DirectAccess OTP.  
  
**Solution**  
  
Utilisez la console Active Directory Users and Computers sur le contrôleur de domaine pour vérifier que ces deux attributs sont correctement définis pour l’authentification de l’utilisateur.  
  
## <a name="otp-certificate-is-not-trusted-for-login"></a>Certificat de secret à usage unique n’est pas approuvé pour la connexion  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Cause**  
  
L’autorité de certification qui émet des certificats de secret à usage unique n’est pas dans le magasin NTAuth d’entreprise ; Par conséquent, les certificats inscrits ne peut pas être utilisés pour la connexion. Cela peut se produire dans plusieurs environnements domaine et dans un environnement multi-forêt où entre domaines de confiance de l’autorité de certification n’est pas établie.  
  
**Solution**  
  
Assurez-vous que le certificat de la racine de la hiérarchie d’autorité de certification qui émet des certificats de l’OTP est installé dans l’entreprise NTAuth certificat stocker du domaine auquel l’utilisateur tente de s’authentifier.  
  
## <a name="windows-could-not-verify-user-credentials"></a>Windows n’a pas pu vérifier les informations d’identification de l’utilisateur  
**Scénario**. Utilisateur ne parvient pas à s’authentifier à l’aide de secret à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (ordinateur Client). Quelque chose s’est produite pendant que Windows a été vérifier vos informations d’identification. Réessayez, ou demandez à votre administrateur de l’aide.  
  
**Cause**  
  
Le protocole d’authentification Kerberos ne fonctionne pas lorsque le certificat d’ouverture de session de DirectAccess OTP n’inclut pas d’une liste de révocation. Le certificat d’ouverture de session de DirectAccess OTP n’inclut pas une liste de révocation, car soit :  
  
-   Le modèle de connexion DirectAccess OTP a été configuré avec l’option **n’incluent pas les informations de révocation de certificats émis**.  
  
-   L’autorité de certification est configurée pour ne pas pour publier les CRL.  
  
**Solution**  
  
1.  Pour confirmer la cause de cette erreur, dans la console de gestion de l’accès à distance, dans **étape 2 : serveur accès à distance**, cliquez sur **modifier**, puis, dans le **le programme d’installation du serveur d’accès à distance** Assistant, cliquez sur **les modèles de certificats OTP**. Prenez note du modèle de certificat utilisé pour l’inscription des certificats émis pour l’authentification unique. Ouvrez la console Autorité de Certification, dans le volet gauche, cliquez sur **modèles de certificats**, double-cliquez sur le certificat d’ouverture de session OTP pour afficher les propriétés de modèle de certificat.  
  
    Pour résoudre ce problème, configurez un certificat pour le certificat d’ouverture de session OTP et ne sélectionnez pas le **n’incluent pas les informations de révocation de certificats émis** case à cocher sur la **Server** onglet du modèle boîte de dialogue de propriétés.  
  
2.  Sur le serveur d’autorité de certification, ouvrez le MMC Autorité de Certification, cliquez avec le bouton droit sur l’autorité de certification émettrice et cliquez sur **propriétés**. Sur le **Extensions** onglet vous assurer que la publication CRL est correctement configurée.  
  


