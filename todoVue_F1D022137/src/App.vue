<template>
  <div class="wrapper">
    <div class="card">
      <h1 class="title">Daftar Tugas</h1>

      <form @submit.prevent="addTask" class="form-box">
        <input
          v-model="newTask"
          type="text"
          placeholder="Tambah tugas baru..."
          class="input-task"
        />
        <button type="submit" class="btn btn-add">Tambah</button>
      </form>

      <p v-if="tasks.length === 0" class="empty">Belum ada tugas</p>

      <transition-group name="fade" tag="ul" class="task-list">
        <li
          v-for="(task, index) in tasks"
          :key="task.id"
          class="task-item"
        >
          <div v-if="task.editing" class="edit-box">
            <input
              v-model="task.text"
              type="text"
              class="edit-input"
            />
            <button class="btn btn-save" @click="saveTask(index)">Simpan</button>
          </div>

          <div v-else class="task-view">
            <span class="task-text">{{ task.text }}</span>
            <div class="btn-group">
              <button class="btn btn-edit" @click="editTask(index)">Edit</button>
              <button class="btn btn-delete" @click="deleteTask(index)">Hapus</button>
            </div>
          </div>
        </li>
      </transition-group>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const tasks = ref([]);
const newTask = ref("");

function addTask() {
  const teks = newTask.value.trim();
  if (teks !== "") {
    tasks.value.push({
      id: Date.now(),
      text: teks,
      editing: false,
    });
    newTask.value = "";
  }
}

function deleteTask(index) {
  tasks.value.splice(index, 1);
}

function editTask(index) {
  tasks.value[index].editing = true;
}

function saveTask(index) {
  tasks.value[index].editing = false;
}
</script>

<style>
body {
  margin: 0;
  font-family: "Inter", Arial, sans-serif;
  background: linear-gradient(135deg, #e0eaff, #f5f9ff);
}

.wrapper {
  display: flex;
  justify-content: center;
  padding: 40px 20px;
}

.card {
  width: 100%;
  max-width: 480px;
  background: #ffffff;
  padding: 24px;
  border-radius: 18px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
  animation: slideDown 0.4s ease-out;
}

.title {
  margin: 0 0 16px 0;
  font-size: 2rem;
  font-weight: 700;
  color: #253858;
  text-align: center;
}

.form-box {
  display: flex;
  gap: 10px;
  margin-bottom: 18px;
}

.input-task {
  flex: 1;
  padding: 12px 14px;
  font-size: 1rem;
  border-radius: 10px;
  border: 1px solid #d4ddee;
  outline: none;
}
.input-task:focus {
  border-color: #4f8dfb;
  box-shadow: 0 0 0 3px rgba(79, 141, 251, 0.2);
}

.btn {
  padding: 10px 14px;
  border-radius: 10px;
  border: none;
  cursor: pointer;
  font-weight: 600;
  transition: 0.2s ease;
}

.btn-add {
  background: #4f8dfb;
  color: white;
}
.btn-add:hover {
  background: #3677e8;
  transform: translateY(-2px);
}

.btn-delete {
  background: #ffe0e0;
  color: #d83a3a;
  border: 1px solid #ffcdcd;
}
.btn-delete:hover {
  background: #ffcccc;
  transform: translateY(-2px);
}

.btn-edit {
  background: #fff5d1;
  color: #b48a00;
  border: 1px solid #ffe9a3;
}
.btn-edit:hover {
  background: #ffe9a3;
  transform: translateY(-2px);
}

.btn-save {
  background: #34d399;
  color: white;
}
.btn-save:hover {
  background: #22c193;
  transform: translateY(-2px);
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.task-item {
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 12px 14px;
  margin-bottom: 10px;
  border-radius: 12px;
  background: #f7f9ff;
  border: 1px solid #e2e8f6;
  animation: fadeIn 0.4s ease;
}

.task-view {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task-text {
  font-size: 1rem;
  color: #1f2937;
}

.btn-group {
  display: flex;
  gap: 6px;
}

.edit-box {
  display: flex;
  gap: 10px;
}
.edit-input {
  flex: 1;
  padding: 10px 12px;
  border-radius: 10px;
  border: 1px solid #c9d4ee;
}

.empty {
  text-align: center;
  color: #8a94a6;
  font-style: italic;
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}
</style>
