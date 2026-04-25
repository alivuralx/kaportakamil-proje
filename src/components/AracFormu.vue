<template>
  <div class="w-full max-w-md bg-black/70 backdrop-blur-lg p-6 rounded-3xl shadow-2xl border border-white/20 mb-10">
    <button 
      @click="formAcik = !formAcik" 
      class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-2xl transition-all duration-300 transform hover:scale-105 shadow-lg flex items-center justify-center gap-2"
    >
      <span>{{ formAcik ? 'Formu Kapat' : 'Hemen Randevu Al' }}</span>
      <span class="text-xl">{{ formAcik ? '×' : '+' }}</span>
    </button>

    <div v-if="formAcik" class="mt-6 space-y-4 animate-fade-in">
      <form @submit.prevent="randevuEkle" class="space-y-4">
        <div class="grid grid-cols-1 gap-4">
          <input v-model="form.musteri" type="text" placeholder="Ad Soyad" class="w-full p-2 rounded bg-white/10 text-white border border-white/20 outline-none focus:border-blue-500" required>
          <input v-model="form.telefon" type="tel" pattern="05[0-9]{9}" maxlength="11" placeholder="Telefon (05XXXXXXXXX)" class="w-full p-2 rounded bg-white/10 text-white border border-white/20 outline-none" required>
          <input v-model="form.marka" type="text" placeholder="Araç Marka / Model" class="w-full p-2 rounded bg-white/10 text-white border border-white/20 outline-none">
          <input v-model="form.plaka" type="text" placeholder="Plaka" class="w-full p-2 rounded bg-white/10 text-white border border-white/20 outline-none">
          <input v-model="form.tarih" type="date" :min="bugun" class="w-full p-2 rounded bg-white/10 text-white border border-white/20 outline-none" required>
          <textarea v-model="form.sikayet" placeholder="Şikayetiniz nedir?" class="w-full p-2 rounded bg-white/10 text-white border border-white/20 outline-none h-20"></textarea>
          
          <div class="space-y-2">
            <label class="block text-xs text-blue-300 font-bold uppercase tracking-widest">Araç / Hasar Fotoğrafları</label>
            <input 
              @change="resimlerSec" 
              type="file" 
              accept="image/*" 
              multiple
              class="w-full text-xs text-gray-400 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-xs file:font-semibold file:bg-blue-600 file:text-white hover:file:bg-blue-700 cursor-pointer"
            >
            
            <div v-if="form.resimler.length > 0" class="flex gap-3 overflow-x-auto py-2 scrollbar-hide">
              <div v-for="(img, idx) in form.resimler" :key="idx" class="relative flex-shrink-0">
                <img :src="img" class="w-20 h-20 object-cover rounded-lg border border-white/20 shadow-md">
                <button 
                  @click.prevent="resimSil(idx)" 
                  class="absolute -top-2 -right-2 bg-red-600 text-white w-6 h-6 rounded-full flex items-center justify-center text-xs font-bold shadow-lg hover:bg-red-700 transition-colors border border-white/30"
                >
                  ×
                </button>
              </div>
            </div>
          </div>
        </div>

        <button type="submit" class="w-full bg-green-600 hover:bg-green-700 text-white font-bold py-3 rounded-xl transition-all shadow-lg">
          Randevu Talebi Gönder
        </button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const emit = defineEmits(['yeniRandevu']);
const formAcik = ref(false);
const bugun = new Date().toISOString().split('T')[0];

const form = ref({
  musteri: '', telefon: '', marka: '', plaka: '', sikayet: '', tarih: '', resimler: [], durum: 'bekliyor' 
});

const resimlerSec = (event) => {
  const dosyalar = Array.from(event.target.files);
  // Önceki seçimlerin üzerine eklemek istiyorsan aşağıdaki satırı silebilirsin
  // form.value.resimler = []; 

  dosyalar.forEach(dosya => {
    const okuyucu = new FileReader();
    okuyucu.onload = (e) => {
      form.value.resimler.push(e.target.result);
    };
    okuyucu.readAsDataURL(dosya);
  });
};

// Seçilen resmi listeden çıkaran yeni fonksiyon
const resimSil = (index) => {
  form.value.resimler.splice(index, 1);
};

const randevuEkle = () => {
  emit('yeniRandevu', { ...form.value });
  form.value = { musteri: '', telefon: '', marka: '', plaka: '', sikayet: '', tarih: '', resimler: [], durum: 'bekliyor' };
  formAcik.value = false;
};
</script>

<style scoped>
.scrollbar-hide::-webkit-scrollbar { display: none; }
.scrollbar-hide { -ms-overflow-style: none; scrollbar-width: none; }
.animate-fade-in { animation: fadeIn 0.4s ease-out; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(-10px); } to { opacity: 1; transform: translateY(0); } }
</style>