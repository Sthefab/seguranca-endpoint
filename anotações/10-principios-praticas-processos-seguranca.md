# Princípios, Práticas e Processos de Segurança Cibernética

## O Cubo da Segurança Cibernética

Ferramenta pra pensar em proteção de dados através de 3 dimensões:

### 1. Princípios de segurança (CIA)

- **Confidencialidade**: impede divulgação de info a pessoas/recursos/processos não autorizados
- **Integridade**: precisão, consistência e confiabilidade dos dados
- **Disponibilidade**: garante que a informação esteja acessível a usuários autorizados quando necessário

### 2. Estados dos dados

- Dados **em trânsito**
- Dados **inativos** (em repouso/armazenamento)
- Dados **em processo**

A segurança precisa proteger os três estados, não só um.

### 3. Proteções (as bases da defesa)

- **Tecnologia**
- **Políticas e práticas**
- **Pessoas** (educação, treinamento e conscientização)

## Confidencialidade – técnicas

- **Tokenização**: substitui dados originais por um valor aleatório sem relação matemática com o dado real; preserva formato/tamanho, útil pra banco de dados e pagamento com cartão; o token não tem valor fora do sistema (alternativa à criptografia)
- **DRM (Digital Rights Management)**: protege material com direitos autorais (música, filme, livro); conteúdo é criptografado e só pode ser copiado/usado com chave de descriptografia licenciada
- **IRM (Information Rights Management)**: usado em e-mails e arquivos corporativos; permite que o dono do documento controle e gerencie o acesso a ele

## Tipos de informação sensível

- **Informações pessoais**: qualquer coisa rastreável a uma pessoa (registros médicos, cartão de crédito, número de Previdência Social)
- **Informações comerciais**: representam risco pra empresa se vazarem (planos de aquisição, dados de clientes, segredos comerciais)
- **Informações classificadas**: pertencem a órgão governamental; níveis de sigilo (secreto, confidencial, restrito)

## Integridade dos Dados

Dados devem permanecer inalterados por entidades não autorizadas durante captura, armazenamento, recuperação, atualização e transferência.

Métodos pra garantir integridade: **hashing**, verificações de validação de dados, verificações de consistência e controles de acesso.

Níveis de necessidade de integridade (varia por tipo de organização):

| Nível | Exemplo |
|---|---|
| Crítico | Saúde — dados de prescrição validados/testados/verificados continuamente |
| Alto | E-commerce/análises — transações e contas verificadas com frequência |
| Médio | Buscadores/vendas online — pouca verificação, dados não totalmente confiáveis |
| Baixo | Blogs/redes sociais — sem verificação, baixa confiabilidade |

## Garantindo a Disponibilidade

Medidas que aumentam disponibilidade de sistemas/serviços:
- Manutenção regular de equipamentos
- Atualizações e patches de SO e software
- Teste de backup (não basta fazer backup, tem que testar se funciona)
- Planejamento contra desastres (funcionários e clientes sabendo como reagir)
- Implementação e avaliação contínua de novas tecnologias
- Monitoramento contínuo de logs, alertas e acessos
- Testes de disponibilidade: varredura de porta, varredura de vulnerabilidade, teste de penetração

## Dados em Repouso (Data at Rest)

Dados armazenados, sem nenhum processo/usuário acessando ou alterando no momento.

Opções de armazenamento:

| Tipo | Descrição |
|---|---|
| DAS (Direct Attached Storage) | Conectado direto ao computador (HD, pendrive); não compartilha com a rede por padrão |
| RAID | Vários discos combinados formando um único disco lógico; melhora desempenho e tolerância a falhas |
| NAS (Network Attached Storage) | Dispositivo de armazenamento em rede, centralizado, flexível e escalável |
| SAN (Storage Area Network) | Armazenamento em rede de alta velocidade, conecta vários servidores a um repositório centralizado |
| Armazenamento em nuvem | Remoto, via provedor (Google Drive, iCloud, Dropbox), acessível via internet |

Desafios: armazenamento local é vulnerável a ataques no próprio host e difícil de gerenciar; sistemas em rede (RAID/SAN/NAS) são mais seguros e redundantes, mas mais complexos de configurar, testar e monitorar.

## Dados em Trânsito

Dados sendo transmitidos entre dispositivos (não estão nem em repouso, nem em processamento).

Métodos de transmissão:
- **Rede de tênis** (sneakernet): transporte físico via mídia removível
- **Redes com fio**: cobre e fibra óptica, LAN ou WAN
- **Redes sem fio**: ondas de rádio, crescendo e aumentando a superfície de ataque

Protocolos padrão como **IP** e **HTTP** definem a estrutura dos pacotes.

Desafios de proteção:
- **Confidencialidade**: risco de interceptação/roubo → soluções: VPN, SSL, IPsec e outros métodos de criptografia
- **Integridade**: risco de modificação em trânsito → soluções: hash e redundância de dados
- **Disponibilidade**: risco de dispositivos falsos/não autorizados interceptando ou derrubando a conexão (ex: access point falso) → solução: sistemas de autenticação mútua (usuário autentica o servidor e vice-versa)

## Dados em Processo

Dados durante entrada, modificação, computação ou saída.

- **Entrada**: coleta via digitação manual, digitalização, upload, sensores. Corrupção pode vir de rotulagem errada, formato incompatível, erro de digitação ou sensor com defeito/desconectado
- **Modificação**: alteração dos dados (intencional, como edição/codificação/criptografia, ou não intencional/maliciosa = **corrupção de dados**, causada por falha de equipamento ou código malicioso)
- **Saída**: envio pra dispositivos de saída (impressora, monitor, alto-falante). Corrupção pode vir de delimitador errado, configuração de comunicação incorreta ou impressora mal configurada

## Contramedidas de Segurança Cibernética

### Baseadas em software (instaladas em hosts/servidores individuais)

| Tecnologia | Função |
|---|---|
| Firewall de software | Controla acesso remoto ao sistema |
| Scanner de rede e porta | Descobre e monitora portas abertas |
| Analisador de protocolo | Coleta/examina tráfego, identifica problemas de desempenho, configs erradas e comportamento anômalo |
| Scanner de vulnerabilidade | Avalia pontos fracos em computadores/redes |
| IDS baseado em host | Examina atividade só no sistema host; gera log/alarme em atividade incomum |

### Baseadas em hardware

| Tecnologia | Função |
|---|---|
| Firewall | Bloqueia tráfego indesejado com base em regras |
| Servidor proxy | Mascara o endereço IP real do cliente ao solicitar serviços |
| Controle de acesso por hardware | Biometria (impressão digital, íris) pra confirmar identidade |
| Switch de rede | Ponto de conexão da rede, contribui pra eficiência de segurança |

## Cultura de Conscientização de Segurança

Tecnologia sozinha não resolve — pessoas são o elo mais fraco se não forem bem treinadas.

Formas de treinar:
- Incluir treinamento de segurança na integração de novos funcionários
- Vincular conscientização de segurança a avaliação de desempenho
- Treinamentos presenciais com gamificação (ex: capture the flag)
- Cursos/módulos online

Um programa de conscientização depende do ambiente da empresa, do nível de ameaça e da natureza dos dados que ela mantém.

## Políticas de Segurança

Definem objetivos, regras de comportamento e requisitos do sistema. Uma política abrangente:
- Demonstra comprometimento da empresa com segurança
- Define regras de comportamento esperado
- Garante consistência nas operações e no uso de hardware/software
- Define consequências jurídicas de violações
- Dá suporte da gerência à equipe de segurança

Tipos de política:

| Política | O que cobre |
|---|---|
| Identificação e autenticação | Quem pode acessar recursos e como se verifica isso |
| Senha | Requisitos mínimos e troca regular |
| Uso aceitável (AUP) | O que pode/não pode ser feito nos sistemas; deve ser bem explícita |
| Acesso remoto | Como usuários remotos acessam a rede e o que é acessível |
| Manutenção de rede | SOs de dispositivos de rede e procedimentos de atualização |
| Tratamento de incidentes | Como incidentes de segurança são tratados |

## Padrões

Mantêm consistência no funcionamento da rede. São obrigatórios dentro da organização.

Exemplo: política de senha padrão pode exigir mínimo de 8 caracteres alfanuméricos (maiúsculas/minúsculas) + 1 caractere especial, troca a cada 30 dias, e histórico das últimas 12 senhas pra impedir reutilização.

## Diretrizes

Sugestões flexíveis (não obrigatórias) de como fazer as coisas com mais eficiência e segurança. Ajudam a desenvolver os padrões e a seguir as políticas gerais.

Fontes de diretrizes: melhores práticas da própria empresa, NIST (Instituto Nacional de Padrões e Tecnologia), NSA (orientações de configuração de segurança) e o padrão Critérios Comuns.

Exemplo prático: transformar uma frase memorável ("Eu tenho um sonho") numa senha forte substituindo letras por símbolos (ex: `Ihv@dr3@m`), variando número/símbolo/pontuação pra criar outras senhas a partir da mesma base.
