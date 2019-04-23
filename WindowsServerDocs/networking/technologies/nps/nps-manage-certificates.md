---
title: Gérer les certificats utilisés avec les serveurs NPS
description: Cette rubrique fournit des informations sur l’utilisation de certificats de serveur avec le serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73f3d6a1e9dc6ae1520b1d685b6b05b5f3aed601
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864230"
---
# <a name="manage-certificates-used-with-nps"></a>Gérer les certificats utilisés avec les serveurs NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Si vous déployez une méthode de l’authentification basée sur certificat, comme protocole d’authentification Extensible\-Transport Layer Security \(EAP\-TLS\), Protected Extensible Authentication Protocol\-Transport Layer Security \(PEAP\-TLS\), PEAP et\-Microsoft Challenge Handshake Authentication Protocol version 2 \(MS\-CHAP v2\), vous devez inscrire un certificat de serveur à l’ensemble de votre NPSs. Le certificat de serveur doit :

- La configuration requise minimale du serveur certificat comme décrit dans [configurer des modèles de certificats pour PEAP et EAP](nps-manage-cert-requirements.md)

- Être émis par une autorité de certification \(autorité de certification\) qui est approuvé par les ordinateurs clients. Une autorité de certification est approuvée lors de son certificat existe dans le magasin de certificats Autorités de Certification racine pour l’utilisateur actuel et l’ordinateur local.

Les instructions suivantes permettent de gérer les certificats de serveur NPS dans les déploiements où l’autorité de certification racine de confiance est une autorité de certification tierce, telle que Verisign, ou est une autorité de certification que vous avez déployé pour votre infrastructure à clé publique \(PKI\) à l’aide d’Active Services de certificats Directory \(AD CS\).

## <a name="change-the-cached-tls-handle-expiry"></a>Modifier l’expiration du Handle mis en cache TLS

Pendant les processus d’authentification initial pour le protocole EAP\-TLS, PEAP\-TLS et PEAP\-MS\-CHAP v2, le serveur NPS met en cache une partie des propriétés de connexion du client de la connexion TLS. Le client met également en cache une partie des propriétés de connexion du serveur NPS TLS.

Chaque collection de ces propriétés de connexion TLS est appelée un handle TLS.

Les ordinateurs clients peuvent mettre en cache les descripteurs TLS de plusieurs authentificateurs, tandis que NPSs peut mettre en cache les handles TLS de nombreux ordinateurs clients.

Les handles TLS mis en cache sur le client et le serveur permettent au processus de la réauthentification de se produire plus rapidement. Par exemple, lorsqu’un ordinateur sans fil se réauthentifie à un serveur NPS, le serveur NPS peut examiner le handle TLS pour le client sans fil et peut déterminer rapidement que la connexion cliente est une reconnexion. Le serveur NPS autorise la connexion sans effectuer l’authentification complète.

En conséquence, le client examine le handle TLS pour le serveur NPS, détermine qu’il est une reconnexion et que vous n’avez pas besoin effectuer l’authentification du serveur.

Sur les ordinateurs exécutant Windows 10 et Windows Server 2016, l’expiration du handle TLS par défaut est de 10 heures.

Dans certains cas, vous souhaiterez augmenter ou diminuer le délai d’expiration du handle TLS.

Par exemple, vous souhaiterez peut-être réduire le délai d’expiration de handle TLS dans des circonstances où les certificats d’un utilisateur sont révoqué par un administrateur et que le certificat a expiré. Dans ce scénario, l’utilisateur peut toujours se connecter au réseau si un serveur NPS est mis en cache à un handle TLS qui n’a pas expiré. En réduisant le TLS handle expiration peut-être aider à empêcher ces utilisateurs avec des certificats révoqués de se reconnecter.

>[!NOTE]
>La meilleure solution à ce scénario consiste à désactiver le compte d’utilisateur dans Active Directory, ou pour supprimer le compte d’utilisateur dans le groupe Active Directory qui est autorisé à se connecter au réseau de la stratégie de réseau. La propagation de ces modifications sur tous les contrôleurs de domaine peut également être retardée, toutefois, en raison de la latence de réplication. 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>Configurer le délai d’expiration du Handle TLS sur les ordinateurs clients

Vous pouvez utiliser cette procédure pour modifier la quantité de temps que les ordinateurs clients mettent en cache le handle TLS d’un serveur NPS. Après avoir authentifié un serveur NPS, les ordinateurs clients mettent en cache les propriétés de connexion TLS de serveur NPS en tant que TLS handle. Le handle TLS a une durée par défaut de 10 heures \(36,000,000 millisecondes\). Vous pouvez augmenter ou diminuer le délai d’expiration du handle TLS à l’aide de la procédure suivante.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

>[!IMPORTANT]
>Cette procédure doit être effectuée sur un serveur NPS, pas sur un ordinateur client.

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>Pour configurer le TLS gérer le délai d’expiration sur les ordinateurs clients

1. Sur un serveur NPS, ouvrez l’Éditeur du Registre.

2. Accédez à la clé de Registre **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Sur le **modifier** menu, cliquez sur **New**, puis cliquez sur **clé**.

4. Type **ClientCacheTime**, puis appuyez sur ENTRÉE.

5. Avec le bouton droit **ClientCacheTime**, cliquez sur **New**, puis cliquez sur **valeur DWORD (32-bit)**.

6. Tapez la quantité de temps, en millisecondes, que vous souhaitez mettre en cache le handle TLS d’un serveur NPS après la première authentification réussie tentative par le serveur NPS, les ordinateurs clients.

## <a name="configure-the-tls-handle-expiry-time-on-npss"></a>Configurer le délai d’expiration du Handle TLS sur NPSs

Utilisez cette procédure pour modifier la quantité de temps que NPSs cache le handle du TLS des ordinateurs clients. Après avoir authentifié un client d’accès, NPSs mettre en cache les propriétés de connexion TLS de l’ordinateur client en tant que TLS handle. Le handle TLS a une durée par défaut de 10 heures \(36,000,000 millisecondes\). Vous pouvez augmenter ou diminuer le délai d’expiration du handle TLS à l’aide de la procédure suivante.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

>[!IMPORTANT]
>Cette procédure doit être effectuée sur un serveur NPS, pas sur un ordinateur client.

### <a name="to-configure-the-tls-handle-expiry-time-on-npss"></a>Pour configurer le TLS gérer heure d’expiration sur NPSs

1. Sur un serveur NPS, ouvrez l’Éditeur du Registre.

2. Accédez à la clé de Registre **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Sur le **modifier** menu, cliquez sur **New**, puis cliquez sur **clé**.

4. Type **ServerCacheTime**, puis appuyez sur ENTRÉE.

5. Avec le bouton droit **ServerCacheTime**, cliquez sur **New**, puis cliquez sur **valeur DWORD (32-bit)**.

6. Tapez la quantité de temps, en millisecondes, que vous souhaitez NPSs pour mettre en cache le handle TLS d’un ordinateur client après la première authentification réussie tentative par le client.

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Obtenir le hachage SHA-1 d’un certificat d’autorité de certification racine approuvée

Utilisez cette procédure pour obtenir l’algorithme de hachage sécurisé (SHA-1) le hachage d’une autorité de certification (CA) racine approuvée à partir d’un certificat est installé sur l’ordinateur local. Dans certains cas, comme lors du déploiement de stratégie de groupe, il est nécessaire de désigner un certificat à l’aide du hachage SHA-1 du certificat.

Lorsque vous utilisez la stratégie de groupe, vous pouvez désigner un ou plusieurs certificats d’autorité de certification racine de confiance que les clients doivent utiliser pour authentifier le serveur NPS pendant le processus d’authentification mutuelle avec EAP ou PEAP. Pour désigner un certificat d’autorité de certification racine de confiance que les clients doivent utiliser pour valider le certificat de serveur, vous pouvez entrer le hachage SHA-1 du certificat.

Cette procédure montre comment obtenir le SHA-1 hachage d’un certificat d’autorité de certification racine approuvée à l’aide du composant logiciel enfichable certificats Microsoft Management Console (MMC). 

Pour effectuer cette procédure, vous devez être un membre de la **utilisateurs** groupe sur l’ordinateur local.

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Pour obtenir le SHA-1 hachage d’un certificat d’autorité de certification racine de confiance

1. Dans la boîte de dialogue Exécuter ou Windows PowerShell, tapez **mmc**, puis appuyez sur ENTRÉE. La Console de gestion Microsoft \(MMC\) s’ouvre. Dans la console MMC, cliquez sur **fichier**, puis cliquez sur **Ajout/Suppression Snap\in**. La boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** s'ouvre.

2. Dans **Ajouter ou supprimer des composants logiciels enfichables**, dans **Composants logiciels enfichables disponibles**, double-cliquez sur **Certificats**. L’Assistant du composant logiciel enfichable Certificats s’ouvre. Cliquez sur **Un compte d'ordinateur**, puis cliquez sur **Suivant**.

3. Dans **sélectionner un ordinateur**, vérifiez que **ordinateur Local (l’ordinateur sur cette console s’exécute)** est sélectionnée, cliquez sur **Terminer**, puis cliquez sur **OK**.

4. Dans le volet gauche, double-cliquez sur **certificats (ordinateur Local)**, puis double-cliquez sur le **Trusted Root Certification Authorities** dossier.

5. Le **certificats** dossier est un sous-dossier de la **Trusted Root Certification Authorities** dossier. Cliquez sur le dossier **Certificats**.

6. Dans le volet de détails, accédez au certificat d’autorité de certification racine approuvée. Double-cliquez sur le certificat. La boîte de dialogue **Certificat** s'ouvre.

7. Dans la boîte de dialogue **Certificat**, cliquez sur l'onglet **Détails**.

8. Dans la liste des champs, faites défiler jusqu'à et sélectionnez **empreinte**.

9. Dans le volet inférieur, la chaîne hexadécimale qui représente le hachage SHA-1 de votre certificat s'affiche. Sélectionnez le hachage SHA-1, puis appuyez sur le raccourci clavier Windows pour la commande de copie \(CTRL\+C\) pour copier le hachage dans le Presse-papiers Windows.

10. Ouvrir l’emplacement auquel vous souhaitez coller le hachage SHA-1, localiser correctement le curseur, puis appuyez sur le raccourci clavier Windows pour la commande Coller \(CTRL\+V\). 

Pour plus d’informations sur les certificats et de NPS, consultez [configurer des modèles de certificats pour PEAP et EAP exigences](nps-manage-cert-requirements.md).

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
