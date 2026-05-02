# CentralEXAuto

Repositório só com binários e manifestos para OTA.

## Ficheiros

| Ficheiro | Função |
|----------|--------|
| update.json | Versão do APK **CentralEXAuto** (`versionCode`, `versionName`, `apkUrl`, `sha256`). |
| CentralEXAuto-geely-platform-signed.apk | APK da central. |
| ca_fix.apk | ConnAdaptor modificado (colocar nesta pasta ao publicar). |
| connadaptor.json | Controlo OTA do **ca_fix**: campo `md5` (32 hex do ficheiro) e `apkUrl` (URL raw do mesmo `ca_fix.apk`). |

Ao correr `./gradlew signGeelyPlatform` no projeto **TelaPretaCentral**, se existir `CentralEXAuto/ca_fix.apk`, o `connadaptor.json` é regenerado com o MD5 certo. Caso contrário, atualize o MD5 antes do push (`md5sum ca_fix.apk` — usar 32 hex sem espaços no JSON).

O placeholder com `md5` só zeros deve ser substituído antes de uso real; a app valida o download contra este valor.
