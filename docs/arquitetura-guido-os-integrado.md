# Guido OS Integrado

## Objetivo

Transformar o Guido OS em uma camada executiva unica para quatro sistemas principais:

1. Guido Command Center
2. Radar da Vida
3. Guido Dividend System
4. Gestor de Imoveis

A decisao de arquitetura e simples: o Radar da Vida sera o nucleo de memoria e eventos; os demais sistemas serao paineis ou modulos especializados que leem e escrevem eventos nesse nucleo.

---

## Papel de cada modulo

### 1. Guido Command Center

Funcao: tela inicial executiva.

Responsabilidades:

- mostrar resumo do dia;
- exibir alertas;
- abrir sistemas externos;
- enviar perguntas estruturadas ao ChatGPT;
- consultar eventos do Radar da Vida;
- apresentar cards sinteticos de investimentos, imoveis, T&G, juridico, rotina e projetos.

O Command Center nao deve ser o banco de dados principal. Ele deve ser a interface de comando.

### 2. Radar da Vida

Funcao: nucleo de memoria operacional.

Responsabilidades:

- receber registros do WhatsApp;
- receber registros manuais;
- guardar eventos estruturados;
- classificar eventos por projeto, categoria, entidade e valor;
- gerar resumos diarios, semanais e mensais;
- servir como fonte para o Command Center.

### 3. Guido Dividend System

Funcao: modulo de investimentos.

Responsabilidades:

- ler extratos e dados da B3;
- organizar dividendos, ativos, proventos e historico;
- enviar eventos financeiros ao Radar da Vida;
- permitir que o Command Center mostre renda passiva, vencimentos, recebimentos e alertas.

Observacao: o Command Center pode exibir dados brutos e resumos, mas nao deve calcular, julgar ou avaliar pesos de portfolio quando essa restricao estiver ativa.

### 4. Gestor de Imoveis

Funcao: modulo patrimonial e locaticio.

Responsabilidades:

- cadastrar imoveis;
- cadastrar contratos e inquilinos;
- registrar alugueis recebidos;
- registrar despesas, manutencoes, IPTU, condominio e vistorias;
- enviar eventos ao Radar da Vida;
- permitir resumo de caixa, pendencias e rentabilidade operacional dos imoveis.

---

## Fluxo principal

```text
WhatsApp / Formularios / Sistemas
        |
        v
Radar da Vida API
        |
        v
Banco de eventos estruturados
        |
        +--> Guido Command Center
        +--> Guido Dividend System
        +--> Gestor de Imoveis
```

---

## Entidade central: Evento

Todo registro importante deve virar um evento.

Exemplo:

```json
{
  "id": "evt_20260630_0001",
  "createdAt": "2026-06-30T01:43:00-03:00",
  "date": "2026-06-30",
  "source": "manual",
  "system": "gestor-imoveis",
  "project": "Imoveis",
  "category": "Receita",
  "type": "Aluguel recebido",
  "description": "Aluguel recebido do imovel Casa Centro",
  "amount": 1200,
  "currency": "BRL",
  "status": "recebido",
  "entity": {
    "kind": "imovel",
    "name": "Casa Centro"
  },
  "tags": ["imoveis", "aluguel", "caixa"]
}
```

---

## MVP tecnico

### Fase 1

Adicionar ao Guido OS:

- manifesto dos modulos;
- schema de eventos;
- contratos de integracao;
- card visual de integracao no Command Center;
- captura local convertida em evento JSON.

### Fase 2

Conectar Command Center ao Radar da Vida:

- `GET /api/events`
- `POST /api/events`
- `GET /api/summary/today`
- `GET /api/summary/month`

### Fase 3

Conectar Dividend System:

- dividendos recebidos viram eventos;
- proventos previstos viram alertas;
- relatorios mensais alimentam o Command Center.

### Fase 4

Criar Gestor de Imoveis:

- cadastro de imovel;
- cadastro de contrato;
- lancamento de aluguel;
- lancamento de despesa;
- alerta de vencimento;
- resumo mensal por imovel.

---

## Decisao tecnica recomendada

### Curto prazo

Manter o Guido OS como site estatico no GitHub Pages, com armazenamento local e integracao por links/JSON.

### Medio prazo

Usar o Radar da Vida no Render como API intermediaria.

### Longo prazo

Migrar o banco de eventos para Supabase/PostgreSQL e manter o Render como camada de inteligencia, webhooks e classificacao por IA.

---

## Principio de produto

O sistema deve seguir uma regra:

> Um cerebro central de eventos, varios paineis especializados.

O Radar da Vida sera o cerebro. O Command Center sera a tela. O Dividend System sera o modulo financeiro. O Gestor de Imoveis sera o modulo patrimonial.
