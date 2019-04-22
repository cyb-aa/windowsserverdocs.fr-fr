---
title: Configurer les notifications par courrier électronique
description: Cet article explique comment configurer les notifications par courrier électronique
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d53be34d04edfac9f30b6e269833be74a6ebcf22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820350"
---
# <a name="configure-e-mail-notifications"></a>Configurer les notifications par courrier électronique

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Quand vous créez des quotas et des filtres de fichiers, vous pouvez envoyer des notifications par courrier électronique aux utilisateurs quand leur limite de quota approche ou quand ils ont essayé d'enregistrer des fichiers qui ont été bloqués. Quand vous générez des rapports de stockage, vous pouvez les envoyer par courrier électronique à des destinataires spécifiques. Si vous souhaitez informer certains administrateurs des événements liés aux quotas et aux filtres de fichiers ou envoyer des rapports de stockage, vous pouvez configurer un ou plusieurs destinataires par défaut.

Pour envoyer ces notifications et ces rapports de stockage, vous devez spécifier le serveur SMTP à utiliser pour le transfert des messages électroniques.

## <a name="to-configure-e-mail-options"></a>Pour configurer les options de messagerie

1.  Dans l'arborescence de la console, cliquez avec le bouton droit sur **Gestionnaire de ressources du serveur de fichiers**, puis cliquez sur **Configurer les options**. La boîte de dialogue **Options du Gestionnaire de ressources du serveur de fichiers** s'affiche.

2.  Sous l'onglet **Notifications par courrier électronique**, sous **Nom ou adresse IP du serveur SMTP**, tapez le nom d'hôte ou l'adresse IP du serveur SMTP qui transfèrera les notifications et les rapports de stockage par courrier électronique.

3.  Si vous souhaitez informer certains administrateurs des événements liés aux quotas ou aux filtres de fichiers ou envoyer des rapports de stockage par courrier électronique, sous **Administrateurs destinataires par défaut**, tapez chaque adresse de messagerie.

    Utilisez le format *account@domain*. Séparez les différents comptes par des points-virgules.

4.  Pour spécifier une autre adresse d'expéditeur pour envoyer des notifications et des rapports de stockage à partir du Gestionnaire de ressources du serveur de fichiers, sous **Adresse de messagerie de l’expéditeur par défaut**, tapez l’adresse de messagerie que vous souhaitez voir apparaître dans votre message.

5.  Pour tester vos paramètres, cliquez sur **Envoyer un message de test**.

6.  Cliquez sur **OK**.


## <a name="see-also"></a>Voir aussi

-   [Options de paramètre File Server Resource Manager](setting-file-server-resource-manager-options.md)