# Task-Manager-CLI-Application
import json

class Task:
    def __init__(self, task_id, title, completed=False):
        self.id = task_id
        self.title = title
        self.completed = completed

    def __repr__(self):
        status = "Completed" if self.completed else "Pending"
        return f"Task({self.id}, {self.title}, {status})"


tasks = []

def add_task(title):
    # Check for duplicate titles
    if any(task.title == title for task in tasks):
        print(f"A task with the title '{title}' already exists.")
        return
    
    task_id = len(tasks) + 1
    task = Task(task_id, title)
    tasks.append(task)
    print(f"Task '{title}' added successfully.")

def view_tasks():
    if not tasks:
        print("No tasks available.")
    else:
        for task in tasks:
            print(task)

def delete_task(task_id):
    global tasks
    tasks = [task for task in tasks if task.id != task_id]

def mark_task_complete(task_id):
    for task in tasks:
        if task.id == task_id:
            task.completed = True
            break

def save_tasks(filename='tasks.json'):
    with open(filename, 'w') as file:
        json.dump([task.__dict__ for task in tasks], file)

def load_tasks(filename='tasks.json'):
    try:
        with open(filename, 'r') as file:
            task_list = json.load(file)
            for task_dict in task_list:
                task = Task(task_dict['id'], task_dict['title'], task_dict['completed'])
                tasks.append(task)
    except FileNotFoundError:
        pass

def show_menu():
    print("\n1. Add Task")
    print("2. View Tasks")
    print("3. Delete Task")
    print("4. Mark Task as Complete")
    print("5. Exit")

def main():
    load_tasks()  # Load tasks on start
    while True:
        show_menu()
        choice = input("\nEnter your choice: ")
        
        if choice == '1':
            title = input("Enter task title: ")
            add_task(title)
        elif choice == '2':
            view_tasks()
        elif choice == '3':
            task_id = int(input("Enter task ID to delete: "))
            delete_task(task_id)
        elif choice == '4':
            task_id = int(input("Enter task ID to mark as complete: "))
            mark_task_complete(task_id)
        elif choice == '5':
            save_tasks()  # Save tasks before exiting
            print("Exiting...")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
