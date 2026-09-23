# Comunicação de Rede Sem Fio (Wireless)

Resumo sobre WLANs, funcionamento, ameaças e formas de proteção.

## 1. LANs Wireless x LANs Wired

WLANs usam radiofrequência (RF) na camada física e na subcamada MAC. Seguem o padrão **IEEE 802.11**, enquanto as LANs com fio seguem o **802.3 (Ethernet)**.

| Característica | 802.11 (WLAN) | 802.3 (Ethernet) |
| --- | --- | --- |
| Camada física | Radiofrequência (RF) | Cabos físicos |
| Acesso à mídia | Prevenção de colisão (CSMA/CA) | Detecção de colisão (CSMA/CD) |
| Disponibilidade | Qualquer um com placa wireless no alcance do AP | Precisa de conexão física |
| Interferência | Sim | Mínima |
| Regulamentações | Variam por país | Padrão IEEE |

Outras diferenças:
- Clientes se conectam por AP ou roteador wireless, não por switch
- Dispositivos móveis, geralmente com bateria
- Formato de quadro diferente, com mais informações no cabeçalho
- Mais problemas de privacidade, já que o sinal pode passar do ambiente físico

## 2. Estrutura do quadro 802.11

Igual ao Ethernet (cabeçalho, payload e FCS), mas com mais campos:

- **Controle de quadro:** tipo do quadro, versão do protocolo, gerenciamento de energia, segurança
- **Duração:** tempo restante para a próxima transmissão
- **Address1:** MAC do receptor (dispositivo ou AP)
- **Address2:** MAC do transmissor
- **Address3:** MAC do destino (ex: gateway padrão)
- **Sequence Control:** sequenciamento e fragmentação
- **Address4:** só usado no modo ad hoc
- **Payload:** dados
- **FCS:** controle de erro da camada 2

## 3. CSMA/CA

WLANs são half-duplex e de mídia compartilhada. O cliente não consegue ouvir enquanto envia, então não dá pra detectar colisão. A solução é **preveni-la**:

1. Ouve o canal para ver se está ocioso
2. Envia **RTS** (Ready to Send) pro AP
3. Recebe **CTS** (Clear to Send) do AP
4. Sem CTS, espera um tempo aleatório e recomeça
5. Com CTS, transmite os dados
6. Toda transmissão precisa de confirmação (ACK). Sem ACK, assume colisão e recomeça

## 4. Associação cliente e AP

Três estágios:
1. Descobrir o AP
2. Autenticar no AP
3. Associar ao AP

Parâmetros que precisam bater entre cliente e AP:
- **SSID:** nome da rede (pode mapear pra uma VLAN)
- **Senha:** usada na autenticação
- **Modo de rede:** padrões 802.11a/b/g/n/ac/ad (modo misto suporta vários)
- **Modo de segurança:** WEP, WPA, WPA2 (sempre usar o mais alto suportado)
- **Canal:** faixa de frequência (automático ou manual)

## 5. Descoberta passiva x ativa

- **Passivo:** o AP envia **beacons** periodicamente com SSID, padrões e segurança. O cliente escolhe a rede.
- **Ativo:** o cliente envia um **probe request** e o AP responde com **probe response**. Necessário quando o AP não transmite beacon. O cliente também pode mandar probe request sem SSID pra descobrir redes próximas (APs com broadcast de SSID desativado não respondem).

## 6. Dispositivos: AP, LWAP e WLC

- **Roteador wireless doméstico:** junta roteador, switch e AP em um só aparelho
- **AP autônomo:** gerenciado individualmente
- **WLC (Wireless LAN Controller):** centraliza gerenciamento e configuração (SSIDs, autenticação)
- **LWAP (Lightweight AP):** com uma WLC, o AP só encaminha dados entre a WLAN e a WLC

## 7. Ameaças em WLANs

- **Interceptação de dados:** resolvido com criptografia
- **Intrusos wireless:** resolvido com autenticação forte
- **Ataques DoS:** por configuração errada, ataque malicioso ou interferência acidental (micro-ondas, telefone sem fio, etc.)
- **APs não autorizados (rogue AP):** instalados sem permissão, com ou sem má intenção

### DoS
Como reduzir o risco: proteger dispositivos, usar senhas fortes, fazer backups, aplicar mudanças fora do horário comercial e monitorar interferência. A banda de **2,4 GHz** sofre mais interferência que a de **5 GHz**.

### Rogue AP
Permite ao invasor capturar MACs e pacotes, acessar recursos da rede ou fazer MITM. Um hotspot pessoal num host autorizado também conta como rogue AP. Prevenção: políticas de rogue AP na WLC e software de monitoramento do espectro de rádio.

### Man-in-the-Middle (Evil Twin)
O atacante cria um AP falso com o **mesmo SSID** de um legítimo. Clientes próximos se conectam ao sinal mais forte, e o tráfego passa pelo atacante, que captura e repassa os dados. É comum em Wi-Fi gratuito (aeroportos, cafés, restaurantes) por causa da autenticação aberta. Prevenção: autenticar usuários, identificar dispositivos legítimos e monitorar tráfego anormal.

## 8. Protegendo a WLAN

### Medidas antigas (fracas)
- **Ocultação de SSID:** desativa o beacon do SSID
- **Filtragem de MAC:** permite ou nega por endereço MAC

Nenhuma das duas segura um invasor esperto, porque SSIDs são fáceis de descobrir e MACs podem ser falsificados.

### Autenticação original 802.11
- **Sistema aberto:** qualquer um conecta. Só serve quando segurança não importa (cafés, hotéis). O cliente deve usar VPN.
- **Chave compartilhada:** senha pré-compartilhada (WEP, WPA, WPA2, WPA3)

### Métodos de chave compartilhada

| Método | Descrição |
| --- | --- |
| **WEP** | Criptografia RC4 com chave estática. Fácil de quebrar. **Nunca usar.** |
| **WPA** | Usa TKIP, que muda a chave a cada pacote |
| **WPA2** | Padrão atual do setor. Usa **AES** |
| **WPA3** | Próxima geração. Exige PMF (quadros de gerenciamento protegidos) e remove protocolos legados |

### WPA2: Personal x Enterprise
- **Personal:** autenticação por PSK (senha pré-compartilhada), sem servidor especial. Indicado pra casa e pequeno escritório.
- **Enterprise:** exige servidor **RADIUS**, autenticação **802.1X** com **EAP**. Mais complexo, porém mais seguro.

### Criptografia
- **TKIP (WPA):** ainda baseado em WEP, com chave por pacote e verificação de integridade (MIC)
- **AES (WPA2):** método preferido e mais forte, usa **CCMP** pra detectar alterações nos dados

### Autenticação Enterprise (RADIUS)
Configurações no AP:
- **IP do servidor RADIUS**
- **Portas UDP:** 1812 (autenticação) e 1813 (contabilidade). Também podem ser 1645 e 1646.
- **Chave compartilhada:** autentica o AP com o RADIUS. **Não** é configurada no cliente.

## 9. WPA3

Recomendado como o método mais seguro, com quatro recursos:

- **WPA3-Personal:** usa **SAE** (Simultaneous Authentication of Equals), então o PSK nunca é exposto e o brute force no handshake deixa de funcionar
- **WPA3-Enterprise:** continua com 802.1X/EAP, mas exige conjunto criptográfico de **192 bits** (CNSA) e elimina mistura de protocolos antigos
- **Redes abertas:** sem autenticação, mas com **OWE** (Opportunistic Wireless Encryption) criptografando todo o tráfego
- **Integração IoT:** o **DPP** (Device Provisioning Protocol) substitui o WPS, que é vulnerável. Dispositivos sem tela usam uma chave pública (geralmente em QR Code) para entrarem na rede.

## Resumo rápido

- WLAN = RF + CSMA/CA, half-duplex e mídia compartilhada
- Ameaças principais: interceptação, intrusos, DoS, rogue AP e evil twin
- Ocultar SSID e filtrar MAC **não bastam**
- WEP nunca, WPA2 com AES como mínimo, WPA3 sempre que possível
- Em ambiente corporativo: WPA2/WPA3 Enterprise com RADIUS e 802.1X
