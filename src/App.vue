<script setup>
import CardAlbum from '@/components/CardAlbum.vue'
import {records} from '@/assets/js/allRecords.js'
import {ref,computed} from "vue"

console.log(records)

const localRecords   = ref(records)
const displayRecords = ref(records)

const stockOnly   = ref(false)
const sortOptions = ref('')

console.log(localRecords)

const totalRecords = computed(() => {
  // console.log(displayRecords.value.length)
  return displayRecords.value.length
  
})


const totalStock = computed(()=>{
  return displayRecords.value.reduce((acc, cur) =>{
    return acc + cur.stock;
  }, 0);
})

const handleStockCheckboxe =()=>{
  if(stockOnly.value){
    displayRecords.value = localRecords.value.filter(album => album.stock > 0);
  }else{
    displayRecords.value = records
  }
}
const handleSortOptions =()=>{
    console.log('changement de sort!!!', sortOptions.value)
    if(sortOptions.value=='Year'){
      displayRecords.value.sort((albumA,albumB)=>{
        return  +albumB.year - +albumA.year 
      })
    }
    if(sortOptions.value=='Pitchfork'){
      displayRecords.value.sort((albumA,albumB)=>{
        return  albumA.pitchforkPos - albumB.pitchforkPos 
      })
    }
    if(sortOptions.value==''){
      displayRecords.value = records
    }

}
const handleFavAlbum =(id)=>{

  const selectedAlbum = displayRecords.value.find((album) => album.id===id)

  selectedAlbum.isFav = !selectedAlbum.isFav

}


handleStockCheckboxe()


</script>

<template>
  <div class="min-h-screen flex flex-col">
      <header class="h-32 text-2xl w-full flex-none -ml-full shadow-lg bg-gradient-to-br from-teal-600 to-cyan-400">
        <div class="p-4 h-32 text-2xl w-full flex-none -ml-full rounded-2xl transform shadow-lg bg-gradient-to-br from-cyan-400 to-teal-600 -rotate-1 sm:-rotate-1">Ynov Records (({{totalStock}}))</div>
      </header>
      
    
      <div class="flex-1 flex flex-col sm:flex-row ">
        <main class="flex-1 bg-white py-5">

          <!-- component -->

          <CardAlbum 
            v-for="(album,idx) in displayRecords"
            :key="album.id"
            :album="album"
            :isFav="album.isFav"
            @click="handleFavAlbum(album.id)"
          />

          
         
        </main>
    
        <div class="order-first w-2/12 flex px-8 bg-cyan-100 "> <!-- left filter panel -->
          <div class="mt-2">

              <div class="overflow-hidden shadow sm:rounded-md">
                <div class="bg-white p-6 w-64 h-96">
                  
                  <fieldset>
                    <legend class="sr-only"> Filtres </legend>
                    <div class="text-base font-medium text-gray-900" aria-hidden="true">Filtres ({{totalRecords}})</div>
                      <div class="mt-4 space-y-4">

                        <div class="flex items-start">
                          <div class="flex h-5 items-center">
                            <input 
                              id="comments" 
                              name="comments" 
                              type="checkbox" 
                              class="h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500"
                              v-model="stockOnly"
                              @change="handleStockCheckboxe"
                            >
                          </div>
                          <div class="ml-3 text-sm">
                            <label for="comments" class="font-medium text-cyan-700">In stock only</label>
                          </div>
                          <pre>{{sortOptions}}</pre>
                        </div>
   
                      </div>
                      <label for="sortBy" class="block text-sm mt-2 font-medium text-cyan-700">Sort by</label>
                      <select v-model='sortOptions' @change='handleSortOptions' id="sortBy" name="sortBy" autocomplete="country-name" class="mt-1 block w-full rounded-md border border-gray-300 bg-white py-2 px-3 shadow-sm focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm">
                        <option></option>
                        <option>Year     </option>
                        <option>Pitchfork</option>
                      </select>
                  </fieldset>
                 
                </div>
               
              </div>
          </div>
          
        </div>
      </div>
    
      <footer class="bg-gray-100">Pied</footer>
    </div>
  
</template>

<style scoped>

</style>
