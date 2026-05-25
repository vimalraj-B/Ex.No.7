# Exno.7-Develop a prompt-based application tailored to their personal needs, fostering creativity and practical problem-solving skills while leveraging the capabilities of large language models.

# Date: 25.05.26
# Register no. : 212224230304
# Aim: To develop a prompt-based application using ChatGPT - To demonstrate how to create a prompt-based application to organize daily tasks, showing the progression from simple to more advanced prompt designs and their corresponding outputs.

#AI Tools Required: 


# Explanation: 
Prompt:
"Design a personal productivity assistant that can help manage daily tasks, schedule reminders, suggest wellness tips, and answer general queries. The assistant should interact using natural language and be adaptable to the user’s changing preferences over time."
Procedure:
1. Define the core requirements of a personal productivity assistant.
2. Identify and construct appropriate prompts for each task using an LLM (e.g., ChatGPT).
3. Simulate natural user interaction through a simple interface or command-line system.
4. Collect feedback or inputs from users and adapt responses accordingly.
5. (Optional) Integrate basic memory to simulate preference adaptation.


Here is the breakdown of how to use and operate this altered Productivity Assistant application:

### 1. Running the Application

* Save the code provided above into a file named `productivity_app.py`.
* Open your terminal or command prompt, navigate to the folder containing the file, and run:
```bash
python productivity_app.py

```



---

### 2. Step-by-Step Operating Procedure

#### ➕ Adding a Task

1. Locate the **Create New Task** panel on the top left.
2. Click inside the **Task Name** field and type out your objective (e.g., *"Review project proposal"*).
3. Click the **Priority** dropdown box and select **High**, **Medium**, or **Low**.
4. Click the **➕ Add Task** button.
* *Result:* The task will instantly populate the main interactive grid table on the right side, and a confirmation message will print to the Activity Log.



#### ✓ Completing a Task

1. Look at the live spreadsheet-style grid (**Task Title** table) on the right side.
2. **Left-click** directly on the row of the task you want to complete to highlight it.
3. Move your cursor to the left panel and click the orange **✓ Mark Selected Complete** button.
4. *Result:* The task's status flag in the grid will visually shift from `🔴 Pending` to `🟢 Completed`.

#### 🧠 Consulting the Productivity Assistant

1. Locate the **Productivity Assistant** text entry field on the bottom left.
2. Type in a keyword related to your current work hurdle, such as `stress`, `study`, or `procrastination`.
3. Click the **🧠 Ask Assistant** button.
4. *Result:* The custom response or psychological strategy will be instantly printed directly into the **Assistant Activity Log** terminal at the bottom right.

#### 📊 Running Metrics & Break Resets

* **Daily Summary:** Click the **📊 Run Daily Summary** button on top of the right panel to print a real-time data assessment—including your cumulative task volume and percentage calculation of finished work—into the log.
* **Wellness Break:** Click the **🌱 Wellness Break** button whenever you feel burnt out to fetch a sudden, healthy reminder sequence.
* **Clear Log:** Click the **🗑 Clear Assistant Log** button to completely wipe clean the text feed in the bottom console window without disturbing your live task list database up top.
# Code :

```
import tkinter as tk
from tkinter import messagebox, ttk

tasks = []

# --- Business Logic Functions ---

def update_treeview():
    """Refreshes the modern task list display."""
    # Clear current entries
    for item in task_tree.get_children():
        task_tree.delete(item)
        
    # Repopulate from tasks list
    for i, task in enumerate(tasks):
        status = "🟢 Completed" if task["completed"] else "🔴 Pending"
        # Using i+1 as a visual ID for the user
        task_tree.insert("", tk.END, iid=str(i), values=(f"{i+1}", task['task'], task['priority'], status))

def add_task():
    task = task_entry.get().strip()
    priority = priority_combobox.get()

    if not task or not priority:
        messagebox.showwarning("Warning", "Please enter a task and select a priority.")
        return

    task_data = {
        "task": task,
        "priority": priority,
        "completed": False
    }

    tasks.append(task_data)
    log_output(f"✔ Task Added: \"{task}\" [{priority} Priority]")
    update_treeview()
    
    # Clear fields
    task_entry.delete(0, tk.END)
    priority_combobox.set('')

def complete_task():
    """Completes a task based on selection in the Treeview list."""
    selected_item = task_tree.selection()
    
    if not selected_item:
        messagebox.showwarning("Warning", "Please select a task from the list below to complete.")
        return
        
    # The iid corresponds directly to the list index string
    task_index = int(selected_item[0])
    
    if tasks[task_index]["completed"]:
        log_output(f"ℹ \"{tasks[task_index]['task']}\" is already marked completed.")
    else:
        tasks[task_index]["completed"] = True
        log_output(f"✅ \"{tasks[task_index]['task']}\" marked as completed!")
        update_treeview()

def wellness_tip():
    log_output("\n💡 Wellness Tip: Drink a glass of water, stretch your back, and take a 5-minute break.")

def assistant_query():
    query = query_entry.get().lower().strip()
    if not query:
        messagebox.showwarning("Warning", "Please enter a question for the assistant.")
        return

    if "study" in query or "work" in query:
        response = "Try using the Pomodoro technique: 25 minutes of focus followed by a 5-minute break."
    elif "stress" in query or "tired" in query:
        response = "Take a step back. Take three deep breaths or practice a 2-minute mindfulness reset."
    elif "procrastinat" in query:
        response = "Break your biggest task down into a ridiculously tiny, 2-minute step and start there."
    else:
        response = "I'm here to boost your productivity! Try asking me about 'study balance', managing 'stress', or tackling 'procrastination'."

    log_output(f"\n🤖 Assistant: {response}")
    query_entry.delete(0, tk.END)

def daily_summary():
    total = len(tasks)
    completed = sum(1 for t in tasks if t["completed"])
    pending = total - completed

    log_output("\n=================== DAILY SUMMARY ===================")
    log_output(f"📊 Total Tasks Managed: {total}")
    log_output(f"✅ Completed Checklist: {completed}")
    log_output(f"⏳ Pending Checklist  : {pending}")
    if total > 0:
        completion_rate = (completed / total) * 100
        log_output(f"📈 Progress Rate      : {completion_rate:.1f}%")

def log_output(text):
    """Helper to insert text and auto-scroll to the bottom"""
    output_box.insert(tk.END, text + "\n")
    output_box.see(tk.END)

def clear_output():
    output_box.delete("1.0", tk.END)


# --- UI Setup ---

root = tk.Tk()
root.title("Personal Productivity Assistant")
root.geometry("850x680")
root.configure(bg="#f4f6f9")

# Custom Styles for clean look
style = ttk.Style()
style.theme_use('clam')
style.configure("TLabelframe", background="#f4f6f9", font=("Arial", 10, "bold"))
style.configure("TLabelframe.Label", background="#f4f6f9", foreground="#333333")
style.configure("Treeview.Heading", font=("Arial", 9, "bold"), background="#e6eaf0")

# Main Title
title_label = tk.Label(
    root, 
    text="🎯 Personal Productivity Assistant", 
    font=("Arial", 18, "bold"), 
    bg="#f4f6f9", 
    fg="#2c3e50"
)
title_label.pack(pady=15)

# Outer grid container for layout organization
main_frame = tk.Frame(root, bg="#f4f6f9")
main_frame.pack(fill=tk.BOTH, expand=True, padx=20)

# --- LEFT COLUMN: Input Panels ---
left_panel = tk.Frame(main_frame, bg="#f4f6f9")
left_panel.grid(row=0, column=0, sticky="n", padx=(0, 15))

# Frame 1: Creation & Add Task
add_frame = ttk.LabelFrame(left_panel, text=" Create New Task ")
add_frame.pack(fill="x", pady=5, ipady=5)

tk.Label(add_frame, text="Task Name:", bg="#f4f6f9").grid(row=0, column=0, padx=5, pady=5, sticky="w")
task_entry = tk.Entry(add_frame, width=30)
task_entry.grid(row=0, column=1, padx=5, pady=5)

tk.Label(add_frame, text="Priority:", bg="#f4f6f9").grid(row=1, column=0, padx=5, pady=5, sticky="w")
priority_combobox = ttk.Combobox(add_frame, values=["High", "Medium", "Low"], width=28, state="readonly")
priority_combobox.grid(row=1, column=1, padx=5, pady=5)

add_btn = tk.Button(add_frame, text="➕ Add Task", command=add_task, bg="#3498db", fg="white", font=("Arial", 9, "bold"), bd=0, padx=10, pady=4)
add_btn.grid(row=2, column=0, columnspan=2, pady=8)

# Frame 2: Operations / Quick Tools
ops_frame = ttk.LabelFrame(left_panel, text=" Task Controls ")
ops_frame.pack(fill="x", pady=10, ipady=5)

complete_btn = tk.Button(ops_frame, text="✓ Mark Selected Complete", command=complete_task, bg="#e67e22", fg="white", font=("Arial", 9, "bold"), bd=0, width=25, pady=6)
complete_btn.pack(pady=5, padx=10)

# Frame 3: Smart Assistant & Extra Insights
assistant_frame = ttk.LabelFrame(left_panel, text=" Productivity Assistant ")
assistant_frame.pack(fill="x", pady=5, ipady=5)

tk.Label(assistant_frame, text="Ask advice (e.g., 'stress', 'study'):", bg="#f4f6f9").pack(anchor="w", padx=10, pady=2)
query_entry = tk.Entry(assistant_frame, width=30)
query_entry.pack(padx=10, pady=2)

ask_btn = tk.Button(assistant_frame, text="🧠 Ask Assistant", command=assistant_query, bg="#9b59b6", fg="white", font=("Arial", 9, "bold"), bd=0, width=22, pady=4)
ask_btn.pack(pady=5)


# --- RIGHT COLUMN: Task List & Assistant Console ---
right_panel = tk.Frame(main_frame, bg="#f4f6f9")
right_panel.grid(row=0, column=1, sticky="nsew")

# Utility buttons panel (Top of Right Panel)
utils_frame = tk.Frame(right_panel, bg="#f4f6f9")
utils_frame.pack(fill="x", pady=(0, 5))

summary_btn = tk.Button(utils_frame, text="📊 Run Daily Summary", command=daily_summary, bg="#34495e", fg="white", font=("Arial", 9, "bold"), bd=0, padx=10, pady=4)
summary_btn.pack(side=tk.LEFT, padx=2)

wellness_btn = tk.Button(utils_frame, text="🌱 Wellness Break", command=wellness_tip, bg="#1abc9c", fg="white", font=("Arial", 9, "bold"), bd=0, padx=10, pady=4)
wellness_btn.pack(side=tk.LEFT, padx=2)

clear_btn = tk.Button(utils_frame, text="🗑 Clear Assistant Log", command=clear_output, bg="#95a5a6", fg="white", font=("Arial", 9, "bold"), bd=0, padx=10, pady=4)
clear_btn.pack(side=tk.RIGHT, padx=2)

# Interactive Task Treeview 
tree_frame = tk.Frame(right_panel)
tree_frame.pack(fill=tk.BOTH, expand=True, pady=(0, 10))

columns = ("id", "task", "priority", "status")
task_tree = ttk.Treeview(tree_frame, columns=columns, show="headings", height=10)

task_tree.heading("id", text="#")
task_tree.heading("task", text="Task Title")
task_tree.heading("priority", text="Priority")
task_tree.heading("status", text="Status")

task_tree.column("id", width=40, anchor="center")
task_tree.column("task", width=220, anchor="w")
task_tree.column("priority", width=90, anchor="center")
task_tree.column("status", width=110, anchor="center")

tree_scroll = ttk.Scrollbar(tree_frame, orient="vertical", command=task_tree.yview)
task_tree.configure(yscrollcommand=tree_scroll.set)

task_tree.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)
tree_scroll.pack(side=tk.RIGHT, fill=tk.Y)

# Console/Log Box Label
tk.Label(right_panel, text="📋 Assistant Activity Log:", font=("Arial", 9, "bold"), bg="#f4f6f9", fg="#555555").pack(anchor="w")

# System Text Log Display (At the bottom right)
text_container = tk.Frame(right_panel)
text_container.pack(fill=tk.BOTH, expand=True)

scrollbar = tk.Scrollbar(text_container)
scrollbar.pack(side=tk.RIGHT, fill=tk.Y)

output_box = tk.Text(text_container, height=10, width=52, font=("Courier New", 10), yscrollcommand=scrollbar.set, bg="#ffffff", fg="#2c3e50", padx=8, pady=8)
output_box.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)
scrollbar.config(command=output_box.yview)

# Weight initialization for flexible layouts
main_frame.columnconfigure(1, weight=1)

root.mainloop()
```

Personal Productivity Assistant Features:
1. Daily Task Manager:
o Accept tasks via natural language (e.g., "Remind me to call mom at 6 PM").
o Organize tasks by priority and deadline.
o Provide daily summaries and pending items.
2. Smart Scheduler:
o Schedule events and set reminders using contextual understanding.
o Notify user of overlapping appointments or free time slots.
3. Wellness Tips Generator:
o Suggest daily wellness advice (hydration, exercise, screen-time breaks).
o Adapt suggestions based on past user preferences and responses.

# Output : 


<img width="1919" height="1079" alt="Screenshot 2026-05-25 214946" src="https://github.com/user-attachments/assets/0d8f7c12-1983-4381-9438-a259690ac46c" />



# Result: 
The lab exercise resulted in the creation of a prototype concept for a personal assistant powered by large language models. Students were able to:
 Understand how to tailor LLM prompts to real-life applications.
 Foster creativity by designing features suited to their personal or academic lives.
 Learn prompt engineering techniques for optimal interaction with AI tools.
 Experience the versatility and utility of generative AI in solving everyday problems.
