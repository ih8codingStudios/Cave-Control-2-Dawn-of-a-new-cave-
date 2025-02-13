import curses
import time
import random
import pickle

# AI Learning: Save & Load Enemy Strength Data
AI_DATA_FILE = "ai_data.pkl"

def save_ai_data(data):
    with open(AI_DATA_FILE, "wb") as f:
        pickle.dump(data, f)

def load_ai_data():
    try:
        with open(AI_DATA_FILE, "rb") as f:
            return pickle.load(f)
    except FileNotFoundError:
        return {"enemy_strength": 1}  # Default AI settings

# Load AI learning data
ai_memory = load_ai_data()
enemy_strength = ai_memory["enemy_strength"]

# Game Variables
year = 1500
population = 5
food = 100
gold = 50
wood = 100
iron = 10
research = 0
houses = 1
town_center = True
age = "Medieval Age"

# Function to Progress Time and Upgrade Age
def advance_age(stdscr):
    global year, age, research
    if research >= 100:
        year += 100
        research = 0
        if year >= 1800:
            age = "Industrial Age"
        if year >= 2100:
            age = "Future Age"
        stdscr.addstr(f"\nAdvanced to {age}! Year: {year}\n")
    else:
        stdscr.addstr("Not enough research points!\n")

# Game Loop using Curses
def main(stdscr):
    global gold, food, wood, iron, research, houses, population, town_center, year, enemy_strength

    # Initialize Curses
    curses.curs_set(0)  # Hide cursor
    stdscr.nodelay(0)  # Wait for user input
    stdscr.clear()

    while town_center:
        # Clear Screen
        stdscr.clear()

        # Display Stats
        stdscr.addstr(f"Year: {year} | Age: {age}\n")
        stdscr.addstr(f"Population: {population} | Food: {food}\n")
        stdscr.addstr(f"Gold: {gold} | Wood: {wood} | Iron: {iron}\n")
        stdscr.addstr(f"Research Points: {research} | Enemy Strength: {enemy_strength}\n\n")

        # Action Choices
        stdscr.addstr("Choose an action:\n")
        stdscr.addstr("1. Build House ($50)\n")
        stdscr.addstr("2. Farm (Increase Food)\n")
        stdscr.addstr("3. Mine (Collect Gold & Iron)\n")
        stdscr.addstr("4. Cut Lumber (Gather Wood)\n")
        stdscr.addstr("5. Train Soldiers\n")
        stdscr.addstr("6. Attack Rival Faction\n")
        stdscr.addstr("7. Research Technology\n")
        stdscr.addstr("8. Advance Age\n")
        stdscr.addstr("9. End Turn (Day/Night Cycle)\n")

        stdscr.refresh()

        # Get user input
        choice = stdscr.getch()

        if choice == ord("1"):
            cost = 50 + (houses * 50)
            if gold >= cost:
                gold -= cost
                houses += 1
                population += 5
                stdscr.addstr("\nHouse built! Population increased.\n")
            else:
                stdscr.addstr("\nNot enough gold!\n")

        elif choice == ord("2"):
            food += 20
            stdscr.addstr("\nFarming successful! Food increased.\n")

        elif choice == ord("3"):
            gold += 10
            iron += 5
            stdscr.addstr("\nMining operation complete!\n")

        elif choice == ord("4"):
            wood += 20
            stdscr.addstr("\nGathered wood!\n")

        elif choice == ord("5"):
            if gold >= 20:
                gold -= 20
                enemy_strength += 1
                stdscr.addstr("\nSoldiers trained!\n")
            else:
                stdscr.addstr("\nNot enough gold!\n")

        elif choice == ord("6"):
            battle_outcome = random.randint(0, enemy_strength)
            if battle_outcome > 2:
                gold += 50
                stdscr.addstr("\nVictory! Loot collected.\n")
                enemy_strength += 0.5  # AI learns from defeats
            else:
                population -= 2
                stdscr.addstr("\nLost battle. Population decreased!\n")

        elif choice == ord("7"):
            if food >= 10:
                food -= 10
                research += 20
                stdscr.addstr("\nResearch successful!\n")
            else:
                stdscr.addstr("\nNot enough food!\n")

        elif choice == ord("8"):
            advance_age(stdscr)

        elif choice == ord("9"):
            year += 1
            if year % 5 == 0:
                enemy_strength += 1  # AI Evolution
            stdscr.addstr(f"\nYear advanced to {year}. Day/Night cycle passed.\n")

        # AI Debugging Information
        stdscr.addstr("\n[AI Debugging]: Saving AI progress...\n")

        # Save AI learning data
        ai_memory["enemy_strength"] = enemy_strength
        save_ai_data(ai_memory)

        # Game Over Conditions
        if population <= 0:
            stdscr.addstr("All population lost. Game Over.\n")
            break

        if not town_center:
            stdscr.addstr("Town Center Destroyed! Game Over.\n")
            break

        stdscr.refresh()
        time.sleep(1)

# Run Game with Curses
curses.wrapper(main)
