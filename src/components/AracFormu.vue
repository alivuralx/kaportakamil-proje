<template>
  <div class="bg-black/40 backdrop-blur-sm p-6 rounded-xl shadow-lg w-full max-w-md mx-auto mt-8 border border-white/20 transition-all duration-300">
    
    <div 
      @click="formAcik = !formAcik" 
      class="flex justify-between items-center cursor-pointer border-b border-white/20 pb-2 mb-2 group"
    >
      <h2 class="text-2xl font-bold text-white group-hover:text-blue-300 transition-colors">
        Yeni Randevu Oluştur
      </h2>
      <span class="text-white text-xl transform transition-transform duration-300" :class="formAcik ? 'rotate-180' : ''">
        ▼
      </span>
    </div>
    
    <form v-show="formAcik" @submit.prevent="randevuEkle" class="space-y-6 mt-6">
      
      <div class="space-y-4">
        <h3 class="text-lg font-semibold text-blue-300">1. Kişisel Bilgiler</h3>
        <div>
          <label class="block text-white text-sm font-bold mb-1">Ad Soyad *</label>
          <input v-model="form.musteri" type="text" placeholder="Örn: Ali Vural" class="w-full p-2 rounded bg-white/80 text-gray-800 outline-none focus:ring-2 focus:ring-blue-500" required>
        </div>
        <div>
          <label class="block text-white text-sm font-bold mb-1">Telefon Numarası *</label>
          <input 
            v-model="form.telefon" 
            type="tel" 
            pattern="05[0-9]{9}" 
            maxlength="11" 
            title="Lütfen 05XX... formatında 11 haneli, boşluksuz numara girin."
            placeholder="Örn: 05551234567" 
            class="w-full p-2 rounded bg-white/80 text-gray-800 outline-none focus:ring-2 focus:ring-blue-500" 
            required
          >
        </div>
        <div>
          <label class="block text-white text-sm font-bold mb-1">E-posta Adresi *</label>
          <input v-model="form.email" type="email" placeholder="Örn: alivuralq@gmail.com" class="w-full p-2 rounded bg-white/80 text-gray-800 outline-none focus:ring-2 focus:ring-blue-500" required>
        </div>
      </div>

      <div class="space-y-4 pt-4 border-t border-white/20">
        <h3 class="text-lg font-semibold text-blue-300">2. Araç Bilgileri (İsteğe Bağlı)</h3>
        <div>
          <label class="block text-white text-sm font-bold mb-1">Araç Markası / Modeli</label>
          <input v-model="form.marka" type="text" placeholder="Örn: Volkswagen Golf" class="w-full p-2 rounded bg-white/80 text-gray-800 outline-none focus:ring-2 focus:ring-blue-500">
        </div>
        <div>
          <label class="block text-white text-sm font-bold mb-1">Araç Plakası</label>
          <input v-model="form.plaka" type="text" placeholder="Örn: 38AV123" class="w-full p-2 rounded bg-white/80 text-gray-800 outline-none focus:ring-2 focus:ring-blue-500">
        </div>
        <div>
          <label class="block text-white text-sm font-bold mb-1">Hasar / Şikayet</label>
          <textarea v-model="form.sikayet" rows="2" placeholder="Araçtaki sorun nedir?" class="w-full p-2 rounded bg-white/80 text-gray-800 outline-none focus:ring-2 focus:ring-blue-500"></textarea>
        </div>
        <div>
          <label class="block text-white text-sm font-bold mb-1">Randevu Tarihi</label>
          <input v-model="form.tarih" type="date" :min="bugun" class="w-full p-2 rounded bg-white/80 text-gray-800 outline-none focus:ring-2 focus:ring-blue-500">
        </div>
      </div>

      <button type="submit" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-4 rounded-lg transition duration-300 shadow-lg mt-4">
        Randevu Talebi Gönder
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// App.vue'ya haber göndermek için "emit" tanımlıyoruz
const emit = defineEmits(['yeniRandevu']);

const formAcik = ref(false);
const bugun = new Date().toISOString().split('T')[0];

const form = ref({
  musteri: '', telefon: '', email: '', marka: '', plaka: '', sikayet: '', tarih: '', durum: 'bekliyor' 
});

const randevuEkle = () => {
  // Formdaki verinin bir kopyasını 'yeniRandevu' adıyla yukarıya (App.vue) gönder
  emit('yeniRandevu', { ...form.value });
  
  // Formu sıfırla ve kapat
  form.value = { musteri: '', telefon: '', email: '', marka: '', plaka: '', sikayet: '', tarih: '', durum: 'bekliyor' };
  formAcik.value = false;
};
</script>