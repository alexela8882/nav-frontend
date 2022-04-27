<script>
export default {
  data () {
    return {
      data: "Test"
    }
  },

  created () {
    this.initialize()
  },

  methods: {
    async initialize() {
      this.data = null // reset

      let request = null

      var xhttp = new XMLHttpRequest()
      xhttp.open("GET", "http://LEKZPC:7048/DynamicsNAV100/ODataV4/Company('CRONUS%20International%20Ltd.')/powerbifinance", true);
      xhttp.withCredentials = true
      xhttp.send("Username: Admin", "Password: acf0106")
      xhttp.onreadystatechange = function () {
        if (xhttp.readyState === XMLHttpRequest.DONE) {
          if (xhttp.status === 200) {
            const response = JSON.parse(xhttp.response)
            console.log(response.value)
            request = response.value
          } else console.log('There was a problem with the request.')
        }
      }

      setTimeout( () => {
        this.data = request
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

    <strong>DATA:</strong>
    <pre>{{ data ? data : 'Loading. Please wait...' }}</pre>

    <NuxtLink
      class="btn m-3 text-sm"
      to="/"
    >
      Back
    </NuxtLink>
  </div>
</template>
