
### Modelagem de ameaças passo a passo ###

**Passo 1- O objetivo é entender o que estamos estamos construindo**

Como ponto de partida, precisamos definir o escopo do nosso modelo de ameaças. 
Nossa definição de escopo seguirá a ideia de Izar Tarandach autor do livro "Threat modeling A Practical Guide for Development Teams" que defende a "modelagem de ameaças para cada história de usuário", pois uma modelagem geral do sistema consome tempo, recursos e logo fica obsoleta.

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

**1. Identificação do ator**

* Quem exerce a ação?
* Quem deseja fazer algo?
* Quem inicia essa ação?

**2. Ação ou evento invocado**

* Qual endpoint, API, UI ou job é chamado?
* Qual evento é invocado?

**3. Mini-fluxo / diagrama da história**

* Desenhar componentes essenciais: usuário, interface, servidor de aplicação, servidor de DB
* Não incluir elementos irrelevantes para esta história

**4. Fronteiras de confiança / trust boundaries**

* Onde a confiança muda? (Internet → backend → serviço interno → terceiro)
* Identificar pontos que exigem autenticação ou criptografia

**5. Dados sensíveis envolvidos**

* Quais dados são enviados e recebidos?
* Apenas campos críticos e sensíveis.

**6. Persistência dos dados**

* Onde os dados são armazenados? (DB, serviço interno)

**7. Serviços externos**

* Há integrações com serviços externos? (IDP, pagamento, CDN)

**8. Ativos**

* Isso é algo que o sistema gerencia e que é valioso ou crítico para o negócio?
* Tem algum requisito regulatório ou de compliance?

**Checklist para identificar se algo é um ativo**

* O sistema cria, processa ou mantém esse item?

  * Exemplos: endpoint, banco de dados, serviço, fila, arquivo
  * Métrica: Se o sistema gera ou transforma dados → sim
* A perda ou corrupção desse item impactaria o negócio ou operação?

  * Impacto financeiro, operacional, regulatório ou de reputação
  * Métrica: Se sim, impacto > 0 → sim
* Existem dados ou funcionalidades valiosas associadas a ele?

  * Dados sensíveis, pedidos, informações pessoais, logs críticos
  * Métrica: Se tiver valor → sim
* Precisamos controlar acesso ou uso desse item?

  * Se usuários não autorizados poderiam alterar, acessar ou apagar
  * Métrica: Se sim → sim
* O item é único ou essencial para o fluxo do sistema?

  * Se removido, o fluxo de negócio seria interrompido
  * Métrica: Se essencial → sim

**Pergunta adicional de acesso aos ativos**

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

          
    
  	
      
