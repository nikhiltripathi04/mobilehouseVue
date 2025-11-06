<template>
    <NavBar />
    <br>
    <br>
    
  <section class="container mx-auto px-4 py-10">
    <!-- Header -->
    <div class="flex items-end justify-between">
      <div>
        <p class="uppercase tracking-wide opacity-60 text-xs">Browse</p>
        <h1 class="text-3xl md:text-5xl font-semibold">Mobile Phones</h1>
      </div>

      <!-- optional search -->
      <div class="hidden md:block">
        <input
          v-model.trim="q"
          type="text"
          placeholder="Search models…"
          class="border rounded-full px-4 py-2 text-sm outline-none focus:ring-2 focus:ring-black"
        />
      </div>
    </div>

    <!-- Brand Tabs -->
    <div class="mt-6 overflow-x-auto">
      <div class="flex gap-2 md:gap-3">
        <button
          v-for="brand in brands"
          :key="brand"
          @click="selectBrand(brand)"
          class="px-4 py-2 rounded-full border text-sm whitespace-nowrap transition"
          :class="brand === selectedBrand ? 'bg-black text-white border-black' : 'hover:bg-black hover:text-white'"
        >
          {{ brand }}
          <span class="opacity-70 ml-1">({{ countByBrand(brand) }})</span>
        </button>
      </div>
    </div>

    <!-- Results -->
    <div class="mt-8 grid sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
      <article
        v-for="m in filtered"
        :key="m.name"
        class="rounded-2xl border overflow-hidden group bg-white flex flex-col"
      >
        <img :src="m.image" :alt="m.name" class="w-full h-56 object-cover" loading="lazy" />
        <div class="p-4 grow flex flex-col">
          <p class="text-xs opacity-60 mb-1">{{ m.brand }}</p>
          <h3 class="text-lg lg:text-2xl leading-snug">{{ m.name }}</h3>
          <div class="mt-auto pt-4">
          </div>
        </div>
      </article>
    </div>

    <!-- Empty state -->
    <div v-if="filtered.length === 0" class="text-center py-16 opacity-70">
      No results for “{{ q }}” in {{ selectedBrand }}.
    </div>
  </section>
  <Footer />
</template>

<script setup lang="ts">
import Footer from '@/components/Footer.vue'
import NavBar from '@/components/NavBar.vue'
import { computed, onMounted, ref, watch } from 'vue'
import { useRoute, useRouter } from 'vue-router'

type Mobile = {
  name: string
  image: string
  brand: string
}

const topMobiles: Mobile[] = [
  { name: "iPhone 16 Pro Max", image: "https://i.gadgets360cdn.com/products/large/iphone-16-pro-max-634x800-1725909635.jpg?downsize=*:420&output-quality=80", brand: "Apple" },
  { name: "iPhone 16 Pro", image: "https://i.gadgets360cdn.com/products/large/iphone-16-pro-641x800-1725909596.jpg?downsize=*:420&output-quality=80", brand: "Apple" },
  { name: "iPhone 16 SE", image: "https://i.gadgets360cdn.com/products/large/iphone-16e-651x800-1739982268.jpg?downsize=*:420&output-quality=80", brand: "Apple" },
  { name: "iPhone 16", image: "https://i.gadgets360cdn.com/products/large/iphone-16-650x800-1725909609.jpg?downsize=*:420&output-quality=80", brand: "Apple" },
  { name: "iPhone 16 Plus", image: "https://i.gadgets360cdn.com/products/large/iphone-16-plus-642x800-1725909623.jpg?downsize=*:420&output-quality=80", brand: "Apple" },
  { name: "Samsung Fold 7", image: "https://i.gadgets360cdn.com/products/large/Samsung-Galaxy-Z-Fold-7-db-700x790-1752071871.jpg?downsize=*:420&output-quality=80", brand: "Samsung" },
  { name: "Samsung Flip 7", image: "https://m.media-amazon.com/images/I/61ZtVMmiwML._UF894,1000_QL80_.jpg", brand: "Samsung" },
]

const moreMobiles: Mobile[] = [
  // APPLE
  { name: "iPhone 13 Pro Max", image: "https://i.gadgets360cdn.com/products/large/iphone-13-pro-max-400x800-1631648541.jpg?downsize=*:420&output-quality=80", brand: "Apple" },
  { name: "iPhone 13 Pro", image: "https://i.gadgets360cdn.com/products/large/iphone-13-pro-400x800-1631648140.jpg?downsize=*:420&output-quality=80", brand: "Apple" },
  { name: "iPhone 13 Mini", image: "https://i.gadgets360cdn.com/products/large/iphone-13-mini-1-396x800-1631648715.jpg?downsize=*:420&output-quality=80", brand: "Apple" },
  { name: "iPhone 13", image: "https://i.gadgets360cdn.com/products/large/iphone-13-1-396x800-1631648728.jpg?downsize=*:420&output-quality=80", brand: "Apple" },
  { name: "iPhone 15 Pro Max", image: "https://i.gadgets360cdn.com/products/large/iphone-15-pro-600x800-1694549264.jpg?downsize=*:420&output-quality=80", brand: "Apple" },
  { name: "iPhone 15 Plus", image: "https://i.gadgets360cdn.com/products/large/iphone-15-plus-553x800-1694549239.jpg?downsize=*:420&output-quality=80", brand: "Apple" },
  { name: "iPhone 15", image: "https://i.gadgets360cdn.com/products/large/iphone-15-plus-553x800-1694549227.jpg?downsize=*:420&output-quality=80", brand: "Apple" },
  // SAMSUNG
  { name: "Samsung S25 Edge", image: "https://i.gadgets360cdn.com/products/large/samsung-galaxy-s25-edge-db-653x800-1747113807.jpg?downsize=*:420&output-quality=80", brand: "Samsung" },
  { name: "Samsung S25 Ultra", image: "https://i.gadgets360cdn.com/products/large/samsung-galaxy-s25-ultra-795x800-1738321292.jpg?downsize=*:420&output-quality=80", brand: "Samsung" },
  { name: "Samsung S25", image: "https://i.gadgets360cdn.com/products/large/galaxy-s25-657x800-1737554203.jpg?downsize=*:420&output-quality=80", brand: "Samsung" },
  { name: "Samsung Galaxy F36 5G", image: "https://i.gadgets360cdn.com/products/large/Galaxy-f36-5g-240x180-1752912328.jpg?downsize=*:420&output-quality=80", brand: "Samsung" },
  { name: "Samsung Galaxy M36 5G", image: "https://i.gadgets360cdn.com/products/large/Galaxy-M36-5G-DB-709x800-1751029368.jpg?downsize=*:420&output-quality=80", brand: "Samsung" },
  { name: "Samsung Galaxy M56 5G", image: "https://i.gadgets360cdn.com/products/large/galaxy-m56-5g-samsung-db-654x800-1744872507.jpg?downsize=*:420&output-quality=80", brand: "Samsung" },
  { name: "Samsung Galaxy F16 5G", image: "https://i.gadgets360cdn.com/products/large/galaxy-f16-5g-samsung-db-639x800-1741860451.jpg?downsize=*:420&output-quality=80", brand: "Samsung" },
  { name: "Samsung Galaxy A26 5G", image: "https://i.gadgets360cdn.com/products/large/samsung-galaxy-a26-5g-606x800-1740913935.jpg?downsize=*:420&output-quality=80", brand: "Samsung" },
  { name: "Samsung Galaxy A36 5G", image: "https://i.gadgets360cdn.com/products/large/samsung-galaxy-a36-5g-617x800-1740914965.jpg?downsize=*:420&output-quality=80", brand: "Samsung" },
  { name: "Samsung Galaxy A56 5G", image: "https://i.gadgets360cdn.com/products/large/samsung-galaxy-a56-5g-612x800-1740913945.jpg?downsize=*:420&output-quality=80", brand: "Samsung" },
  // ONEPLUS
  { name: "OnePlus CE5", image: "https://i.gadgets360cdn.com/products/large/CE5-1-DB-373x800-1751917851.jpg?downsize=*:420&output-quality=80", brand: "OnePlus" },
  { name: "OnePlus Nord 5", image: "https://i.gadgets360cdn.com/products/large/OnePlus-Nord-5-DB-378x800-1751917676.jpg?downsize=*:420&output-quality=80", brand: "OnePlus" },
  { name: "OnePlus 13s", image: "https://i.gadgets360cdn.com/products/large/oneplus-13s-646x800-1749107425.jpg?downsize=*:420&output-quality=80", brand: "OnePlus" },
  { name: "OnePlus 13", image: "https://i.gadgets360cdn.com/products/large/oneplus-13-630x800-1730372404.jpg?downsize=*:420&output-quality=80", brand: "OnePlus" },
  { name: "OnePlus 13R", image: "https://i.gadgets360cdn.com/products/large/oneplus-13r-658x800-1736267136.jpg?downsize=*:420&output-quality=80", brand: "OnePlus" },
  // NOTHING
  { name: "Nothing Phone 3", image: "https://i.gadgets360cdn.com/products/large/Nothing-Phone-3-press-1200x675-1751391432.jpg?downsize=*:420&output-quality=80", brand: "Nothing" },
  { name: "CMF Phone 2 Pro", image: "https://i.gadgets360cdn.com/products/large/CMF-Phone-2-Pro-DB-709x800-1745852035.jpg?downsize=*:420&output-quality=80", brand: "Nothing" },
  { name: "Nothing Phone 3A", image: "https://i.gadgets360cdn.com/products/large/nothing-phone-3a-nothing-db-660x800-1741093567.jpg?downsize=*:420&output-quality=80", brand: "Nothing" },
  { name: "Nothing Phone 3A Pro", image: "https://i.gadgets360cdn.com/products/large/phone-3a-pro-nothing-db-388x800-1741093526.jpg?downsize=*:420&output-quality=80", brand: "Nothing" },
  { name: "Nothing Phone 2A", image: "https://i.gadgets360cdn.com/products/large/Phone-2a-DB-709x800-1709698950.jpg?downsize=*:420&output-quality=80", brand: "Nothing" },
  { name: "Nothing Phone 2", image: "https://i.gadgets360cdn.com/products/large/nothing-phone-2-676x800-1689130263.jpg?downsize=*:420&output-quality=80", brand: "Nothing" },
  // OPPO
  { name: "Oppo Reno 14", image: "https://i.gadgets360cdn.com/products/large/oppo-reno-14-oppo-db-666x800-1747309893.jpg?downsize=*:420&output-quality=80", brand: "Oppo" },
  { name: "Oppo K13x 5G", image: "https://i.gadgets360cdn.com/products/large/oppo-k13x-5g-oppo-db-647x800-1750660848.jpg?downsize=*:420&output-quality=80", brand: "Oppo" },
  { name: "Oppo A5", image: "https://i.gadgets360cdn.com/products/large/Oppo-A5-709x800-1747404807.jpg?downsize=*:420&output-quality=80", brand: "Oppo" },
  { name: "Oppo A5X", image: "https://i.gadgets360cdn.com/products/large/Oppo-A5X-709x800-1747404950.jpg?downsize=*:420&output-quality=80", brand: "Oppo" },
  { name: "Oppo Reno 14 Pro", image: "https://i.gadgets360cdn.com/products/large/oppo-reno-14-pro-oppo-db-700x800-1747310025.jpg?downsize=*:420&output-quality=80", brand: "Oppo" },
  { name: "Oppo A5 Pro 5G", image: "https://i.gadgets360cdn.com/products/large/oppo-a5-pro-5g-oppo-db-1-646x800-1745474483.jpg?downsize=*:420&output-quality=80", brand: "Oppo" },
  { name: "Oppo Find X8 Ultra", image: "https://i.gadgets360cdn.com/products/large/Oppo-Find-X8-Ultra-DB-709x800-1744301285.jpg?downsize=*:420&output-quality=80", brand: "Oppo" },
  { name: "Oppo F29 Pro 5G", image: "https://i.gadgets360cdn.com/products/large/oppo-f29-pro-5g-oppo-db-642x800-1742451547.jpg?downsize=*:420&output-quality=80", brand: "Oppo" },
  { name: "Oppo F29 5G", image: "https://i.gadgets360cdn.com/products/large/oppo-f29-5g-oppo-db-636x800-1742451502.jpg?downsize=*:420&output-quality=80", brand: "Oppo" },
  { name: "Oppo Reno 13", image: "https://i.gadgets360cdn.com/products/large/reno-13-db-645x800-1732623713.jpg?downsize=*:420&output-quality=80", brand: "Oppo" },
  { name: "Oppo Find X8 Pro", image: "https://i.gadgets360cdn.com/products/large/oppo-find-x8-pro-db-660x800-1732173841.jpg?downsize=*:420&output-quality=80", brand: "Oppo" },
  { name: "Oppo F27 5G", image: "https://i.gadgets360cdn.com/products/large/oppo-f27-5g-643x800-1724152051.jpg?downsize=*:420&output-quality=80", brand: "Oppo" },
  { name: "Oppo A3 5G", image: "https://i.gadgets360cdn.com/products/large/oppo-a3-5g-641x800-1724069314.jpg?downsize=*:420&output-quality=80", brand: "Oppo" },
  // VIVO
  { name: "Vivo T4R 5G", image: "https://i.gadgets360cdn.com/products/large/vivo-t4r-5g-vivo-db-719x800-1753945214.jpg?downsize=*:420&output-quality=80", brand: "Vivo" },
  { name: "Vivo Y50m 5G", image: "https://i.gadgets360cdn.com/products/large/Vivo-Y50m-5G-DB-709x800-1753100795.jpg?downsize=*:420&output-quality=80", brand: "Vivo" },
  { name: "Vivo Y50 5G", image: "https://i.gadgets360cdn.com/products/large/Vivo-Y50-5G-DB-709x800-1753100858.jpg?downsize=*:420&output-quality=80", brand: "Vivo" },
  { name: "Vivo X Fold5", image: "https://i.gadgets360cdn.com/products/large/vivo-xfold5-vivo-db-654x800-1752479620.jpg?downsize=*:420&output-quality=80", brand: "Vivo" },
  { name: "Vivo T4 Lite 5G", image: "https://i.gadgets360cdn.com/products/large/vivo-t4-lite-5g-vivo-db-610x800-1750747604.jpg?downsize=*:420&output-quality=80", brand: "Vivo" },
  { name: "Vivo X200 FE", image: "https://i.gadgets360cdn.com/products/large/Vivo-X200-FE-DB-709x800-1750682393.jpg?downsize=*:420&output-quality=80", brand: "Vivo" },
  { name: "Vivo Y400 Pro 5G", image: "https://i.gadgets360cdn.com/products/large/vivo-y400-pro-5g-vivo-db-696x800-1750401406.jpg?downsize=*:420&output-quality=80", brand: "Vivo" },
  { name: "Vivo T4 Ultra", image: "https://i.gadgets360cdn.com/products/large/vivo-t4-ultra-vivo-db-669x800-1749623906.jpg?downsize=*:420&output-quality=80", brand: "Vivo" },
  { name: "Vivo S30", image: "https://i.gadgets360cdn.com/products/large/vivo-S30-db-709x800-1748528113.jpg?downsize=*:420&output-quality=80", brand: "Vivo" },
  { name: "Vivo Y19 5G", image: "https://i.gadgets360cdn.com/products/large/vivo-y19-5g-db-672x800-1746085349.jpg?downsize=*:420&output-quality=80", brand: "Vivo" },
  { name: "Vivo Y300i 5G", image: "https://i.gadgets360cdn.com/products/large/Vivo-Y300i-5G-DB-709x800-1743170854.jpg?downsize=*:420&output-quality=80", brand: "Vivo" },
  { name: "Vivo V50", image: "https://i.gadgets360cdn.com/products/large/vivo-v50-vivo-db-684x800-1739776991.jpg?downsize=*:420&output-quality=80", brand: "Vivo" },
  { name: "Vivo S18", image: "https://i.gadgets360cdn.com/products/large/vivo-s18-649x800-1702640543.jpg?downsize=*:420&output-quality=80", brand: "Vivo" },
  // XIAOMI
  { name: "Xiaomi Mix Flip 2", image: "https://i.gadgets360cdn.com/products/large/Xiaomi-Mix-Flip-2-DB-709x800-1750941643.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Xiaomi Civi 5 Pro", image: "https://i.gadgets360cdn.com/products/large/xiaomi-civi-5-pro-xiaomi-db-648x800-1747977993.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Xiaomi 15S Pro", image: "https://i.gadgets360cdn.com/products/large/xiaomi-15s-pro-xiaomi-db-1-611x800-1747973807.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Xiaomi 15 Ultra", image: "https://i.gadgets360cdn.com/products/large/xiaomi-15-ultra-db-642x800-1740720580.png?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Xiaomi 14T", image: "https://i.gadgets360cdn.com/products/large/Xiaomi-14T-709x800-1727417081.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Xiaomi 14T DB", image: "https://i.gadgets360cdn.com/products/large/Xiaomi-14T-DB-709x800-1727416306.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Redmi Note 14 SE 5G", image: "https://i.gadgets360cdn.com/products/large/redmi-note-14-se-5g-xiaomi-db-644x800-1753685901.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Redmi K80 Ultra", image: "https://i.gadgets360cdn.com/products/large/redmi-k80-ultra-290x800-1750947729.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Redmi Turbo 4 Pro", image: "https://i.gadgets360cdn.com/products/large/redmi-turbo-4-pro-xiaomi-db-634x800-1745499832.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Redmi A5", image: "https://i.gadgets360cdn.com/products/large/Redmi-A5-709x800-1744727607.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Redmi Note 14S", image: "https://i.gadgets360cdn.com/products/large/redmi-note-14s-641x800-1742021471.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Redmi K80 Pro", image: "https://i.gadgets360cdn.com/products/large/redmi-k80-pro-1-db-1495x800-1732879543.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Redmi A4 5G", image: "https://i.gadgets360cdn.com/products/large/redmi-a4-5g-db-648x800-1732087838.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Redmi Note 14 Pro Plus 5G", image: "https://i.gadgets360cdn.com/products/large/Redmi-Note-14-Pro-Plus-5g-DB-709x800-1750854642.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  { name: "Redmi Note 14 Pro 5G", image: "https://i.gadgets360cdn.com/products/large/Redmi-Note-14-Pro-5g-db-709x800-1750854700.jpg?downsize=*:420&output-quality=80", brand: "Xiaomi" },
  // REALME
  { name: "Realme 15", image: "https://i.gadgets360cdn.com/products/large/realme-15-realme-db-637x800-1753357676.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  { name: "Realme 15 Pro", image: "https://i.gadgets360cdn.com/products/large/realme-15-pro-realme-db-641x800-1753357983.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  { name: "Realme C71 5G", image: "https://i.gadgets360cdn.com/products/large/Realme-c71-5g-realme-db-643x800-1752567551.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  { name: "Realme Narzo 80 Lite 5G", image: "https://i.gadgets360cdn.com/products/large/realme-narzo-80-lite-5g-realme-db-668x800-1750053327.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  { name: "Realme C71 DB", image: "https://i.gadgets360cdn.com/products/large/Realme-C71-DB-709x800-1748929467.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  { name: "Realme C73 5G", image: "https://i.gadgets360cdn.com/products/large/realme-c73-5g-realme-db-643x800-1748846447.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  { name: "Realme GT 7", image: "https://i.gadgets360cdn.com/products/large/realme-gt7-realme-db-652x800-1745402417.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  { name: "Realme C75 5G", image: "https://i.gadgets360cdn.com/products/large/realme-c75-5g-realme-db-659x800-1746450927.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  { name: "Realme GT 7 Dream Edition", image: "https://i.gadgets360cdn.com/products/large/Realme-GT-7-Dream-Edition-db-709x800-1748341234.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  { name: "Realme 14T 5G", image: "https://i.gadgets360cdn.com/products/large/realme-14t-5g-realme-db-644x800-1745563844.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  { name: "Realme Narzo 80X 5G", image: "https://i.gadgets360cdn.com/products/large/realme-narzo-80x-5g-realme-db-661x800-1744201287.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  { name: "Realme Narzo 80 Pro 5G", image: "https://i.gadgets360cdn.com/products/large/realme-narzo-80-pro-5g-realme-db-676x800-1744201606.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  { name: "Realme P3 5G", image: "https://i.gadgets360cdn.com/products/large/realme-p3-5g-realme-db-651x800-1742394454.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  { name: "Realme P3 Ultra 5G", image: "https://i.gadgets360cdn.com/products/large/Realme-P3-Ultra-5G-db-657x800-1742394595.jpg?downsize=*:420&output-quality=80", brand: "Realme" },
  // MOTOROLA
  { name: "Moto G86 Power 5G", image: "https://i.gadgets360cdn.com/products/large/moto-g86-power-5g-moto-db-527x800-1753857510.jpg?downsize=*:420&output-quality=80", brand: "Motorola" },
  { name: "Moto G86", image: "https://i.gadgets360cdn.com/products/large/motorola-g86-db-376x800-1753270703.jpg?downsize=*:420&output-quality=80", brand: "Motorola" },
  { name: "Motorola Edge 2025", image: "https://i.gadgets360cdn.com/products/large/Motorola-Edge-2025-DB-709x800-1748443250.jpg?downsize=*:420&output-quality=80", brand: "Motorola" },
  { name: "Motorola Razr 60", image: "https://i.gadgets360cdn.com/products/large/motorola-razr-60-db-349x800-1745562204.jpg?downsize=*:420&output-quality=80", brand: "Motorola" },
  { name: "Motorola Edge 60", image: "https://i.gadgets360cdn.com/products/large/motorola-edge-60-db-575x800-1745563678.jpg?downsize=*:420&output-quality=80", brand: "Motorola" },
  // GOOGLE
  { name: "Google Pixel 9a", image: "https://i.gadgets360cdn.com/products/large/google-pixel-9a-380x800-1742395533.jpg?downsize=*:420&output-quality=80", brand: "Google" },
  { name: "Google Pixel 9", image: "https://i.gadgets360cdn.com/products/large/Pixel-9-DB-709x800-1723555719.jpg?downsize=*:420&output-quality=80", brand: "Google" },
  { name: "Google Pixel 9 Pro", image: "https://i.gadgets360cdn.com/products/large/Pixel-9-Pro-DB-709x800-1723607993.jpg?downsize=*:420&output-quality=80", brand: "Google" },
  { name: "Google Pixel 9 Pro XL", image: "https://i.gadgets360cdn.com/products/large/Pixel-9-Pro-XL-1-709x800-1723558086.jpg?downsize=*:420&output-quality=80", brand: "Google" },
  { name: "Google Pixel 9 Pro Fold", image: "https://i.gadgets360cdn.com/products/large/pixel-9-pro-fold-db-778x800-1723562534.jpg?downsize=*:420&output-quality=80", brand: "Google" },
]

const all = computed<Mobile[]>(() => [...topMobiles, ...moreMobiles])

const router = useRouter()
const route = useRoute()

const brands = computed(() => {
  const set = new Set(all.value.map(m => m.brand))
  return Array.from(set).sort()
})

const selectedBrand = ref<string>('Apple')
const q = ref('')

const filtered = computed(() => {
  return all.value
    .filter(m => m.brand === selectedBrand.value)
    .filter(m => (q.value ? m.name.toLowerCase().includes(q.value.toLowerCase()) : true))
})

function countByBrand(brand: string) {
  return all.value.filter(m => m.brand === brand).length
}
function selectBrand(brand: string) {
  selectedBrand.value = brand
}

onMounted(() => {
  // Initialize from ?brand= in URL if present
  const incoming = (route.query.brand as string) || ''
  if (incoming && brands.value.includes(incoming)) {
    selectedBrand.value = incoming
  } else {
    // keep default (Apple) or first brand
    if (!brands.value.includes(selectedBrand.value) && brands.value.length) {
      selectedBrand.value = brands.value[0]
    }
  }
})

// keep URL in sync when brand changes
watch(selectedBrand, (b) => {
  router.replace({ query: { ...route.query, brand: b } })
})
</script>

<style scoped>
/* optional: consistent heading/desc utilities */
</style>
