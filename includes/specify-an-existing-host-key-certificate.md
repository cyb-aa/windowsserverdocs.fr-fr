Vous pouvez également spécifier une empreinte numérique si vous souhaitez utiliser votre propre certificat. Cela peut être utile si vous souhaitez partager un certificat sur plusieurs ordinateurs, ou utiliser un certificat lié à un module de plateforme sécurisée ou d’un module HSM. Voici un exemple de création d’un certificat lié à du module de plateforme sécurisée (ce qui empêche de générer la clé privée volé et utilisé sur un autre ordinateur et nécessite un TPM 1.2) :

```powersehll
$tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
```


<!-- Appears in set-up-hgs-for-always-encrypted-in-sql-server.md and guarded-fabric-create-host-key.md
-->