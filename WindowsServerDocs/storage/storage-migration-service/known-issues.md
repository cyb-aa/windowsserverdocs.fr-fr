---
title: Service de Migration de stockage problèmes connus
description: Problèmes connus et dépannage prise en charge du Service de Migration de stockage, notamment comment collecter des journaux pour le Support Microsoft.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 02/27/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: f5fefab2c1b7ba8b62ffd6734217eab9a13ae95e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888870"
---
# <a name="storage-migration-service-known-issues"></a>Service de Migration de stockage problèmes connus

Cette rubrique contient des réponses aux problèmes connus lors de l’utilisation [Service de Migration de stockage](overview.md) pour migrer des serveurs.

## <a name="collecting-logs"></a> Comment collecter des fichiers journaux lorsque vous travaillez avec le Support Microsoft

Le Service de Migration de stockage contient des journaux des événements pour le service Orchestrator et le Service de Proxy. Le serveur urchestrator contient toujours les deux journaux des événements, et les serveurs de destination avec le service de proxy installé contiennent les journaux de proxy. Ces journaux sont situés sous :

- Journaux des applications et Services \ Microsoft \ Windows \ StorageMigrationService
- Journaux des applications et Services \ Microsoft \ Windows \ StorageMigrationService-Proxy

Si vous avez besoin pour collecter ces journaux hors connexion ou à envoyer au Support Microsoft, il est un script PowerShell disponible sur GitHub, open source :

 [Application auxiliaire Service de Migration de stockage](https://aka.ms/smslogs) 

Passez en revue le fichier Lisezmoi pour l’utilisation.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>Service de Migration de stockage n’apparaît pas dans Windows Admin Center, sauf si la gestion de Windows Server 2019

Lorsque vous utilisez la version 1809 de Windows Admin Center pour gérer un orchestrateur de Windows Server 2019, vous ne voyez pas l’option d’outil pour le Service de Migration de stockage. 

L’extension de Service de Migration de stockage Windows Admin Center est lié aux version pour gérer uniquement la version de Windows Server 2019 1809 ou des systèmes d’exploitation ultérieurs. Si vous l’utilisez pour gérer les systèmes d’exploitation Windows Server ou des versions préliminaires insider, l’outil ne s’affiche pas. Ce comportement est normal. 

Pour résoudre, utilisez ou mettre à niveau vers Windows Server 2019 build 1809 ou version ultérieure.

## <a name="storage-migration-service-doesnt-let-you-choose-static-ip-on-cutover"></a>Service de Migration de stockage ne vous permettent de choisir une adresse IP statique sur le basculement

Lorsque vous utilisez la version 0,57 la Migration du Service de stockage extension dans Windows Admin Center et vous atteindre la phase de basculement, vous ne pouvez pas sélectionner une adresse IP statique pour une adresse. Vous êtes obligé d’utiliser le protocole DHCP.

Pour résoudre ce problème, dans Windows Admin Center, regardez sous **paramètres** > **Extensions** pour une alerte indiquant que la version mise à jour Service de Migration de stockage 0.57.2 peut être installée. Vous devrez peut-être redémarrer votre onglet de navigateur pour le centre d’administration de Windows.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>Validation de basculement de Service de Migration de stockage échoue avec l’erreur « Accès refusé pour la stratégie de jeton de filtre sur l’ordinateur de destination »

Lors de l’exécution de la validation de basculement, vous recevez l’erreur « Échec : Accès refusé pour la stratégie de jeton de filtre sur l’ordinateur de destination. » Cela se produit même si vous avez fourni les informations d’identification d’administrateur local approprié pour les ordinateurs source et de destination.

Ce problème est dû à une erreur de code dans Windows Server 2019. Le problème se produit lorsque vous utilisez l’ordinateur de destination en tant qu’un orchestrateur de Service de Migration de stockage.

Pour contourner ce problème, installez le Service de Migration de stockage sur un ordinateur Windows Server 2019 qui n’est pas la destination de migration souhaité, puis se connecter à ce serveur avec Windows Admin Center et effectuer la migration.

Nous avons l’intention de résoudre ce problème dans une version ultérieure de Windows Server. Ouvrez une demande de support via [Support Microsoft](https://support.microsoft.com) pour demander un rétroporter de ce correctif soit créé.

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-edition"></a>Service de Migration de stockage n’est pas inclus dans l’édition d’évaluation de Windows Server 2019

Lorsque vous utilisez Windows Admin Center pour vous connecter à un [version d’évaluation de Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) il n’est pas une option permettant de gérer le Service de Migration de stockage. Service de Migration de stockage n’est pas également inclus dans les rôles et fonctionnalités.

Ce problème est dû à un problème de maintenance dans le support d’évaluation de Windows Server 2019. 

Pour contourner ce problème, installez une vente au détail, de la version MSDN, OEM ou licence en Volume de Windows Server 2019 et ne pas l’activer. Sans activation, toutes les éditions de Windows Server fonctionnent en mode d’évaluation de 180 jours. 

Nous avons résolu ce problème dans une version ultérieure de Windows Server 2019.  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>Service de Migration de stockage expire de téléchargement de l’erreur de transfert CSV

Lorsque vous utilisez Windows Admin Center ou PowerShell pour télécharger l’opérations détaillées erreurs uniquement CSV journal de transfert, erreur :

 >   Journal de transfert, vérifiez que le partage de fichiers est autorisé dans votre pare-feu. : Cette opération de demande envoyée à NET.TCP://localhost : 28940/sms/service/1/transfert n’a pas reçu de réponse dans le délai imparti (00 : 01:00). Le temps alloué à cette opération peut avoir fait partie d’un délai d’expiration plus long. Cela peut être, car le service traite toujours l’opération ou le service n’a pas pu envoyer un message de réponse. Veuillez, envisagez d’augmenter le délai d’expiration de l’opération (en castant le canal/proxy vers IContextChannel et en définissant la propriété OperationTimeout) et vous assurer que le service est en mesure de se connecter au client.

Ce problème est dû à un très grand nombre de fichiers transférés qui ne peuvent pas être filtrées dans le délai d’une minute par défaut autorisé par le Service de MIgration de stockage. 

Pour contourner ce problème :

1. Sur l’ordinateur orchestrator, modifiez le *%SYSTEMROOT%\SMS\Microsoft.StorageMigration.Service.exe.config* fichier à l’aide de Notepad.exe pour modifier le paramètre « sendTimeout » à partir de sa valeur par défaut de 1 minute à 10 minutes

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. Redémarrez le service « Service de Migration de stockage » sur l’ordinateur orchestrator. 
3. Sur l’ordinateur orchestrator, lancez Regedit.exe
4. Localisez la sous-clé de Registre suivante, puis cliquez dessus : 

   `HKEY_LOCAL_MACHINE\\Software\\Microsoft\\SMSPowershell`

5. Dans le menu Edition, pointez sur Nouveau, puis cliquez sur DWORD Value. 
6. Tapez « WcfOperationTimeoutInMinutes » pour le nom de la valeur DWORD, puis appuyez sur ENTRÉE.
7. Cliquez sur « WcfOperationTimeoutInMinutes », puis cliquez sur Modifier. 
8. Dans la zone de la Base de données, cliquez sur « Décimal »
9. Dans la zone de données de valeur, tapez « 10 », puis cliquez sur OK.
10. Quittez l’Éditeur du Registre.
11. Essayez à nouveau de télécharger le fichier CSV erreurs uniquement. 

Nous avons l’intention de modifier ce comportement dans une version ultérieure de Windows Server 2019.  

## <a name="cutover-fails-when-migrating-between-networks"></a>Le basculement échoue lors de la migration entre les réseaux

Lorsque vous migrez vers une machine virtuelle de destination en cours d’exécution dans un autre réseau que la source, par exemple une instance Azure IaaS, le basculement échoue lorsque la source utilisait une adresse IP statique. 

Ce comportement est normal, afin d’éviter les problèmes de connectivité après la migration à partir des utilisateurs, des applications et des scripts pour se connecter via l’adresse IP. Lorsque l’adresse IP est déplacée à partir de l’ancien ordinateur source vers la nouvelle cible de destination, il ne correspondra pas les nouvelles informations de sous-réseau de réseau et de peut-être DNS et de WINS.

Pour contourner ce problème, effectuez une migration vers un ordinateur sur le même réseau. Déplacer cet ordinateur vers un nouveau réseau, puis réaffecter ses informations IP. Par exemple, si une migration vers Azure IaaS, tout d’abord migrer vers une machine virtuelle locale, puis utiliser Azure Migrate pour déplacer la machine virtuelle vers Azure.  

Nous avons résolu ce problème dans une version ultérieure de Windows Server 2019. Nous allons permettent à présent vous permettent de spécifier des migrations qui ne pas modifier les paramètres de réseau du serveur de destination. Nous pouvons publier une mise à jour vers la version existante de Windows Server 2019 dans le cadre du cycle de mise à jour mensuel normal. 


## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble du Service de Migration de stockage](overview.md)