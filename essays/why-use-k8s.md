* Git Ops

    * IAC
    * Git
    * Continuos Integration

## Ambientes
* Camada: DB, network, Storage, Compute
* Grau: Staging, Prod
* Circulo: Funcionário, Beta, Premium


# Docker 
agrupa todos os mecanismos que voce precisa para rodar algo, em uma unica ferarmenta

# Kubernetes & Docker - Kelsey Hightower
Could be compared with post mail, the letter are an abstraction to containers, the  only concern that you need is build the content that will be inside, put a address and send. Now, the path that the letter travel until reach the destination could be considered as a Kubenetes, you don't know how things are doing, but the letter reach. - Promise theory

---

Passei um tempo refletindo sobre o que eu acho sobre kubernetes, coisas como e quando deve ser usado, se é um sistema muito complexo para resolver qualquer tipo de problema e de fato é irrefutavel que para cada problema existem ferramentas que vão se encaixar melhor ou não, a escolha e implantação delas fica por conta de cada engenheiro. 

Mas a minha reflexão foi mais tendenciosa para entender sobre sistemas complexos e eles não precisam ser só voltados para computação, vamos pensar por exemplo sobre a Constituição de uma nação. Por mais atormentador que seja, jogar o jogo da vida sem saber suas regras que estão descritas na Constituição, mas todos vivem normalmente, sabemos uma conceito cá outro acola, mas isso não impede que vivamos em sociedade. Mesmo assim seria ignorante de minha parte, como solução para isso, dizer que a constituição deveria ser mais simples, a verdade que já percebi a algum tempo, é que não existem sistemas simples para problemas complexos, você pode seguir padrões, adotar quaiquer medidas que ache necessária para sua construção, mas é fato também que uma obra que tente descrever algo como limites para um comportamente complexo, será complexa bem como.

Consigo observar comportamentos muito semelhantes em desenvolvimento de software, uma das disciplinas da faculdade que mais me deixou abismado, foi a complexidade de Sistemas operacionais, um exemplo é toda abstração para você executar um ```ls``` feliz da vida em seu terminal, ler um arquivo, conectar um dispositivo externo a sua máquina, e por ai vai. São tantas nuances e detalhes, mas mesmo assim, todos os desenvolvedores, usam Unix diariamente em suas máquinas sem saber sobre eles, mas construem coisas incriveis mesmo assim.

Acho que por fim, posso dizer algo parecido sobre o Kubernetes, será que o desenvolvedor, por traz da maquina dele, tem tempo e vontade de saber sobre todas as matizes de infrastrutura, mesmo que básicas para colocar um sistema rodando? Será que ele quer saber como a distribuição de carga entre diferentes máquinas funciona? Detalhes sobre rede? Resolução de certificados TLS? Problemas voltados a sistemas distruidos em geral? Eu posso garantir que existem algumas peças raras que conseguem pincelar bem sobre todos esses assuntos, mas são pessoas com décadas de experiência na área. E de fato são assuntos importantes que engenheiros consistentes devem ter conhecimento. Mas a moral é que na maioria das vezes você não quer se atentar sobre isso, você quer apenas uma abstração para subir seu serviço em paz. O Kubernetes é um software que mira nessa dor. "Eu não me importo o que você vai me dar para rodar, mas te prometo que isso vai funcionar, e todo o conhecimento por traz disso, será abstraido por traz de uma API".
