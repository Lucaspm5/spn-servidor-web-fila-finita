# Dimensionamento de Servidores Web com Fila Finita via Redes de Petri Estocásticas

Repositório complementar do artigo **"Dimensionamento de Servidores Web com Fila Finita: Avaliação de Desempenho via Redes de Petri Estocásticas"**.
Trabalho 07 — Modelagem e Avaliação de Desempenho com SPN.

**Autor:** Lucas Emanuel Pereira Macêdo Silva
**Instituição:** Universidade Federal do Piauí (UFPI), Campus Picos
**Ferramenta:** Jupiter

---

## Descrição

O trabalho modela um servidor web com `N` servidores e fila de capacidade finita `C`, com descarte por saturação representado por arco inibidor. O modelo é uma SPN generalizada (GSPN), implementada e avaliada no Jupiter.

Métricas calculadas: vazão (TP), tempo médio de resposta (MRT), tempo médio de fila (MQT), utilização (U) e probabilidade de descarte (DP).

## Estrutura do repositório

```
.
├── README.md
├── modelo/
│   ├── trabalho_07.json        # modelo SPN exportado do Jupiter
│   └── trabalho_07.png         # figura do modelo
└── resultados/
    ├── E1_demanda/             # E1: taxa de chegada
    ├── E2_servidores/          # E2: número de servidores
    ├── E3_fila/                # E3: capacidade da fila
    └── E4_cenarios/            # E4: cenários combinados
```

## Modelo SPN

| Elemento | Significado | Marcação inicial / tempo |
|---|---|---|
| `P_Cheg` | Gerador de requisições | 1 ficha |
| `P_Req` | Requisição recém-chegada | 0 |
| `P_Cap` | Vagas livres na fila | `C` |
| `P_Fila` | Requisições em espera | 0 |
| `P_Srv` | Servidores livres | `N` |
| `P_Proc` | Requisições em processamento | 0 |
| `T_chg` | Chegada (temporizada) | `AD = 1/λ` |
| `T_adm` | Admissão (imediata) | — |
| `T_desc` | Descarte (imediata, inibida por `P_Cap`) | — |
| `T_ini` | Início do serviço (imediata) | — |
| `T_serv` | Processamento (temporizada, *infinite server*) | `S = 1/μ` |

Parâmetros do cenário de referência: `λ = 40 req/s` (`AD = 25 ms`), `S = 100 ms` (`μ = 10 req/s`), `N = 4`, `C = 20`. Tempo em milissegundos no Jupiter.

## Procedimento no Jupiter

Todas as varreduras usam o parâmetro `AD` (em ms) com a opção **Treat as rate** marcada, de modo que o eixo é a taxa de chegada em req/s.

### E1 — Taxa de chegada (`N = 4`, `C = 20`)

| Campo | Valor |
|---|---|
| X-Axis, Parameter | `AD` (Treat as rate) |
| Start / End / Steps | `0,01` / `0,05` / `5` (λ = 10, 20, 30, 40 e 50 req/s) |
| Lines | nenhuma |
| Métricas | uma rodada para cada: MRT, DP, U, TP e MQT |

### E2 — Número de servidores (`C = 20`)

| Campo | Valor |
|---|---|
| X-Axis, Parameter | `AD` (Treat as rate) |
| Start / End / Steps | `0,02` / `0,10` / `5` (λ = 20, 40, 60, 80 e 100 req/s) |
| Lines, 2º parâmetro | `N`, de 2 a 8, 4 steps (N = 2, 4, 6 e 8) |
| Métricas | MRT e depois DP |

A varredura linear gera também `N = 6`, que não é usado no artigo.

### E3 — Capacidade da fila (`N = 4`)

| Campo | Valor |
|---|---|
| X-Axis, Parameter | `AD` (Treat as rate) |
| Start / End / Steps | `0,01` / `0,05` / `5` (λ = 10 a 50 req/s) |
| Lines, 2º parâmetro | `C`, de 5 a 50, 4 steps (C = 5, 20, 35 e 50) |
| Métricas | MRT e depois DP |

A varredura linear gera também `C = 35`, que não é usado no artigo.

### E4 — Demanda e recursos combinados (`C = 20`)

Três simulações individuais (*Unique Simulation*), anotando U, MRT, DP e TP de cada:

| Cenário | AD (ms) | λ (req/s) | N | C |
|---|---|---|---|---|
| Cen1 | 25 | 40 | 4 | 20 |
| Cen2 | 12,5 | 80 | 4 | 20 |
| Cen3 | 12,5 | 80 | 10 | 20 |

## Mapa dos arquivos

A numeração dos arquivos originais ("Rodada X_Ey") indicava a rodada e a métrica. Nesta organização os nomes foram trocados para indicar o experimento e a métrica.

| Arquivo neste repositório | Conteúdo | Nome original |
|---|---|---|
| `resultados/E1_demanda/E1_MRT.png` | E1, MRT | Rodada 1_E1 |
| `resultados/E1_demanda/E1_DP.png` | E1, DP | Rodada 1_E2 |
| `resultados/E1_demanda/E1_U.png` | E1, U | Rodada 1_E3 |
| `resultados/E1_demanda/E1_TP.png` | E1, TP | Rodada 1_E4 |
| `resultados/E1_demanda/E1_MQT.png` | E1, MQT | Rodada 1_E5 |
| `resultados/E2_servidores/E2_MRT.*` | E2, MRT (dados + gráfico) | Rodada 2_E1 |
| `resultados/E2_servidores/E2_DP.*` | E2, DP (dados + gráfico) | Rodada 2_E2 |
| `resultados/E3_fila/E3_MRT.*` | E3, MRT (dados + gráfico) | Rodada 3_E1 |
| `resultados/E3_fila/E3_DP.*` | E3, DP (dados + gráfico) | Rodada 3_E2 |
| `resultados/E4_cenarios/E4_Cen1.txt` | E4, Cen1 | Rodada 4_E1 |
| `resultados/E4_cenarios/E4_Cen2.txt` | E4, Cen2 | Rodada 4_E2 |
| `resultados/E4_cenarios/E4_Cen3.txt` | E4, Cen3 | Rodada 4_E3 |

## Origem dos valores

- **E1 e E4:** simulação estacionária no Jupiter. Em E1, `10^6` ms por ponto e cinco execuções independentes, com IC de 95% (t de Student, 4 graus de liberdade). Em E4, `2×10^5` ms por simulação, com o IC de 95% fornecido pelo Jupiter.
- **E2 e E3:** a DP foi simulada apenas em parte dos níveis (`10^6` ms, duas execuções). Os demais valores, marcados com † na tabela do artigo, vêm da solução analítica M/M/`N`/`K` (`K = N + C`), exata para o modelo.
- Os valores extras das varreduras (`N = 6` em E2 e `C = 35` em E3) estão nos arquivos, mas não aparecem no artigo.

## Métricas

Com `E{#P}` o número esperado de fichas no lugar `P` e `S` em segundos:

- `U = E{#P_Proc} / N × 100` [%]
- `TP = E{#P_Proc} / S` [req/s]
- `DP = P{#P_Cap = 0} × 100` [%]
- `MQT = E{#P_Fila} / TP` [s]
- `MRT = (E{#P_Fila} + E{#P_Proc}) / TP` [s]

Os resultados são reportados em milissegundos.

## Limitações

O modelo supõe chegadas de Poisson, tempos de serviço exponenciais, servidores homogêneos e sem falhas. Não houve validação com medições de um servidor real; a verificação foi feita contra a solução analítica M/M/`N`/`K`.

## Como reproduzir

1. Abra `modelo/trabalho_07.json` no Jupiter.
2. Ajuste os parâmetros `AD`, `N` e `C` conforme as tabelas acima.
3. Execute as varreduras (E1 a E3) e as simulações únicas (E4) e compare com os arquivos de `resultados/`.
