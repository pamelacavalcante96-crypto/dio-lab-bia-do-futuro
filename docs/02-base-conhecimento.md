# Base de Conhecimento

## Dados Utilizados

| Arquivo | Formato | Para que serve a Maya?|
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Consultar o histórico e dar continuidade ao atendimento. |
| `perfil_investidor.json` | JSON |Personalizar o atendimento de acordo com o perfil do investidor. |
| `produtos_financeiros.json` | JSON |Consultar produtos e apresentar opções adequadas ao perfil. |
| `transacoes.csv` | CSV | Identificar padrões de gastos e movimentações financeiras. |

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

O produto Fundo Imobiliário (FII) substituiu o Fundo Multimercado, pois pessoalmente me sinto mais confiente em usar apenas produtos financeiros que eu conheço. Assim, poderei validar as respostas da Maya de forma mais assertiva.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Os arquivos CSV e JSON são carregados pelo Python no início da execução da assistente, esses dados ficam disponíveis para a Maya consultar durante o atendimento.

```python
import pandas as pd
import json

historico = pd.read_csv("historico_atendimento.csv")
transacoes = pd.read_csv("transacoes.csv")

with open("perfil_investidor.json", "r", encoding="utf-8") as arquivo:
    perfil_investidor = json.load(arquivo)

with open("produtos_financeiros.json", "r", encoding="utf-8") as arquivo:
    produtos_financeiros = json.load(arquivo)
```

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

```text
DADOS DO CLIENTE E PERFIL
{
  "nome": "João Silva",
  "idade": 32,
  "profissao": "Analista de Sistemas",
  "renda_mensal": 5000.00,
  "perfil_investidor": "moderado",
  "objetivo_principal": "Construir reserva de emergência",
  "patrimonio_total": 15000.00,
  "reserva_emergencia_atual": 10000.00,
  "aceita_risco": false,
  "metas": [
    {
      "meta": "Completar reserva de emergência",
      "valor_necessario": 15000.00,
      "prazo": "2026-06"
    },
    {
      "meta": "Entrada do apartamento",
      "valor_necessario": 50000.00,
      "prazo": "2027-12"
    }
  ]
}

TRANSAÇÕES DO CLIENTE:


HISTÓRICO DO CLIENTE:


PRODUTOS DISPONÍVEIS PARA ENSINO
```

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```
