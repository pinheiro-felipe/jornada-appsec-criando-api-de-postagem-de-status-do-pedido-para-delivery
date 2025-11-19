**Passo a passo**

**Passo 1- O objetivo é entender o que estamos estamos construindo**

Como ponto de partida, precisamos definir o escopo do nosso modelo de ameaças. 
  Nossa definição de escopo seguirá a ideia de Izar Tarandach autor do livro "Threat modeling A Practical Guide for Development Teams" que defende a "modelagem de ameaças para cada história de usuário", pois uma modelagem global do sistema consome tempo, recursos e logo fica obsoleta.
  Esta etapa geralmente destaca os "ativos", que podem ser qualquer coisa que você queira proteger, pontos de acesso estratégico ou itens desejados por atacantes. 
  
Identifique o ator que exerce a ação.

Qual endpoint/ação é chamada? Qual evento é invocado?

Identifique os dados sensíveis envolvidos (envio/recebimento).

Onde os dados são persistidos? (DB, serviço)

Há serviços externos envolvidos?

Ativos

Isso é algo que o sistema gerencia e que é valioso ou crítico para o negócio?

Tem algum requisito regulatório/compliance?

Checklist de ativos (criação, impacto, valor, controle de acesso, essencialidade)

Quem precisa de acesso e qual nível?

Quem pode acessar a funcionalidade? (com base nos perfis e níveis de acesso)

Identifique a superfície de ataque.

Desenhe mini-fluxo da história (apenas componentes relevantes).

Marque fronteiras/trust boundaries.
               
          
    
  	
      
