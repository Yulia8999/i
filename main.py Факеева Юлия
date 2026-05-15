import tkinter as tk
from tkinter import messagebox
import json
import os
from datetime import datetime

class TrainingPlanner:
    def __init__(self, root):
        self.root = root
        self.root.title("Training Planner")

        # Поля ввода
        self.date_label = tk.Label(root, text="Дата (YYYY-MM-DD):")
        self.date_label.pack()
        self.date_entry = tk.Entry(root)
        self.date_entry.pack()

        self.type_label = tk.Label(root, text="Тип тренировки:")
        self.type_label.pack()
        self.type_entry = tk.Entry(root)
        self.type_entry.pack()

        self.duration_label = tk.Label(root, text="Длительность (мин):")
        self.duration_label.pack()
        self.duration_entry = tk.Entry(root)
        self.duration_entry.pack()

        # Кнопка добавления тренировки
        self.add_button = tk.Button(root, text="Добавить тренировку", command=self.add_training)
        self.add_button.pack()

        # Таблица для отображения тренировок
        self.training_list = tk.Listbox(root)
        self.training_list.pack()

        # Фильтры
        self.filter_date_label = tk.Label(root, text="Фильтр по дате (YYYY-MM-DD):")
        self.filter_date_label.pack()
        self.filter_date_entry = tk.Entry(root)
        self.filter_date_entry.pack()

        self.filter_button = tk.Button(root, text="Фильтровать", command=self.filter_trainings)
        self.filter_button.pack()

    def add_training(self):
        date = self.date_entry.get()
        training_type = self.type_entry.get()
        duration = self.duration_entry.get()

        if not self.validate_input(date, duration):
            return

        training_record = {
            "date": date,
            "type": training_type,
            "duration": int(duration)
        }

        self.training_list.insert(tk.END, f"{date} - {training_type} - {duration} мин")
        self.save_to_json(training_record)

    def validate_input(self, date, duration):
        try:
            datetime.strptime(date, '%Y-%m-%d')
            if int(duration) <= 0:
                raise ValueError("Длительность должна быть положительным числом.")
            return True
        except ValueError as e:
            messagebox.showerror("Ошибка ввода", str(e))
            return False

    def save_to_json(self, record):
        if not os.path.exists('trainings.json'):
            with open('trainings.json', 'w') as f:
                json.dump([], f)

        with open('trainings.json', 'r+') as f:
            data = json.load(f)
            data.append(record)
            f.seek(0)
            json.dump(data, f)

    def filter_trainings(self):
        filter_date = self.filter_date_entry.get()
        filtered_records = []

        with open('trainings.json', 'r') as f:
            data = json.load(f)
        
        for record in data:
            if record['date'] == filter_date:
                filtered_records.append(f"{record['date']} - {record['type']} - {record['duration']} мин")

        self.training_list.delete(0, tk.END)
        for record in filtered_records:
            self.training_list.insert(tk.END, record)

if __name__ == "__main__":
    root = tk.Tk()
    app = TrainingPlanner(root)
    root.mainloop()
