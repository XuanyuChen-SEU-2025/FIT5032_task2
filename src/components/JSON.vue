<template>
  <div class="json-lab">
    <h1>JSON Data & Vue Directives Lab</h1>

    <section class="lab-section">
      <h2>Working with JSON Arrays</h2>
      <p>Our <code>authors.json</code> contains an array of author objects.</p>

      <h3>Iterating through Arrays</h3>
      <ul>
        <li v-for="author in authors" :key="author.id">
          {{ author.name }} - {{ author.birthYear }}
        </li>
      </ul>

      <h3>Filtering Arrays</h3>
      <p>Authors born after 1850:</p>
      <ul>
        <li v-for="author in modernAuthors" :key="author.id">
          {{ author.name }} - {{ author.birthYear }}
        </li>
      </ul>

      <h3>Mapping Arrays</h3>
      <p>Famous works:</p>
      <ul>
        <li
          v-for="work in allFamousWorks"
          :key="`${work.authorName}-${work.title}-${work.year}`"
        >
          {{ work.title }} ({{ work.year }}) by {{ work.authorName }}
        </li>
      </ul>

      <h3>Finding in Arrays</h3>
      <p>Finding by property: {{ orwell?.name || "Author not found" }}</p>

      <h3>Nested Arrays/Objects</h3>
      <template v-if="austen">
        <p>{{ austen.name }}'s works:</p>
        <ul>
          <li
            v-for="work in austen.famousWorks"
            :key="`${austen.id}-${work.title}-${work.year}`"
          >
            {{ work.title }} ({{ work.year }})
          </li>
        </ul>
      </template>
      <p v-else>Jane Austen was not found.</p>
    </section>

    <section class="lab-section">
      <h2>Working with JSON Objects</h2>
      <p>Our <code>bookstores.json</code> is a JSON object.</p>

      <h3>Accessing Properties</h3>
      <p>Company: {{ bookstores.name }}</p>
      <p>Total Stores: {{ bookstores.totalStores }}</p>

      <h3>Iterating Object Properties</h3>
      <p>Store Types:</p>
      <ul>
        <li
          v-for="(storeCount, storeType) in bookstores.storeTypes"
          :key="storeType"
        >
          {{ storeType }}: {{ storeCount }}
        </li>
      </ul>

      <h3>Nested Objects</h3>
      <p>Opening Hours:</p>
      <ul>
        <li
          v-for="(hours, dayType) in bookstores.openingHours"
          :key="dayType"
        >
          {{ dayType }}: open {{ hours.open }}, close {{ hours.close }}
        </li>
      </ul>

      <h3>Working with Arrays in Objects</h3>
      <p>Top sellers:</p>
      <ul v-if="bookstores.topSellers?.length">
        <li v-for="book in bookstores.topSellers" :key="book">
          {{ book }}
        </li>
      </ul>
      <p v-else>No top sellers are available.</p>
      <p>We operate in: {{ countryList }}</p>
      <p>Our #1 seller: {{ firstTopSeller }}</p>
    </section>

    <section class="lab-section">
      <h2>v-if & v-else</h2>
      <p>Toggle visibility based on a condition.</p>
      <button @click="showMessage = !showMessage">Toggle Message</button>
      <p v-if="showMessage" class="message success">
        You're a Vue superstar!
      </p>
      <p v-else class="message hint">Click the button to see a message.</p>
    </section>

    <section class="lab-section">
      <h2>Attribute, Class and Style Binding with <code>v-bind</code></h2>
      <p>Highlighting Specific Authors:</p>
      <ul>
        <li
          v-for="author in authors"
          :key="`binding-${author.id}`"
          :title="`Author: ${author.name}`"
          :data-author-id="author.id"
          :class="{ highlight: author.name === 'George Orwell' }"
          :style="getAuthorStyle(author)"
        >
          <strong>{{ author.name }}</strong>
          <span>Born {{ author.birthYear }}</span>
        </li>
      </ul>
    </section>
  </div>
</template>

<script setup>
import { ref, computed } from "vue"
import authors from "../assets/json/authors.json"
import bookstores from "../assets/json/bookstores.json"

const showMessage = ref(false)

const modernAuthors = computed(() =>
  authors.filter((author) => author.birthYear > 1850),
)

const allFamousWorks = computed(() =>
  authors.flatMap((author) =>
    author.famousWorks.map((work) => ({
      title: work.title,
      year: work.year,
      authorName: author.name,
    })),
  ),
)

const orwell = computed(() =>
  authors.find((author) => author.name === "George Orwell"),
)

const austen = computed(() => authors.find((author) => author.id === 1))

const countryList = computed(() =>
  bookstores.countries?.length ? bookstores.countries.join(", ") : "No countries listed",
)

const firstTopSeller = computed(
  () => bookstores.topSellers?.[0] || "No top seller available",
)

const getAuthorStyle = (author) =>
  author.name === "George Orwell"
    ? {
        color: "#1a202c",
        border: "2px solid #2f855a",
        fontWeight: "800",
        padding: "14px",
      }
    : {
        color: "#374151",
        border: "1px solid #d6dde8",
        fontWeight: "500",
        padding: "10px",
      }
</script>

<style scoped>
.json-lab {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  max-width: min(1000px, calc(100vw - 32px));
  margin: 0 auto;
  padding: 20px;
  background-color: #f4f4f4;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

h1,
h2 {
  color: #333;
}

h1 {
  text-align: center;
}

.lab-section {
  background-color: white;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.message {
  padding: 10px;
  border-radius: 5px;
  margin-top: 10px;
}

.success {
  background-color: #e7faf3;
  color: #276749;
  border: 1px solid #42b883;
}

.hint {
  background-color: #eef2ff;
  color: #243b53;
  border: 1px solid #bcccdc;
}

.highlight {
  background-color: #d8f3dc;
}

code {
  background-color: #e0e0e0;
  padding: 2px 5px;
  border-radius: 4px;
  font-family: "Courier New", Courier, monospace;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  background-color: #f0f0f0;
  padding: 10px;
  margin: 5px 0;
  border-radius: 5px;
}
</style>
