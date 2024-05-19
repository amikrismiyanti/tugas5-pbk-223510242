<template>
  <q-layout view="lHh lpr lFf">
    <q-header elevated class="header">
      <q-toolbar>
        <q-toolbar-title>Toko Buah dan Sayur</q-toolbar-title>
        <q-space />
        <q-input v-model="search" debounce="300" placeholder="Cari produk..." @input="filterProduk" outlined dense></q-input>
        <q-btn flat round dense icon="shopping_cart" @click="goToCartPage" class="header-btn" />
        <q-btn flat round dense icon="person" @click="goToProfilePage" class="header-btn" />
      </q-toolbar>
    </q-header>

    <q-page-container>
      <q-page padding class="page-background">
        <q-carousel
          animated
          v-model="slide"
          :autoplay="5000"
          arrows
          infinite
          class="carousel"
        >
          <q-carousel-slide
            :name="1"
            img-src="https://www.pinhome.id/info-area/wp-content/uploads/2022/04/Toko-Buah-di-Bogor.jpg"
          />
          <q-carousel-slide
            :name="2"
            img-src="https://blue.kumparan.com/image/upload/fl_progressive,fl_lossy,c_fill,q_auto:best,w_640/v1634025439/01hhe78arhahcfrwzsp2ggmn8q.jpg"
          />
          <q-carousel-slide
            :name="3"
            img-src="https://asset.kompas.com/crops/7Fpdr2h4y0xBUj1HEvSMnMM3qMM=/0x0:1000x667/750x500/data/photo/2019/05/26/1310122708.jpg"
          />
          <q-carousel-slide
            :name="4"
            img-src="https://www.rukita.co/stories/wp-content/uploads/2023/01/toko-buah-terdekat.jpeg"
          />
        </q-carousel>

        <q-tabs v-model="tab" class="q-mt-lg tabs" align="justify" indicator-color="primary">
          <q-tab name="semua" label="Semua" />
          <q-tab name="buah" label="Buah" />
          <q-tab name="sayuran" label="Sayuran" />
        </q-tabs>

        <q-tab-panels v-model="tab" animated>
          <q-tab-panel name="semua" class="tab-panel">
            <div class="q-gutter-md">
              <q-card
                v-for="produk in filteredProduk"
                :key="produk.nama"
                class="q-ma-md produk-card"
              >
                <q-card-section>
                  <q-img
                    :src="produk.gambar"
                    class="product-image"
                    :alt="produk.nama"
                  />
                  <div class="product-details">
                    <div class="text-h6">{{ produk.nama }}</div>
                    <div class="text-subtitle2">Warna: {{ produk.warna }}</div>
                    <div class="text-subtitle2">Rasa: {{ produk.rasa }}</div>
                    <div class="text-subtitle2">Berat: {{ produk.berat }} gram</div>
                    <div class="text-h6">Harga: Rp {{ produk.harga }}</div>
                    <q-rating v-model="produk.rating" max="5" size="20px" color="amber" />
                    <q-btn color="primary" label="Tambah ke Keranjang" @click="addToCart(produk)" class="add-to-cart-btn"></q-btn>
                  </div>
                </q-card-section>
              </q-card>
            </div>
          </q-tab-panel>

          <q-tab-panel name="buah" class="tab-panel">
            <div class="q-gutter-md">
              <q-card
                v-for="produk in filteredProduk"
                :key="produk.nama"
                class="q-ma-md produk-card"
              >
                <q-card-section>
                  <q-img
                    :src="produk.gambar"
                    class="product-image"
                    :alt="produk.nama"
                  />
                  <div class="product-details">
                    <div class="text-h6">{{ produk.nama }}</div>
                    <div class="text-subtitle2">Warna: {{ produk.warna }}</div>
                    <div class="text-subtitle2">Rasa: {{ produk.rasa }}</div>
                    <div class="text-subtitle2">Berat: {{ produk.berat }} gram</div>
                    <div class="text-h6">Harga: Rp {{ produk.harga }}</div>
                    <q-rating v-model="produk.rating" max="5" size="20px" color="amber" />
                    <q-btn color="primary" label="Tambah ke Keranjang" @click="addToCart(produk)" class="add-to-cart-btn"></q-btn>
                  </div>
                </q-card-section>
              </q-card>
            </div>
          </q-tab-panel>

          <q-tab-panel name="sayuran" class="tab-panel">
            <div class="q-gutter-md">
              <q-card
                v-for="produk in filteredProduk"
                :key="produk.nama"
                class="q-ma-md produk-card"
              >
                <q-card-section>
                  <q-img
                    :src="produk.gambar"
                    class="product-image"
                    :alt="produk.nama"
                  />
                  <div class="product-details">
                    <div class="text-h6">{{ produk.nama }}</div>
                    <div class="text-subtitle2">Warna: {{ produk.warna }}</div>
                    <div class="text-subtitle2">Rasa: {{ produk.rasa }}</div>
                    <div class="text-subtitle2">Berat: {{ produk.berat }} gram</div>
                    <div class="text-h6">Harga: Rp {{ produk.harga }}</div>
                    <q-rating v-model="produk.rating" max="5" size="20px" color="amber" />
                    <q-btn color="primary" label="Tambah ke Keranjang" @click="addToCart(produk)" class="add-to-cart-btn"></q-btn>
                  </div>
                </q-card-section>
              </q-card>
            </div>
          </q-tab-panel>
        </q-tab-panels>

        <div class="q-my-lg">
          <q-banner class="q-mb-lg promo-banner" color="purple-5" text-color="white" inline-actions>
            <div class="text-h6">Promo Hari Ini!</div>
            <div>Diskon 20% untuk semua produk segar.</div>
            <q-btn flat label="Lihat Promo" text-color="white" @click="lihatPromo" />
          </q-banner>
        </div>

        <div class="q-my-lg">
          <q-card class="q-ma-md testimoni-card">
            <q-card-section class="testimoni-section">
              <q-avatar icon="person" />
              <div class="testimoni-details">
                <div class="text-subtitle1">Remboo</div>
                <div class="q-mt-sm">"Produk-produk di toko ini sangat segar dan berkualitas tinggi. Saya sangat puas!"</div>
              </div>
            </q-card-section>
          </q-card>
          <q-card class="q-ma-md testimoni-card">
            <q-card-section class="testimoni-section">
              <q-avatar icon="person" />
              <div class="testimoni-details">
                <div class="text-subtitle1">Agus takut buk</div>
                <div class="q-mt-sm">"Hai guys, gue mau bagi nih review tentang toko online buah-sayur yang lagi hits banget! Jujur ya, awalnya ragu mau coba, tapi pas udah pesen, gilaa, barangnya sampe cepet banget! Sayurnya segar bener, kaya abis petik langsung dari kebun. Buahnya juga manis-manis, bener-bener bikin nagih! Pokoknya recomended banget dah buat yang lagi cari belanja online yang oke punya!"</div>
              </div>
            </q-card-section>
          </q-card>
        </div>

      </q-page>
    </q-page-container>

    <q-footer elevated class="footer">
      <q-toolbar>
        <q-toolbar-title>
          @buyur_ami
        </q-toolbar-title>
        <q-space />
        <q-btn flat round dense icon="phone" @click="contactUs" />
      </q-toolbar>
    </q-footer>
  </q-layout>
</template>

<script>
import { ref, watch } from "vue";

export default {
  setup() {
    const slide = ref(1);
    const tab = ref('buah');
    const search = ref('');
    const produk = ref([
  {
    nama: "Apel",
    jenis: "buah",
    warna: "Merah",
    berat: 150,
    rasa: "Manis",
    harga: 10000,
    gambar: "https://images.tokopedia.net/img/cache/700/product-1/2020/6/22/8859906/8859906_f9aca0f9-0c4a-42ef-8983-7b78266271c2_1007_1007.jpg",
    rating: 4.5
  },
  {
    nama: "Jeruk",
    jenis: "buah",
    warna: "Oranye",
    berat: 200,
    rasa: "Segar",
    harga: 15000,
    gambar: "https://images.tokopedia.net/img/cache/700/VqbcmM/2021/2/5/843d0556-9713-4a9e-b64a-ba2df13aee57.png",
    rating: 4.7
  },
  {
    nama: "Pisang",
    jenis: "buah",
    warna: "Kuning",
    berat: 120,
    rasa: "Manis",
    harga: 8000,
    gambar: "https://png.pngtree.com/background/20220720/original/pngtree-exquisite-hanging-display-of-imported-bananas-in-the-fruit-shop-picture-image_1673921.jpg",
    rating: 4.2
  },
  {
    nama: "Mangga",
    jenis: "buah",
    warna: "Hijau",
    berat: 250,
    rasa: "Manis",
    harga: 12000,
    gambar: "https://dukaan.b-cdn.net/700x700/webp/projecteagle/images/6f849299-244e-4df7-bd7a-194720cfc136.jpg",
    rating: 4.6
  },
  {
    nama: "Anggur",
    jenis: "buah",
    warna: "Ungu",
    berat: 300,
    rasa: "Manis",
    harga: 18000,
    gambar: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQyiu5ZlqZrlucTyiQX3-737qwpITlrp7Hrw3G-IfnidA&s",
    rating: 4.4
  },
  {
    nama: "Bayam",
    jenis: "sayuran",
    warna: "Hijau",
    berat: 200,
    rasa: "Segar",
    harga: 5000,
    gambar: "https://images.tokopedia.net/img/cache/700/hDjmkQ/2022/12/6/421a5b77-807b-49a2-80b4-bfc6cdaaa8d6.jpg",
    rating: 4.2
  },
  {
    nama: "Kangkung",
    jenis: "sayuran",
    warna: "Hijau",
    berat: 150,
    rasa: "Segar",
    harga: 4000,
    gambar: "https://assets2.rumah-bumn.id/produk/product-15096010213787.jpg",
    rating: 4.0
  },
  {
    nama: "Tomat",
    jenis: "sayuran",
    warna: "Merah",
    berat: 100,
    rasa: "Asam",
    harga: 6000,
    gambar: "https://images.tokopedia.net/img/cache/700/product-1/2020/6/30/106880639/106880639_89f620fa-949f-40a3-beb9-a5a39183757b_1280_1280",
    rating: 4.3
  },
  {
    nama: "Wortel",
    jenis: "sayuran",
    warna: "Oranye",
    berat: 120,
    rasa: "Manis",
    harga: 7000,
    gambar: "https://images.tokopedia.net/img/cache/700/VqbcmM/2020/12/16/8ef4611d-f571-4458-b31d-c879fd338758.jpg",
    rating: 4.1
  },
  {
    nama: "Brokoli",
    jenis: "sayuran",
    warna: "Hijau",
    berat: 180,
    rasa: "Segar",
    harga: 8000,
    gambar: "https://cdn.pgmall.my/image/cache/catalog/seller_store/store764/product/0355857001658379732brokoliimport1-600x600.jpg",
    rating: 4.5
  },
  {
    nama: "Nanas",
    jenis: "buah",
    warna: "Kuning",
    berat: 1200,
    rasa: "Asam Manis",
    harga: 20000,
    gambar: "https://images.tokopedia.net/img/cache/700/product-1/2020/5/14/534635792/534635792_57f9a1ec-cd38-4d7b-95cd-ca21ffc04a03_918_918.jpg",
    rating: 4.6
  },
  {
    nama: "Strawberi",
    jenis: "buah",
    warna: "Merah",
    berat: 20,
    rasa: "Manis",
    harga: 30000,
    gambar: "https://www.static-src.com/wcsstore/Indraprastha/images/catalog/full//105/MTA-26059649/no-brand_no-brand_full01.jpg",
    rating: 4.8
  },
  {
    nama: "Pepaya",
    jenis: "buah",
    warna: "Oranye",
    berat: 500,
    rasa: "Manis",
    harga: 12000,
    gambar: "https://img.lazcdn.com/g/ff/kf/S94cff52a7cd64004bc7a442b4a2308e3G.jpg_720x720q80.jpg",
    rating: 4.4
  },
  {
    nama: "Bawang Merah",
    jenis: "sayuran",
    warna: "Merah",
    berat: 50,
    rasa: "Pedas",
    harga: 15000,
gambar: "https://images.tokopedia.net/img/cache/500-square/product-1/2019/8/27/5452844/5452844_cc811506-ada3-4ec3-b7b2-2bf94ab4ca13_1500_1500.jpg",
rating: 4.3
},
{
nama: "Labu Siam",
jenis: "sayuran",
warna: "Hijau",
berat: 300,
rasa: "Manis",
harga: 10000,
gambar: "https://img.lazcdn.com/g/p/7e70db1a331a00459fb3f310f2576672.jpg_720x720q80.jpg",
rating: 4.2
},
{
nama: "Terong",
jenis: "sayuran",
warna: "Ungu",
berat: 200,
rasa: "Manis",
harga: 8000,
gambar: "https://images.tokopedia.net/img/cache/700/VqbcmM/2021/3/12/95bb307d-5a9f-4348-8c08-9144d2ae77c9.jpg",
rating: 4.5
}
]);

const drawer = ref(false);
    const filteredProduk = ref([...produk.value]);

    watch([tab, search], () => {
      if (tab.value === 'semua') {
        filteredProduk.value = produk.value.filter(p => p.nama.toLowerCase().includes(search.value.toLowerCase()));
      } else {
        filteredProduk.value = produk.value.filter(p => p.kategori === tab.value && p.nama.toLowerCase().includes(search.value.toLowerCase()));
      }
    });

    const goToPage = (page) => {
      console.log(`Navigasi ke halaman ${page}`);
    };

    const addToCart = (item) => {
      console.log(`Tambah ke keranjang: ${item.nama}`);
    };

    const goToCartPage = () => {
      console.log('Navigasi ke halaman keranjang');
    };

    const goToProfilePage = () => {
      console.log('Navigasi ke halaman profil');
    };

    const filterProduk = () => {
      if (tab.value === 'semua') {
        filteredProduk.value = produk.value.filter(p => p.nama.toLowerCase().includes(search.value.toLowerCase()));
      } else {
        filteredProduk.value = produk.value.filter(p => p.kategori === tab.value && p.nama.toLowerCase().includes(search.value.toLowerCase()));
      }
    };

    const lihatPromo = () => {
      console.log('Navigasi ke halaman promo');
    };

    const contactUs = () => {
      console.log('Hubungi kami');
    };

    return {
      slide,
      tab,
      search,
      produk,
      drawer,
      filteredProduk,
      goToPage,
      addToCart,
      goToCartPage,
      goToProfilePage,
      filterProduk,
      lihatPromo,
      contactUs
    };
  }
};
</script>

<style scoped>
.header {
  background-color: #2E8B57; /* Hijau Laut Tua */
  color: white;
}

.header-btn {
  margin-left: 10px;
  color: white;
}

.header-search {
  margin-left: auto;
  margin-right: 10px;
}

.page-background {
  background-color: #F0FFFF; /* Azure */
}

.carousel img {
  height: 300px;
  object-fit: cover;
}

.tabs {
  background-color: #3CB371; /* Hijau Laut Muda */
  color: white;
}

.tab-panel {
  background-color: #FFFFFF; /* Putih */
  padding: 20px;
}

.produk-card {
  max-width: 300px;
  margin: auto;
  margin-bottom: 20px;
  border: 1px solid #2E8B57; /* Hijau Laut Tua */
}

.product-image {
  height: 150px;
  object-fit: cover;
}

.product-details {
  text-align: center;
}

.add-to-cart-btn {
  margin-top: 10px;
  background-color: #3CB371; /* Hijau Laut Muda */
  color: white;
  transition: background-color 0.3s, color 0.3s;
}

.add-to-cart-btn:hover {
  background-color: #2E8B57; /* Hijau Laut Tua */
  color: white;
}

.promo-banner {
  background-color: #F0FFFF; /* Azure */
  color: #3CB371; /* Hijau Laut Muda */
}

.testimoni-card {
  max-width: 600px;
  margin: auto;
  margin-bottom: 20px;
  border: 1px solid #2E8B57; /* Hijau Laut Tua */
}

.testimoni-section {
  display: flex;
  align-items: center;
}

.testimoni-details {
  margin-left: 15px;
}

.footer {
  background-color: #2E8B57; /* Hijau Laut Tua */
  color: white;
}
</style>
