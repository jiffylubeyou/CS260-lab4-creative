<template>
<div class="home">
  <section class="image-gallery">
    <div class="image" v-for="item in items" :key="item.id">
      <h2>{{item.title}}</h2>
      <p>{{item.description}}</p>
      <img :src="item.path" />
    </div>
    <div class="charity">
  <p>Click this button for charity! {{counter}} clicks already!!!</p>
  <button button v-on:click="buttonClick">Click</button>
  </div>
  </section>
</div>

</template>

<script>
// @ is an alias to /src
import axios from 'axios';

export default {
  name: 'Home',
  data() {
    return {
     items: [],
     counter: 0
    };
  },
  created() {
    this.getItems();
    this.getCounter();
  },
  methods: {
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
//        console.log(error);
      }
    },
    async getCounter() {
       try {
        let response = await axios.get("/api/button");
        this.counter = response.data.counter;
        return true;
      } catch (error) {
//        console.log(error);
      }
    },
    async buttonClick() {0

      try {
        this.counter = this.counter + 1;
        await axios.put("/api/button/", { counter: this.counter });
        return true;
      } catch (error) {
 //       console.log(error);
      }
    },
  }
}
</script>

<style scoped>
.image h2 {
  font-style: italic;
}

/* Masonry */
*,
*:before,
*:after {
  box-sizing: inherit;
}

.image-gallery {
  column-gap: 1.5em;
}

.image {
  margin: 0 0 1.5em;
  display: inline-block;
  width: 100%;
}

.charity {
  text-align: center;
}

.image img {
  width: 100%;
}

/* Masonry on large screens */
@media only screen and (min-width: 1024px) {
  .image-gallery {
    column-count: 4;
  }
}

/* Masonry on medium-sized screens */
@media only screen and (max-width: 1023px) and (min-width: 768px) {
  .image-gallery {
    column-count: 3;
  }
}

/* Masonry on small screens */
@media only screen and (max-width: 767px) and (min-width: 540px) {
  .image-gallery {
    column-count: 2;
  }
}
</style>
