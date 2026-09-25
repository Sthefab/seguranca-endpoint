# O Sistema Operacional Windows

Resumo sobre histórico, arquitetura, configuração e segurança do Windows.

## 1. Histórico do Windows

### Sistema Operacional de Disco (DOS)
- Antes dos discos modernos, dados eram armazenados em cartão perfurado, fita de papel, fita magnética e cassete
- **DOS** é o SO que permite ler/gravar em dispositivos de armazenamento. A Microsoft comprou e criou o **MS-DOS**
- Interface só por linha de comando

Comandos básicos do MS-DOS (ainda funcionam no `cmd` do Windows):

| Comando | Função |
| --- | --- |
| `dir` | Lista arquivos do diretório atual |
| `cd diretorio` | Muda de diretório |
| `cd ..` | Sobe um nível |
| `cd \` | Vai pro diretório raiz |
| `copy origem destino` | Copia arquivos |
| `del arquivo` | Exclui arquivo |
| `find` | Procura texto em arquivos |
| `mkdir diretorio` | Cria diretório |
| `ren antigo novo` | Renomeia arquivo |
| `help` / `help comando` | Mostra ajuda |

- Windows 1.0 (1985) era uma GUI rodando sobre o MS-DOS
- Windows moderno (a partir do NT) não é mais um SO de disco: o próprio SO controla o hardware direto e suporta múltiplos usuários/processos

### Versões do Windows
- Desde 1993, mais de 20 versões baseadas no **NT** (New Technology)
- A partir do XP existe edição de **64 bits**: espaço de endereço muito maior (32 bits ≈ 4 GB de RAM; 64 bits ≈ 16,8 milhões de TB, na teoria)
- Programas de 32 bits rodam em 64 bits, mas o contrário não funciona
- Cada versão trouxe mais edições (Windows 7: 6 edições, Windows 10: 8 edições)

### Windows GUI
- **Área de Trabalho:** personalizável, guarda arquivos, pastas, atalhos e a Lixeira
- **Barra de Tarefas:** menu Iniciar (esquerda), ícones de inicialização rápida (centro), área de notificação (direita)
- **Menu de Contexto:** clique direito mostra ações rápidas (copiar, excluir, compartilhar etc.)
- Gerenciamento de arquivos feito pelo **Explorador de Arquivos**

### Vulnerabilidades comuns do SO
- Antivírus/malware desativado (Windows Defender)
- Serviços desconhecidos rodando em segundo plano
- Falta de criptografia dos dados
- Política de segurança mal configurada
- Firewall com regras desatualizadas
- Permissões de arquivo/compartilhamento mal definidas (ex: dar "Controle Total" pro grupo "Todos")
- Senha fraca ou ausente
- Uso do login como Administrador no dia a dia (ideal é usar Usuário Padrão)

## 2. Arquitetura e operações do Windows

### HAL (Hardware Abstraction Layer)
Camada de software que isola o kernel das diferenças de hardware. O **kernel** é o núcleo do SO, controla entrada/saída, memória e periféricos.

### Modo Usuário x Modo Kernel
| | Modo Kernel | Modo Usuário |
| --- | --- | --- |
| Acesso ao hardware | Irrestrito | Só via SO |
| Falha | Trava o sistema todo | Só derruba o app |
| Quem roda | Código do SO | Aplicativos |

Drivers podem rodar em modo kernel ou usuário, dependendo do tipo.

### Sistemas de arquivos
| Sistema | Uso |
| --- | --- |
| **exFAT/FAT16/FAT32** | Simples, compatível com vários SOs, mas com limitações (não usado mais em HD/SSD) |
| **HFS+** | Usado no Mac OS X. Windows só lê, com software especial |
| **EXT** | Usado no Linux. Windows só lê, com software especial |
| **NTFS** | Padrão do Windows moderno. Suporta arquivos/partições grandes, criptografia, permissões e timestamps (MACE) |

Estruturas do NTFS: setor de inicialização, **MFT** (Master File Table), arquivos de sistema e área de arquivos.

**Atenção:** formatar não apaga os dados de verdade — é preciso apagamento seguro pra sobrescrever tudo.

### Fluxos de Dados Alternativos (ADS)
- NTFS permite anexar dados extras a um arquivo via `arquivo.txt:ADS`
- Usado por apps legítimos, mas também é forma comum de esconder malware, já que o `dir` normal não mostra o ADS (precisa do `dir /r`)

### Processo de inicialização
1. **BIOS**: POST testa hardware, depois busca o **MBR**, que localiza o SO
2. **UEFI**: mais moderno, carrega arquivos `.efi` da partição **ESP**, entra direto em modo protegido (mais seguro)
3. `Bootmgr.exe` muda pro modo protegido e lê o **BCD**
4. Se saindo de hibernação → `Winresume.exe` (lê `Hiberfil.sys`)
5. Se boot frio → `Winload.exe` (verifica assinatura dos drivers via **KMCS**)
6. `Winload.exe` chama `Ntoskrnl.exe` (inicia o kernel e o HAL)
7. **SMSS** prepara o ambiente do usuário e inicia o Winlogon

Chaves de registro usadas na inicialização: **HKEY_LOCAL_MACHINE** (serviços gerais) e **HKEY_CURRENT_USER** (serviços do usuário logado). Gerenciadas com `Msconfig.exe`.

### Desligamento
Opções: Desligar, Reiniciar, Hibernar. Sempre fechar processos de modo usuário antes dos de modo kernel — desligamento forçado pode corromper arquivos.

### Processos, threads e serviços
- **Processo:** programa em execução
- **Thread:** parte executável de um processo (processador roda cálculos na thread)
- **Serviço:** processo em segundo plano dando suporte ao SO/apps, pode iniciar automático ou manual

### Memória
- Processo de 32 bits: até 4 GB de espaço de endereço virtual
- Processo de 64 bits: até 8 TB
- Processos de usuário usam espaço isolado; acesso ao kernel só via identificador de processo
- Ferramenta útil: **RAMMap** (Sysinternals)

### Registro do Windows
Banco de dados hierárquico (ramo > chave > subchave > valor), editado com `regedit.exe`.

| Hive | Conteúdo |
| --- | --- |
| **HKCU** | Usuário conectado no momento |
| **HKU** | Todas as contas de usuário |
| **HKCR** | Registros OLE |
| **HKLM** | Configurações do sistema |
| **HKCC** | Perfil de hardware atual |

Tipos de valor: `REG_BINARY`, `REG_DWORD`, `REG_SZ`. Malware costuma usar entradas de inicialização no registro pra rodar escondido (ex: keylogger).

## 3. Configuração e monitoramento

### Executar como Administrador
Melhor prática: não logar como Admin no dia a dia. Usar "Executar como Administrador" só quando necessário (clique direito no app ou no `cmd`).

### Usuários, grupos e domínios
- Contas: usuário local, Convidado e Administrador (as duas últimas desativadas por padrão)
- **Grupos:** conjunto de permissões aplicadas a vários usuários de uma vez (gerenciados em `lusrmgr.msc`)
- **Domínio:** rede onde usuários/computadores são autenticados e controlados por um **controlador de domínio (DC)**

### CLI e PowerShell
- `cmd.exe`: comandos básicos, não diferencia maiúsc/minúsc, usa `/` pra opções, `Tab` autocompleta
- **PowerShell**: mais poderoso, roda **cmdlets**, scripts `.ps1` e funções. Ajuda: `get-help`, `-examples`, `-detailed`, `-full`

### WMI (Windows Management Instrumentation)
Usado pra gerenciar/monitorar computadores remotos. Também é abusado por atacantes pra se mover lateralmente sem deixar rastro fácil — por isso o acesso deve ser bem restrito.

### Comando net
`net help` lista subcomandos como `net accounts`, `net session`, `net share`, `net start/stop`, `net use`, `net view`.

### Gerenciador de Tarefas x Monitor de Recursos
- **Gerenciador de Tarefas:** processos, desempenho, histórico de apps, itens de inicialização, usuários, detalhes e serviços
- **Monitor de Recursos:** visão mais profunda de CPU, memória, disco e rede — bom pra achar processo suspeito se comunicando pela rede

### Rede
- **Centro de Rede e Compartilhamento:** configura adaptadores, tipo de rede (privada/pública), IP (via TCP/IPv4 ou IPv6)
- `netsh` configura rede via linha de comando
- `nslookup` testa resolução DNS
- `netstat` mostra conexões ativas e portas abertas

### Acesso a recursos de rede
- **SMB** + formato **UNC** (`\\servidor\compartilhamento\arquivo`) pra acessar arquivos remotos
- **Compartilhamentos administrativos:** identificados por `$` (ex: `C$`, `admin$`), só acessíveis com privilégio de admin
- **RDP:** acesso remoto a área de trabalho — alvo comum de ataque, deve ter exposição limitada à internet

### Windows Server
Versão voltada a datacenter, oferece: serviços de rede (DNS, DHCP, Hyper-V), arquivo (SMB, NFS, DFS), web (FTP, HTTP/HTTPS) e gerenciamento (Política de Grupo, Active Directory).

## 4. Segurança do Windows

### Comando netstat (investigação de malware)
`netstat -abno` (como admin) mostra conexões TCP ativas + PID do processo responsável — útil pra achar malware escutando portas não autorizadas. Depois é só cruzar o PID com o Gerenciador de Tarefas.

### Visualizador de Eventos
Registra logs de aplicativo, segurança e sistema, com níveis (informação, aviso, erro, crítico). Dá pra criar visualizações personalizadas; existe uma pronta chamada **Eventos administrativos**.

### Atualizações do Windows
- **Patches** corrigem vulnerabilidades específicas; **service packs** juntam vários patches
- Protegem contra **ataques de dia zero** (exploração antes de existir correção)
- Configuração feita em Windows Update

### Política de Segurança Local
Usada em máquinas fora de domínio. Principais itens:
- **Política de senha:** define requisitos de senha
- **Política de bloqueio de conta:** bloqueia após X tentativas erradas (proteção contra força bruta)
- Bloqueio automático de tela após inatividade
- Dá pra exportar a política (`.inf`) e aplicar em outras máquinas

### Windows Defender
Proteção nativa contra malware, ativado por padrão. Tipos de proteção: antivírus, antiadware, antiphishing, antispyware, fontes confiáveis/não confiáveis. Recomendação: rodar só um programa de proteção por vez pra não conflitar.

### Windows Defender Firewall
- Política **restritiva** (só libera o necessário) é mais segura que **permissiva** (libera tudo, exceto o que é negado)
- Configurado em Painel de Controle > Sistema e Segurança > Firewall do Windows Defender
- Configurações avançadas permitem regras de entrada/saída, importar/exportar políticas

## Resumo rápido

- Windows moderno = NT, não é mais um SO de disco
- NTFS é o sistema de arquivos padrão, com suporte a segurança, ADS e recuperação
- Boot: BIOS/UEFI → MBR/ESP → Bootmgr → Winload → Ntoskrnl → SMSS
- Modo kernel = acesso total (falha derruba tudo); modo usuário = isolado (falha só derruba o app)
- Ferramentas-chave de investigação: Gerenciador de Tarefas, Monitor de Recursos, netstat, Visualizador de Eventos, WMI
- Segurança básica: não logar como admin, senha forte, firewall restritivo, atualizações em dia, Defender ativo
