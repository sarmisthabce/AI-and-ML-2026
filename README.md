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
Quantum DriftThe ObjectiveBe the first player to empty your hand by matching cards to a "Core" that constantly changes its requirements.The SetupDeal: Each player gets 7 cards.The Core: Place the rest of the deck face down. Flip the top card over to start the Core Pile.The Variable: Flip a second card next to it. This is the Modifier.How to PlayOn your turn, you must play a card from your hand onto the Core. To "drift" a card onto the pile, it must meet one of these conditions based on the Modifier:If the Modifier is Red (Hearts/Diamonds): You must play a card that is higher in value than the current Core card.If the Modifier is Black (Spades/Clubs): You must play a card that is lower in value than the current Core card.The Match: Regardless of the Modifier, you can always play a card of the same rank (e.g., a 7 on a 7) to "Reset" the drift.If you cannot play: Draw one card. If you still can't play, your turn ends.Special Action CardsSome cards have "Quantum" effects that shake up the game:CardEffectAceThe Flip: Change the Modifier card to a new one from the deck.JackThe Skip: The next player loses their turn.QueenThe Reverse: Reverse the direction of play.KingThe Singularity: Everyone else must draw 2 cards. The King counts as both Red and Black.

The first person to play their last card wins. However, you must shout "Drift!" when you have only one card left. If someone catches you before the next player starts their turn, you must draw 2 cards.
