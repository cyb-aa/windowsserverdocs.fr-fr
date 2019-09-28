---
title: Résolution des problèmes d’activation du mot de passe à usage unique
description: Cette rubrique fait partie du guide déployer l’accès à distance avec l’authentification par mot de passe à usage unique dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b58252ca-4c1d-4664-a3c4-7301e2121517
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a1c18f264a6a8d263f3e9f50bc325ef97f4240af
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366916"
---
# <a name="troubleshooting-enabling-otp"></a>Résolution des problèmes d’activation du mot de passe à usage unique

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique contient des informations de dépannage pour les problèmes liés à l’activation de l’authentification par mot de passe à usage unique DirectAccess avec l’applet de commande PowerShell **Enable-DAOtpAuthentication** ou la console de gestion de l’accès à distance.
  
## <a name="failed-to-enroll-the-otp-signing-certificate"></a>Échec de l’inscription du certificat de signature avec mot de passe à usage unique  
**Erreur reçue** (Journal des événements du serveur). Un certificat de signature avec mot de passe à usage unique ne peut pas être inscrit à l’aide du modèle de certificat < OTP_signing_template_name >  
  
**Cause**  
  
Il existe trois causes possibles pour cette erreur :  
  
-   Le modèle n’existe pas.  
  
-   Les autorisations définies sur le modèle n’autorisent pas l’inscription du serveur DirectAccess.  
  
-   Il n’existe aucune connectivité réseau à l’autorité de certification émettrice.  
  
**Solution**  
  
1.  Assurez-vous que le modèle de certificat de signature avec mot de passe à usage unique avec le nom donné :  
  
    1.  Existe et dispose des autorisations appropriées.  
  
    2.  Est configuré pour être émis par au moins une autorité de certification qui peut émettre des certificats pour le serveur DirectAccess.  
  
2.  Si le modèle n’existe pas, créez-le comme décrit dans 3,3 planifier le certificat de l’autorité d’inscription, ou si un autre modèle de correspondance existe, reconfigurez le mot de passe à usage unique DirectAccess avec le nouveau nom de modèle.  
  
## <a name="failed-to-enable-directaccess-otp-when-webdav-is-installed"></a>Échec de l’activation du mot de passe à usage unique DirectAccess lors de l’installation de WebDAV  
**Scénario**. Lors de la tentative d’application de la configuration de mot de passe à usage unique DirectAccess dans la console de gestion de l’accès à distance ou à l’aide de l’applet de commande PowerShell `Enable-DAOtpAuthentication`, l’opération échoue.  
  
**Erreur reçue** (Journal des événements du serveur). Impossible d’appliquer les paramètres de mot de passe à usage unique DirectAccess, car l’extension IIS WebDAV est en cours d’exécution sur le serveur. Supprimez WebDAV et réappliquez les paramètres.  
  
**Cause**  
  
Le service de mot de passe à usage unique DirectAccess n’est pas compatible avec la fonctionnalité de publication WebDAV et ne peut pas être activé pendant l’installation de WebDAV.  
  
**Solution**  
  
Désinstallez le rôle WebDAV :  
  
1.  Dans la console Gestionnaire de serveur, dans le volet gauche, cliquez sur **IIS**.  
  
2.  Dans le volet principal, accédez à **rôles et fonctionnalités**.  
  
3.  Cliquez avec le bouton droit sur **publication WebDAV**, puis cliquez sur **supprimer un rôle ou une fonctionnalité**.  
  
4.  Terminez l’Assistant Suppression de rôles et de fonctionnalités.  
  
5.  Appliquez à nouveau la configuration du mot de passe à usage unique DirectAccess.  
  
## <a name="no-templates-available-in-the-remote-access-management-console"></a>Aucun modèle disponible dans la console de gestion de l’accès à distance  
**Scénario**. Lors de la configuration des modèles de certificat OTP ou autorité d’inscription à l’aide de la console de gestion de l’accès à distance, certains ou tous les modèles sont manquants dans les fenêtres de sélection.  
  
**Cause**  
  
Il existe deux causes possibles pour cette erreur :  
  
-   Le modèle n’est pas configuré conformément aux exigences de mot de passe à usage unique DirectAccess et ne peut donc pas être sélectionné.  
  
-   Les autorités de certification sélectionnées sous les **serveurs d’autorité de certification à usage unique** ne sont pas configurées pour émettre les modèles requis.  
  
**Solution**  
  
1.  Vérifiez que le modèle de connexion par mot de passe à usage unique et le modèle de certificat de signature avec mot de passe à usage unique sont correctement configurés, comme décrit dans 3,2 planifier le modèle de certificat OTP et 3,3 planifier le certificat d’autorité d’inscription.  
  
2.  Vérifiez que les autorités de certification configurées dans la liste des **serveurs d’autorité de certification avec mot de passe à usage unique** sont configurées pour émettre les modèles appropriés :  
  
    1.  Sur le serveur de l’autorité de certification, ouvrez la console autorité de certification.  
  
    2.  Dans le volet gauche, développez le serveur de l’autorité de certification choisi.  
  
    3.  Cliquez sur **modèles de certificats** et assurez-vous que les modèles requis sont activés. Si ce n’est pas le cas, cliquez avec le bouton droit sur **modèles de certificats**, cliquez sur **nouveau**, cliquez sur **modèle de certificat à délivrer**, puis sélectionnez les modèles que vous souhaitez activer.  
  
## <a name="cannot-set-renewal-period-of-otp-template-to-1-hour"></a>Impossible de définir la période de renouvellement du modèle de mot de passe à usage unique sur 1 heure  
**Scénario**. Lors de la configuration du modèle de connexion à mot de passe à usage unique DirectAccess à l’aide de l’autorité de certification Windows 2003, il n’est pas possible de définir la période de renouvellement du modèle sur 1 heure.  
  
**Cause**  
  
Le composant logiciel enfichable MMC modèles de certificats dans Windows Server 2003 ne vous permet pas de définir la période de renouvellement d’un modèle sur 1 heure.  
  
**Solution**  
  
Installez le composant logiciel enfichable modèles de certificats sur un serveur postérieur à Windows Server 2003 et utilisez-le pour configurer le modèle d’ouverture de session par mot de passe à usage unique. consultez [installer le composant logiciel enfichable modèles de certificats](https://technet.microsoft.com/library/cc732445.aspx).  
  


