
### Modelagem de ameaças passo a passo ###

**Passo 1- O objetivo é entender o que estamos estamos construindo**

Como ponto de partida, precisamos definir o escopo do nosso modelo de ameaças. 
Nossa definição de escopo seguirá a ideia de Izar Tarandach autor do livro "Threat modeling A Practical Guide for Development Teams" que defende a "modelagem de ameaças para cada história de usuário", pois uma modelagem geral do sistema consome tempo, recursos e logo fica obsoleta.

Para exemplificar nosso processo de modelagem de ameaças da forma mais prática possível, escolhemos uma história de usuário do nosso projeto.

**1- História de usuário que terá suas ameaças modeladas**

      História de usuário em formato narrativo:
      
         E1-F1-H1: Como delivery de alimentação, quero adicionar uma nova postagem de mídia referente ao status atual do pedido, 
         para que o cliente acompanhe o andamento do seu pedido desde o preparo até a entrega.


      Mesma história de usuário em formato estruturado:

         Ator: Como delivery de alimentação

         Ação: quero adicionar

         Dado: uma nova postagem de mídia referente ao status atual do pedido

         Ator que recebe o valor (ator implícito ou explícito): para que o cliente
    
         Valor recebido: acompanhe o andamento do seu pedido desde o preparo até a entrega.

**2- Identificação do ator**

      Quem exerce a ação?
      Quem deseja fazer algo?

         Ator: Delivery de alimentação
         
**3- Ação ou evento invocado**

      Qual evento é invocado?

         Evento + Dado: Adicionar um nova postagem de mídia referente ao status atual do pedido


**4- Desenhar mini-fluxo / diagrama da história de usuário**

      Desenhei o mini-fluxo usando a ferramenta "Threat Dragon v2.5.0-latest" mencionada no OWASP.
      
      Incluí apenas elementos relevantes para a história de usuário:
      
          Ator
          Ativos (Algo que é valioso ou crítico para o negócio e que pode ser impactado por uma ameaça em nossa história de usuário)
              Identificar pontos que exigem autenticação ou criptografia
              Identificar quais dados críticos e sensíveis são trafegados.
              Identificar requisitoS regulatórioS ou de compliance.

              Tipos de ativos
                     
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
                         
          Fronteiras de confiança, ou seja, onde a confiança muda. (Internet, backend, serviço interno, serviço externo e etc.)
          Local onde os dados são persistidos. (Banco de dados, serviço interno e etc.)
          Serviços externos se houver integração. (IDP, pagamento, CDN e etc.)
          
![Descrição da imagem](https://raw.githubusercontent.com/pinheiro-felipe/jornada-appsec-criando-api-de-postagem-de-status-do-pedido-para-delivery/320aec9d4266e95aac31f45a1a9a396930b6b3c9/docs/project/images/Modelagem-de-amea%C3%A7as-(mini-fluxo)-E1-F1-H1.png)

        
          
**Checklist para identificar se algo é um ativo**

* Precisamos controlar acesso ou uso desse item?

* Quem precisa de acesso a esse ativo e qual nível?

**7. Controle de acesso da funcionalidade**

* Quem pode executar esta funcionalidade?
* Exemplos:

  * Gestor da plataforma: leitura apenas
  * Diretor da plataforma: leitura, alteração e refazer
* Observação: determinar autenticação, autorização e privilégios diferenciados

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

          
    
  	
      
