import random
import time

# List of awesome players you can pull from packs
EPIC_POOL = ["Epic Lionel Messi 🐐", "Epic Neymar Jr 🇧🇷", "Epic Ronaldinho 🤙", "Epic Cristiano Ronaldo 🇵🇹"]
HIGHLIGHT_POOL = ["Mo Salah 🇪🇬", "Kylian Mbappe 🇫🇷", "Erling Haaland 🇳🇴", "Kevin De Bruyne 🇧🇪"]
STANDARD_POOL = ["Maguire 🏴󠁧󠁢󠁥󠁮󠁧󠁿", "Mudryk 🇺🇦", "Hincapiè🇪🇨", "Antony 🇧🇷"]

class EFootballSimulator:
    def __init__(self):
        self.coins = 2000
        self.squad = []
        self.team_power = 0

    def show_menu(self):
        print("\n=== EFOOTBALL PACK & SQUAD SIMULATOR ===")
        print(f"💰 Current Coins: {self.coins}")
        print(f"🏃 Players in Squad: {len(self.squad)}")
        print(f"⚡ Total Team Power: {self.team_power}")
        print("========================================")
        print("1. Spin 1 Single Pack (100 Coins)")
        print("2. Spin 10x Mega Pack (900 Coins - Deal!)")
        print("3. View My Dream Team Squad")
        print("4. Rage Quit Game")

    def spin_pack(self):
        # Rolling a random percentage number between 0 and 100
        roll = random.uniform(0, 100)
        
        # 3% chance for an Epic, 17% for Highlight, 80% for Standard
        if roll <= 3.0:
            player = random.choice(EPIC_POOL)
            rating = random.randint(99, 103)
            print(f"🔥 UNREAL LUCK!! You pulled: {player} (Rating: {rating})")
        elif roll <= 20.0:
            player = random.choice(HIGHLIGHT_POOL)
            rating = random.randint(90, 98)
            print(f"✨ Green Lights! You pulled: {player} (Rating: {rating})")
        else:
            player = random.choice(STANDARD_POOL)
            rating = random.randint(75, 89)
            print(f"🗑️ Standard Animation... You got: {player} (Rating: {rating})")
            
        self.squad.append({"name": player, "rating": rating})
        self.update_team_power()

    def update_team_power(self):
        if not self.squad:
            self.team_power = 0
            return
        # Summing up all ratings to calculate total power
        total_ratings = sum(p["rating"] for p in self.squad)
        self.team_power = int(total_ratings / len(self.squad))

    def buy_packs(self, amount):
        cost = 100 if amount == 1 else 900
        if self.coins < cost:
            print("❌ No cash! You don't have enough eFootball coins. Go play matchday!")
            return
            
        self.coins -= cost
        print("\nConnecting to Konami servers... 🌐")
        time.sleep(1)
        
        for i in range(amount):
            print(f"Opening Pack #{i+1}...")
            self.spin_pack()
            time.sleep(0.5)

    def print_squad(self):
        if not self.squad:
            print("Your squad is totally empty! Go spin some player packs.")
            return
        print("\n--- MY CURRENT SQUAD ---")
        for index, player in enumerate(self.squad, 1):
            print(f"{index}. {player['name']} - Rating: {player['rating']}")

# Main game runtime loop
sim = EFootballSimulator()
while True:
    sim.show_menu()
    choice = input("\nChoose an option (1-4): ")
    
    if choice == "1":
        sim.buy_packs(1)
    elif choice == "2":
        sim.buy_packs(10)
    elif choice == "3":
        sim.print_squad()
    elif choice == "4":
        print("Thanks for playing my game! Drop a star on GitHub! 🌟")
        break
    else:
        print("Invalid choice bro, type a number from 1 to 4!")
