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

### 3. Brick Collision and Display (`brick.vhd`):
- The `brick.vhd` component handles brick collision logic. It determines when the ball hits a brick and deactivates the brick when it is destroyed.
- It also generates and arranges the bricks on the screen. Each brick is drawn, and collision detection ensures that the ball interacts with the correct brick.

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
- Create 11 new source files of file type VHDL:
  - `bat_n_ball.vhd`
  - `brick.vhd`
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

These are the only files we have modified from the base files we got from the Lab GitHub

### 1. `pong.vhd`
- The `pong.vhd` file has been mostly kept the same.
- A `game_count` vector was added and connected to the display through the `bat_n_ball` port map
- This would have enabled the real-time tracking and visualization of the game’s progress if we had a bit more time.

### 2. `bat_n_ball.vhd`
- A new `game_count` logic vector was added to count the number of games cleared.
- Signals and constants (e.g., `colorcode`, `brick_on`) were introduced to manage new functionality.
- Components for `brick` and `tone` were added, along with the necessary signals and connections to integrate them with `bat_n_ball`.
- The color system was revamped to make it more adaptable using `colorcode`. Though initially set up for bricks to have multiple colors, a decision was made to use a uniform brick color for aesthetic reasons.
- A `bricks` signal was added to count the number of destroyed bricks. When all bricks are destroyed, the game resets, and the `game_count` is incremented.
- Port maps for `brick` and `tone` were included to complete connections between the modules.
- The `mball` process was extensively overhauled to integrate brick interactions, enabling ball movement logic to work seamlessly with brick collision and destruction.

### 3. `brick.vhd`
`brick.vhd` is a brand new file that we created in order to add in bricks and place them on the screen and to handle the ball's collision with the bricks.
INPUT PORTS:
- v_sync: synchronization signal for processing the screen refresh
- pixel_row and pixel_col: coordinates of the pixel on the screen
- left_b, right_b, top_b, bottom_b: boundaries of the brick on screen
- ball_x, ball_y: current position of the ball
- ball_x_motion_test, ball_y_motion_test: indicators of the balls motion in x and y directions
- serve and game_on: control signals to start and restart the game
- ball_speed: ball speed
OUTPUTS:
- brick_on: indicates whether the brick is being drawn on the screen
- ball_bounce_bottom, ball_bounce_top, ball_bounce_right, ball_bounce_left: signals to indicate the direction of ball bounces

INTERNAL SIGNALS
- brick_active: tracks whether the brick is visible and interactive
- once: ensures collision logic is only executed once per interaction

PROCESSES
- brickdraw: determines whether the brick sould be visible (brick_on = '1') based on the current pixel coordinates and the brick boundaries. The brick is only visible if the pixel is within the brick's boundaries, and the brick active (brick_acive = '1')
- collision: handles the logic for collisions and manages the brick's state. when serve = '1' and game_on = '0' the brick is activated and all bounce signals are cleared.
  - hits right side: ball is near the right boundary (ball_x + 8 overlaps right_b)
  - hits left side: ball is near the left boundary (ball_x - 8 overlaps left_b)
  - hits bottom side: ball is near the bottom boundary (ball_y + 8 overlaps bottom_b)
  - hits top side: ball is near the top boundary (ball_y - 8 overlaps top_b)
- when a collision occurs:
  - the corresponding bounce signal (ball_bounce_****) is set to 1
  - the brick is deactivated (brick_active = '0')
  - once is set to 1 to avoid redundant collisions
- resetting bounces:
  - if once = '1', bounce signals are reset (ball_bounce_**** = '0')

`brick.vhd`\
INPUT PORTS
- v_sync: synchronization signal for processing the screen refresh
- pixel_row and pixel_col: coordinates of the pixel on the screen
- left_b, right_b, top_b, bottom_b: boundaries of the brick on screen
- ball_x, ball_y: current position of the ball
- ball_x_motion_test, ball_y_motion_test: indicators of the balls motion in x and y directions
- serve and game_on: control signals to start and restart the game
- ball_speed: ball speed
OUTPUTS:
- brick_on: indicates whether the brick is being drawn on the screen
- ball_bounce_bottom, ball_bounce_top, ball_bounce_right, ball_bounce_left: signals to indicate the direction of ball bounces
- brick_active: tracks whether the brick is visible and interactive
- once: ensures collision logic is only executed once per interaction
- count: a signal we didn't end up getting to work correctly to count the destroyed bricks in order to try and trigger a refresh when all the bricks were destroyed

PROCESSES
- brickdraw: determines whether the brick sould be visible (brick_on = '1') based on the current pixel coordinates and the brick boundaries. The brick is only visible if the pixel is within the brick's boundaries, and the brick active (brick_acive = '1')
- collision:
  - right side: the ball overlaps the brick's right boundary
  - left side: the ball overlaps the brick's left boundary
  - bottom side: the ball overlaps the brick's bottom boundary and is moving downward (ball_y_motion_test > 0)
  - top side: the ball overlaps the brick's top boundary and is moving upward (ball_y_motion_test < 0)
  - for each collision:
    - the corresponding bounce signal is set (ball_bounce_****) to 1
    - the brick is deactivated (brick_active = '0')
    - once is set to 1 to avoid redundant collisions
  - resetting bounces:
    - if once = '1', bounce signals are reset (ball_bounce_**** = '0')

### 4. `tone.vhd`
- A `trigger` STD_LOGIC was added to initiate the sound to be played.
- An IF loop was added to play sound if `trigger` = 1

### 5. `pong.xdc`
- It is almost entirely the same from the source file.
- The necessary ports for the sound generation were added.

## Summary:
The project implements a complete brickbreaker game. It integrates components for ball movement, bat control, brick collision detection, attempted score tracking, VGA display output, and attempted sound generation. The game logic is encapsulated in modular VHDL components, which communicate with each other to create an interactive and visually engaging game. The final product demonstrates how FPGA-based design can be used for real-time game development with video output.
