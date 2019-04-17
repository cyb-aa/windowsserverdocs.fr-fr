---
title: Gérer des certificats utilisés avec le serveur NPS
description: Cette rubrique fournit des informations sur l’utilisation de certificats de serveur avec le serveur NPS dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2044cf30cc90c1673e05a1948ac9196d05940d1f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-certificates-used-with-nps"></a>Gérer des certificats utilisés avec le serveur NPS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Si vous déployez une méthode d’authentification par certificat, tels que \(EAP\-TLS\) Extensible Authentication Protocol\-Transport Layer Security, Protected Extensible Authentication Protocol\-Transport Layer Security \(PEAP\-TLS\) et PEAP\-MicrosoftChallenge Handshake Authentication Protocol version2 \ (v2\ MS\-CHAP), vous devez inscrire un certificat de serveur à l’ensemble de vos serveurs NPS. Le certificat de serveur doit:

- Respecter les exigences de certificat de serveur minimale, comme décrit dans [configurer des modèles de certificats pour PEAP et EAP exigences](nps-manage-cert-requirements.md)

- Être émis par une autorité de certification \(CA\) qui est approuvé par les ordinateurs clients. Une autorité de certification est approuvée lors de son certificat existe dans le magasin de certificats Autorités de Certification racine pour l’utilisateur actuel et l’ordinateur local.

Les instructions suivantes vous aider à la gestion des certificats de serveur NPS dans les déploiements où l’autorité de certification racine de confiance est une autorité de certification tierce, telle que Verisign, ou est une autorité de certification que vous avez déployé pour votre infrastructure à clé publique \(PKI\) à l’aide des Services de certificats ActiveDirectory \(ADCS\).

## <a name="change-the-cached-tls-handle-expiry"></a>Modifier l’expiration du Handle mis en cache TLS

Pendant les processus d’authentification initiale pour EAP\-TLS, PEAP\-TLS et PEAP\-MS\-CHAP v2, le serveur NPS met en cache une partie des propriétés de connexion du client de la connexion TLS. Le client met également en cache une partie des propriétés de la connexion TLS du serveur NPS.

Chaque collection de ces propriétés de connexion TLS est appelée un handle TLS.

Les ordinateurs clients peuvent mettre en cache les descripteurs TLS de plusieurs authentificateurs, tandis que les serveurs NPS peuvent mettre en cache les descripteurs TLS de nombreux ordinateurs clients.

Les poignées de TLS mises en cache sur le serveur et client permettent le processus se produire plus rapidement de réauthentification. Par exemple, lorsqu’un ordinateur sans fil se réauthentifie à avec un serveur NPS, le serveur NPS peut examiner le handle TLS pour le client sans fil et peut déterminer rapidement que la connexion cliente est une reconnexion. Le serveur NPS autorise la connexion sans effectuer l’authentification complète.

En conséquence, le client examine le handle TLS pour le serveur NPS, détermine qu’il est une reconnexion et il est inutile d’effectuer l’authentification du serveur.

Sur les ordinateurs exécutant Windows10 et Windows Server2016, l’expiration de handle TLS par défaut est de 10heures.

Dans certaines circonstances, vous souhaiterez peut-être augmenter ou réduire le délai d’expiration handle TLS.

Par exemple, vous souhaiterez peut-être afin de réduire le délai d’expiration handle TLS dans les cas où un certificat de l’utilisateur est révoqué par un administrateur et que le certificat a expiré. Dans ce scénario, l’utilisateur peut toujours vous connecter au réseau si un serveur NPS possède un handle mis en cache TLS qui n’a pas expiré. Ce qui réduit le protocole TLS handle expiration peut-être vous aider à empêcher ces utilisateurs avec les certificats révoqués de se reconnecter.

>[!NOTE]
>La meilleure solution à ce scénario consiste à désactiver le compte d’utilisateur dans ActiveDirectory, ou pour supprimer le compte d’utilisateur du groupe ActiveDirectory qui est accordé l’autorisation de se connecter au réseau de la stratégie de réseau. La propagation de ces modifications sur tous les contrôleurs de domaine peut également être retardée, toutefois, en raison de la latence de réplication. 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>Configurez le délai d’expiration TLS Handle sur les ordinateurs clients

Vous pouvez utiliser cette procédure pour modifier la quantité de temps que les ordinateurs clients mettent en cache le handle TLS d’un serveur NPS. Après une authentification réussie un serveur NPS, les ordinateurs clients mettent en cache des propriétés de connexion TLS du serveur NPS en tant qu’un descripteur TLS. Le handle TLS a une durée par défaut de 10heures \(36,000,000milliseconds\). Vous pouvez augmenter ou diminuer le délai d’expiration handle TLS à l’aide de la procédure suivante.

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

>[!IMPORTANT]
>Cette procédure doit être effectuée sur un serveur NPS, pas sur un ordinateur client.

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>Pour configurer le protocole TLS gérer le délai d’expiration sur les ordinateurs clients

1. Sur un serveur NPS, ouvrez l’Éditeur du Registre.

2. Accédez à la clé de Registre **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Sur le **modifier** menu, cliquez sur **New**, puis cliquez sur **clé**.

4. Type **ClientCacheTime**, puis appuyez sur ENTRÉE.

5. Avec le bouton droit **ClientCacheTime**, cliquez sur **New**, puis cliquez sur **valeur DWORD (32bits)**.

6. Tapez la durée, en millisecondes, que vous souhaitez les ordinateurs clients de mettre en cache le handle TLS d’un serveur NPS après la première authentification réussie tentative par le serveur NPS.

## <a name="configure-the-tls-handle-expiry-time-on-nps-servers"></a>Configurer le délai d’expiration TLS Handle sur les serveurs NPS

Utilisez cette procédure pour modifier la quantité de temps que les serveurs NPS cache le handle TLS des ordinateurs clients. Après une authentification réussie un client d’accès, serveurs NPS cache des propriétés de la connexion TLS de l’ordinateur client en tant qu’un handle TLS. Le handle TLS a une durée par défaut de 10heures \(36,000,000milliseconds\). Vous pouvez augmenter ou diminuer le délai d’expiration handle TLS à l’aide de la procédure suivante.

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

>[!IMPORTANT]
>Cette procédure doit être effectuée sur un serveur NPS, pas sur un ordinateur client.

### <a name="to-configure-the-tls-handle-expiry-time-on-nps-servers"></a>Pour configurer le protocole TLS gérer le délai d’expiration sur les serveurs NPS

1. Sur un serveur NPS, ouvrez l’Éditeur du Registre.

2. Accédez à la clé de Registre **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Sur le **modifier** menu, cliquez sur **New**, puis cliquez sur **clé**.

4. Type **ServerCacheTime**, puis appuyez sur ENTRÉE.

5. Avec le bouton droit **ServerCacheTime**, cliquez sur **New**, puis cliquez sur **valeur DWORD (32bits)**.

6. Tapez la durée, en millisecondes, que vous voulez mettre en cache le handle TLS d’un ordinateur client après l’authentification réussie première tentative par le client, les serveurs NPS.

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Obtenir le SHA-1 hachage d’un certificat d’autorité de certification racine approuvée

Utilisez cette procédure pour obtenir l’algorithme de hachage sécurisé (SHA-1) le hachage d’une autorité de certification racine de confiance (CA) à partir d’un certificat qui est installé sur l’ordinateur local. Dans certains cas, comme lors du déploiement de stratégie de groupe, il est nécessaire de désigner un certificat à l’aide du hachage SHA-1 du certificat.

Lorsque vous utilisez la stratégie de groupe, vous pouvez désigner un ou plusieurs certificats d’autorité de certification racines de confiance que les clients doivent utiliser afin d’authentifier le serveur NPS au cours du processus d’authentification mutuelle avec EAP ou PEAP. Pour désigner un certificat d’autorité de certification racine de confiance que les clients doivent utiliser pour valider le certificat de serveur, vous pouvez entrer le hachage SHA-1 du certificat.

Cette procédure illustre comment obtenir le hachage SHA-1 d’un certificat d’autorité de certification racine de confiance à l’aide du composant logiciel enfichable MicrosoftManagement Console (MMC) de certificats. 

Pour effectuer cette procédure, vous devez être un membre de la **utilisateurs** groupe sur l’ordinateur local.

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Pour obtenir le SHA-1 hachage d’un certificat d’autorité de certification racine de confiance

1. Dans la boîte de dialogue Exécuter ou de Windows PowerShell, tapez **mmc**, puis appuyez sur ENTRÉE. La Console de gestion Microsoft \(MMC\) s’ouvre. Dans la console MMC, cliquez sur **fichier**, puis cliquez sur **ajouter/supprimer Snap\in**. Le **ajouter ou supprimer des composants logiciel enfichables** boîte de dialogue s’ouvre.

2. Dans **ajouter ou supprimer des composants logiciel enfichables**, dans **des composants logiciels enfichables disponibles**, double-cliquez sur **certificats**. L’Assistant de composant logiciel enfichable Certificats s’ouvre. Cliquez sur **compte d’ordinateur**, puis cliquez sur **suivant**.

3. Dans **sélectionner un ordinateur**, assurez-vous que **ordinateur Local (l’ordinateur sur cette console s’exécute)** est sélectionné, cliquez sur **Terminer**, puis cliquez sur **OK**.

4. Dans le volet gauche, double-cliquez sur **certificats (ordinateur Local)**, puis double-cliquez sur le **Trusted Root Certification Authorities** dossier.

5. Le **certificats** dossier est un sous-dossier du **Trusted Root Certification Authorities** dossier. Cliquez sur le **certificats** dossier.

6. Dans le volet d’informations, recherchez le certificat pour votre autorité de certification racine approuvée. Double-cliquez sur le certificat. Le **certificat** boîte de dialogue s’ouvre.

7. Dans le **certificat** boîte de dialogue, cliquez sur le **détails** onglet.

8. Dans la liste des champs, faites défiler jusqu'à et sélectionnez **empreinte numérique**.

9. Dans le volet inférieur, la chaîne hexadécimale qui représente le hachage SHA-1 de votre certificat s’affiche. Sélectionnez le SHA-1 hachage et appuyez sur le raccourci clavier Windows pour la copie de la commande \(CTRL\+C\) pour copier le hachage dans le Presse-papiers Windows.

10. Ouvrez l’emplacement vers lequel vous voulez coller le hachage SHA-1, localiser correctement le curseur et appuyez sur le raccourci clavier Windows pour le collage de commandes \(CTRL\+V\). 

Pour plus d’informations sur les certificats et NPS, voir [configurer des modèles de certificats pour PEAP et EAP exigences](nps-manage-cert-requirements.md).

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).
