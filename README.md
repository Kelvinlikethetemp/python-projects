# Kelvin Thomas 


def show_instructions():

    # Display game instructions and commands.

    print("Murder House Game")
    print("You find yourself in a dream in which you must find 6 keys made of different materials to wake up.")
    print("A silver key, a rubber key, a gold key, a wood key, a marble key, and a bronze key must all be found, and you must avoid encountering the killer.")
    print("Move commands: go North, go South, go East, go West")
    print("Add to Inventory: get 'item name'")
    print("Collect 6 keys to win the game or get caught by the murderer.")
    print('------------------------------------------')

def show_status(current_room, inventory, rooms):

    # Display players current status.

    print('------------------------------------------')
    print(f'You are in the {current_room}')

    if 'item' in rooms[current_room]:
        print(f"You see a {rooms[current_room]['item']}")
    else:
        print("There is nothing here.")

    print(f"Inventory: {inventory}")

    possible_directions = [direction for direction in rooms[current_room].keys() if direction != 'item']
    print(f"You can move in the following directions: {', '.join(possible_directions)}")
    print('------------------------------------------')

def main():
    # Define rooms and items
    rooms = {
        'Kitchen': {'South': 'Dining Room', 'North': 'Library', 'West': 'Bedroom', 'East': 'Gymnasium'},
        'Dining Room': {'East': 'Game Room', 'North': 'Kitchen', 'item': 'marble key'},
        'Game Room': {'West': 'Dining Room', 'item': 'bronze key'},
        'Bedroom': {'East': 'Kitchen', 'item': 'wooden key'},
        'Gymnasium': {'West': 'Kitchen', 'North': 'Garage', 'item': 'silver key'},
        'Garage': {'South': 'Gymnasium', 'Boss': 'killer'},
        'Library': {'South': 'Kitchen', 'East': 'Office', 'item': 'gold key'},
        'Office': {'West': 'Library', 'item': 'rubber key'},
    }

    # Start player in kitchen with no items
    current_room = 'Kitchen'
    inventory = []

    show_instructions()

    # Start the gameplay loop
    while True:
        show_status(current_room, inventory, rooms)

        # Check if the player has collected all the keys before encountering the boss
        if len(inventory) == 6:
            print('Congratulations! You have collected all the keys and escaped the dream!')
            break

        # Check for losing condition if player encounters the boss without all keys
        if 'Boss' in rooms[current_room]:
            print('You encountered the killer and failed to collect all the keys. Game Over!')
            break

        # Get the player's next move
        player_entry = input('Enter your move: ').strip().title().split(' ')
        action = player_entry[0]

        if len(player_entry) > 1:
            target = ' '.join(player_entry[1:]).title()
        else:
            target = ""

        if action == 'Go':
            if target in rooms[current_room]:
                current_room = rooms[current_room][target]
            else:
                print("You can't go that way.")

        elif action == 'Get':
            if 'item' in rooms[current_room] and target.lower() == rooms[current_room]['item']:
                if target.lower() not in inventory:
                    inventory.append(target.lower())
                    print(f'You picked up the {target}.')
                    del rooms[current_room]['item']
                else:
                    print('You already have this item.')
            else:
                print('There is no such item here.')

        else:
            print('Invalid command.')

# Run the game
if __name__ == "__main__":
    main()
