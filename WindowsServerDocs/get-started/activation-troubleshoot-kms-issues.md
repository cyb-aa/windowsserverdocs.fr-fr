---
title: Problèmes connus de l’activation KMS
description: Décrit les problèmes courants qui peuvent se produire pendant le processus d’activation KMS (Key Management Service, Service de gestion de clés), et fournit des solutions et des conseils
ms.topic: article
ms.date: 10/3/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: 3446ad0954510d8c96e9a2d361f24c90d325b782
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826252"
---
# <a name="kms-activation-known-issues"></a>Activation KMS : problèmes connus

Cet article décrit les questions et les problèmes courants qui peuvent se poser lors des activations KMS, et fournit des conseils pour résoudre les problèmes.

> [!NOTE]
> Si vous pensez que votre problème est lié à DNS, consultez [Procédures de dépannage courantes pour les problèmes KMS et DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="should-i-back-up-kms-host-information"></a>Dois-je sauvegarder les informations de l’hôte KMS ?

La sauvegarde n’est pas obligatoire pour les hôtes KMS. Cependant, si vous utilisez un outil pour nettoyer régulièrement les journaux des événements, l’historique des activations stocké dans les journaux peut être perdu. Si vous utilisez le journal des événements pour suivre ou documenter les activations KMS, exportez périodiquement le journal des événements du service de gestion de clés à partir du dossier Journaux des applications et des services de l’Observateur d’événements.

Si vous utilisez System Center Operations Manager, la base de données de l’entrepôt de données System Center stocke les données du journal des événements pour la création de rapports : vous ne devez donc pas sauvegarder les journaux des événements séparément.

## <a name="is-the-kms-client-computer-activated"></a>L’ordinateur client KMS est-il activé ?

Sur l’ordinateur client KMS, ouvrez le Panneau de configuration **Système** et recherchez le message **Windows est activé**. Vous pouvez aussi exécuter Slmgr.vbs et utiliser l’option de ligne de commande **/dli**.

## <a name="the-kms-client-computer-does-not-activate"></a>L’ordinateur client KMS ne s’active pas

Vérifiez si le seuil d’activation KMS est atteint. Sur l’ordinateur hôte KMS, exécutez Slmgr.vbs et utilisez l’option de ligne de commande **/dli** pour déterminer le nombre actuel de clients de l’hôte. Tant que l’hôte KMS n’a pas un total de 25, les ordinateurs clients Windows 7 ne peuvent pas être activés. Les clients KMS Windows Server 2008 R2 nécessitent un nombre de clients KMS de 5 pour l’activation. Pour plus d’informations sur la configuration requise de KMS, consultez le [Guide de planification d’activation en volume](https://go.microsoft.com/fwlink/?linkid=155926). 

Sur l’ordinateur client KMS, recherchez l’ID d’événement 12289 dans le journal des événements d’application. Vérifiez les informations suivantes pour cet événement :

- Le code du résultat est-il **0** ? Toute autre valeur indique une erreur.
- Le nom d’hôte KMS dans l’événement est-il correct ?
- Le port KMS est-il correct ?
- L’hôte KMS est-il accessible ?
- Si le client exécute un pare-feu non-Microsoft, le port de sortie doit-il être configuré ?

Sur l’ordinateur hôte KMS, recherchez l’ID d’événement 12290 dans le journal des événements KMS. Vérifiez les informations suivantes pour cet événement :

- L’hôte KMS a-t-il consigné dans le journal une demande de l’ordinateur client ? Vérifiez que le nom de l’ordinateur client KMS figure dans la liste. Vérifiez que le client et l’hôte KMS peuvent communiquer. Le client a-t-il reçu la réponse ?
- Si aucun événement n’est consigné à partir du client KMS, c’est que la demande n’a pas atteint l’hôte KMS ou que l’hôte KMS n’a pas pu la traiter. Vérifiez que les routeurs ne bloquent pas le trafic qui utilise le port TCP 1688 (si le port par défaut est utilisé) et que le trafic avec état vers le client KMS est autorisé.

## <a name="what-does-this-error-code-mean"></a>Que signifie ce code d’erreur ?

À l’exception des événements KMS qui ont l’ID d’événement 12290, Windows consigne tous les événements d’activation dans le journal des événements d’application sous le nom du fournisseur d’événements Microsoft-Windows-Security-SPP. Windows consigne les événements KMS dans le journal du service de gestion de clés dans le dossier Applications et services. Les professionnels de l’informatique peuvent exécuter Slui.exe pour afficher une description de la plupart des codes d’erreur liés à l’activation. La syntaxe générale de cette commande est la suivante :

```cmd
slui.exe 0x2a ErrorCode
```

Par exemple, si l’ID d’événement 12293 contient le code d’erreur 0x8007267C, vous pouvez afficher une description de cette erreur en exécutant la commande suivante :

```cmd
slui.exe 0x2a 0x8007267C
```

Pour plus d’informations sur les codes d’erreur spécifiques et sur la manière de les résoudre, consultez [Résolution des codes d’erreur d’activation courants](activation-error-codes.md).

## <a name="clients-are-not-adding-to-the-kms-count"></a>Les clients ne sont pas ajoutés au nombre de clients KMS

Pour réinitialiser l’ID d’ordinateur client (CMID) et les autres informations d’activation de produit, exécutez **sysprep /generalize** ou **slmgr /rearm**. Sinon, chaque ordinateur client apparaît identique et l’hôte KMS ne les compte pas comme des clients KMS distincts.

## <a name="kms-hosts-are-unable-to-create-srv-records"></a>Les hôtes KMS ne peuvent pas créer d’enregistrements SRV

Le système DNS (Domain Name System) peut restreindre l’accès en écriture ou ne pas prendre en charge le DNS dynamique (DDNS). Dans ce cas, accordez à l’hôte KMS un accès en écriture à la base de données DNS ou créez manuellement l’enregistrement de ressource de service (SRV). Pour plus d’informations sur les problèmes liés à KMS et au DNS, consultez [Procédures de dépannage courantes pour les problèmes KMS et DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="only-the-first-kms-host-is-able-to-create-srv-records"></a>Seul le premier hôte KMS est en mesure de créer des enregistrements SRV

Si l’organisation a plusieurs hôtes KMS, les autres hôtes peuvent ne pas être en mesure de mettre à jour l’enregistrement de ressource de service (SRV), sauf si les autorisations par défaut de SRV sont changées. Pour plus d’informations sur les problèmes liés à KMS et au DNS, consultez [Procédures de dépannage courantes pour les problèmes KMS et DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="i-installed-a-kms-key-on-the-kms-client"></a>J’ai installé une clé KMS sur le client KMS

Les clés KMS doivent être installées seulement sur des hôtes KMS, et non pas sur des clients KMS. Exécutez **slmgr.vbs -ipk &lt;Clé_installation&gt;** . Pour des tableaux de clés que vous pouvez utiliser pour configurer l’ordinateur comme client KMS, consultez [Clés d’installation du client KMS](KMSclientkeys.md). Ces clés sont connues publiquement et sont spécifiques à l’édition. N’oubliez pas de supprimer les enregistrements de ressource SRV non nécessaires dans le DNS, puis redémarrez les ordinateurs.

## <a name="a-kms-host-failed"></a>Échec d’un hôte KMS

Si un hôte KMS échoue, vous devez installer une clé d’hôte KMS sur un nouvel hôte, puis activer l’hôte. Vérifiez que le nouvel hôte KMS a un enregistrement de ressource SRV dans la base de données DNS. Si vous installez le nouvel hôte KMS en utilisant le même nom d’ordinateur et la même adresse IP que l’hôte KMS défaillant, le nouvel hôte KMS peut utiliser l’enregistrement SRV DNS de l’hôte défaillant. Si le nouvel hôte a un nom d’ordinateur différent, vous pouvez supprimer manuellement l’enregistrement de ressource SRV DNS de l’hôte défaillant ou (si le nettoyage est activé dans DNS) laisser DNS le supprimer automatiquement. Si le réseau utilise DDNS, le nouvel hôte KMS crée automatiquement un enregistrement de ressource SRV sur le serveur DNS. Le nouvel hôte KMS commence ensuite à collecter les demandes de renouvellement de client et commence à activer des clients dès que le seuil d’activation KMS est atteint.

Si vos clients KMS utilisent la découverte automatique, ils sélectionnent automatiquement un autre hôte KMS si l’hôte KMS d’origine ne répond pas aux demandes de renouvellement. Si les clients n’utilisent pas la détection automatique, vous devez mettre à jour manuellement les ordinateurs clients KMS affectés à l’hôte KMS défaillant en exécutant **slmgr.vbs /skms**. Pour éviter ce scénario, configurez les clients KMS pour qu’ils utilisent la découverte automatique. Pour plus d’informations, consultez le [Guide de déploiement d’activation en volume](https://go.microsoft.com/fwlink/?linkid=150083).
