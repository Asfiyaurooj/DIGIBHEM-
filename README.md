# DIGIBHEM-
Python intershipimport tkinter as tk
import random

class SnakeGame:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Snake Game")
        self.root.resizable(False, False)

        self.canvas = tk.Canvas(self.root, width=400, height=400, bg="black")
        self.canvas.pack()

        self.snake_coords = [(200, 200), (220, 200), (240, 200)]
        self.food_coords = self.generate_food()
        self.direction = "Right"
        self.score = 0

        self.draw_snake()
        self.draw_food()

        self.root.bind("<Key>", self.change_direction)
        self.root.after(100, self.move_snake)

        self.score_label = tk.Label(self.root, text="Score: 0", fg="white", bg="black")
        self.score_label.pack()

        self.root.mainloop()

    def generate_food(self):
        x = random.randint(0, 39) * 10
        y = random.randint(0, 39) * 10
        return (x, y)

    def draw_snake(self):
        self.canvas.delete("snake")
        for x, y in self.snake_coords:
            self.canvas.create_rectangle(x, y, x+10, y+10, fill="green", tag="snake")

    def draw_food(self):
        self.canvas.delete("food")
        x, y = self.food_coords
        self.canvas.create_rectangle(x, y, x+10, y+10, fill="red", tag="food")

    def change_direction(self, event):
        if event.keysym == "Up" and self.direction!= "Down":
            self.direction = "Up"
        elif event.keysym == "Down" and self.direction!= "Up":
            self.direction = "Down"
        elif event.keysym == "Left" and self.direction!= "Right":
            self.direction = "Left"
        elif event.keysym == "Right" and self.direction!= "Left":
            self.direction = "Right"

    def move_snake(self):
        head_x, head_y = self.snake_coords[0]
        if self.direction == "Up":
            new_head = (head_x, head_y-10)
        elif self.direction == "Down":
            new_head = (head_x, head_y+10)
        elif self.direction == "Left":
            new_head = (head_x-10, head_y)
        elif self.direction == "Right":
            new_head = (head_x+10, head_y)

        self.snake_coords.insert(0, new_head)

        if self.snake_coords[0] == self.food_coords:
            self.score += 1
            self.score_label.config(text=f"Score: {self.score}")
            self.food_coords = self.generate_food()
        else:
            self.snake_coords.pop()

        self.draw_snake()
        self.draw_food()

        if self.check_collision():
            self.game_over()
        else:
            self.root.after(100, self.move_snake)

    def check_collision(self):
        head_x, head_y = self.snake_coords[0]
        if (head_x < 0 or head_x >= 400 or
            head_y < 0 or head_y >= 400 or
            self.snake_coords[0] in self.snake_coords[1:]):
            return True
        return False

    def game_over(self):
        self.canvas.create_text(200, 200, text="Game Over!", font=("Arial", 24), fill="white")
        self.root.after(1000, self.root.destroy)

if __name__ == "__main__":
    game = SnakeGame()
