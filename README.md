# 💰 CalculadoraDescontos

![.NET](https://img.shields.io/badge/.NET-10-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![xUnit](https://img.shields.io/badge/Testes-xUnit-5C2D91?style=for-the-badge)
![License](https://img.shields.io/badge/Licença-MIT-green?style=for-the-badge)

Uma solução .NET para calcular categorias de cliente, descontos e elegibilidade a cupons, com testes **parametrizados** usando xUnit. 🧮

> [!NOTE]
> Este projeto foi desenvolvido como atividade prática da disciplina de **Garantia da Qualidade de Software**, com foco na diferença entre testes `[Fact]` e `[Theory]`.

---

## 📖 Sobre o projeto

O `DescontoService` implementa três regras de negócio, cada uma cobrindo um tipo de retorno diferente — **string**, **int** e **bool**:

| Método | Retorno | O que faz |
|---|:---:|---|
| `ObterCategoriaCliente(totalCompras)` | `string` | `"BRONZE"` (< 5 compras), `"PRATA"` (5 a 10) ou `"OURO"` (> 10) |
| `CalcularDescontoPorPercentual(valorOriginal, percentualDesconto)` | `int` | Retorna o valor final já com o desconto aplicado |
| `EValidoParaCupom(idade, primeiraCompra)` | `bool` | `true` se o cliente tiver 18 anos ou mais **OU** for a primeira compra |

---

## 🧪 `[Fact]` vs `[Theory]`: qual a diferença?

O xUnit oferece dois atributos principais para marcar métodos de teste, e este projeto usa exclusivamente o segundo:

| | `[Fact]` | `[Theory]` |
|---|---|---|
| **O que é** | Um teste único, com entrada e saída fixas no próprio código | Um teste **parametrizado**, que roda várias vezes com dados diferentes |
| **Quando usar** | Quando só existe um cenário relevante para validar | Quando a mesma lógica precisa ser validada em múltiplos cenários (ex: valores de borda, categorias diferentes) |
| **Como alimenta dados** | Não recebe parâmetros — os valores ficam escritos direto no corpo do teste | Recebe parâmetros via `[InlineData(...)]`, um conjunto por execução |
| **Resultado no relatório** | Aparece como **1 teste** | Aparece como **N testes** (um para cada `[InlineData]`), mesmo sendo um único método |

**Por que `[Theory]` foi a escolha certa aqui?** Em vez de escrever três métodos `[Fact]` quase idênticos para testar `BRONZE`, `PRATA` e `OURO` (como fizemos em atividades anteriores com `[Fact]`), um único método `[Theory]` com três `[InlineData]` testa os três cenários — evitando duplicação de código e deixando a suíte mais fácil de estender (basta adicionar uma nova linha `[InlineData]` para cobrir um caso novo).

---

## 🔬 Testes implementados

| Teste | Cenários (`[InlineData]`) | O que valida |
|---|---|---|
| `ObterCategoriaCliente_DeveRetornarCategoriaCorreta` | `(2, "BRONZE")`, `(7, "PRATA")`, `(15, "OURO")` | As três faixas de categoria de cliente |
| `CalcularDescontoPorPercentual_DeveCalcularValorFinalCorretamente` | `(100, 10, 90)`, `(200, 20, 160)`, `(50, 0, 50)` | Cálculo do valor final com desconto, incluindo o caso de 0% |
| `EValidoParaCupom_DeveValidarElegibilidadeCorretamente` | `(20, false, true)`, `(16, true, true)`, `(17, false, false)` | Maioridade, primeira compra, e a combinação de ambas |

No total, são **3 métodos de teste × 3 cenários cada = 9 execuções** reportadas pelo xUnit.

---

## 🚀 Como executar

### Pré-requisitos

![.NET SDK](https://img.shields.io/badge/Requer-.NET%2010%20SDK-blue?style=flat-square)

```bash
dotnet --version
```

### Instalação

```bash
git clone https://github.com/JoTaP-MX/calculadora-descontos-xunit.git
cd calculadora-descontos-xunit
```

### Rodando os testes

```bash
dotnet test
```

### Exemplo de saída

Resumo do teste: total: 9; falhou: 0; bem-sucedido: 9; ignorado: 0
Construir êxito em 8,6s

---

## 🛠️ Tecnologias utilizadas

- 🟣 **.NET 10**
- 🧪 **xUnit** — com uso de `[Theory]` e `[InlineData]` para testes parametrizados
- 📦 Estrutura de solução com dois projetos: `CalculadoraDescontos.App` (produção) e `CalculadoraDescontos.Tests` (testes)

---

## 👤 Sobre o Autor

Feito com 💻 por **João Pedro Gonçalves**

---

## 📄 Licença

Este projeto está sob a licença **MIT**. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.
