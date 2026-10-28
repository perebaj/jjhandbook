# ReviewBench — Methodology

https://review-bench.ai/methodology/

Corpus público (219 PRs, com commits base/head congelados): https://github.com/review-bench/ReviewBench/blob/main/corpus/manifest.json

Benchmark aberto para agentes de AI code review. O artigo descreve como eles constroem o ground truth e calculam os scores — e é, na prática, uma aula de que os fundamentos de estatística e ciência de dados continuam sendo a espinha dorsal de avaliação de modelos/agentes.

## Resumo

- **Corpus**: 219 PRs reais, congelados no commit em que a revisão aconteceu, escolhidos para cobrir a distribuição real de code review (linguagens, tipos de mudança, maturidade de projeto).
- **Golden set (ground truth)**: findings vêm de produtores heterogêneos (comentários humanos, ações inferidas do autor, linters, agentes LLM) para não enviesar o corpus para uma única fonte. Cada finding é rotulado TP/FP por um classificador (Claude Sonnet 5) operacionalizando guidelines escritas por engenheiros seniores — e depois **auditado por humanos** (47 labels corrigidos à mão).
- **Calibração do juiz**: o classificador LLM não é aceito como verdade; eles medem concordância com labels humanos — 96,6% em TP/FP, 98,7% dentro de um nível de severidade, 79,7% exato em categoria — e publicam os prompts e as taxas de desacordo.
- **Métricas**: precision/recall clássicos em duas famílias — *grounded* (só contra o golden set, comparável entre agentes) e *augmented* (inclui findings novos julgados pelo LLM, não comparável like-for-like). Ranking por F_β, com β ajustável para preferir precisão ou recall.
- **Rigor estatístico**: macro vs micro averaging (PR típico vs finding típico), métricas estratificadas por severidade/categoria (recall baixo em severity=low pode ser feature, não bug), 3 runs independentes com média e desvio padrão publicados para inspecionar variância, e uma seção explícita de *threats to validity* (golden set incompleto, dependência do classificador, convergência offline/online).

## A tese

Sim — a leitura está certa, com uma nuance. O artigo mostra que avaliar um agente (de code review ou qualquer outro) é um problema clássico de medição: definir o construto ("um revisor sênior competente agiria sobre isso?"), construir ground truth auditado, medir concordância entre anotadores, reportar precision/recall com estratificação, quantificar variância entre runs e declarar as ameaças à validade. A novidade é só a escala: o LLM-as-judge substitui a anotação humana em volume, mas **apenas depois de calibrado e auditado contra humanos** — os fundamentos não mudaram, mudou quem executa o rótulo.
