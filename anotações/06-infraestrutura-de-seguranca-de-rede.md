# Infraestrutura de segurança de rede

Resumo do módulo da Cisco Networking Academy sobre dispositivos e serviços que protegem e ajudam a monitorar uma rede.

## Dispositivos de segurança

### Firewalls

Um firewall é um sistema (ou grupo de sistemas) que aplica uma política de controle de acesso entre redes.

**O que todo firewall tem em comum**

- Resistem a ataques de rede
- Todo o tráfego entre a rede interna e a externa passa por ele
- Reforçam a política de controle de acesso

**Benefícios**

- Protegem hosts, recursos e aplicações sensíveis de usuários não confiáveis
- Sanitizam o fluxo do protocolo, evitando exploração de falhas
- Bloqueiam dados maliciosos vindos de servidores e clientes
- Reduzem a complexidade do gerenciamento de segurança

**Limitações**

- Firewall mal configurado pode virar um ponto único de falha
- Nem todo aplicativo passa com segurança por ele
- Usuários podem tentar contornar o firewall, expondo a rede
- Pode reduzir o desempenho da rede
- Tráfego não autorizado pode se esconder como tráfego legítimo

### Arquiteturas comuns

| Design | Como funciona |
| --- | --- |
| Privado e público | Duas interfaces. O tráfego da rede privada (confiável) pode sair e o retorno é permitido. O tráfego que vem da rede pública para a privada geralmente é bloqueado |
| DMZ | Três interfaces: interna, externa e DMZ. A DMZ recebe seletivamente tráfego da rede pública (e-mail, DNS, HTTP, HTTPS). O tráfego da DMZ para a rede privada é bloqueado |
| ZPF (firewall de política baseado em zona) | Interfaces com funções parecidas ficam agrupadas em uma zona. Dentro da mesma zona o tráfego passa livre. De zona para zona é tudo bloqueado, a menos que exista uma política liberando |

Detalhe do ZPF: a única exceção ao "bloqueia tudo entre zonas" é a **zona auto**, que é o próprio roteador e todos os IPs das interfaces dele. Por padrão não tem política pra ela, então é preciso pensar em tráfego de gerenciamento e controle (SSH, SNMP, protocolos de roteamento).

### Tipos de firewall

| Tipo | Resumo |
| --- | --- |
| Filtragem de pacotes (sem estado) | Decide com base nas camadas 3 e 4, consultando uma tabela de políticas. Exemplo: bloquear a porta 25 de uma estação pra ela não espalhar vírus por e-mail |
| Com estado (stateful) | O mais comum e versátil. Guarda informações das conexões em uma tabela de estado. Analisa até as camadas 4 e 5 |
| Gateway de aplicação (proxy) | Filtra nas camadas 3, 4, 5 e 7. O cliente se conecta ao proxy e o proxy se conecta ao servidor. O servidor só enxerga o proxy |
| Próxima geração (NGFW) | Vai além do stateful: prevenção de intrusão integrada, controle de aplicações, atualizações com feeds de ameaças |

Outras formas de implementar:

- **Baseado em host:** software de firewall rodando em um PC ou servidor
- **Transparente:** filtra tráfego IP entre um par de interfaces em ponte
- **Híbrido:** combinação de tipos (ex: stateful + gateway de aplicação)

## IDS e IPS

Os dois usam **assinaturas** (conjuntos de regras) pra detectar atividade maliciosa. Podem ser assinaturas atômicas (um pacote) ou compostas (vários pacotes). Funcionam como sensores, que podem ser um roteador com Cisco IOS IPS, um appliance dedicado ou um módulo em ASA, switch ou roteador.

| | IDS | IPS |
| --- | --- | --- |
| Posição | Fora do fluxo (modo offline) | Em linha (inline) |
| Vantagens | Não afeta o desempenho da rede. Se o sensor falhar, a rede continua funcionando | Consegue parar o pacote que disparou o alerta. Usa normalização de fluxo |
| Desvantagens | Não para o pacote de gatilho. Ajuste demorado. Mais vulnerável a técnicas de evasão | Falha ou sobrecarga do sensor afeta a rede. Pode causar latência e jitter |

- **Normalização de fluxo:** técnica pra reconstruir o fluxo de dados quando o ataque vem espalhado em vários segmentos
- IDS e IPS podem ser usados juntos. Por exemplo, o IDS faz uma inspeção mais profunda offline pra validar o IPS, que fica focado nos padrões mais críticos
- Um IPS precisa ser bem dimensionado pra não prejudicar aplicações sensíveis ao tempo, como VoIP

### Tipos de IPS

**HIPS (baseado em host):** software instalado no host que monitora o sistema operacional e processos críticos. Pode ser visto como antivírus + antimalware + firewall juntos.

- Vantagens: proteção específica pro SO, em nível de aplicativo e sistema, e protege o host depois que a mensagem é descriptografada
- Desvantagens: depende do sistema operacional e precisa ser instalado em todos os hosts. Só enxerga o que acontece localmente

**IPS baseado em rede:** sensores em pontos designados da rede, detectando atividade maliciosa em tempo real. O ideal é integrar soluções de host com o IPS de rede.

## Appliances de segurança especializados

- **AMP (Advanced Malware Protection):** proteção contra malware antes, durante e depois do ataque. Continua monitorando arquivos mesmo depois da inspeção inicial e alerta se um arquivo "bom" começar a se comportar mal. Usa a inteligência do Cisco Talos
- **WSA (Web Security Appliance):** gateway web seguro. Bloqueia sites arriscados, testa sites desconhecidos e aplica políticas de uso aceitável. Não protege quem está fora da rede protegida (ex: Wi-Fi público), e pra isso existe o **Cisco Cloud Web Security (CWS)**
- **ESA (Email Security Appliance):** defende o e-mail contra ameaças. Recursos: inteligência global contra ameaças (Talos), bloqueio de spam em camadas, proteção avançada contra malware (AMP) e controle de mensagens de saída

## Serviços de segurança

### ACLs

Uma ACL é uma série de comandos que controla se o dispositivo encaminha ou descarta pacotes com base no cabeçalho do pacote.

**Para que servem**

- Limitar tráfego e melhorar o desempenho
- Controlar o fluxo de tráfego (ex: aceitar atualizações de roteamento só de fontes conhecidas)
- Dar um nível básico de segurança de acesso
- Filtrar por tipo de tráfego (ex: permitir e-mail e bloquear Telnet)
- Permitir ou negar acesso a serviços como FTP e HTTP
- Classificar tráfego pra dar prioridade (tipo ingresso VIP)

**Tipos**

- **Padrão:** filtra só pelo endereço IPv4 de origem
- **Estendida:** filtra por protocolo, IP de origem e destino, portas TCP/UDP de origem e destino e informações opcionais

As duas podem ser **numeradas** ou **nomeadas**. Numeradas funcionam bem em redes pequenas, mas o número não diz nada sobre o propósito, então o nome ajuda. Também dá pra configurar log de ACL e liberar só tráfego TCP de sessões já estabelecidas (bits ACK ou RST).

### SNMP

Protocolo da camada de aplicação que permite gerenciar dispositivos (servidores, roteadores, switches, firewalls) em uma rede IP.

- **Gerenciador SNMP:** roda o software de gerenciamento (faz parte do NMS)
- **Agentes SNMP:** os dispositivos monitorados
- **MIB:** banco de dados do agente com dados e estatísticas do dispositivo
- **get:** o gerenciador coleta informação do agente
- **set:** o gerenciador altera configuração no agente
- **trap:** o agente manda informação sozinho pro gerenciador

### NetFlow

Tecnologia Cisco IOS que gera estatísticas sobre pacotes IP que passam por roteadores e switches multicamadas. Serve pra monitoramento, segurança, planejamento, análise de tráfego (achar gargalos) e contabilidade IP. As estatísticas vão pra um servidor chamado **coletor NetFlow**.

Um fluxo é identificado por 7 campos. Se qualquer um mudar, é outro fluxo:

- IP de origem
- IP de destino
- Porta de origem
- Porta de destino
- Tipo de protocolo da camada 3
- Marcação ToS (tipo de serviço)
- Interface lógica de entrada

### Espelhamento de portas (port mirroring)

Como o switch isola o tráfego, um sniffer ou IDS não enxerga tudo em um segmento. O espelhamento faz o switch copiar o tráfego e mandar a cópia por uma porta com um monitor conectado. O tráfego original segue normalmente.

### Syslog

Protocolo mais comum pra receber mensagens de sistema de roteadores, switches, firewalls e outros dispositivos em um servidor syslog. Três funções principais:

- Coletar informações de log pra monitorar e solucionar problemas
- Escolher que tipo de informação é capturada
- Definir o destino das mensagens capturadas

### NTP

Sincronizar a hora em todos os dispositivos é essencial, porque sem isso não dá pra saber a ordem dos eventos em diferentes partes da rede. Ajustar manualmente não escala, então o ideal é usar o NTP, sincronizando com um relógio mestre privado ou um servidor público.

A hierarquia é dividida em **estratos** (stratum), que contam os saltos até a fonte oficial de horário:

- **Estrato 0:** fontes de tempo de alta precisão
- **Estrato 1:** ligados diretamente às fontes do estrato 0, são o padrão principal de horário da rede
- **Estrato 2 e abaixo:** sincronizam com o estrato acima e podem servir o estrato seguinte

Número menor significa mais perto da fonte oficial. O máximo é 15 saltos e o **estrato 16 indica dispositivo não sincronizado**.

### AAA

- **Autenticação:** provar quem é (usuário e senha, desafio e resposta, tokens)
- **Autorização:** o que o usuário autenticado pode acessar e fazer (ex: "o usuário aluno só acessa o servidor XYZ via SSH")
- **Accounting:** registrar o que o usuário fez, por quanto tempo e o que mudou

**TACACS+ vs RADIUS** (protocolos usados pra falar com servidores AAA):

| | TACACS+ | RADIUS |
| --- | --- | --- |
| Padrão | Principalmente Cisco | Aberto (RFC) |
| Transporte | TCP | UDP |
| Criptografia | Pacote inteiro | Só a senha |
| AAA | Separa autenticação, autorização e accounting | Combina autenticação e autorização |
| Autorização de comandos | Por usuário ou grupo | Não tem |
| Accounting | Limitado | Abrangente |

O TACACS+ é considerado mais seguro por criptografar tudo.

### VPN

Rede privada criada em cima de uma rede pública (geralmente a internet). É **virtual** porque usa conexões virtuais em vez de link físico dedicado, e **privada** porque o tráfego é criptografado.

- As primeiras VPNs eram túneis IP sem autenticação nem criptografia. Exemplo: **GRE**, protocolo da Cisco que encapsula vários protocolos dentro de túneis IP
- Exemplos de VPN de camada 3: GRE, MPLS e IPsec
- **IPsec:** conjunto de protocolos que oferece autenticação, integridade, controle de acesso e confidencialidade
- **Site a site:** conecta escritórios (central e remotos)
- **Acesso remoto:** dá acesso seguro a quem viaja ou trabalha de casa

---

## Resumão

| Tema | Ideia principal |
| --- | --- |
| Firewall | Aplica a política de controle de acesso entre redes. Tipos: filtragem de pacotes (sem estado), com estado, gateway de aplicação (proxy) e próxima geração (NGFW) |
| Arquiteturas | Privado/público, DMZ e ZPF (política baseada em zona) |
| IDS | Fora do fluxo, só detecta e alerta. Não afeta a rede, mas não para o pacote de gatilho |
| IPS | Em linha, detecta e bloqueia. Pode causar latência e jitter |
| AMP / WSA / ESA | Proteção contra malware, tráfego web e e-mail |
| ACL | Permite ou nega pacotes pelo cabeçalho. Padrão filtra só pela origem, estendida filtra por protocolo, origem, destino e portas |
| SNMP | Gerenciamento de dispositivos (get, set e trap) |
| NetFlow | Estatísticas de fluxos IP, enviadas a um coletor |
| Espelhamento de portas | Switch copia o tráfego pra uma porta com sniffer ou IDS |
| Syslog | Centraliza as mensagens de sistema dos dispositivos |
| NTP | Sincroniza a hora da rede por estratos (0 a 15, e 16 é não sincronizado) |
| AAA | Autenticação, autorização e accounting, via TACACS+ ou RADIUS |
| VPN | Rede privada sobre rede pública. Site a site ou acesso remoto, com IPsec |

### Comparações que mais caem

| | IDS | IPS |
| --- | --- | --- |
| Posição | Offline | Inline |
| Bloqueia o pacote | Não | Sim |
| Impacto na rede | Nenhum | Latência e jitter |

| | TACACS+ | RADIUS |
| --- | --- | --- |
| Transporte | TCP | UDP |
| Criptografia | Pacote inteiro | Só a senha |
| Padrão | Cisco | Aberto (RFC) |
