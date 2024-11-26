# M2P2---C-
Moment 2, Programmering 2 C#
Uppgift
Du är fordonssamlare. Du har skapat ett program för att hålla reda på dina bilar, motorcyklar, båtar
m.fl. Skapa en lista i en Console App som visar och hanterar dina fordon. Du ska kunna loopa igenom
en dialog som gör olika saker.
- Du loopar ända tills användaren väljer att avsluta.
- Du ska kunna lägga till (+) fordon
- Du ska kunna ta bort (-) fordon
- Du ska kunna avsluta dialogen genom att ange 0 (noll)
- Du ska kunna visa dina fordon i listan på skärmen.
- Du ska kunna instantiera m.h.a. båda constructorerna.
- Du ska ta hand om olika typer av input från användaren på lämpligt sätt för att göra dialogen
felsäker. T.ex. att användaren anger fel tecken eller trycker return.
Du ska skapa en Klass för fordon. Klassen ska innehålla 4 egenskaper make, model, color och year. Du
ska också göra två Constructors med Overload. Den ena initialiserar make, model och color. Den
andra initialiserar make, model och year. Alla egenskaper ska vara skyddade.
För varje instans av klassen ska du skapa ett unikt id. Du kan göra det på valfritt sätt.
Du ska också:
 kommentera koden på ett sätt som visar att du förstår vad du gör


Kod:
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Fordon
{
    internal class Program
    {
        static void Main()
        {
            List<Vehicle> vehicles = new List<Vehicle>();
            bool running = true;

            Console.WriteLine("Välkommen till Fordonssamlaren!");

            while (running)
            {
                Console.WriteLine("\nMeny:");
                Console.WriteLine("+ : Lägg till ett fordon");
                Console.WriteLine("- : Ta bort ett fordon");
                Console.WriteLine("1 : Visa alla fordon");
                Console.WriteLine("0 : Avsluta programmet");
                Console.Write("Välj ett alternativ: ");

                string input = Console.ReadLine();

                switch (input)
                {
                    case "+":
                        AddVehicle(vehicles);
                        break;

                    case "-":
                        RemoveVehicle(vehicles);
                        break;

                    case "1":
                        ShowVehicles(vehicles);
                        break;

                    case "0":
                        running = false;
                        Console.WriteLine("Programmet avslutas...");
                        break;

                    default:
                        Console.WriteLine("Ogiltigt val. Försök igen.");
                        break;
                }
            }
        }

        // Metod för att lägga till ett fordon
        static void AddVehicle(List<Vehicle> vehicles)
        {
            Console.Write("Ange märke: ");
            string make = Console.ReadLine();

            Console.Write("Ange modell: ");
            string model = Console.ReadLine();

            Console.Write("Vill du ange färg eller år? (Färg = F, År = Å): ");
            string choice = Console.ReadLine();

            if (choice.Equals("F", StringComparison.OrdinalIgnoreCase))
            {
                Console.Write("Ange färg: ");
                string color = Console.ReadLine();
                vehicles.Add(new Vehicle(make, model, color));
            }
            else if (choice.Equals("Å", StringComparison.OrdinalIgnoreCase))
            {
                Console.Write("Ange år: ");
                if (int.TryParse(Console.ReadLine(), out int year))
                {
                    vehicles.Add(new Vehicle(make, model, year));
                }
                else
                {
                    Console.WriteLine("Ogiltigt årtal. Fordonet lades inte till.");
                }
            }
            else
            {
                Console.WriteLine("Ogiltigt val. Fordonet lades inte till.");
            }

            Console.WriteLine("Fordon tillagt!");
        }

        // Metod för att ta bort ett fordon
        static void RemoveVehicle(List<Vehicle> vehicles)
        {
            Console.Write("Ange fordon du vill ta bort: ");
            if (int.TryParse(Console.ReadLine(), out int id))
            {
                Vehicle vehicleToRemove = vehicles.Find(v => v.Id == id);

                if (vehicleToRemove != null)
                {
                    vehicles.Remove(vehicleToRemove);
                    Console.WriteLine("Fordon borttaget!");
                }
                else
                {
                    Console.WriteLine("Fordon med angivet ID hittades inte.");
                }
            }
            else
            {
                Console.WriteLine("Ogiltigt ID.");
            }
        }

        // Metod för att visa alla fordon
        static void ShowVehicles(List<Vehicle> vehicles)
        {
            if (vehicles.Count == 0)
            {
                Console.WriteLine("Inga fordon i listan.");
            }
            else
            {
                Console.WriteLine("\nLista över fordon:");
                foreach (var vehicle in vehicles)
                {
                    Console.WriteLine(vehicle);
                }
            }
        }
    }

    public class Vehicle
    {
        // Skyddade egenskaper för fordon
        protected string Make { get; set; }
        protected string Model { get; set; }
        protected string Color { get; set; }
        protected int? Year { get; set; } // nullable för att tillåta överlagring
        public int Id { get; private set; }

        // Statisk räknare för att skapa unika ID:n
        private static int idCounter = 1;

        // Konstruktor som tar make, model och color
        public Vehicle(string make, string model, string color)
        {
            Make = make;
            Model = model;
            Color = color;
            Id = GenerateId();
        }

        // Konstruktor som tar make, model och year
        public Vehicle(string make, string model, int year)
        {
            Make = make;
            Model = model;
            Year = year;
            Id = GenerateId();
        }

        // Metod för att generera ett unikt ID för varje fordon
        private static int GenerateId()
        {
            return idCounter++;
        }

        // Metod för att visa fordonets information
        public override string ToString()
        {
            return $"ID: {Id} - Märke: {Make}, Modell: {Model}, Färg: {Color ?? "Ej specificerad"}, År: {Year?.ToString() ?? "Ej specificerat"}";
        }
    }
}
