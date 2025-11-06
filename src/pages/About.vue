<template>
  <div class="w-screen overflow-x-hidden">
    <!-- HERO -->
    <section class="container mx-auto px-4 pt-24 md:pt-28 pb-10">
      <div class="relative overflow-hidden rounded-3xl border bg-white">
        <!-- gradient/shape background -->
        <div aria-hidden="true" class="pointer-events-none absolute inset-0">
          <div class="absolute -left-20 -top-20 h-72 w-72 rounded-full opacity-20 blur-3xl"
               style="background: radial-gradient(60% 60% at 50% 50%, #E20015 0%, rgba(226,0,21,0) 70%);"></div>
          <div class="absolute -right-16 -bottom-16 h-72 w-72 rounded-full opacity-20 blur-3xl"
               style="background: radial-gradient(60% 60% at 50% 50%, #000 0%, rgba(0,0,0,0) 70%);"></div>
        </div>

        <div class="relative grid lg:grid-cols-2 gap-8 items-center p-6 md:p-10">
          <div>
            <p class="tagline">premium phones • honest advice</p>
            <h1 class="title mt-2">Your trusted Premium Mobile Devices Destination.</h1>
            <p class="desc mt-4 text-sm md:text-base">
              Experience the future of mobile tech with a curated collection, expert guidance, and
              unbeatable prices in INR. Your perfect device <span class="font-semibold">awaits.</span>
            </p>

            <!-- CTA buttons -->
            <div class="mt-6 flex flex-wrap gap-3">
              <RouterLink to="/mobile"
                class="btn-primary">Shop phones</RouterLink>
              <a href="#contact" class="btn-ghost">Trade-in</a>
            </div>

            <!-- Trust badges -->
            <ul class="mt-6 flex flex-wrap gap-4 text-xs md:text-sm">
              <li class="badge"><i class="las la-shield-alt text-base"></i> 1-Year Warranty Options</li>
              <li class="badge"><i class="las la-rupee-sign text-base"></i> Best Prices in INR</li>
              <li class="badge"><i class="las la-shipping-fast text-base"></i> Fast Delivery</li>
            </ul>
          </div>

          <!-- Brand visual -->
          <div class="relative">
            <div class="aspect-[4/3] md:aspect-[5/4] rounded-2xl border bg-white/70 backdrop-blur center p-6">
              <img src="/images/mhlogo.jpg" alt="Mobile House"
                   class="max-h-64 md:max-h-80 w-auto object-contain mx-auto select-none"
                   loading="lazy" />
            </div>
            <!-- floating card -->
            <div class="hidden md:block absolute -bottom-4 -left-4 rounded-xl border bg-white px-4 py-3 shadow-sm">
              <p class="text-xs opacity-70">Top brands</p>
              <p class="text-sm font-medium">Apple • Samsung • OnePlus • Pixel</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- WHY CHOOSE US -->
    <section class="container mx-auto px-4 pb-8">
      <div class="rounded-3xl border bg-white p-6 md:p-10">
        <div class="grid md:grid-cols-2 gap-8 md:gap-10">
          <div>
            <h2 class="title">Why choose Mobile House?</h2>
            <p class="desc mt-3 text-sm md:text-base">
              With over a decade of excellence, Mobile House has become India’s most trusted
              destination for premium smartphones, accessories, and expert mobile solutions.
            </p>

            <!-- stats -->
            <div class="mt-6 grid grid-cols-3 gap-3">
              <div class="stat">
                <span class="stat-num">10K+</span>
                <span class="stat-label">Happy Customers</span>
              </div>
              <div class="stat">
                <span class="stat-num">500+</span>
                <span class="stat-label">Models in Stock</span>
              </div>
              <div class="stat">
                <span class="stat-num">4.8★</span>
                <span class="stat-label">Average Rating</span>
              </div>
            </div>
          </div>

          <!-- feature cards -->
          <ul class="grid sm:grid-cols-2 gap-4">
            <li v-for="f in features" :key="f.title" class="feature-card">
              <div class="flex items-start gap-3">
                <div class="icon">
                  <i :class="f.icon"></i>
                </div>
                <div>
                  <p class="feature-title">{{ f.title }}</p>
                  <p class="feature-desc">{{ f.desc }}</p>
                </div>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </section>

    <!-- ACTION STRIP -->
    <section class="container mx-auto px-4 pb-16">
      <div class="rounded-2xl border p-5 md:p-8 flex flex-col md:flex-row gap-4 md:items-center md:justify-between bg-white">
        <div>
          <h3 class="text-lg md:text-2xl font-semibold">Ready to upgrade?</h3>
          <p class="text-sm opacity-80">Browse by brand, compare specs, and pick your perfect phone.</p>
        </div>
        <div class="flex gap-3">
          <RouterLink to="/mobile" class="btn-primary">Browse Mobiles</RouterLink>
          <a href="#contact" class="btn-ghost">Contact us</a>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
// no external imports required
const features = [
  {
    icon: 'las la-tools text-xl',
    title: 'Expert Guidance',
    desc: 'Get the right phone for photography, gaming, or work — no fluff.'
  },
  {
    icon: 'las la-sync-alt text-xl',
    title: 'Easy Trade-in',
    desc: 'Instant valuations and seamless upgrades at fair prices.'
  },
  {
    icon: 'las la-shield-alt text-xl',
    title: 'Warranty & Support',
    desc: 'Authorized accessories and reliable after-sales assistance.'
  },
  {
    icon: 'las la-tags text-xl',
    title: 'Transparent Pricing',
    desc: 'Clear INR pricing with seasonal offers and bundle deals.'
  }
]
</script>

<style scoped>
/* Typographic utilities (reuse your style language) */
.title { @apply text-3xl md:text-5xl font-semibold; }
.tagline { @apply uppercase tracking-wide opacity-70 text-xs md:text-sm; }
.desc { @apply leading-relaxed; }
.center { display: grid; place-items: center; }

/* Buttons */
.btn-primary { @apply bg-black text-white rounded-full px-5 py-2 text-sm hover:opacity-90 transition; }
.btn-ghost { @apply border border-black rounded-full px-5 py-2 text-sm hover:bg-black hover:text-white transition; }

/* Badges */
.badge { @apply inline-flex items-center gap-2 rounded-full border px-3 py-1 bg-white; }

/* Stats */
.stat { @apply rounded-xl border bg-white p-4 text-center; }
.stat-num { @apply text-xl md:text-2xl font-semibold; }
.stat-label { @apply block text-[11px] md:text-xs opacity-70 mt-0.5 uppercase tracking-wide; }

/* Feature cards */
.feature-card { @apply rounded-xl border bg-white p-4 hover:shadow-sm transition; }
.icon { @apply h-9 w-9 rounded-lg border grid place-items-center; }
.feature-title { @apply font-semibold; }
.feature-desc { @apply text-sm opacity-80 mt-1; }

/* Image sizing */
.juice img { max-height: 80vh; }
</style>
