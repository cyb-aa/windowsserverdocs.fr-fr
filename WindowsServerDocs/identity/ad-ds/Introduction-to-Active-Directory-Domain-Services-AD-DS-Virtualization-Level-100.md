---
title: Virtualisation sécurisée des services de domaine Active Directory (AD DS)
description: Restauration USN et virtualisation sécurisée d’Active Directory
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
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391026"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>Virtualisation sécurisée des services de domaine Active Directory (AD DS)

>S'applique à : Windows Server

Depuis Windows Server 2012, AD DS offre une prise en charge accrue de la virtualisation des contrôleurs de domaine grâce à l’introduction de technologies sécurisées pour la virtualisation. Cet article explique le rôle que les USN et les InvocationID jouent dans la réplication du contrôleur de domaine et décrit certains problèmes potentiels qui peuvent se produire.

## <a name="update-sequence-number-and-invocationid"></a>Mettre à jour le numéro de séquence et InvocationID

Les environnements virtuels posent des défis uniques pour les charges de travail distribuées qui dépendent d’un schéma de réplication fondé sur un mécanisme d’horloge logique. Par exemple, la réplication AD DS utilise une valeur qui augmente de manière monotone (appelée « USN » ou « numéro de séquence de mise à jour ») et est affectée aux transactions sur chaque contrôleur de domaine. Chaque instance de base de données du contrôleur de domaine se voit également attribuer une identité appelée « InvocationID ». La valeur InvocationID d’un contrôleur de domaine et sa valeur USN jouent ensemble le rôle d’un identificateur unique associé à chaque transaction en écriture réalisée dans chaque contrôleur de domaine et doivent être uniques au sein de la forêt.

La réplication AD DS exploite des valeurs InvocationID et USN sur chaque contrôleur de domaine afin de déterminer les changements à répliquer sur d’autres contrôleurs de domaine. Si un contrôleur de domaine est restauré à temps sans que le contrôleur de domaine en ait connaissance et si un USN est réutilisé pour une transaction complètement différente, la réplication ne fera aucun lien puisque les autres contrôleurs de domaine penseront qu’ils ont déjà reçu les mises à jour associées au numéro USN réutilisé dans le contexte de cette valeur InvocationID.

Par exemple, l’illustration suivante décrit la séquence des événements qui ont lieu dans Windows Server 2008 R2 et des systèmes d’exploitation antérieurs lorsqu’une restauration USN est détectée sur VDC2, le contrôleur de domaine de destination exécuté sur un ordinateur virtuel. Dans cette illustration, la détection de la restauration USN se produit sur VDC2 lorsqu’un partenaire de réplication détecte que VDC2 a envoyé une valeur USN de mise à jour déjà vue par le partenaire de réplication, ce qui signifie qu’une version antérieure erronée de la base de données de VDC2 a été restaurée.

![Séquence d’événements lorsque la restauration USN est détectée](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Un ordinateur virtuel permet aux administrateurs d’hyperviseurs de restaurer les valeurs USN (l’horloge logique) d’un contrôleur de domaine, notamment en appliquant une capture instantanée sans que ce dernier ne le sache. Pour obtenir des informations sur les numéros USN et la restauration USN et consulter une autre illustration décrivant les instances de restauration USN non détectées, voir les sections [USN et Restauration USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

Depuis Windows Server 2012, les contrôleurs de domaine virtuels AD DS hébergés sur des plateformes d’hyperviseur qui dévoilent un identificateur appelé « ID de génération d’ordinateur virtuel » peuvent détecter et employer des mesures de sécurité nécessaires à la protection de l’environnement AD DS si l’ordinateur virtuel est restauré à temps par réalisation d’une capture instantanée d’ordinateur virtuel. La structure de l’ID de génération d’ordinateur virtuel repose sur un mécanisme hyperviseur/fournisseur indépendant qui présente l’identificateur dans l’espace d’adressage de l’ordinateur virtuel invité, de sorte que l’expérience de virtualisation sécurisée reste systématiquement disponible depuis chaque hyperviseur prenant en charge les ID de génération d’ordinateur virtuel. Cet identificateur peut être testé par les services et les applications en cours d’exécution sur l’ordinateur virtuel afin de détecter si un ordinateur virtuel a été restauré à temps.

## <a name="effects-of-usn-rollback"></a>Effets de la restauration USN

Lors d’une restauration USN, les modifications apportées aux objets et aux attributs ne sont pas répliquées en entrée par les contrôleurs de domaine de destination ayant déjà vu l’USN.

Étant donnée que ces contrôleurs de domaine de destination pensent qu’ils sont à jour, aucune erreur de réplication n’est signalée dans les journaux des événements du service de répertoire ou via les outils de supervision et de diagnostics.

La restauration USN peut avoir une incidence sur la réplication d’un objet ou d’un attribut dans n’importe quelle partition. L’effet secondaire le plus fréquent est que les comptes d’utilisateurs et d’ordinateurs créés sur le contrôleur de domaine de restauration n’existent pas sur un ou plusieurs partenaires de réplication. Dans certains cas, les mises à jour de mot de passe provenant du contrôleur de domaine de restauration n’existent pas sur les partenaires de réplication.

Une restauration USN peut empêcher la réplication d’un type d’objet dans une partition de Active Directory. Ces types de données incluent :

* La topologie et le calendrier de réplication d’Active Directory
* L’existence de contrôleurs de domaine dans la forêt et les rôles qu’ils occupent
* L’existence de partitions de domaine et d’application dans la forêt
* L’existence de groupes de sécurité et leur appartenance à des groupes actuels
* Inscription des enregistrements DNS dans les zones DNS intégrées à Active Directory

La taille du trou USN peut représenter des centaines, des milliers, voire des dizaines de milliers de modifications pour les utilisateurs, les ordinateurs, les approbations, les mots de passe et les groupes de sécurité. Le trou USN correspond à la différence entre le numéro USN le plus élevé qui existait lors de la sauvegarde de l’état du système restauré et le nombre de modifications d’origine qui ont été créées sur le contrôleur de domaine restauré avant qu’il ne soit mis hors connexion.

## <a name="detecting-a-usn-rollback"></a>Détection d’une restauration USN

Étant donné qu’il est difficile de détecter une restauration USN, un contrôleur de domaine enregistre l’événement 2095 lorsqu’un contrôleur de domaine source envoie un numéro USN précédemment reconnu à un contrôleur de domaine de destination sans modification correspondante de l’ID d’invocation.

Pour éviter la création de mises à jour d’origine uniques Active Directory sur le contrôleur de domaine mal restauré, le service Accès réseau est interrompu. Lorsque le service Accès réseau est interrompu, les comptes d’utilisateurs et d’ordinateurs ne peuvent pas modifier le mot de passe sur un contrôleur de domaine qui n’effectue pas de réplication sortante de ces modifications. De même, les outils d’administration Active Directory privilégient un contrôleur de domaine sain lorsqu’ils effectuent des mises à jour d’objets dans Active Directory.

Sur un contrôleur de domaine, les messages d’événement qui ressemblent à ce qui suit sont enregistrés si les conditions suivantes sont remplies :

* Un contrôleur de domaine source envoie un numéro USN précédemment reconnu à un contrôleur de domaine de destination.
* Il n’y a aucune modification correspondante dans l’ID d’invocation.

Ces événements peuvent être enregistrés dans le journal des événements du service de répertoire. Toutefois, ils peuvent être remplacés avant d’être examinés par un administrateur.

Si vous pensez qu’une restauration USN a été effectuée mais que vous ne voyez aucun événement correspondant dans les journaux des événements, recherchez l’entrée DSA non accessible en écriture dans le registre. Cette entrée vous fournit la preuve qu’une restauration USN a été effectuée.

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> Si vous supprimez ou la modifiez manuellement la valeur de l’entrée de registre DSA non accessible en écriture, le contrôleur de domaine de restauration sera en permanence dans un état non pris en charge. C’est pourquoi ces changements ne sont pas pris en charge. Plus précisément, la modification de la valeur supprime le comportement de quarantaine ajouté par le code de détection de la restauration USN. Les partitions Active Directory du contrôleur de domaine de restauration seront définitivement incohérentes avec les partenaires de réplication directs et transitives dans la même forêt Active Directory.

Pour plus d’informations sur cette clé de registre et les étapes de résolution, consultez l’article de support [Erreur de réplication 8456 ou 8457 Active Directory : « Le serveur source | de destination rejette actuellement les demandes de réplication »](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination).

## <a name="virtualization-based-safeguards"></a>Dispositif de protection basée sur la virtualisation

Lors de l’installation du contrôleur de domaine, AD DS stocke d’abord l’identificateur de génération d’ordinateur virtuel en tant que composant de l’attribut msDS-GenerationID dans l’objet ordinateur du contrôleur de domaine, à l’intérieur de sa base de données (on parle souvent d’arborescence d’information de répertoire ou de « DIT »). L’ID de génération d’ordinateur virtuel est contrôlé de manière indépendante par un pilote Windows au sein de l’ordinateur virtuel.

Lorsqu’un administrateur restaure l’ordinateur virtuel à partir d’une capture instantanée précédente, la valeur actuelle de l’ID de génération d’ordinateur virtuel extraite du pilote de l’ordinateur virtuel est comparée à une valeur au sein du DIT.

Si les deux valeurs sont différentes, l’attribut InvocationID est réinitialisé et le pool RID est rejeté, ce qui empêche toute réutilisation des numéros USN. Si les valeurs sont identiques, la transaction est validée comme une transaction normale.

Les services de domaine Active Directory (AD DS) comparent également la valeur actuelle de l’ID de génération d’ordinateur virtuel provenant de l’ordinateur virtuel à la valeur interne du DIT chaque fois que le contrôleur de domaine est redémarré. S’ils trouvent une valeur différente, ils réinitialisent l’attribut InvocationID, rejettent le pool RID et mettent à jour le DIT pour y inclure la nouvelle valeur. Ils procèdent également à une synchronisation ne faisant pas autorité du dossier SYSVOL pour mener la restauration à terme, en toute sécurité. Les mécanismes de protection peuvent ainsi étendre l’application des captures instantanées sur des ordinateurs virtuels qui ont été arrêtés. Ces dispositifs de protection introduits dans Windows Server 2012 permettent aux administrateurs AD DS de bénéficier des avantages uniques inhérents au déploiement et à la gestion des contrôleurs de domaine dans un environnement virtualisé.

L’illustration suivante montre comment les dispositifs de protection de la virtualisation sont appliqués lorsque la même restauration USN est détectée sur un contrôleur de domaine virtualisé qui exécute Windows Server 2012 sur un hyperviseur prenant en charge l’ID de génération d’ordinateur virtuel.

![Dispositifs de protection appliqués lorsque la même restauration USN est détectée](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

Dans ce cas, dès que l’hyperviseur détecte que la valeur de l’ID de génération d’ordinateur virtuel a été modifiée, les mécanismes de protection de la virtualisation sont déclenchés, y compris la réinitialisation de l’attribut InvocationID pour le contrôleur de domaine virtualisé (de A à B dans l’exemple précédent) et la mise à jour de la valeur de l’ID de génération d’ordinateur virtuel enregistrée sur l’ordinateur virtuel pour correspondre à la nouvelle valeur (G2) stockée par l’hyperviseur. Les mécanismes de protection de la virtualisation s’assurent que la réplication converge pour les deux contrôleurs de domaine.

Avec Windows Server 2012, AD DS applique des dispositifs de protection sur les contrôleurs de domaine virtuels hébergés sur des hyperviseurs prenant en charge l’ID de génération d’ordinateur virtuel, puis s’assure que toute application fortuite des captures instantanées ou de mécanismes similaires activés par hyperviseur susceptibles de restaurer l’état d’un ordinateur virtuel ne cause aucun dommage à l’environnement AD DS (en empêchant tout problème de réplication comme une bulle USN ou des objets en attente).

La restauration d’un contrôleur de domaine par application d’une capture instantanée d’ordinateur virtuel n’est pas conseillée et ne constitue pas un mécanisme de secours pour la sauvegarde d‘un contrôleur de domaine. Nous vous recommandons de continuer à utiliser la fonctionnalité Sauvegarde Windows Server ou d’autres solutions de sauvegarde fondées sur l’enregistreur VSS.

> [!CAUTION]
> Si un contrôleur de domaine d’un environnement de production revient accidentellement à l’état de capture instantanée, il est préférable que vous consultiez les fournisseurs d’applications et des services hébergés sur cet ordinateur virtuel pour obtenir des conseils sur la vérification de l’état de ces programmes après restauration de la capture instantanée.

Pour plus d‘informations, voir [Architecture de restauration sécurisée des contrôleurs de domaine virtualisés](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="recovering-from-a-usn-rollback"></a>Récupération à partir d’une restauration USN

Deux approches permettent d’effectuer une récupération à partir d’une restauration USN :

* Supprimez le contrôleur de domaine du domaine
* Restaurez l’état du système d’une sauvegarde correcte

### <a name="remove-the-domain-controller-from-the-domain"></a>Supprimez le contrôleur de domaine du domaine

1. Supprimez Active Directory du contrôleur de domaine pour le forcer à être un serveur autonome.
2. Arrêtez le serveur rétrogradé.
3. Sur un contrôleur de domaine sain, nettoyez les métadonnées du contrôleur de domaine rétrogradé.
4. Si le contrôleur de domaine restauré de manière incorrecte héberge des rôles de maître d’opérations, transférez-les vers un contrôleur de domaine sain.
5. Redémarrez le serveur rétrogradé.
6. Si nécessaire, réinstallez Active Directory sur le serveur autonome.
7. Si le contrôleur de domaine était auparavant un catalogue global, configurez le contrôleur de domaine en tant que catalogue global.
8. Si le contrôleur de domaine hébergeait auparavant des rôles de maître d’opérations, transférez-les rôles vers le contrôleur de domaine.

### <a name="restore-the-system-state-of-a-good-backup"></a>Restaurez l’état du système d’une sauvegarde correcte

Vérifiez s’il existe des sauvegardes de l’état du système valides pour ce contrôleur de domaine. Si une sauvegarde de l’état du système valide a été effectuée avant que le contrôleur de domaine ne soit incorrectement restauré et que la sauvegarde contient les modifications récentes apportées au contrôleur de domaine, restaurez l’état du système à partir de la sauvegarde la plus récente.

Vous pouvez également utiliser l’instantané comme source d’une sauvegarde. Vous pouvez également configurer la base de données pour qu’elle s’attribue un nouvel ID d’invocation. Pour cela, suivez la procédure décrite dans la section [Restaurer un contrôleur de domaine virtuel en l’absence de sauvegarde appropriée des données sur l’état du système](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available)

## <a name="next-steps"></a>Étapes suivantes

* Pour plus d’informations sur la résolution des problèmes liés aux contrôleurs de domaine virtualisés, voir [Résolution des problèmes des contrôleurs de domaine virtualisés](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).
* [Informations détaillées sur le service de temps Windows (W32Time)](../../networking/windows-time-service/windows-time-service-top.md)
