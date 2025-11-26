# Python-Task-2

# 📝 Console To-Do List App

## Objective

- Implement a simple to-do list manager.

## Features

- Add new tasks
- View all tasks with completion status
- Mark tasks as done or undone
- Remove tasks by number
- Clear all tasks
- Persistent storage using `tasks.txt`

## How It Works

- Menu shows options: View, Add, Remove, Toggle done/undone and Quit
- Choose tasks by number.
- Tasks are listed and saved in tasks.txt.
- When app starts, previously saved tasks are loaded.

## Program Code
```
import os

DATA_FILE = "tasks.txt"

def load_tasks():
    tasks = []
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, "r", encoding="utf-8") as f:
            for line in f:
                line = line.rstrip("\n")
                if not line:
                    continue
                # Format: status|task text
                status, text = line.split("|", 1)
                tasks.append({"text": text, "done": (status == "1")})
    return tasks

def save_tasks(tasks):
    with open(DATA_FILE, "w", encoding="utf-8") as f:
        for t in tasks:
            status = "1" if t["done"] else "0"
            f.write(f"{status}|{t['text']}\n")

def show_tasks(tasks):
    if not tasks:
        print("\nNo tasks yet. Add one!\n")
        return
    print("\nYour tasks:")
    for i, t in enumerate(tasks, start=1):
        status = "✓" if t["done"] else " "
        print(f"  {i}. [{status}] {t['text']}")
    print()

def add_task(tasks):
    text = input("Enter task: ").strip()
    if text:
        tasks.append({"text": text, "done": False})
        save_tasks(tasks)
        print("Task added.\n")
    else:
        print("Empty task ignored.\n")

def remove_task(tasks):
    show_tasks(tasks)
    if not tasks:
        return
    try:
        idx = int(input("Enter task number to remove: "))
        if 1 <= idx <= len(tasks):
            removed = tasks.pop(idx - 1)
            save_tasks(tasks)
            print(f"Removed: {removed['text']}\n")
        else:
            print("Invalid number.\n")
    except ValueError:
        print("Please enter a valid integer.\n")

def toggle_done(tasks):
    show_tasks(tasks)
    if not tasks:
        return
    try:
        idx = int(input("Enter task number to toggle done: "))
        if 1 <= idx <= len(tasks):
            tasks[idx - 1]["done"] = not tasks[idx - 1]["done"]
            save_tasks(tasks)
            state = "completed" if tasks[idx - 1]["done"] else "active"
            print(f"Task marked {state}.\n")
        else:
            print("Invalid number.\n")
    except ValueError:
        print("Please enter a valid integer.\n")

def clear_all(tasks):
    confirm = input("Clear ALL tasks? Type YES to confirm: ").strip()
    if confirm == "YES":
        tasks.clear()
        save_tasks(tasks)
        print("All tasks cleared.\n")
    else:
        print("Cancelled.\n")

def menu():
    print("To-Do List (persistent)")
    print("------------------------")
    print("1) View tasks")
    print("2) Add task")
    print("3) Remove task")
    print("4) Toggle done/undone")
    print("5) Clear all")
    print("6) Quit")
    choice = input("Choose an option (1-6): ").strip()
    return choice

def main():
    tasks = load_tasks()
    while True:
        choice = menu()
        if choice == "1":
            show_tasks(tasks)
        elif choice == "2":
            add_task(tasks)
        elif choice == "3":
            remove_task(tasks)
        elif choice == "4":
            toggle_done(tasks)
        elif choice == "5":
            clear_all(tasks)
        elif choice == "6":
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Try 1-6.\n")

if __name__ == "__main__":
    main()
```

## Output Screenshot
<img width="692" height="576" alt="image" src="https://github.com/user-attachments/assets/95e5431d-f524-4fe2-a0d5-c55caf13c99e" />
<img width="367" height="467" alt="image" src="https://github.com/user-attachments/assets/69f53a76-e101-4f53-a48a-6af87183a7da" />


