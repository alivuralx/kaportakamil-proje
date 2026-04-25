<script setup>
import { ref, watch, onMounted } from 'vue';
import AracFormu from './components/AracFormu.vue';
import RandevuListesi from './components/RandevuListesi.vue';

const randevular = ref([]);

// TOHUM VERİLER: Listeyi 6 kişiye çıkardık ve durumlarını çeşitlendirdik
const hazirRandevular = [
  {
    musteri: 'Ahmet Yılmaz',
    telefon: '05551112233',
    email: 'ahmet@gmail.com',
    marka: 'Renault Megane',
    plaka: '38AHMT159',
    sikayet: 'Sağ ön kapı göçük düzeltme ve lokal boya.',
    tarih: '2026-04-28',
    durum: 'bekliyor'
  },
  {
    musteri: 'Ayşe Kaya',
    telefon: '05324445566',
    email: 'ayse@gmail.com',
    marka: 'Honda Civic',
    plaka: '09ABU272',
    sikayet: 'Bel altı boya.',
    tarih: '2026-04-30',
    durum: 'bekliyor'
  },
  {
    musteri: 'Mehmet Demir',
    telefon: '05447778899',
    email: 'mehmet@gmail.com',
    marka: 'Ford Focus',
    plaka: '35GHI789',
    sikayet: 'Tavan dolu hasarı onarımı.',
    tarih: '2026-05-02',
    durum: 'bekliyor'
  },
  {
    musteri: 'Tarık Erdem',
    telefon: '05327456223',
    email: 'Tarık@gmail.com',
    marka: 'Ford Curier',
    plaka: '06DEF456',
    sikayet: 'Arka tampon çizik giderme ve pasta cila.',
    tarih: '2026-07-05',
    durum: 'bekliyor'
  },
  {
    musteri: 'Yusuf Efe',
    telefon: '05596327841',
    email: 'ayse@gmail.com',
    marka: 'Citroen C5',
    plaka: '65YEÇ354',
    sikayet: 'Tavanda Göçük.',
    tarih: '2026-05-01',
    durum: 'bekliyor'
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
  if(confirm("Bu randevuyu silmek istediğine emin misin Kamil Usta?")) {
    randevular.value.splice(index, 1);
  }
};
</script>

<template>
  <div 
    class="min-h-screen bg-cover bg-center bg-no-repeat flex flex-col items-center py-10 px-4 overflow-y-auto"
    style="background-image: url('/kaportakamil_arkaplan.png');"
  >
    <div class="bg-black/60 p-6 rounded-2xl text-white text-center backdrop-blur-md shadow-2xl mb-8 w-full max-w-md border border-white/10">
      <h1 class="text-4xl font-bold mb-2 tracking-wider">KAPORTA KAMİL</h1>
      <p class="text-xl font-light italic">"Hızlı Kayıt, Jilet Gibi Teslimat"</p>
    </div>

    <AracFormu @yeni-randevu="listeyeEkle" />

    <RandevuListesi 
      :randevular="randevular" 
      @sil="randevuSil" 
    />
  </div>
</template>