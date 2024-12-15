# Final Project for CPE 487: Brickbreaker Game (Bat and Ball)
Authors: Adrian Torres and Angel Ordonez Retamar

## Objective:
The objective of this project is to design an FPGA-based brickbreaker game using VHDL. The game involves controlling a bat that moves horizontally to bounce a ball, with the goal of breaking bricks arranged in a grid. The ball interacts with the bricks, and the game logic handles collisions, attempted scoring, and updating the display. The project integrates various components such as ball movement, bat control, brick collision detection, VGA display output, and attempted sound generation.

## Structure:

### 1. **Top-Level Module: Pong (`pong.vhd`)**:
- The `pong.vhd` file is the top-level module of the project. It integrates all other components, including `bat_n_ball.vhd`, `vga_sync.vhd`, `leddec16.vhd`, `clk_wiz_0.vhd`, and the sound generation modules (`tone.vhd`, `wail.vhd`, and `dac_if.vhd`).
- This module coordinates the interaction between the game logic, video output, and the attempted sound generation.

### 2. Ball and Bat Logic (`bat_n_ball.vhd`):
- The `bat_n_ball.vhd` component defines the game’s core logic for ball movement, bat control, and collision detection.
- The ball’s position and direction are updated each clock cycle. The bat moves horizontally in response to user input.
- Collision detection is implemented for the ball hitting the bat, the walls, and the bricks. The ball's direction changes accordingly, and bricks are removed upon being hit.

### 3. Brick Collision and Display (`brick.vhd` and `brickmaker.vhd`):
- The `brick.vhd` component handles brick collision logic. It determines when the ball hits a brick and deactivates the brick when it is destroyed.
- The `brickmaker.vhd` component generates and arranges the bricks on the screen. Each brick is drawn, and collision detection ensures that the ball interacts with the correct brick.

### 4. VGA Display (`vga_sync.vhd`):
- The `vga_sync.vhd` component generates the necessary horizontal and vertical synchronization signals (`hsync`, `vsync`) for the VGA display.
- The video signal outputs RGB values based on the ball's position, the bat's position, and the state of each brick (visible or destroyed).
- The VGA output is responsible for rendering the game’s visual elements, including the background, ball, bat, and bricks.

### 5. Attempted Sound Effects (`siren.vhd`, `tone.vhd`, `wail.vhd`, and `dac_if.vhd`):
- The `tone.vhd` component would have generated a triangle wave sound for the game, and the `wail.vhd` component would have modulated the tone for sound effects like the ball hitting the walls or the bat.
- The `dac_if.vhd` component would have interfaced the FPGA with a DAC, converting the tone signal into audio output that can be played through a speaker.

### 6. Attempted Score Display (`leddec16.vhd`):
- The score is displayed on a 7-segment display, with each brick hit contributing to the score.
- The `leddec16.vhd` component handles the multiplexing of the 7-segment display, it would have been updated with the current score during gameplay.

### 7. Clock Management (`clk_wiz_0.vhd` and `clk_wiz_0_clk_wiz.vhd`):
- Clock management is handled by the `clk_wiz_0.vhd` and `clk_wiz_0_clk_wiz.vhd` components, which generate the necessary clock signals for the game’s various subsystems.
- A 100 MHz clock is used as the input, which is then scaled to a lower frequency to drive the audio and video systems.

## Code:

### 1. **Top-Level Integration (Pong) (`pong.vhd`)**:
- The `pong.vhd` top-level module orchestrates the entire game system.
- It instantiates the `bat_n_ball.vhd` for ball movement and collision, the `vga_sync.vhd` for VGA synchronization and rendering, the `leddec16.vhd` for score display, and the attempted sound components (`tone.vhd`, `wail.vhd`, `dac_if.vhd`) for audio output.
- It connects signals between components to control the flow of the game logic, from ball movement to video output and sound generation.

### 2. Ball and Bat Movement:
- The ball’s position is updated based on its velocity and direction. It moves across the screen, bouncing off walls and the bat.
- The bat is controlled by user input buttons and moves horizontally to intercept the ball.

### 3. Brick Collision Detection:
- The ball's position is checked against the brick grid. If a collision occurs, the corresponding brick is marked as destroyed, and the ball's direction is adjusted based on the collision side.
- The ball's trajectory is updated based on which side of the brick was hit (left, right, top, or bottom).

### 4. VGA Output:
- The `vga_sync.vhd` component continuously outputs synchronization signals to the VGA monitor, updating the pixel data for the ball, bat, and bricks based on their positions and the game state.
- When a brick is destroyed, it becomes invisible on the screen, and the score is updated accordingly.

### 5. Sound Generation(attempted):
- The `tone.vhd` and `wail.vhd` components would have modulate the pitch to generate sound effects for the ball hitting the bat, walls, and bricks.
- The audio data would be converted to serial format by the `dac_if.vhd` component and output through the DAC.

### 6. Score Display(attempted):
- The current score would have been calculated based on the number of screens cleared. The `leddec16.vhd` component displays the score on a 7-segment display, updating the digits as the score changes.

## Process:

### 1. Create a new RTL project `brick` in Vivado Quick Start:
- Create 12 new source files of file type VHDL:
  - `bat_n_ball.vhd`
  - `brick.vhd`
  - `brickmaker.vhd`
  - `clk_wiz_0.vhd`
  - `clk_wiz_0_clk_wiz.vhd`
  - `dac_if.vhd`
  - `leddec16.vhd`
  - `pong.vhd`
  - `siren.vhd`
  - `tone.vhd`
  - `vga_sync.vhd`
  - `wail.vhd`
- Create a new constraint file of file type XDC called `pong`.
- Choose **Nexys A7-100T board** for the project.
- Click **Finish**.

### 2. Add the VHDL code:
- In **design sources**, copy and paste the VHDL code from the following files:
   - `bat_n_ball.vhd`
  - `brick.vhd`
  - `brickmaker.vhd`
  - `clk_wiz_0.vhd`
  - `clk_wiz_0_clk_wiz.vhd`
  - `dac_if.vhd`
  - `leddec16.vhd`
  - `pong.vhd`
  - `siren.vhd`
  - `tone.vhd`
  - `vga_sync.vhd`
  - `wail.vhd`
- In **constraints**, copy and paste the code from `pong.xdc`.

### 3. Run synthesis.

### 4. Run implementation and open implemented design.

### 5. Generate bitstream, open hardware manager, and program device:
- Click **Generate Bitstream**.
- Click **Open Hardware Manager** and then **Open Target**, followed by **Auto Connect**.
- Click **Program Device** and select `xc7a100t_0` to download `pong.bit` to the Nexys A7-100T board.

### 6. After successfully programming the device, the screen should show a simple game of brick breaker

## Modifications:

These are the only files we have modified from the base files we got from the Lab GiHub

### 1. `pong.vhd`
DESCRIPTION OF CHANGES

### 2. `bat_n_ball.vhd`
DESCRIPTION OF CHANGES

### 3. `brick.vhd` and `brickmaker.vhd`
DESCRIPTION OF CHANGES

### 4. `tone.vhd`
DESCRIPTION OF CHANGES

### 5. `pong.xdc`
DESCRIPTION OF CHANGES

## Summary:
The project implements a complete brickbreaker game. It integrates components for ball movement, bat control, brick collision detection, attempted score tracking, VGA display output, and attempted sound generation. The game logic is encapsulated in modular VHDL components, which communicate with each other to create an interactive and visually engaging game. The final product demonstrates how FPGA-based design can be used for real-time game development with video output.
