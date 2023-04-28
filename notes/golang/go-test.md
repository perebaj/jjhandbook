## Mock para testes unitários

### Formas de fazer isso

* 1) Simular as operações de um banco de dado utilizando alguma estrutura de dados
* 2) Stubs: Objetos que imitam o comportamento de um componente real
* 3) É possivel ver em alguns blockpost qque algumas ferramentas de fato simularam um banco de dados ou até mesmo se sobe um banco paralelo para realizar os teste, mas como estamos falando de teste unitários, as abordagens 1 e 2 são mais plausíveis.

w
Geralmente a opção 1 é muito mais trabalhosa e gera muito código adicional aos seus testes, essas operações podem ficar tão complexas que você pode deixar de confiar em seus próprios teste

## Table tests

You just pass our parameters and the expected returned value

```
func TestAdd(t *testing.T) {
    tests := []struct {
        a, b, want int
    }{
        {2, 3, 5},
        {-1, 1, 0},
        {0, 0, 0},
        {100, -100, 0},
    }
    for _, tt := range tests {
        got := Add(tt.a, tt.b)
        if got != tt.want {
            t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, got, tt.want)
        }
    }
}
```