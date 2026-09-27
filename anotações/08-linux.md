# Linux Básico – Resumo

## O que é Linux?

Sistema operacional criado em 1991, open source, leve e altamente personalizável. Mantido por uma comunidade de programadores (diferente do Windows/Mac). Roda em qualquer coisa, de relógios a supercomputadores, e foi pensado desde o início pra funcionar bem em rede.

Distribuição (distro) = kernel Linux + ferramentas + pacotes de software, empacotados por diferentes organizações (Debian, Red Hat, Ubuntu, CentOS, SUSE...).

## Por que Linux é tão usado em SOC

- Open source: dá pra modificar livremente pra análise de segurança
- CLI muito poderosa: permite automação e acesso remoto
- Root tem controle total do sistema, inclusive sobre a pilha de rede
- Excelente controle sobre comunicação de rede

**Security Onion** é uma distro Linux voltada pra análise de segurança, reunindo ferramentas como:
- Captura de pacotes (ex: Wireshark)
- Análise de malware
- IDS (detecção de intrusão)
- Firewalls
- Gerenciador de logs
- SIEM
- Sistema de tickets

**Kali Linux** reúne várias ferramentas de pentest (geradores de pacotes, scanners de porta, exploits de prova de conceito).

## Shell e comandos básicos

Acesso à CLI via emulador de terminal (Terminator, xterm, gnome-terminal etc). Comando `man` mostra documentação de qualquer comando.

O shell procura os comandos digitados numa lista de diretórios chamada **path** (caminho). Se um comando não estiver no path, é preciso informar a localização completa dele, senão o shell não encontra. Dá pra adicionar diretórios ao path.

Comandos essenciais:

| Comando | Função |
|---|---|
| ls | lista arquivos do diretório |
| cd | muda de diretório |
| mkdir | cria diretório |
| cp | copia arquivos |
| mv | move/renomeia |
| rm | remove arquivos |
| cat | mostra conteúdo de um arquivo |
| grep | busca string em arquivo/saída |
| chmod | muda permissões |
| chown | muda dono do arquivo |
| pwd | mostra diretório atual |
| ps / top | lista processos |
| su / sudo | privilégios de superusuário |
| ifconfig / ip address | config de rede (ifconfig está obsoleto) |
| apt-get | gerenciador de pacotes Debian |

**nano** é um editor de texto de linha de comando (útil em conexões SSH sem GUI). `Ctrl+O` salva, `Ctrl+W` busca, `Ctrl+G` abre ajuda.

No Linux tudo é tratado como arquivo, inclusive dispositivos e configurações. Arquivos de configuração geralmente seguem o formato `opção = valor`, com `#` pra comentário. Exemplo: `/etc/nginx/nginx.conf`, `/etc/ntp.conf`, `/etc/snort/snort.conf`.

## Cliente-servidor e portas

Servidor = computador rodando software que oferece serviço a clientes via rede. Cada serviço "escuta" numa porta.

Portas conhecidas:

| Porta | Serviço |
|---|---|
| 20/21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 67/68 | DHCP |
| 69 | TFTP |
| 80 | HTTP |
| 110 | POP3 |
| 123 | NTP |
| 143 | IMAP |
| 161/162 | SNMP |
| 443 | HTTPS |

## Hardening de dispositivos

Boas práticas:
- Segurança física
- Minimizar pacotes instalados
- Desativar serviços não usados
- Usar SSH e desabilitar login root via SSH
- Manter sistema atualizado
- Desativar autodetecção de USB
- Senhas fortes e trocas periódicas, sem reutilização

## Logs no Linux

**Daemon**: processo em segundo plano que roda sem precisar de interação do usuário (ex: SSSD, que cuida de autenticação remota).

| Log | O que registra |
|---|---|
| /var/log/messages (ou /var/log/syslog no Debian) | eventos genéricos do sistema |
| /var/log/auth.log | autenticação (Debian/Ubuntu) |
| /var/log/secure | autenticação (RedHat/CentOS) |
| /var/log/boot.log | inicialização |
| /var/log/dmesg | mensagens do kernel sobre hardware |
| /var/log/kern.log | kernel |
| /var/log/cron | tarefas agendadas |
| /var/log/mysqld.log ou mysql.log | log do MySQL |

## Sistema de arquivos

Principais tipos:
- **ext2**: sem journaling, bom pra mídia flash
- **ext3**: adiciona journaling (registro de mudanças pra recuperação em caso de queda de energia), até 32TB por arquivo
- **ext4**: sucessor do ext3, mais performático
- **NFS**: sistema de arquivos via rede
- **CDFS**: pra mídia óptica
- **swap**: partição usada quando a RAM acaba
- **HFS+/APFS**: usados pela Apple
- **MBR**: guarda info de como o sistema de arquivos está organizado, no primeiro setor do disco

Montagem (mount) = associar uma partição a um diretório (ponto de montagem). Comando `mount` sem parâmetros lista tudo que está montado.

## Permissões de arquivo

`ls -l` mostra permissões no formato `-rwxrw-r--`.

Exemplo completo: `-rwxrw-r-- 1 analyst staff 253 May 20 12:49 space.txt`

Campos, na ordem:
1. Permissões (usuário, grupo, outros)
2. Número de links rígidos (hard links) pro arquivo
3. Dono do arquivo
4. Grupo dono do arquivo
5. Tamanho em bytes
6. Data/hora da última modificação
7. Nome do arquivo

Blocos de permissão:
- 1º bloco: dono (read/write/execute)
- 2º bloco: grupo
- 3º bloco: outros

Root pode sobrescrever qualquer permissão.

Valores octais:

| Octal | Permissão |
|---|---|
| 0 | --- |
| 1 | --x |
| 2 | -w- |
| 3 | -wx |
| 4 | r-- |
| 5 | r-x |
| 6 | rw- |
| 7 | rwx |

**Link rígido (hard link)**: outro nome apontando pro mesmo inode; se apagar o original, o link continua funcionando. Limitado ao mesmo sistema de arquivos, não funciona com diretórios.

**Link simbólico (symlink)**: aponta pro caminho do arquivo original; se apagar o original, o link quebra. Funciona entre sistemas de arquivos diferentes e pode apontar pra diretórios.

## GUI no Linux

Baseada no X Window System (X11), que cuida só da estrutura gráfica básica — quem define a aparência são os gerenciadores de janelas (Gnome, KDE). Por isso a GUI varia muito entre distros.

Ubuntu usa Gnome 3 por padrão, com componentes como Menu Apps, Dock, Barra superior, Atividades e Menu de Status.

## Gerenciadores de pacotes

| Tarefa | Arch | Debian/Ubuntu |
|---|---|---|
| Instalar pacote | pacman -S | apt install |
| Remover pacote | pacman -Rs | apt remove |
| Atualizar lista local | pacman -Syy | apt-get update |
| Atualizar tudo | pacman -Syu | apt-get upgrade |

## Processos

Processo = instância em execução de um programa. Fork = mecanismo do kernel pra um processo criar uma cópia de si mesmo (processo pai gera processo filho).

| Comando | Função |
|---|---|
| ps | lista processos em execução |
| top | lista processos em tempo real (sai com "q") |
| kill | mata, reinicia ou pausa um processo |

## Malware em Linux

Linux é considerado mais protegido (estrutura de arquivos, permissões, restrições de conta), mas não é imune. Vetor comum de ataque: serviços/processos desatualizados expostos em portas abertas (ex: versão vulnerável do Apache ou Nginx, descoberta via Telnet na porta 80).

**Rootkit**: malware que eleva privilégios ou mantém backdoor, alterando o próprio kernel. Difícil de detectar e difícil de remover (às vezes só reinstalando o SO; rootkits de firmware exigem troca de hardware).

Métodos de detecção (geralmente feitos a partir de mídia confiável/live CD, já que o sistema comprometido não é confiável):
- Baseado em comportamento
- Varredura de assinatura
- Varredura de diferenças (diff scanning)
- Análise de dump de memória

`chkrootkit` funciona como um script shell que usa ferramentas como `strings` e `grep` pra comparar assinaturas de programas contra as conhecidas, além de comparar o `/proc` com a saída do `ps`. Não é 100% confiável.

## Pipe (|)

Encadeia comandos, jogando a saída de um na entrada de outro. Exemplo: `ls -l | grep host` filtra a listagem só pra arquivos com "host" no nome.
