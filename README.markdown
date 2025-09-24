# Autonomous Delivery Agent

This project implements an autonomous delivery agent that navigates a grid-based map to find a path from a start position to a goal position. The agent supports multiple pathfinding algorithms (Uniform Cost Search, A*, and Local Search) and can handle both static and dynamic obstacles. The project includes ASCII-based and graphical (Matplotlib) visualizations of the grid and path.

## Features

- **Pathfinding Algorithms**:
  - **Uniform Cost Search (UCS)**: Finds the optimal path by exploring nodes based on cumulative path cost.
  - **A***: Uses a heuristic (Manhattan distance) for efficient pathfinding, optimal with admissible heuristics.
  - **Local Search**: A greedy approach with multiple restarts for faster, potentially suboptimal paths.
- **Grid Types**:
  - **Static Grid**: Fixed obstacles and terrain costs.
  - **Dynamic Grid**: Includes a moving obstacle that changes position over time, requiring time-aware planning.
- **Visualization**:
  - **ASCII Visualization**: Displays the grid, path, start, goal, and obstacles with optional ANSI colors.
  - **Matplotlib Plot**: Graphical visualization of the grid with a heatmap of costs, path, and obstacles.
- **Dynamic Obstacle Handling**:
  - For the `dynamic` map, the agent can replan using Local Search if the planned path is blocked by the moving obstacle.
- **Command-Line Interface**:
  - Configurable via command-line arguments for map selection, planner choice, and visualization options.

## Requirements

- **Python Version**: Python 2.7 (fully compatible) or Python 3.x.
- **Dependencies** (for `--plot` option):
  - `matplotlib`: For graphical visualization.
  - `numpy`: Required by Matplotlib for array operations.
- **Terminal**: A terminal supporting ANSI colors (e.g., Windows Command Prompt, Linux/macOS terminals) for colored ASCII visualization.

## Installation

1. **Install Python**:
   - Ensure Python 2.7 or Python 3.x is installed. Check with:
     ```bash
     python --version
     ```
   - For Python 2.7, download from [python.org](https://www.python.org/downloads/) or use a version manager like `pyenv`.
   - Python 3.6+ is recommended for modern syntax (e.g., f-strings, `print` function).

2. **Install Dependencies** (if using `--plot`):
   ```bash
   pip install matplotlib numpy
   ```
   - If installation fails, try upgrading:
     ```bash
     pip install --upgrade matplotlib numpy
     ```

3. **Clone the Repository**:
   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

4. **Save the Code**:
   - Ensure the `main3.py` file (provided in this repository) is in the project directory.

## Usage

Run the script using the command-line interface with the following options:

```bash
python main3.py [--map {small,medium,large,dynamic}] [--planner {ucs,astar,local}] [--visualize] [--plot]
```

### Command-Line Arguments
- `--map`: Choose the grid map (default: `small`).
  - Options: `small` (5x5), `medium` (10x10), `large` (20x20), `dynamic` (10x10 with moving obstacle).
- `--planner`: Choose the pathfinding algorithm (default: `ucs`).
  - Options: `ucs` (Uniform Cost Search), `astar` (A*), `local` (Local Search).
- `--visualize`: Enable ASCII grid visualization with path and obstacles.
- `--plot`: Enable Matplotlib graphical visualization (requires `matplotlib` and `numpy`).

### Example Commands
1. Run A* on the dynamic map with ASCII visualization:
   ```bash
   python main3.py --map dynamic --planner astar --visualize
   ```
2. Run UCS on the small map with both ASCII and graphical visualizations:
   ```bash
   python main3.py --map small --planner ucs --visualize --plot
   ```
3. Run Local Search on the medium map:
   ```bash
   python main3.py --map medium --planner local
   ```

### Example Output
For `python main3.py --map dynamic --planner astar --visualize`:
```
Planner: astar, Map: dynamic
Path length: 19
Cost: 22
Nodes expanded: 62
Time: 0.0025s
Path: [(0, 0), (0, 1), (0, 2), (0, 3), (0, 4), (0, 5), (0, 6), (0, 7), (0, 8), (0, 9), (1, 9), (2, 9), (3, 9), (4, 9), (5, 9), (6, 9), (7, 9), (8, 9), (9, 9)]
Grid Visualization (Dynamic 10x10, t=0)
 1 |  1 |  1 |  1 |  1 |  1 |  1 |  1 |  1 |  *
 1 |  1 |  # |  1 |  2 |  1 |  1 |  1 |  1 |  *
 1 |  2 |  1 |  1 |  1 |  3 |  1 |  1 |  1 |  *
 1 |  1 |  1 |  # |  1 |  1 |  1 |  2 |  1 |  *
 1 |  1 |  2 |  1 |  1 |  1 |  1 |  1 |  1 |  *
 1 |  1 |  1 |  1 |  1 |  D |  1 |  1 |  1 |  *
 1 |  3 |  1 |  1 |  2 |  1 |  1 |  1 |  1 |  *
 1 |  1 |  1 |  1 |  1 |  1 |  3 |  1 |  1 |  *
 1 |  1 |  2 |  1 |  1 |  1 |  1 |  1 |  # |  *
 S |  * |  * |  * |  * |  * |  * |  * |  * |  G
Path: (0, 0) -> (0, 1) -> (0, 2) -> (0, 3) -> (0, 4) -> (0, 5) -> (0, 6) -> (0, 7) -> (0, 8) -> (0, 9) -> (1, 9) -> (2, 9) -> (3, 9) -> (4, 9) -> (5, 9) -> (6, 9) -> (7, 9) -> (8, 9) -> (9, 9) (Cost: 22)
Legend: #=obstacle | S=start | G=goal | *=path | D=dynamic | Numbers=cost (blue=low, red=high)
Dynamic: D at (5, 0) (moves right 1 cell/step, wraps at col 10)
------------------------------------------------------------
```

- **Legend**:
  - `#`: Static obstacle (impassable).
  - `S`: Start position (green with ANSI colors).
  - `G`: Goal position (red with ANSI colors).
  - `*`: Path cells (yellow with ANSI colors).
  - `D`: Dynamic obstacle (magenta with ANSI colors, moves right in `dynamic` map).
  - Numbers: Terrain costs (blue for low, red for high with ANSI colors).

If `--plot` is used (with `matplotlib` installed), a graphical plot shows a heatmap of costs, black squares for obstacles, a red path, green start, red goal, and a black diamond for the dynamic obstacle (if applicable).

## Code Structure

- **Classes**:
  - `Grid`: Represents a static grid with terrain costs, obstacles, start, and goal positions.
  - `DynamicGrid`: Extends `Grid` to include a dynamic obstacle that moves right in row 5, wrapping around the grid width.
- **Pathfinding Functions**:
  - `uniform_cost_search(grid, use_time=False)`: Implements UCS, exploring nodes by lowest cumulative cost.
  - `a_star(grid, use_time=False)`: Implements A* with Manhattan distance heuristic, optimized for speed.
  - `local_search(grid, start, current_cost, goal, restarts=10)`: Greedy search with multiple restarts for fast, approximate paths.
  - `simulate_delivery(grid)`: Simulates navigation on the dynamic map, replanning with Local Search if blocked.
- **Visualization Functions**:
  - `visualize_grid(grid, path=None, t=0, ...)`: Displays an ASCII grid with path, obstacles, and costs.
  - `visualize_plot(grid, path=None, t=0)`: Generates a Matplotlib plot (requires `matplotlib` and `numpy`).
- **Main Function**:
  - Parses command-line arguments using `argparse`.
  - Initializes the grid (`Grid` or `DynamicGrid` based on `--map`).
  - Runs the specified planner and displays results and visualizations.

## Notes

- **Python 2.7 Compatibility**:
  - The code is fully compatible with Python 2.7, using:
    - `.format()` instead of f-strings.
    - Python 2.x `print` syntax (e.g., `print(' |'),` instead of `print(' |', end=' ')`).
    - New-style classes (`class Grid(object)`) for proper `super()` usage.
  - For Python 3.x, the code works without modification, but Python 3.6+ allows modern syntax (e.g., f-strings).

- **Dynamic Map**:
  - The `dynamic` map includes a moving obstacle in row 5, which shifts right one cell per time step and wraps around at column 10.
  - A* and UCS use time-aware planning (`use_time=True`) to avoid the dynamic obstacle.
  - Local Search with `--map dynamic` triggers `simulate_delivery`, which replans if the path is blocked.

- **Visualization**:
  - ASCII visualization uses ANSI colors by default. If colors don't display, modify `main()` to call:
    ```python
    visualize_grid(grid, path, t, use_colors=False)
    ```
  - Matplotlib visualization requires `matplotlib` and `numpy`. If not installed, you'll see:
    ```
    Plot error: No module named matplotlib. Use --visualize for ASCII.
    ```

- **Troubleshooting**:
  - **ModuleNotFoundError**: Ensure `matplotlib` and `numpy` are installed for `--plot`.
  - **Incorrect Path**: If the path hits obstacles, verify the map data or debug the planner logic.
  - **Syntax Errors**: If using Python 3.6+, you can revert to f-strings and `print(..., end=' ')` for cleaner code. Contact the maintainer for assistance.

## Contributing

Contributions are welcome! To contribute:
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/your-feature`).
3. Commit changes (`git commit -m "Add your feature"`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a pull request.

Please ensure code is compatible with Python 2.7 or clearly document Python 3.x requirements.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact

For issues or questions, open an issue on GitHub or contact the maintainer at [your-email@example.com](mailto:your-email@example.com).