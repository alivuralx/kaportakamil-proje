<template>
  <div class="w-full max-w-4xl mx-auto mt-8 px-2">
    <h2 class="text-2xl font-bold text-white mb-4 drop-shadow-md">Kayıtlı Randevular</h2>
    
    <div class="bg-blue-900/60 border-l-4 border-blue-400 p-4 mb-6 rounded-lg text-blue-100 text-sm backdrop-blur-sm shadow-md">
      📌 <strong>Bilgilendirme:</strong> Randevuların onay işlemleri Admin Panelinden yetkililer tarafından yapılacaktır. Lütfen onay sürecini bekleyiniz.
    </div>

    <div v-if="siraliRandevular.length === 0" class="bg-black/40 backdrop-blur-sm p-6 rounded-xl border border-white/20 text-center text-gray-300">
      Henüz bir randevu kaydı bulunmuyor.
    </div>

    <div v-else class="space-y-3">
      
      <div 
        v-for="(randevu, index) in siraliRandevular" 
        :key="index" 
        class="bg-black/60 backdrop-blur-md rounded-xl border-l-4 shadow-lg transition-all overflow-hidden"
        :class="randevu.durum === 'bekliyor' ? 'border-yellow-500' : 'border-green-500'"
      >
        <div 
          @click="detayGosterGizle(index)"
          class="p-4 flex justify-between items-center cursor-pointer hover:bg-white/5 transition-colors group"
        >
          <div class="flex items-center gap-4">
            <div class="w-20 sm:w-24 text-center border-r border-white/20 pr-4">
              <p class="text-[10px] text-blue-300 font-bold uppercase">Tarih</p>
              <p class="text-xs sm:text-sm text-white font-semibold">{{ trTarih(randevu.tarih) }}</p>
            </div>
            <div>
              <h3 class="text-md sm:text-lg font-bold text-white group-hover:text-blue-300 transition-colors">{{ randevu.musteri }}</h3>
              <p class="text-xs text-gray-400">{{ randevu.marka || 'Araç Belirtilmedi' }}</p>
            </div>
          </div>
          
          <div class="flex items-center gap-3">
            <span 
              class="px-3 py-1 rounded-full text-[10px] font-bold uppercase tracking-wider hidden sm:block"
              :class="randevu.durum === 'bekliyor' ? 'bg-yellow-500/20 text-yellow-400' : 'bg-green-500/20 text-green-400'"
            >
              {{ randevu.durum === 'bekliyor' ? 'Onay Bekliyor' : 'Onaylandı' }}
            </span>
            <span class="text-white text-lg transform transition-transform duration-300" :class="acikRandevu === index ? 'rotate-180' : ''">
              ▼
            </span>
          </div>
        </div>

        <div v-show="acikRandevu === index" class="p-4 border-t border-white/10 bg-black/40 animate-fade-in">
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 text-sm text-gray-200">
            <div class="space-y-3">
              <p><span class="font-semibold text-blue-300">Telefon:</span> {{ randevu.telefon }}</p>
              <p><span class="font-semibold text-blue-300">Plaka:</span> {{ randevu.plaka || 'Belirtilmedi' }}</p>
              
              <div v-if="randevu.resimler && randevu.resimler.length > 0" class="mt-4">
                <p class="text-xs text-blue-300 font-bold mb-2 uppercase tracking-widest">Hasar Görselleri ({{ randevu.resimler.length }})</p>
                <div class="grid grid-cols-2 sm:grid-cols-3 gap-2">
                  <img 
                    v-for="(resim, rIdx) in randevu.resimler" 
                    :key="rIdx" 
                    :src="resim" 
                    class="w-full h-24 object-cover rounded-lg border border-white/10 shadow-lg hover:scale-105 transition-transform cursor-pointer"
                    @click.stop="resmiYeniSekmedeAc(resim)"
                  >
                </div>
              </div>
            </div>
            
            <div class="flex flex-col justify-between">
              <div>
                <p class="font-semibold text-blue-300 mb-2">Müşteri Şikayeti / Detaylar:</p>
                <p class="bg-white/5 p-4 rounded-lg italic text-gray-300 border border-white/5 leading-relaxed">
                  "{{ randevu.sikayet || 'Herhangi bir detay belirtilmedi.' }}"
                </p>
              </div>

              <div class="mt-6">
                <button 
                  @click="$emit('sil', props.randevular.indexOf(randevu))"
                  class="w-full bg-red-600/60 hover:bg-red-700 text-white py-2.5 rounded-lg transition-all duration-300 text-xs font-bold uppercase tracking-widest border border-red-500/50"
                >
                  Talebi İptal Et (Sil)
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
  randevular: { type: Array, required: true }
});

defineEmits(['sil']);

const acikRandevu = ref(null);

const detayGosterGizle = (index) => {
  acikRandevu.value = acikRandevu.value === index ? null : index;
};

const trTarih = (tarih) => {
  if (!tarih) return 'Tarih Yok';
  const [yil, ay, gun] = tarih.split('-');
  return `${gun}.${ay}.${yil}`;
};

// Resme tıklayınca daha büyük görmek için yeni sekmede açar
const resmiYeniSekmedeAc = (resimData) => {
  const image = new Image();
  image.src = resimData;
  const w = window.open("");
  w.document.write(image.outerHTML);
};

const siraliRandevular = computed(() => {
  return [...props.randevular].sort((a, b) => new Date(a.tarih) - new Date(b.tarih));
});
</script>

<style scoped>
.animate-fade-in {
  animation: fadeIn 0.3s ease-in-out;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>