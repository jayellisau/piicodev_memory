"""
Raspberry Pi Pico 
https://piico.dev/p8 - Expansion Board for Raspberry Pi Pico
https://piico.dev/p18 - buzzer
https://piico.dev/p12 - touchsensor
https://piico.dev/p13 - LED
https://piico.dev/p14 - OLED Display
Madde by https://github.com/jayellisau
"""


from PiicoDev_RGB import PiicoDev_RGB
from PiicoDev_Unified import sleep_ms # Cross-platform compatible sleep function
from PiicoDev_CAP1203 import PiicoDev_CAP1203 # PiicoDev Capacitive Touch Sensor
from PiicoDev_Buzzer import PiicoDev_Buzzer # PiicoDev Buzzer
from PiicoDev_SSD1306 import create_PiicoDev_SSD1306 # PiicoDev OLED Display
import random

# Initialize the RGB LED module, touch sensor, buzzer, and OLED display
lightUp = PiicoDev_RGB()
touchSensor = PiicoDev_CAP1203()
buzz = PiicoDev_Buzzer()
display = create_PiicoDev_SSD1306()

# display.load_pbm('piicodev-logo.pbm', 1)
# display.show()
# sleep_ms(800)

# Define colors for the LEDs
red = [255, 0, 0]
amber = [225, 165, 0]
green = [0, 255, 0]
blue = [0, 0, 255]
colors = [blue, amber, green]

# Define tones for the buzzer
tones = [800, 600, 400]

# Function to light up a specific LED and play a tone simultaneously
def light_up_led_and_play_tone(led_index, duration=500):
    lightUp.clear()
    lightUp.setPixel(led_index, colors[led_index])
    lightUp.show()
    buzz.tone(tones[led_index])  # Start playing tone
    sleep_ms(duration)  # Wait for the duration
    buzz.noTone()  # Stop playing tone
    lightUp.clear()
    lightUp.show()

# Function to wait for a button press and return the index of the pressed button
def wait_for_button_press(debounce_ms=300, play_sound=True, light_up=True):
    while True:
        status = touchSensor.read()
        for i in range(3):
            if status[i + 1] == 1:
                if play_sound:
                    buzz.tone(tones[i], 300)  # Play tone for 300ms when button is pressed
                if light_up:
                    lightUp.setPixel(i, colors[i])  # Light up the corresponding LED
                    lightUp.show()
                sleep_ms(debounce_ms)  # Debounce delay
                if light_up:
                    lightUp.clear()
                    lightUp.show()
                buzz.noTone()
                return i
        sleep_ms(100)

# Function to play the correct pattern tone
def play_correct_tone():
    buzz.tone(1000, 500)  # High tone for correct pattern
    sleep_ms(500)
    buzz.noTone()

# Function to display welcome message on OLED with difficulty options
def display_welcome_message():
    display.fill(0)  # Clear the display
    display.text("RE-MEMORY GAME", 0, 0, 1)
    display.text("Easy : Press 1", 0, 15, 1)
    display.text("Med  : Press 2", 0, 30, 1)
    display.text("Hard : Press 3", 0, 45, 1)
    display.show()

# Function to display game over message on OLED
def display_game_over_message():
    display.fill(0)  # Clear the display
    display.fill_rect(20, 7, 88, 50, 1) # filled rectangle (white) 128x64
    display.text("Game Over!", 25, 30, 0)
    display.show()


def display_game_over_score(score, difficulty):
    display.fill(0)  # Clear the display
    display.text("Level : {}".format(difficulty), 0, 0, 1)
    display.text("Score : {}".format(score), 0, 15, 1)
    display.text("Press any button", 0, 30, 1)
    display.text("to restart", 20, 45, 1)
    display.show()

# Function to display live score on OLED
def display_live_score(score, difficulty):
    display.fill(0)  # Clear the display
    display.text("Level : {}".format(difficulty), 0, 0, 1)
    display.text("Score : {}".format(score), 0, 15, 1)+
#    display.text(str(score), 0, 30, 1)
    display.show()

# Function to display Good Luck on OLED
def display_starting_game(difficulty):
    display.fill(0)  # Clear the display
    display.fill_rect(20, 7, 88, 50, 1) # filled rectangle (white) 128x64
    display.text("Good Luck", 25, 30, 0)
    display.show()
    sleep_ms(800)
    display.fill(0)  # Clear the display
    display.show()
    sleep_ms(800)


# Function to select difficulty
def select_difficulty():
    display_welcome_message()
    choice = wait_for_button_press(play_sound=False, light_up=False)  # Get the difficulty choice
    if choice == 0:  # Easy
        #display_duration, min_display_duration, duration_decrement
        return 500, 200, 50, "Easy"
    elif choice == 1:  # Medium
        return 300, 50, 30, "Medium"
    elif choice == 2:  # Hard
        return 200, 20, 20, "Hard"

# Main game loop
def simon_game():
    while True:
        sequence = []
        display_duration, min_display_duration, duration_decrement, difficulty = select_difficulty()
        display_starting_game(difficulty)

        while True:
            # Add a random LED to the sequence
            next_led = random.randint(0, 2)
            sequence.append(next_led)
            incremented_sequence = [x + 1 for x in sequence]
            print(','.join(map(str, incremented_sequence)))

            # Show the sequence to the player
            for led in sequence:
                light_up_led_and_play_tone(led, duration=display_duration)
                sleep_ms(display_duration)

            # Decrease the display duration for the next round, but not below the minimum
            display_duration = max(min_display_duration, display_duration - duration_decrement)

            # Wait for the player's input
            for led in sequence:
                pressed_button = wait_for_button_press()
                if pressed_button != led:
                    print("Incorrect! Game Over.")
                    lightUp.clear()
                    display_game_over_message()
                    for _ in range(3):
                        lightUp.setPixel(0, [255, 0, 0])
                        lightUp.setPixel(1, [255, 0, 0])
                        lightUp.setPixel(2, [255, 0, 0])
                        lightUp.show()
                        buzz.tone(200, 500)  # Play low tone for incorrect pattern
                        sleep_ms(200)
                        lightUp.clear()
                        lightUp.show()
                        buzz.noTone()
                        sleep_ms(200)
                    display_game_over_score(len(sequence) - 1, difficulty)                    
                    wait_for_button_press(play_sound=False, light_up=False)  # Wait for the player to restart the game
                    break

            else:
                print("Correct! Next ")
                display_live_score(len(sequence), difficulty)
                sleep_ms(500)
                continue

            break

# Start the game
simon_game()





