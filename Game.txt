using System;

class Player
{
    public string Name { get; set; }
    public int Health { get; set; }
    public int AttackDamage { get; set; }

    public Player(string name, int health, int attackDamage)
    {
        Name = name;
        Health = health;
        AttackDamage = attackDamage;
    }

    public void Attack(Enemy enemy)
    {
        Console.WriteLine($"{Name} attacks {enemy.Name} for {AttackDamage} damage!");
        enemy.TakeDamage(AttackDamage);
    }

    public void Defend()
    {
        Console.WriteLine($"{Name} defends, reducing incoming damage by half!");
    }

    public void TakeDamage(int damage)
    {
        Health -= damage;
        Console.WriteLine($"{Name} takes {damage} damage! Health remaining: {Health}");
    }

    public bool IsAlive()
    {
        return Health > 0;
    }

    public void Flee()
    {
        Console.WriteLine($"{Name} has fled from the battle!");
    }
}

class Enemy
{
    public string Name { get; set; }
    public int Health { get; set; }
    public int AttackDamage { get; set; }

    public Enemy(string name, int health, int attackDamage)
    {
        Name = name;
        Health = health;
        AttackDamage = attackDamage;
    }

    public void Attack(Player player)
    {
        Console.WriteLine($"{Name} attacks {player.Name} for {AttackDamage} damage!");
        player.TakeDamage(AttackDamage);
    }

    public void TakeDamage(int damage)
    {
        Health -= damage;
        Console.WriteLine($"{Name} takes {damage} damage! Health remaining: {Health}");
    }

    public bool IsAlive()
    {
        return Health > 0;
    }

    public void Defend()
    {
        Console.WriteLine($"{Name} defends, reducing incoming damage!");
    }
}

class Game
{
    static void Main(string[] args)
    {
        Player player = new Player("IronMan", 100, 20);
        Random random = new Random();

        while (true)
        {
            // Choosing an enemy
            Enemy[] enemies = new Enemy[]
            {
                new Enemy("Flash", 60, 10),
                new Enemy("Batman", 80, 15),
                new Enemy("Superman", 100, 12)
            };

            int choice = -1;
            bool validChoice = false;

            while (!validChoice)
            {
                Console.WriteLine("Choose an enemy to fight:");
                for (int i = 0; i < enemies.Length; i++)
                {
                    Console.WriteLine($"{i + 1}. {enemies[i].Name} (Health: {enemies[i].Health}, Attack: {enemies[i].AttackDamage})");
                }

                string input = Console.ReadLine();

                // Check if input is a valid integer
                if (int.TryParse(input, out choice) && choice >= 1 && choice <= enemies.Length)
                {
                    validChoice = true;  // Check for valid choice
                }
                else
                {
                    Console.WriteLine("Invalid choice. Please choose from (1-3).");
                }
            }

            // Now that we have a valid choice, adjust for array index (1-based to 0-based)
            Enemy enemy = enemies[choice - 1];
            Console.WriteLine($"You are fighting {enemy.Name}!");

            // Combat loop
            while (player.IsAlive() && enemy.IsAlive())
            {
                // Player's turn
                Console.WriteLine("Choose an action: 1. Attack 2. Defend 3. Flee");
                string playerAction = Console.ReadLine();

                if (playerAction == "1") // Attack
                {
                    player.Attack(enemy);
                }
                else if (playerAction == "2") // Defend
                {
                    player.Defend();
                }
                else if (playerAction == "3") // Flee
                {
                    player.Flee();
                    return;
                }

                // If the enemy is still alive, they get a turn
                if (enemy.IsAlive())
                {
                    // Enemy's turn to choose a random action
                    int enemyAction = random.Next(1, 3); // Randomly choose between attack or defend
                    if (enemyAction == 1)
                    {
                        enemy.Attack(player);
                    }
                    else
                    {
                        enemy.Defend();
                    }
                }

                // Check if player is alive.
                if (!player.IsAlive())
                {
                    Console.WriteLine("You have been defeated. Rest in peace, brave adventurer.");
                    return;
                }
            }

            //Condition for Victory
            if (player.IsAlive())
            {
                Console.WriteLine($"You have defeated {enemy.Name}!");
                Console.WriteLine("Do you want to continue your journey? (y/n)");
                string continueJourney = Console.ReadLine();

                if (continueJourney.ToLower() != "y")
                {
                    Console.WriteLine("End of DC!");
                    break;
                }
            }
        }
    }
}
