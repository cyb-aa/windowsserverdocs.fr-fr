---
title: Sur le chiffrement de vidage
description: Décrit comment chiffrer des fichiers de vidage et de résoudre les problèmes de chiffrement.
ms.prod: windows-server-threshold
manager: dongill
ms.topic: article
author: larsiwer
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.author: kathydav
ms.date: 11/03/2016
ms.openlocfilehash: e0ca8829aa8f93e7543b4183539beade3b4aeb47
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812390"
---
# <a name="about-dump-encryption"></a>Sur le chiffrement de vidage
Chiffrement de vidage peut être utilisé pour chiffrer des vidages sur incident et en direct vidages générés pour un système. Les images mémoire sont chiffrées à l’aide d’une clé de chiffrement symétrique qui est générée pour chaque image. Cette clé lui-même est ensuite chiffrée à l’aide de la clé publique spécifiée par l’administrateur approuvé de l’hôte (protecteur de clé incident vidage chiffrement). Cela garantit que seules les personnes ayant la clé privée correspondante peuvent déchiffrer et donc accéder aux contenu de l’image mémoire. Cette capacité est exploitée dans une infrastructure protégée.
Remarque: Si vous configurez le chiffrement d’image mémoire, également désactiver rapport d’erreurs Windows. Rapport d’erreurs Windows ne peut pas lire les vidages sur incident chiffré.

# <a name="configuring-dump-encryption"></a>Configuration du chiffrement de vidage
## <a name="manual-configuration"></a>Configuration manuelle
Pour activer le chiffrement de vidage à l’aide du Registre, configurez les valeurs de Registre suivantes sous `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| Nom de valeur | Type | Value |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 1 pour activer le chiffrement de vidage, 0 pour désactiver le chiffrement de vidage |
| EncryptionCertificates\Certificate.1::PublicKey | Binary | Clé publique (RSA, 2 048 bits) qui doit être utilisé pour le chiffrement des vidages. Cette valeur doit être au format [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| EncryptionCertificates\Certificate.1::Thumbprint | Chaîne | Empreinte numérique du certificat pour permettre la recherche automatique de la clé privée dans le magasin de certificats local lors du déchiffrage d’un vidage sur incident. |


## <a name="configuration-using-script"></a>Configuration à l’aide du script
Pour simplifier la configuration, un [exemple de script](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption) est disponible pour activer le chiffrement de vidage basé sur une clé publique à partir d’un certificat.

1. Dans un environnement approuvé : Créez un certificat avec une clé RSA 2 048 bits et exportez le certificat public
2. Sur les ordinateurs hôtes cibles : Importez le certificat public dans le magasin de certificats local
3. Exécutez l’exemple de script de configuration 
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

# <a name="decrypting-encrypted-dumps"></a>Déchiffrement de vidages de cryptée
Pour déchiffrer un fichier de vidage chiffré existant, vous devez télécharger et installer les outils de débogage pour Windows. Cet ensemble d’outils contient KernelDumpDecrypt.exe qui peut être utilisé pour déchiffrer un fichier de vidage de cryptée.
Si le certificat dont la clé privée est présent dans le magasin de certificats de l’utilisateur actuel, le fichier de vidage peut être déchiffré en appelant

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
Après le déchiffrement, outils, tels que WinDbg peuvent ouvrir le fichier de vidage déchiffrée.

# <a name="troubleshooting-dump-encryption"></a>Résolution des problèmes de chiffrement d’image mémoire
Si le chiffrement d’image mémoire est activé sur un système, mais aucun vidages ne sont générés, vérifiez le système `System` journal des événements pour `Kernel-IO` événement 1207. Lorsque le chiffrement de vidage ne peut pas être initialisé, cet événement est créé et vidages sont désactivées.

| Message d’erreur détaillé | Étapes permettant de résoudre |
| ---------------------- | ----------------- |
| Public Key ou empreinte numérique du Registre manquant | Vérifier si les deux valeurs de Registre sont présents dans l’emplacement attendu |
| Clé publique non valide | Assurez-vous que la clé publique stockée dans la valeur de Registre PublicKey est stockée comme [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| Taille de clé publique non pris en charge | Actuellement, seules les clés RSA 2 048 bits sont prises en charge. Configurer une clé qui correspond à cette exigence |

Vérifiez également si la valeur `GuardedHost` sous `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled` est définie sur une valeur différente de 0. Cela désactive complètement les vidages sur incident. Si c’est le cas, définissez-la sur 0.
