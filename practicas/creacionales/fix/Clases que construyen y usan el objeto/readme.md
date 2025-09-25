Refactor Creacional - Oscar Miguel Ching Ochoa  
1️⃣ Generate C# dotnet 8 bad code problem, to apply GoF in  DotNet 8  a random real world case. Present table with 10 objetives to resolve.  Publish the badcode to the user in csharp dotnet 8. Use emojis.
📝 What’s Wrong Here

God class doing everything.

Tight coupling between ordering, cooking, payment, and billing.

Hardcoded logic (if-else explosion).

No abstraction for payment or product types.

📝 Qué está mal

Clase Dios que hace todo.

Acoplamiento fuerte entre pedidos, cocina, pagos y facturación.

Lógica con if/else repetitiva y rígida.

No hay abstracciones para pagos ni productos.

me puedes ayudar con el código con esto Clases que construyen y usan el objeto

Claro 😊.
Lo que quieres es ver cómo separar (refactorizar) la clase “Dios” en clases que construyan y usen objetos en vez de hacerlo todo en una sola.

Para este caso podemos usar dos patrones GoF muy comunes:

Builder → para construir la pizza paso a paso.

Factory Method / Abstract Factory → para crear objetos del mismo “tipo” sin usar if/else.

Así separamos:

Una clase que construye (Builder/Factory).

Otras clases que usan el objeto construido (el sistema principal).


using System;
using System.Collections.Generic;

namespace RestauranteConBuilder
{
    // =======================
    // PRODUCTO
    // =======================
    public class Pizza
    {
        public string Tipo { get; set; } = "";
        public List<string> Ingredientes { get; } = new();

        public void MostrarInfo()
        {
            Console.WriteLine($"Pizza: {Tipo}");
            Console.WriteLine("Ingredientes:");
            foreach (var ing in Ingredientes)
            {
                Console.WriteLine($" - {ing}");
            }
        }
    }

    // =======================
    // INTERFAZ BUILDER
    // =======================
    public interface IPizzaBuilder
    {
        void CrearBase();
        void AgregarIngredientes();
        Pizza ObtenerPizza();
    }

    // =======================
    // BUILDERS CONCRETOS
    // =======================
    public class PizzaMargaritaBuilder : IPizzaBuilder
    {
        private Pizza _pizza = new Pizza();

        public void CrearBase()
        {
            _pizza.Tipo = "Margarita";
            _pizza.Ingredientes.Add("Masa tradicional");
            _pizza.Ingredientes.Add("Salsa de tomate");
        }

        public void AgregarIngredientes()
        {
            _pizza.Ingredientes.Add("Queso mozzarella");
        }

        public Pizza ObtenerPizza() => _pizza;
    }

    public class PizzaPepperoniBuilder : IPizzaBuilder
    {
        private Pizza _pizza = new Pizza();

        public void CrearBase()
        {
            _pizza.Tipo = "Pepperoni";
            _pizza.Ingredientes.Add("Masa tradicional");
            _pizza.Ingredientes.Add("Salsa de tomate");
        }

        public void AgregarIngredientes()
        {
            _pizza.Ingredientes.Add("Queso mozzarella");
            _pizza.Ingredientes.Add("Pepperoni");
        }

        public Pizza ObtenerPizza() => _pizza;
    }

    // =======================
    // DIRECTOR (USA EL BUILDER)
    // =======================
    public class Cocina
    {
        public Pizza PrepararPizza(IPizzaBuilder builder)
        {
            builder.CrearBase();
            builder.AgregarIngredientes();
            return builder.ObtenerPizza();
        }
    }

    // =======================
    // PROGRAMA PRINCIPAL
    // =======================
    public class Program
    {
        public static void Main()
        {
            var cocina = new Cocina();

            // Construir Margarita
            IPizzaBuilder builderMargarita = new PizzaMargaritaBuilder();
            Pizza pizzaMargarita = cocina.PrepararPizza(builderMargarita);
            pizzaMargarita.MostrarInfo();

            Console.WriteLine();

            // Construir Pepperoni
            IPizzaBuilder builderPepperoni = new PizzaPepperoniBuilder();
            Pizza pizzaPepperoni = cocina.PrepararPizza(builderPepperoni);
            pizzaPepperoni.MostrarInfo();
        }
    }
}



