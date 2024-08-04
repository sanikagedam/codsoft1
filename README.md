# codsoft1
#TO-DO LIST
import tkinter as tk
from tkinter import messagebox
import os
import json

TODO_FILE = 'todo.json'

def load_todos():
    if not os.path.exists(TODO_FILE):
        return []
    with open(TODO_FILE, 'r') as file:
        return json.load(file)

def save_todos(todos):
    with open(TODO_FILE, 'w') as file:
        json.dump(todos, file, indent=4)

def add_task():
    task = task_entry.get()
    if task:
        todos.append({"task": task, "done": False})
        save_todos(todos)
        task_entry.delete(0, tk.END)
        refresh_listbox()
    else:
        messagebox.showwarning("Input Error", "Please enter a task.")

def complete_task():
    try:
        selected_task_index = task_listbox.curselection()[0]
        todos[selected_task_index]['done'] = True
        save_todos(todos)
        refresh_listbox()
    except IndexError:
        messagebox.showwarning("Selection Error", "Please select a task.")

def delete_task():
    try:
        selected_task_index = task_listbox.curselection()[0]
        del todos[selected_task_index]
        save_todos(todos)
        refresh_listbox()
    except IndexError:
        messagebox.showwarning("Selection Error", "Please select a task.")

def refresh_listbox():
    task_listbox.delete(0, tk.END)
    for todo in todos:
        status = '✓' if todo['done'] else '✗'
        task_listbox.insert(tk.END, f'[{status}] {todo["task"]}')

# Load tasks from file
todos = load_todos()

# GUI Setup
root = tk.Tk()
root.title("To-Do List")

task_entry = tk.Entry(root, width=50)
task_entry.pack(pady=10)
