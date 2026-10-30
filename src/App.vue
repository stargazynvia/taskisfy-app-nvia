<script setup>

import { ref } from 'vue'

// states: buat menyimpan data tugas tau tasks
const tasks = ref([])

// states: buat menyimpan inputan dari form
const inputTask = ref('')

// action: buat menambahkan tugas baru ke dalam daftar tasks
const addTask = (title) => {

  // bikin object task baru
  const newTask = {
    id: new Date().getTime(),
    title: title,
    status: false
  }

  tasks.value.push(newTask)

  inputTask.value = ''
}

// action: buat menghapus tugas berdasarkan ID
const deleteTask = (id) => {
  
  // cari dulu task berdasarkan ID
  const task = tasks.value.find(item => item.id == id)

  // jika task tidak ditemukan, kita munculkan alert
  if (!task) {
    alert("Task tidak ditemukan!")
    return
  }

  // minta konfirmasi ke user
  const confirmation = confirm("Apakah anda yakin ingin menghapus?")

  if (confirmation) {
     // hapus task dari state tasks
  tasks.value = tasks.value.filter(item => item.id !== id)
  }
}

</script>

<template>
  <!-- Background Utama bertema Pink Striped Coquette & Brown Retro -->
  <div class="coquette-bg min-h-screen py-10 flex items-center justify-center font-sans">
    
    <section class="mx-auto max-w-xl px-4 w-full">

      <!-- Main Browser Window Frame -->
      <div class="rounded-3xl bg-[#5c3a32] p-4 sm:p-5 border-2 border-[#3b221c] shadow-[4px_6px_0px_#3b221c] overflow-hidden relative">
        
        <!-- Header Browser ala Retro (Simbol Panah Kiri-Kanan & Pencaharian) -->
        <div class="mb-4 flex flex-wrap items-center justify-between gap-2 border-b-2 border-[#3b221c] pb-3 bg-[#4c2f28] -mx-4 -mt-4 p-3 px-4">
          <!-- Tombol Panah Kiri-Kanan -->
          <div class="flex items-center gap-1.5 text-xs font-bold text-[#f7d6e0]">
            <span class="bg-[#f7d6e0] text-[#5c3a32] px-2.5 py-0.5 rounded border border-[#3b221c] shadow-sm cursor-pointer">>ࡇ<</span>
            <span class="bg-[#f7d6e0] text-[#5c3a32] px-2.5 py-0.5 rounded border border-[#3b221c] shadow-sm cursor-pointer">➜</span>
          </div>

          <!-- Simbol Pencaharian (Search Bar Mini) -->
          <div class="flex-1 max-w-[180px] bg-[#fffafb] border border-[#3b221c] rounded-full px-3 py-0.5 text-[11px] text-[#8c5867] flex items-center gap-1.5 shadow-inner">
            <span>⌕</span>
            <span class="truncate">Cari tugas...</span>
          </div>

          <!-- Tombol / Simbol Kontrol Kanan -->
          <div class="text-[#f7d6e0] text-sm tracking-widest font-bold">
            ♥ ✦ ✖
          </div>
        </div>

        <!-- Inner Content Window -->
        <div class="rounded-2xl bg-[#fff0f3] p-5 border-2 border-[#3b221c] shadow-inner relative">
          
          <!-- Judul Utama (Tetap Taskify-Vue) -->
          <div class="text-center my-3">
            <h1 class="text-2xl sm:text-3xl font-extrabold text-[#5c3a32] tracking-wide inline-block bg-[#f7d6e0] px-6 py-1.5 rounded-full border-2 border-[#3b221c] shadow-sm">
              Taskify-Vue 🧸
            </h1>
            <p class="text-xs text-[#8c5867] mt-1.5 font-semibold">ִ ࣪ ˖ ࣪ ᨰꫀᥣᥴ᥆ꩇꫀ ! ⭑ ִ</p>
          </div>
          
          <!-- Formulir Tambah Tugas -->
          <form 
          @submit.prevent="addTask(inputTask)"
          action="" 
          class="flex gap-2 sm:gap-3 mt-5">
            <input
               v-model="inputTask"
               type="text"
               placeholder="Ketik tugas harian baru di sini..."
               aria-label="Tugas baru"
               class="min-w-0 flex-1 rounded-xl border-2 border-[#3b221c] bg-[#fffafb] px-4 py-3 text-[#5c3a32] placeholder-[#d0a3b3] font-medium outline-none focus:ring-2 focus:ring-[#e0a9bc] transition shadow-inner"
            />   
           <button
           type="submit"
           class="rounded-xl bg-[#e0a9bc] hover:bg-[#d095a8] border-2 border-[#3b221c] px-5 sm:px-6 py-3 font-bold text-[#3b221c] shadow-[2px_3px_0px_#3b221c] active:translate-y-0.5 active:shadow-none transition cursor-pointer"
           > ✚ Tambah</button> 
          </form>
          
          <!-- Daftar Tugas -->
          <ul id="task-list" class="mt-5 space-y-3" v-auto-animate>
              <li v-for="task in tasks" :key="task.id" 
              class="flex items-center justify-between rounded-xl bg-[#fffafb] border-2 border-[#3b221c] px-4 py-3 shadow-[2px_3px_0px_#3b221c] transition">
                    <div class="flex items-center gap-3">
                       <input 
                          v-model="task.status"
                          type="checkbox" 
                          class="h-5 w-5 rounded border-2 border-[#3b221c] accent-[#e0a9bc] cursor-pointer"  />
                        <span class="text-[#5c3a32] font-semibold transition" :class="task.status ? 'line-through opacity-40' : ''">
                         {{ task.title }}   
                        </span>
                    </div>  
                    <button 
                      @click="deleteTask(task.id)" 
                      class="text-[#a83232] hover:text-white bg-[#ffd6d6] hover:bg-[#d9534f] border-2 border-[#3b221c] font-bold text-xs px-3 py-1.5 rounded-lg transition shadow-sm cursor-pointer"
                    >
                      Hapus
                    </button>
              </li>
          </ul>

          <!-- Empty State jika belum ada tugas -->
          <div v-if="tasks.length === 0" class="mt-5 text-center py-6 border-2 border-dashed border-[#e0a9bc] rounded-xl bg-[#fffafb]/60">
            <p class="text-xs text-[#8c5867] font-semibold">Belum ada daftar tugas hari ini. Yuk mulai tambahkan kegiatanmu! 🤍</p>
          </div>

          <!-- Footer kecil di dalam window -->
          <div class="mt-6 text-center border-t border-dashed border-[#e0a9bc] pt-3">
            <span class="text-[10px] text-[#8c5867] font-bold tracking-wider">
              • Taskify-Vue •
            </span>
          </div>

        </div>
        
      </div>
     </section>
  </div>
</template>

<style scoped>
/* Motif Background Garis-Garis Vertikal Pink Pastel */
.coquette-bg {
  background-color: #fce4ec;
  background-image: repeating-linear-gradient(
    90deg,
    #f8bbd0,
    #f8bbd0 18px,
    #fce4ec 18px,
    #fce4ec 36px
  );
}
</style>