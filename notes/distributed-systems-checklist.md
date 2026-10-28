# Checklist: gerenciar sistemas distribuídos como um pro

Cada item é um título a ser detalhado depois. O tema por trás de todos: **imponha limites na camada que é dona do recurso, e assuma que todo client se comporta mal.**

## Governança de recursos

- [ ] **Limites server-side por usuário/tenant** — memória, tempo de execução, linhas lidas/retornadas. A aplicação pede com educação; o servidor impõe.
- [ ] **Quotas por tenant** — requisições simultâneas e requisições por intervalo, para que um tenant barulhento não esgote os demais.
- [ ] **Isolamento de workload** — ingest, leitura interativa e jobs de background com usuários/perfis/prioridades separados, nunca uma identidade compartilhada.
- [ ] **Teto de memória abaixo do limite de kill** — o serviço deve bater no próprio limite (erro gracioso) antes que o kernel/k8s dê OOMKill (loop de restart).
- [ ] **Regra de folga de disco** — defina o uso máximo em steady state (~70%) e o runbook de expansão *antes* do alerta de 90% disparar.

## Conexões & fluxo

- [ ] **Pooling de conexões com teto** — todo client direto carrega seu próprio pool limitado; um gateway só cobre os clients atrás dele. A quota por usuário no servidor é o orçamento global, e a soma dos orçamentos deve caber na capacidade do servidor. Comece sem proxy: profiles + quotas no servidor já fazem a governança; um proxy compartilhado (ex.: chproxy) só entra quando houver réplicas para rotear ou cache de resposta.
- [ ] **Hierarquia de timeouts** — timeout do client < timeout do gateway < kill timeout do servidor, com cancelamento propagado; senão trabalho abandonado continua queimando recurso.
  - Exemplo real: client com timeout de 130s numa query que o servidor tinha permissão de rodar até 240s. O client desistia antes do servidor, um retry automático disparava uma segunda execução idêntica, e a primeira continuava rodando — duas queries iguais ao mesmo tempo, memória dobrada, sem ninguém esperando o resultado da primeira. O fix tem duas partes que se complementam: alinhar os dois números (client timeout acima do server timeout, nunca abaixo) e, à parte, pedir que o próprio servidor cancele a query quando perceber a conexão caída — no ClickHouse isso é a setting `cancel_http_readonly_queries_on_client_close`. A segunda parte cobre toda desconexão, não só a causada por números desalinhados.
- [ ] **Fila ou fail-fast, decidido de propósito** — excesso de carga espera numa fila curta e limitada ou falha com erro claro; nunca uma fila infinita implícita.
- [ ] **Backoff com jitter na reconexão** — reconexão em massa depois de um restart é um DDoS autoinfligido (thundering herd).
- [ ] **Batching no caminho de escrita** — poucas escritas grandes vencem muitas pequenas; imponha no contrato ou bufferize server-side.

## Ciclo de vida dos dados

- [ ] **Retenção definida na criação da tabela** — TTLs nos dados de produto, não só em tabelas de sistema; retrofitar retenção em terabytes é um incidente.
- [ ] **Layout físico consciente de tenant** — particione por tempo, ordene/clusterize por tenant; nunca particione por tenant.
- [ ] **Tiered storage como degrau intermediário** — quente local / frio em object storage compra anos antes do sharding.

## Falha & recuperação

- [ ] **RPO/RTO por escrito** — quanto dado você aceita perder, quanto tempo aceita ficar fora do ar; todo o resto deriva desses dois números.
- [ ] **Backup só é real depois de um restore cronometrado** — drill periódico, duração medida em volume realista, não uma vez com 100 GiB.
- [ ] **Cadeia de backup incremental** — backup full para de escalar muito antes dos dados.
- [ ] **Drills de falha antes de prod** — mate o pod, drene o node, encha o disco; registre o que sobrevive e escreva o runbook a partir do que viu.
- [ ] **HA com custo honesto** — compare alternativas no nível de replicação que o SLA exige, não no preço do nó único.

## Operação

- [ ] **Alertas pageiam sintomas, limites previnem dano** — um alerta é a notificação de que uma proteção funcionou (ou faltou), nunca a proteção em si.
- [ ] **Allow-list de métricas** — observabilidade também tem modelo de custo; cardinalidade sem teto contamina a própria medição.
- [ ] **Load test de concorrência, não só de volume** — "quantos usuários simultâneos por nó" é o número do capacity planning; derive a regra de escala dele.
- [ ] **Upgrades ensaiados em ambiente inferior** — mesmo mecanismo (GitOps), mesmo formato de dados, antes de prod ver a versão.
