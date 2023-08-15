import tkinter as tk
from tkinter import messagebox

class TodoList:
    def _init_(self, root):
        self.root = root
        self.root.title("To-Do List")
                
        self.tasks = []
        self.task_var = tk.StringVar()
        
        self.create_widgets()
    
    def create_widgets(self):
        # Task List
        self.task_listbox = tk.Listbox(self.root, width=60, height=15)
        self.task_listbox.grid(row=0, column=0, columnspan=4, padx=10, pady=10)
        self.task_listbox.bind('<<ListboxSelect>>', self.select_task)
        
        # Add Task Entry and Button
        task_entry = tk.Entry(self.root, textvariable=self.task_var, width=60)
        task_entry.grid(row=1, column=0, padx=10, pady=5, columnspan=3)
        
        add_button = tk.Button(self.root, text="Add Task", command=self.add_task)
        add_button.grid(row=1, column=3, padx=10, pady=5)
        
        # Edit, Delete, and Mark Completed Buttons
        edit_button = tk.Button(self.root, text="Edit", command=self.edit_task)
        edit_button.grid(row=2, column=0, padx=10, pady=5)
        
        delete_button = tk.Button(self.root, text="Delete", command=self.delete_task)
        delete_button.grid(row=2, column=1, padx=10, pady=5)
        
        mark_completed_button = tk.Button(self.root, text="Mark Completed", command=self.mark_completed)
        mark_completed_button.grid(row=2, column=2, padx=10, pady=5)
        
        # Clear All Button
        clear_all_button = tk.Button(self.root, text="Clear All", command=self.clear_all)
        clear_all_button.grid(row=2, column=3, padx=10, pady=5)
    
    def add_task(self):
        task = self.task_var.get()
        if task:
            self.tasks.append({"task": task, "completed": False})
            self.task_listbox.insert(tk.END, task)
            self.task_var.set("")
    
    def edit_task(self):
        selected_index = self.task_listbox.curselection()
        if selected_index:
            selected_index = selected_index[0]
            task = self.task_var.get()
            if task:
                self.tasks[selected_index]["task"] = task
                self.task_listbox.delete(selected_index)
                self.task_listbox.insert(selected_index, task)
                self.task_var.set("")
    
    def delete_task(self):
        selected_index = self.task_listbox.curselection()
        if selected_index:
            selected_index = selected_index[0]
            task = self.tasks[selected_index]["task"]
            response = messagebox.askyesno("Delete Task", f"Are you sure you want to delete the task:\n'{task}'?")
            if response:
                self.task_listbox.delete(selected_index)
                del self.tasks[selected_index]
    
    def select_task(self, event):
        selected_index = self.task_listbox.curselection()
        if selected_index:
            selected_index = selected_index[0]
            task = self.tasks[selected_index]["task"]
            self.task_var.set(task)
    
    def mark_completed(self):
        selected_index = self.task_listbox.curselection()
        if selected_index:
            selected_index = selected_index[0]
            task = self.tasks[selected_index]
            task["completed"] = not task["completed"]
            self.update_listbox()
    
    def clear_all(self):
        response = messagebox.askyesno("Clear All Tasks", "Are you sure you want to clear all tasks?")
        if response:
            self.tasks = []
            self.update_listbox()
    
    def update_listbox(self):
        self.task_listbox.delete(0, tk.END)
        for task in self.tasks:
            task_str = "[X] " + task["task"] if task["completed"] else "[ ] " + task["task"]
            self.task_listbox.insert(tk.END, task_str)

if __name__ == "_main_":
    root = tk.Tk()
    app = TodoList(root)
    root.mainloop()
