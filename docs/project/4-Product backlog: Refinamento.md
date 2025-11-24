
## Modelagem de ameaças passo a passo ##

### 1º- Temos que entender o que estamos estamos construindo ##

Como ponto de partida, precisamos definir o escopo do nosso modelo de ameaças. 
A definição de escopo que seguirei, é a mencionada por Izar Tarandach autor do livro "Threat modeling A Practical Guide for Development Teams" que defende a "modelagem de ameaças para cada história de usuário", pois uma modelagem geral do sistema consome tempo, recursos e logo fica obsoleta.

**1.1- História de usuário que terá suas ameaças modeladas**

    História de usuário em formato narrativo:
      
        E1-F1-H1: Como delivery de alimentação, quero adicionar uma nova postagem de mídia referente ao status atual do pedido, 
        para que o cliente acompanhe o andamento do seu pedido desde o preparo até a entrega.

        E1: Representa o épico
        F1: Representa a funcionalidade
        H1: Representa a história de usuário

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


**1.4- Desenho do mini-fluxo da história de usuário**

      Desenhei usando a ferramenta "Threat Dragon v2.5.0-latest" mencionada no OWASP.
      
      Incluí apenas elementos relevantes para a história de usuário:
      
          Entidade externa/Ator (External entity): Qualquer coisa que interage com o sistema, ou seja, quem executa a ação.
          
          Processo (Process): Algo que faz alguma ação, executa lógica ou processamento.
          
          Armazenamento de dado (Data storage): Qualquer lugar onde o dado fica guardado.
          
          Limite de Confiança (Boundary / Trust Boundary): Uma linha ou caixa que mostra onde muda o nível de segurança.
              Desenhe um boundary quando:
                  Mudar a rede (externa → interna)
                  Mudar quem controla (cliente → servidor)
                  Mudar permissões
                  
          Fluxo de dados (Data flow): A seta que mostra como os dados circulam.

![Descrição da imagem](https://raw.githubusercontent.com/pinheiro-felipe/jornada-appsec-criando-api-de-postagem-de-status-do-pedido-para-delivery/320aec9d4266e95aac31f45a1a9a396930b6b3c9/docs/project/images/Modelagem-de-amea%C3%A7as-(mini-fluxo)-E1-F1-H1.png)

**1.5- Identificar os ativos no fluxo desenhado**

      Ativo (Algo que é valioso ou crítico para o negócio e que pode ser impactado por uma ameaça em nossa história de usuário)

          Tipos de ativos:
                     
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
                  
              Processos
                  Seu worker/processador de mensagens
                  Sua API
                  Servidor web

              Infraestrutura
                  Host da API
                  Porta 3336 do banco
                  Container que executa o worker

          Identificar pontos que exigem autenticação e autorização
          Identificar quais dados críticos e sensíveis são trafegados.
          Identificar requisitoS regulatórioS ou de compliance.
          Identificar pontos que exigem criptografia
          Identificar quem precisa de permissão de acesso ao ativo e qual nível mpinimo necessário.
              
          Fronteiras de confiança, ou seja, onde a confiança muda. (Internet, backend, serviço interno, serviço externo e etc.)
          Local onde os dados são persistidos. (Banco de dados, serviço interno e etc.)
          Serviços externos se houver integração. (IDP, pagamento, CDN e etc.)
          


        
          

**8. Identificação da superfície de ataque**

* Quais endpoints, serviços ou dados estão expostos?
* Mapear somente o fluxo relevante da história.
      Qual endpoint, API, UI ou job é chamado?
  formulario ou endpoint
**11. Aplicação STRIDE (rápido)**

* O que pode dar errado nesta história?
* Quem poderia abusar deste endpoint?
* Pode ocorrer spoofing?
* Pode ocorrer tampering?
* Precisa de autenticação?
* Para cada alerta, escrever mitigação e ação (ex: autenticação obrigatória, logs, rate limit)

---

**Dicas Tarandach:**

* Foco na história de usuário, não em DFD completo
* Diagrama rápido, descartável, story-focused
* Parte do fluxo natural de implementação, não trava o sprint
* Checklist If-This / Then-That:

  * Entrada de usuário → validar, sanitizar, limitar
  * Dados enviados a outro serviço → autenticação, autorização, integridade
  * Dados sensíveis → medidas de confidencialidade

Esta ordem garante que **ativos, controle de acesso e ameaças** sejam identificados de forma **rápida, objetiva e replicável** para qualquer dev trabalhando na história.

          
    
  	
      
