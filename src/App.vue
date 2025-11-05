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

</script>

<template>
  <main>
    <form @submit.prevent="onSubmit" action="">
      <label for="">URL</label>
      <input v-model="state.long_url" type="url" name="" id="">
      <button>submit</button>
    </form>

    <div v-if="data">
      <span v-if="data?.short_url">{{ `http://localhost:3000/api/${data.short_url}` }}</span>
      <!-- <span v-if="data?.long_url">{{ data.long_url }}</span>
      <span v-if="data?.created_at">{{ data.created_at}}</span> -->
    </div>
  </main>
</template>

<style scoped></style>
