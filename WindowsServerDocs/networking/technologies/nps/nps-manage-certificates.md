---
title: Gérer les certificats utilisés avec les serveurs NPS
description: Cette rubrique fournit des informations sur l’utilisation des certificats de serveur avec le serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b83f68b52a9cceef779e5204e295bbc9e45e7a14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396195"
---
# <a name="manage-certificates-used-with-nps"></a>Gérer les certificats utilisés avec les serveurs NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Si vous déployez une méthode d’authentification basée sur les certificats, telle que le protocole EAP (Extensible Authentication Protocol)\-Transport Layer Security \(EAP\-TLS\), Protected Extensible Authentication Protocol\-Transport Layer Security \(PEAP\-TLS\)et PEAP\-Microsoft Challenge Handshake Authentication Protocol version 2 \(MS\-CHAP v2\), vous devez inscrire un certificat de serveur pour tous vos NPSs. Le certificat de serveur doit :

- Respectez la configuration minimale requise pour les certificats de serveur, comme décrit dans [configurer les modèles de certificats pour les exigences PEAP et EAP](nps-manage-cert-requirements.md) .

- Être émis par une autorité de certification \(\) d’autorité de certification approuvée par les ordinateurs clients. Une autorité de certification est approuvée quand son certificat existe dans le magasin de certificats des autorités de certification racines de confiance pour l’utilisateur actuel et l’ordinateur local.

Les instructions suivantes permettent de gérer des certificats NPS dans des déploiements où l’autorité de certification racine de confiance est une autorité de certification tierce, telle que VeriSign, ou une autorité de certification que vous avez déployée pour votre infrastructure de clé publique \(PKI\) à l’aide de Active Directory Services de certificats \(AD CS\).

## <a name="change-the-cached-tls-handle-expiry"></a>Modifier l’expiration du handle TLS mis en cache

Pendant les processus d’authentification initiaux pour EAP\-TLS, PEAP\-TLS et PEAP\-MS\-CHAP v2, le serveur NPS met en cache une partie des propriétés de connexion TLS du client qui se connecte. Le client met également en cache une partie des propriétés de connexion TLS du serveur NPS.

Chaque collection individuelle de ces propriétés de connexion TLS est appelée un descripteur TLS.

Les ordinateurs clients peuvent mettre en cache les descripteurs TLS pour plusieurs authentificateurs, tandis que NPSs peut mettre en cache les descripteurs TLS de nombreux ordinateurs clients.

Les descripteurs TLS mis en cache sur le client et le serveur permettent au processus de réauthentification de se produire plus rapidement. Par exemple, lorsqu’un ordinateur sans fil se réauthentifie auprès d’un serveur NPS, le serveur NPS peut examiner le descripteur TLS du client sans fil et peut rapidement déterminer que la connexion cliente est une reconnexion. Le serveur NPS autorise la connexion sans effectuer l’authentification complète.

En conséquence, le client examine le descripteur TLS pour le serveur NPS, détermine qu’il s’agit d’une reconnexion et n’a pas besoin d’effectuer une authentification du serveur.

Sur les ordinateurs exécutant Windows 10 et Windows Server 2016, l’expiration du descripteur TLS par défaut est de 10 heures.

Dans certains cas, vous souhaiterez peut-être augmenter ou diminuer le délai d’expiration du handle TLS.

Par exemple, vous souhaiterez peut-être réduire le délai d’expiration du handle TLS dans les circonstances où le certificat d’un utilisateur est révoqué par un administrateur et que le certificat a expiré. Dans ce scénario, l’utilisateur peut toujours se connecter au réseau si un serveur NPS possède un descripteur TLS mis en cache qui n’a pas expiré. La réduction de l’expiration du handle TLS peut aider à empêcher de tels utilisateurs avec des certificats révoqués de se reconnecter.

>[!NOTE]
>La meilleure solution pour ce scénario consiste à désactiver le compte d’utilisateur dans Active Directory ou à supprimer le compte d’utilisateur du groupe Active Directory qui est autorisé à se connecter au réseau dans la stratégie réseau. Toutefois, la propagation de ces modifications à tous les contrôleurs de domaine peut également être retardée, en raison de la latence de la réplication. 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>Configurer le délai d’expiration du handle TLS sur les ordinateurs clients

Vous pouvez utiliser cette procédure pour modifier la durée pendant laquelle les ordinateurs clients met en cache le descripteur TLS d’un serveur NPS. Une fois l’authentification d’un serveur NPS effectuée, les ordinateurs clients cachent les propriétés de connexion TLS du serveur NPS en tant que descripteur TLS. Le descripteur TLS a une durée par défaut de 10 heures \(36 millions millisecondes\). Vous pouvez augmenter ou diminuer le temps d’expiration du handle TLS à l’aide de la procédure suivante.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

>[!IMPORTANT]
>Cette procédure doit être effectuée sur un serveur NPS, et non sur un ordinateur client.

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>Pour configurer le délai d’expiration du handle TLS sur les ordinateurs clients

1. Sur un serveur NPS, ouvrez l’éditeur du Registre.

2. Accédez à la clé de Registre **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Dans le menu **Edition** , cliquez sur **nouveau**, puis sur **clé**.

4. Tapez **ClientCacheTime**, puis appuyez sur entrée.

5. Cliquez avec le bouton droit sur **ClientCacheTime**, cliquez sur **nouveau**, puis sur **valeur DWORD (32 bits)** .

6. Tapez la durée, en millisecondes, pendant laquelle les ordinateurs clients doivent mettre en cache le descripteur TLS d’un serveur NPS après la première tentative d’authentification réussie par le serveur NPS.

## <a name="configure-the-tls-handle-expiry-time-on-npss"></a>Configurer le délai d’expiration du handle TLS sur NPSs

Utilisez cette procédure pour modifier la durée pendant laquelle NPSs met en cache le descripteur TLS des ordinateurs clients. Après avoir authentifié avec succès un client Access, NPSs les propriétés de connexion TLS de l’ordinateur client en tant que handle TLS. Le descripteur TLS a une durée par défaut de 10 heures \(36 millions millisecondes\). Vous pouvez augmenter ou diminuer le temps d’expiration du handle TLS à l’aide de la procédure suivante.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

>[!IMPORTANT]
>Cette procédure doit être effectuée sur un serveur NPS, et non sur un ordinateur client.

### <a name="to-configure-the-tls-handle-expiry-time-on-npss"></a>Pour configurer le délai d’expiration du handle TLS sur NPSs

1. Sur un serveur NPS, ouvrez l’éditeur du Registre.

2. Accédez à la clé de Registre **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Dans le menu **Edition** , cliquez sur **nouveau**, puis sur **clé**.

4. Tapez **lorsque supérieure**, puis appuyez sur entrée.

5. Cliquez avec le bouton droit sur **lorsque supérieure**, cliquez sur **nouveau**, puis sur **valeur DWORD (32 bits)** .

6. Tapez la durée, en millisecondes, pendant laquelle vous souhaitez que NPSs met en cache le handle TLS d’un ordinateur client après la première tentative d’authentification réussie par le client.

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Obtenir le hachage SHA-1 d’un certificat d’autorité de certification racine approuvée

Utilisez cette procédure pour obtenir le hachage SHA-1 (Secure Hash Algorithm) d’une autorité de certification racine de confiance à partir d’un certificat installé sur l’ordinateur local. Dans certains cas, par exemple lors du déploiement d’stratégie de groupe, il est nécessaire de désigner un certificat à l’aide du hachage SHA-1 du certificat.

Lorsque vous utilisez stratégie de groupe, vous pouvez désigner un ou plusieurs certificats d’autorité de certification racine de confiance que les clients doivent utiliser pour authentifier le serveur NPS au cours du processus d’authentification mutuelle avec EAP ou PEAP. Pour désigner un certificat d’autorité de certification racine approuvé que les clients doivent utiliser pour valider le certificat de serveur, vous pouvez entrer le hachage SHA-1 du certificat.

Cette procédure montre comment obtenir le hachage SHA-1 d’un certificat d’autorité de certification racine approuvé à l’aide du composant logiciel enfichable Certificats de la console MMC (Microsoft Management Console). 

Pour effectuer cette procédure, vous devez être membre du groupe **utilisateurs** sur l’ordinateur local.

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Pour obtenir le hachage SHA-1 d’un certificat d’autorité de certification racine approuvée

1. Dans la boîte de dialogue Exécuter ou Windows PowerShell, tapez **MMC**, puis appuyez sur entrée. La console MMC (Microsoft Management Console) \(\) s’ouvre. Dans la console MMC, cliquez sur **fichier**, puis sur **Ajouter/supprimer des Snap\in**. La boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** s'ouvre.

2. Dans **Ajouter ou supprimer des composants logiciels enfichables**, dans **Composants logiciels enfichables disponibles**, double-cliquez sur **Certificats**. L’Assistant composant logiciel enfichable Certificats s’ouvre. Cliquez sur **Un compte d'ordinateur**, puis cliquez sur **Suivant**.

3. Dans **Sélectionner un ordinateur**, vérifiez **que l’option ordinateur local (l’ordinateur sur lequel cette console s’exécute)** est sélectionnée, cliquez sur **Terminer**, puis sur **OK**.

4. Dans le volet gauche, double-cliquez sur **certificats (ordinateur local)** , puis double-cliquez sur le dossier **autorités de certification racines de confiance** .

5. Le dossier **certificats** est un sous-dossier du dossier **autorités de certification racines de confiance** . Cliquez sur le dossier **Certificats**.

6. Dans le volet d’informations, accédez au certificat de votre autorité de certification racine de confiance. Double-cliquez sur le certificat. La boîte de dialogue **Certificat** s'ouvre.

7. Dans la boîte de dialogue **Certificat**, cliquez sur l'onglet **Détails**.

8. Dans la liste des champs, faites défiler jusqu’à et sélectionnez **empreinte numérique**.

9. Dans le volet inférieur, la chaîne hexadécimale qui représente le hachage SHA-1 de votre certificat s'affiche. Sélectionnez le hachage SHA-1, puis appuyez sur le raccourci clavier Windows pour la commande copier \(CTRL\+C\) pour copier le hachage dans le presse-papiers Windows.

10. Ouvrez l’emplacement où vous souhaitez coller le hachage SHA-1, localisez le curseur correctement, puis appuyez sur le raccourci clavier Windows pour la commande Coller \(CTRL\+V\). 

Pour plus d’informations sur les certificats et NPS, consultez [configurer des modèles de certificats pour les exigences PEAP et EAP](nps-manage-cert-requirements.md).

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
