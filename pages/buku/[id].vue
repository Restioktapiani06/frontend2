<template>
  <div class="container shadow p-2 mt-3">
    <h2 class="text-start my-4">{{ buku.judul }}</h2>
      <div class="row mt-4">
        <div class="col-4">
          <img :src="buku.cover" class="cover" alt="cover buku" height="340px" width="340px">
        </div>
        <div class="col-md-6">
          <div class="badge bg-primary p-2">{{ buku.kategori }}</div>
            <ul class="list-group lish-group-plush">
              <li class="list-group-item">penulis: {{ buku.penulis }}</li>
              <li class="list-group-item">tahun_terbit: {{ buku.tahun_terbit }}</li>
              <li class="list-group-item">penerbit: {{ buku.penerbit }}</li>
              <li class="list-group-item">deskripsi: {{ buku.deskripsi }}</li>
            </ul>
          </div>
        </div>
      <div>
      <nuxt-link to="/buku">
        <button type="submit" class="btn btn-primary btn-lg rounded-5 px-5">Selesai</button>
      </nuxt-link>
    </div>
  </div>
</template>

<style scoped>
</style>

<script setup>
const supabase = useSupabaseClient()

const route = useRoute()
const buku = ref([])

const getBookById = async () => {
  const { data, error } = await supabase.from('buku').select(`*,kategori_buku(*)`)
  .eq('id', route.params.id)
  if(data) buku.value = data[0]
  if(error)console.log(error)
}

onMounted(() => {
  getBookById()
})
</script>