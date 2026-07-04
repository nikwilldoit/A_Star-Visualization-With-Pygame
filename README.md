# Pygame Grid Pathfinding (A* Visualizer)

This project implements an interactive visualization of the A* pathfinding algorithm using Python and Pygame, allowing users to create custom grids, place obstacles, and observe how the shortest path between two points is computed step by step.

The application is designed as an educational tool to help users understand how A* evaluates nodes, calculates heuristic values, and explores the search space to find optimal paths.

## Background

This project was developed as an assignment for the Artificial Intelligence course at Vilnius University during my Erasmus studies.  
Following my instructor’s suggestion, the implementation was selected to be showcased to the rest of the class, as the instructor appreciated the clarity of the design and the educational focus of the visualization.  
The goal of the presentation was to help fellow students understand how the A* algorithm works in practice through an intuitive, step-by-step visual demonstration.

## Features

- Interactive grid creation with customizable dimensions.
- Two input modes: manual setup or file-based map loading.
- Real-time visualization of A* execution with open/closed sets and final path.
- Display of f, g, and h values inside each cell during pathfinding. 
- Mouse-based placement and removal of barriers, start and end nodes.
- Export and import of grid layouts in a simple text format (`map.txt`).
- Dual rendering: plain mode for small grids, zoom/pan mode for large grids (≥ 70×70).
- Stable visualization during algorithm execution (no camera movement while A* runs).

## Technologies

| Technology      | Purpose                                   |
|-----------------|-------------------------------------------|
| Python 3.x      | Programming language              |
| Pygame          | Graphics rendering and event handling     |
| `PriorityQueue` | Managing the open set in the A* algorithm |
| `math` module   | Calculating heuristic distances           |

## How It Works

### Coordinate System

The project uses a logical coordinate system with origin at the bottom-left corner, where coordinates start at (1, 1) and the y-axis increases upwards (Cartesian convention).
Internal array indices are converted to and from this logical system to make position specification more intuitive for users.

### A* Algorithm

The A* algorithm minimizes the cost function \(f(n) = g(n) + h(n)\), where \(g(n)\) is the actual cost from the start node to node \(n\), and \(h(n)\) is the heuristic estimate from \(n\) to the goal.
This implementation uses the Manhattan distance heuristic, which is admissible and well suited for grid movement in four cardinal directions (up, down, left, right).

High-level steps:

1. Initialize the open set with the start node.  
2. While the open set is not empty:
   - Select the node with the lowest f value.
   - If the node is the goal, reconstruct and return the path.
   - Mark the current node as closed.
   - For each neighbor, compute a tentative g score; if it improves the path, update g, h, f and add the neighbor to the open set.
3. If the open set becomes empty, no path exists.

Each node explores up to four neighbors (no diagonal movement), which ensures optimality under Manhattan distance.

### Program Architecture

- **Node class**: encapsulates grid cell state (open, closed, barrier, start, end, path), position, neighbors, and g/h/f values, as well as drawing logic with text overlays.
- **Grid management**: functions to create, draw, and update the grid.
- **A\*** **module**: pathfinding logic with hooks for visualization updates.
- **Input handling**: processes mouse and keyboard events.
- **Rendering system**: plain mode for small grids, zoomed surface with pan/zoom for large grids. 
- **File I/O**: text-based export/import of maps (`map.txt`).

## Controls

| Input             | Action                                       |
|-------------------|----------------------------------------------|
| Left mouse click  | Place start, end, or barrier cell            |
| Right mouse click | Clear cell                                   |
| `SPACE`           | Run A* algorithm                             |
| `S`               | Save current grid to `map.txt`               |
| `C`               | Clear entire grid                            |
| `Q`               | Zoom in (large grids only)                   |
| `E`               | Zoom out (large grids only)                  |
| Arrow keys        | Pan camera (large grids only)                |
During execution each cell shows its current f value at the top and the breakdown `g+h` at the bottom, helping users see how the algorithm evaluates different paths.

## Display & Color Coding

Typical display configuration: window size 1180×780, base cell size 10 pixels, zoom range 0.5×–3.0×, zoom mode enabled for grids ≥ 70×70.

| State       | Color | RGB             |
|------------|--------|-----------------|
| Empty cell | White  | (255, 255, 255) |
| Start node | Blue   | (0, 0, 255)     |
| End node   | Red    | (255, 0, 0)     |
| Barrier    | Brown  | (150, 75, 0)    |
| Open set   | Green  | (0, 255, 0)     |
| Closed set | Grey   | (128, 128, 128) |
| Path       | Pink   | (255, 192, 203) |
| Grid lines | Grey   | (128, 128, 128) |

## Map File Format

Maps are stored in a simple text format to facilitate manual editing, version control, and sharing.

- Each line represents one row of the grid.
- Characters:
  - `'0'` empty cell  
  - `'1'` barrier  
  - `'s'` start node  
  - `'t'` end node [file:1]  
- Default filename is `map.txt`.

Example:

<p align="center">
  <img width="900" height="600" alt="image" src="https://github.com/user-attachments/assets/470ba915-f12f-4cd5-9b09-6a6a9da440e8" />
</p>

<p align="center">
  <img width="900" height="600" alt="image" src="https://github.com/user-attachments/assets/5beb326e-d05f-463e-8f10-c532ffb6137c" />
</p>

## Usage

1. Run the Python script (e.g. `python main.py`).
2. Choose input mode:
   - Enter `y` to load from file and provide a map filename (e.g. `10x20.txt`).
   - Enter `n` for manual setup (dimensions, start, end).

### Manual Setup

1. Enter grid dimensions: `rows columns` (positive integers).
2. Enter start position: `row column`, using 1-based indexing.
3. Enter end position: `row column`, using 1-based indexing.
4. Use the mouse to place additional barriers.
5. Press `SPACE` to run A*.

### File Loading Mode

1. Provide the map filename (default `map.txt`). 
2. The program loads start, end, and barriers from the file. 
3. Optionally edit the grid with the mouse.
4. Press `SPACE` to run A*.

## Performance Notes

With branching factor \(b = 4\) and solution depth \(d\), A* runs in time \(O(b^d)\) and uses \(O(b^d)\) space for the open and closed sets.
A small delay (~1 ms) per step is added to keep the visualization observable; this can be tuned for faster or slower animations.

For large grids (≥ 70×70), the zoom rendering mode draws to an off-screen surface and applies scaling and camera transforms, reducing direct screen writes and keeping interaction responsive.

## Educational Value & Learning Outcomes

The project emphasizes algorithmic clarity and visual explanation of A*, making it suitable for teaching pathfinding and heuristics.
Through development, concepts such as priority queues, event-driven programming with Pygame, coordinate transformations, real-time visualization, file I/O, and object-oriented design were applied in practice.
