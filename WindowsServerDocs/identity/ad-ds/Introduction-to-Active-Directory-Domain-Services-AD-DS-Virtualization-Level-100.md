---
title: Virtualisation Active Directory Domain Services en toute sécurité (AD DS)
description: Restauration USN et virtualisation sécurisée des Active Directory
ms.topic: article
ms.prod: windows-server
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: 67e35a47467b1f5f66bfd073c6f9db06094ea3f9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391026"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>Virtualisation Active Directory Domain Services en toute sécurité (AD DS)

>S'applique à : Windows Server

À compter de Windows Server 2012, AD DS fournit une prise en charge accrue pour la virtualisation des contrôleurs de domaine en introduisant des fonctionnalités sécurisées pour la virtualisation. Cet article explique le rôle des USN et des InvocationIDs dans la réplication du contrôleur de domaine et décrit certains problèmes potentiels qui peuvent se produire.

## <a name="update-sequence-number-and-invocationid"></a>Mettre à jour le numéro de séquence et l’invocation

Les environnements virtuels posent des défis uniques pour les charges de travail distribuées qui dépendent d’un schéma de réplication fondé sur un mécanisme d’horloge logique. Par exemple, la réplication AD DS utilise une valeur qui augmente de manière monotone (appelée « USN » ou « numéro de séquence de mise à jour ») et est affectée aux transactions sur chaque contrôleur de domaine. L’instance de base de données de chaque contrôleur de domaine reçoit également une identité, appelée « invocation ». La valeur InvocationID d’un contrôleur de domaine et sa valeur USN jouent ensemble le rôle d’un identificateur unique associé à chaque transaction en écriture réalisée dans chaque contrôleur de domaine et doivent être uniques au sein de la forêt.

La réplication AD DS exploite des valeurs InvocationID et USN sur chaque contrôleur de domaine afin de déterminer les changements à répliquer sur d’autres contrôleurs de domaine. Si un contrôleur de domaine est restauré dans le temps en dehors de la connaissance du contrôleur de domaine et qu’un USN est réutilisé pour une transaction totalement différente, la réplication ne convergera pas, car les autres contrôleurs de domaine pensent qu’ils ont déjà reçu les mises à jour. associé à l’USN réutilisé dans le contexte de cet invocation.

Par exemple, l’illustration suivante décrit la séquence des événements qui ont lieu dans Windows Server 2008 R2 et des systèmes d’exploitation antérieurs lorsqu’une restauration USN est détectée sur VDC2, le contrôleur de domaine de destination exécuté sur un ordinateur virtuel. Dans cette illustration, la détection de la restauration USN se produit sur VDC2 lorsqu’un partenaire de réplication détecte que VDC2 a envoyé une valeur USN de mise à jour qui a été vue précédemment par le partenaire de réplication, ce qui indique que la base de données Vdc2 a été restaurée dans le temps mal.

![Séquence d’événements lorsque la restauration USN est détectée](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Une machine virtuelle permet aux administrateurs de l’hyperviseur de restaurer facilement les USN d’un contrôleur de domaine (son horloge logique) en appliquant, par exemple, un instantané en dehors de la connaissance du contrôleur de domaine. Pour obtenir des informations sur les numéros USN et la restauration USN et consulter une autre illustration décrivant les instances de restauration USN non détectées, voir les sections [USN et Restauration USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

À partir de Windows Server 2012, AD DS les contrôleurs de domaine virtuels hébergés sur des plateformes hyperviseur qui exposent un identificateur appelé ID de génération d’ordinateur virtuel peuvent détecter et utiliser les mesures de sécurité nécessaires pour protéger l’environnement AD DS si l’ordinateur virtuel est déployé en arrière-plan par l’application d’un instantané de machine virtuelle. La structure de l’ID de génération d’ordinateur virtuel repose sur un mécanisme hyperviseur/fournisseur indépendant qui présente l’identificateur dans l’espace d’adressage de l’ordinateur virtuel invité, de sorte que l’expérience de virtualisation sécurisée reste systématiquement disponible depuis chaque hyperviseur prenant en charge les ID de génération d’ordinateur virtuel. Cet identificateur peut être testé par les services et les applications en cours d’exécution sur l’ordinateur virtuel afin de détecter si un ordinateur virtuel a été restauré à temps.

## <a name="effects-of-usn-rollback"></a>Effets de la restauration USN

Lorsque des restaurations USN se produisent, les modifications apportées aux objets et aux attributs ne sont pas répliquées en entrée par les contrôleurs de domaine de destination qui ont déjà vu l’USN.

Étant donné que ces contrôleurs de domaine de destination pensent qu’ils sont à jour, aucune erreur de réplication n’est signalée dans les journaux des événements du service d’annuaire ou par les outils de surveillance et de diagnostic.

La restauration USN peut affecter la réplication d’un objet ou d’un attribut dans n’importe quelle partition. L’effet secondaire le plus fréquent est que les comptes d’utilisateurs et d’ordinateurs créés sur le contrôleur de domaine de restauration n’existent pas sur un ou plusieurs partenaires de réplication. Ou bien, les mises à jour de mot de passe provenant du contrôleur de domaine de restauration n’existent pas sur les partenaires de réplication.

Une restauration USN peut empêcher la réplication d’un type d’objet dans une partition de Active Directory. Ces types d’objets sont les suivants :

* La topologie et la planification de la réplication Active Directory
* L’existence de contrôleurs de domaine dans la forêt et les rôles que ces contrôleurs de domaine détiennent
* L’existence de partitions de domaine et d’application dans la forêt
* L’existence de groupes de sécurité et leurs appartenances de groupe actuelles
* Inscription des enregistrements DNS dans les zones DNS intégrées à Active Directory

La taille du trou USN peut représenter des centaines, des milliers, voire des dizaines de milliers de modifications pour les utilisateurs, les ordinateurs, les approbations, les mots de passe et les groupes de sécurité. Le trou USN est défini par la différence entre le numéro USN le plus élevé qui existait lors de la sauvegarde de l’état du système restaurée et le nombre de modifications d’origine qui ont été créées sur le contrôleur de domaine restauré avant qu’il ne soit mis hors connexion.

## <a name="detecting-a-usn-rollback"></a>Détection d’une restauration USN

Comme une restauration USN est difficile à détecter, un contrôleur de domaine enregistre l’événement 2095 lorsqu’un contrôleur de domaine source envoie un numéro USN précédemment reconnu à un contrôleur de domaine de destination sans modification correspondante dans l’ID d’invocation.

Pour empêcher que des mises à jour d’origine uniques Active Directory soient créées sur le contrôleur de domaine restauré de manière incorrecte, le service accès réseau est suspendu. Lorsque le service accès réseau est suspendu, les comptes d’utilisateurs et d’ordinateurs ne peuvent pas modifier le mot de passe sur un contrôleur de domaine qui n’effectue pas de réplication sortante de ces modifications. De même, Active Directory outils d’administration privilégient un contrôleur de domaine sain lorsqu’ils effectuent des mises à jour d’objets dans Active Directory.

Sur un contrôleur de domaine, les messages d’événement qui ressemblent à ce qui suit sont enregistrés si les conditions suivantes sont remplies :

* Un contrôleur de domaine source envoie un numéro USN reconnu précédemment à un contrôleur de domaine de destination.
* Il n’y a aucune modification correspondante dans l’ID d’invocation.

Ces événements peuvent être capturés dans le journal des événements du service d’annuaire. Toutefois, elles peuvent être remplacées avant d’être observées par un administrateur.

Si vous pensez qu’une restauration USN s’est produite mais que vous ne voyez pas d’événement correspondant dans les journaux des événements, recherchez l’entrée DSA non accessible en écriture dans le registre. Cette entrée fournit une preuve légale qu’une restauration USN s’est produite.

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> La suppression ou la modification manuelle de la valeur de l’entrée de Registre DSA not inscriptible met le contrôleur de domaine Rollback dans un État non pris en charge de façon permanente. Par conséquent, ces modifications ne sont pas prises en charge. En particulier, la modification de la valeur supprime le comportement de mise en quarantaine ajouté par le code de détection de restauration USN. Les partitions Active Directory sur le contrôleur de domaine de restauration seront définitivement incohérentes avec les partenaires de réplication directs et transitives dans la même forêt Active Directory.

Pour plus d’informations sur cette clé de Registre et les étapes de résolution, consultez l’article de support [Active Directory Replication Error 8456 ou 8457 : «La source | le serveur de destination rejette actuellement les demandes de réplication «](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination).

## <a name="virtualization-based-safeguards"></a>Protections basées sur la virtualisation

Au cours de l’installation du contrôleur de domaine, AD DS stocke initialement l’identificateur GenerationID de la machine virtuelle dans le cadre de l’attribut msDS-GenerationID sur l’objet ordinateur du contrôleur de domaine dans sa base de données (souvent appelée arborescence d’informations de répertoire ou DIT). L’ID de génération d’ordinateur virtuel est contrôlé de manière indépendante par un pilote Windows au sein de l’ordinateur virtuel.

Lorsqu’un administrateur restaure l’ordinateur virtuel à partir d’une capture instantanée précédente, la valeur actuelle de l’ID de génération d’ordinateur virtuel extraite du pilote de l’ordinateur virtuel est comparée à une valeur au sein du DIT.

Si les deux valeurs sont différentes, l’attribut InvocationID est réinitialisé et le pool RID est rejeté, ce qui empêche toute réutilisation des numéros USN. Si les valeurs sont identiques, la transaction est validée comme une transaction normale.

Les services de domaine Active Directory (AD DS) comparent également la valeur actuelle de l’ID de génération d’ordinateur virtuel provenant de l’ordinateur virtuel à la valeur interne du DIT chaque fois que le contrôleur de domaine est redémarré. S’ils trouvent une valeur différente, ils réinitialisent l’attribut InvocationID, rejettent le pool RID et mettent à jour le DIT pour y inclure la nouvelle valeur. Ils procèdent également à une synchronisation ne faisant pas autorité du dossier SYSVOL pour mener la restauration à terme, en toute sécurité. Les mécanismes de protection peuvent ainsi étendre l’application des captures instantanées sur des ordinateurs virtuels qui ont été arrêtés. Ces dispositifs de protection introduits dans Windows Server 2012 permettent aux administrateurs AD DS de tirer parti des avantages uniques du déploiement et de la gestion des contrôleurs de domaine dans un environnement virtualisé.

L’illustration suivante montre comment les dispositifs de protection de la virtualisation sont appliqués lorsque la même restauration USN est détectée sur un contrôleur de domaine virtualisé qui exécute Windows Server 2012 sur un hyperviseur qui prend en charge VM-GenerationID.

![Dispositifs de protection appliqués lorsque la même restauration USN est détectée](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

Dans ce cas, dès que l’hyperviseur détecte que la valeur de l’ID de génération d’ordinateur virtuel a été modifiée, les mécanismes de protection de la virtualisation sont déclenchés, y compris la réinitialisation de l’attribut InvocationID pour le contrôleur de domaine virtualisé (de A à B dans l’exemple précédent) et la mise à jour de la valeur de l’ID de génération d’ordinateur virtuel enregistrée sur l’ordinateur virtuel pour correspondre à la nouvelle valeur (G2) stockée par l’hyperviseur. Les mécanismes de protection de la virtualisation s’assurent que la réplication converge pour les deux contrôleurs de domaine.

Avec Windows Server 2012, AD DS utilise des protections sur les contrôleurs de domaine virtuels hébergés sur des hyperviseurs compatibles GenerationID et s’assure que l’application accidentelle de captures instantanées ou d’autres mécanismes avec hyperviseur qui pourraient restaurer une machine virtuelle l’état de l’ordinateur n’interrompt pas l’environnement de AD DS (en empêchant les problèmes de réplication tels qu’une bulle USN ou des objets en attente).

La restauration d’un contrôleur de domaine en appliquant un instantané d’ordinateur virtuel n’est pas recommandée comme autre mécanisme de sauvegarde d’un contrôleur de domaine. Nous vous recommandons de continuer à utiliser la fonctionnalité Sauvegarde Windows Server ou d’autres solutions de sauvegarde fondées sur l’enregistreur VSS.

> [!CAUTION]
> Si un contrôleur de domaine dans un environnement de production est rétabli accidentellement dans un instantané, il est recommandé de consulter les fournisseurs pour les applications et les services hébergés sur cet ordinateur virtuel, pour obtenir des conseils sur la vérification de l’état de ces programmes après restauration de capture instantanée.

Pour plus d‘informations, voir [Architecture de restauration sécurisée des contrôleurs de domaine virtualisés](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="recovering-from-a-usn-rollback"></a>Récupération à partir d’une restauration USN

Il existe deux approches pour récupérer à partir d’une restauration USN :

* Supprimer le contrôleur de domaine du domaine
* Restaurer l’état du système d’une sauvegarde correcte

### <a name="remove-the-domain-controller-from-the-domain"></a>Supprimer le contrôleur de domaine du domaine

1. Supprimez Active Directory du contrôleur de domaine pour le forcer à être un serveur autonome.
2. Arrêtez le serveur rétrogradé.
3. Sur un contrôleur de domaine sain, nettoyez les métadonnées du contrôleur de domaine rétrogradé.
4. Si le contrôleur de domaine restauré de manière incorrecte héberge des rôles de maître d’opérations, transférez ces rôles vers un contrôleur de domaine sain.
5. Redémarrez le serveur rétrogradé.
6. Si vous avez besoin de, installez à nouveau Active Directory sur le serveur autonome.
7. Si le contrôleur de domaine était auparavant un catalogue global, configurez le contrôleur de domaine en tant que catalogue global.
8. Si le contrôleur de domaine hébergeait précédemment des rôles de maître d’opérations hébergés, transférez les rôles de maître d’opérations vers le contrôleur de domaine.

### <a name="restore-the-system-state-of-a-good-backup"></a>Restaurer l’état du système d’une sauvegarde correcte

Déterminez si des sauvegardes d’État du système valides existent pour ce contrôleur de domaine. Si une sauvegarde de l’état du système a été effectuée avant que le contrôleur de domaine restauré n’ait été restauré de manière incorrecte et que la sauvegarde contient les modifications récentes apportées sur le contrôleur de domaine, restaurez l’état du système à partir de la sauvegarde la plus récente.

Vous pouvez également utiliser l’instantané comme source d’une sauvegarde. Ou vous pouvez définir la base de données pour qu’elle se donne un nouvel ID d’appel à l’aide de la procédure décrite dans la section [restauration d’un contrôleur de domaine virtuel lorsqu’une sauvegarde de données d’État du système appropriée n’est pas disponible](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available) .

## <a name="next-steps"></a>Étapes suivantes

* Pour plus d’informations sur la résolution des problèmes liés aux contrôleurs de domaine virtualisés, voir [Résolution des problèmes des contrôleurs de domaine virtualisés](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).
* [Informations détaillées sur le service de temps Windows (W32Time)](../../networking/windows-time-service/windows-time-service-top.md)
