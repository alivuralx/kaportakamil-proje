<script setup>
import { ref, watch, onMounted } from 'vue';
import AracFormu from './components/AracFormu.vue';
import RandevuListesi from './components/RandevuListesi.vue';

const randevular = ref([]);

// TOHUM VERİLER: Senin seçtiğin görsellerle güncellendi
const hazirRandevular = [
  {
    musteri: 'Ahmet Yılmaz',
    telefon: '05551112233',
    email: 'ahmet@mail.com',
    marka: 'Renault Megane',
    plaka: '38AHMT159',
    sikayet: 'Sağ ön kapı göçük düzeltme ve lokal boya.',
    tarih: '2026-04-28',
    durum: 'onaylandı',
    resimler: ['https://arbstorage.mncdn.com/ilanfotograflari/2026/04/16/39723690/995331bd-c73f-4f90-8778-df12fe386563_image_for_silan_39723690_580x435.jpg']
  },
  {
    musteri: 'Ayşe Kaya',
    telefon: '05324445566',
    email: 'ayse@mail.com',
    marka: 'Honda Civic',
    plaka: '09ABU272',
    sikayet: 'Bel altı boya.',
    tarih: '2026-04-30',
    durum: 'bekliyor',
    resimler: ['https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTfg7LCSdLEiYIKMptTTEYN1ngeqXYH-U-fWQ&s']
  },
  {
    musteri: 'Mehmet Demir',
    telefon: '05447778899',
    email: 'mehmet@mail.com',
    marka: 'Ford Focus',
    plaka: '35GHI789',
    sikayet: 'Tavan dolu hasarı onarımı.',
    tarih: '2026-05-02',
    durum: 'onaylandı',
    resimler: ['https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSL4WIZwk_K1ZZJdR1kTgWsBgHWK1azMEzM3Q&s']
  },
  {
    musteri: 'Tarık Erdem',
    telefon: '05327456223',
    email: 'Tarık@gmail.com',
    marka: 'Ford Courier',
    plaka: '06DEF456',
    sikayet: 'Arka tampon çizik giderme ve pasta cila.',
    tarih: '2026-07-05',
    durum: 'bekliyor',
    resimler: ['https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcS1UqJciVsq9oYpSDwDKERGmXb3qzGjslYreQ&s']
  },
  {
    musteri: 'Yusuf Efe',
    telefon: '05596327841',
    email: 'yusuf@gmail.com',
    marka: 'Citroen C5',
    plaka: '65YEÇ354',
    sikayet: 'Tavanda Göçük.',
    tarih: '2026-05-01',
    durum: 'bekliyor',
    resimler: ['https://files.sikayetvar.com/complaint/3123/31234265/citroen-c5in-guvenlik-sorunlari-kucuk-kaza-buyuk-hasar-6.jpg']
  }
];

onMounted(() => {
  const taslaktakiVeriler = localStorage.getItem('kaporta_randevular');
  
  if (taslaktakiVeriler && JSON.parse(taslaktakiVeriler).length > 0) {
    randevular.value = JSON.parse(taslaktakiVeriler);
  } else {
    randevular.value = [...hazirRandevular];
  }
});

watch(randevular, (yeniListe) => {
  localStorage.setItem('kaporta_randevular', JSON.stringify(yeniListe));
}, { deep: true });

const listeyeEkle = (yeniVeri) => {
  randevular.value.push(yeniVeri);
};

const randevuSil = (index) => {
  if(confirm("Bu randevuyu silmek istediğine emin misin?")) {
    randevular.value.splice(index, 1);
  }
};
</script>

<template>
  <div 
    class="min-h-screen bg-cover bg-center bg-no-repeat flex flex-col items-center py-10 px-4 overflow-y-auto"
    style="background-image: url('/kaportakamil_arkaplan.png');"
  >
    <div class="w-full max-w-2xl mb-8 bg-black/80 border border-amber-500/30 backdrop-blur-md p-4 rounded-2xl text-center shadow-xl">
      <p class="text-amber-200 text-xs sm:text-sm font-medium leading-relaxed">
        ⚠️ <strong>SİSTEM TEST AŞAMASINDADIR:</strong> Bu bir portfolyo/öğrenci projesidir. 
        Girilen veriler gerçek bir veritabanına kaydedilmez, sadece tarayıcınızın geçici hafızasında saklanır. 
        Görseller ve müşteri bilgileri örnek amaçlıdır.
      </p>
    </div>

    <div class="bg-black/60 p-6 rounded-3xl text-white text-center backdrop-blur-xl shadow-2xl mb-8 w-full max-w-md border border-white/10">
      <h1 class="text-4xl font-extrabold mb-2 tracking-tighter text-blue-400">KAPORTA KAMİL</h1>
      <p class="text-lg font-light italic text-gray-300">"Hızlı Kayıt, Jilet Gibi Teslimat"</p>
    </div>

    <AracFormu @yeni-randevu="listeyeEkle" />

    <RandevuListesi 
      :randevular="randevular" 
      @sil="randevuSil" 
    />
  </div>
</template>

<style>
/* Sayfa genelinde daha yumuşak bir kaydırma ve font */
body {
  font-family: 'Inter', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>