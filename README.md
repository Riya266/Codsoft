# Codsoft
task 1
  import json
  class Task:
    def __init__(self, description, completed=False):
        self.description = description
        self.completed = completed

    def __repr__(self):
        status = "✓" if self.completed else "✗"
        return f"[{status}] {self.description}"

    def mark_complete(self):
        self.completed = True

class ToDoList:
    def __init__(self):
        self.tasks = []
        self.load_tasks()

    def add_task(self, description):
        self.tasks.append(Task(description))
        self.save_tasks()

    def view_tasks(self):
        for i, task in enumerate(self.tasks, 1):
            print(f"{i}. {task}")

    def update_task(self, index, description):
        if 0 <= index < len(self.tasks):
            self.tasks[index].description = description
            self.save_tasks()
        else:
            print("Invalid task number.")

    def delete_task(self, index):
        if 0 <= index < len(self.tasks):
            del self.tasks[index]
            self.save_tasks()
        else:
            print("Invalid task number.")

    def mark_task_complete(self, index):
        if 0 <= index < len(self.tasks):
            self.tasks[index].mark_complete()
            self.save_tasks()
        else:
            print("Invalid task number.")

    def save_tasks(self):
        with open('tasks.json', 'w') as file:
            json.dump([task.__dict__ for task in self.tasks], file)

    def load_tasks(self):
        try:
            with open('tasks.json', 'r') as file:
                tasks_data = json.load(file)
                self.tasks = [Task(**data) for data in tasks_data]
        except FileNotFoundError:
            self.tasks = []

def main():
    todo_list = ToDoList()
    while True:
        print("\n1. Add Task\n2. View Tasks\n3. Update Task\n4. Delete Task\n5. Mark Task Complete\n6. Exit")
        choice = input("Choose a number: ")
        if choice == '1':
            description = input("Enter task description: ")
            todo_list.add_task(description)
        elif choice == '2':
            todo_list.view_tasks()
        elif choice == '3':
            index = int(input("Enter task number to update: ")) - 1
            description = input("Enter new description: ")
            todo_list.update_task(index, description)
        elif choice == '4':
            index = int(input("Enter task number to delete: ")) - 1
            todo_list.delete_task(index)
        elif choice == '5':
            index = int(input("Enter task number to mark complete: ")) - 1
            todo_list.mark_task_complete(index)
        elif choice == '6':
            break
        else:
            print("Invalid choice. Try again.")

if __name__ == "__main__":
    main()
