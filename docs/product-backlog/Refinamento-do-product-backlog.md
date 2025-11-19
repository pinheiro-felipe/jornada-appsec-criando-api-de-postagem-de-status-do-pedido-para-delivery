# Guia de Refinamento do Backlog – API de Postagem de Status do Pedido

## 🎯 Objetivo do Refinamento
O refinamento existe para transformar ideias e necessidades em **itens claros, pequenos, testáveis e prontos para desenvolvimento**.  
Aqui definimos:
- o que será feito  
- por que é importante  
- critérios de aceite  
- riscos  
- dependências  
- tarefas técnicas  

Este guia explica **como o refinamento é feito neste projeto**.

---

# 🧭 1. Mapear o contexto do épico
Todo refinamento começa revisando o épico:

- Qual problema ele resolve?
- Quem é o ator principal? (cliente, estabelecimento, backend etc.)
- Quais são os resultados esperados?
- Onde este épico se conecta no fluxo do usuário?

> Exemplo deste projeto: O cliente deseja acompanhar o status do seu pedido em formato de post de mídia social.

---

# 🧩 2. Dividir em funcionalidades (F1, F2, F3…)
Cada épico é quebrado em **funcionalidades independentes**, como:

- F1: Adicionar status
- F2: Pesquisar status
- F3: Remover ou atualizar status

Cada funcionalidade deve representar um pedaço completo do fluxo.

---

# 📝 3. Criar User Stories
Cada funcionalidade vira uma **história de usuário**, seguindo o padrão:

**Como `<ator>`, eu quero `<ação>` para `<resultado>`.**

Exemplo:
- Como delivery de alimentação, quero adicionar o status do pedido para registrar o avanço.

Dicas:
- Use linguagem do usuário.
- Não descreva implementação ainda.

---

# ✔️ 4. Adicionar Critérios de Aceite (GWT)
Critérios devem usar formato GWT:

- **Given** (contexto)  
- **When** (ação)  
- **Then** (resultado)

Exemplo:
Given um ID de pedido válido  
When o sistema recebe uma atualização  
Then o status é registrado e registrado com data e hora

---

# 🔍 5. Analisar riscos
Perguntas:
- Existe risco técnico?
- Existe dependência entre equipes?
- Existe risco de segurança?
- Existe risco de dados sensíveis?

Isso ajuda a priorizar.

---

# 🧯 6. Definir a Prioridade
Usamos o modelo:
- Alta (bloqueia o fluxo principal)
- Média (melhora a experiência)
- Baixa (pode ser postergado)

---

# 🛠️ 7. Criar as Tarefas Técnicas
Cada história possui 3 tipos de tarefas:

### **T1 – Back-end: Implementação**
- Estrutura da API
- Modelo de dados
- Persistência
- Logs
- Testes unitários

### **T2 – Front-end: Verificação / consumo**
- Representação do resultado
- Testes de integração
- Feedback visual

### **T3 – Infra / Operações**
- Variáveis de ambiente
- Segurança (tokens, permissões)
- Deploy
- Observabilidade

---

# 🧪 8. Criar testes de verificação
Testes manuais e automáticos:

- Validação do input
- Retorno da API
- Erros esperados
- Fluxo completo ponta a ponta

---

# 🔗 9. Vincular tudo no GitHub Project
Para cada item:
- Associar o épico
- Associar o parent issue
- Linkar documentação
- Linkar branches e pull requests

---

# 📌 10. Checklist final para marcar como “Pronto para desenvolvimento”
- [ ] História clara  
- [ ] Critérios de aceite definidos  
- [ ] Riscos avaliados  
- [ ] Tarefas técnicas criadas (BE/FE/Infra)  
- [ ] Testes definidos  
- [ ] Prioridade marcada  
- [ ] Documentação vinculada  
---

Pronto — essa é a sequência oficial usada neste projeto para refinamento.

