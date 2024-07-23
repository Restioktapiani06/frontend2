<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <div class="my-3">
          <div class="my-3">
            <form @submit.prevent="getBooks">
              <input v-model="keyword" type="search" class="form-control rounded-5" placeholder="MASUKAN JUDUL BUKU?">
            </form>
        </div>
        <div class="my-3 text-muted">Menampilkan {{ books.length }} dari {{ jumlah }}</div>
        <div class="row">
          <div v-for="(book,i) in books" :key="i" class="col-md-2">
            <div class="card mb-2">
              <div class="card-body">
                <nuxt-link :to="`/buku/${book.id}`">
                <img :src="book.cover" alt="" width="180" height="200" >
                <h4>{{ book.judul }}</h4>
                </nuxt-link>
              </div>
            </div>
          </div>
          
        </div>
      </div>
    </div>

    <nuxt-link to="/">
      <button type="submit" class="btn btn-primary btn-lg rounded-5 px-5">Selesai</button>
        </nuxt-link>
        </div>
  </div>
</template> 


<style scoped>
.card-body {
    width: 100%;
    height: 20em;
    padding: 0;
}

.cover {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: 0 30;
}
</style>

<script setup>
const supabase = useSupabaseClient()

const books = ref([])
const jumlah = ref(0)
const keyword = ref('')


const getBooks = async () => {
  const { data, error } = await supabase.from('buku').select(`*, kategori(*)`)
    .ilike('judul', `%${keyword.value}%`)
  if (data) books.value = data
}

const totalBuku = async () => {
  const { data, count } = await supabase.from('buku').select("*", { count: 'exact' })
  if (data) jumlah.value = count
}

onMounted(() => {
  getBooks()
  totalBuku()
})
</script>