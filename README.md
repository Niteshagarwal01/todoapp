# Todo App — Project Guide (Small Build #5)

A responsive and feature-rich task management application with priority levels, due dates, categories, and local storage persistence. Built with vanilla JavaScript for efficient task organization and productivity.

## 🚀 Features

  * **Task Management:** Create, edit, delete, and mark tasks as complete with intuitive controls.
  * **Priority Levels:** Assign High, Medium, or Low priority with color-coded visual indicators.
  * **Due Dates:** Set deadlines for tasks with automatic overdue detection and highlighting.
  * **Categories:** Organize tasks by Personal, Work, Shopping, Health, or Other categories.
  * **Smart Filtering:** Filter tasks by All, Active, Completed, High Priority, or Overdue status.
  * **Real-time Search:** Instantly search through tasks as you type.
  * **Flexible Sorting:** Sort tasks by Date Added, Due Date, or Priority with a single click.
  * **Local Storage:** Automatically saves all tasks to browser storage for persistence.
  * **Keyboard Support:** Press Enter to quickly add tasks.
  * **Task Counter:** Live count of active (uncompleted) tasks.

## 🛠️ Tech Stack

  * **Frontend:** HTML5, CSS3
  * **Logic:** Vanilla JavaScript (ES6+) with modern array methods and localStorage API.
  * **Styling:** CSS Custom Properties (CSS Variables) for theming.
  * **Icons:** Font Awesome 6.7.2
  * **Storage:** Browser localStorage (no backend required)

## 📂 File Structure

  * `index.html` - The DOM structure with input controls, filters, task list, and footer actions.
  * `style.css` - Modern styling with priority colors, responsive layout, and smooth transitions.
  * `script.js` - Core application logic for task CRUD operations, filtering, and localStorage management.

## ⚙️ How It Works

The application uses JavaScript to manage a `todos` array that stores task objects with the following structure:

```javascript
{
  id: 1732636800000,           // Unique timestamp ID
  text: "Complete project",     // Task description
  completed: false,             // Completion status
  priority: "high",             // Priority level (high/medium/low)
  dueDate: "2025-11-30",       // Due date (YYYY-MM-DD)
  category: "work",             // Task category
  createdAt: "2025-11-26T..."  // ISO timestamp
}
```

## 📝 Usage Examples

**Adding a Task:**
```javascript
function addTodo(text) {
  if (text.trim() === "") return;

  const todo = {
    id: Date.now(),
    text,
    completed: false,
    priority: prioritySelect.value,
    dueDate: dueDateInput.value,
    category: categorySelect.value,
    createdAt: new Date().toISOString(),
  };

  todos.push(todo);
  saveTodos();
  renderTodos();
  // Reset form inputs
}
```

**Smart Filtering Logic:**
```javascript
function filterTodos(filter) {
  let filtered = todos;
  
  // Apply filter
  switch (filter) {
    case "active":
      filtered = todos.filter((todo) => !todo.completed);
      break;
    case "completed":
      filtered = todos.filter((todo) => todo.completed);
      break;
    case "high":
      filtered = todos.filter((todo) => 
        todo.priority === "high" && !todo.completed
      );
      break;
    case "overdue":
      filtered = todos.filter((todo) => {
        if (!todo.dueDate || todo.completed) return false;
        return new Date(todo.dueDate) < new Date();
      });
      break;
  }

  // Apply search query
  if (searchQuery) {
    filtered = filtered.filter((todo) =>
      todo.text.toLowerCase().includes(searchQuery)
    );
  }

  // Apply sorting
  return sortTodos(filtered);
}
```

**LocalStorage Persistence:**
```javascript
function saveTodos() {
  localStorage.setItem("todos", JSON.stringify(todos));
  updateItemsCount();
  checkEmptyState();
}

function loadTodos() {
  const storedTodos = localStorage.getItem("todos");
  if (storedTodos) todos = JSON.parse(storedTodos);
  renderTodos();
}
```

**Dynamic Sorting:**
```javascript
function sortTodos(todos) {
  const sorted = [...todos];
  
  switch (sortOrder) {
    case "dueDate":
      sorted.sort((a, b) => {
        if (!a.dueDate && !b.dueDate) return 0;
        if (!a.dueDate) return 1;
        if (!b.dueDate) return -1;
        return new Date(a.dueDate) - new Date(b.dueDate);
      });
      break;
    case "priority":
      const priorityOrder = { high: 0, medium: 1, low: 2 };
      sorted.sort((a, b) => 
        priorityOrder[a.priority] - priorityOrder[b.priority]
      );
      break;
    case "dateAdded":
    default:
      sorted.sort((a, b) => b.id - a.id); // Newest first
  }
  
  return sorted;
}
```

## 💻 Run Locally

1.  Clone the repository or download the files:
    ```bash
    git clone https://github.com/Niteshagarwal01/todoapp.git
    cd todoapp
    ```
2.  Open `index.html` in your browser.
3.  **No Installation Required:** Pure vanilla JavaScript - works immediately in any modern browser.
4.  **Data Persistence:** Your tasks are automatically saved to browser localStorage and will persist across sessions.

## 🔧 Customization

  * **Color Scheme:** Modify CSS custom properties in `:root` to change the theme:
    ```css
    :root {
      --primary: #7749f8;        /* Main accent color */
      --priority-high: #ff6b6b;  /* High priority color */
      --priority-medium: #ffd43b; /* Medium priority color */
      --priority-low: #51cf66;   /* Low priority color */
    }
    ```
  
  * **Categories:** Add or modify categories in `index.html`:
    ```html
    <select id="category-select" title="Category">
      <option value="personal">Personal</option>
      <option value="work">Work</option>
      <option value="fitness">Fitness</option> <!-- Custom category -->
    </select>
    ```

  * **Default Values:** Change default priority or category in `addTodo()`:
    ```javascript
    prioritySelect.value = "low";  // Default to low priority
    categorySelect.value = "work"; // Default to work category
    ```

  * **Date Display:** Customize date formatting in `renderTodos()`:
    ```javascript
    const formattedDate = new Date(todo.dueDate).toLocaleDateString("en-US", {
      weekday: "short",  // Add day of week
      month: "short",
      day: "numeric",
      year: "numeric",   // Add year
    });
    ```

## 🎨 UI/UX Features

  * **Visual Priority Indicators:** Color-coded left border on each task (red, yellow, green)
  * **Overdue Highlighting:** Tasks past their due date get a light red background
  * **Empty State:** Friendly message when no tasks match current filter
  * **Hover Effects:** Smooth transitions reveal edit and delete buttons
  * **Responsive Design:** Works seamlessly on mobile, tablet, and desktop
  * **Task Metadata:** Each task displays category badge, due date, and priority flag

## ⚡ Key Functions

| Function | Purpose |
|----------|---------|
| `addTodo(text)` | Creates new task with all properties and saves to storage |
| `toggleTodo(id)` | Marks task as complete/incomplete |
| `editTodo(id)` | Loads task into input form for editing |
| `deleteTodo(id)` | Removes task from array and storage |
| `filterTodos(filter)` | Applies active filter, search, and sort to task list |
| `renderTodos()` | Dynamically generates and displays task elements in DOM |
| `saveTodos()` | Persists todos array to localStorage |
| `loadTodos()` | Retrieves saved tasks on page load |

## 📊 Browser Storage

Tasks are stored in localStorage under the key `"todos"` as a JSON string. You can inspect saved data in browser DevTools:

```javascript
// View all saved todos
console.log(localStorage.getItem("todos"));

// Clear all todos (destructive!)
localStorage.removeItem("todos");
```

## 🔍 Search & Filter Logic

The app combines multiple filtering mechanisms:

1. **Primary Filter:** All, Active, Completed, High Priority, Overdue
2. **Search Query:** Real-time text matching (case-insensitive)
3. **Sort Order:** Date Added, Due Date, or Priority

All three work together seamlessly to help you find exactly what you need.

---

*Contributions welcome — feel free to open issues or create PRs to add features like task notes, recurring tasks, or dark mode support.*
