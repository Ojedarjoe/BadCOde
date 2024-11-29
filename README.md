🛠 Refactorización de Código Rígido con Patrón Strategy
Contexto: Gestión de Pedidos
El sistema actual procesa pedidos, pero tiene varios problemas de diseño. El objetivo es identificar y resolver esos problemas aplicando el Patrón Strategy, siguiendo los principios SOLID.

Código Inicial con Problemas Detectados 📜
csharp
Copy code
using System;

namespace BadCodeExample
{
    public class OrderProcessor
    {
        public void ProcessOrder(string orderType)
        {
            if (orderType == "Normal")
            {
                Console.WriteLine("Processing normal order...");
            }
            else if (orderType == "Express")
            {
                Console.WriteLine("Processing express order...");
            }
            else if (orderType == "International")
            {
                Console.WriteLine("Processing international order...");
            }
            else
            {
                Console.WriteLine("Unknown order type.");
            }
        }
    }

    public class Program
    {
        public static void Main(string[] args)
        {
            OrderProcessor processor = new OrderProcessor();
            processor.ProcessOrder("Normal");
            processor.ProcessOrder("Express");
            processor.ProcessOrder("International");
        }
    }
}
Problemas Detectados 🛑
Problema	Descripción
Código rígido	La lógica de pedidos está acoplada y requiere modificar OrderProcessor para nuevos tipos.
Violación de OCP	No cumple con el Principio Abierto/Cerrado; no se pueden extender los comportamientos sin modificar código.
Código repetitivo	Lógica similar para diferentes tipos de pedidos.
Falta de extensibilidad	No permite agregar nuevas funcionalidades de manera sencilla.
Solución Propuesta: Patrón Strategy 🚀
Objetivos de Mejora
Desacoplar la lógica de procesamiento de pedidos mediante el Patrón Strategy.
Cumplir con el Principio Abierto/Cerrado (OCP), permitiendo extender comportamientos sin modificar código existente.
Eliminar duplicación y mejorar la cohesión del sistema.
Preparar el código para futuras expansiones con nuevos tipos de pedidos.
Código Refactorizado con Patrón Strategy ✅
Paso 1: Define una Interfaz para las Estrategias
csharp
Copy code
public interface IOrderProcessor
{
    void Process();
}
Paso 2: Crea Clases para Cada Estrategia
csharp
Copy code
public class NormalOrderProcessor : IOrderProcessor
{
    public void Process() => Console.WriteLine("Processing normal order...");
}

public class ExpressOrderProcessor : IOrderProcessor
{
    public void Process() => Console.WriteLine("Processing express order...");
}

public class InternationalOrderProcessor : IOrderProcessor
{
    public void Process() => Console.WriteLine("Processing international order...");
}
Paso 3: Usa una Clase que Gestione las Estrategias
csharp
Copy code
public class OrderProcessor
{
    private readonly IOrderProcessor _orderProcessor;

    public OrderProcessor(IOrderProcessor orderProcessor)
    {
        _orderProcessor = orderProcessor;
    }

    public void ProcessOrder() => _orderProcessor.Process();
}
Paso 4: Implementa el Patrón en el Punto de Entrada
csharp
Copy code
public class Program
{
    public static void Main(string[] args)
    {
        IOrderProcessor normalProcessor = new NormalOrderProcessor();
        OrderProcessor orderProcessor = new OrderProcessor(normalProcessor);
        orderProcessor.ProcessOrder();

        IOrderProcessor expressProcessor = new ExpressOrderProcessor();
        orderProcessor = new OrderProcessor(expressProcessor);
        orderProcessor.ProcessOrder();

        IOrderProcessor internationalProcessor = new InternationalOrderProcessor();
        orderProcessor = new OrderProcessor(internationalProcessor);
        orderProcessor.ProcessOrder();
    }
}
Beneficios de la Refactorización 🎯
Extensibilidad: Ahora puedes agregar nuevos tipos de pedidos sin modificar el código existente.
Reutilización: La lógica de cada tipo de pedido está encapsulada en clases separadas.
Mantenimiento: Reducción de duplicación de código y mayor claridad en responsabilidades.
Recursos Sugeridos
DotNet Fiddle para ejecutar y probar este código en línea.
Patrones de Diseño GoF: Revisa la implementación del Patrón Strategy en escenarios reales.
