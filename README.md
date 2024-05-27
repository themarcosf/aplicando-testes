# Aplicando Testes Unitarios:

## Classe a ser testada:

* Abaixo está a classe a ser testada:

```csharp
namespace Temperatura
{
    public static class ConversorTemperatura
    {
        public static double FahrenheitParaCelsius(double temperatura)
        {
           /*return (temperatura - 32) / 1.8; // Simulação de falha*/
            return Math.Round((temperatura - 32) / 1.8, 2); 
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Digite a temperatura em Fahrenheit:");
            double fahrenheit = Convert.ToDouble(Console.ReadLine());
            double celsius = ConversorTemperatura.FahrenheitParaCelsius(fahrenheit);
            Console.WriteLine($"{fahrenheit}°F é equivalente a {celsius}°C.");
        }
    }
}

```
Nesse código, a classe estática chamada ConversorTemperatura com um método estático FahrenheitParaCelsius que converte temperaturas de Fahrenheit para Celsius, arredondando o resultado para duas casas decimais.  A classe Program contém o método Main, que solicita ao usuário uma temperatura em Fahrenheit, converte essa temperatura para Celsius usando o método FahrenheitParaCelsius e, em seguida, exibe o resultado convertido.

Para fazer os testes foi utilizado os frameworks xUnit, NUnit e MSTest. Abaixo está o código e os resultados dos respectivos frameworks.

### Teste com XUnit:

No xUnit, o atributo InlineData é usado em conjunto com o atributo Theory. O Theory indica que o método de teste é um teste parametrizado que deve ser executado múltiplas vezes com diferentes conjuntos de dados. O InlineData é usado para definir esses conjuntos de dados. Cada uso do InlineData especifica um conjunto diferente de parâmetros que serão passados para o método de teste. 

```csharp
namespace Test.XUnit.Temperatura
{
    public class TestesConversorTemperatura
    {
        [Theory]
        [InlineData(32, 0)]
        [InlineData(47, 8.33)]
        [InlineData(86, 30)]
        [InlineData(90.5, 32.5)]
        [InlineData(120.18, 48.99)]
        [InlineData(212, 100)]
        public void TestarConversaoTemperatura(
            double fahrenheit, double celsius)
        {
            double valorCalculado =
                ConversorTemperatura.FahrenheitParaCelsius(fahrenheit);
            Assert.Equal(celsius, valorCalculado);
        }
    }
}
```

#### Resultado dos testes:

* Teste falhando com a simulação de falha da função FahrenheitParaCelsius da classe ConversorTemperatura:

![teste_falhando_xunit](/Assets/falhaTesteXunit.png)

* Teste passando da função FahrenheitParaCelsius da classe ConversorTemperatura:

![teste_passando_xunit](/Assets/passandoTesteXunit%20.png)

### Teste com NUnit:

No NUnit, o TestCase é o atributo equivalente ao InlineData do xUnit. Ele também é usado para definir conjuntos de dados para testes parametrizados, mas é especificamente parte do NUnit. O TestCase permite especificar os valores de entrada e os valores esperados diretamente na assinatura do método de teste.

```csharp
namespace Temperatura.Testes
{
    public class TestesConversorTemperatura
    {
        [TestCase(32, 0)]
        [TestCase(47, 8.33)]
        [TestCase(86, 30)]
        [TestCase(90.5, 32.5)]
        [TestCase(120.18, 48.99)]
        [TestCase(212, 100)]
        public void TesteConversaoTemperatura(
            double tempFahrenheit, double tempCelsius)
        {
            double valorCalculado =
                ConversorTemperatura.FahrenheitParaCelsius(tempFahrenheit);
            Assert.AreEqual(tempCelsius, valorCalculado);
        }
    }
}
```

#### Resultado dos testes:

* Teste falhando com a simulação de falha da função FahrenheitParaCelsius da classe ConversorTemperatura:

![teste_falhando_nunit](/Assets/falhaTesteNUnit.png)


* Teste passando da função FahrenheitParaCelsius da classe ConversorTemperatura:

![teste_falhando_nunit](/Assets/passandoTesteNUnit.png)

### Teste com MSTest:

O MSTest usa o atributo DataRow em conjunto com DataTestMethod para realizar testes parametrizados. O DataRow fornecer os dados de entrada para cada iteração do teste. O DataTestMethod indica que o método de teste é um teste baseado em dados e deve ser invocado uma vez para cada DataRow fornecido.

```
namespace Temperatura.Testes
{
    [TestClass]
    public class TestesConversorTemperatura
    {
        [DataRow(32, 0)]
        [DataRow(47, 8.33)]
        [DataRow(86, 30)]
        [DataRow(90.5, 32.5)]
        [DataRow(120.18, 48.99)]
        [DataRow(212, 100)]
        [DataTestMethod]
        public void TesteConversaoTemperatura(
            double tempFahrenheit, double tempCelsius)
        {
            double valorCalculado =
                ConversorTemperatura.FahrenheitParaCelsius(tempFahrenheit);
            Assert.AreEqual(tempCelsius, valorCalculado);
        }
    }
}
```

#### Resultado dos testes:

* Teste falhando com a simulação de falha da função FahrenheitParaCelsius da classe ConversorTemperatura:

![teste_falhando_mstest](/Assets/falhaMSTest.png)


* Teste passando da função FahrenheitParaCelsius da classe ConversorTemperatura:

![teste_falhando_mstest](/Assets/passandoMSTest.png)


### Todos os testes executando:

![todos testem passando](/Assets/allTestPass.png)
