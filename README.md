# CentralEXAuto

Ecossistema de apps para a central multimídia **Geely IHU629G** (Android Automotive / ECarX) e o celular do motorista. Quatro peças principais, cada uma com um papel distinto:

## TelaPretaCentral (`com.ex.auto`)

O app principal, roda **na central do carro**. É um app de sistema (`sharedUserId="android.uid.system"`, assinado com a chave de plataforma da Geely), o que dá acesso a APIs privilegiadas (Car API, WiFi, energia) que um app comum não teria.

Principais funções:
- **Tela preta (blackout)** — cobre o display da central quando não está em uso.
- **Acesso Rápido** — floater (overlay) com atalhos: modo condução/regeneração, telemetria, AVAS, HVAC.
- **Drive/Regen** — sincroniza os controles de condução/regeneração e do ar-condicionado com o estado real do carro (lido ao vivo do VHAL, não de preferências salvas).
- **AVAS** — controle do som de alerta para pedestres.
- **WiFi privilegiado** — gestão de rede/hotspot com permissões que um app normal não tem.
- **Telemetria** — expõe os dados do VHAL (velocidade, bateria, etc.) via TCP na porta 47800, consumidos por outros apps (painel, AAExCarro).
- **Sincronização com o celular** — troca configurações e comandos com o CentralPhoneApp em tempo real.
- **Watchdog** — serviço em foreground que mantém tudo isso vivo, reaplica ajustes no boot/troca de marcha e se recupera de crash/bootloop.
- **Instalador de apps** — recebe e instala os APKs baixados pelo AAExInstall.

## AAExInstall (`com.central.aaexinstall`)

Roda **no celular** do motorista (não na central). É a "loja de apps" do ecossistema: baixa, confere o SHA-256 e instala/atualiza os demais apps (incluindo os três abaixo e vários outros de terceiros úteis no carro), buscando a versão mais recente de cada um a partir deste repositório.

## AAExCarro (`com.aaex.carro`)

Roda **no celular**, com superfície no Android Auto. É o app de telemetria e custos do veículo:
- Lê dados de OBD2 via adaptador ELM327 (a central não consegue requisitar OBD2 diretamente).
- Recebe telemetria do VHAL enviada pela central (velocidade, bateria, etc.).
- Mantém histórico offline das viagens.
- Módulo de **Custos & Eficiência**: recargas, despesas, relatórios de custo por km e mensais, eficiência real vs. prometida (inspirado no Drivvo).

## CentralPhoneApp (`com.central.telapreta.phone`)

Roda **no celular**. É o companheiro do TelaPretaCentral: sincroniza configurações e envia comandos remotos (Android Auto/CarPlay, clima, reboot) entre o celular e a central, por WiFi ou Bluetooth. A central é sempre a fonte da verdade para a maioria dos ajustes; o celular pode comandar diretamente o ar-condicionado (ventilação/temperatura).
