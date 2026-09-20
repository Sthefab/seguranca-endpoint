# Protegendo Redes

Resumo do módulo sobre a situação atual da segurança de redes: por que redes são alvos, quem ataca, como ataca e como as organizações se defendem.

---

## 1. Situação atual: redes são alvos

- Redes sofrem ataques o tempo todo, e notícias de organizações comprometidas são rotina.
- Exemplos de ferramentas para acompanhar ameaças:
  - **Kaspersky Cyberthreat Real-Time Map:** mapa interativo com ataques em tempo real, alimentado por produtos de segurança da Kaspersky no mundo todo.
  - **Cisco Talos Intelligence Group:** inteligência de ameaças e segurança para defender clientes e ativos.
  - **Cisco PSIRT (Product Security Incident Response Team):** time que investiga e mitiga vulnerabilidades em produtos Cisco, publicando avisos de segurança para os administradores.

> **Traduzindo:** Talos é quem estuda as ameaças, PSIRT é quem cuida das falhas nos produtos da própria Cisco.

---

## 2. Por que a segurança de rede importa

- Segurança de rede está diretamente ligada à **continuidade dos negócios**.
- Uma violação pode causar:
  - Interrupção do e-commerce
  - Perda de dados comerciais
  - Ameaça à privacidade das pessoas
  - Comprometimento da integridade das informações
  - Perda de receita, roubo de propriedade intelectual, ações judiciais e até risco à segurança pública
- Exige **vigilância constante** dos profissionais, que precisam acompanhar novas ameaças, ataques e vulnerabilidades de dispositivos e aplicativos.

---

## 3. Vetores de ataque

**Vetor de ataque:** caminho que um atacante usa para obter acesso a um servidor, equipamento ou rede. Pode vir de **fora** (ex: pela internet) ou de **dentro** da rede corporativa.

> **Traduzindo:** é a "porta de entrada" que o atacante usa.

**Exemplo externo:** ataque de **DoS (Denial of Service)**, quando um dispositivo ou aplicativo fica incapaz de atender usuários legítimos.

**Ameaças internas:** um funcionário pode, de forma acidental ou intencional:
- Roubar e copiar dados confidenciais (mídia removível, e-mail, mensagens)
- Comprometer servidores internos ou dispositivos de infraestrutura
- Derrubar uma conexão crítica e causar interrupção na rede
- Conectar um USB infectado em um computador corporativo

> **Por que interno pode ser pior?** O usuário interno já tem acesso físico ao prédio e aos dispositivos, e conhece a rede, os recursos e os dados sensíveis.

---

## 4. Perda de dados (exfiltração de dados)

**Exfiltração de dados:** quando dados são perdidos, roubados ou vazados para fora da organização, de forma intencional ou não.

Dados são provavelmente o **ativo mais valioso** da organização (P&D, vendas, financeiro, RH, jurídico, funcionários, contratados, clientes).

**Consequências:**
- Dano à marca e perda de reputação
- Perda de vantagem competitiva
- Perda de clientes e de receita
- Ações judiciais, multas e penalidades civis
- Custo e esforço para notificar afetados e se recuperar

**Defesa:** controles de **DLP (Data Loss Prevention)**, combinando medidas estratégicas, operacionais e táticas.

### Vetores comuns de perda de dados

| Vetor | Como acontece |
|---|---|
| E-mail / redes sociais | Vetor mais comum. Mensagens interceptadas podem revelar informações confidenciais. |
| Dispositivos não criptografados | Laptop roubado sem criptografia entrega os dados de bandeja. |
| Armazenamento em nuvem | Configurações fracas de segurança podem expor dados. |
| Mídia removível | Transferência não autorizada para USB ou perda do pendrive. |
| Cópia impressa | Documento descartado sem ser triturado pode ser recuperado. |
| Controle de acesso inadequado | Senhas fracas ou comprometidas dão acesso fácil aos dados. |

---

## 5. Ameaça, vulnerabilidade e risco

**Ativos:** tudo que tem valor para a organização (dados, propriedade intelectual, servidores, computadores, smartphones, tablets etc.).

| Termo | Definição | Traduzindo |
|---|---|---|
| **Ameaça** | Perigo potencial para um ativo (dados ou a própria rede). | O que pode dar errado. |
| **Vulnerabilidade** | Fraqueza em um sistema ou no design dele que pode ser explorada por uma ameaça. | O ponto fraco. |
| **Superfície de ataque** | Soma de todas as vulnerabilidades de um sistema que estão acessíveis a um invasor. | Todos os pontos por onde dá pra entrar. Ex: SO e navegador desatualizados expostos na rede. |
| **Exploit** | Mecanismo usado para aproveitar uma vulnerabilidade e comprometer um ativo. | A "chave" que abre a falha. |
| **Risco** | Probabilidade de uma ameaça explorar uma vulnerabilidade e gerar uma consequência indesejada. | Chance de dar ruim x tamanho do estrago. |
| **Contramedida** | Ação tomada para proteger ativos, atenuando uma ameaça ou reduzindo o risco. | A defesa. |
| **Impacto** | Dano potencial causado à organização pela ameaça. | Quanto vai doer. |

### Tipos de exploit

- **Remoto:** funciona pela rede, **sem acesso prévio** ao alvo. O atacante não precisa de conta no sistema.
- **Local:** o atacante já tem algum acesso (usuário ou administrativo) ao sistema. Não significa necessariamente acesso físico.

---

## 6. Gestão de risco

**Gestão de risco:** processo que equilibra o **custo** de aplicar medidas de proteção com o **ganho** de proteger o ativo.

| Estratégia | O que é | Observação |
|---|---|---|
| **Aceitação** | O custo de tratar o risco é maior que o custo do risco em si. Nada é feito. | Risco aceito conscientemente. |
| **Prevenção (evitar)** | Eliminar a atividade ou dispositivo que gera o risco. | Perde também todos os benefícios da atividade. |
| **Redução (mitigação)** | Tomar medidas para diminuir a exposição ou o impacto. | **Estratégia mais usada.** Exige avaliar custo da perda, custo da mitigação e benefício da atividade. |
| **Transferência** | Passar parte ou todo o risco para um terceiro. | Ex: seguradora. |

---

## 7. Hacker x ator de ameaça

"Hacker" tem vários significados: programador habilidoso, profissional de rede que garante que a rede não seja vulnerável, quem tenta acesso não autorizado, ou quem derruba acesso e corrompe dados.

Por isso o curso usa **ator de ameaça** para se referir a hackers **gray hat** e **black hat**.

| Tipo | Perfil |
|---|---|
| **White Hat** | Hackers éticos. Fazem testes de penetração para achar vulnerabilidades e reportam aos desenvolvedores e times de segurança para corrigir antes da exploração. Algumas empresas pagam recompensas (bug bounty). |
| **Gray Hat** | Fazem coisas possivelmente antiéticas ou ilegais, mas sem fins de lucro pessoal ou de causar dano. Ex: invadem sem permissão e depois divulgam a falha (às vezes avisando a empresa depois). |
| **Black Hat** | Criminosos que violam sistemas para ganho pessoal ou por malícia, explorando vulnerabilidades para comprometer sistemas. |

> **Traduzindo:** a diferença está na **permissão** e na **intenção**. White hat tem autorização e quer proteger. Black hat não tem e quer lucrar ou causar dano. Gray hat fica no meio.

---

## 8. Evolução dos atores de ameaça

- **Anos 1960:** *phreaking* telefônico. Usavam frequências de áudio (apitos que imitavam tons) para enganar as centrais telefônicas e fazer ligações longa distância de graça.
- **Meados dos anos 1980:** conexões discadas e modems. Surgiram os programas de **"war dialing"**, que discavam todos os números de uma região procurando computadores, BBS e fax. Ao achar um, usavam programas de quebra de senha para entrar.
- Desde então, os perfis e motivações dos atores mudaram bastante.

### Tipos de atores de ameaça

| Ator | Descrição |
|---|---|
| **Script kiddies** | Surgiram nos anos 1990. Inexperientes que rodam scripts, ferramentas e exploits prontos para causar dano, geralmente sem lucro. |
| **Corretores de vulnerabilidade** (*vulnerability brokers*) | Gray hats que descobrem exploits e reportam aos fornecedores, às vezes por recompensa. |
| **Hacktivistas** | Gray hats que protestam contra ideias políticas e sociais. Publicam conteúdo, vazam informações e fazem ataques DDoS. |
| **Cibercriminosos** | Black hats, autônomos ou ligados ao crime organizado. Roubam bilhões de dólares por ano de consumidores e empresas. |
| **Patrocinados pelo Estado** | Roubam segredos de governo, coletam informações e sabotam redes de governos estrangeiros, terroristas e corporações. Dependendo da perspectiva, podem ser white ou black hat. |

### Cibercriminosos em detalhe

- Motivação principal: **ganhar dinheiro** por qualquer meio.
- Costumam ser financiados por organizações criminosas.
- Atuam no "submundo", onde compram, vendem e trocam exploits, ferramentas, dados pessoais e propriedade intelectual roubados.
- Atacam desde pequenas empresas e consumidores até grandes corporações e setores inteiros.

---

## 9. Tarefas de segurança cibernética

- Atores de ameaça **não discriminam**: atacam usuários domésticos, pequenas e médias empresas e grandes organizações públicas e privadas.
- Segurança cibernética é **responsabilidade compartilhada**. Todo usuário deve:
  - Denunciar crimes cibernéticos às autoridades
  - Ficar atento a ameaças em e-mail e na web
  - Proteger informações importantes contra roubo
- As organizações devem desenvolver e praticar tarefas de segurança para proteger ativos, usuários e clientes.

---

## 10. Indicadores de ameaças: IOC e IOA

### IOC (Indicator of Compromise)

**Indicador de comprometimento:** evidência de que um ataque **já ocorreu**. Cada ataque tem atributos identificáveis únicos.

Exemplos de IOC:
- Hashes de arquivos de malware
- Endereços IP de servidores usados no ataque
- Nomes de arquivos e domínios
- Alterações características feitas no sistema final

> **Exemplo de e-mail de "prêmio":** o usuário não estava em sorteio nenhum, o IP do remetente, o assunto do e-mail, a URL do link e o anexo são todos IOCs.

Exemplo de resumo de IOC para um malware:

```
Malware File - "studiox-link-standalone-v20.03.8-stable.exe"
sha256 6a6c28f5666b12beecd56a3d1d517e409b5d6866c03f9be44ddd9efffa90f1e0
sha1   eb019ad1c73ee69195c3fc84ebf44e95c147bef8
md5    3a104b73bb96dfed288097e9dc0a11a8
DNS requests: log.studiox.link, my.studiox.link, sip.studiox.link
Connections (IPs): 198.51.100.248, 203.0.113.82
```

> **Traduzindo:** o hash é a "impressão digital" do arquivo, e os domínios e IPs mostram com quem o malware tenta se comunicar.

### IOA (Indicator of Attack)

**Indicador de ataque:** foca na **motivação e na estratégia** do atacante, ou seja, em *como* ele pretende comprometer vulnerabilidades para chegar aos ativos.

- Ajuda a criar uma abordagem **proativa** de segurança.
- Estratégias são reutilizadas em vários ataques, então defender contra uma estratégia pode barrar ataques futuros parecidos.

| | IOC | IOA |
|---|---|---|
| Foco | Evidência do que **já aconteceu** | Estratégia e intenção **em andamento** |
| Postura | Reativa (investigação e resposta) | Proativa (prevenção) |
| Exemplo | Hash do malware, IP do servidor malicioso | Padrão de movimentação lateral, tentativa de escalar privilégios |

---

## 11. Compartilhamento de ameaças e conscientização

- **CISA** (Cybersecurity and Infrastructure Security Agency, EUA): lidera esforços para automatizar o compartilhamento de informações de segurança com organizações públicas e privadas, sem custo.
- **AIS (Automated Indicator Sharing):** sistema da CISA que compartilha indicadores de ataque entre governo dos EUA e setor privado assim que as ameaças são verificadas.
- **NCASM (National Cybersecurity Awareness Month):** campanha anual de outubro da CISA com a NCSA. O tema de 2019 foi *"Own IT. Secure IT. Protect IT."* Temas abordados:
  - Segurança em redes sociais
  - Configurações de privacidade
  - Segurança de apps de dispositivos
  - Manter software atualizado
  - Compras online seguras
  - Segurança de Wi-Fi
  - Proteção de dados de clientes
- **ENISA:** agência da União Europeia para cibersegurança. Faz na Europa um papel parecido com o da CISA nos EUA.

---

## 12. Extras (complementos importantes)

### Tríade CIA

Base de tudo em segurança da informação:
- **Confidencialidade:** só quem deve acessar, acessa (criptografia, controle de acesso).
- **Integridade:** o dado não foi alterado de forma indevida (hashes, assinaturas).
- **Disponibilidade:** o sistema e os dados estão acessíveis quando precisam (redundância, proteção contra DoS).

> **Conectando:** vazamento de dados quebra a confidencialidade. DoS quebra a disponibilidade.

### DoS x DDoS

- **DoS:** um único atacante ou fonte sobrecarrega o alvo.
- **DDoS (Distributed DoS):** o ataque vem de **muitas fontes ao mesmo tempo** (geralmente botnets), o que dificulta bloquear.

### Defesa em profundidade

Usar **várias camadas** de proteção (firewall, IDS/IPS, antivírus, criptografia, controle de acesso, treinamento de usuários) para que, se uma falhar, as outras segurem.

### Engenharia social e phishing

Muitos ataques exploram o **fator humano**, não a tecnologia. Phishing é o exemplo clássico: mensagem falsa para induzir a pessoa a clicar, baixar ou entregar credenciais. Por isso a conscientização dos usuários é parte da defesa.

---

## Resumão pra revisão rápida

- Redes são alvos o tempo todo, e segurança é ligada à continuidade do negócio.
- Ataques podem vir de **fora** ou de **dentro**, e os internos podem ser piores.
- **Dados** são o ativo mais valioso, e a perda deles (exfiltração) tem custo alto. DLP é a defesa.
- **Ameaça** + **vulnerabilidade** + **exploit** = **risco**. Contramedida reduz o risco.
- Gestão de risco: **aceitar, evitar, reduzir (mais comum), transferir**.
- Atores: **white, gray e black hat**, script kiddies, brokers, hacktivistas, cibercriminosos e patrocinados pelo Estado.
- **IOC** = evidência de ataque que já rolou. **IOA** = estratégia do atacante.
- Compartilhar inteligência de ameaças (CISA, AIS, ENISA) ajuda a defender todo mundo.
