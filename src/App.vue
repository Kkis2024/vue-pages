<template>
  <div id="app" class="container">
    <h1>Random Quote Generator</h1>

    <!-- Display the quote or a loading message -->
    <p v-if="loading">Loading...</p>
    <div v-else>
      <p>"{{ quote }}"</p>
      <p class="author">— {{ author }}</p>
    </div>

    <!-- The button that triggers the API call -->
    <button @click="getQuote">Get New Quote</button>
  </div>
</template>


<script>
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      quote: '',      // stores the quote text
      author: '',     // store author name
      loading: false  // tracks loading status
    }
  },
  methods: {
    async getQuote() {
      this.loading = true
      try {
        // Send a GET request to the Quotable API
        const response = await axios.get(
          'https://quotes15.p.rapidapi.com/quotes/random/?language_code=en',
          {
            headers: {
              'x-rapidapi-host': 'quotes15.p.rapidapi.com',
              'x-rapidapi-key': '266512ae34msh491de925f3a5c20p1b48bejsn3eac852a6cb1'
            }
          }
        )
        // Update the quote with the response data
        this.quote = response.data.content
        this.author = response.data.originator.name
      } catch (error) {
        console.error('Error fetching quote:', error)
        this.quote = 'Oops! Something went wrong.'
        this.author = ''
      } finally {
        this.loading = false
      }
    }
  },
  mounted() {
    // Load a random quote when the app first starts
    this.getQuote()
  }
}
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #502c48ff;
  margin-top: 60px;
}


</style>

<style>
.container {
  text-align: center;
  font-family: Arial, sans-serif;
  margin-top: 50px;
}
button {
  background: linear-gradient(135deg, #e27fa0ff, #ee468cff);
  border: none;
  border-radius: 25px;
  padding: 12px 24px;
  font-size: 16px;
  font-weight: bold;
  color: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 6px rgba(0,0,0,0.2);
}

button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 10px rgba(0,0,0,0.25);
  background: linear-gradient(135deg, #bb0f57ff, #660022ff);
}

button:active {
  transform: translateY(0);
  box-shadow: 0 4px 6px rgba(0,0,0,0.2);
}
.author {
  font-style: italic;
  color: #9950a0ff;
  margin-top: 5px;
}

</style>

