---
title: Vérifier la configuration après les modifications de NPS
description: Vous pouvez utiliser cette rubrique pour vérifier la configuration du serveur de stratégie réseau Windows Server 2016 après une modification d’adresse IP ou de nom sur le serveur.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9ba1f5f494228f6bdba22b300a2336fa6eb37690
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405354"
---
# <a name="verify-configuration-after-nps-changes"></a>Vérifier la configuration après les modifications de NPS

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour vérifier la configuration de NPS après une modification d’adresse IP ou de nom sur le serveur.

## <a name="verify-configuration-after-an-nps-ip-address-change"></a>Vérifier la configuration après une modification de l’adresse IP du serveur NPS

Il peut arriver que vous deviez modifier l’adresse IP d’un serveur NPS ou proxy, par exemple lorsque vous déplacez le serveur vers un sous-réseau IP différent. 

Si vous modifiez une adresse IP de serveur NPS ou proxy, vous devez reconfigurer certaines parties de votre déploiement NPS. 

Utilisez les instructions générales suivantes pour vous aider à vérifier qu’un changement d’adresse IP n’interrompt pas l’authentification de l’accès réseau, l’autorisation ou la gestion des comptes sur votre réseau pour les serveurs RADIUS NPS et les serveurs proxy RADIUS.

Pour exécuter ces procédures, vous devez être membre du **groupe administrateurs**ou d’un groupe équivalent.

### <a name="to-verify-configuration-after-an-nps-ip-address-change"></a>Pour vérifier la configuration après une modification de l’adresse IP du serveur NPS

1. Reconfigurez tous les clients RADIUS, tels que les points d’accès sans fil et les serveurs VPN, avec la nouvelle adresse IP du serveur NPS.

2. Si le serveur NPS est membre d’un groupe de serveurs RADIUS distants, reconfigurez le proxy NPS avec la nouvelle adresse IP du serveur NPS.

3. Si vous avez configuré le serveur NPS pour utiliser SQL Server la journalisation, vérifiez que la connectivité entre l’ordinateur exécutant SQL Server et le serveur NPS fonctionne toujours correctement.

4. Si vous avez déployé IPsec pour sécuriser le trafic RADIUS entre votre serveur NPS et un proxy NPS ou d’autres serveurs ou périphériques, reconfigurez la stratégie IPsec ou la règle de sécurité de connexion dans le pare-feu Windows avec fonctions avancées de sécurité pour utiliser la nouvelle adresse IP du serveur NPS.

5. Si le serveur NPS est multirésident et que vous avez configuré le serveur pour qu’il soit lié à une carte réseau spécifique, reconfigurez les paramètres de port NPS avec la nouvelle adresse IP.

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>Pour vérifier la configuration après une modification de l’adresse IP du proxy NPS

1. Reconfigurez tous les clients RADIUS, tels que les points d’accès sans fil et les serveurs VPN, avec la nouvelle adresse IP du proxy NPS.

2. Si le proxy NPS est multirésident et que vous avez configuré le proxy pour qu’il soit lié à une carte réseau spécifique, reconfigurez les paramètres de port NPS avec la nouvelle adresse IP.

3. Reconfigurez tous les membres de tous les groupes de serveurs RADIUS distants avec l’adresse IP du serveur proxy. Pour accomplir cette tâche, sur chaque serveur NPS dont le proxy NPS est configuré en tant que client RADIUS :

    a. Double-cliquez sur **NPS (local)** , double-cliquez sur **clients et serveurs RADIUS**, cliquez sur **clients RADIUS**, puis, dans le volet d’informations, double-cliquez sur le client RADIUS que vous souhaitez modifier.

    b. Dans **Propriétés**du client RADIUS, dans **adresse \(IP ou DNS @ no__t-3**, tapez la nouvelle adresse IP du proxy NPS.

4. Si vous avez configuré le proxy NPS pour utiliser SQL Server la journalisation, vérifiez que la connectivité entre l’ordinateur exécutant SQL Server et le proxy NPS fonctionne toujours correctement.

## <a name="verify-configuration-after-renaming-an-nps"></a>Vérifier la configuration après avoir renommé un serveur NPS

Dans certains cas, vous devrez peut-être modifier le nom d’un serveur NPS ou d’un proxy, par exemple lorsque vous remaniez les conventions d’affectation de noms de vos serveurs.

Si vous modifiez un nom NPS ou proxy, vous devez reconfigurer certaines parties de votre déploiement NPS. 

Utilisez les instructions générales suivantes pour vous aider à vérifier qu’un changement de nom de serveur n’interrompt pas l’authentification, l’autorisation ou la gestion de l’accès réseau.

Pour effectuer cette procédure, vous devez être membre du **groupe administrateurs**ou d’un groupe équivalent.

### <a name="to-verify-configuration-after-an-nps-or-proxy-name-change"></a>Pour vérifier la configuration après la modification d’un nom de proxy ou NPS

1. Si le serveur NPS est membre d’un groupe de serveurs RADIUS distants et que le groupe est configuré avec des noms d’ordinateur plutôt que des adresses IP, reconfigurez le groupe de serveurs RADIUS distants avec le nouveau nom de serveur NPS.

2. Si les méthodes d’authentification basées sur les certificats sont déployées sur le serveur NPS, la modification du nom invalide le certificat du serveur. Vous pouvez demander un nouveau certificat auprès de l’administrateur de l’autorité de certification ou, si l’ordinateur est un ordinateur membre du domaine et que vous inscrivez automatiquement les certificats sur les membres du domaine, vous pouvez actualiser stratégie de groupe pour obtenir un nouveau certificat par le biais de l’inscription automatique. . Pour actualiser stratégie de groupe :

    a. Ouvrez l’invite de commandes ou Windows PowerShell.

    b. Tapez **gpupdate** et appuyez sur Entrée.


3. Une fois que vous disposez d’un nouveau certificat de serveur, demandez à l’administrateur de l’autorité de certification de révoquer l’ancien certificat. 

     Une fois l’ancien certificat révoqué, NPS continue à l’utiliser jusqu’à l’expiration de l’ancien certificat. Par défaut, l’ancien certificat reste valide pour une durée maximale d’une semaine et 10 heures. Cette période peut être différente selon que la liste de révocation de certificats (CRL) a expiré et que la durée d’expiration du cache TLS (Transport Layer Security) a été modifiée par rapport à ses valeurs par défaut. L’expiration de la liste de révocation de certificats par défaut est d’une semaine ; l’heure d’expiration du cache TLS par défaut est de 10 heures. 

     Toutefois, si vous souhaitez configurer NPS pour utiliser immédiatement le nouveau certificat, vous pouvez reconfigurer manuellement les stratégies réseau avec le nouveau certificat.

4. Après l’expiration de l’ancien certificat, le serveur NPS commence automatiquement à utiliser le nouveau certificat. 

5. Si vous avez configuré le serveur NPS pour utiliser SQL Server la journalisation, vérifiez que la connectivité entre l’ordinateur exécutant SQL Server et le serveur NPS fonctionne toujours correctement.

