<template>
  <div class="min-h-screen bg-gray-100">
    <div class="container py-6">
      <div class="bg-white shadow rounded-md p-4">
        <div class="flex items-center mb-6">
          <div class="flex-1">
            <select @change="getSpecificSura" class="select-form">
              <option v-for="sura in suras" :key="sura.id" :value="sura.number">{{ sura.englishName }} - {{ sura.name }}</option>
            </select>
          </div>
          <div class="flex-1">
            <h3 class="text-center font-bold ">{{ currentSura.name }} - {{currentSura.englishName}}</h3>
            <p class="text-center text-sm">{{ currentSura.englishNameTranslation }} | Ayahs - {{currentSura.numberOfAyahs}}</p>
          </div>
          <div class="flex-1">
            <div v-if="currentSura.hasOwnProperty('ayahs')" >
              <select @change="getSpecificAyah" class="select-form" name="" id="">
              <option class="pl-5" v-for="ayat in currentSura.ayahs" :key="ayat.id" :value="ayat.numberInSurah"> Ayah - {{ ayat.numberInSurah }}</option>
            </select>
            </div>
          </div>
        </div>
        <div v-if="loading" class="flex">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 animate-spin">
            <path stroke-linecap="round" stroke-linejoin="round" d="M4.5 12a7.5 7.5 0 0015 0m-15 0a7.5 7.5 0 1115 0m-15 0H3m16.5 0H21m-1.5 0H12m-8.457 3.077l1.41-.513m14.095-5.13l1.41-.513M5.106 17.785l1.15-.964m11.49-9.642l1.149-.964M7.501 19.795l.75-1.3m7.5-12.99l.75-1.3m-6.063 16.658l.26-1.477m2.605-14.772l.26-1.477m0 17.726l-.26-1.477M10.698 4.614l-.26-1.477M16.5 19.794l-.75-1.299M7.5 4.205L12 12m6.894 5.785l-1.149-.964M6.256 7.178l-1.15-.964m15.352 8.864l-1.41-.513M4.954 9.435l-1.41-.514M12.002 12l-3.75 6.495" />
          </svg>

          loading ...

        </div>
        <div class="content">
            <div v-if="currentSura.hasOwnProperty('ayahs')" >
              <p class="my-4 flex items-center" v-for="ayah in currentSura.ayahs" :key="ayah.id" >
                <span class="text-xl rounded bg-sky-300 w-5 h-5 p-4 flex justify-center items-center mr-2">{{ ayah.numberInSurah }}</span>
                 <span class="text-4xl my-2">{{ ayah.text }}</span>
            </p>
            </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
  data () {
    return {
      suras: [],
      currentSura : [],
      loading: true,
      currentAyah: []
    }
  },
  mounted () {

    axios.get('https://api.alquran.cloud/v1/surah')
      .then(response => {
        this.suras = response.data.data
      })

    this.querySpecificSura(1);

  },
  methods: {

    getSpecificSura(e) {
      this.querySpecificSura(e.target.value);
    },

    querySpecificSura(suraNumber) {
      this.loading = true;
      axios.get('https://api.alquran.cloud/v1/surah/' + suraNumber).then(response => {
        this.currentSura = response.data.data;
        this.loading = false;
      })
    },

    getSpecificAyah(e) {
      this.getSpecificAyah(e.target.value);
    },

    getSpecificAyah(suraNumber, ayahNumber) {
      this.loading = true;
      axios.get('https://api.alquran.cloud/v1/surah/'+ suraNumber + '/' +ayahNumber).then(response => {
        this.currentAyah = response.data.data;
        this.loading = false;
      })
    }
    
  }
};
</script>

<style scoped>

</style>
