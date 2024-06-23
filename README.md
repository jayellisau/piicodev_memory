# piicodev_memory

This memory game is a modern take on the classic Simon game, implemented using PiicoDev components, including RGB LEDs, a capacitive touch sensor, a buzzer, and an OLED display.

## Table of Contents

1. [Introduction](#introduction)
2. [Components](#components)
3. [Setup](#setup)
4. [Code Details](#code-details)
    - [Variables](#variables)
    - [Functions](#functions)
    - [Difficulty Settings](#difficulty-settings)
5. [Customization](#customization)
    - [Colors](#colors)
    - [Speed](#speed)
    - [Tones](#tones)
6. [Contributing](#contributing)
7. [License](#license)

## Introduction

This game challenges players to remember and repeat an increasingly complex sequence of lights and sounds. Players can select their desired difficulty at the start of the game. The OLED display provides clear instructions, live score updates, and game-over messages, enhancing the user experience. 

## Components

- Raspberry Pi Pico 
- https://piico.dev/p8 - Expansion Board for Raspberry Pi Pico
- https://piico.dev/p18 - buzzer
- https://piico.dev/p12 - touchsensor
- https://piico.dev/p13 - LED
- https://piico.dev/p14 - OLED Display

## Setup

1. Connect the PiicoDev components to your microcontroller.
2. Install the necessary PiicoDev libraries.
3. Upload the provided code to your microcontroller.

## Code Details

### Variables

- `lightUp`: Instance of `PiicoDev_RGB` to control the RGB LEDs.
- `touchSensor`: Instance of `PiicoDev_CAP1203` to read touch input.
- `buzz`: Instance of `PiicoDev_Buzzer` to control the buzzer.
- `display`: Instance of `PiicoDev_SSD1306` to control the OLED display.
- `colors`: List of RGB values for red, amber, and green.
- `tones`: List of tones (in Hz) corresponding to each button.

### Functions

- `light_up_led_and_play_tone(led_index, duration)`: Lights up an LED and plays a tone for a specified duration.
- `wait_for_button_press(debounce_ms, play_sound, light_up)`: Waits for a button press and returns the index of the pressed button.
- `play_correct_tone()`: Plays a tone indicating a correct sequence.
- `display_welcome_message()`: Displays the welcome message on the OLED.
- `display_game_over_message(score)`: Displays the game over message with the final score.
- `display_live_score(score)`: Displays the current score.
- `display_starting_game()`: Displays a starting game message.
- `select_difficulty()`: Allows the player to select a difficulty level.

### Difficulty Settings

The difficulty settings affect the speed of the light sequence:

- **Easy**: Initial display duration of 500ms, minimum duration of 200ms, decrement by 50ms.
- **Medium**: Initial display duration of 300ms, minimum duration of 50ms, decrement by 30ms.
- **Hard**: Initial display duration of 200ms, minimum duration of 25ms, decrement by 20ms.

## Customization

### Colors

The LED colors can be customized by changing the `colors` list. Each color is an RGB value:

```python
blue = [0, 0, 255]
amber = [225, 165, 0]
green = [0, 255, 0]
colors = [blue, amber, green]
```
### Speed
The speed of the game can be adjusted by modifying the difficulty settings in the 
select_difficulty function:
```python
def select_difficulty():
    display_welcome_message()
    choice = wait_for_button_press(play_sound=False, light_up=False)  # Get the difficulty choice
    if choice == 0:  # Easy
        return 500, 200, 50, "easy"
    elif choice == 1:  # Medium
        return 300, 50, 30, "medium"
    elif choice == 2:  # Hard
        return 200, 25, 20, "hard"
```
### Tones
The tones corresponding to each button can be changed by modifying the tones list:

```python
tones = [800, 600, 400]
```
