# 📘 DOCUMENTO MESTRE — ESCOPO GERAL DO SISTEMA DE ATENDIMENTO INTELIGENTE (Versão 1.0)

## 🧭 1. VISÃO GERAL DO PRODUTO

**Nome:** Afixgraf Lead Master  
**Objetivo:** Automatizar atendimento via IA no WhatsApp, permitir que vendedoras assumam leads, registrar e exibir tudo num painel com métricas de desempenho.

---

## 🏛️ 2. ARQUITETURA DO SISTEMA (MACRO)

**Componentes:**
- WhatsApp (Evolution API)
- n8n (pipeline inteligente + IA + Redis)
- Supabase + Postgres (dados e histórico)
- Backend REST API (painel e controle)
- Frontend (painel vendedoras + dashboard admin)

---

## 🔀 3. FLUXO OPERACIONAL COMPLETO

1. Cliente envia msg no WhatsApp
2. Evolution → Webhook no n8n
3. n8n processa (classifica, junta, envia para IA)
4. IA responde (se autorizado)
5. Se IA estiver pausada → vendedora responde via painel
6. Vendedora assume lead no painel (backend altera status no banco)
7. Mensagens da vendedora entram no n8n via webhook exclusivo
8. n8n envia para Evolution → cliente
9. Histórico salvo em chat_messages + dados_cliente
10. Admin pode ver métricas, controle de equipe, produtividade etc.

---

## 🗄️ 4. ESTRUTURA DAS TABELAS

### **dados_cliente**
- id (uuid)
- telefone
- nomewpp
- atendimento_ia ("active", "pause", "reativada")
- responsavel_atual
- responsavel_id
- assumido_em
- criado_em, atualizado_em

### **chats**
- id
- phone
- criado_em
- atualizado_em

### **chat_messages**
- id
- phone
- agent_type ("ia", "cliente", "vendedora")
- agent_name
- message
- message_type ("text", "audio", "image")
- event ("incoming", "outgoing", "outgoing_vendor")
- respondido_em
- criado_em

### **vendedoras**
- id
- nome
- email
- senha_hash
- ativo

### **n8n_chat_histories**
*(técnico – fluxo IA)*

---

## 🧩 5. COMPONENTES E INTERSEÇÕES

FRONTEND (Painel) ──→ BACKEND API ──→ n8n Webhook ──→ Evolution API
↑ ↓
Supabase/Postgres ← Histórico
---

## 🧠 6. CASOS DE USO

### Atores:
- Cliente
- IA
- Vendedora
- Administrador

### Fluxos:
- Cliente envia mensagem → IA responde
- Vendedora assume lead → IA pausa
- Vendedora responde via painel
- Mensagem vai para n8n → Evolution → WhatsApp
- Tudo salvo no banco
- Admin vê métricas e histórico

---

## 🔁 7. SEQUÊNCIA COMPLETA (WhatsApp → IA → Painel → WhatsApp)

Cliente → Evolution → n8n → IA → WhatsApp
↘
Webhook painel (vendedora)
↘
Mensagem humana → n8n → WhatsApp


---

## 🧭 8. TELAS DO SISTEMA (Painel Vendedora + Admin)

### Vendedora:
- Login
- Lista de leads
- Tela de chat com lead
- Botões: Assumir, Responder, Voltar IA

### Admin:
- Dashboard de produtividade
- Lista de atendimentos
- Filtros por instância, vendedora, tempo

---

## 📊 9. RELATÓRIOS E MÉTRICAS

- Leads por vendedora
- Tempo médio de resposta
- Leads novos vs. em atendimento
- Conversas assumidas vs. IA
- Conversões por vendedora

---

## 🧠 10. PAPÉIS NO SISTEMA

- **Cliente**: interage via WhatsApp
- **IA**: responde enquanto não pausada
- **Vendedora**: assume lead, responde via painel
- **Admin**: vê métricas, supervisiona
- **Sistema (n8n + backend)**: controla tudo

---

## ✅ 11. PRINCIPAIS FUNCIONALIDADES

- IA responde com RAG + base interna
- Vendedora assume via botão
- Histórico unificado com agente, horário e canal
- API REST com autenticação
- Integração real-time com Evolution e n8n
- Métricas visuais no painel

---

## 🔐 12. REQUISITOS NÃO FUNCIONAIS

- Segurança (login, autenticação)
- Disponibilidade alta
- Logs e rastreamento
- Baixa latência
- Escalabilidade

---

## ✅ STATUS: Projeto em Sprint 0 (Arquitetura pronta)

Pronto para seguir para a Sprint 1 assim que aprovado.





