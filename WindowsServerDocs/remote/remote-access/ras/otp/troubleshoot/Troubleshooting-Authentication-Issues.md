---
title: Résolution des problèmes d’authentification
description: Cette rubrique fait partie du guide déployer l’accès à distance avec l’authentification par mot de passe à usage unique dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71307757-f8f4-4f82-b8b3-ffd4fd8c5d6d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73fe8458910cbe7dfaf000a6546bcba9263a9683
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404310"
---
# <a name="troubleshooting-authentication-issues"></a>Résolution des problèmes d’authentification

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique contient des informations de dépannage pour les problèmes liés aux problèmes que les utilisateurs peuvent rencontrer lors de la tentative de connexion à DirectAccess avec l’authentification par mot de passe à usage unique. Les événements associés au mot de passe à usage unique DirectAccerss sont enregistrés sur l’ordinateur client dans observateur d’événements sous **journaux des applications et des services/Microsoft/Windows/OtpCredentialProvider**. Assurez-vous que ce journal est activé lors de la résolution des problèmes liés au mot de passe à usage unique DirectAccess.  
  
## <a name="failed-to-access-the-ca-that-issues-otp-certificates"></a>Échec de l’accès à l’autorité de certification qui émet des certificats OTP  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (Journal des événements du client). L’inscription de certificat OTP pour l’utilisateur <username> a échoué sur le serveur d’autorité de certification < CA_name >, échec de la demande, raisons possibles de l’échec : Impossible de résoudre le nom du serveur de l’autorité de certification, le serveur de l’autorité de certification n’est pas accessible via le premier tunnel DirectAccess ou la connexion au serveur de l’autorité de certification ne peut pas être établie.  
  
**Cause**  
  
L’utilisateur a fourni un mot de passe à usage unique valide et le serveur DirectAccess a signé la demande de certificat ; Toutefois, l’ordinateur client ne peut pas contacter l’autorité de certification qui émet des certificats avec mot de passe à usage unique pour terminer le processus d’inscription.  
  
**Solution**  
  
Sur le serveur DirectAccess, exécutez les commandes Windows PowerShell suivantes :  
  
1.  Obtient la liste des autorités de certification émettrices avec mot de passe à usage unique configurées et vérifie la valeur de « caserver » : `Get-DAOtpAuthentication`  
  
2.  Assurez-vous que les autorités de certification sont configurées en tant que serveurs d’administration : `Get-DAMgmtServer -Type All`  
  
3.  Assurez-vous que l’ordinateur client a établi le tunnel d’infrastructure : Dans la console pare-feu Windows avec fonctions avancées de sécurité, développez **associations de surveillance/sécurité**, cliquez sur **mode principal**, et assurez-vous que les associations de sécurité IPSec apparaissent avec les adresses distantes correctes pour votre DirectAccess. configuré.  
  
## <a name="directaccess-server-connectivity-issues"></a>Problèmes de connectivité du serveur DirectAccess  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (Journal des événements du client)  
  
L’une des erreurs suivantes :  
  
-   Impossible d’établir une connexion au serveur d’accès à distance < DirectAccess_server_hostname > à l’aide du chemin de base < OTP_authentication_path > et du port < OTP_authentication_port >. Code d’erreur : < > internal_error_code.  
  
-   Les informations d’identification de l’utilisateur ne peuvent pas être envoyées au serveur d’accès à distance < DirectAccess_server_hostname > à l’aide du chemin de base < OTP_authentication_path > et port < OTP_authentication_port >. Code d’erreur : < > internal_error_code.  
  
-   Aucune réponse n’a été reçue de la part du serveur d’accès à distance < DirectAccess_server_hostname > à l’aide du chemin de base < OTP_authentication_path > et du port < OTP_authentication_port >. Code d’erreur : < > internal_error_code.  
  
**Cause**  
  
L’ordinateur client ne peut pas accéder au serveur DirectAccess via Internet, en raison de problèmes réseau ou d’un serveur IIS mal configuré sur le serveur DirectAccess.  
  
**Solution**  
  
Assurez-vous que la connexion Internet sur l’ordinateur client fonctionne, et assurez-vous que le service DirectAccess est en cours d’exécution et qu’il est accessible via Internet.  
  
## <a name="failed-to-enroll-for-the-directaccess-otp-logon-certificate"></a>Échec de l’inscription pour le certificat d’ouverture de session à mot de passe à usage unique DirectAccess  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (Journal des événements du client). Échec de l’inscription de certificat auprès de l’autorité de certification < CA_name >. La demande n’a pas été signée comme prévu par le certificat de signature avec mot de passe à usage unique, ou l’utilisateur n’est pas autorisé à s’inscrire.  
  
**Cause**  
  
Le mot de passe à usage unique fourni par l’utilisateur est correct, mais l’autorité de certification émettrice a refusé d’émettre le certificat d’ouverture de session par mot de passe à usage unique. La demande de certificat n’est peut-être pas correctement signée avec la stratégie d’application de l’autorité d’inscription EKU (OTP) appropriée, ou l’utilisateur ne dispose pas de l’autorisation d’inscription sur le modèle de mot de passe à usage unique DA.  
  
**Solution**  
  
Assurez-vous que les utilisateurs de mot de passe à usage unique DirectAccess ont l’autorisation de s’inscrire pour le certificat de connexion à mot de passe à usage unique DirectAccess et que la « stratégie d’application » appropriée est incluse dans le modèle de signature de l’autorité d’inscription Assurez-vous également que le certificat de l’autorité d’inscription DirectAccess sur le serveur d’accès à distance est valide. Consultez 3,2 planifier le modèle de certificat avec mot de passe à usage unique et 3,3 planifier le certificat d’autorité d’inscription.  
  
## <a name="missing-or-invalid-computer-account-certificate"></a>Certificat de compte d’ordinateur manquant ou non valide  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (Journal des événements du client).  Impossible d’effectuer l’authentification par mot de passe à usage unique car le certificat d’ordinateur requis pour OTP est introuvable dans le magasin de certificats de l’ordinateur local.  
  
**Cause**  
  
L’authentification par mot de passe à usage unique DirectAccess requiert un certificat d’ordinateur client pour établir une connexion SSL avec le serveur DirectAccess. Toutefois, le certificat de l’ordinateur client est introuvable ou n’est pas valide, par exemple, si le certificat a expiré.  
  
**Solution**  
  
Assurez-vous que le certificat d’ordinateur existe et qu’il est valide :  
  
1.  Sur l’ordinateur client, dans la console certificats MMC, pour le compte d’ordinateur local, ouvrez **personnel/certificats**.  
  
2.  Assurez-vous qu’il existe un certificat émis qui correspond au nom de l’ordinateur et double-cliquez sur le certificat.  
  
3.  Dans la boîte de dialogue **certificat** , sous l’onglet **chemin d’accès** du certificat, sous **État du certificat**, vérifiez que le message « ce certificat est OK » s’affiche.  
  
Si un certificat valide est introuvable, supprimez le certificat non valide (s’il existe) et réinscrivez-le pour le certificat d’ordinateur en exécutant `gpupdate /Force` à partir d’une invite de commandes avec élévation de privilèges ou en redémarrant l’ordinateur client.  
  
## <a name="missing-ca-that-issues-otp-certificates"></a>Autorité de certification manquante qui émet des certificats OTP  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (Journal des événements du client). L’authentification par mot de passe à usage unique ne peut pas être effectuée car le serveur DA n’a pas retourné d’adresse d’autorité de certification émettrice.  
  
**Cause**  
  
Soit il n’y a aucune autorité de certification qui émet des certificats à usage unique configurés, soit toutes les autorités de certification configurées qui émettent des certificats à usage unique ne répondent pas.  
  
**Solution**  
  
1.  Utilisez la commande suivante pour récupérer la liste des autorités de certification qui émettent des certificats de mot de passe à usage unique (le nom de l’autorité de certification est indiqué dans caserver) : `Get-DAOtpAuthentication`.  
  
2.  Si aucune autorité de certification n’est configurée :  
  
    1.  Utilisez la commande `Set-DAOtpAuthentication` ou la console de gestion de l’accès à distance pour configurer les autorités de certification qui émettent le certificat de connexion à mot de passe à usage unique DirectAccess.  
  
    2.  Appliquez la nouvelle configuration et forcez les clients à actualiser les paramètres de l’objet de stratégie de groupe DirectAccess en exécutant `gpupdate /Force` à partir d’une invite de commandes avec élévation de privilèges ou en redémarrant l’ordinateur client.  
  
3.  Si des autorités de certification sont configurées, assurez-vous qu’elles sont en ligne et répondent aux demandes d’inscription.  
  
## <a name="misconfigured-directaccess-server-address"></a>Adresse du serveur DirectAccess mal configurée  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (Journal des événements du client). L’authentification OTP ne peut pas se terminer comme prévu. Impossible de déterminer le nom ou l’adresse du serveur d’accès à distance.  Code d’erreur : < Code_erreur >. Les paramètres DirectAccess doivent être validés par l’administrateur du serveur.  
  
**Cause**  
  
L’adresse du serveur DirectAccess n’est pas configurée correctement.  
  
**Solution**  
  
Vérifiez l’adresse du serveur DirectAccess configurée à l’aide de `Get-DirectAccess` et corrigez l’adresse si elle est mal configurée.  
  
Assurez-vous que les derniers paramètres sont déployés sur l’ordinateur client en exécutant `gpupdate /force` à partir d’une invite de commandes avec élévation de privilèges ou redémarrez l’ordinateur client.  
  
## <a name="failed-to-generate-the-otp-logon-certificate-request"></a>Échec de la génération de la demande de certificat d’ouverture de session à usage unique  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (Journal des événements du client). La demande de certificat pour l’authentification par mot de passe à usage unique ne peut pas être initialisée. Une clé privée ne peut pas être générée ou l’utilisateur <username> ne peut pas accéder au modèle de certificat < OTP_template_name > sur le contrôleur de domaine.  
  
**Cause**  
  
Il existe deux causes possibles pour cette erreur :  
  
-   L’utilisateur n’est pas autorisé à lire le modèle de connexion par mot de passe à usage unique.  
  
-   L’ordinateur de l’utilisateur ne peut pas accéder au contrôleur de domaine en raison de problèmes réseau.  
  
**Solution**  
  
-   Examinez le paramètre autorisations sur le modèle de connexion par mot de passe à usage unique et assurez-vous que tous les utilisateurs configurés pour le mot de passe à usage unique DirectAccess ont l’autorisation « lecture ».  
  
-   Assurez-vous que le contrôleur de domaine est configuré en tant que serveur d’administration et que l’ordinateur client peut atteindre le contrôleur de domaine via le tunnel d’infrastructure. Consultez 3,2 planifier le modèle de certificat avec mot de passe à usage unique.  
  
## <a name="no-connection-to-the-domain-controller"></a>Aucune connexion au contrôleur de domaine  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (Journal des événements du client). Impossible d’établir une connexion avec le contrôleur de domaine pour l’authentification par mot de passe à usage unique. Code d’erreur : < Code_erreur >.  
  
**Cause**  
  
Il existe deux causes possibles pour cette erreur :  
  
-   L’ordinateur de l’utilisateur n’a pas de connectivité réseau.  
  
-   Le contrôleur de domaine n’est pas accessible via le tunnel d’infrastructure.  
  
**Solution**  
  
-   Assurez-vous que le contrôleur de domaine est configuré en tant que serveur d’administration en exécutant la commande suivante à partir d’une invite PowerShell : `Get-DAMgmtServer -Type All`.  
  
-   Assurez-vous que l’ordinateur client peut atteindre le contrôleur de domaine via le tunnel d’infrastructure.  
  
## <a name="otp-provider-requires-challengeresponse"></a>Le fournisseur OTP requiert une stimulation/réponse  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (Journal des événements du client). L’authentification par mot de passe à usage unique avec le serveur d’accès à distance (< DirectAccess_server_name >) pour l’utilisateur (<username>) nécessitait un défi de la part de l’utilisateur.  
  
**Cause**  
  
Le fournisseur OTP utilisé exige que l’utilisateur fournisse des informations d’identification supplémentaires sous la forme d’un échange de stimulation/réponse RADIUS, qui n’est pas pris en charge par le mot de passe à usage unique DirectAccess de Windows Server 2012.  
  
**Solution**  
  
Configurez le fournisseur de mot de passe à usage unique pour ne pas exiger de stimulation/réponse dans aucun scénario.  
  
## <a name="incorrect-otp-logon-template-used"></a>Modèle d’ouverture de session à usage unique incorrect utilisé  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (Journal des événements du client). Modèle d’autorité de certification à partir duquel l’utilisateur <username> a demandé qu’un certificat ne soit pas configuré pour émettre des certificats à usage unique.  
  
**Cause**  
  
Le modèle de connexion à mot de passe à usage unique a été remplacé et l’ordinateur client tente de s’authentifier à l’aide d’un modèle plus ancien.  
  
**Solution**  
  
Assurez-vous que l’ordinateur client utilise la dernière configuration de mot de passe à usage unique en effectuant l’une des opérations suivantes :  
  
-   Forcez une mise à jour stratégie de groupe en exécutant la commande suivante à partir d’une invite de commandes avec élévation de privilèges : `gpupdate /Force`.  
  
-   Redémarrez l’ordinateur client.  
  
## <a name="missing-otp-signing-certificate"></a>Certificat de signature avec mot de passe à usage unique manquant  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (Journal des événements du client). Impossible de trouver un certificat de signature avec mot de passe à usage unique. Impossible de signer la demande d’inscription de certificat par mot de passe à usage unique.  
  
**Cause**  
  
Le certificat de signature avec mot de passe à usage unique DirectAccess est introuvable sur le serveur d’accès à distance ; par conséquent, la demande de certificat utilisateur ne peut pas être signée par le serveur d’accès à distance. Soit il n’existe aucun certificat de signature, soit le certificat de signature a expiré et n’a pas été renouvelé.  
  
**Solution**  
  
Procédez comme suit sur le serveur d’accès à distance.  
  
1.  Vérifiez le nom du modèle de certificat de signature avec mot de passe à usage unique configuré en exécutant l’applet de commande PowerShell `Get-DAOtpAuthentication` et inspectez la valeur de `SigningCertificateTemplateName`.  
  
2.  Utilisez le composant logiciel enfichable MMC Certificats pour vous assurer qu’un certificat valide inscrit à partir de ce modèle existe sur l’ordinateur.  
  
3.  Si aucun certificat n’existe, supprimez le certificat expiré (s’il en existe un) et inscrivez-vous pour un nouveau certificat basé sur ce modèle.  
  
Pour créer le modèle de certificat de signature avec mot de passe à usage unique, consultez 3,3 planifier le certificat d’autorité d’inscription.  
  
## <a name="missing-or-incorrect-upndn-for-the-user"></a>UPN/DN manquant ou incorrect pour l’utilisateur  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (Journal des événements du client)  
  
L’une des erreurs suivantes :  
  
-   L’utilisateur <username> ne peut pas être authentifié avec un mot de passe à usage unique. Assurez-vous qu’un UPN est défini pour le nom d’utilisateur dans Active Directory. Code d’erreur : < Code_erreur >.  
  
-   L’utilisateur <username> ne peut pas être authentifié avec un mot de passe à usage unique. Assurez-vous qu’un DN est défini pour le nom d’utilisateur dans Active Directory. Code d’erreur : < Code_erreur >.  
  
**Erreur reçue** (Journal des événements du serveur)  
  
Le nom d’utilisateur <username> spécifié pour l’authentification par mot de passe à usage unique n’existe pas.  
  
**Cause**  
  
L’utilisateur n’a pas les attributs UPN (nom d’utilisateur principal) ou nom unique (DN) définis correctement dans le compte d’utilisateur, ces propriétés sont requises pour le bon fonctionnement du mot de passe à usage unique DirectAccess.  
  
**Solution**  
  
Utilisez la console utilisateurs et ordinateurs Active Directory sur le contrôleur de domaine pour vérifier que ces deux attributs sont correctement définis pour l’utilisateur d’authentification.  
  
## <a name="otp-certificate-is-not-trusted-for-login"></a>Le certificat OTP n’est pas approuvé pour la connexion  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Cause**  
  
L’autorité de certification qui émet les certificats de mot de passe à usage unique ne se trouve pas dans le magasin Enterprise NTAuth. par conséquent, les certificats inscrits ne peuvent pas être utilisés pour l’ouverture de session. Cela peut se produire dans les environnements à plusieurs domaines et forêts où l’approbation inter-domaines n’est pas établie.  
  
**Solution**  
  
Assurez-vous que le certificat de la racine de la hiérarchie d’autorité de certification qui émet des certificats de mot de passe à usage unique est installé dans le magasin de certificats d’entreprise NTAuth du domaine auquel l’utilisateur tente de s’authentifier.  
  
## <a name="windows-could-not-verify-user-credentials"></a>Windows n’a pas pu vérifier les informations d’identification de l’utilisateur  
**Scénario**. L’utilisateur ne peut pas s’authentifier avec le mot de passe à usage unique avec l’erreur : « Échec de l’authentification en raison d’une erreur interne »  
  
**Erreur reçue** (ordinateur client). Une erreur s’est produite lors de la vérification de vos informations d’identification par Windows. Réessayez ou demandez de l’aide à votre administrateur.  
  
**Cause**  
  
Le protocole d’authentification Kerberos ne fonctionne pas quand le certificat de connexion à mot de passe à usage unique DirectAccess n’inclut pas de liste de révocation de certificats. Le certificat de connexion à mot de passe à usage unique DirectAccess n’inclut pas de liste de révocation des certificats :  
  
-   Le modèle de connexion par mot de passe à usage unique DirectAccess a été configuré avec l’option **ne pas inclure les informations de révocation dans les certificats émis**.  
  
-   L’autorité de certification est configurée pour ne pas publier les listes de révocation de certificats.  
  
**Solution**  
  
1.  Pour confirmer la cause de cette erreur, dans la console Gestion de l’accès à distance, dans **étape 2 serveur d’accès à distance**, cliquez sur **modifier**, puis dans l’Assistant **installation du serveur d’accès à distance** , cliquez sur **modèles de certificats à usage unique**. Prenez note du modèle de certificat utilisé pour l’inscription des certificats émis pour l’authentification par mot de passe à usage unique. Ouvrez la console autorité de certification, dans le volet gauche, cliquez sur **modèles de certificats**, double-cliquez sur le certificat d’ouverture de session par mot de passe à usage unique pour afficher les propriétés du modèle de certificat.  
  
    Pour résoudre ce problème, configurez un certificat pour le certificat d’ouverture de session par mot de passe à usage unique et n’activez pas la case à cocher **ne pas inclure les informations de révocation dans les certificats délivrés** sous l’onglet **serveur** de la boîte de dialogue Propriétés du modèle.  
  
2.  Sur le serveur de l’autorité de certification, ouvrez la console MMC Autorité de certification, cliquez avec le bouton droit sur l’autorité de certification émettrice, puis cliquez sur **Propriétés**. Sous l’onglet **Extensions** , assurez-vous que la publication des listes de révocation de certificats est correctement configurée.  
  


