```python
from kivy.app import App
from kivy.uix.gridlayout import GridLayout
from kivy.uix.label import Label
from kivy.uix.progressbar import ProgressBar
from kivy.uix.behaviors import ButtonBehavior
from kivy.uix.image import Image
from kivy.uix.widget import Widget
from kivy.graphics import Color, Rectangle
from kivy.animation import Animation
import random

# Constants
EMPTY_CELL = ' '
GRID_WIDTH = 8
GRID_HEIGHT = 8
SYMBOLS = ['A', 'B', 'C', 'D', 'E']
CELL_SIZE = 50

class CellButton(ButtonBehavior, Image):
    def __init__(self, row, col, **kwargs):
        super().__init__(**kwargs)
        self.row = row
        self.col = col
        self.source = 'cell.png'  # Replace 'cell.png' with your cell image
        self.size_hint = (None, None)
        self.size = (CELL_SIZE, CELL_SIZE)

class GameState:
    PLAYING = 'playing'
    LEVEL_COMPLETED = 'level_completed'
    GAME_OVER = 'game_over'

class Match3Game(App):
    def __init__(self):
        super().__init__()
        self.grid = self.generate_grid()
        self.score = 0
        self.level = 1
        self.target_score = 1000
        self.progress_bar = ProgressBar(max=self.target_score)
        self.game_state = GameState.PLAYING
        self.selected_cell = None

    def generate_grid(self):
        return [[random.choice(SYMBOLS) for _ in range(GRID_WIDTH)] for _ in range(GRID_HEIGHT)]

    def draw_grid(self):
        layout = GridLayout(cols=GRID_WIDTH, size_hint=(None, None), width=CELL_SIZE * GRID_WIDTH, height=CELL_SIZE * GRID_HEIGHT)
        
        header_layout = GridLayout(cols=2, size_hint=(1, None), height=50)
        score_label = Label(text=f"Score: {self.score}", font_size='18sp')
        level_label = Label(text=f"Level: {self.level}", font_size='18sp')
        header_layout.add_widget(score_label)
        header_layout.add_widget(level_label)

        for row in range(GRID_HEIGHT):
            for col in range(GRID_WIDTH):
                cell_button = CellButton(row, col)
                cell_button.bind(on_press=self.on_cell_button_pressed)
                layout.add_widget(cell_button)
                
        layout.add_widget(Widget())  # For progress bar placement
        layout.add_widget(header_layout)
        layout.add_widget(self.progress_bar)
        return layout

    def on_cell_button_pressed(self, instance):
        if self.game_state == GameState.PLAYING:
            row, col = instance.row, instance.col
            if self.selected_cell is None:
                self.selected_cell = (row, col)
            else:
                if abs(row - self.selected_cell[0]) + abs(col - self.selected_cell[1]) == 1:
                    self.animate_swap(self.selected_cell[0], self.selected_cell[1], row, col)
                    self.selected_cell = None

    def animate_swap(self, row1, col1, row2, col2):
        # Animate the swap of symbols between two cells
        cell1 = [widget for widget in self.root.children if isinstance(widget, CellButton) and widget.row == row1 and widget.col == col1][0]
        cell2 = [widget for widget in self.root.children if isinstance(widget, CellButton) and widget.row == row2 and widget.col == col2][0]

        anim_cell1 = Animation(pos=cell2.pos, duration=0.2) + Animation(pos=cell1.pos, duration=0.2)
        anim_cell2 = Animation(pos=cell1.pos, duration=0.2) + Animation(pos=cell2.pos, duration=0.2)

        anim_cell1.start(cell1)
        anim_cell2.start(cell2)

        # Simulate the swap in the grid after animation completes (for demonstration purposes)
        self.grid[row1][col1], self.grid[row2][col2] = self.grid[row2][col2], self.grid[row1][col1]

        # Your existing logic for matching, updating grid, and checking win conditions
        # ...

        self.progress_bar.value = self.score
        self.root.clear_widgets()
        self.root.add_widget(self.draw_grid())

    def reset_game(self):
        # Reset game state for the next level or restart
        self.grid = self.generate_grid()
        self.score = 0
        self.selected_cell = None

    def build(self):
        return self.draw_grid()

if __name__ == "__main__":
    Match3Game().run()
```
