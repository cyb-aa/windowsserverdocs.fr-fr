---
title: Configuration d’une protection LSA supplémentaire
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 038e7c2b-c032-491f-8727-6f3f01116ef9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: eaebac19119525b659c09b5506c497afdbd9a263
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386991"
---
# <a name="configuring-additional-lsa-protection"></a>Configuration d’une protection LSA supplémentaire

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l’informatique explique comment configurer une protection supplémentaire pour le processus de l’autorité de sécurité locale (LSA) afin d’empêcher toute injection de code qui pourrait compromettre les informations d’identification.

L’autorité LSA, qui comprend le processus LSASS (Local Security Authority Server Service), valide les utilisateurs pour les connexions locales et à distance, et applique les stratégies de sécurité locales. Le système d’exploitation Windows 8.1 fournit une protection supplémentaire pour l’autorité LSA afin d’empêcher la lecture de la mémoire et l’injection de code par des processus non protégés. La sécurité des informations d’identification stockées et gérées par l’autorité LSA est ainsi renforcée. Le paramètre de processus protégé pour l’autorité LSA peut être configuré dans Windows 8.1, mais il ne peut pas être configuré dans Windows RT 8,1. Lorsque ce paramètre est utilisé avec le démarrage sécurisé, une protection supplémentaire est obtenue car la désactivation de la clé de Registre HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa n’a aucun effet.

### <a name="protected-process-requirements-for-plug-ins-or-drivers"></a>Exigences spécifiques au processus protégé pour les plug-ins ou pilotes
Pour qu’un plug-in ou pilote LSA soit correctement chargé en tant que processus protégé, il doit répondre aux critères suivants :

1.  Vérification de signature

    Le mode protégé exige que tout plug-in chargé dans l’autorité LSA soit signé numériquement avec une signature Microsoft. Par conséquent, tout plug-in sans signature ou non signé avec une signature Microsoft ne pourra pas être chargé dans l’autorité LSA. Ces plug-ins sont notamment les pilotes de cartes à puce, les plug-ins de chiffrement et les filtres de mots de passe.

    Les plug-ins LSA qui sont des pilotes, tels que les pilotes de cartes à puce, doivent être signés à l’aide de la certification WHQL. Pour plus d’informations, consultez [signature de version WHQL](https://msdn.microsoft.com/library/windows/hardware/ff553976%28v=vs.85%29.aspx).

    Les plug-ins LSA sans processus de certification WHQL doivent être signés à l’aide du [service de signature de fichiers pour LSA](https://go.microsoft.com/fwlink/?LinkId=392590).

2.  Respect du guide de processus SDL (Security Development Lifecycle) Microsoft

    Tous les plug-ins doivent être conformes au guide de processus SDL applicable. Pour plus d'informations, consultez l' [Annexe SDL (Security Development Lifecycle) Microsoft](https://msdn.microsoft.com/library/windows/desktop/cc307891.aspx).

    Même si les plug-ins sont correctement signés avec une signature Microsoft, la non-conformité avec le processus SDL peut aboutir à l’échec du chargement d’un plug-in.

#### <a name="recommended-practices"></a>Pratiques recommandées
Utilisez la liste suivante pour vérifier soigneusement que la protection LSA est activée avant de déployer globalement la fonctionnalité :

-   Identifiez tous les plug-ins et pilotes LSA qui sont utilisés au sein de votre organisation. 
    Cela inclut les pilotes ou plug-ins non-Microsoft, tels que les pilotes de cartes à puce et les plug-ins de chiffrement, ainsi que tout logiciel développé en interne qui est utilisé pour appliquer des filtres de mots de passe ou notifications de changement de mot de passe.

-   Vérifiez que tous les plug-ins LSA sont signés numériquement avec un certificat Microsoft pour que le chargement du plug-in réussisse.

-   Vérifiez que tous les plug-ins correctement signés peuvent être chargés dans l’autorité LSA et qu’ils fonctionnent comme prévu.

-   Utilisez les journaux d’audit pour identifier les plug-ins et pilotes LSA dont l’exécution en tant que processus protégé échoue.

#### <a name="limitations-introduced-with-enabled-lsa-protection"></a>Limitations introduites avec la protection LSA activée

Si la protection LSA est activée, vous ne pouvez pas déboguer un plug-in LSA personnalisé.
Vous ne pouvez pas attacher un débogueur à LSASS lorsqu’il s’agit d’un processus protégé.
En général, il n’existe aucune méthode prise en charge pour déboguer un processus protégé en cours d’exécution.

## <a name="how-to-identify-lsa-plug-ins-and-drivers-that-fail-to-run-as-a-protected-process"></a>Comment identifier les plug-ins et pilotes LSA dont l’exécution en tant que processus protégé échoue
Les événements décrits dans cette section figurent dans le journal des opérations sous Journaux des applications et des services\Microsoft\Windows\CodeIntegrity. Ils peuvent vous aider à identifier les plug-ins et pilotes LSA dont le chargement échoue pour des raisons de signature. Pour gérer ces événements, vous pouvez utiliser l’outil en ligne de commande **wevtutil** . Pour plus d’informations sur cet outil, voir [Wevtutil](../../administration/windows-commands/Wevtutil.md).

### <a name="before-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Avant de vous inscrire : comment identifier les plug-ins et pilotes chargés par lsass.exe
Vous pouvez utiliser le mode audit pour identifier les plug-ins et pilotes LSA dont le chargement échouera en mode de protection LSA. En mode audit, le système génère des journaux des événements qui identifient tous les plug-ins et pilotes qui ne pourront pas être chargés sous LSA si la protection LSA est activée. Les messages sont consignés sans bloquer les plug-ins ni pilotes.

##### <a name="to-enable-the-audit-mode-for-lsassexe-on-a-single-computer-by-editing-the-registry"></a>Pour activer le mode audit pour Lsass.exe sur un seul ordinateur en modifiant le Registre

1.  Ouvrez l'Éditeur du Registre (RegEdit.exe) et accédez à la clé de Registre qui se trouve à l'emplacement suivant : HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe.

2.  Définissez **AuditLevel=dword:00000008** comme valeur de la clé de Registre.

3.  Redémarrez l’ordinateur.

Analysez les résultats de l’événement 3065 et de l’événement 3066.

Après cela, vous pouvez voir ces événements dans observateur d’événements : Microsoft-Windows-CodeIntegrity/Operational :

-   **Événement 3065** : cet événement rapporte qu'une vérification de l'intégrité du code a déterminé qu'un processus (généralement lsass.exe) a tenté de charger un pilote spécifique qui ne répondait pas aux conditions requises en matière de sécurité pour les sections partagées. Toutefois, en raison de la stratégie système qui est définie, le chargement de l’image a été autorisé.

-   **Événement 3066** : cet événement rapporte qu'une vérification de l'intégrité du code a déterminé qu'un processus (généralement lsass.exe) a tenté de charger un pilote spécifique qui ne répondait pas aux conditions requises en matière de niveau de signature Microsoft. Toutefois, en raison de la stratégie système qui est définie, le chargement de l’image a été autorisé.

> [!IMPORTANT]
> Ces événements opérationnels ne sont pas générés lorsqu’un débogueur du noyau est attaché et activé sur un système.
> 
> Si un plug-in ou pilote contient des sections partagées, l’événement 3066 est consigné avec l’événement 3065. La suppression des sections partagées doit empêcher les deux événements de survenir, à moins que le plug-in ne réponde pas aux exigences de niveau de signature Microsoft.

Pour activer le mode audit pour plusieurs ordinateurs d’un domaine, vous pouvez utiliser l’extension de Registre côté client pour la stratégie de groupe afin de déployer la valeur du Registre de niveau d’audit Lsass.exe. Vous devez modifier la clé de Registre HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe.

##### <a name="to-create-the-auditlevel-value-setting-in-a-gpo"></a>Pour créer le paramètre de valeur AuditLevel dans un objet de stratégie de groupe

1.  Ouvrez la Console de gestion des stratégies de groupe (GPMC, Group Policy Management Console).

2.  Créez un objet de stratégie de groupe qui est lié au niveau du domaine ou qui est lié à l’unité d’organisation qui contient vos comptes d’ordinateurs. Vous pouvez également sélectionner un objet de stratégie de groupe qui est déjà déployé.

3.  Cliquez avec le bouton droit sur l’objet de stratégie de groupe, puis cliquez sur **Modifier** pour ouvrir l’Éditeur de gestion des stratégies de groupe.

4.  Développez **Configuration ordinateur**, **Préférences**, puis **Paramètres Windows**.

5.  Cliquez avec le bouton droit sur **Registre**, pointez sur **Nouveau**, puis cliquez sur **Élément Registre**. La boîte de dialogue **Nouvelles propriétés de Registre** s’affiche.

6.  Dans la liste **Ruche** , cliquez sur **HKEY_LOCAL_MACHINE.**

7.  Dans la liste **Chemin d'accès à la clé**, recherchez **SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe**.

8.  Dans la zone **Nom de la valeur** , tapez **AuditLevel**.

9. Dans la zone **Type de la valeur** , cliquez pour sélectionner **REG_DWORD**.

10. Dans la zone **Données de la valeur** , tapez **00000008**.

11. Cliquez sur **OK**.

> [!NOTE]
> Pour que l’objet de stratégie de groupe entre en vigueur, la modification qui lui est apportée doit être répliquée vers tous les contrôleurs du domaine.

Pour vous inscrire pour une protection LSA supplémentaire sur plusieurs ordinateurs, vous pouvez utiliser l’extension de Registre côté client pour la stratégie de groupe en modifiant HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa. Pour connaître la procédure à suivre, consultez [Comment configurer une protection LSA supplémentaire des informations d’identification](#BKMK_HowToConfigure) dans cette rubrique.

### <a name="after-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Après vous être inscrit : comment identifier les plug-ins et pilotes chargés par lsass.exe
Vous pouvez utiliser le journal des événements pour identifier les plug-ins et pilotes LSA dont le chargement a échoué en mode de protection LSA. Lorsque le processus protégé LSA est activé, le système génère des journaux des événements qui identifient tous les plug-ins et pilotes qui n’ont pas pu être chargés sous LSA.

Analysez les résultats de l’événement 3033 et de l’événement 3063.

Après cela, vous pouvez voir ces événements dans observateur d’événements : Microsoft-Windows-CodeIntegrity/Operational :

-   **Événement 3033** : cet événement rapporte qu'une vérification de l'intégrité du code a déterminé qu'un processus (généralement lsass.exe) a tenté de charger un pilote qui ne répondait pas aux conditions requises en matière de niveau de signature Microsoft.

-   **Événement 3063** : cet événement rapporte qu'une vérification de l'intégrité du code a déterminé qu'un processus (généralement lsass.exe) a tenté de charger un pilote qui ne répondait pas aux conditions requises en matière de sécurité pour les sections partagées.

Les sections partagées sont généralement le résultat de techniques de programmation qui permettent aux données d’instance d’interagir avec d’autres processus qui utilisent le même contexte de sécurité. Il peut en résulter des failles de sécurité.

## <a name="BKMK_HowToConfigure"></a>Comment configurer une protection LSA supplémentaire des informations d’identification
Sur les appareils exécutant Windows 8.1 (avec ou sans démarrage sécurisé ou UEFI), la configuration est possible en effectuant les procédures décrites dans cette section. Pour les appareils exécutant Windows RT 8,1, la protection lsass. exe est toujours activée et ne peut pas être désactivée.

### <a name="on-x86-based-or-x64-based-devices-using-secure-boot-and-uefi-or-not"></a>Sur les appareils x86 ou x64 avec ou sans démarrage sécurisé et UEFI
Sur les appareils x86 ou x64 qui utilisent le démarrage sécurisé et UEFI, une variable UEFI est définie dans le microprogramme UEFI lorsque la protection LSA est activée à l’aide de la clé de Registre. Lorsque le paramètre est stocké dans le microprogramme, la variable UEFI ne peut pas être supprimée ni modifiée dans la clé de Registre. La variable UEFI doit être réinitialisée.

Les appareils x86 ou x64 qui ne prennent pas en charge UEFI ni le démarrage sécurisé sont désactivés, ne peuvent pas stocker la configuration pour la protection LSA dans le microprogramme et comptent uniquement sur la présence de la clé de Registre. Dans ce scénario, il est possible de désactiver la protection LSA en utilisant l’accès à distance à l’appareil.

Vous pouvez utiliser les procédures suivantes pour activer ou désactiver la protection LSA :

##### <a name="to-enable-lsa-protection-on-a-single-computer"></a>Pour activer la protection LSA sur un seul ordinateur

1.  Ouvrez l'Éditeur du Registre (RegEdit.exe) et accédez à la clé de Registre qui se trouve à l'emplacement suivant : HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Affectez la valeur suivante à la clé de Registre : "RunAsPPL"=dword:00000001.

3.  Redémarrez l’ordinateur.

##### <a name="to-enable-lsa-protection-using-group-policy"></a>Pour activer la protection LSA à l’aide d’une stratégie de groupe

1.  Ouvrez la Console de gestion des stratégies de groupe (GPMC, Group Policy Management Console).

2.  Créez un objet de stratégie de groupe qui est lié au niveau du domaine ou qui est lié à l’unité d’organisation qui contient vos comptes d’ordinateurs. Vous pouvez également sélectionner un objet de stratégie de groupe qui est déjà déployé.

3.  Cliquez avec le bouton droit sur l’objet de stratégie de groupe, puis cliquez sur **Modifier** pour ouvrir l’Éditeur de gestion des stratégies de groupe.

4.  Développez **Configuration ordinateur**, **Préférences**, puis **Paramètres Windows**.

5.  Cliquez avec le bouton droit sur **Registre**, pointez sur **Nouveau**, puis cliquez sur **Élément Registre**. La boîte de dialogue **Nouvelles propriétés de Registre** s’affiche.

6.  Dans la liste **Ruche**, cliquez sur **HKEY_LOCAL_MACHINE**.

7.  Dans la liste **Chemin d’accès à la clé** , recherchez **SYSTEM\CurrentControlSet\Control\Lsa**.

8.  Dans la zone **Nom de la valeur**, tapez **RunAsPPL**.

9. Dans la zone **Type de la valeur** , cliquez sur **REG_DWORD**.

10. Dans la zone **Données de la valeur** , tapez **00000001**.

11. Cliquez sur **OK**.

##### <a name="to-disable-lsa-protection"></a>Pour désactiver la protection LSA

1.  Ouvrez l'Éditeur du Registre (RegEdit.exe) et accédez à la clé de Registre qui se trouve à l'emplacement suivant : HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Supprimez la valeur suivante dans la clé de Registre : "RunAsPPL"=dword:00000001.

3.  Utilisez l'outil d'exclusion de processus protégé LSA pour supprimer la variable UEFI si l'appareil utilise le démarrage sécurisé.

    Pour plus d'informations sur l'outil d'exclusion, voir [Télécharger l'outil d'exclusion de processus protégé LSA du Centre de téléchargement Microsoft officiel](https://www.microsoft.com/download/details.aspx?id=40897).

    Pour plus d’informations sur la gestion du démarrage sécurisé, voir [Microprogramme UEFI](https://technet.microsoft.com/library/hh824898.aspx).

    > [!WARNING]
    > Lorsque le démarrage sécurisé est désactivé, toutes les configurations liées au démarrage sécurisé et à UEFI sont réinitialisées. Vous ne devez désactiver le démarrage sécurisé que lorsque tous les autres moyens de désactivation de la protection LSA ont échoué.

### <a name="verifying-lsa-protection"></a>Vérification de la protection LSA
Pour déterminer si LSA a démarré en mode protégé au démarrage de Windows, recherchez l’événement WinInit suivant dans le journal **Système** sous **Journaux Windows** :

-   12 : LSASS.exe a démarré en tant que processus protégé avec le niveau : 4

## <a name="additional-resources"></a>Ressources supplémentaires
[Gestion et protection des informations d'identification](credentials-protection-and-management.md)

[Service de signature de fichiers pour LSA](https://go.microsoft.com/fwlink/?LinkId=392590)


