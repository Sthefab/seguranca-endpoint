# Atacando a Fundação

Resumo do módulo sobre ataques às camadas de rede e transporte: como o IP, o ICMP, o TCP e o UDP podem ser explorados.

---

## 1. Detalhes do PDU IP

O **IP** é um protocolo de **Camada 3**, **sem conexão**. Ele só entrega o pacote da origem ao destino por um conjunto de redes interconectadas. Ele **não rastreia nem gerencia** o fluxo dos pacotes (isso, quando necessário, fica com o **TCP** na camada 4).

**O ponto crítico de segurança:** o IP **não valida** se o endereço IP de origem do pacote realmente veio daquela origem. Por isso os atores de ameaça conseguem:
- Enviar pacotes com IP de origem **falsificado (spoofing)**
- Adulterar outros campos do cabeçalho IP

> **Traduzindo:** o IP é como uma carta em que ninguém confere o remetente escrito no envelope. Por isso o analista de segurança precisa conhecer bem os campos dos cabeçalhos.

---

## 2. Cabeçalho IPv4

| Campo | O que é |
|---|---|
| **Versão** | 4 bits, valor `0100`. Identifica como IPv4. |
| **Comprimento do cabeçalho (IHL)** | 4 bits. Tamanho do cabeçalho IP. Mínimo de **20 bytes**. |
| **DiffServ (DS)** | 8 bits. Antes chamado de **ToS**. Define a **prioridade** do pacote. Os 6 primeiros bits são o **DSCP** e os 2 últimos são o **ECN** (notificação de congestionamento). |
| **Comprimento total** | 2 bytes. Tamanho do pacote inteiro (cabeçalho + dados). Máximo de **65.535 bytes**, mas na prática é bem menor. |
| **Identificação, flags e deslocamento de fragmento** | Usados para **fragmentar e remontar** pacotes quando a rota não suporta o tamanho. |
| **TTL (Time to Live)** | 8 bits. Limita a vida do pacote. Cada roteador **subtrai 1**. Ao chegar a **0**, o roteador descarta e manda um ICMP de **tempo excedido** para a origem. |
| **Protocolo** | 8 bits. Diz qual protocolo de camada superior vem dentro: **ICMP (1), TCP (6), UDP (17)**. |
| **Checksum do cabeçalho** | Valor calculado a partir do cabeçalho para detectar erros na transmissão. |
| **IP de origem** | 32 bits. Sempre um endereço **unicast**. |
| **IP de destino** | 32 bits. |
| **Opções e preenchimento** | Tamanho variável (0 até múltiplo de 32 bits). Se não fechar em múltiplo de 32, completa com zeros (padding). |

> **Traduzindo:** o TTL evita que um pacote fique rodando na internet pra sempre em caso de loop de roteamento. É também o princípio por trás do `traceroute`.

---

## 3. Cabeçalho IPv6

Só **8 campos** (bem mais simples que o IPv4):

| Campo | O que é | Equivalente no IPv4 |
|---|---|---|
| **Versão** | 4 bits, valor `0110`. | Versão |
| **Classe de tráfego** | 8 bits. Prioridade do pacote. | DiffServ (DS) |
| **Rótulo de fluxo** | 20 bits. Sugere que pacotes com o mesmo rótulo recebam o **mesmo tratamento** dos roteadores. | (não tem) |
| **Tamanho da carga** | 16 bits. Tamanho só da parte de dados (payload). | Comprimento total (parecido) |
| **Próximo cabeçalho** | 8 bits. Tipo de carga que vem dentro. | Protocolo |
| **Limite de saltos** | 8 bits. Cada roteador subtrai 1. Ao chegar a 0, descarta e envia ICMPv6 de tempo excedido. | TTL |
| **IPv6 de origem** | 128 bits. | IP de origem |
| **IPv6 de destino** | 128 bits. | IP de destino |

**Cabeçalhos de extensão (EH):**
- Opcionais. Ficam **entre o cabeçalho IPv6 e a carga**.
- Usados para fragmentação, segurança, suporte à mobilidade etc.

> **Diferença importante:** no IPv6, os **roteadores não fragmentam** os pacotes. No IPv4, eles fragmentam.

---

## 4. Vulnerabilidades do IP: visão geral

| Ataque | Resumo |
|---|---|
| **ICMP** | Usa pacotes de eco (ping) para descobrir hosts e sub-redes, gerar DoS por inundação e até alterar tabelas de roteamento. |
| **DoS** | Impede usuários legítimos de acessar informações ou serviços. |
| **DDoS** | Igual ao DoS, mas **coordenado e simultâneo** a partir de várias máquinas. |
| **Falsificação de endereço (spoofing)** | Forja o endereço de origem, de forma cega ou não cega. |
| **MiTM (Man-in-the-Middle)** | O atacante se posiciona **entre origem e destino** para espiar, capturar ou alterar a comunicação. |
| **Sequestro de sessão** | O atacante ganha acesso à rede física e usa MiTM para **tomar** uma sessão. |

---

## 5. Ataques ICMP

**ICMP** foi criado para levar **mensagens de diagnóstico e erro** (rota, host ou porta indisponível). O `ping` é uma mensagem ICMP gerada pelo usuário (**echo request**) para testar conectividade.

**Como o atacante abusa:**
- **Reconhecimento e varredura:** mapear a topologia, descobrir **hosts ativos**, fazer **OS fingerprinting** (identificar o sistema operacional) e descobrir o estado do firewall.
- **DoS e DDoS:** por meio de **inundação ICMP (ICMP flood)**.

> **Nota:** ICMPv4 e ICMPv6 são vulneráveis a ataques parecidos.

### Mensagens ICMP de interesse do atacante

| Mensagem | Uso malicioso |
|---|---|
| **Echo request / echo reply** | Verificar hosts ativos e fazer DoS. |
| **Inacessível (unreachable)** | Reconhecimento e varredura de rede. |
| **Resposta de máscara (mask reply)** | Mapear a rede IP interna. |
| **Redirect** | Fazer o host alvo mandar **todo o tráfego** por um dispositivo comprometido, gerando **MiTM**. |
| **Descoberta de rotas (router discovery)** | Injetar **rotas falsas** na tabela de roteamento do alvo. |

### Defesa
- **ACLs** rigorosas de ICMP na **borda da rede** para bloquear sondagem vinda da internet.
- Analistas devem detectar esses ataques olhando **tráfego capturado e logs**.
- Em redes grandes, **firewalls e IDS** devem detectar e gerar alertas.

---

## 6. Ataques de amplificação e reflexão

Técnicas usadas para criar **DoS**, fazendo com que **terceiros** enviem o tráfego para a vítima.

- **Amplificação:** o atacante manda uma requisição pequena e a resposta gerada é **muito maior**.
- **Reflexão:** o atacante usa **outros dispositivos como "espelho"** e forja o IP de origem como sendo o da vítima. As respostas vão todas para ela.

**Exemplo clássico: ataque Smurf**
1. O atacante envia um **ICMP echo request** para o endereço de **broadcast** de uma rede.
2. Ele **falsifica o IP de origem** como sendo o da vítima.
3. **Todos os hosts** da rede respondem com echo reply, e tudo cai na vítima.

> **Traduzindo:** é como pedir pra 500 pessoas ligarem para o número de alguém, usando o nome dessa pessoa. O celular dela derrete.

**Versões mais recentes:** reflexão e amplificação baseadas em **DNS** e em **NTP (Network Time Protocol)**.

**Ataques de exaustão de recursos:** consomem os recursos do alvo (CPU, memória, conexões, banda) para travá-lo ou esgotar a rede.

---

## 7. Ataques de falsificação de endereços (spoofing)

**Spoofing de IP:** o atacante cria pacotes com **IP de origem falso** para **esconder sua identidade** ou **se passar por outro usuário legítimo**. Pode dar acesso a dados inacessíveis ou burlar configurações de segurança. Geralmente vem embutido em outro ataque (ex: Smurf).

### Cego x não cego

| Tipo | O atacante vê o tráfego? | Usado para |
|---|---|---|
| **Não cego (non-blind)** | **Sim** | Inspecionar a resposta da vítima, descobrir o estado do firewall, **prever número de sequência** e sequestrar sessão autorizada. |
| **Cego (blind)** | **Não** | Ataques de **DoS**. |

### Spoofing de MAC

Usado quando o atacante **já está na rede interna**.

1. O atacante muda o MAC do próprio host para o **MAC de um host alvo** (ex: um servidor).
2. Envia um quadro na rede com esse MAC.
3. O **switch** olha o MAC de origem e **atualiza a tabela CAM**, associando aquele MAC à **porta do atacante**.
4. A partir daí, o switch encaminha para o atacante os quadros destinados ao alvo.

> **Traduzindo:** a **tabela CAM** é a "agenda" do switch que liga cada MAC a uma porta. O atacante engana o switch e faz ele trocar o contato.

### Spoofing de aplicativo/serviço
Exemplo: **servidor DHCP falso (rogue DHCP)** ligado na rede para criar uma condição de **MiTM**.

---

## 8. TCP: cabeçalho e serviços

O segmento TCP aparece **logo após o cabeçalho IP**.

### Serviços do TCP

| Serviço | Como funciona |
|---|---|
| **Entrega confiável** | Usa **confirmações (ACK)**. Se o ACK não chega a tempo, o remetente **retransmite**. Usado por HTTP, SSL/TLS, FTP, transferências de zona DNS etc. Os ACKs podem gerar atraso. |
| **Controle de fluxo** | Em vez de confirmar um segmento por vez, vários segmentos podem ser confirmados com **um único ACK**. |
| **Comunicação stateful (com estado)** | A conexão é aberta com o **handshake de três vias** antes de trocar dados. |

### Handshake de três vias (three-way handshake)

1. **SYN:** o cliente pede para abrir a conexão.
2. **SYN-ACK:** o servidor confirma e também abre o lado dele.
3. **ACK:** o cliente confirma. Conexão estabelecida.

---

## 9. Ataques TCP

Aplicações em rede usam **portas TCP ou UDP**. Atores de ameaça fazem **varredura de portas (port scan)** para descobrir quais serviços o alvo oferece.

### 9.1 Inundação de SYN (TCP SYN Flood)

Explora o handshake de três vias.

1. O atacante manda **SYN em massa** com IP de origem **falsificado aleatoriamente**.
2. O alvo responde com **SYN-ACK** para esses IPs falsos e **espera o ACK**.
3. O ACK **nunca chega**.
4. O alvo fica cheio de **conexões semiabertas (half-open)** e passa a **negar serviço** a usuários legítimos.

> **Traduzindo:** é como uma fila de atendimento onde várias pessoas pegam senha e somem. O balcão fica esperando e ninguém de verdade é atendido.

### 9.2 Ataque de redefinição de TCP (TCP Reset)

- O TCP fecha conexões normalmente com uma troca de **quatro vias** (pares de **FIN e ACK** de cada lado).
- Já o bit **RST** encerra a conexão de forma **abrupta e imediata**.
- O atacante manda um pacote **falsificado com RST** para um ou ambos os pontos e **derruba a conexão**.

### 9.3 Sequestro de sessão TCP (TCP Session Hijacking)

- **Difícil de executar.** O atacante assume o lugar de um **host já autenticado** durante a comunicação com o alvo.
- Precisa:
  1. **Falsificar o IP** de um dos hosts
  2. **Prever o próximo número de sequência**
  3. Enviar um **ACK** para o outro host
- Se der certo, o atacante consegue **enviar**, mas **não receber**, dados do alvo.

> **Por que não recebe?** Porque as respostas do alvo vão para o IP real do host legítimo, não para o atacante.

---

## 10. UDP: cabeçalho e operação

- Protocolo da camada de transporte **não orientado à conexão**.
- **Sobrecarga muito menor** que o TCP (cabeçalho bem menor). **Não tem** retransmissão, sequenciamento nem controle de fluxo.
- Muito usado por: **DNS, DHCP, TFTP, NFS, SNMP**, além de aplicações em tempo real (**streaming, VoIP**).
- Ótimo para transações simples de requisição e resposta. Ex: usar TCP para DHCP geraria tráfego desnecessário. Se não vier resposta, o dispositivo reenvia a requisição.

> **"UDP é não confiável" não significa "UDP é ruim".** Só quer dizer que as funções de confiabilidade **não vêm no protocolo** e, se precisar, a aplicação implementa por conta própria.

**Nota:** tecnicamente o UDP trabalha com **datagramas**, mas o termo "segmento" é usado de forma genérica.

---

## 11. Ataques UDP

### Falta de criptografia e adulteração
- O UDP **não tem criptografia por padrão** (dá pra adicionar, mas não vem nativo).
- Qualquer pessoa pode **ver, alterar e reenviar** o tráfego.
- O **checksum de 16 bits** é **opcional**. Se for usado, o atacante pode **recalcular** o checksum com os dados alterados, e o destino não percebe a adulteração.
- Esse ataque **não é muito usado** na prática.

### Inundação UDP (UDP Flood)

O mais provável de aparecer.

1. Ferramentas como **UDP Unicorn** ou **Low Orbit Ion Cannon (LOIC)** mandam uma **enxurrada de pacotes UDP**, geralmente de um host falsificado, para um servidor da sub-rede.
2. O programa **varre todas as portas conhecidas** procurando portas fechadas.
3. O servidor responde com **ICMP port unreachable** para cada porta fechada.
4. Como são muitas portas fechadas, gera **muito tráfego** e consome quase toda a banda.

O resultado é parecido com um **DoS**.

---

## 12. Extras (complementos importantes)

### TCP x UDP

| | TCP | UDP |
|---|---|---|
| Conexão | Orientado à conexão (handshake) | Sem conexão |
| Confiabilidade | Sim (ACK, retransmissão) | Não (fica com a aplicação) |
| Sequenciamento e controle de fluxo | Sim | Não |
| Sobrecarga | Maior | Menor |
| Exemplos de uso | HTTP, HTTPS, FTP, SSH | DNS, DHCP, VoIP, streaming |
| Ataques típicos | SYN flood, reset, session hijacking | UDP flood, adulteração |

### Flags TCP mais cobradas

- **SYN:** inicia a conexão
- **ACK:** confirma o recebimento
- **FIN:** encerra a conexão de forma normal
- **RST:** encerra de forma abrupta
- Outras: **PSH** (entregar dados logo) e **URG** (dados urgentes)

### Como se defender (visão geral)

- **Anti-spoofing:** filtragem de entrada e saída (ingress/egress) para descartar pacotes com IP de origem impossível.
- **ACLs e firewalls** filtrando ICMP e tráfego indevido na borda.
- **SYN cookies** e limites de conexões semiabertas contra SYN flood.
- **Limitação de taxa (rate limiting)** contra floods (ICMP, UDP).
- **IDS/IPS** para detectar varreduras e padrões de ataque.
- **Segurança de portas e DHCP snooping** nos switches contra spoofing de MAC e DHCP falso.
- Desabilitar **broadcast direcionado** para evitar Smurf.

### Conectando com o módulo anterior

- **DoS/DDoS, SYN flood, UDP flood e Smurf** atacam a **disponibilidade**.
- **MiTM e sequestro de sessão** atacam **confidencialidade e integridade**.
- Sinais como um pico de SYN sem ACK ou muitos ICMP unreachable são **IOCs** que o analista de SOC observa.

---

## Resumão pra revisão rápida

- **IP** é sem conexão e **não valida a origem**, por isso o spoofing é possível.
- **IPv4** tem TTL e fragmentação nos roteadores. **IPv6** tem limite de saltos, cabeçalho mais simples e **não fragmenta** nos roteadores.
- **ICMP** é usado para reconhecimento (ping, unreachable, mask reply), DoS (flood), MiTM (redirect) e rotas falsas (router discovery).
- **Amplificação e reflexão** (Smurf, DNS, NTP) fazem terceiros atacarem a vítima. **Exaustão de recursos** trava o alvo.
- **Spoofing:** IP (cego para DoS, não cego para hijacking), **MAC** (engana a tabela CAM do switch) e **DHCP falso**.
- **TCP:** handshake de três vias. **SYN flood** deixa conexões semiabertas. **RST** derruba a conexão. **Session hijacking** exige prever o número de sequência.
- **UDP:** leve, sem conexão e sem criptografia. O ataque mais comum é o **UDP flood**, que gera respostas ICMP unreachable e consome a banda.
