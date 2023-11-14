import random

# Player Class
class Player:
    def __init__(self, name):
        self.name = name
        self.initial_hp = 20
        self.hp = self.initial_hp
        self.hand = []
        self.life = 0
        self.shield = 0
        self.wisdom = 0

    def draw_initial_hand(self, remaining_deck):
        # Draw 4 initial cards for the player, ensuring no duplicates
        new_cards = []
        for _ in range(4):
            if remaining_deck:
                card = random.choice(remaining_deck)
                new_cards.append(card)
                remaining_deck.remove(card)
        self.hand = new_cards

    def play_round(self, remaining_deck):
        # Player's turn to play a round
        if len(self.hand) >= 1:
            self.display_hand()
            card_index = int(input(f"{self.name}, select a card by entering its number: "))
            if 1 <= card_index <= len(self.hand):
                card_to_play = self.hand[card_index - 1]
                self.hand.remove(card_to_play)
                self.use_card(card_to_play)
            else:
                print("Invalid card number. Please select a card from your hand.")
        else:
            print(f"{self.name} has no usable cards and must draw one from the remaining deck.")
            self.draw_random_card(remaining_deck)

    def use_card(self, card):
        # Use the selected card
        if card[0] == 'Red Heart':
            self.life += card[1]
        elif card[0] == 'Spades':
            self.attack_opponent(card[1])
        elif card[0] == 'Diamond':
            self.shield = card[1]
        elif card[0] == 'Plum Blossom':
            self.wisdom += card[1]

    def attack_opponent(self, damage):
        # Perform an attack on the opponent
        opponent = player2 if self == player1 else player1
        damage -= opponent.shield
        damage = max(0, damage)
        opponent.hp -= damage

    def display_hand(self):
        # Display the player's hand
        print(f"{self.name}'s hand:")
        for i, card in enumerate(self.hand):
            print(f"{i + 1}. {card[0]} {card[1]}")

    def display_status(self):
        # Display the player's status
        print(f"{self.name}: HP: {self.hp}, Life: {self.life}, Shield: {self.shield}, Wisdom: {self.wisdom}")

    def counterattack(self, boss_attack):
        # Counterattack during the boss's attack
        if len(self.hand) >= 1:
            self.display_hand()
            card_index = int(input(f"{self.name}, select a card to counterattack by entering its number: "))
            if 1 <= card_index <= len(self.hand):
                card_to_play = self.hand[card_index - 1]
                damage = boss_attack
                if card_to_play[0] == 'Plum Blossom':
                    damage += card_to_play[1]
                if card_to_play[0] == 'Spades':
                    damage -= card_to_play[1]
                damage -= self.shield
                damage = max(0, damage)
                self.hp -= damage
                self.hand.remove(card_to_play)
            else:
                print("Invalid card number. Please select a card from your hand.")

    def draw_random_card(self, remaining_deck):
        if remaining_deck:
            card = random.choice(remaining_deck)
            self.hand.append(card)
            remaining_deck.remove(card)
            print(f"{self.name} randomly obtained a card: {card[0]} {card[1]}")
        else:
            print(f"{self.name} cannot draw a card as the remaining deck is empty.")

# Enemy Class
class Enemy:
    def __init__(self, name, attack, hp):
        self.name = name
        self.attack = attack
        self.hp = hp

    def take_damage(self, damage):
        self.hp -= damage

    def is_alive(self):
        return self.hp > 0

# Function to decide trust for the boss's attack
def decide_trust():
    choice = input("Do you trust your opponent for the boss's attack? (yes/no): ").strip().lower()
    return choice == 'yes'

# Deck of cards
deck = [('Red Heart', 1), ('Red Heart', 2), ('Spades', 3), ('Spades', 4), ('Diamond', 3), ('Diamond', 4), ('Plum Blossom', 3), ('Plum Blossom', 7)]

# Initialize players
player1 = Player("Player 1")
player2 = Player("Player 2")

# Initialize enemies
jack = Enemy("Jack", 6, 18)
queen = Enemy("Queen", 6, 24)
king = Enemy("King", 6, 30)

remaining_deck = list(deck)  # Create a copy of the deck

round_number = 0
while player1.hp > 0 and player2.hp > 0:
    round_number += 1
    player1.display_status()
    player2.display_status()
    
    # Player 1's turn
    player1.draw_initial_hand(remaining_deck)
    player1.play_round(remaining_deck)

    # Player 2's turn
    player2.draw_initial_hand(remaining_deck)
    player2.play_round(remaining_deck)

    if round_number % 4 == 0:
        print("Entering 4th small cycle:")
        trust = decide_trust()
        if trust:
            print("Both players choose to be loyal friends!")
            boss_attack = (jack.attack + queen.attack + king.attack) // 2
        else:
            print("At least one player doesn't trust the other.")
            boss_attack = random.choice([jack.attack, queen.attack, king.attack])

        player1.counterattack(boss_attack)
        player2.counterattack(boss_attack)

    if round_number % 3 == 0:
        # Draw an extra card for the next round
        player1.draw_initial_hand(remaining_deck)
        player2.draw_initial_hand(remaining_deck)

if player1.hp > 0:
    print("Player 1 wins!")
elif player2.hp > 0:
    print("Player 2 wins!")
else:
    print("It's a draw!")
