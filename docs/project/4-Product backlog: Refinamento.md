
## Modelagem de ameaças passo a passo ## SENDO CORRIGIDO
Antes de começar a modelagem de ameaças, devo possuir um esboço de uma arquitetura base de como será a aplicação ou pelo menos possuir uma proposta de solução técnica. Não precisa ser perfeita, é apenas uma arquitetura candidata.

![Descrição da imagem](https://raw.githubusercontent.com/pinheiro-felipe/jornada-appsec-criando-api-de-postagem-de-status-do-pedido-para-delivery/b5b43eefc18acf3b21a0c18146aeffb671da547e/docs/project/images/Modelagem-de-amea%C3%A7as-arquitetura-base.png)

<br>

### 1º- Temos que entender o que estamos estamos construindo ##

Como ponto de partida, precisamos definir o escopo do nosso modelo de ameaças. 
A definição de escopo que seguirei, é a mencionada por Izar Tarandach autor do livro "Threat modeling A Practical 
Guide for Development Teams" que defende a "modelagem de ameaças para cada história de usuário", pois uma modelagem geral 
do consome tempo, recursos e logo fica obsoleta.

**1.1- História de usuário que terá suas ameaças modeladas**

    Peguei uma história de usuário do projeto deste repositório para demonstrar a construção do meu processo de modelagem de ameaças
    
    História de usuário em formato narrativo:
      
        E1-F1-H1: Como delivery de alimentação, quero adicionar uma nova postagem de mídia referente ao status atual do pedido, 
        para que o cliente acompanhe o andamento do seu pedido desde o preparo até a entrega.

        E1: Indica o épico ao qual a funcionalidade a seguir está vinculada.
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

**1.4- Desenho do diagrama de raciocínio das ameaças**

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
          
      Para formar nosso racicínio das ameaças, vamos identificar:

          [ ] Entidade que interage com o sistema (usuário ou um serviço externo).
          [ ] Processo onde a ação é executada.
          [ ] Local onde o dado é persistido.
          [ ] Limite de Confiança.
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
                          
      O diagrama de racicínio foi desenhado usando a ferramenta "Threat Dragon v2.5.0-latest".

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
      O arquivo original com o desenho será armazenado em breve para que você possa fazer o donwload e visualizar no seu "Threat Dragon v2.5.0-latest".

![Descrição da imagem](https://raw.githubusercontent.com/pinheiro-felipe/jornada-appsec-criando-api-de-postagem-de-status-do-pedido-para-delivery/b9c62dde0f9f38ac5a8dfb36bbcabb1744d99766/docs/project/images/Modelagem-de-amea%C3%A7as-(mini-fluxo)-E1-F1-H1.png)

<br>
<br>

### 2º- Descobrir as ameaças e o que pode dar errado ##
Esta é uma atividade de pesquisa na qual você busca identificar as principais ameaças da aplicação.
Você pode fazer brainstorming ou utilizar alguma estrutura para orientar o raciocínio, como STRIDE, Kill Chain, CAPEC, Cornucopia (OWASP) e outras.

O objetivo da categorização de ameaças é ajudar a identificar ameaças do invasor (STRIDE). O diagrama produzido anteriormente ajuda a identificar 
os possíveis alvos de ameaças da perspectiva do invasor, como fontes de dados, processos, fluxos de dados e interações com usuários.
  
**2.1- Compenente**

**2.2- STRIDE: Baseado em categoria de ataque**

    Quais categorias de ataques podem afetar cada um dos componentes do diagrama de raciocínio de ameaças (Process, Data Store e Data Flow)?
        Pode ocorrer Spoofing?
        Pode ocorrer Tampering?
        Pode ocorrer Repudiation?
        Pode ocorrer Information Disclosure?
        Pode ocorrer Denial of Service?
        Pode ocorrer Elevation of Privilege?
    Para cada alerta existem mitigações.
    
**2.3- Cheklist If-This / Then-That motor de descoberta de ameaças baseado em lógica e comportamento**

    Checklist If-This (representa a condição que precisa ser satisfeita) / Then-That (representa a ação que será executada se a condição for satisfeita).
    Busca descobrir o que acontece no sistema quando uma condição específica acontece.
    Ele identifica falhas lógicas, casos extremos, cenários condicionais, que o STRIDE às vezes não pega sozinho
    
    Principalmente:
        Falhas de fluxo
        Falhas de regra de negócios
        Falhas de sequência
        Falhas por condição inesperada
        Falhas de estado
        Falhas de validação contextual
        Falhas que só existem quando eventos se encadeiam

    Exemplos de condicionais e ações:
        
        SE minha história envolve entrada de usuário,
            ENTÃO considerar validação, sanitização, limites.
        
        SE minha história envia dados para outro serviço,
            ENTÃO considerar autenticação, autorização, integridade.
        
        SE minha história manipula dados sensíveis,
            ENTÃO aplicar medidas de proteção de confidencialidade.

        SE o usuário clicar para alterar o endereço sem estar autenticado,
            ENTÃO redirecionar para login.
        
        SE alguém tentar alterar o endereço para um local já utilizado por outro cliente,
            ENTÃO retornar erro.
        
        SE o sistema receber o update com campo vazio,
            ENTÃO ignorar alteração.
        
        SE o request vier sem CSRF token,
            ENTÃO bloquear tentativa suspeita.


**2.4- Tabela com os dados das ameaças para o endpoint**

    O que este endpoint "POST /posts" possui em termos de:
        Ameaça
        Sensibilidade
        Exposição
        Atacante (perfil / motivação)
        Como o atacante atacaria (Técnicas e Métodos)
        Controle (mitigações)
        
         
    Após o desenho do diagrama de racicínio criamos uma tabela com os dados que encontramos. Fique a vontade para fazer se quiser ou não. EM DESENVOLVIMENTO

| Endpoint                     | Dados de entrada / sensíveis (S)                                                           | Dados de saída / sensíveis (S)                                             | Exposição                                                                                                                                                  | IF-THIS / THEN-THAT                                                                                            | STRIDE           | Atacante                           | Técnicas e Métodos                                              | Controles                                                                                  |
| ---------------------------- | ------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- | ---------------- | ---------------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **POST /posts**              | `idPedido (S)`, `statusAtual`, `descricaoStatus`, `timestamp`, `localizacaoEntregador (S)` | `idPost`, `statusSalvo`, `timestampSalvo`                                  | Endpoint autenticado acessível a todos → **exposição moderada**.<br>Exposição a requisições repetidas (brute force).<br>Aceita POST → aumenta superfície.  | **If** `idPedido` inválido **then** rejeitar<br>**If** payload inesperado **then** bloquear                    | S, T, R, I, D, E | Cliente malicioso / externo        | JSON tampering, brute force de IDs, DoS via payload grande      | Autenticação forte, validação, schema enforcement, rate limit, logs imutáveis, RBAC, HTTPS |



  	
      
