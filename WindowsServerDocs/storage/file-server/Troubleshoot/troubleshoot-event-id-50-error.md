---
title: Résoudre les problèmes liés au message d’erreur de l’ID d’événement 50
description: Décrit comment résoudre les problèmes liés au message d’erreur 50 de l’ID d’événement
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 202e0604fc492ff72cd1794bc8197a12c1ab9163
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654380"
---
# <a name="troubleshoot-the-event-id-50-error-message"></a>Résoudre les problèmes liés au message d’erreur de l’ID d’événement 50

##  <a name="symptoms"></a>Symptômes

Lorsque des informations sont écrites sur le disque physique, les deux messages d’événements suivants peuvent être consignés dans le journal des événements système : 

```
Event ID: 50 
Event Type: Warning 
Event Source: Ftdisk 
Description: {Lost Delayed-Write Data} The system was attempting to transfer file data from buffers to \Device\HarddiskVolume4. The write operation failed, and only some of the data may have been written to the file.
Data: 
0000: 00 00 04 00 02 00 56 00 
0008: 00 00 00 00 32 00 04 80 
0010: 00 00 00 00 00 00 00 00 
0018: 00 00 00 00 00 00 00 00 
0020: 00 00 00 00 00 00 00 00 
0028: 11 00 00 80 
```

```
Event ID: 26 
Event Type: Information
Event Source: Application Popup
Description: Windows - Delayed Write Failed : Windows was unable to save all the data for the file \Device\HarddiskVolume4\Program Files\Microsoft SQL Server\MSSQL$INSTANCETWO\LOG\ERRORLOG. The data has been lost. This error may be caused by a failure of your computer hardware or network connection.

Please try to save this file elsewhere.
```

Ces messages d’ID d’événement signifient exactement la même chose et sont générés pour les mêmes raisons. Dans le cadre de cet article, seul le message ID d’événement 50 est décrit.

> [!NOTE] 
> L’appareil et le chemin d’accès dans la description et les données hexadécimales spécifiques varient. 

##  <a name="more-information"></a>Plus d’informations

Un message d’ID d’événement 50 est consigné si une erreur générique se produit lorsque Windows tente d’écrire des informations sur le disque. Cette erreur se produit lorsque Windows tente de valider des données à partir du gestionnaire de cache du système de fichiers (pas le cache au niveau matériel) sur le disque physique. Ce comportement fait partie de la gestion de la mémoire de Windows. Par exemple, si un programme envoie une demande d’écriture, la demande d’écriture est mise en cache par le gestionnaire de cache et le programme est informé que l’écriture s’est terminée avec succès. Ultérieurement, le gestionnaire de cache tente d’écrire tardivement les données sur le disque physique. Lorsque le gestionnaire de cache tente de valider les données sur le disque, une erreur se produit lors de l’écriture des données, et les données sont vidées du cache et ignorées. La mise en cache en écriture différée améliore les performances du système, mais la perte de données et la perte d’intégrité du volume peuvent se produire suite à la perte d’échecs d’écriture différée.

Il est important de se souvenir que les e/s ne sont pas toutes des e/s mises en mémoire tampon par le gestionnaire de cache. Les programmes peuvent définir un indicateur de FILE_FLAG_NO_BUFFERING qui contourne le gestionnaire de cache. Lorsque SQL effectue des écritures critiques dans une base de données, cet indicateur est défini pour garantir que la transaction est exécutée directement sur le disque. Par exemple, les écritures non critiques dans des fichiers journaux effectuent des e/s mises en mémoire tampon pour améliorer les performances globales. Un message d’ID d’événement 50 ne résulte jamais des e/s sans mise en mémoire tampon.

Il existe plusieurs sources différentes pour un message d’ID d’événement 50. Par exemple, un message d’ID d’événement 50 enregistré à partir d’une source MRxSmb se produit en cas de problème de connectivité réseau avec le redirecteur. Pour éviter d’effectuer des étapes de dépannage incorrectes, veillez à consulter le message d’ID d’événement 50 pour confirmer qu’il fait référence à un problème d’e/s de disque et que cet article s’applique.

Un message d’ID d’événement 50 est semblable à l’ID d’événement 9 et à un message d’ID d’événement 11. Bien que l’erreur ne soit pas aussi grave que l’erreur indiquée par l’ID d’événement 9 et un message d’ID d’événement 11, vous pouvez utiliser les mêmes techniques de dépannage pour un message d’ID d’événement 50 que pour l’ID d’événement 9 et un message d’ID d’événement 11. Toutefois, n’oubliez pas que tout ce qui se trouve dans la pile peut entraîner des pertes de temps d’écriture, telles que les pilotes de filtre et les pilotes mini-ports. 

Vous pouvez utiliser les données binaires associées à toute erreur « disque » associée (indiquée par l’ID d’événement 9, 11, 51 message d’erreur ou d’autres messages) pour vous aider à identifier le problème.

###  <a name="how-to-decode-the-data-section-of-an-event-id-50-event-message"></a>Comment décoder la section de données d’un message d’événement ID d’événement 50 

Quand vous décodez la section de données dans l’exemple d’un message d’ID d’événement 50 inclus dans la section « Résumé », vous constatez que la tentative d’exécution d’une opération d’écriture a échoué parce que l’appareil est occupé et que les données ont été perdues. Cette section décrit comment décoder ce message d’ID d’événement 50. 

Le tableau suivant décrit ce que représente chaque décalage de ce message : 

|OffsetLengthValues|Durée|Valeurs|
|-----------|------------|---------|
|0x00|2|Non utilisé|
|0x02|2|Taille des données de vidage = 0x0004|
|0x04|2|Nombre de chaînes = 0x0002|
|0x06|2|Décalage des chaînes|
|0x08|2|Catégorie d'événement|
|0x0c|4|Code d’erreur NTSTATUS = 0x80040032 = IO_LOST_DELAYED_WRITE|
|0x10|8|Non utilisé|
|0x18|8|Non utilisé|
|0x20|8|Non utilisé|
|0x28|4|Code d’erreur de l’état NT|

#### <a name="key-sections-to-decode"></a>Sections clés à décoder

**Code d’erreur**

Dans l’exemple de la section « Résumé », le code d’erreur est indiqué sur la deuxième ligne. Cette ligne commence par « 0008 : » et comprend les quatre derniers octets de cette ligne : 0008:00 00 00 00 32 00 04 80 dans ce cas, le code d’erreur est 0x80040032. Le code suivant est le code de l’erreur 50, et il est identique pour tous les messages de l’ID d’événement 50 : IO_LOST_DELAYED_WRITEWARNINGNote lorsque vous convertissez les données hexadécimales du message d’ID d’événement en code d’État, n’oubliez pas que les valeurs sont représentées dans la format Little endian.

**Le disque cible**

Vous pouvez identifier le disque sur lequel l’écriture a été tentée à l’aide du lien symbolique figurant sur le lecteur figurant dans la section « Description » du message d’ID d’événement, par exemple : \Device\HarddiskVolume4. Pour plus d’informations sur l’identification du lecteur, cliquez sur le numéro d’article suivant pour afficher l’article de la base de connaissances Microsoft : [159865](/EN-US/help/159865) comment faire la distinction entre un périphérique de disque physique et un message d’événement

**Le code d’état final**

Le code d’état final est l’information la plus importante dans un message d’ID d’événement 50. Il s’agit du code d’erreur qui est retourné lorsque la requête d’e/s a été effectuée, et il s’agit de la principale source d’informations. Dans l’exemple de la section « Résumé », le code d’état final est listé à 0x28, la sixième ligne, qui commence par « 0028 : » et comprend les quatre octets de cette ligne : 

```
0028: 11 00 00 80 
```

Dans ce cas, l’état final est égal à 0x80000011. Ce code d’État est mappé à STATUS_DEVICE_BUSY et implique que l’appareil est actuellement occupé.

>[!NOTE] 
> Lorsque vous convertissez les données hexadécimales du message ID d’événement 50 en code d’État, n’oubliez pas que les valeurs sont représentées au format Little endian. Étant donné que le code d’État est la seule information qui vous intéresse, il peut être plus facile d’afficher les données au format WORDs au lieu d’octets. Si vous procédez ainsi, les octets seront au format correct et les données peuvent être plus faciles à interpréter rapidement.

Pour ce faire, cliquez sur **mots** dans la fenêtre Propriétés de l' **événement** . Dans la vue des mots de données, l’exemple de la section « symptômes » est le suivant : Data : 

```
() Bytes (.) 
Words 0000: 00040000 00560002 00000000 80040032 0010: 00000000 00000000 00000000 00000000 0020: 00000000 00000000 80000011
```

Pour obtenir la liste des codes d’État Windows NT, consultez NTSTATUS. H dans le kit de développement logiciel (SDK) Windows.