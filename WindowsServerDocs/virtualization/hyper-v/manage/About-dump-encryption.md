---
title: À propos du chiffrement de vidage
description: Décrit comment chiffrer des fichiers dump et résoudre les problèmes de chiffrement.
ms.prod: windows-server
manager: dongill
ms.topic: article
author: larsiwer
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.author: kathydav
ms.date: 11/03/2016
ms.openlocfilehash: 2232f62090e171060f25e4c2513a217e2ab98eaa
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950546"
---
# <a name="about-dump-encryption"></a>À propos du chiffrement de vidage
Le chiffrement de vidage peut être utilisé pour chiffrer des vidages sur incident et des vidages dynamiques générés pour un système. Les vidages sont chiffrés à l’aide d’une clé de chiffrement symétrique qui est générée pour chaque vidage. Cette clé est ensuite chiffrée à l’aide de la clé publique spécifiée par l’administrateur approuvé de l’hôte (protecteur de clé de chiffrement de vidage sur incident). Cela garantit que seule une personne ayant la clé privée correspondante peut déchiffrer et donc accéder au contenu de l’image mémoire. Cette fonctionnalité est exploitée dans une infrastructure protégée.
Remarque : Si vous configurez le chiffrement de vidage, désactivez également Rapport d’erreurs Windows. WER ne peut pas lire les vidages sur incident chiffrés.

## <a name="configuring-dump-encryption"></a>Configuration du chiffrement de vidage
### <a name="manual-configuration"></a>Configuration manuelle
Pour activer le chiffrement de vidage à l’aide du Registre, configurez les valeurs de Registre suivantes sous `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| Nom de valeur | Tapez | Value |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 1 pour activer le chiffrement de vidage, 0 pour désactiver le chiffrement de vidage |
| EncryptionCertificates\Certificate.1 ::P ublicKey | Binaire | Clé publique (RSA, 2048 bits) qui doit être utilisée pour le chiffrement des vidages. Celui-ci doit être mis en forme en tant que [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| EncryptionCertificates\Certificate.1 :: empreinte | Chaîne | Empreinte numérique du certificat permettant la recherche automatique de la clé privée dans le magasin de certificats local lors du déchiffrement d’un vidage sur incident. |


### <a name="configuration-using-script"></a>Configuration à l’aide d’un script
Pour simplifier la configuration, un [exemple de script](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption) est disponible pour activer le chiffrement de vidage basé sur une clé publique à partir d’un certificat.

1. Dans un environnement approuvé : créez un certificat avec une clé RSA 2048 bits et exportez le certificat public
2. Sur les hôtes cibles : importez le certificat public dans le magasin de certificats local
3. Exécuter l’exemple de script de configuration 
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

## <a name="decrypting-encrypted-dumps"></a>Déchiffrement des vidages chiffrés
Pour déchiffrer un fichier de vidage chiffré existant, vous devez télécharger et installer les outils de débogage pour Windows. Cet ensemble d’outils contient KernelDumpDecrypt. exe qui peut être utilisé pour déchiffrer un fichier de vidage chiffré.
Si le certificat incluant la clé privée est présent dans le magasin de certificats de l’utilisateur actuel, le fichier de vidage peut être déchiffré en appelant

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
Après le déchiffrement, les outils tels que WinDbg peuvent ouvrir le fichier dump déchiffré.

## <a name="troubleshooting-dump-encryption"></a>Résolution des problèmes de chiffrement de vidage
Si le chiffrement de vidage est activé sur un système mais qu’aucun vidage n’est généré, consultez le journal des événements `System` du système pour `Kernel-IO` événement 1207. Lorsque le chiffrement du dump ne peut pas être initialisé, cet événement est créé et les vidages sont désactivés.

| Messages d’erreur détaillés | Étapes à suivre pour atténuer |
| ---------------------- | ----------------- |
| Registre de clé publique ou d’empreinte manquant | Vérifier si les deux valeurs de Registre existent à l’emplacement attendu |
| Clé publique non valide | Assurez-vous que la clé publique stockée dans la valeur de Registre PublicKey est stockée en tant que [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| Taille de clé publique non prise en charge | Actuellement, seules les clés RSA 2048 bits sont prises en charge. Configurer une clé qui correspond à cette exigence |

Vérifiez également si la valeur `GuardedHost` sous `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled` est définie sur une valeur autre que 0. Cela désactive complètement les vidages sur incident. Si c’est le cas, affectez-lui la valeur 0.
