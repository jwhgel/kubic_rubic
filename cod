import tkinter as tk
import time
import threading

FIRST_STATE = list(
    "WWWWWWWWW"
    "OOOOOOOOO"
    "GGGGGGGGG"
    "RRRRRRRRR"
    "BBBBBBBBB"
    "YYYYYYYYY"
)

POSITIONS = {
    "W": (1, 0), "O": (0, 1), "G": (1, 1),
    "R": (2, 1), "B": (3, 1), "Y": (1, 2)
}

COLOR = {
    "W": "#ffffff", "O": "#f97316", "G": "#10b981",
    "R": "#ef4444", "B": "#3b82f6", "Y": "#ffff00"
}

class Rubiks2DCanvas:
    def __init__(self, root):
        self.root = root
        self.root.title("Параллельная развертка кубика рубика")
        
        self.canvas = tk.Canvas(root, width=550, height=420, bg="#111622")
        self.canvas.pack()
        
        self.btn_action = tk.Button(
            root, text="Запустить анимацию", 
            font=("Arial", 11, "bold"), bg="#3b82f6", fg="white",
            command=self.startt
        )
        self.btn_action.pack(pady=15)
        
        self.state = FIRST_STATE.copy()
        self.cube()

    def cube(self):
        self.canvas.delete("all")
        face = ["W", "O", "G", "R", "B", "Y"]
        for f_idx, face_name in enumerate(face):
            face_chars = self.state[f_idx * 9 : (f_idx + 1) * 9]
            grid_x, grid_y = POSITIONS[face_name]
            start_x = grid_x * 115 + 45
            start_y = grid_y * 115 + 25
            
            count = 0
            for row in range(3):
                for col in range(3):
                    char = face_chars[count]
                    color = COLOR.get(char, "#64748b")
                    x1 = start_x + col * 34
                    y1 = start_y + row * 34
                    x2 = x1 + 31
                    y2 = y1 + 31
                    self.canvas.create_rectangle(
                        x1, y1, x2, y2, fill=color, outline="#090d16", width=2
                    )
                    count += 1

    def front(self, f_idx):
        start = f_idx * 9
        f = self.state[start : start + 9]
        self.state[start : start + 9] = [f[6], f[3], f[0], f[7], f[4], f[1], f[8], f[5], f[2]]

    def block_front(self, f_idx):
        start = f_idx * 9
        f = self.state[start : start + 9]
        self.state[start : start + 9] = [f[2], f[5], f[8], f[1], f[4], f[7], f[0], f[3], f[6]]
    def del_move(self, move):
        if move == "U":
            self.front(0)
            tmp = self.state[2*9 : 2*9+3]
            self.state[2*9 : 2*9+3] = self.state[3*9 : 3*9+3]
            self.state[3*9 : 3*9+3] = self.state[4*9 : 4*9+3]
            self.state[4*9 : 4*9+3] = self.state[1*9 : 1*9+3]
            self.state[1*9 : 1*9+3] = tmp
        elif move == "U'":
            self.block_front(0)
            tmp = self.state[2*9 : 2*9+3]
            self.state[2*9 : 2*9+3] = self.state[1*9 : 1*9+3]
            self.state[1*9 : 1*9+3] = self.state[4*9 : 4*9+3]
            self.state[4*9 : 4*9+3] = self.state[3*9 : 3*9+3]
            self.state[3*9 : 3*9+3] = tmp
        elif move == "R":
            self.front(3)
            w_col = [self.state[0*9+2], self.state[0*9+5], self.state[0*9+8]]
            g_col = [self.state[2*9+2], self.state[2*9+5], self.state[2*9+8]]
            y_col = [self.state[5*9+2], self.state[5*9+5], self.state[5*9+8]]
            b_col = [self.state[4*9+0], self.state[4*9+3], self.state[4*9+6]]
            self.state[0*9+2], self.state[0*9+5], self.state[0*9+8] = g_col
            self.state[2*9+2], self.state[2*9+5], self.state[2*9+8] = y_col
            self.state[5*9+2], self.state[5*9+5], self.state[5*9+8] = b_col[::-1]
            self.state[4*9+0], self.state[4*9+3], self.state[4*9+6] = w_col[::-1]
        elif move == "R'":
            self.block_front(3)
            w_col = [self.state[0*9+2], self.state[0*9+5], self.state[0*9+8]]
            g_col = [self.state[2*9+2], self.state[2*9+5], self.state[2*9+8]]
            y_col = [self.state[5*9+2], self.state[5*9+5], self.state[5*9+8]]
            b_col = [self.state[4*9+0], self.state[4*9+3], self.state[4*9+6]]
            self.state[0*9+2], self.state[0*9+5], self.state[0*9+8] = b_col[::-1]
            self.state[4*9+0], self.state[4*9+3], self.state[4*9+6] = y_col[::-1]
            self.state[5*9+2], self.state[5*9+5], self.state[5*9+8] = g_col
            self.state[2*9+2], self.state[2*9+5], self.state[2*9+8] = w_col

    def startt(self):
        self.btn_action.config(state="disabled")
        animation_thread = threading.Thread(target=self.run_ani)
        animation_thread.start()

    def run_ani(self):
        mix_moves = ["R", "U", "R'", "U'"]
        for move in mix_moves:
            self.del_move(move)
            self.cube()
            self.canvas.create_text(275, 385, text=f"РАЗБОР КУБИКА. Ход: {move}", fill="#ef4444", font=("Arial", 12, "bold"))
            time.sleep(1.2)

        self.canvas.create_text(275, 405, text=f"Начинаем обратную сборку...", fill="#f59e0b", font=("Arial", 11, "italic"))
        time.sleep(1.5)

        solve_moves = ["U", "R", "U'", "R'"]
        for move in solve_moves:
            self.del_move(move)
            self.cube()
            self.canvas.create_text(275, 385, text=f"ОБРАТНАЯ СБОРКА. Ход: {move}", fill="#10b981", font=("Arial", 12, "bold"))
            time.sleep(1.2)

        self.canvas.delete("all")
        self.cube()
        self.canvas.create_text(275, 390, text="КУБИК СОБРАН!", fill="white", font=("Arial", 14, "bold"))

if __name__ == "__main__":
    window = tk.Tk()
    app = Rubiks2DCanvas(window)
    window.mainloop()
