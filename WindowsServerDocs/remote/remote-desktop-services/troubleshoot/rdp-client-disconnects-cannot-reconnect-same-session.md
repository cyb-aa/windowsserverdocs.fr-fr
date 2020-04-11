---
title: Le client Bureau à distance se déconnecte et ne peut pas se reconnecter à la même session
description: 'Résolution du problème suivant : le client Bureau à distance se déconnecte et ne peut pas se reconnecter à la même session.'
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 0d116c99b7c8b1daffc4ec58bd93414781eea321
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857202"
---
# <a name="remote-desktop-client-disconnects-and-cant-reconnect-to-the-same-session"></a>Le client Bureau à distance se déconnecte et ne peut pas se reconnecter à la même session

Quand un client Bureau à distance perd sa connexion au Bureau à distance, il ne peut pas se reconnecter immédiatement. L’utilisateur reçoit l’un des messages d’erreur suivants :

  - Le client n’a pas pu se connecter au serveur Terminal Server en raison d’une erreur de sécurité. Vérifiez que vous êtes connecté au réseau, puis réessayez de vous connecter.
  - Bureau à distance déconnecté. Le client n’a pas pu se connecter à l’ordinateur distant en raison d’une erreur de sécurité. Vérifiez que vous êtes connecté au réseau et essayez de vous reconnecter.

Quand le client Bureau à distance se reconnecte, le serveur Hôte de session Bureau à distance (RDSH, Remote Desktop Session Host) reconnecte le client à une nouvelle session et non à la session d’origine. Toutefois, quand vous vérifiez le serveur RDSH, vous voyez qu’il indique que la session d’origine est toujours active et n’est pas passée à un état déconnecté.

Pour contourner ce problème, vous pouvez activer la stratégie **Configurer l’intervalle de conservation des connexions** dans le dossier de stratégie de groupe **Configuration ordinateur\\Modèles d’administration\\Composants Windows\\Services Bureau à distance\\Hôte de session Bureau à distance\\Connexions**. Si vous activez cette stratégie, vous devez entrer un intervalle de conservation. L’intervalle de conservation détermine la fréquence, en minutes, à laquelle le serveur vérifie l’état de session.

Vous pouvez également résoudre ce problème en reconfigurant vos paramètres d’authentification et de configuration. Vous pouvez reconfigurer ces paramètres au niveau du serveur ou à l’aide d’objets de stratégie de groupe. Procédez comme suit pour reconfigurer vos paramètres : Dossier de stratégie de groupe **Configuration ordinateur\\Modèles d’administration\\Composants Windows\\Services Bureau à distance\\Hôte de session Bureau à distance\\Sécurité**.

1. Sur le serveur Hôte de session Bureau à distance, ouvrez **Configuration d’hôte de session Bureau à distance**.
2. Sous **Connexions**, cliquez avec le bouton droit sur le nom de la connexion, puis sélectionnez **Propriétés**.
3. Dans la boîte de dialogue **Propriétés** de la connexion, sous l’onglet **Général**, dans la couche **Sécurité**, sélectionnez une méthode de sécurité.
4. Sous **Niveau de chiffrement**, sélectionnez le niveau souhaité. Vous pouvez sélectionner **Faible**, **Compatible avec le client**, **Élevé** ou **Compatible FIPS**.

> [!NOTE]  
>  - Quand les communications entre les clients et les serveurs Hôtes de session Bureau à distance nécessitent le niveau de chiffrement le plus élevé, utilisez le chiffrement Compatible FIPS.
>  - Les paramètres de niveau de chiffrement que vous configurez dans la stratégie de groupe remplacent ceux que vous avez configurés à l’aide de l’outil Configuration des services Bureau à distance. En outre, si vous activez la stratégie [Chiffrement système : utilisez des algorithmes compatibles FIPS pour le chiffrement, le hachage et la signature](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/system-cryptography-use-fips-compliant-algorithms-for-encryption-hashing-and-signing), ce paramètre remplace la stratégie **Définir le niveau de chiffrement de la connexion client**. La stratégie de chiffrement système se trouve dans le dossier **Configuration ordinateur\\Paramètres Windows\\Paramètres de sécurité\\Stratégies locales\\Options de sécurité**.
>  - Quand vous modifiez le niveau de chiffrement, le nouveau niveau prend effet lors de la prochaine connexion d’un utilisateur. Si vous avez besoin de plusieurs niveaux de chiffrement sur un serveur, installez plusieurs cartes réseau et configurez chaque carte séparément.
>  - Pour vérifier que le certificat a une clé privée correspondante, dans Configuration des services Bureau à distance, cliquez avec le bouton droit sur la connexion dont vous souhaitez voir le certificat, sélectionnez **Général**, puis sélectionnez **Modifier**. Ensuite, sélectionnez **Afficher le certificat**. Sous l’onglet **Général**, l’information « Vous avez une clé privée qui correspond à ce certificat » doit apparaître si une clé est présente. Vous pouvez également voir ces informations à l’aide du composant logiciel enfichable Certificats.
>  - Le chiffrement compatible FIPS (la stratégie **Chiffrement système : utilisez des algorithmes compatibles FIPS pour le chiffrement, le hachage et la signature** ou le paramètre **Compatible FIPS** dans Configuration des services Bureau à distance) chiffre et déchiffre les données envoyées entre le serveur et le client avec les algorithmes de chiffrement FIPS (Federal Information Processing Standard) 140-1, qui utilisent les modules de chiffrement Microsoft. Pour plus d’informations, consultez [Validation FIPS 140](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation).
>  - Le paramètre **Élevé** chiffre les données envoyées entre le serveur et le client à l’aide d’un chiffrement renforcé 128 bits.
>  - Le paramètre **Compatible avec le client** chiffre les données envoyées entre le client et le serveur avec la force de clé maximale prise en charge par le client.
>  - Le paramètre **Faible** chiffre les données envoyées du client au serveur à l’aide du chiffrement 56 bits.
