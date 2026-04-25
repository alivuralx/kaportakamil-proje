<template>
  <div class="w-full max-w-4xl mx-auto mt-8">
    <h2 class="text-2xl font-bold text-white mb-4 drop-shadow-md">Kayıtlı Randevular</h2>
    
    <div class="bg-blue-900/60 border-l-4 border-blue-400 p-4 mb-6 rounded-lg text-blue-100 text-sm backdrop-blur-sm shadow-md">
      📌 <strong>Bilgilendirme:</strong> Randevuların onay işlemleri yetkililer tarafından yapılacaktır. Lütfen onay sürecini bekleyiniz.
    </div>

    <div v-if="randevular.length === 0" class="bg-black/40 backdrop-blur-sm p-6 rounded-xl border border-white/20 text-center text-gray-300">
      Henüz bir randevu kaydı bulunmuyor.
    </div>

    <div v-else class="grid grid-cols-1 md:grid-cols-2 gap-4">
      
      <div 
        v-for="(randevu, index) in randevular" 
        :key="index" 
        class="bg-black/60 backdrop-blur-md p-5 rounded-xl border-l-4 shadow-lg transition-all"
        :class="randevu.durum === 'bekliyor' ? 'border-yellow-500' : 'border-green-500'"
      >
        <div class="flex justify-between items-start mb-3">
          <div>
            <h3 class="text-xl font-bold text-white">{{ randevu.musteri }}</h3>
            <p class="text-sm text-gray-300">{{ randevu.telefon }}</p>
          </div>
          
          <span class="px-3 py-1 rounded-full text-xs font-bold uppercase tracking-wider bg-yellow-500/20 text-yellow-400">
            Onay Bekliyor
          </span>
        </div>

        <div class="text-gray-200 text-sm space-y-1 mb-4">
          <p><span class="font-semibold text-blue-300">Araç:</span> {{ randevu.marka || 'Belirtilmedi' }} ({{ randevu.plaka || 'Plaka Yok' }})</p>
          
          <p><span class="font-semibold text-blue-300">Tarih:</span> {{ trTarih(randevu.tarih) }}</p>
          
          <p class="mt-2 bg-white/10 p-2 rounded italic text-gray-300">"{{ randevu.sikayet || 'Şikayet belirtilmedi.' }}"</p>
        </div>

        <div class="flex gap-2 mt-4 pt-4 border-t border-white/10">
          <button 
            @click="$emit('sil', index)"
            class="w-full bg-red-600 hover:bg-red-700 text-white py-2 rounded transition-colors text-sm font-bold"
          >
            Talebi İptal Et (Sil)
          </button>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup>
defineProps({
  randevular: { type: Array, required: true }
});

// Sadece 'sil' komutu kaldı
defineEmits(['sil']);

// YENİ: Tarihi YYYY-MM-DD'den GG.AA.YYYY'ye çeviren fonksiyon
const trTarih = (tarih) => {
  if (!tarih) return 'Tarih Seçilmedi';
  const [yil, ay, gun] = tarih.split('-'); // Tireden böl
  return `${gun}.${ay}.${yil}`; // Nokta ile birleştir
};
</script>