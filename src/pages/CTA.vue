<template>
  <section id="contact" class="container mx-auto px-4 py-24">
    <div class="rounded-[28px] border bg-white/80 backdrop-blur shadow-sm overflow-hidden">
      <div class="grid lg:grid-cols-2">

        <!-- LEFT: STORE INFO -->
        <div class="p-10 lg:p-14 flex flex-col justify-center gap-8">
          <header>
            <h2 class="text-4xl md:text-5xl font-semibold text-logo-red leading-tight">Get in touch</h2>
            <p class="text-gray-600 mt-3 text-base leading-relaxed">
              Need help choosing a phone, trading in your old one, or checking availability?
              We reply <span class="font-medium">quickly</span>.
            </p>
          </header>

          <ul class="space-y-4 text-gray-800 text-base">
            <li class="flex items-start gap-3">
              <i class="las la-map-marker-alt text-2xl text-logo-red"></i>
              <span>{{ addressLine1 }}<br>{{ addressLine2 }}</span>
            </li>
            <li class="flex items-center gap-3">
              <i class="las la-phone text-2xl text-logo-red"></i>
              <a :href="`tel:${phone}`" class="hover:text-logo-red transition">{{ phone }}</a>
            </li>
            <li class="flex items-center gap-3">
              <i class="lab la-whatsapp text-2xl text-green-600"></i>
              <a :href="`https://wa.me/${whatsAppE164}`" target="_blank" class="hover:text-logo-red transition">
                WhatsApp us
              </a>
            </li>
            <li class="flex items-center gap-3">
              <i class="las la-envelope text-2xl text-logo-red"></i>
              <a :href="`mailto:${email}`" class="hover:text-logo-red transition">{{ email }}</a>
            </li>
            <li class="flex items-start gap-3">
              <i class="las la-clock text-2xl text-logo-red"></i>
              <span>{{ hours }}</span>
            </li>
          </ul>

          <div class="flex flex-wrap gap-3 pt-3">
            <a :href="directionsUrl" target="_blank" class="btn-outline">Get directions</a>
            <a :href="`mailto:${email}`" class="btn-outline">Email us</a>
          </div>
        </div>

        <!-- RIGHT: FORM + MAP -->
        <div class="bg-gray-50 border-l flex flex-col gap-8 p-8 lg:p-10">
          
          <!-- FORM -->
          <form @submit.prevent="onSubmit" class="bg-white border rounded-2xl p-6 lg:p-7 space-y-5 shadow-sm">
            <div class="grid md:grid-cols-2 gap-4">
              <div>
                <label class="label">Name</label>
                <input v-model="form.name" required class="input" placeholder="Your name" />
              </div>
              <div>
                <label class="label">Phone</label>
                <input v-model="form.phone" required class="input" placeholder="+91 98xxxxxxx" />
              </div>
            </div>

            <div>
              <label class="label">Email (optional)</label>
              <input v-model="form.email" class="input" placeholder="you@example.com" />
            </div>

            <div>
              <label class="label">Message</label>
              <textarea v-model="form.message" required rows="4" class="input"
                placeholder="Tell us what you need…"></textarea>
            </div>

            <!-- ANTI-SPAM -->
            <input v-model="form.hpt" type="text" class="hidden" />

            <div class="flex items-center gap-4 pt-2">
              <button type="submit" class="btn-primary" :disabled="submitting">
                {{ submitting ? 'Sending…' : 'Send message' }}
              </button>
              <p v-if="status" class="text-sm"
                :class="status.type === 'error' ? 'text-red-600' : 'text-green-600'">
                {{ status.text }}
              </p>
            </div>
          </form>

          <!-- MAP -->
          <iframe
            :src="mapEmbedUrl"
            class="w-full h-64 rounded-xl border shadow-sm"
            loading="lazy"
            allowfullscreen
            referrerpolicy="no-referrer-when-downgrade"
          ></iframe>

        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { computed, reactive, ref } from 'vue'
const props = withDefaults(defineProps<{ addressLine1?:string; addressLine2?:string; email?:string; phone?:string; whatsapp?:string; hours?:string; lat?:number; lng?:number; placeQuery?:string; submitEndpoint?:string }>(), {
  addressLine1: 'Mobile House, Hazratganj',
  addressLine2: 'Lucknow, Uttar Pradesh 226001',
  email: 'mobilehousemhnoida@gmail.com',
  phone: '+91 9953335567',
  whatsapp: '+91 9953335567',
  hours: 'Mon–Sun, 10 AM – 8 PM',
  placeQuery: 'Mobile House Hazratganj',
  submitEndpoint: '/api/contact'
})
const whatsAppE164 = computed(() => props.whatsapp.replace(/\D/g, ''))
const directionsUrl = computed(() => props.lat && props.lng ? `https://www.google.com/maps/dir/?api=1&destination=${props.lat},${props.lng}` : `https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(props.placeQuery)}`)
const mapEmbedUrl = computed(() => props.lat && props.lng ? `https://www.google.com/maps?q=${props.lat},${props.lng}&z=15&output=embed` : `https://www.google.com/maps?q=${encodeURIComponent(props.placeQuery)}&z=15&output=embed`)
const form = reactive({ name:'', phone:'', email:'', message:'', hpt:'' })
const submitting = ref(false)
const status = ref(null)
async function onSubmit() {
  status.value = null
  if (form.hpt) return (status.value = { type:'ok', text:'Thanks!' })
  submitting.value = true
  try {
    const res = await fetch(props.submitEndpoint,{ method:'POST', headers:{ 'Content-Type': 'application/json' }, body: JSON.stringify(form) })
    if (!res.ok) throw new Error()
    status.value = { type:'ok', text:'Message sent!' }
    Object.assign(form,{ name:'', phone:'', email:'', message:'' })
  } catch { status.value = { type:'error', text:'Could not send. Try again.' } }
  submitting.value = false
}
</script>

<style scoped>
.text-logo-red { color:#E20015; }

.label { @apply text-xs font-medium text-gray-600 mb-1; }
.input { @apply w-full border rounded-lg px-3 py-2 outline-none; @apply focus:ring-2 focus:ring-[#E20015] focus:border-[#E20015]; }

.btn-primary { @apply bg-[#E20015] text-white rounded-full px-6 py-2 text-sm font-medium hover:bg-red-700 transition disabled:opacity-60; }
.btn-outline { @apply border border-[#E20015] text-[#E20015] rounded-full px-5 py-2 text-sm hover:bg-[#E20015] hover:text-white transition; }

.title { @apply font-semibold; }
</style>
