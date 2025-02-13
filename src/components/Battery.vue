<script setup>
  import {ref, onMounted, computed, watch} from 'vue'
  import {useBattery} from '@vueuse/core'
  const {charging, chargingTime, level}= useBattery()
  import gsap from 'gsap'
  const battery= ref(null)
  const batteryPercent= ref(0)
  const icon= ref(null)
  const isCharging= computed(()=>{
    return charging.value == false ? 'Tidak' : 'Ya'
  })
  const charge = (value)=>{
    const finalValue= value* 100
    gsap.to(battery.value, {
      width: `${finalValue}%`
    })
  }
  
  watch(charging, (val)=>{
    if (val== false){
      icon.value.attributes.fill.value= "#2563EA"
      return
    }else{
      icon.value.attributes.fill.value= "#23C55E"
    }
  })

  
  onMounted(async ()=>{
    setTimeout(()=>{
      charge(level.value)
      batteryPercent.value= 100* level.value
    }, 500)
  })
  
  
</script>

<template>
  <div class="text-white bg-blue-950 p-6 px-8 rounded-3xl border-8 border-sky-400">
    <div class="flex gap-3 items-center justify-around">
      <div class="flex flex-col">
        <p class="text-5xl font-bold">{{batteryPercent}}%</p>
        <p class="text-xs text-gray-300 ">Kekuatan baterai</p>
      </div>
      <div class="flex items-center justify-center p-1 rounded-full bg-blue-900">
        <svg ref="icon" fill="#2563EA" width="40px" height="40px" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M13,9h6L8,22l3-10H5L10,2h7Z"/></svg>
      </div>
    </div>
    <div class="my-3"></div>
    <div class="relative w-[200px] h-[80px] rounded-xl bg-blue-900 border-8 border-blue-600 overflow-hidden">
      <div ref="battery" class="absolute h-full rounded-lg top-0 border-4 border-blue-900 left-0 bg-sky-400"></div>
    </div>
    <div class="my-2"></div>
    <div class="flex flex-col items-center gap-1">
      <p class="text-xs text-gray-300">Charging:</p>
      <p class="bg-green-500 w-min p-1 px-4 rounded-lg text-xs font-bold">{{isCharging}}</p>
    </div>
  </div>
</template>