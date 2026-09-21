# Atacando o que Fazemos

Resumo do módulo sobre ataques a serviços de rede (ARP, DNS, DHCP), serviços corporativos (web, e-mail, bancos de dados, scripts no cliente) e como mitigar os ataques mais comuns.

---

## 1. Serviços IP

### 1.1 ARP e suas vulnerabilidades

**ARP (Address Resolution Protocol):** descobre o **MAC** de um host a partir do **IP** dele.

**Como funciona:**
1. Um host manda uma **requisição ARP em broadcast** perguntando "quem tem o IP X?".
2. Todos na sub-rede recebem e processam.
3. Só o dono do IP responde com uma **resposta ARP**.

**ARP gratuito (gratuitous ARP):** resposta ARP **não solicitada**. Normalmente é usada quando um dispositivo liga, para avisar a rede do MAC dele. Quem recebe **guarda** o par IP/MAC na tabela ARP.

**O problema:** o ARP **não tem autenticação**. Qualquer host pode dizer que é dono de qualquer IP/MAC, e os outros acreditam.

> **Traduzindo:** o ARP é como um grupo de WhatsApp onde qualquer um pode falar "esse número agora é meu" e todo mundo atualiza a agenda sem conferir.

### 1.2 Envenenamento de cache ARP (ARP Poisoning)

O atacante **envenena o cache ARP** dos dispositivos da LAN para montar um ataque **MiTM**.

**Objetivo:** associar o **MAC do atacante** ao **IP do gateway padrão** nos caches dos hosts. Assim ele fica no meio entre a vítima e tudo que está fora da sub-rede.

**Passo a passo (exemplo com PC-A e o gateway R1):**
1. **Requisição ARP:** o PC-A quer o MAC do gateway (R1, 192.168.10.1) e manda um ARP request.
2. **Resposta ARP:** o R1 atualiza o cache dele com IP/MAC do PC-A e responde. O PC-A atualiza com o IP/MAC do R1. Tudo normal.
3. **Respostas falsas gratuitas:** o atacante manda **dois ARPs gratuitos falsificados** com o **próprio MAC**:
   - Pro PC-A: "o gateway é o meu MAC".
   - Pro R1: "o IP do PC-A é o meu MAC".
4. Resultado: todo o tráfego entre o PC-A e o gateway **passa pelo atacante**.

| Tipo | O que o atacante faz |
|---|---|
| **Passivo** | Só **rouba informações** sigilosas (espiona). |
| **Ativo** | **Modifica dados** em trânsito ou **injeta dados maliciosos**. |

**Ferramentas:** dsniff, Cain & Abel, ettercap, Yersinia.

**Defesa:** **DAI (Dynamic ARP Inspection)** nos switches, além de port security.

---

### 1.3 Ataques DNS

**DNS (Domain Name System):** traduz **nomes** (www.cisco.com) em **endereços IP**. Usa **registros de recursos (RR)** para identificar o tipo de resposta.

> A segurança do DNS costuma ser ignorada, mas ele é crucial para a rede funcionar.

**Categorias de ataque:**
- Resolvedor aberto
- Furtivos (stealth)
- Sombreamento de domínio
- Tunelamento

#### Ataques a resolvedores abertos

**Resolvedor aberto (open resolver):** servidor DNS que responde consultas de clientes **de fora do seu domínio administrativo** (ex: Google DNS `8.8.8.8`).

| Ataque | O que faz |
|---|---|
| **Envenenamento de cache DNS** | Manda **RRs falsos** ao resolvedor para redirecionar usuários de sites legítimos para **sites maliciosos**. Pode fazer o resolvedor usar um servidor de nomes malicioso. |
| **Amplificação e reflexão** | Usa o resolvedor aberto para **aumentar o volume** do DoS/DDoS e **esconder a origem**. O atacante manda consultas usando o **IP da vítima**, e o resolvedor responde pra ela. Funciona porque o resolvedor responde **qualquer um**. |
| **Utilização de recursos** | DoS que **consome todos os recursos** do resolvedor. Pode exigir reinicializar o serviço. |

#### Ataques DNS furtivos (stealth)

Técnicas para **esconder a identidade** do atacante.

| Técnica | Como funciona | Traduzindo |
|---|---|---|
| **Fast Flux** | Esconde sites de phishing e malware atrás de uma **rede de hosts DNS comprometidos** cujos IPs **mudam a cada poucos minutos**. Muito usado por **botnets**. | O site muda de endereço toda hora, então é difícil bloquear por IP. |
| **Double IP Flux** | Muda rapidamente o **mapeamento nome para IP** e também o **servidor de nomes autoritativo**. | Fast Flux com mais uma camada de confusão. |
| **DGA (Domain Generation Algorithms)** | O malware **gera domínios aleatórios** para usar como ponto de encontro com o servidor **C&C (comando e controle)**. | O malware tem milhares de "endereços de encontro" possíveis, e o atacante só registra alguns. |

#### Sombreamento de domínio (Domain Shadowing)

O atacante **coleta credenciais da conta de domínio** e cria **vários subdomínios silenciosamente**, apontando para **servidores maliciosos**, sem o dono do domínio pai perceber.

> **Traduzindo:** o atacante invade o painel do seu domínio e cria `promo.seusite.com` apontando pra um site golpista. Como o domínio pai é legítimo, ele tem boa reputação.

#### Tunelamento DNS (DNS Tunneling)

O atacante coloca **tráfego que não é DNS dentro de tráfego DNS**. Isso costuma **passar despercebido** por soluções de segurança, porque o DNS quase sempre é liberado.

- Usado por **botnets** para receber comandos e **exfiltrar dados**.
- Altera tipos de registro: **TXT, MX, SRV, NULL, A, CNAME**. O **TXT** é o mais fácil de usar para guardar comandos.

**Passo a passo (usando TXT):**
1. Os dados são **divididos em blocos codificados**.
2. Cada bloco vai em um **rótulo de subdomínio** da consulta DNS.
3. Como o DNS local não sabe responder, a consulta vai para os **servidores DNS recursivos do ISP**.
4. O DNS recursivo encaminha para o **servidor de nomes autoritativo do atacante**.
5. Repete até enviar todos os blocos.
6. O servidor do atacante responde cada consulta com **comandos encapsulados e codificados**.
7. O malware no host **recombina os pedaços** e executa os comandos.

**Como detectar e defender:**
- **Filtro que inspecione o tráfego DNS.**
- Atenção a **consultas DNS muito longas** e a **domínios suspeitos**.
- Domínios de **DNS dinâmico** devem ser considerados **altamente suspeitos**.
- Soluções como o **Cisco Umbrella** (antigo OpenDNS) bloqueiam boa parte do tunelamento.
- **Bloquear comunicações de saída** dos hosts infectados.

> **Por que é tão perigoso?** Quando o tráfego DNS aparece na investigação de um incidente, o ataque geralmente **já acabou**.

---

### 1.4 DHCP

**DHCP:** entrega **configuração IP dinamicamente** aos clientes (IP, máscara, gateway, DNS).

**Operação normal (DORA):**

| Etapa | Mensagem | Tipo |
|---|---|---|
| **D**iscover | Cliente procura um servidor DHCP | **Broadcast** |
| **O**ffer | Servidor oferece endereçamento | **Unicast** |
| **R**equest | Cliente aceita a oferta | **Broadcast** |
| **A**cknowledge | Servidor confirma | **Unicast** |

### 1.5 Ataque de falsificação de DHCP (DHCP Spoofing)

Um **servidor DHCP invasor (rogue)** é ligado na rede e dá **configurações IP falsas** aos clientes legítimos.

**O que o servidor falso pode entregar:**

| Informação falsa | Consequência |
|---|---|
| **Gateway padrão errado** | Aponta pro **IP do atacante** e cria um **MiTM**. Difícil de notar, porque o tráfego continua fluindo. |
| **Servidor DNS errado** | Leva o usuário para um **site malicioso**. |
| **Endereço IP errado** | IP ou gateway inválidos, gerando **DoS** no cliente. |

**Como o ataque acontece:**
1. O cliente manda o **Discover** em broadcast e **os dois servidores** (legítimo e falso) recebem.
2. Os dois respondem com **Offer**.
3. O cliente **aceita a primeira oferta** que chegar. Se for a do falso, ele manda o **Request** em broadcast.
4. **Só o servidor falso** manda o **ACK**. O legítimo para de falar com o cliente, porque a solicitação já foi atendida.

> **Traduzindo:** é uma corrida. Quem responder primeiro ganha o cliente.

**Defesa:** **DHCP snooping** no switch (só portas confiáveis podem responder como servidor DHCP).

---

## 2. Serviços corporativos

### 2.1 HTTP e HTTPS

Bloquear a navegação web **não é opção**, porque as empresas precisam de acesso à web. O jeito é **navegar com segurança**.

#### Etapas de um ataque web típico

1. A vítima visita, sem saber, uma **página comprometida** por malware.
2. A página **redireciona** o usuário (muitas vezes por vários servidores comprometidos) para um site com **código malicioso**.
3. O usuário visita o site e o computador é infectado (**drive-by download**). Um **exploit kit** varre o software da vítima (SO, Java, Flash) atrás de uma falha. O kit costuma ser um **script PHP** com um **console de gerenciamento** para o atacante.
4. Achando um software vulnerável, o exploit kit **baixa do servidor dele** o código que explora a falha.
5. Com o PC comprometido, ele **conecta ao servidor de malware** e baixa uma **carga útil (payload)**. Pode ser o malware em si ou um "downloader" que baixa outros.
6. O **malware final é executado**.

> **Traduzindo:** o objetivo do atacante é sempre o mesmo: fazer o navegador da vítima **chegar na página dele**, que serve o exploit.

**Detecção:** redes grandes usam **IDS** para verificar arquivos baixados. Se achar malware, gera **alerta** e **registra em log**.

#### Códigos de status HTTP

| Classe | Significado |
|---|---|
| **1xx Informativo** | Resposta provisória. |
| **2xx Sucesso** | Requisição recebida, entendida e aceita. |
| **3xx Redirecionamento** | O cliente precisa fazer outra ação. O cliente deve detectar loops infinitos de redirecionamento. |
| **4xx Erro do cliente** | O cliente parece ter errado (ex: 404 não encontrado, 403 proibido). |
| **5xx Erro do servidor** | O servidor errou ou não consegue atender (ex: 500). |

> Os **logs de conexão** ajudam a revelar o tipo de varredura ou ataque. Muitos 404 seguidos, por exemplo, podem indicar alguém **procurando páginas** no servidor.

#### Contramedidas contra ataques web
- Manter **SO e navegadores atualizados** (patches).
- Usar **proxy web** (ex: Cisco Cloud Web Security, Cisco Web Security Appliance) para bloquear sites maliciosos.
- Seguir as boas práticas da **OWASP** no desenvolvimento web.
- **Educar os usuários**.

> **OWASP Top 10:** lista das vulnerabilidades mais exploradas em aplicações web. Vale muito conhecer.

### 2.2 Exploits HTTP comuns

#### iFrames maliciosos

**iFrame:** elemento HTML que carrega **outra página dentro da sua página**, de outra fonte. É muito usado para exibir anúncios.

- O atacante **compromete um servidor web** e insere um iFrame malicioso apontando para o servidor dele.
- Às vezes o iFrame tem **poucos pixels**, então o usuário nem vê.
- Pode entregar **spam, exploit kit e outros malwares**.

**Prevenção:**
- Proxy web para bloquear sites maliciosos.
- **Evitar iFrames** nas páginas. Isso isola conteúdo de terceiros e facilita achar páginas modificadas.
- Cisco Umbrella para barrar sites conhecidamente maliciosos.
- Usuários precisam entender o que é um iFrame.

#### HTTP 302 Cushioning (amortecimento HTTP 302)

O atacante abusa do **redirecionamento HTTP legítimo**.

- O código **302 Found** manda o navegador para uma **nova URL** (campo `Location`).
- O atacante encadeia **vários redirecionamentos** até o navegador **cair na página com o exploit**.
- É **difícil de detectar**, porque redirecionamentos legítimos são comuns na rede.

**Prevenção:** proxy web, Cisco Umbrella e educar o usuário sobre a cadeia de redirecionamentos.

#### Sombreamento de domínio (na web)

Primeiro o atacante **compromete um domínio**. Depois usa **logins sequestrados do registro de domínio** para criar **vários subdomínios**. Se um subdomínio malicioso for descoberto e bloqueado, ele **cria outro** sem perder o domínio pai.

**Sequência típica:**
1. Um site é comprometido.
2. O **302 cushioning** envia o navegador para sites maliciosos.
3. O **domain shadowing** direciona para um servidor comprometido.
4. Acessa-se a **página inicial do exploit kit**.
5. O **malware é baixado** dessa página.

**Prevenção:**
- **Proteger contas de dono de domínio** com senha forte e **2FA**.
- Proxy web e Cisco Umbrella.
- Donos de domínio devem **auditar os subdomínios** e procurar algum que não autorizaram.

---

### 2.3 E-mail

O e-mail é a **espinha dorsal** da comunicação corporativa (mais de 100 bilhões de mensagens por dia). Hoje é acessado em **vários dispositivos, muitas vezes fora do firewall da empresa**, e o **HTML** permite mais ataques por poder driblar camadas de segurança.

| Ameaça | Como funciona |
|---|---|
| **Baseados em anexos** | Conteúdo malicioso dentro de arquivos de negócio (ex: um falso e-mail do TI). Costumam mirar um **setor específico** para parecerem legítimos. |
| **Falsificação de e-mail (spoofing)** | E-mail com **remetente forjado** para enganar o destinatário e obter dinheiro ou dados. Ex: "banco" pedindo para atualizar credenciais, com logotipo idêntico ao verdadeiro. |
| **Spam** | E-mails não solicitados com anúncios ou arquivos maliciosos. Muitas vezes servem para **confirmar que o e-mail é válido** quando alguém abre ou responde. |
| **Open Relay** | Servidor **SMTP mal configurado** que **deixa qualquer um na internet enviar e-mails** por ele. Vira ferramenta para spam e malware em massa. **Nunca configurar um servidor corporativo como open relay.** |
| **Homoglifos** | Caracteres **visualmente parecidos ou idênticos** aos legítimos, como `O` (letra) e `0` (zero), ou `l` (L minúsculo) e `1` (um). No DNS são caracteres diferentes, então o link leva a um **site totalmente outro**. |

> **Traduzindo (homoglifo):** `paypa1.com` com o número 1 no lugar do "l". Em uma leitura rápida, passa batido.

**Defesa:**
- Manter o **software SMTP atualizado**.
- Usar **appliance de segurança de e-mail** (ex: Cisco Email Security Appliance) contra phishing, spam e malware.
- **Educar o usuário final**, que é a **última linha de defesa**: reconhecer spam, phishing, links suspeitos, homoglifos e **nunca abrir anexos suspeitos**.

---

### 2.4 Bancos de dados expostos pela web

Aplicações web se conectam a **bancos relacionais**, que guardam dados sensíveis. Por isso são **alvos frequentes**.

#### Injeção de código (Code Injection)

O atacante executa **comandos no SO do servidor web** por meio de uma aplicação vulnerável.
- Acontece quando a aplicação tem **campos de entrada** e **não valida** o que recebe.
- Os comandos rodam com as **mesmas permissões da aplicação web**.
- Ex: injetar **código PHP** em um campo inseguro.

#### Injeção de SQL (SQL Injection)

**SQL** é a linguagem de consulta dos bancos relacionais. No ataque, o atacante **insere uma consulta SQL** pelos dados de entrada da aplicação.

**Um ataque bem-sucedido pode:**
- **Ler** dados sensíveis
- **Modificar** dados
- Executar **operações administrativas** no banco
- Às vezes, emitir **comandos para o SO**

**Por que funciona:** a aplicação aceita e processa a entrada do usuário **sem validação**.

> **Exemplo clássico:** no campo de login, digitar `' OR '1'='1` faz a consulta ficar sempre verdadeira e pode burlar a autenticação.

**Papel do analista de segurança:**
- **Reconhecer consultas SQL suspeitas** nos logs.
- Descobrir **qual ID de usuário** o atacante usou para logar.
- Identificar **que outros dados ou acessos** ele pode ter conseguido depois.

**Defesa (extra):** validação rigorosa de entrada, **consultas parametrizadas (prepared statements)** e privilégio mínimo no banco.

---

### 2.5 Scripts do lado do cliente: XSS

**XSS (Cross-Site Scripting):** scripts maliciosos são **injetados em páginas web** e **executados no navegador da vítima**. Podem ser em JavaScript, Visual Basic etc. Servem para acessar o computador, coletar informações sensíveis ou espalhar malware.

**Causa:** falta de **validação de entrada** em um site confiável onde o atacante consegue postar conteúdo. Os visitantes seguintes ficam expostos.

| Tipo | Como funciona |
|---|---|
| **Armazenado (persistente)** | O script fica **guardado no servidor** e atinge **todos os visitantes** da página infectada. |
| **Refletido (não persistente)** | O script está em um **link**. Só infecta quem **clicar** nele. |

> **Traduzindo:** no armazenado, o "veneno" fica no site esperando (ex: um comentário malicioso). No refletido, a vítima precisa ser convencida a clicar em um link armadilha.

**Prevenção:**
- Conscientizar **desenvolvedores** sobre XSS.
- **IPS** para detectar e evitar scripts maliciosos.
- Proxy web e Cisco Umbrella.
- Educar usuários para identificar phishing e **avisar o time de InfoSec** quando desconfiarem.

---

## 3. Mitigando ataques de rede comuns

### 3.1 Defendendo a rede (boas práticas)

- Ter uma **política de segurança escrita** na empresa.
- Educar funcionários sobre **engenharia social** e criar formas de **validar identidades** (telefone, e-mail, pessoalmente).
- **Controlar o acesso físico** aos sistemas.
- **Senhas fortes**, trocadas com frequência.
- **Criptografar e proteger com senha** dados sensíveis.
- Usar **firewalls, IPS, VPN, antivírus e filtragem de conteúdo**.
- Fazer **backups** e **testá-los** regularmente.
- **Desligar serviços e portas desnecessários.**
- Manter **patches atualizados** (semanal ou diariamente, se possível) para evitar buffer overflow e escalonamento de privilégio.
- Fazer **auditorias de segurança**.

### 3.2 Mitigando malware

Malware inclui **vírus, worms e cavalos de Troia**. As técnicas de mitigação também são chamadas de **contramedidas**.

- **Antivírus** é o produto de segurança **mais implantado** do mercado. Ele impede que hosts sejam infectados e espalhem código malicioso.
- Manter o antivírus **atualizado (definições e software)** dá muito menos trabalho do que limpar máquinas infectadas. A **atualização automática** é o requisito mais crítico e deve estar na política de segurança.
- **Limitação:** o antivírus é **baseado em host** e **não impede que vírus entrem na rede**.
- Outra camada: **dispositivos de segurança no perímetro** identificam malware conhecido pelos **IOCs** e removem os arquivos do fluxo de entrada.
- **Mas:** os atacantes **alteram o malware o suficiente** para escapar da detecção.

> **Conclusão importante:** nenhuma técnica de mitigação é 100% eficaz. **Incidentes vão acontecer.**

### 3.3 Mitigando worms

Worms são **mais baseados em rede** que vírus. A resposta tem **4 fases**:

| Fase | O que faz |
|---|---|
| **1. Contenção** | **Limita a propagação** do worm. Usa **compartimentação e segmentação** da rede e **ACLs** de entrada e saída em roteadores e firewalls. |
| **2. Inoculação** | Aplica o **patch do fornecedor** nos sistemas **não infectados**. Tira do worm os alvos disponíveis. Roda em paralelo ou depois da contenção. |
| **3. Quarentena** | **Rastreia e identifica** as máquinas infectadas dentro das áreas contidas e as **desconecta, bloqueia ou remove**. |
| **4. Tratamento** | **Desinfecta** os sistemas: encerra o processo do worm, remove arquivos e configurações alterados e **corrige a vulnerabilidade**. Em casos graves, **reinstala o sistema**. |

> **Macete:** **C**ontenção, **I**noculação, **Q**uarentena, **T**ratamento. Segurar o incêndio, vacinar os saudáveis, isolar os doentes e curar.

### 3.4 Mitigando ataques de reconhecimento

O reconhecimento é normalmente o **precursor de outros ataques**.

**Detecção:** **alarmes pré-configurados** disparados quando algum parâmetro é excedido (ex: número de pedidos ICMP por segundo). Ferramentas: **Cisco ASA** (prevenção de intrusão em dispositivo standalone) e roteadores **Cisco ISR** com software adicional.

**Formas de mitigar:**
- **Autenticação** para garantir acesso adequado.
- **Criptografia**, que deixa os dados capturados por sniffer ilegíveis.
- **Ferramentas anti-sniffer** (detectam hosts processando mais tráfego que o esperado, pela mudança no tempo de resposta).
- **Infraestrutura comutada (switches)**.
- **Firewall e IPS.**

**Sobre varredura de portas e ping:**
- É **impossível impedir** a varredura de portas, mas **IPS e firewall limitam** o que ela descobre.
- Pode-se parar **ping sweeps** desligando **echo e echo reply ICMP** nos roteadores de borda. O custo é **perder dados de diagnóstico** de rede.
- Port scans funcionam **mesmo sem ping sweep**. Só ficam **mais lentos**, porque varrem IPs inativos também.

### 3.5 Mitigando ataques de acesso

Muitos ataques de acesso são **adivinhação de senha** ou **dicionário e força bruta**.

**Política de autenticação forte:**
- **Senhas fortes:** mínimo de **8 caracteres**, com maiúsculas, minúsculas, números e caracteres especiais.
- **Bloquear contas** após um número de tentativas de login malsucedidas.

**Outras medidas:**
- **Princípio da confiança mínima:** sistemas **não devem confiar uns nos outros sem necessidade**. Ex: um servidor confiável não deve confiar incondicionalmente em servidores web (não confiáveis).
- **Criptografia** no acesso remoto e no tráfego de protocolos de roteamento. Quanto mais tráfego criptografado, menos chance de MiTM.
- **Protocolos de autenticação criptografados ou com hash**, junto de uma política de senha forte.
- **Educar** sobre engenharia social.
- **MFA (autenticação multifator):** dois ou mais meios **independentes** de verificação. Ex: senha + código por SMS, ou **tokens de uso único**. Impede o uso de senhas adivinhadas ou roubadas.

**Detecção:** revisar **logs, uso de banda e carga de processamento**. A política deve exigir **logs formais** de todos os dispositivos e servidores, para notar um número incomum de logins com falha.

### 3.6 Mitigando ataques DoS

**Primeiros sinais:** muitas reclamações de usuários sobre recursos indisponíveis ou **rede muito lenta**.

**O que fazer:**
- Manter um **software de utilização de rede sempre rodando**.
- Usar **análise de comportamento de rede** para detectar padrões incomuns.
- Um **gráfico de utilização** com atividade anormal também indica DoS.

**Por que preocupa:** o DoS pode ser **parte de uma ofensiva maior** e pode atingir também os **dispositivos de rede no caminho** (ex: estourar a capacidade de pacotes por segundo de um roteador). Em escala grande, pode afetar **regiões inteiras** de conectividade.

**Anti-spoofing** (muitos DoS vêm de endereços falsificados). Cisco oferece:
- **Port security**
- **DHCP snooping**
- **IP Source Guard**
- **DAI (Dynamic ARP Inspection)**
- **ACLs**

---

## 4. Extras (complementos importantes)

### Protocolos e o que protege cada um

| Ataque | Camada de defesa no switch/rede |
|---|---|
| **ARP poisoning** | **DAI** (Dynamic ARP Inspection) |
| **DHCP spoofing / rogue DHCP** | **DHCP snooping** |
| **MAC spoofing / flood de MAC** | **Port security** |
| **IP spoofing** | **IP Source Guard**, ACLs, filtragem ingress/egress |
| **DNS tunneling / DNS malicioso** | Inspeção de DNS, Cisco Umbrella, bloqueio de saída |

### DHCP Starvation

Além do servidor falso, existe o **DHCP starvation**: o atacante manda uma enxurrada de Discovers com **MACs falsos** para **esgotar o pool de IPs** do servidor. Aí sobra espaço para o **rogue DHCP** assumir. O **DHCP snooping** e o **port security** ajudam a barrar.

### Autenticação de e-mail (SPF, DKIM, DMARC)

Ajudam contra **spoofing de e-mail**:
- **SPF:** define **quais servidores** podem enviar e-mail pelo seu domínio.
- **DKIM:** **assina** a mensagem para provar que não foi alterada.
- **DMARC:** define a **política** (rejeitar, quarentena) quando SPF/DKIM falham.

### Como cada ataque afeta a tríade CIA

| Ataque | Princípio atingido |
|---|---|
| ARP poisoning, DHCP spoofing, MiTM | **Confidencialidade** e **integridade** |
| SQL injection, XSS, tunelamento DNS | **Confidencialidade** (vazamento de dados) e às vezes **integridade** |
| DoS, worms, resource exhaustion | **Disponibilidade** |

### Sinais que o analista de SOC vigia (IOCs)

- Consultas DNS muito **longas** ou para **domínios de DNS dinâmico**.
- Muitos códigos **404/403** de um mesmo IP nos logs web.
- Cadeias de **redirecionamentos 302**.
- Muitos **logins com falha** seguidos.
- Consultas SQL com padrões como `OR 1=1`.
- Respostas **DHCP vindas de um servidor desconhecido**.
- **Mudança repentina** de MAC associado ao IP do gateway (indício de ARP poisoning).

---

## Resumão pra revisão rápida

- **ARP** não tem autenticação, então dá pra **envenenar o cache** e fazer **MiTM**. Defesa: **DAI**.
- **DNS:** ataques a resolvedor aberto (envenenamento, amplificação, exaustão), **stealth** (Fast Flux, Double IP Flux, DGA), **domain shadowing** e **tunelamento** (dados escondidos em consultas, principalmente TXT).
- **DHCP spoofing:** servidor falso entrega gateway, DNS ou IP errados. O cliente aceita a **primeira oferta**. Defesa: **DHCP snooping**. Sequência normal: **DORA**.
- **Ataque web:** página comprometida, redirecionamentos, **drive-by download**, **exploit kit**, payload e malware.
- **Exploits HTTP:** **iFrame malicioso**, **302 cushioning** e **domain shadowing**.
- **E-mail:** anexos maliciosos, spoofing, spam, **open relay** e **homoglifos**. O usuário é a última linha de defesa.
- **SQL Injection** vem de falta de validação de entrada. **XSS** pode ser **armazenado** (persistente) ou **refletido** (via link).
- **Worms:** contenção, inoculação, quarentena, tratamento.
- **Reconhecimento:** não dá pra impedir port scan, dá pra limitar com IPS e firewall.
- **Ataques de acesso:** senha forte, bloqueio de conta, confiança mínima, criptografia e **MFA**.
- **DoS:** monitorar a rede e usar tecnologias **anti-spoofing** (port security, DHCP snooping, IP Source Guard, DAI, ACLs).
- **Nenhuma mitigação é 100%.** Incidentes vão acontecer.
