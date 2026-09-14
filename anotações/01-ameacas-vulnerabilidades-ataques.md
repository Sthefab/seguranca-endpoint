# Ameaças, vulnerabilidades e ataques a segurança cibernética

Notas de estudo completas sobre domínios de ameaça, tipos de ataque, malware, engenharia social e técnicas de defesa em segurança cibernética.

## Domínios de ameaça

Um domínio de ameaça é uma área de controle, autoridade ou proteção que invasores podem explorar para obter acesso a um sistema. Formas comuns de exploração incluem:

1. Acesso direto e físico a sistemas e redes
2. Redes sem fio que ultrapassam os limites físicos da empresa
3. Bluetooth ou NFC (comunicação de campo próximo)
4. Anexos de email maliciosos
5. Elos fracos na cadeia de fornecimento
6. Contas de mídia social da empresa
7. Mídia removível (pen drives, HDs externos)
8. Aplicativos baseados em nuvem

## Categorias de ameaças

Classificar ameaças ajuda a empresa a priorizar esforços de segurança com base em probabilidade e impacto financeiro.

| Categoria | Exemplos |
|---|---|
| Ataques de software | DoS bem sucedido, vírus de computador |
| Erros de software | Bug de software, aplicativo fora do ar, XSS, compartilhamento ilegal de arquivos |
| Sabotagem | Comprometimento de banco de dados por usuário autorizado, desfiguração de site |
| Erro humano | Erro de entrada de dados, firewall mal configurado |
| Roubo | Laptops e equipamentos roubados de salas destrancadas |
| Falhas de hardware | Disco rígido trava |
| Interrupção de utilidade | Queda de energia, dano por água (falha de sprinkler) |
| Desastres naturais | Furacões, terremotos, inundações, incêndios |

## Ameaças internas vs externas

**Internas**: funcionários atuais ou antigos, ou parceiros de contrato, que acidental ou intencionalmente manipulam dados sensíveis ou comprometem infraestrutura (por exemplo, conectando mídia infectada ou acessando sites maliciosos).

**Externas**: invasores amadores ou qualificados que exploram vulnerabilidades de rede ou usam engenharia social para obter acesso.

## Domínio do usuário

O usuário é considerado o elo mais fraco da segurança da informação. Principais riscos:

1. **Falta de conscientização de segurança**: usuário não entende políticas, dados sensíveis ou contramedidas em vigor
2. **Políticas aplicadas incorretamente**: falta de clareza sobre as consequências da não conformidade
3. **Roubo de dados**: gera dano reputacional e responsabilidade legal
4. **Downloads e mídias não autorizados**: infecções rastreadas a downloads de músicas, vídeos e apps não autorizados
5. **VPNs não autorizadas**: a criptografia pode esconder roubo de dados de administradores de rede
6. **Sites não autorizados**: podem solicitar download de scripts ou plugins maliciosos, e até assumir controle de câmeras e outros dispositivos
7. **Destruição de sistemas ou dados**: sabotagem por ativistas, funcionários insatisfeitos ou concorrentes

Vale lembrar: não existe solução técnica que torne um sistema mais seguro do que o comportamento das pessoas que o usam.

## Ameaças por domínio de rede

**Dispositivos**: dispositivos ligados e sem supervisão, downloads de fontes não confiáveis, exploração de vulnerabilidades de software não corrigido, uso de mídia removível não autorizada, hardware e software desatualizados.

**LAN (rede local)**: acesso não autorizado a data centers e armários de fiação, vulnerabilidades de SO de rede, usuários desonestos em redes sem fio, hardware e SO inconsistentes entre servidores, sondagem e varredura de portas não autorizada, firewalls mal configurados.

**Nuvem privada**: sondagem e varredura de portas, acesso não autorizado a recursos, vulnerabilidades de firewall ou roteador, erros de configuração, usuários remotos baixando dados sensíveis.

**Nuvem pública**: recursos de computação compartilhados entre organizações via provedor de internet. Existem três modelos de serviço:

1. **SaaS** (Software como Serviço): software hospedado e acessado via navegador ou app, não armazenado localmente
2. **PaaS** (Plataforma como Serviço): plataforma para desenvolver, rodar e gerenciar aplicações no hardware do provedor
3. **IaaS** (Infraestrutura como Serviço): recursos de computação virtual (hardware, servidores, armazenamento) fornecidos via internet

**Aplicações**: acesso não autorizado a data centers e sistemas, tempo de inatividade em manutenção, vulnerabilidades de SO de rede, perda de dados, falhas em desenvolvimento de apps cliente servidor ou web.

## Complexidade da ameaça

**APT (Advanced Persistent Threat)**: ataque contínuo e elaborado, envolvendo múltiplos agentes e/ou malware sofisticado, que opera sob o radar por longos períodos. Geralmente visa governos e grandes organizações, sendo bem financiado e orquestrado.

**Ataques de algoritmo**: exploram algoritmos de software legítimo para gerar comportamento indesejado. Por exemplo, forçar uso excessivo de RAM ou CPU para travar um sistema, ou disparar alertas falsos.

## Backdoors e rootkits

**Backdoors**: programas (como Netbus e Back Orifice) que dão acesso não autorizado ignorando a autenticação normal. Frequentemente instalados via ferramentas de administração remota (RAT). Permitem acesso contínuo mesmo depois que a vulnerabilidade original é corrigida.

**Rootkits**: modificam o sistema operacional para criar um backdoor. Exploram vulnerabilidades para escalar privilégios e podem modificar ferramentas forenses e de monitoramento, tornando se muito difíceis de detectar. Na maioria dos casos exigem reinstalação completa do sistema.

## Inteligência de ameaças e fontes de pesquisa

**CVE (Common Vulnerabilities and Exposures)**: dicionário de vulnerabilidades mantido pela MITRE Corporation, patrocinado pelo US CERT. Cada entrada tem ID padrão, descrição e referências.

**Dark web**: conteúdo criptografado não indexado por buscadores convencionais. Pesquisadores especializados monitoram essa camada da internet em busca de novas ameaças.

**IOC (Indicador de Comprometimento)**: evidências de violação de segurança, como assinaturas de malware ou domínios maliciosos.

**AIS (Automated Indicator Sharing)**: recurso da CISA para troca de indicadores em tempo real, usando STIX (linguagem estruturada para descrever a ameaça) e TAXII (protocolo de troca dessas informações).

## Engenharia social

Estratégia não técnica que manipula pessoas para realizar ações ou divulgar informações, explorando a disposição em ajudar, a ganância ou a vaidade.

**Tipos de ataque:**

1. **Pretexting**: mentir para obter dados privilegiados, como fingir precisar confirmar a identidade de alguém
2. **Quid pro quo**: solicitar dados pessoais em troca de algo, como um prêmio ou viagem grátis
3. **Fraude de identidade**: usar identidade roubada para obter bens ou serviços por engano

**Táticas psicológicas exploradas:**

1. **Autoridade**: pessoas obedecem mais quando instruídas por uma figura de autoridade (exemplo: executivo abre um PDF de "intimação" infectado)
2. **Intimidação**: pressionar a vítima a agir sob ameaça (exemplo: culpar a secretária por um arquivo corrompido)
3. **Consenso, ou prova social**: agir como os outros ao redor (exemplo: contas falsas validando uma "oportunidade de negócio")
4. **Escassez**: criar senso de quantidade limitada disponível
5. **Urgência**: criar senso de tempo limitado para agir
6. **Familiaridade**: construir relacionamento com a vítima, ou clonar o perfil de um amigo dela
7. **Confiança**: estabelecida ao longo do tempo (exemplo: falso "especialista em segurança" que "descobre" um problema grave)

**Shoulder surfing**: observar por cima do ombro do alvo para capturar PINs, senhas ou dados de cartão. Pode ser feito à distância, com binóculos ou câmeras de segurança.

**Dumpster diving (mergulho no lixo)**: vasculhar o lixo do alvo em busca de informações descartadas. A defesa é fragmentar ou incinerar documentos sensíveis.

**Outras técnicas de disfarce:**

1. **Representação (impersonation)**: fingir ser outra pessoa, como um falso funcionário de órgão público cobrando dívida sob ameaça de prisão
2. **Farsas (hoaxes)**: mensagens enganosas, como alertas falsos de vírus pedindo para o usuário repassar a mensagem, causando pânico desnecessário
3. **Piggybacking e tailgating**: seguir uma pessoa autorizada para entrar fisicamente em área restrita, fingindo estar acompanhado ou se misturando numa multidão. Defesa comum: mantrap, duas portas em sequência, onde a primeira precisa fechar antes da segunda abrir
4. **Fraude de fatura**: fatura falsa com linguagem urgente pedindo login em tela falsa
5. **Watering hole (ataque do regador)**: infectar sites que o alvo costuma visitar
6. **Typosquatting**: registrar domínios com erros de digitação comuns para capturar tráfego
7. **Adendo**: remover a tag de "externo" do email para simular origem interna da empresa
8. **Campanhas de influência**: combinam notícias falsas, desinformação e publicações em mídia social, comuns em guerra cibernética

**Defesas contra engenharia social:**

1. Nunca divulgar credenciais ou dados sensíveis a desconhecidos
2. Resistir a clicar em links e emails atrativos
3. Desconfiar de downloads automáticos ou não iniciados
4. Educar funcionários sobre as políticas de segurança
5. Incentivar responsabilidade compartilhada pelos problemas de segurança
6. Não ceder a pressão de pessoas desconhecidas

## Malware

**Vírus**: se replica e se anexa a arquivos ou programas legítimos, inserindo seu próprio código. Requer interação do usuário para ativar e pode ser programado para agir em data ou hora específica. Propaga se por mídia removível, downloads e anexos de email. Exemplo: o vírus Melissa (1999) causou danos estimados em US$ 1,2 bilhão.

**Worms**: se replicam sozinhos, explorando vulnerabilidades de rede, sem precisar de um programa host nem de interação do usuário após a infecção inicial. Espalham se muito rápido e podem sobrecarregar redes inteiras. Exemplo: o worm Code Red infectou mais de 300 mil servidores em 19 horas, em 2001.

**Trojans (cavalos de troia)**: mascaram sua intenção maliciosa se passando por algo legítimo. Não se replicam sozinhos, geralmente vêm anexados a arquivos não executáveis, como imagem, áudio ou vídeo. Exploram os privilégios do usuário que os executa.

**Bombas lógicas**: ficam inativas até um gatilho específico, como uma data ou entrada de banco de dados. Podem sabotar registros, apagar arquivos ou até sobrecarregar componentes de hardware, como ventoinhas, CPU e memória, até superaquecerem ou falharem.

**Ransomware**: criptografa os dados da vítima e exige pagamento (geralmente por meio não rastreável) para liberar o acesso. Muitas vítimas não recuperam os dados mesmo depois de pagar. É comumente distribuído por phishing.

**Grayware**: aplicativos indesejados que não são malware reconhecido, mas ainda representam risco, como rastrear localização ou exibir publicidade indesejada. Costuma estar "autorizado" nas letras miúdas do termo de licença do software.

**Keylogging**: captura e registra cada tecla digitada, via software instalado no sistema ou hardware fisicamente conectado ao computador. O arquivo de log pode revelar usuários, senhas, sites visitados e outras informações sensíveis. Softwares anti spyware conseguem detectar e remover keyloggers não autorizados.

## Ataques de negação de serviço

Ataques relativamente simples de executar, mesmo por invasores não qualificados, mas que causam grande perda de tempo e dinheiro. Podem afetar até tecnologia operacional, como hardware e software de fábricas e provedores de serviços públicos.

**DoS (Denial of Service)**: um único invasor ou origem sobrecarrega o alvo. Duas formas principais:

1. **Tráfego esmagador**: volume de dados que a rede, host ou aplicação não consegue processar, causando lentidão ou falha
2. **Pacotes malformados**: pacotes com erros que o dispositivo receptor não consegue interpretar corretamente, travando ou desacelerando o serviço

**DDoS (Distributed Denial of Service)**: versão distribuída do DoS, em que múltiplos dispositivos comprometidos atacam o mesmo alvo ao mesmo tempo. Esses dispositivos costumam formar uma botnet, uma rede de máquinas infectadas controladas remotamente pelo invasor sem o conhecimento dos donos. Por vir de várias origens diferentes ao mesmo tempo, o DDoS é bem mais difícil de bloquear que um DoS comum, já que não dá pra simplesmente banir um único IP.

## Ataques ao DNS e domínio

**Reputação de domínio**: o DNS converte nomes de domínio, como www.exemplo.com, em endereços IP. Empresas precisam monitorar sua reputação e IP contra associação com domínios maliciosos externos.

**DNS spoofing (envenenamento de cache)**: insere dados falsos no cache de um resolvedor DNS, redirecionando o tráfego de um domínio legítimo para o computador do invasor.

**Sequestro de domínio**: invasor obtém controle das informações DNS de um alvo, geralmente via engenharia social sobre o email do administrador, que pode ser encontrado no registro público WHOIS.

**Redirecionamento malicioso de URL**: explora funcionalidades legítimas de redirecionamento (como voltar a uma tela de login) para levar a vítima a um site malicioso em vez do destino esperado.

## Ataques de camada 2

A camada 2 do modelo OSI é responsável por mover dados pela rede física, mapeando endereços IP para endereços MAC através do protocolo ARP.

1. **Spoofing de MAC**: disfarça o dispositivo do invasor como um MAC autorizado, ignorando o processo de autenticação
2. **ARP spoofing**: envia mensagens ARP falsas vinculando o MAC do invasor ao IP de um dispositivo autorizado na rede
3. **IP spoofing**: envia pacotes IP com endereço de origem falsificado
4. **Inundação de MAC**: satura o switch de rede com endereços MAC falsos, comprometendo a segurança da comutação de pacotes

## Sniffing

Captura passiva do tráfego de rede usando um sniffer de pacotes, ferramenta que escuta os dados que trafegam entre dois pontos, como um access point e um usuário legítimo. Mesmo quando a criptografia (como WPA2) impede a leitura direta do conteúdo, o invasor ainda pode analisar padrões de tráfego. Costuma ser o primeiro passo antes de um ataque man in the middle.

## Ataques man in the middle

**MitM (Man in the Middle)**: o invasor assume controle de uma comunicação sem o conhecimento das partes envolvidas, podendo interceptar, manipular e retransmitir informações falsas entre remetente e destinatário.

**MitMo (Man in the Mobile)**: variação do MitM focada em dispositivos móveis. Quando o celular é infectado, é instruído a capturar informações sensíveis do usuário e enviá las ao invasor. Exemplo: o malware ZeuS, que permite capturar silenciosamente SMS de verificação em duas etapas.

## Ataques de dia zero

Exploram vulnerabilidades de software antes que se tornem conhecidas ou sejam corrigidas pelo fornecedor. A rede fica extremamente vulnerável entre a hora zero (quando o exploit é descoberto) e o tempo que leva para o fornecedor lançar um patch. Defender se exige uma abordagem de segurança mais sofisticada e holística, já que ainda não existe assinatura ou patch conhecido.

## Defesas gerais contra ataques de rede

1. Configurar firewalls para descartar pacotes externos com endereço de origem interno (anti spoofing)
2. Manter patches e atualizações em dia
3. Distribuir a carga de trabalho entre servidores
4. Bloquear pacotes ICMP externos para mitigar DoS e DDoS

## Ataques a dispositivos móveis e sem fio

**SMiShing**: phishing via SMS, induzindo a acessar um site malicioso ou ligar para um número fraudulento.

**Access point não autorizado**: instalado sem autorização explícita na rede. Pode ser configurado como um dispositivo MitM, capturando informações de login ao desconectar o access point legítimo e forçar uma reautenticação.

**Ataque de gêmeo do mal (evil twin)**: access point falso configurado para parecer a melhor opção de conexão disponível. Depois que a vítima se conecta, o invasor pode analisar o tráfego e executar ataques MitM.

**Interferência de radiofrequência**: bloqueio deliberado de sinal sem fio usando um jammer com frequência, modulação e potência equivalentes ao sinal original.

**Bluejacking**: envio de mensagens ou imagens não autorizadas via Bluetooth para outro dispositivo.

**Bluesnarfing**: cópia de dados, como emails e listas de contatos, de um dispositivo alvo via conexão Bluetooth, sem o conhecimento da vítima.

**WEP**: protocolo de segurança sem fio antigo, com gerenciamento de chaves fraco e vetor de inicialização pequeno, legível e estático, o que o torna facilmente comprometido.

**WPA e WPA2**: substituem o WEP com criptografia mais forte. Ainda são vulneráveis a sniffing de pacotes entre o access point e o usuário, mesmo que a chave não possa ser recuperada só observando o tráfego.

**Defesas Wi-Fi e móvel:**

1. Alterar configurações padrão e usar autenticação e criptografia
2. Posicionar access points fora do firewall ou em uma DMZ (zona desmilitarizada)
3. Usar ferramentas como NetStumbler para detectar access points não autorizados
4. Criar política de acesso para convidados
5. Exigir VPN de acesso remoto para funcionários que usam a WLAN

## Ataques a aplicações

**XSS (Cross Site Scripting)**: script malicioso injetado em uma página web, executado no navegador da vítima, capturando cookies e tokens de sessão para se passar pelo usuário.

**Injeção SQL**: insere uma instrução SQL maliciosa em um campo de entrada não filtrado corretamente, permitindo acesso não autorizado, falsificação de identidade, modificação ou destruição de dados, e até controle total do servidor de banco de dados.

**Injeção XML**: interfere no processamento ou consulta de dados XML, corrompendo o banco de dados e ameaçando a segurança do site.

**Injeção DLL**: engana o aplicativo para carregar um arquivo DLL malicioso, que é executado como parte do processo alvo.

**Injeção LDAP**: explora falhas de validação de entrada para injetar consultas em servidores LDAP, extraindo informações sensíveis do diretório da empresa.

**Buffer overflow**: grava dados além dos limites de um buffer, permitindo acesso à memória alocada para outros processos. Pode causar falha do sistema, comprometer dados ou fornecer escalação de privilégios.

**Execução remota de código**: permite executar comandos com os privilégios do usuário que roda o aplicativo alvo. Ferramentas como o Metasploit Framework (usado para testes de penetração) incluem o payload Meterpreter, que roda inteiramente em memória, sem tocar o disco, o que dificulta a detecção por antivírus.

**CSRF (Cross Site Request Forgery)**: comandos não autorizados são enviados do navegador de um usuário para um aplicativo web confiável, sem o conhecimento do usuário, via imagens, formulários ocultos ou requisições JavaScript maliciosas.

**Ataque de condição de corrida (race condition)**: força um sistema, projetado para executar tarefas em sequência, a rodar duas ou mais operações ao mesmo tempo, explorando o intervalo entre verificação e uso de um recurso.

**Tratamento inadequado de entrada**: dados de usuário não validados corretamente podem causar buffer overflow ou abrir brechas para injeção SQL.

**Tratamento malicioso de erro**: invasores usam mensagens de erro para extrair nomes de host, diretórios, arquivos, nomes de bancos de dados e tabelas, que podem ser usados para montar outros ataques.

**Ataque a API**: ocorre quando um criminoso abusa de um endpoint de API para acessar ou manipular dados indevidamente.

**Ataque de repetição (replay)**: intercepta uma transmissão de dados válida e a reenvia de forma maliciosa, fazendo o destinatário repetir uma ação.

**Directory traversal**: permite ler arquivos no servidor web fora do diretório do site, podendo expor arquivos de configuração sensíveis ou levar ao controle total do servidor.

**Exaustão de recursos**: sobrecarrega os recursos de hardware disponíveis no servidor alvo, em vez da largura de banda de rede como no DoS tradicional.

**Defesas contra ataques de aplicação:**

1. Escrever código sólido é a primeira linha de defesa
2. Tratar e validar toda entrada externa como se fosse hostil
3. Manter todo software atualizado, sem ignorar prompts de atualização

## Spam

Email não solicitado, geralmente com fim publicitário, mas frequentemente enviado em massa por computadores já infectados com vírus ou worms. Pode conter links maliciosos, malware ou conteúdo enganoso.

**Indicadores comuns de spam:**

1. Email sem assunto
2. Pedido para atualizar dados da conta
3. Erros de ortografia ou pontuação estranha
4. Links longos ou incompreensíveis
5. Aparência de empresa legítima com pequenas diferenças suspeitas
6. Pedido urgente para abrir um anexo

## Phishing e variantes

**Phishing**: contato por email, mensagem instantânea ou outro meio, disfarçado de fonte legítima, com o objetivo de induzir a vítima a instalar malware ou compartilhar dados como credenciais de login e informações financeiras.

**Spear phishing**: ataque altamente direcionado, com email personalizado baseado em informações reais sobre a vítima, como interesses, preferências ou projetos de trabalho.

**Vishing**: phishing por voz, usando VoIP ou mensagens gravadas para parecer uma chamada legítima e induzir a vítima a divulgar dados, como número de cartão de crédito.

**Pharming**: redireciona a vítima para uma versão falsa de um site oficial, levando a inserir credenciais no site fraudulento acreditando ser o legítimo.

**Whaling**: ataque de phishing direcionado a vítimas de alto perfil dentro de uma empresa, como executivos seniores.

**Defesas contra email e navegador:**

1. ISPs e softwares antivírus costumam filtrar spam automaticamente
2. Educar funcionários sobre os perigos de anexos não solicitados
3. Nunca presumir que um anexo é seguro, mesmo vindo de um contato confiável
4. Participar de iniciativas como o APWG (Anti Phishing Working Group)
5. Manter todo software com as correções de segurança mais recentes aplicadas

## Outros vetores de ataque

**Ataques físicos**: carregar malware em pen drives que infectam o dispositivo ao conectar, usar cabos ou adaptadores modificados com chips sem fio para controlar remotamente um dispositivo, ou clonar cartões de crédito e débito com terminais especializados.

**Ataques adversos a inteligência artificial**: aproveitam que modelos de machine learning dependem dos dados de entrada. Dados corrompidos podem enganar o modelo e distorcer o resultado previsto, como fazer um veículo autônomo interpretar mal placas de rua.

**Ataques à cadeia de fornecimento**: exploram terceiros que fornecem sistemas, componentes ou software, incluindo peças de fontes estrangeiras. Podem envolver alteração de datas de fim de vida útil (EOL) de contratos de suporte, tirando a empresa da elegibilidade para manutenção.

**Ataques na nuvem**: exploram dados sensíveis, aplicações, plataformas ou infraestrutura hospedados em provedores de nuvem, cobrindo os modelos SaaS, PaaS e IaaS.
