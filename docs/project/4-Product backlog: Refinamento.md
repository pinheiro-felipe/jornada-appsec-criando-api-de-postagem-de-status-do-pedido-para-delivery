
## Modelagem de ameaças passo a passo ## SENDO CORRIGIDO
Antes de começar a modelagem de ameaças, devo possuir um esboço de uma arquitetura base de como será a aplicação ou pelo menos possuir uma proposta de solução técnica. Não precisa ser perfeita, é apenas uma arquitetura candidata.

![Descrição da imagem](https://raw.githubusercontent.com/pinheiro-felipe/jornada-appsec-criando-api-de-postagem-de-status-do-pedido-para-delivery/b5b43eefc18acf3b21a0c18146aeffb671da547e/docs/project/images/Modelagem-de-amea%C3%A7as-arquitetura-base.png)

<br>

### 1º- Temos que entender o que estamos estamos construindo ##

Como ponto de partida, precisamos definir o escopo do nosso modelo de ameaças. 
A definição de escopo que seguirei, é a mencionada por Izar Tarandach autor do livro "Threat modeling A Practical 
Guide for Development Teams" que defende a "modelagem de ameaças para cada história de usuário", pois uma modelagem geral 
do sistema consome tempo, recursos e logo fica obsoleta.

**1.1- História de usuário que terá suas ameaças modeladas**

    Peguei uma história de usuário do projeto deste repositório para demonstrar a construção do meu processo de modelagem de ameaças
    
    História de usuário em formato narrativo:
      
        E1-F1-H1: Como delivery de alimentação, quero adicionar uma nova postagem de mídia referente ao status atual do pedido, 
        para que o cliente acompanhe o andamento do seu pedido desde o preparo até a entrega.

        E1: Indica o épico ao qual a funcionalidade está vinculada.
        F1: Indica a funcionalidade ao qual a história de usuário está vinculada.
        H1: Indica a história de usuário.

    Mesma história de usuário acima em formato estruturado:

        Ator: Como delivery de alimentação

        Ação: quero adicionar

        Dado: uma nova postagem de mídia referente ao status atual do pedido

        Ator que recebe o valor (ator implícito ou explícito): para que o cliente
    
        Valor recebido: acompanhe o andamento do seu pedido desde o preparo até a entrega.

**1.2- Ator**

      Quem exerce a ação?
      Quem deseja fazer algo?

         Ator: Delivery de alimentação
         
**1.3- Ação ou evento invocado**

      Qual evento é invocado?

         Evento + Dado: Adicionar um nova postagem de mídia referente ao status atual do pedido

**1.4- Desenho do diagrama de raciocínio das ameaças para entender nosso cenário**

      O diagrama de raciocínio das ameaças, é um diagrama que se cria enquanto se rascunha a modelagem de ameaças para a história de usuário.
     
      Para fazer o desenho do diagrama de raciocínio, é preciso saber como é a arquitetura da aplicação ou como ela será.
      
      O engenheiro/arquiteto de segurança da informação Izar Tarandach defende uma abordagem de modelagem de ameaças simples o suficiente para que:
      
          O dev consiga aplicar sozinho enquanto lê a história de usuário e pensa na implementação.
          O processo seja rápido.
          Se encaixe no seu fluxo natural de programação.
          Não trave o sprint.

      Dicas gerais:
          Não se preocupe com artefatos visuais complexos.
          O diagrama de raciocínio das ameaças não é feito para ser eterno, nem pretende cobrir o sistema como um todo, apenas o necessário para entender 
          as ameaças no contexto da história de usuário.
          Ele é criado para ser descartável, mas pode ser armazenado como uma forma de documentar o raciocínio do dev na modelagem de ameaças, o que pode ajudar muitos outros. 
          Ele de maneira nenhuma tem a pretenção de representar a arquitetura oficial da aplicação, apenas o pensanmento do dev naquele momento.
          Pode ser revisitado sempre que preciso, mas sempre deve ser comparando com a arquitetura oficial.
          
      Para formar nosso raciocínio das ameaças, vamos identificar:

          [ ] Entidade que interage com o sistema (usuário ou um serviço externo).
          [ ] Processo onde a ação é executada.
          [ ] Local onde o dado é persistido.
          [ ] Limite de confiança.
          [ ] Fluxo dos dados.
          [ ] Endpoints.
          [ ] Dados críticos e sensíveis trafegados.
          [ ] Dados críticos e sensíveis armazenados.
          [ ] Pontos que exigem criptografia.
          [ ] Pontos que exigem autenticação e autorização.
          [ ] Protocolo de comunicação.
          [ ] Requisitos regulatórios ou de compliance.        
          [ ] Ativos (Algo que é valioso ou crítico para o negócio e que pode ser impactado por uma ameaça em nossa história de usuário)
          
                  Alguns tipos de ativos:
              
                      Dados sensíveis
                          Dados de pedido
                          Dados do cliente
                          Token JWT
                          ID do pedido
                          Status do pedido
                          Mensagem enviada para o WhatsApp
                          Dados persistidos no banco

                      Serviços críticos
                          API do status do pedido
                          API do WhatsApp
                          Serviço de envio de mensagens
                          Fila
                          Banco de dados
                          Seu worker/processador de mensagens
                          Servidor web

                      Infraestrutura
                          Host da API
                          Porta 3336 do banco
                          Container que executa o worker
                          
      O nosso diagrama de racicínio foi desenhado usando a ferramenta "Threat Dragon v2.5.0-latest".

      Componentes da ferramenta Threat Dragon:
                          
          Entidade externa (External entity): Qualquer coisa externa que não está sob nosso controle e interage com o sistema.
              Pode ser um ator, como um usuário final ou um serviço externo que consome a aplicação ou é consumido por ela, 
              como meios de pagamento, CDN e etc.
          
          Processo (Process): Algo que faz alguma ação, executa lógica ou processamento e está sob nosso controle
          
          Armazenamento de dado (Data storage): Qualquer lugar onde o dado é persistido. (Banco de dados, serviço interno e etc.)
          
          Limite de Confiança (Trust Boundary): Uma linha ou caixa que mostra onde muda o nível de confiança.
              Desenhe um trust boundary quando:
                  Mudar a rede (externa → interna)
                  Mudar quem controla (cliente → servidor)
                  Mudar permissões
                  
          Fluxo de dados (Data flow): A seta que mostra como os dados circulam.

      Em nosso diagrama de raciocínio abaixo, incluí apenas elementos relevantes para a história de usuário.
      As informações que não estão visíveis no diagrama como "dados críticos e sensíveis trafegados" estão dentro de cada componente.
      Ao clicar neles é possível ver suas informações. 
      O arquivo original com o desenho será armazenado em breve para que você possa fazer o download e visualizar no seu "Threat Dragon v2.5.0-latest".

![Descrição da imagem](https://raw.githubusercontent.com/pinheiro-felipe/jornada-appsec-criando-api-de-postagem-de-status-do-pedido-para-delivery/0ceb616f6ac050cabead67f9c2e1ebb8493342e9/docs/project/images/Modelagem-de-amea%C3%A7as-(diagrama-raciocinio)-E1-F1-H1.png)

<br>
<br>

### 2º- Descobrir as ameaças e o que pode dar errado ##
Esta é uma atividade de pesquisa na qual você busca identificar as principais ameaças da história de usuário.
Você pode fazer brainstorming ou utilizar alguma estrutura para orientar o raciocínio das ameaças, como STRIDE, Kill Chain, CAPEC, Cornucopia (OWASP) e outras.

O diagrama produzido anteriormente ajuda a identificar os possíveis alvos de ameaças da perspectiva do invasor, como fontes de dados, processos, fluxos de dados e interações com usuários.
  
**2.1- STRIDE**

    O STRIDE é um modelo de categorização de ameaças que ajuda a identificar e classificar possíveis ações de um atacante.
    No STRIDE identificamos o que o atacante quer fazer num alto nível e que pilar da segurança pode ser atingido (Confidencialidade, Integridade, Disponibilidade, etc.).
    STRIDE revela a intenção do atacante e te ajuda a descobrir onde procurar vulnerabilidades posteriormente.
    Como alguém poderia abusar desse fluxo? O que ele quer? E porque?
    
    Para cada componente do diagrama de raciocínio (Process, Data Store e Data Flow)), pergunte: Qual objetivo STRIDE (Spoofing, Tampering, etc.) um atacante gostaria de alcançar aqui?
       S – Spoofing (Falsificação de identidade) → O atacante pode se passar por outro usuário?
       T – Tampering (Alteração indevida) → O atacante pode alterar dados ou parâmetros?
       R – Repudiation (Negação de ações) → O usuário pode negar o que fez?
       I – Information Disclosure (Exposição de dados) → Dados podem ser expostos?
       D – Denial of Service (Negação de serviço) → O atacante pode derrubar o serviço?
       E – Elevation of Privilege (Elevação de privilégio) → O atacante pode ganhar acesso superior ao que já possui?
    Para cada ameaça identificada, existem mitigações específicas.

**2.2- Cornucopia (OWASP)** 
   
    Cornucopia (OWASP) é um baralho de cartas criado para ajudar equipes a identificar requisitos de segurança e potenciais vulnerabilidades em seus softwares de forma colaborativa.
 
    Forma de uso especificada no owasp cornucopia:
        Embaralhe as cartas, distribua entre os participantes e, em cada rodada, cada pessoa apresenta uma carta e justifica o porque aquela carta se encaixa na história de usuário. Em         seguida, o grupo diz se corncorda ou não e acontecem os confrontos de ideias. A carta que for aceita é registrada. Em cada uma ja possui dicas de mitigação.

     Forma de uso que pensei:
        Reúna a equipe em um brainstorming e deixe o baralho acessível para todos. O grupo discute como um abusador poderia explorar o sistema a partir da história de usuário visando           seu objetivo definido no STRIDE. 
        Cada pessoa seleciona as cartas que melhor representam o cenário, focando nas categorias mais relevantes para a funcionalidade analisada. 
        Todos podem propor cartas e justificar por que ela se aplica; os demais podem concordar ou discordar de forma objetiva. Assim, a equipe inclui ou descarta 
        a carta na hora, usando o brainstorming como um filtro para determinar o que realmente deve entrar.
     
     Depois de identificar o objetivo STRIDE, você deve se perguntar: Como o agente de ameaça pode tecnicamente explorar vulnerabilidades do sistema para atingir seu objetivo? 
     Quais vulnerabilidades podem ser exploraradas na história de usuário para o agente de ameaça alcançar o que deseja? 
           
     O abusador é o agente de ameaça que pode ser:

         Atacante Não-Autenticado (Anônimo): O que ele pode fazer sem fazer login? (Geralmente, focado em Denial of Service ou ataques de força bruta).

         Atacante Autenticado (Usuário Comum): O que um usuário normal pode fazer a outro usuário, ou a um recurso que não é dele? (Foco em Broken Access Control, levando a Information          Disclosure ou Elevation of Privilege).

         Atacante Interno (Funcionário/Admin): O que um usuário de alto privilégio pode fazer indevidamente (muitas vezes focando em Repudiation - fazer algo e negar que fez).
    
**2.3- Cheklist SE "Vulnerabilidade" / ENTÃO "Ameaça" / E "Objetivo" ajuda a definir nossas hipóteses**

    Checklist SE (representa a condição de vulnerabilidade que precisa ser satisfeita) / ENTÃO (representa a ação que poderá ser executada para explorar a vulnerabilidade se a condição for satisfeita) / E (representa o que o agente de ameaça deseja alcançar).
    Esse checklist busca descobrir o que acontece no sistema quando uma condição específica relacionada a alguma vulnerabilidade é satisfeita.
    Ele identifica falhas lógicas, casos extremos, cenários condicionais, que o STRIDE às vezes não pega sozinho
    
    Principalmente casos de:
        Falhas de fluxo
        Falhas de regra de negócios
        Falhas de sequência
        Falhas por condição inesperada
        Falhas de estado
        Falhas de validação contextual
        Falhas que só existem quando eventos se encadeiam

    Exemplo de condição relacionada a vulnerabilidade, ameaça e objetivo:

        SE "faltar validação nos campos de input"
           ENTÃO "o agente de ameaça pode injetar código sql malicioso"
              E "obter dados sensíveis do clientes"      
        
**2.4- Tabela com a organização dos dados das possíveis ameaças para o nosso endpoint**

    O que este endpoint "POST /posts" possui em termos de:
        Sensibilidade
        Exposição
        Vulnerabilidade | Ameaça | Objetivo
        STRIDE
        Atacante (perfil / motivação)
        Como o atacante atacaria (Técnicas e Métodos)
        Controle (mitigações)
        
         
    Após o desenho do diagrama de racicínio criamos uma tabela com os dados que encontramos. Fique a vontade para fazer essa tabela ou não. EM DESENVOLVIMENTO

| Endpoint                     | Dados de entrada / sensíveis (S)                                                           | Dados de saída / sensíveis (S)                                             | Exposição                                                                                                                                                  | Vulnerabilidade | Ameaça | Objetivo                                                                                            | STRIDE           | Atacante                           | Técnicas e Métodos                                              | Controles                                                                                  |
| ---------------------------- | ------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- | ---------------- | ---------------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **POST /posts**              | `idPedido (S)`, `statusAtual`, `descricaoStatus`, `timestamp`, `localizacaoEntregador (S)` | `idPost`, `statusSalvo`, `timestampSalvo`                                  | Endpoint autenticado acessível a todos → **exposição moderada**.<br>Exposição a requisições repetidas (brute force).<br>Aceita POST → aumenta superfície.  | **If** `idPedido` inválido **then** rejeitar<br>**If** payload inesperado **then** bloquear                    | S, T, R, I, D, E | Cliente malicioso / externo        | JSON tampering, brute force de IDs, DoS via payload grande      | Autenticação forte, validação, schema enforcement, rate limit, logs imutáveis, RBAC, HTTPS |



  	
      
