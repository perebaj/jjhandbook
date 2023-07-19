NO SQL

Minha experência com banco de dados na maior parte das vezes, incluia bastante frustação, é uma arte da computação que de fato precisa de especialistas para modelar e entender um problema que caiba como uma luva dentro do banco certo e e não adicione complexidade ou atrase a velocidade de desolvimento de seu projeto.

Como já estou a um tempo extretamente ansioso com o fato de não ter opiniões fortes sobre coisas que considero elementares no desenvolvimento de software, procurei fazer algumas reflexões básicas sobre como a escolha de um banco relacional ou NoSQL deve ser feita de acordo com o problema que você possui, e de novo, na linha de começar a me fazer pensar a formar conceitos fundamentais já presupondo que isso é mais importante que tooling.

Vamos pensar na diferença clara entre os dois tipos de banco, o que pode ter levado alguém a pensar que todo o processo de receber um objeto em memória e ao invés de modelar ele de forma que operações de busca e consulta fossem otimizadas para algo como simplesmente pegar o objeto em memória e salva-lo diretamente no seu disco rígido?

Algumas coisas que descobri foram que bancos SQL ou RDBMS, foram construidos por design para ser utilizados em máquinas gigantes, o que até faz muito sentido, pelo menos até agora nunca ouvi falar em um banco relacional que possui uma tabela distruibuida entre diferentes cluster, diferente de bancos orientados a objetos, esses sim, logo de cara, já se percebem que são fortemente atrelados a distruibuição de dados entre muitos nós de um mesh de computadores.

O ponto interessante nesse aspecto, é que essa é uma informação fundamental para entender a utilidade de cada ferramenta, não por acaso, os bancos NoSQL ganharam força no mercado, logo quando as BigTech começaram a evoluir formente na programação distrubuida logo no final da década de 90. Acredito que o inicio do barateamente do hardware deve ter uma enorme influência nesse aspecto.

Esses bancos que foram surgindo, que digo aqui estarem no topo de uma topologia, agrupam diversas outras vertentes de como os dados são salvos, por exemplo, orientados a documentaçõs, chave-valor, grafos e por ai vai...
Mas pensando em algo em comum entre eles, acho que podemos listar 3 principais aspectos:
- Non Relational
- Cluster friendly
- Schema-less
