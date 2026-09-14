# Ameaças, vulnerabilidades e ataques a segurança cibernética

Notas de estudo completas sobre domínios de ameaça, tipos de ataque, malware, engenharia social e técnicas de defesa em segurança cibernética.

## Domínios de ameaça

Um domínio de ameaça é uma área de controle, autoridade ou proteção que invasores podem explorar para obter acesso a um sistema. Formas comuns de exploração:

- Acesso direto e físico a sistemas e redes
- Redes sem fio que ultrapassam os limites físicos da empresa
- Bluetooth ou NFC (comunicação de campo próximo)
- Anexos de email maliciosos
- Elos fracos na cadeia de fornecimento
- Contas de mídia social da empresa
- Mídia removível (pen drives, HDs externos)
- Aplicativos baseados em nuvem

## Categorias de ameaças

Classificar ameaças ajuda a empresa a priorizar esforços de segurança com base em probabilidade e impacto financeiro.

| Categoria | Exemplos |
|---|---|
| Ataques de software | DoS bem-sucedido, vírus de computador |
| Erros de software | Bug de software, aplicativo fora do ar, XSS, compartilhamento ilegal de arquivos |
| Sabotagem | Comprometimento de banco de dados por usuário autorizado, desfiguração de site |
| Erro humano | Erro de entrada de dados, firewall mal configurado |
| Roubo | Laptops/equipamentos roubados de salas destrancadas |
| Falhas de hardware | Disco rígido trava |
| Interrupção de utilidade | Queda de energia, dano por água (falha de sprinkler) |
| Desastres naturais | Furacões, terremotos, inundações, incêndios |

## Ameaças internas vs externas

- **Internas**: funcionários atuais/antigos ou parceiros de contrato que, acidental ou intencionalmente, manipulam dados sensíveis ou comprometem infraestrutura (ex: conectar mídia infectada, acessar sites maliciosos).
- **Externas**: invasores amadores ou qualificados que exploram vulnerabilidades de rede ou usam engenharia social para obter acesso.

## Domínio do usuário

O usuário é considerado o elo mais fraco da segurança da informação. Riscos principais:

- **Falta de conscientização de segurança**: usuário não entende políticas, dados sensíveis ou contramedidas em vigor
- **Políticas aplicadas incorretamente**: falta de clareza sobre consequências da não conformidade
- **Roubo de dados**: gera dano reputacional e responsabilidade legal
- **Downloads e mídias não autorizados**: infecções rastreadas a downloads de músicas, vídeos, apps não autorizados
- **VPNs não autorizadas**: a criptografia pode esconder roubo de dados de administradores de rede
- **Sites não autorizados**: podem solicitar download de scripts/plugins maliciosos ou até assumir controle de câmeras e outros dispositivos
- **Destruição de sistemas/dados**: sabotagem por ativistas, funcionários insatisfeitos ou concorrentes

> Não existe solução técnica que torne um sistema mais seguro do que o comportamento das pessoas que o usam.

## Ameaças por domínio de rede

**Dispositivos**: dispositivos ligados e sem supervisão, downloads de fontes não confiáveis, exploração de vulnerabilidades de software não corrigido, uso de mídia removível não autorizada, hardware/software desatualizado.

**LAN (rede local)**: acesso não autorizado a data centers/armários de fiação, vulnerabilidades de SO de rede, usuários desonestos em redes sem fio, hardware/SO inconsistentes entre servidores, sondagem/varredura de portas não autorizada, firewalls mal configurados.

**Nuvem privada**: sondagem e varredura de portas, acesso não autorizado a recursos, vulnerabilidades de firewall/roteador, erros de configuração, usuários remotos baixando dados sensíveis.

**Nuvem pública**: recursos compartilhados entre organizações via provedor de internet. Três modelos de serviço:
- **SaaS** (Software como Serviço): software hospedado e acessado via navegador/app, não armazenado localmente
- **PaaS** (Plataforma como Serviço): plataforma para desenvolver/rodar/gerenciar aplicações no hardware do provedor
- **IaaS** (Infraestrutura como Serviço): recursos de computação virtual (hardware, servidores, armazenamento) via internet

**Aplicações**: acesso não autorizado a data centers/sistemas, tempo de inatividade em manutenção, vulnerabilidades de SO de rede, perda de dados, falhas em desenvolvimento de apps cliente-servidor ou web.

## Complexidade da ameaça

- **APT (Advanced Persistent Threat)**: ataque contínuo e elaborado, envolvendo múltiplos agentes e/ou malware sofisticado, que opera "sob o radar" por longos períodos. Geralmente visa governos e grandes organizações, bem financiado e orquestrado.
- **Ataques de algoritmo**: exploram algoritmos de software legítimo para gerar comportamento indesejado — por exemplo, forçar uso excessivo de RAM/CPU para travar um sistema ou disparar alertas falsos.

## Backdoors e rootkits

- **Backdoors**: programas (ex: Netbus, Back Orifice) que dão acesso não autorizado ignorando autenticação normal. Frequentemente instalados via ferramentas de administração remota (RAT). Permitem acesso contínuo mesmo após a vulnerabilidade original ser corrigida.
- **Rootkits**: modificam o sistema operacional para criar um backdoor. Exploram vulnerabilidades para escalar privilégios e podem modificar ferramentas forenses/monitoramento, tornando-se muito difíceis de detectar. Geralmente exigem reinstalação completa do sistema.

## Inteligência de ameaças e fontes de pesquisa

- **CVE (Common Vulnerabilities and Exposures)**: dicionário de vulnerabilidades mantido pela MITRE Corporation, patrocinado pelo US-CERT. Cada entrada tem ID padrão, descrição e referências.
- **Dark web**: conteúdo criptografado não indexado por buscadores convencionais; pesquisadores monitoram em busca de novas ameaças.
- **IOC (Indicador de Comprometimento)**: evidências de violação de segurança (assinaturas de malware, domínios maliciosos).
- **AIS (Automated Indicator Sharing)**: recurso da CISA para troca de indicadores em tempo real usando STIX (linguagem estruturada) e TAXII (protocolo de troca).

## Engenharia social

Estratégia não técnica que manipula pessoas para realizar ações ou divulgar informações, explorando disposição a ajudar, ganância ou vaidade.

**Tipos de ataque:**
- **Pretexting**: mentir para obter dados privilegiados (ex: fingir confirmar identidade)
- **Quid pro quo**: solicitar dados pessoais em troca de algo (ex: prêmio, viagem grátis)
- **Fraude de identidade**: usar identidade roubada para obter bens/serviços

**Táticas psicológicas exploradas:**
- **Autoridade**: pessoas obedecem figuras de autoridade (ex: executivo abre PDF de "intimação" infectado)
- **Intimidação**: pressionar a vítima a agir sob ameaça (ex: culpar secretária por arquivo corrompido)
- **Consenso/prova social**: agir como os outros ao redor (ex: contas falsas validando uma "oportunidade de negócio")
- **Escassez**: criar senso de quantidade limitada
- **Urgência**: criar senso de tempo limitado
- **Familiaridade**: construir relacionamento ou clonar perfil de um amigo
- **Confiança**: estabelecer confiança ao longo do tempo (ex: falso "especialista em segurança")

**Shoulder surfing**: observar por cima do ombro para capturar PINs, senhas, dados de cartão — pode usar binóculos/câmeras à distância.

**Dumpster diving (mergulho no lixo)**: vasculhar lixo em busca de informações descartadas. Defesa: fragmentar/incinerar documentos sensíveis.

**Outras técnicas de disfarce:**
- **Representação (impersonation)**: fingir ser outra pessoa (ex: falso funcionário do IRS cobrando dívida)
- **Farsas (hoaxes)**: mensagens enganosas (ex: alerta de vírus falso pedindo repasse)
- **Piggybacking/tailgating**: seguir pessoa autorizada para entrar em área restrita (escoltado, misturado em multidão, ou explorando desatenção). Defesa: mantrap (duas portas em sequência)
- **Fraude de fatura**: fatura falsa com linguagem urgente pedindo login em tela falsa
- **Watering hole (ataque do regador)**: infectar sites que o alvo costuma visitar
- **Typosquatting**: registrar domínios com erros de digitação comuns
- **Adendo**: remover tag de "externo" do email para simular origem interna
- **Campanhas de influência**: combinam notícias falsas, desinformação e mídia social, comuns em guerra cibernética

**Defesas contra engenharia social:**
- Nunca divulgar credenciais/dados sensíveis a desconhecidos
- Resistir a clicar em links/emails atrativos
- Desconfiar de downloads automáticos
- Educar funcionários sobre políticas de segurança
- Incentivar responsabilidade compartilhada
- Não ceder a pressão de desconhecidos

## Malware

- **Vírus**: se replica e se anexa a arquivos/programas legítimos. Requer interação do usuário para ativar; pode ser programado para data/hora específica. Propaga-se por mídia removível, downloads e anexos de email. Ex: vírus Melissa (1999), US$ 1,2 bilhão em danos.
- **Worms**: se replicam sozinhos explorando vulnerabilidades, sem precisar de programa host nem interação do usuário após infecção inicial. Espalham-se rápido e podem sobrecarregar redes. Ex: Code Red infectou 300 mil servidores em 19 horas (2001).
- **Trojans (cavalos de troia)**: mascaram intenção maliciosa como programa legítimo. Não se replicam sozinhos; geralmente anexados a arquivos não executáveis (imagem, áudio, vídeo). Exploram privilégios do usuário que os executa.
- **Bombas lógicas**: ficam inativas até um gatilho (data, entrada de banco de dados). Podem sabotar registros, apagar arquivos, ou até sobrecarregar componentes de hardware (fans, CPU, memória) até superaquecer/falhar.
- **Ransomware**: criptografa dados e exige pagamento (geralmente rastreável) para liberação — muitas vítimas não recuperam acesso mesmo pagando. Comumente distribuído via phishing.

## Ataques de negação de serviço (DoS)

Ataques relativamente simples de executar, mesmo por invasores não qualificados, mas causam grande perda de tempo/dinheiro. Podem afetar até tecnologia operacional (hardware/software de fábricas e serviços públicos).

- **Tráfego esmagador**: volume de dados que a rede/host não consegue processar
- **Pacotes malformados**: pacotes com erros que o receptor não consegue tratar, causando lentidão ou falha

## Ataques ao DNS e domínio

- **Reputação de domínio**: DNS converte nomes de domínio em endereços IP; empresas devem monitorar sua reputação/IP
- **DNS spoofing (envenenamento de cache)**: insere dados falsos no cache DNS, redirecionando tráfego para o computador do invasor
- **Sequestro de domínio**: invasor obtém controle das informações DNS de um alvo (geralmente via engenharia social sobre o email do administrador, encontrado no registro público WHOIS)
- **Redirecionamento de URL malicioso**: explora funcionalidades legítimas de redirecionamento para levar a sites maliciosos

## Ataques de camada 2 (link de dados)

A camada 2 do modelo OSI move dados pela rede física, mapeando IPs para endereços MAC via ARP (Address Resolution Protocol).

- **Spoofing de MAC**: disfarça dispositivo como válido para ignorar autenticação
- **ARP spoofing**: vincula o MAC do invasor ao IP de um dispositivo autorizado
- **IP spoofing**: envia pacotes com endereço de origem falsificado
- **Inundação de MAC**: satura a tabela do switch com MACs falsos, comprometendo a segurança da comutação

## Ataques Man-in-the-Middle

- **MitM**: invasor assume controle de um dispositivo sem o usuário saber, podendo interceptar, manipular e retransmitir informações
- **MitMo (Man-in-the-Mobile)**: variação focada em dispositivos móveis; captura dados sensíveis e envia ao invasor. Ex: malware ZeuS captura SMS de verificação em duas etapas

## Ataques de dia zero

Exploram vulnerabilidades antes de serem conhecidas ou corrigidas pelo fornecedor. A rede fica extremamente vulnerável entre a "hora zero" (descoberta do exploit) e o lançamento do patch.

## Keylogging

Captura cada tecla digitada, via software instalado ou hardware conectado ao computador. Pode revelar usuários, senhas e sites visitados. Softwares anti-spyware podem detectar e remover keyloggers.

## Defesas gerais contra ataques de rede

- Configurar firewalls para descartar pacotes externos com endereço de origem interno (anti-spoofing)
- Manter patches e atualizações em dia
- Distribuir carga de trabalho entre servidores
- Bloquear pacotes ICMP externos para mitigar DoS/DDoS

## Ataques a dispositivos móveis e sem fio

- **Grayware**: apps indesejados que não são malware reconhecido, mas rastreiam localização ou exibem publicidade — geralmente "autorizado" em letras miúdas do termo de licença
- **SMiShing**: phishing via SMS, levando a site malicioso ou número fraudulento
- **Access point não autorizado**: instalado sem autorização, pode ser configurado como MitM (captura login desconectando o AP legítimo e forçando reautenticação)
- **Ataque de gêmeo do mal (evil twin)**: AP falso parecendo melhor opção de conexão, usado para analisar tráfego e fazer MitM
- **Interferência de radiofrequência**: bloqueio deliberado de sinal via jammer com frequência/potência equivalente ao sinal original
- **Bluejacking**: envio de mensagens/imagens não autorizadas via Bluetooth
- **Bluesnarfing**: cópia de dados (emails, contatos) via conexão Bluetooth sem autorização
- **WEP**: protocolo antigo, sem bom gerenciamento de chaves, vetor de inicialização pequeno e estático — facilmente comprometido
- **WPA/WPA2**: substituem o WEP com criptografia mais forte; ainda vulneráveis a sniffing de pacotes entre AP e usuário

**Defesas Wi-Fi e móvel:**
- Alterar configurações padrão, usar autenticação/criptografia
- Posicionar APs fora do firewall ou em DMZ
- Usar ferramentas como NetStumbler para detectar APs não autorizados
- Política de acesso para convidados
- VPN de acesso remoto para funcionários

## Ataques a aplicações

- **XSS (Cross-Site Scripting)**: script malicioso injetado em página web, executado no navegador da vítima, capturando cookies/tokens de sessão
- **Injeção SQL**: insere instrução SQL maliciosa em campo de entrada não filtrado corretamente, permitindo acesso não autorizado, falsificação de identidade, ou controle do servidor
- **Injeção XML**: interfere no processamento/consulta de dados XML, corrompendo o banco de dados
- **Injeção DLL**: engana o aplicativo para carregar uma DLL maliciosa como parte do processo
- **Injeção LDAP**: explora falhas de validação para extrair dados do diretório LDAP
- **Buffer overflow**: grava dados além dos limites de um buffer, podendo acessar memória de outros processos, causar falha ou permitir escalação de privilégios
- **Execução remota de código**: executa comandos com privilégios do usuário no dispositivo alvo. Ferramentas como Metasploit Framework e o payload Meterpreter (executado em memória, sem tocar o disco, evitando detecção de antivírus) são usadas em testes de penetração — e por atacantes
- **CSRF (Cross-Site Request Forgery)**: comandos não autorizados enviados do navegador de um usuário para um app confiável, sem seu conhecimento
- **Ataque de condição de corrida (race condition)**: força execução simultânea de operações destinadas a ocorrer em sequência
- **Tratamento inadequado de entrada**: dados não validados corretamente causam buffer overflow ou injeção SQL
- **Tratamento de erro malicioso**: mensagens de erro revelam nomes de hosts, diretórios, tabelas de banco — usados para montar outros ataques
- **Ataque a API**: abuso de endpoint de API
- **Ataque de repetição (replay)**: intercepta e reenvia dados válidos maliciosamente
- **Directory traversal**: lê arquivos fora do diretório do site, podendo expor configurações sensíveis
- **Exaustão de recursos**: sobrecarrega hardware do servidor (em vez da largura de banda como no DoS)

**Defesas contra ataques de aplicação:**
- Escrever código sólido (primeira linha de defesa)
- Tratar toda entrada externa como hostil e validá-la
- Manter todo software atualizado, sem ignorar prompts

## Spam

Email não solicitado, geralmente publicitário, mas frequentemente enviado por computadores infectados com vírus/worms — pode conter links maliciosos ou conteúdo enganoso.

**Indicadores de spam:**
- Sem assunto
- Pede atualização de dados da conta
- Erros de ortografia/pontuação estranha
- Links longos ou incompreensíveis
- Parece de empresa legítima mas com pequenas diferenças
- Pede abertura urgente de anexo

## Phishing e variantes

- **Phishing**: contato por email/mensagem disfarçado de fonte legítima, induzindo a instalar malware ou compartilhar dados
- **Spear phishing**: altamente direcionado, personalizado com informações conhecidas sobre a vítima (interesses, projetos)
- **Vishing**: phishing por voz, pode usar VoIP ou mensagens gravadas
- **Pharming**: redireciona para versão falsa de site oficial
- **Whaling**: alvo são executivos de alto escalão

**Defesas contra email/navegador:**
- ISPs e antivírus filtram spam automaticamente
- Educar funcionários sobre riscos de anexos
- Nunca presumir que anexo é seguro, mesmo de contato confiável
- Participar de grupos como o APWG (Anti-Phishing Working Group)
- Manter software com correções de segurança atualizadas

## Outros vetores de ataque

- **Ataques físicos**: malware em pen drive, cabos/adaptadores modificados com chips sem fio, clonagem de cartão de crédito/débito
- **Ataques adversos a IA/machine learning**: dados corrompidos usados para enganar modelos (ex: veículo autônomo interpretando mal placas)
- **Ataques à cadeia de fornecimento**: exploram terceiros, componentes estrangeiros ou datas de fim de vida (EOL) de contratos de suporte
- **Ataques na nuvem**: exploram dados/aplicações/infraestrutura em SaaS, PaaS e IaaS
