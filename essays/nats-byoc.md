# NATS e o jogo do BYOC: vendendo software para quem faz software

Existe um detalhe pouco falado sobre vender software para empresas que fazem software: empresas médias e grandes têm um diferencial na hora de comprar — elas fecham negócio muito mais fácil se o seu software **cabe dentro da cloud delas**. O modelo tem nome: BYOC (bring your own cloud). Você instala o seu produto na infraestrutura do cliente, ele paga a conta de infra e a manutenção dos recursos, e você opera com acesso limitado ao que está rodando lá dentro.

Por que isso destrava a venda? Porque tudo nasce e morre na infra deles. O dado nunca sai do perímetro, o security review encolhe, o time de compliance não precisa inventar um anexo novo de contrato, o custo de infra aparece na fatura de cloud que eles já pagam e já sabem auditar. Comprar software alheio vira um processo parecido com adotar uma dependência interna — e empresa que faz software sabe adotar dependência. O atrito de procurement, que mata deal de meses, despenca.

Só que o BYOC cobra um preço do vendedor, e ele é arquitetural: **você vai operar um sistema distribuído dentro de uma casa que não é sua, com a chave de poucas portas.** Sem SSH livre, sem pager apontando pra você, sem "deixa eu só rodar um comando aqui". Cada componente que o seu produto carrega é um componente que o cliente vai manter — e que você vai debugar por telemetria, de fora.

Esse preço reescreve os critérios de escolha de stack. E é aqui que entra o NATS.

## O que o BYOC exige do seu barramento de mensagens

Quase todo produto de dados/telemetria/eventos precisa de um barramento no meio: ingest, filas, fan-out para workers, buffer para rajada. Fora do BYOC, a resposta padrão é "usa o gerenciado da cloud" (SQS, Pub/Sub, Kinesis) ou "sobe um Kafka". Dentro do BYOC, as duas respostas quebram:

- **Serviço gerenciado da cloud** quebra porque a cloud é *deles*, e varia. Cliente A está na AWS, B no GCP, C na Azure. Se o seu design fala "SQS", você acabou de criar três produtos. O contrato do seu barramento precisa ser abstrato e a implementação precisa viajar com o produto — não pode ser um recurso que você assume existir na casa do cliente.
- **Kafka** quebra no custo de operação com acesso limitado. JVM, tuning, operadores, rebalanceamento, uma cultura inteira de administração — tudo isso agora é "manutenção que o cliente paga". No papel parece ótimo para você; na prática, cada incidente que o time deles não sabe resolver vira um ticket seu, resolvido às cegas. O barramento mais poderoso do mundo não vale nada se o seu único acesso a ele é um dashboard.

O que o BYOC exige, então: o mínimo de partes móveis, zero dependências externas, o mesmo deploy em qualquer cloud, e operabilidade por telemetria — porque telemetria talvez seja o único acesso que você tem.

## Por que o NATS cabe exatamente nesse buraco

- **Um binário estático, sem dependências.** O servidor NATS é um executável Go. Não tem JVM, não tem ZooKeeper, não tem operador obrigatório. No inventário de "coisas que o cliente mantém", ele ocupa uma linha.
- **JetStream embute a persistência.** Stream, fila de trabalho, KV e object store no mesmo binário. O que em outras stacks são três componentes (broker + storage + coordenação), aqui é um. Cada componente a menos é um incidente a menos numa infra que você não administra.
- **Cloud-agnóstico de verdade.** O mesmo manifesto sobe na AWS, no GCP, na Azure, on-prem. O contrato do produto fala "stream com dedup por message ID e consumers duráveis" — e a implementação é a mesma em todas as casas. Um produto, N clouds.
- **Operável de fora.** Clustering por Raft que se resolve sozinho, upgrade por troca de binário, footprint pequeno, e métricas de tudo: pending por consumer, ack em voo, bytes por stream. Quando o seu acesso é limitado, o sistema precisa contar a própria história pela telemetria — e contar rápido.
- **Rajada é problema do stream, não do pager.** Workload de dados vem em ondas. JetStream absorve o pico no stream e deixa os consumers drenarem no ritmo deles; você dimensiona folga, não acorda de madrugada na infra dos outros.
- **Multi-tenancy e topologia flexíveis.** Accounts e subjects isolam domínios sem infra extra; leaf nodes conectam o cluster de dentro do cliente a um plano de controle seu — quando o contrato permite — sem abrir a rede deles.

Eu vivo esse arranjo na prática: passo os dias operando um pipeline que processa terabytes por dia de telemetria com NATS no meio de tudo — ingest, fan-out para uma frota de workers, buffer de rajada — mantido por um time pequeno, majoritariamente observando de fora. A quantidade de coisas que *não* aconteceram por o barramento ser um binário com persistência embutida é o argumento silencioso mais forte que eu tenho.

## Quando o NATS não é a resposta

Honestidade intelectual: se o cliente já opera um Kafka central e exige que você use *o barramento deles*, a conversa muda — aí o diferencial vira falar o protocolo da casa. Se o seu produto depende do ecossistema Kafka (Connect, Flink, históricos de retenção infinita), a gravidade do ecossistema pesa. E NATS pede disciplina nos contratos de consumo (acks, redeliveries, dedup por message ID têm semântica própria que o time precisa dominar).

Mas se a sua demanda é a do título — vender software para quem faz software, entregando o produto dentro da casa do cliente — o critério de escolha do barramento não é throughput de benchmark. É: *quantas partes móveis você tem coragem de deixar na casa dos outros, sabendo que vai mantê-las pelo buraco da fechadura?* Para essa pergunta, eu ainda não encontrei resposta melhor que um binário.
