<script setup>
import { ref } from 'vue';

// State untuk menyimpan status pemesanan
const status = ref("Silakan pesan tiket...");
const isLoading = ref(false);

// 🔹 CALLBACK: Simulasi pemesanan tiket menggunakan callback
function bookTicketCallback(callback) {
  status.value = "⏳ Memproses pemesanan tiket...";
  setTimeout(() => {
    callback("✅ Tiket berhasil dipesan (Callback)");
  }, 2000);
}

// Handler untuk tombol Callback
function handleCallbackBooking() {
  isLoading.value = true;
  bookTicketCallback((result) => {
    status.value = result;
    isLoading.value = false;
  });
}

// 🔹 PROMISE: Simulasi pemesanan tiket dengan status Pending, Fulfilled, dan Rejected
function bookTicketPromise() {
  status.value = "⏳ Memeriksa ketersediaan tiket...";
  isLoading.value = true;

  let ticketAvailable = Math.random() > 0.3; // 70% berhasil, 30% gagal

  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (ticketAvailable) {
        resolve("✅ Tiket berhasil dipesan (Promise)");
      } else {
        reject("❌ Tiket habis! (Promise)");
      }
    }, 2000);
  });
}

// Handler untuk tombol Promise
function handlePromiseBooking() {
  bookTicketPromise()
    .then((result) => {
      status.value = result;
    })
    .catch((error) => {
      status.value = error;
    })
    .finally(() => {
      isLoading.value = false;
    });
}

// 🔹 ASYNC/AWAIT: Simulasi pemesanan tiket dengan cara yang lebih bersih
async function handleAsyncBooking() {
  status.value = "⏳ Memproses pemesanan tiket...";
  isLoading.value = true;

  try {
    let result = await bookTicketPromise();
    status.value = result;
  } catch (error) {
    status.value = error;
  } finally {
    isLoading.value = false;
  }
}
</script>

<template>
  <div class="container">
    <h1>🎟️ Pemesanan Tiket Pesawat</h1>
    
    <p class="status">{{ status }}</p>

    <div class="buttons">
      <button @click="handleCallbackBooking" :disabled="isLoading" class="btn btn-callback">
        Pesan Tiket (Callback)
      </button>
      <button @click="handlePromiseBooking" :disabled="isLoading" class="btn btn-promise">
        Pesan Tiket (Promise)
      </button>
      <button @click="handleAsyncBooking" :disabled="isLoading" class="btn btn-async">
        Pesan Tiket (Async/Await)
      </button>
    </div>
  </div>
</template>

<style scoped>
.container {
  text-align: center;
  padding: 20px;
  background: #171717;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.2);
  border-radius: 10px;
  max-width: 400px;
  margin: 20px auto;
}
h1 {
  color: #007bff;
}
.status {
  font-size: 18px;
  font-weight: bold;
  margin: 15px 0;
}
.buttons button {
  padding: 10px 15px;
  margin: 5px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}
.btn-callback {
  background: #28a745;
  color: white;
}
.btn-promise {
  background: #ffc107;
  color: black;
}
.btn-async {
  background: #17a2b8;
  color: white;
}
button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>
