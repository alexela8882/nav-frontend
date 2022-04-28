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
      let self = this
      self.data = null // reset

      var xhttp = new XMLHttpRequest()
      xhttp.onreadystatechange = function () {
        if (xhttp.readyState === XMLHttpRequest.DONE) {
          if (xhttp.status === 200) {
            const response = JSON.parse(xhttp.response)

            self.url = null // reset
            self.generateDynamicTable(response) // generate table
          } else {
            const msg = 'There was a problem with the request. Please try again.'
            console.log(msg)
            self.data = msg
          }
        }
      }
      xhttp.open("GET", this.url, true)
      xhttp.withCredentials = true
      xhttp.send()
    },

    generateDynamicTable (response) {
      const data = response ? response.value : null
      // EXTRACT VALUE FOR HTML HEADER. 
      var col = []
      for (var i = 0; i < data.length; i++) {
        for (var key in data[i]) {
          if (col.indexOf(key) === -1) {
            col.push(key)
          }
        }
      }
      // push comment
      // CREATE DYNAMIC TABLE.
      var table = document.createElement("table")

      // CREATE HTML TABLE HEADER ROW USING THE EXTRACTED HEADERS ABOVE.
      var tr = table.insertRow(-1) // TABLE ROW.

      for (var i = 0; i < col.length; i++) {
        var th = document.createElement("th") // TABLE HEADER.
        th.innerHTML = col[i]
        tr.appendChild(th)
      }

      // ADD JSON DATA TO THE TABLE AS ROWS.
      for (var i = 0; i < data.length; i++) {
        tr = table.insertRow(-1)
        for (var j = 0; j < col.length; j++) {
          var tabCell = tr.insertCell(-1)
          tabCell.innerHTML = data[i][col[j]]
        }
      }

      // FINALLY ADD THE NEWLY CREATED TABLE WITH JSON DATA TO A CONTAINER.
      var divContainer = document.getElementById("showData")
      divContainer.innerHTML = ""
      divContainer.appendChild(table)

      this.data = response
    },

    clearData () {
      this.data = 'Generate again.'
      document.getElementById("tbl-1").remove()
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

    <button class="btn m-3 text-sm" @click="clearData()">Clear data</button>

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

    <!-- <p>{{ Array.isArray(data) || typeof data === 'object' ? "Data successfully fecthed" : (data !== null ? data : 'Loading. Please wait...' ) }}</p> -->
    <p>{{ data ? (Array.isArray(data) || typeof data === 'object' ? "Data successfully fecthed" : data) : 'Loading. Please wait...' }}</p>

    <p id='showData' class="table-auto"></p>

    <button :disabled="!url || data == null" class="btn m-3 text-sm" @click="getURL()">Get data</button>

    <NuxtLink
      class="btn m-3 text-sm"
      to="/"
    >
      Back
    </NuxtLink>
  </div>
</template>
