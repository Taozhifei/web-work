<template>
  <div class="todo-app">
    <div class="container">
      <h1 class="title">待办事项</h1>
      
      <div class="todo-list">
        <div 
          v-for="todo in todos" 
          :key="todo.id" 
          class="todo-item"
        >
          <div class="todo-content">
            <span class="todo-text">{{ todo.text }}</span>
          </div>
          <button @click="deleteTodo(todo.id)" class="delete-btn">删除</button>
        </div>
      </div>
      
      <div class="input-section">
        <input 
          type="text" 
          v-model="newTodoText" 
          placeholder="请输入待办事项..." 
          class="todo-input"
          @keyup.enter="addTodo"
        >
        <button @click="addTodo" class="add-btn">添加</button>
      </div>
      
      <button 
        v-if="todos.length > 0" 
        @click="clearAll" 
        class="clear-btn"
      >
        清空所有
      </button>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
  name: 'TodoApp',
  setup() {
    const newTodoText = ref('')
    const todos = ref([])
    
    const loadTodos = () => {
      const savedTodos = localStorage.getItem('simple-todos')
      if (savedTodos) {
        todos.value = JSON.parse(savedTodos)
      }
    }
    
    const saveTodos = () => {
      localStorage.setItem('simple-todos', JSON.stringify(todos.value))
    }
    

    const addTodo = () => {
      if (newTodoText.value.trim() === '') {
        alert('请输入待办事项内容！')
        return
      }
      
      todos.value.push({
        id: Date.now(),
        text: newTodoText.value.trim()
      })
      
      newTodoText.value = ''
      saveTodos()
    }
    
    const deleteTodo = (id) => {
      todos.value = todos.value.filter(todo => todo.id !== id)
      saveTodos()
    }
    
    const clearAll = () => {
      if (confirm('确定要清空所有待办事项吗？')) {
        todos.value = []
        saveTodos()
      }
    }
    
    onMounted(() => {
      loadTodos()
    })
    
    return {
      newTodoText,
      todos,
      addTodo,
      deleteTodo,
      clearAll
    }
  }
}
</script>

<style scoped>
.todo-app {
  width: 100%;
  max-width: 400px;
}

.container {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  padding: 20px;
}

.title {
  text-align: center;
  color: #333;
  margin-bottom: 20px;
  font-size: 24px;
  font-weight: normal;
}

.todo-list {
  margin-bottom: 20px;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px;
  border-bottom: 1px solid #eee;
  background: #fafafa;
  margin-bottom: 8px;
  border-radius: 4px;
}

.todo-item:last-child {
  margin-bottom: 0;
}

.todo-content {
  flex: 1;
}

.todo-text {
  color: #333;
  font-size: 16px;
}

.delete-btn {
  background: #ff4757;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.delete-btn:hover {
  background: #ff3742;
}

.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.todo-input {
  flex: 1;
  padding: 10px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.todo-input:focus {
  border-color: #3498db;
  outline: none;
}

.add-btn {
  background: #3498db;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.add-btn:hover {
  background: #2980b9;
}

.clear-btn {
  width: 100%;
  background: #95a5a6;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.clear-btn:hover {
  background: #7f8c8d;
}
</style>