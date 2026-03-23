# AI-and-ML-2026
project on ai and ml


import random

class QuantumDrift:
    def __init__(self):
        self.ranks = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A']
        self.suits = ['Hearts', 'Diamonds', 'Spades', 'Clubs']
        self.values = {r: i for i, r in enumerate(self.ranks)}
        self.deck = [f"{r} of {s}" for r in self.ranks for s in self.suits]
        random.shuffle(self.deck)

        self.player_hand = [self.deck.pop() for _ in range(7)]
        self.cpu_hand = [self.deck.pop() for _ in range(7)]
        self.core = self.deck.pop()
        self.modifier = self.deck.pop()
        self.discard_pile = []

    def get_color(self, card):
        return "Red" if "Hearts" in card or "Diamonds" in card else "Black"

    def is_valid_move(self, card):
        card_rank = card.split(' ')[0]
        core_rank = self.core.split(' ')[0]
        

        if card_rank == core_rank:
            return True
        
    
        if card_rank == 'K':
            return True

       
        if self.get_color(self.modifier) == "Red":
            return self.values[card_rank] > self.values[core_rank]
        else:
            return self.values[card_rank] < self.values[core_rank]

    def apply_effect(self, card, hand, opponent_hand):
        rank = card.split(' ')[0]
        if rank == 'A':
            self.modifier = self.deck.pop() if self.deck else self.modifier
            print(f"--- ACE! Modifier changed to {self.modifier} ---")
        elif rank == 'K':
            for _ in range(2):
                if self.deck: opponent_hand.append(self.deck.pop())
            print("--- KING! Opponent draws 2 cards ---")
        
    def play_game(self):
        while self.player_hand and self.cpu_hand:
            print(f"\n{'='*20}")
            print(f"CORE: {self.core} | MODIFIER: {self.modifier} ({self.get_color(self.modifier)})")
            print(f"Your Hand: {', '.join(self.player_hand)}")
            
           
            valid_cards = [c for c in self.player_hand if self.is_valid_move(c)]
            if not valid_cards:
                print("No moves! Drawing a card...")
                if self.deck: self.player_hand.append(self.deck.pop())
            else:
                choice = input(f"Choose a card to play ({', '.join(valid_cards)}): ")
                if choice in valid_cards:
                    self.player_hand.remove(choice)
                    self.core = choice
                    self.apply_effect(choice, self.player_hand, self.cpu_hand)
                else:
                    print("Invalid selection, skipping turn.")

            if not self.player_hand: break

            cpu_valid = [c for c in self.cpu_hand if self.is_valid_move(c)]
            if cpu_valid:
                cpu_move = random.choice(cpu_valid)
                self.cpu_hand.remove(cpu_move)
                self.core = cpu_move
                self.apply_effect(cpu_move, self.cpu_hand, self.player_hand)
                print(f"CPU played: {cpu_move}")
            else:
                if self.deck: self.cpu_hand.append(self.deck.pop())
                print("CPU drew a card.")

        print("\nGAME OVER")
        print("You won!" if not self.player_hand else "CPU won!")

if __name__ == "__main__":
    game = QuantumDrift()
    game.play_game()


<img width="560" height="890" alt="image" src="https://github.com/user-attachments/assets/4d8c64d2-01eb-44f2-a5e5-d3ca4c58b7ca" />
why i choose this?
In the era of 5G and Edge Computing, applications must be lightweight, portable, and capable of maintaining complex "state" (the current game status) across distributed environments.
Traditional software is often bulky and hard to deploy. I observed a need for a minimalist, logic-driven application that could be used to test:

Container Orchestration: How easily can a logic-based service be deployed via Docker?

State Management: How does the system handle a "Modifier" that changes the rules of engagement in real-time?

Resource Efficiency: A game that runs with near-zero overhead, ideal for testing low-latency mobile edge nodes.

The Solution (The "How")
I developed Quantum Drift, a strategic card game engine built in Python. Unlike standard games, it uses a Dynamic Modifier Logic (Red vs. Black cards) which simulates a shifting environment—much like how a mobile network must adapt to changing signal conditions.

Key Technical Features:

Object-Oriented Design: The game is encapsulated in a QuantumDrift class, making it modular and easy to integrate into larger systems.

Algorithmic Validation: Every move is validated against a central "Core," ensuring data integrity.

CI/CD Ready: The project is structured with a Dockerfile and organized for version control on GitHub, following professional DevOps workflows.

One challenge was ensuring the 'Modifier' logic didn't create a 'Deadlock'—a state where neither the player nor the CPU could make a move. I solved this by implementing a Draw-to-Play mechanic and giving 'Kings' a wild-card status to break the loop
One challenge was ensuring the 'Modifier' logic didn't create a 'Deadlock'—a state where neither the player nor the CPU could make a move. I solved this by implementing a Draw-to-Play mechanic and giving 'Kings' a wild-card status to break the loop
