<script>
export default {
  data () {
    return {
      data: "Your data will be displayed here.",
      url: null,
      username: null,
      password: null
    }
  },

  created () {
    this.initialize()
  },

  methods: {
    initialize () {
      console.log("Components created.")
    },

    getURL () {
      this.data = null // reset

      let request = null

      var xhttp = new XMLHttpRequest()
      xhttp.open("GET", this.url, true);
      xhttp.withCredentials = true
      xhttp.send(`Username: ${this.username}`, `Password: ${this.password}`)
      xhttp.onreadystatechange = function () {
        if (xhttp.readyState === XMLHttpRequest.DONE) {
          if (xhttp.status === 200) {
            const response = JSON.parse(xhttp.response)
            console.log(response.value)
            request = response
          } else {
            const msg = 'There was a problem with the request. Please try again.'
            console.log(msg)
            request = msg
          }
        }
      }

      setTimeout( () => {
        this.data = request
        this.url = null // reset
        console.log(request)
      }, 2000)
    }
  }
}
</script>

<template>
  <div>
    <Logos mb-6 />
    <Suspense>
      <template #fallback>
        <div op50 italic>
          <span animate-pulse>Loading...</span>
        </div>
      </template>
    </Suspense>
    <br>
    <NuxtLink
      class="btn m-3 text-sm"
      to="/"
    >
      Back
    </NuxtLink>

    <br>

    <div class="d-flex flex-column">
      <input
        v-model="url"
        placeholder="Enter your URL here..."
        type="text"
        autocomplete="false"
        p="x-4 y-2"
        m="t-5"
        w="250px"
        text="center"
        bg="transparent"
        border="~ rounded gray-200 dark:gray-700"
        outline="none active:none"
      >

      <input
        v-model="username"
        placeholder="Enter your username"
        type="text"
        autocomplete="false"
        p="x-4 y-2"
        m="t-5"
        w="250px"
        text="center"
        bg="transparent"
        border="~ rounded gray-200 dark:gray-700"
        outline="none active:none"
      >

      <input
        v-model="password"
        placeholder="Enter your password"
        type="password"
        autocomplete="false"
        p="x-4 y-2"
        m="t-5"
        w="250px"
        text="center"
        bg="transparent"
        border="~ rounded gray-200 dark:gray-700"
        outline="none active:none"
      >
    </div>

    <br class="my-5">

    <pre>{{ data ? data : 'Loading. Please wait...' }}</pre>

    <button class="btn m-3 text-sm" @click="getURL()">Get data</button>

    <NuxtLink
      class="btn m-3 text-sm"
      to="/"
    >
      Back
    </NuxtLink>
  </div>
</template>
