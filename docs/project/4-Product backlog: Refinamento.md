
## Modelagem de ameaças passo a passo ##
Antes de começar a modelagem de ameaças, devo possuir um esboço de uma arquitetura base de como será a aplicação ou pelo menos possuir uma proposta de solução técnica. Não precisa ser perfeita, é apenas uma arquitetura candidata.
<br>
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
        H1: Representa a história de usuário.

    Mesma história de usuário em formato estruturado:

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

**1.4- Desenho do mini-fluxo essencial da história de usuário**

      O engenheiro/arquiteto de segurança da informação Izar Tarandach defende uma abordagem simples o suficiente para que:
      
          O dev consiga fazer sozinho enquanto lê a história e pensa na sua implementação
          Seja rápido
          Faça parte do fluxo natural da programação
          Não trave o sprint

      Dicas gerais:
      
          Faça um mini-fluxo essencial pequeno.
          Não se preocupe com artefatos visuais complexos.
          Ele não é feito para ser eterno, nem pretende cobrir o sistema como um todo.
          
      
      Em nosso mini-fluxo essencial incluí apenas elementos relevantes para a história de usuário:
      
          Entidade externa (External entity): Qualquer coisa externa que não está sob nosso controle e interage com o sistema.
              Pode ser um ator, como um usuário final ou um serviço externo que consome a minha aplicação ou é consumido por ela, 
              como meios de pagamento, CDN e etc.
          
          Processo (Process): Algo que faz alguma ação, executa lógica ou processamento e está sob nosso controle
          
          Armazenamento de dado (Data storage): Qualquer lugar onde o dado é persistido. (Banco de dados, serviço interno e etc.)
          
          Limite de Confiança (Boundary / Trust Boundary): Uma linha ou caixa que mostra onde muda o nível de confiança.
              Desenhe um boundary quando:
                  Mudar a rede (externa → interna)
                  Mudar quem controla (cliente → servidor)
                  Mudar permissões
                  
          Fluxo de dados (Data flow): A seta que mostra como os dados circulam.


      Desenhei o nosso mini-fluxo essencial abaixo usando a ferramenta "Threat Dragon v2.5.0-latest" mencionada no OWASP.
      
![Descrição da imagem](https://raw.githubusercontent.com/pinheiro-felipe/jornada-appsec-criando-api-de-postagem-de-status-do-pedido-para-delivery/320aec9d4266e95aac31f45a1a9a396930b6b3c9/docs/project/images/Modelagem-de-amea%C3%A7as-(mini-fluxo)-E1-F1-H1.png)

**1.5- Refinar o mini-fluxo desenhado, identificando**

          [ ] Endpoints.
          [ ] Serviços.
          [ ] Dados críticos e sensíveis trafegados.
          [ ] Dados críticos e sensíveis armazenados.
          [ ] Pontos que exigem criptografia.
          [ ] Pontos que exigem autenticação e autorização.
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
                          API do pedido
                          API do WhatsApp
                          Serviço de envio de mensagens
                          Fila
                          Banco de dados
                          Seu worker/processador de mensagens
                          Sua API
                          Servidor web

                      Infraestrutura
                          Host da API
                          Porta 3336 do banco
                          Container que executa o worker
<br>
<br>

### 2º- Descobrir as ameaças e o que pode dar errado ##

**2.1- Superfície de ataque**

**2.2- STRIDE: Baseado em categoria de ataque**

    Quais categorias de ataques podem afetar cada um dos componentes do mini-fluxo essencial (Process, Data Store e Data Flow)?
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



          
    
  	
      
