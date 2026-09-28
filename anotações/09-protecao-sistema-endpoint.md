# Proteção do Sistema e do Endpoint

## Segurança do Sistema Operacional

O que fortalece e mantém um SO seguro:

- **Bom administrador**: remove programas/serviços desnecessários e instala correções e atualizações de segurança em tempo hábil
- **Abordagem sistemática**: a empresa deve estabelecer procedimentos pra monitorar info de segurança, avaliar atualizações quanto à aplicabilidade, planejar a instalação de patches e instalar usando um plano documentado
- **Linha de base (baseline)**: parâmetro de comparação pra identificar vulnerabilidades, comparando o desempenho atual do sistema com o esperado

## Pontos importantes sobre antimalware

- **Antivírus invasores**: pop-ups falsos que imitam avisos do Windows, alertando sobre infecção e pedindo clique — na verdade instalam malware
- **Ataques sem arquivo (fileless)**: malware que roda direto na memória usando programas legítimos (ex: PowerShell), não deixa rastro em arquivo, e o ataque termina quando o sistema reinicia. Difícil de detectar
- **Scripts também são malware**: linguagens como Python, Bash e VBA (macros da Microsoft) podem ser usadas pra criar malware
- **Software não aprovado**: mesmo sem ser malicioso, pode violar política de segurança e deve ser removido

## Gerenciamento de Patches

**Patches** são atualizações de código dos fabricantes pra evitar ataques de vírus/worms recém-descobertos. Patches + atualizações geralmente vêm combinados em um **service pack**.

Boas práticas:
- Testar o patch antes de implantar em toda a empresa
- Usar ferramenta de gerenciamento de patches local em vez do serviço online do fornecedor

Benefícios de um serviço de patches automático/centralizado:
- Administradores podem aprovar ou recusar atualizações
- Podem forçar atualização até uma data específica
- Podem gerar relatórios do que falta atualizar em cada sistema
- Os computadores baixam do servidor local (não precisam ir no fornecedor)
- Usuários não conseguem desativar ou pular as atualizações

Também é importante manter atualizados aplicativos de terceiros (Adobe Acrobat, Java, Chrome), não só o SO.

## Segurança do Terminal (endpoint)

Soluções baseadas em host (rodam localmente no dispositivo):

| Solução | O que faz |
|---|---|
| Firewall baseado em host | Restringe tráfego de entrada/saída do próprio dispositivo (ex: Firewall do Windows) |
| HIDS (Host Intrusion Detection) | Monitora e analisa atividade suspeita no host; armazena logs localmente; não vê tráfego de rede que não chega no host |
| HIPS (Host Intrusion Prevention) | Monitora anomalias e pode alarmar, registrar, redefinir conexão ou descartar pacotes |
| EDR (Endpoint Detection and Response) | Monitora e coleta dados continuamente, analisa e responde a ameaças (antivírus só bloqueia, EDR também encontra ameaças) |
| DLP (Data Loss Prevention) | Evita perda, mau uso ou acesso não autorizado a dados sensíveis |
| NGFW (Next-Gen Firewall) | Combina firewall tradicional com outras funções, ex: inspeção profunda de pacotes (DPI) + IPS |

## Criptografia de Host

- **EFS** (Encrypting File System): criptografa arquivos, pastas ou disco inteiro no Windows
- **FDE** (Full Disk Encryption): criptografa todo o conteúdo da unidade, inclusive arquivos temporários e memória
- **BitLocker**: ferramenta do Windows pra FDE; exige ativar o **TPM** (Trusted Platform Module, chip da placa-mãe que guarda chaves de criptografia, certificados e senhas) na BIOS
- **BitLocker To Go**: criptografa unidades removíveis, não usa TPM, exige senha pra descriptografar
- **Unidade de autocriptografia**: criptografa automaticamente tudo que é gravado nela

## Integridade da Inicialização

- **Firmware**: instruções básicas armazenadas num chip da placa-mãe
- **BIOS**: primeiro programa executado ao ligar o computador
- **UEFI**: versão mais nova do BIOS, define interface padrão entre SO, firmware e dispositivos externos; roda em modo 64 bits (preferível ao BIOS)
- **Inicialização segura (Secure Boot)**: o firmware verifica a assinatura de cada parte do software de boot (drivers UEFI, apps UEFI, SO); se válida, o firmware passa o controle pro SO
- **Inicialização medida (Measured Boot)**: validação mais forte que a segura; mede cada componente (do firmware aos drivers) e guarda no TPM formando um log, que pode ser verificado remotamente; identifica apps não confiáveis e permite que o antimalware carregue mais cedo

## Recursos de Segurança da Apple

| Recurso | Função |
|---|---|
| Secure Enclave | Chip especial com CPU dedicada, boot próprio e criptografia AES em hardware |
| FileVault / Apple Data Protection | Criptografa/descriptografa arquivos em tempo real usando o AES de hardware, sem expor chaves à CPU/SO |
| Inicialização segura (ROM de boot) | Só permite rodar software Apple genuíno e não alterado |
| Dados biométricos protegidos | Processados em hardware separado do SO, isolados de malware |
| Encontre meu Mac | Rastreia, bloqueia remotamente e apaga dados de dispositivos perdidos/roubados |
| XProtect | Antimalware por assinatura, alerta e remove malware detectado |
| MRT (Malware Removal Tool) | Detecta e remove infecções atuais; regras atualizadas automaticamente pela Apple; monitora no boot e no login |
| Gatekeeper | Só permite instalar software assinado digitalmente por desenvolvedor autenticado pela Apple |

## Proteção Física de Dispositivos

- Bloqueios de cabo pra proteger equipamentos
- Salas de telecomunicações trancadas
- Gaiolas de Faraday (gaiolas de segurança) pra bloquear campos eletromagnéticos
- **Travas de porta**: trava de entrada com chave padrão (fácil de abrir, pode receber trava de bloqueio extra); **fechadura cifrada** usa sequência de botões, pode ser programada por dia/horário e registra logs de acesso
- **RFID**: usa ondas de rádio pra identificar/rastrear objetos; tags pequenas, sem bateria; ajuda a rastrear ativos e bloquear/desbloquear/configurar dispositivos sem fio

## Ameaças de Endpoint

**Endpoint** = qualquer host que acessa ou é acessado pela rede (computadores, servidores, câmeras IP, controladores, dispositivos IoT, dispositivos via VPN etc). Cada endpoint é uma porta de entrada possível pra malware.

Alguns dados que mostram a escala do problema:
- Previsão de um ataque de ransomware a cada 11 segundos até 2021
- Ataques de ransomware custariam US$ 6 trilhões/ano à economia global até 2021
- 8 milhões de tentativas de roubo de recursos via cryptomalware em 2018
- Volume global de spam malicioso girando em torno de 8 a 10%
- Previsão de aumento de ataques por dispositivo macOS de 4,8 (2018) pra 14,2 (2020)
- Muitos tipos de malware mudam recursos em menos de 24h pra evitar detecção

## Segurança de Endpoint (visão geral)

Ataques de rede externa comuns: DoS, desfiguração de servidor web, violação de servidores de dados pra roubo de informação.

Dispositivos de perímetro: roteador reforçado com VPN, firewall de próxima geração (NGFW/ASA), IPS, servidor AAA (autenticação, autorização e contabilidade).

Dois elementos internos de LAN a proteger:
- **Endpoints**: laptops, desktops, impressoras, servidores, telefones IP
- **Infraestrutura de rede**: switches, dispositivos sem fio e de telefonia IP, vulneráveis a ataques de estouro de tabela MAC, spoofing, DHCP, tempestade de LAN, manipulação de STP e VLAN

## Proteção contra Malware Baseada em Host

- **Antivírus/antimalware**: detecta e mitiga vírus/malware (Windows Defender, Cisco AMP, Norton, McAfee, Trend Micro etc). Três abordagens de detecção:
  - **Baseado em assinatura**: reconhece características de malware já conhecido
  - **Baseado em heurística**: reconhece traços gerais compartilhados entre famílias de malware
  - **Baseado em comportamento**: analisa comportamento suspeito
- **Antivírus baseado em agente**: roda em cada máquina protegida
- **Antivírus sem agente**: verificação centralizada, popular em ambientes virtualizados (ex: vShield da VMware)
- **Firewall de host**: restringe conexões iniciadas apenas por aquele host (Windows Defender Firewall, iptables, TCP Wrappers no Linux)
- **Suítes de segurança baseadas em host**: combinam antivírus, anti-phishing, navegação segura, HIPS e firewall, além de telemetria/logs centralizados

## Proteção contra Malware Baseada em Rede

Dispositivos que aplicam proteção no nível da rede, complementando a proteção de host:

- **AMP** (Advanced Malware Protection): proteção de endpoint contra vírus/malware
- **ESA** (Email Security Appliance): filtra spam e e-mails maliciosos antes de chegar ao endpoint
- **WSA** (Web Security Appliance): filtra sites, aplica lista negra e políticas de uso aceitável
- **NAC** (Network Admission Control): só permite conexão de sistemas autorizados e compatíveis

## Firewalls Baseados em Host

Programas independentes que controlam tráfego de entrada/saída, com políticas/perfis predefinidos e regras por endereço, protocolo ou porta. Podem emitir alertas de comportamento suspeito.

Logs de firewall costumam incluir: data/hora, se a conexão foi permitida ou negada, IPs de origem/destino, portas de origem/destino.

**Firewall distribuído**: combina firewalls de host com gerenciamento centralizado (envia regras, recebe logs).

Exemplos:
- **Windows Defender Firewall**: perfis Público (restritivo), Privado (rede isolada da internet) e Domínio (rede confiável)
- **iptables**: configura regras de rede via módulos Netfilter do kernel Linux
- **nftables**: sucessor do iptables, roda numa máquina virtual dentro do kernel Linux
- **TCP Wrappers**: registro e controle de acesso baseado em regras, filtragem por IP e serviço de rede

## Detecção de Intrusão Baseada em Host (HIDS)

HIDS protege contra malware conhecido e desconhecido: análise de log, correlação de eventos, verificação de integridade, aplicação de políticas, detecção de rootkit e alertas. É um sistema **baseado em agente** (roda direto no host) e combina antimalware + firewall.

Estratégias de detecção do HIDS:
- **Baseado em assinatura**: eficaz só contra ameaças conhecidas (não pega ameaças zero-day nem malware polimórfico, que muda a própria assinatura pra escapar da detecção)
- **Baseado em anomalias**: compara comportamento atual com uma linha de base normal; desvios grandes são tratados como possível intrusão (pode gerar muitos falsos positivos)
- **Baseado em política**: compara comportamento com regras predefinidas; violação aciona ação do HIDS (encerrar processo, registrar, alertar)

Produtos HIDS: Cisco AMP, AlienVault USM, Tripwire, **OSSEC** (open source; usa servidor gerenciador central + agentes em Mac/Windows/Linux/Solaris; monitora logs, verifica integridade de arquivo, detecta rootkits e pode disparar scripts em resposta a eventos).

## Segurança de Aplicações

**Superfície de ataque** = soma de todas as vulnerabilidades de um sistema acessíveis a um invasor (portas abertas, software voltado pra internet, protocolos sem fio, até usuários). Ela cresce com IoT, BYOD e uso de nuvem/dispositivos móveis.

Três componentes da superfície de ataque (segundo o SANS Institute):
- **Superfície de rede**: explora protocolos de rede com/sem fio e vulnerabilidades nas camadas de rede e transporte
- **Superfície de software**: explora vulnerabilidades em apps web, na nuvem ou baseados em host
- **Superfície humana**: explora comportamento do usuário (engenharia social, insider malicioso, erro humano)

## Lista de Bloqueio e Lista de Permissões

- **Lista de bloqueio (blacklist)**: define quais apps/sites não podem rodar/ser acessados
- **Lista de permissões (whitelist)**: define quais apps/sites podem rodar, com base numa linha de base de segurança da organização
- Listas negras de sites podem ser criadas manualmente ou vindas de serviços de segurança (ex: Cisco Talos, usado pelo Cisco Firepower)

## Sandboxing Baseado no Sistema

**Sandbox**: ambiente seguro isolado onde arquivos suspeitos são executados e analisados pra entender o comportamento do malware, já que malware polimórfico muda com frequência e sempre surge malware novo.

Ferramentas de sandbox:
- **Cisco AMP**: rastreia a trajetória de um arquivo na rede, pode reverter eventos e capturar o arquivo pra análise (ex: no Cisco Threat Grid Glovebox)
- **Cuckoo Sandbox**: sistema gratuito, pode rodar localmente
- **VirusTotal, Joe Sandbox, CrowdStrike Falcon Sandbox**: sandboxes públicas online
- **ANY.RUN**: sandbox online com relatórios interativos ricos (screenshots da execução, atividade de rede, requisições HTTP/DNS, hashes, visão hex/ASCII do arquivo, indicadores de comprometimento e mapeamento das táticas na Matriz MITRE ATT&CK)
