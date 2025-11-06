<script setup lang="ts">
import { reactive, ref } from 'vue';

// { "short_url": "J1zn19zfZfnE", "long_url": "https://elysiajs.com/essential/life-cycle.html", "created_at": "Thu Nov 06 2025 02:35:31 GMT+0800 (Philippine Standard Time)" } 

type State = {
  long_url: string
}

type Data = {
  short_url: string,
  long_url: string,
  created_at: string
}

const data = ref<Partial<Data>>()
const state = reactive<Partial<State>>({
  long_url: undefined
})


async function onSubmit() {
  if (!state.long_url) return
  try {
    const res = await fetch("/api/", {
      method: "POST",
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(state)
    })

    const resJson = await res.json()
    data.value = resJson
    console.log(res)
  } catch (error) {
    console.log(error)
    throw new Error("Unable to send")
  }
}


async function copy(text:string) {
    try {
      await navigator.clipboard.writeText(text)
    } catch (error) {
      console.log('Faild to copy text:' , error)
    }
}

</script>

<template>
  <main class="flex flex-col justify-center item-center font-mono">
    <form @submit.prevent="onSubmit" action="" class="flex flex-col justify-center item-center">
      <div class="flex justify-center item-center gap-5 p-2">
        <label for="" class="text-base p-2">URL:</label>
        <input v-model="state.long_url" type="url" name="" id="" placeholder="https://example.com"
          class="border-b p-2 appearance-none rounded-t-md">
        <button class="px-2 py-1 bg-blue-500 shadow-xl text-white rounded-md">submit</button>
      </div>
    </form>

    <div v-if="data"
      class="my-10 max-w-xl rounded-2xl shadow-xl bg-green-300 flex flex-col justify-center items-left p-5 gap-5">

    <div class="flex items-center justify-between gap-2">
     

      <span v-if="data?.short_url" class="flex items-center shadow-lg w-full text-left  gap-2 p-2">{{ `http://localhost:3000/api/${data.short_url}` }}</span>

      <button @click="copy(`http://localhost:3000/api/${data.short_url}`)" class="p-2 bg-blue-500 rounded-md text-white hover:opacity-45 active:scale-75">Copy</button>
    </div>
      <span v-if="data?.long_url" class="p-2 shadow-lg">{{ data.long_url }}</span>
  

      <span v-if="data?.created_at" class="p-2 shadow-lg">{{ data.created_at }}</span>
    </div>
  </main>
</template>

<style scoped></style>
