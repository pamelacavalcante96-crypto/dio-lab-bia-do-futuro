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

Para simplificar podemos simplesmente "injetar" os dados em nosso prompt garantindo que a gente tenha um melhor contexto possível.Lembrando que em soluções mais robustas o ideal 
```text
DADOS DO CLIENTE E PERFIL (data/perfil_investidor.json)
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

TRANSAÇÕES DO CLIENTE (data/transacoes.csv)


HISTÓRICO DE ATAENDIMENTO DO CLIENTE:
data,descricao,categoria,valor,tipo
2025-10-01,Salário,receita,5000.00,entrada
2025-10-02,Aluguel,moradia,1200.00,saida
2025-10-03,Supermercado,alimentacao,450.00,saida
2025-10-05,Netflix,lazer,55.90,saida
2025-10-07,Farmácia,saude,89.00,saida
2025-10-10,Restaurante,alimentacao,120.00,saida
2025-10-12,Uber,transporte,45.00,saida
2025-10-15,Conta de Luz,moradia,180.00,saida
2025-10-20,Academia,saude,99.00,saida
2025-10-25,Combustível,transporte,250.00,saida

PRODUTOS DISPONÍVEIS PARA ENSINO (data/produtos_financeiros.json)
[
  {
    "nome": "Tesouro Selic",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "100% da Selic",
    "aporte_minimo": 30.00,
    "indicado_para": "Reserva de emergência e iniciantes"
  },
  {
    "nome": "CDB Liquidez Diária",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "102% do CDI",
    "aporte_minimo": 100.00,
    "indicado_para": "Quem busca segurança com rendimento diário"
  },
  {
    "nome": "LCI/LCA",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "95% do CDI",
    "aporte_minimo": 1000.00,
    "indicado_para": "Quem pode esperar 90 dias (isento de IR)"
  },
  {
    "nome": "Fundo Imobiliário (FII)",
    "categoria": "fundo",
    "risco": "medio",
    "rentabilidade": "6% a 12% ao ano",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil moderado que busca diversificação e renda recorrente mensal"
  },
  {
    "nome": "Fundo de Ações",
    "categoria": "fundo",
    "risco": "alto",
    "rentabilidade": "Variável",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil arrojado com foco no longo prazo"
  }
]
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
