A forma que eu gosto de organizar meus ambientes segue alguns princípios:

Preferencialmente, o código não deve saber se está em produção, staging, teste, etc. Portanto, passar variáveis de ambientes para meu código, sendo essas, variáveis que carregam informações do tipo: PROD, DEV, INTERNAL, é algo que eu evito ao máximo.

Particularmente, esse tipo de prática é um problema, pois não é difícil de se criar um ambiente onde seja muito confuso manejar e controlar o que está acontecendo. Segundo minha experiência, the easiest way to manage a software is dealing with it like a black box, if that is or isn't some option to be turned on, this options should be controled via flags. https://prometheus.io/docs/prometheus/latest/feature_flags/

Take a look into the prometheus documentation, it's something like this.

O método além de funcionar para mim, funciona para outros software por aí, pois fica a cargo do engenheiro por trás daquele software decidir o que ele pretende ou não habilitar de acordo com os recursor q ele tem, a partir do momento que você coloca detalhes de ambientes de deploy no seu código, você está basicamente forçando um padrão que é algo exclusivo a como você roda aquele software naquele exato momento.

Blz, mas como alcançar isso em um ambiente minimamente produtivo. As vezes que organizei isso, a formatação dos meus projetos era a seguinte.

Dentro da minha cloud, eu separada os ambientes em diferentes projetos. Digamos que:

Projeto-production
projeto-internal
projeto-development

Na criação da infraestrutura eu também evito ao máximo colocar prefixos ou sufixos que remetam o nome do meu projeto, a infraestrutura que os 3 carregam, deve sempre ser a mesma, mas redimencionadas de maneira diferente a depender das necessidades que minha empresa demanda. Com isso, erros de digitação e detalhes de implementação entre ambientes são reduzidos drásticamente, você tem N projetos diferentes, que na teoria, carregam configuração muito parecidas. o kustomize/helm/yaml que cada serviço utiliza é quase o mesmo para todos os ambientes, o que muda? Em dev eu quero usar 500vCPU e em produção 2vCPU... A configuração em produção sõa mais agressivas que em outros ambientes.

Mas imagina ter que mexer em arquivos terraform como esses:


resource "creating_of_s3_bucket" "name-of-bucket-production" {
  name = "name-of-bucket-production"
}

resource "creating_of_s3_bucket" "name-of-bucket-internal" {
  name = "name-of-bucket-internal"
}

resource "creating_of_s3_bucket" "name-of-bucket-development" {
  name = "name-of-bucket-development"
}

Isse é um pesadelo na terra, imagina a quantidade de typos que podem acontecer nesse processo. O mesmo vale para o código que vai ser executado em cada ambiente.

Imagina chegar no seu código e ver:

```go
if os.Getenv("ENV") == "production" {
  // code for production
} else if os.Getenv("ENV") == "internal" {
  // code for internal
} else if os.Getenv("ENV") == "development" {
  // code for development
}
```

é muito mais prático, centralizar todas as feature flags que dizem respeito a funcionalidades que estão sendo mantidas, em um arquivo de configuração, e utilizar essa configuração em todo o código. Assim, fica a cargo do usuário a decisão, ele não precisa decidir isso com base no ambiente onde o software está sendo executado.