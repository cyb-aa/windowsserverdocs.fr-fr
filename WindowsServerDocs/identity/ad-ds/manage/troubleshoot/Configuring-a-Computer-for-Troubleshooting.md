---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: "Configuration d’un ordinateur pour la résolution des problèmes"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 915b8d1133b3bee050f5eedce9e7ac445f833048
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configuration d’un ordinateur pour la résolution des problèmes

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>Avant d’utiliser des techniques de dépannage avancé pour identifier et résoudre les problèmes d’ActiveDirectory, configurez vos ordinateurs pour la résolution des problèmes. Vous devez également avoir une compréhension de base <token>nextref_longhorincludes > résolution des problèmes des concepts, des procédures et des outils. </para>
    <para>Pour plus d’informations sur la surveillance des outils pour Windows Server2008, consultez le Step-by-Step Guide pour l’analyse des performances et la fiabilité dans Windows Server2008 (<externalLink><linkText>https://go.microsoft.com/fwlink/?LinkId=123737</linkText><linkUri>https://go.microsoft.com/fwlink/?LinkId=123737</linkUri></externalLink>).</para>
  </introduction>
  <section>
    <title>Tâches de configuration pour la résolution des problèmes</title>
    <content>
      <para>pour configurer votre ordinateur pour le dépannage des Services de domaine ActiveDirectory (ADDS), effectuez les tâches suivantes:</para>
      <para>
        <link xlink:href="#BKMK_2">installer Remote Server Administration Tools pour les services ADDS</link>
      </para>
      <para>
        <link xlink:href="#BKMK_3">configurer moniteur de fiabilité et performances</link>
      </para>
      <para>
        <link xlink:href="#BKMK_4">définir les niveaux d’enregistrement</link>
      </para>
    </content>
    <sections>
      <section address="BKMK_2">
        <title>Installer les outils d’Administration de serveur distant pour les services ADDS</title>
        <content>
          <para>lorsque vous installez les services ADDS pour créer un contrôleur de domaine, les outils d’administration que vous utilisez pour gérer les services ADDS sont installés automatiquement. Si vous souhaitez gérer à distance des contrôleurs de domaine à partir d’un ordinateur qui n’est pas un contrôleur de domaine, vous pouvez installer les outils d’administration sur un serveur membre qui exécute <token>nextref_longhorincludes > ou sur un ordinateur qui exécute WindowsVista avec Service Pack1 (SP1). Sur les serveurs membres qui sont en cours d’exécution <token>nextref_longhorincludes >, vous utilisez la fonctionnalité Outils d’Administration de serveur distant (RSAT) dans le Gestionnaire de serveur pour installer les outils de Services de domaine ActiveDirectory. RSAT remplace les outils de Support de Windows dans Windows Server2003. Vous pouvez également installer les outils de Services de domaine ActiveDirectory sur un ordinateur qui exécute WindowsVista avec Service Pack1 (SP1), téléchargez les outils sur cet ordinateur.</para>
          <para>pour plus d’informations sur l’installation de serveur distant, consultez <link xlink:href="610ba7d9-51b5-4e14-9232-0510a9091aba">l’installation Remote Server Administration Tools pour les services ADDS</link>.</para>
        </content>
      </section>
      <section address="BKMK_3">
        <title>Configurer le moniteur de fiabilité et performances</title>
        <content>
          <para>Windows Server2008 inclut la fiabilité de Windows et d’un analyseur de performances, ce qui est un composant logiciel enfichable MicrosoftManagement Console (MMC) qui combine les fonctionnalités des outils autonomes précédents, y compris les journaux de performances et les alertes, Server Performance Advisor et le Moniteur système. Ce composant logiciel enfichable fournit une interface graphique utilisateur (GUI) pour personnaliser les ensembles de collecteurs de données et des Sessions de suivi d’événements. </para>
          <para>moniteur de fiabilité et performances inclut également le moniteur de fiabilité, un composant logiciel enfichable MMC qui effectue le suivi des modifications au système et les compare aux modifications de la stabilité du système, en fournissant un affichage graphique de leur relation. </para>
        </content>
      </section>
      <section address="BKMK_4"> 
        <title>Définir les niveaux d’enregistrement</title>
        <content>
          <para>si les informations que vous recevez dans le journal du Service d’annuaire dans l’Observateur d’événements ne sont pas suffisantes pour la résolution des problèmes, augmentez les niveaux d’enregistrement à l’aide de l’entrée de Registre appropriées dans</para><embeddedLabel>HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics</embeddedLabel>.
          <para>par défaut, les niveaux d’enregistrement pour toutes les entrées sont définis sur <embeddedLabel>0</embeddedLabel>, qui fournit la quantité minimale d’informations. Le niveau de journalisation la plus élevé est <embeddedLabel>5</embeddedLabel>. Augmenter le niveau d’une entrée entraîne des événements supplémentaires dans le journal des événements Service d’annuaire.</para>
          <para>vous pouvez utiliser la procédure suivante pour modifier le niveau de journalisation pour une entrée de diagnostic.</para>
          <alert class="caution">
            <para>nous recommandons que vous ne modifiez pas directement le Registre à moins qu’aucune autre solution. Modifications du Registre ne sont pas validées par l’Éditeur du Registre ou par Windows avant qu’ils sont appliqués, et par conséquent, des valeurs incorrectes peuvent être stockées. Cela peut entraîner des erreurs irrécupérables dans le système. Si possible, utilisez la stratégie de groupe ou d’autres outils Windows, tels qu’enfichables MMC pour accomplir des tâches, plutôt que de modifier le Registre directement. Si vous devez modifier le Registre, soyez très vigilant.</para>
          </alert>
          <para>
            <embeddedLabel>exigences</embeddedLabel>
          </para>
          <list class="bullet">
            <listItem>
              <para>l’appartenance au groupe <embeddedLabel>Admins du domaine</embeddedLabel>, ou équivalente, est la condition minimale requise pour effectuer cette procédure. <token>review_detailincludes </para>
            </listItem>
            <listItem>
              <para>outils: Regedit.exe</para>
            </listItem>
          </list>
          <procedure>
            <title>pour modifier le niveau d’enregistrement d’une entrée du diagnostic</title>
            <steps class="ordered">
              <step>
                <content>
                  <para>cliquez sur <ui>Démarrer</ui>, cliquez sur <ui>exécuter</ui>, type <userInput>regedit</userInput>, puis cliquez sur <ui>OK</ui>. </para>
                </content>
              </step>
              <step>
                <content>
                  <para>naviguer à l’entrée pour lequel vous souhaitez définir la journalisation dans <embeddedLabel>HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics</embeddedLabel>. </para>
                </content>
              </step>
              <step>
                <content>
                  <para>double-cliquez sur l’entrée et dans <embeddedLabel>Base</embeddedLabel>, cliquez sur <embeddedLabel>décimal</embeddedLabel>. </para>
                </content>
              </step>
              <step>
                <content>
                  <para>dans <embeddedLabel>valeur</embeddedLabel>, tapez un nombre entier compris entre <embeddedLabel>0</embeddedLabel> via <embeddedLabel>5</embeddedLabel>, puis cliquez sur <ui>OK</ui>. </para>
                </content>
              </step>
            </steps>
          </procedure>
        </content>
      </section>
    </sections>
  </section>
  <relatedTopics />
</developerConceptualDocument>


