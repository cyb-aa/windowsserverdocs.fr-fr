---
title: Virtualisation des Services de domaine Active Directory (AD DS) en toute sécurité
description: La restauration USN et virtualisation sécurisée d’Active Directory
ms.topic: article
ms.prod: windows-server-threshold
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: aa84e09e8a958193fee82c7b9c03cd1dca910c55
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63684185"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>Virtualisation des Services de domaine Active Directory (AD DS) en toute sécurité

>S'applique à : Windows Server

À compter de Windows Server 2012, les services AD DS fournissent une prise en charge pour la virtualisation de contrôleurs de domaine en introduisant des fonctionnalités de virtualisation-safe. Cet article explique le rôle de numéros USN et InvocationIDs dans la réplication de contrôleur de domaine et présente certains problèmes potentiels qui peuvent se produire.

## <a name="update-sequence-number-and-invocationid"></a>Numéro de séquence de mise à jour et les ID d’invocation

Les environnements virtuels posent des défis uniques pour les charges de travail distribuées qui dépendent d’un schéma de réplication fondé sur un mécanisme d’horloge logique. Par exemple, la réplication AD DS utilise une valeur qui augmente de manière monotone (appelée « USN » ou « numéro de séquence de mise à jour ») et est affectée aux transactions sur chaque contrôleur de domaine. Instance de base de données de chaque contrôleur de domaine est également attribuer une identité, appelée un ID d’invocation. La valeur InvocationID d’un contrôleur de domaine et sa valeur USN jouent ensemble le rôle d’un identificateur unique associé à chaque transaction en écriture réalisée dans chaque contrôleur de domaine et doivent être uniques au sein de la forêt.

La réplication AD DS exploite des valeurs InvocationID et USN sur chaque contrôleur de domaine afin de déterminer les changements à répliquer sur d’autres contrôleurs de domaine. Si un contrôleur de domaine est restauré à temps en dehors de la reconnaissance du contrôleur de domaine et un USN est réutilisé pour une transaction entièrement différente, la réplication ne fera aucun lien, car les autres contrôleurs de domaine penseront qu’ils ont déjà reçu les mises à jour associé à l’USN réutilisé dans le contexte de cette valeur InvocationID.

Par exemple, l’illustration suivante décrit la séquence des événements qui ont lieu dans Windows Server 2008 R2 et des systèmes d’exploitation antérieurs lorsqu’une restauration USN est détectée sur VDC2, le contrôleur de domaine de destination exécuté sur un ordinateur virtuel. Dans cette illustration, la détection de restauration USN se produit sur VDC2 lorsqu’un partenaire de réplication détecte que VDC2 a envoyé une valeur USN de mise à jour qui a été vu précédemment par le partenaire de réplication, ce qui indique que base de données de VDC2 a été restaurée de manière incorrecte.

![La séquence des événements lors de la restauration USN est détectée](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Une machine virtuelle (VM) rend plus facile pour les administrateurs d’hyperviseurs à restaurer un domaine USN du contrôleur (son horloge logique) par, par exemple, application d’un instantané en dehors de la reconnaissance du contrôleur de domaine. Pour obtenir des informations sur les numéros USN et la restauration USN et consulter une autre illustration décrivant les instances de restauration USN non détectées, voir les sections [USN et Restauration USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

À compter de Windows Server 2012, les contrôleurs de domaine virtuels AD DS hébergés sur des plateformes d’hyperviseur qui dévoilent un identificateur appelé ID de génération d’ordinateur virtuel peuvent détecter et employer des mesures de sécurité nécessaires pour protéger l’environnement AD DS si l’ordinateur virtuel est restauré. dans le temps par l’application d’un instantané de machine virtuelle. La structure de l’ID de génération d’ordinateur virtuel repose sur un mécanisme hyperviseur/fournisseur indépendant qui présente l’identificateur dans l’espace d’adressage de l’ordinateur virtuel invité, de sorte que l’expérience de virtualisation sécurisée reste systématiquement disponible depuis chaque hyperviseur prenant en charge les ID de génération d’ordinateur virtuel. Cet identificateur peut être testé par les services et les applications en cours d’exécution sur l’ordinateur virtuel afin de détecter si un ordinateur virtuel a été restauré à temps.

## <a name="effects-of-usn-rollback"></a>Effets de restauration USN

En cas de restaurations USN, les modifications apportées aux objets et attributs ne sont pas répliquées par les contrôleurs de domaine de destination qui ont déjà été vu l’USN.

Étant donné que ces contrôleurs de domaine de destination pensent qu’ils sont à jour, aucune erreur de réplication n’est signalés dans les journaux des événements de Service d’annuaire ou par les outils de surveillance et de Diagnostics.

La restauration USN peut affecter la réplication d’un objet ou un attribut dans n’importe quelle partition. L’effet secondaire plus fréquemment observé est que les comptes d’utilisateurs et comptes d’ordinateurs qui sont créés sur le contrôleur de domaine de restauration n’existent pas sur un ou plusieurs partenaires de réplication. Ou bien, les mises à jour de mot de passe qui avait été créée sur le contrôleur de domaine de restauration n’existent pas sur les partenaires de réplication.

Une restauration USN peut empêcher tout type d’objet dans n’importe quelle partition Active Directory de réplication. Ces types d’objets sont les suivantes :

* La topologie de réplication Active Directory et la planification
* L’existence de contrôleurs de domaine dans la forêt et les rôles qui contiennent ces contrôleurs de domaine
* L’existence de partitions de domaine et d’application dans la forêt
* L’existence de groupes de sécurité et de leur appartenance aux groupes en cours
* L’inscription d’enregistrement DNS dans les zones DNS intégrées à Active Directory

La taille du trou USN peut représenter des centaines ou des milliers voire des dizaines de milliers de modifications pour les utilisateurs, ordinateurs, des approbations, les mots de passe et les groupes de sécurité. Le trou de l’USN est défini par la différence entre le nombre le plus élevé USN qui existe au moment où la sauvegarde d’état de restauration du système a été effectuée et le nombre de d’origine des modifications qui ont été créés sur le contrôleur de domaine de l’échec de restauration avant qu’il a été mis hors connexion.

## <a name="detecting-a-usn-rollback"></a>Détection d’une restauration USN

Une restauration USN étant difficile à détecter, un contrôleur de domaine enregistre l’événement 2095 lorsqu’un contrôleur de domaine source envoie un numéro USN précédemment accusé de réception à un contrôleur de domaine de destination sans une modification correspondante dans le code d’appel.

Pour empêcher unique provenant de mises à jour dans Active Directory en cours de création sur le contrôleur de domaine incorrectement restauré, le service Net Logon est suspendu. Lorsque le service Net Logon est interrompu, comptes d’utilisateur et ordinateur ne peut pas modifier le mot de passe sur un contrôleur de domaine qui ne sera pas une réplication sortante ces modifications. De même, les outils d’administration Active Directory seront mieux adaptées un contrôleur de domaine intègre lorsqu’ils effectuent des mises à jour aux objets dans Active Directory.

Sur un contrôleur de domaine, les messages d’événements semblables aux suivants sont enregistrés si les conditions suivantes sont remplies :

* Un contrôleur de domaine source envoie un numéro USN précédemment accusé de réception à un contrôleur de domaine de destination.
* Il n’existe aucune modification correspondante dans le code d’appel.

Ces événements peuvent être capturées dans le journal des événements Service d’annuaire. Toutefois, ils peuvent être remplacées avant qu’ils sont observées par un administrateur.

Si vous suspectez une restauration USN s’est produite, mais ne voyez pas un événement correspondant dans le cas des journaux, recherchez l’entrée DSA pas accessible en écriture dans le Registre. Cette entrée fournit des preuves qu’une restauration USN s’est produite.

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> Supprimer ou de modifier manuellement la valeur d’entrée de Registre Dsa pas accessible en écriture place le contrôleur de domaine de restauration dans un état définitivement non pris en charge. Par conséquent, ces modifications ne sont pas pris en charge. Plus précisément, la modification de la valeur supprime le comportement de mise en quarantaine ajouté par le code de détection de restauration USN. Les partitions Active Directory sur le contrôleur de domaine de restauration seront définitivement incohérentes avec les partenaires de réplication direct et transitif dans la même forêt Active Directory.

Vous trouverez plus d’informations sur cette procédure de clé et la résolution du Registre dans l’article du support [Active Directory Replication erreur 8456 ou 8457 : « La source | serveur de destination rejette actuellement les demandes de réplication »](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination).

## <a name="virtualization-based-safeguards"></a>Les dispositifs de protection en fonction de la virtualisation

Pendant l’installation du contrôleur de domaine, les services AD DS stocke d’abord l’identificateur de l’ID de génération de machine virtuelle dans le cadre de l’attribut msDS-GenerationID sur l’objet d’ordinateur du contrôleur de domaine dans sa base de données (souvent appelé l’arborescence d’information d’annuaire ou DIT). L’ID de génération d’ordinateur virtuel est contrôlé de manière indépendante par un pilote Windows au sein de l’ordinateur virtuel.

Lorsqu’un administrateur restaure l’ordinateur virtuel à partir d’une capture instantanée précédente, la valeur actuelle de l’ID de génération d’ordinateur virtuel extraite du pilote de l’ordinateur virtuel est comparée à une valeur au sein du DIT.

Si les deux valeurs sont différentes, l’attribut InvocationID est réinitialisé et le pool RID est rejeté, ce qui empêche toute réutilisation des numéros USN. Si les valeurs sont identiques, la transaction est validée comme une transaction normale.

Les services de domaine Active Directory (AD DS) comparent également la valeur actuelle de l’ID de génération d’ordinateur virtuel provenant de l’ordinateur virtuel à la valeur interne du DIT chaque fois que le contrôleur de domaine est redémarré. S’ils trouvent une valeur différente, ils réinitialisent l’attribut InvocationID, rejettent le pool RID et mettent à jour le DIT pour y inclure la nouvelle valeur. Ils procèdent également à une synchronisation ne faisant pas autorité du dossier SYSVOL pour mener la restauration à terme, en toute sécurité. Les mécanismes de protection peuvent ainsi étendre l’application des captures instantanées sur des ordinateurs virtuels qui ont été arrêtés. Ces mécanismes de protection introduits dans Windows Server 2012 permettent aux administrateurs de services AD DS bénéficier d’avantages uniques inhérents au déploiement et la gestion des contrôleurs de domaine dans un environnement virtualisé.

L’illustration suivante montre comment les dispositifs de protection sont appliqués lorsque la même restauration USN est détectée sur un contrôleur de domaine virtualisé qui exécute Windows Server 2012 sur un hyperviseur qui prend en charge VM-GenerationID.

![Dispositifs de protection appliqués lorsque la même restauration USN est détectée](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

Dans ce cas, dès que l’hyperviseur détecte que la valeur de l’ID de génération d’ordinateur virtuel a été modifiée, les mécanismes de protection de la virtualisation sont déclenchés, y compris la réinitialisation de l’attribut InvocationID pour le contrôleur de domaine virtualisé (de A à B dans l’exemple précédent) et la mise à jour de la valeur de l’ID de génération d’ordinateur virtuel enregistrée sur l’ordinateur virtuel pour correspondre à la nouvelle valeur (G2) stockée par l’hyperviseur. Les mécanismes de protection de la virtualisation s’assurent que la réplication converge pour les deux contrôleurs de domaine.

Avec Windows Server 2012, les services AD DS utilise des dispositifs de protection sur les contrôleurs de domaine virtuels hébergés sur des hyperviseurs prenant en charge VM-GenerationID et garantit que l’application accidentelle de captures instantanées ou d’autres ces mécanismes prenant en charge l’hyperviseur peut restaurer une machine virtuelle état de l’ordinateur n’interrompt pas l’environnement AD DS (en empêchant les problèmes de réplication comme une bulle USN ou des objets en attente).

Restauration d’un contrôleur de domaine en appliquant un instantané de machine virtuelle n’est pas recommandée en tant qu’un autre mécanisme pour la sauvegarde d’un contrôleur de domaine. Nous vous recommandons de continuer à utiliser la fonctionnalité Sauvegarde Windows Server ou d’autres solutions de sauvegarde fondées sur l’enregistreur VSS.

> [!CAUTION]
> Si un instantané est rétabli accidentellement un contrôleur de domaine dans un environnement de production, il est recommandé que vous consultiez les éditeurs pour les applications et les services hébergés sur cette machine virtuelle, pour obtenir des conseils sur la vérification de l’état de ces programmes après restauration de capture instantanée.

Pour plus d‘informations, voir [Architecture de restauration sécurisée des contrôleurs de domaine virtualisés](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="recovering-from-a-usn-rollback"></a>Récupération à partir d’une restauration USN

Il existe deux approches pour récupérer à partir d’une restauration USN :

* Supprimer le contrôleur de domaine du domaine
* Restaurer l’état du système d’une sauvegarde correcte

### <a name="remove-the-domain-controller-from-the-domain"></a>Supprimer le contrôleur de domaine du domaine

1. Supprimez Active Directory à partir du contrôleur de domaine pour le forcer à être un serveur autonome.
2. Arrêter le serveur rétrogradé.
3. Sur un contrôleur de domaine intègre, nettoyez les métadonnées du contrôleur de domaine rétrogradé.
4. Si les rôles de maître d’opérations hôtes de contrôleur de domaine incorrectement restauré, transférez ces rôles à un contrôleur de domaine intègre.
5. Redémarrez le serveur rétrogradé.
6. Si vous devez, réinstallez Active Directory sur le serveur autonome.
7. Si le contrôleur de domaine était auparavant un catalogue global, configurez le contrôleur de domaine pour être un catalogue global.
8. Si le contrôleur de domaine hébergeait précédemment des rôles de maître d’opérations, retransférez ces rôles de maître sauvegarder sur le contrôleur de domaine.

### <a name="restore-the-system-state-of-a-good-backup"></a>Restaurer l’état du système d’une sauvegarde correcte

Déterminez si les sauvegardes d’état du système valide existent pour ce contrôleur de domaine. Si une sauvegarde de l’état système valide a été effectuée avant que le contrôleur de domaine de l’échec de restauration a été restauré correctement et que la sauvegarde contient des modifications récentes ont été effectuées sur le contrôleur de domaine, restaurez l’état du système à partir de la sauvegarde la plus récente.

Vous pouvez également utiliser la capture instantanée en tant que source d’une sauvegarde. Ou vous pouvez définir la base de données pour qu’elle s’attribue un nouvel ID d’appel à l’aide de la procédure dans la section [restauration d’un contrôleur de domaine virtuel lorsqu’une sauvegarde de données d’état système approprié n’est pas disponible](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available)

## <a name="next-steps"></a>Étapes suivantes

* Pour plus d’informations sur la résolution des problèmes liés aux contrôleurs de domaine virtualisés, voir [Résolution des problèmes des contrôleurs de domaine virtualisés](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).
* [Informations détaillées sur le Service de temps Windows (W32Time)](../../networking/windows-time-service/windows-time-service-top.md)
