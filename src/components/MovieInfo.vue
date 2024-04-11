<script setup>
import router from "@/router";
import moviesData from "../movies.json";
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";

if (!moviesData[0]) {
  router.push({ name: "home" });
}
const movies = ref(JSON.parse(moviesData[0]));
const route = useRoute();
const id = ref(route.params.id - 1);

const chengeWidth = () => {
  if (window.innerWidth >= 1210) {
    router.push({ name: "home" });
  }
};

const resizeWidth = () => {
  chengeWidth();

  window.addEventListener("resize", () => {
    chengeWidth();
  });
};
onMounted(() => {
  resizeWidth();
});
</script>

<template>
  <div class="container">
    <div class="shadow"></div>
    <img class="big-pic" :src="movies[id].bg_picture" alt="" srcset="" />
    <section class="info-sec">
      <div class="info">
        <span class="bold">
          <span class="title">{{ movies[id].title }}</span>
          ({{ movies[id].release_year }})
        </span>
        <p>{{ movies[id].description }}</p>
        <div class="rating">
          <span v-for="gener in movies[id].genres" :key="gener.id">
            {{ gener.title }}</span
          >|<span>{{ movies[id].mpa_rating }}</span
          >|<span>{{ movies[id].duration }} m</span>
        </div>
        <span
          >Directors:
          <span
            class="bold"
            v-for="director in movies[id].directors"
            :key="director.id"
          >
            {{ director.first_name }} {{ director.last_name }},
          </span></span
        >
        <span
          >Writers:
          <span
            class="bold"
            v-for="writer in movies[id].writers"
            :key="writer.id"
            >{{ writer.first_name }} {{ writer.last_name }},
          </span></span
        >
        <span
          >Stars:
          <span class="bold" v-for="star in movies[id].stars" :key="star.id"
            >{{ star.first_name }} {{ star.last_name }},
          </span></span
        >
      </div>
      <div class="IMDB">
        <div>
          <button>Watch trailer</button>
        </div>
        <div>
          <span class="bold">IMDB rating</span>
          <span>
            ⭐
            <span class="bold title">{{ movies[id].imdb_rating }}</span>
            /10
          </span>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped lang="scss">
.container {
  position: relative;
  font-family: "Rubik", sans-serif;
  .big-pic {
    width: 100vw;
    height: 45vh;
    object-fit: cover;
    z-index: -2;
  }
  .shadow {
    position: absolute;
    top: 5px;
    right: 0;
    width: 100vw;
    height: 45vh;
    background-image: linear-gradient(#16161600, #16161665, #161616);
  }
  .info-sec {
    display: flex;
    justify-content: space-between;
    padding: 0 64px;
    font-size: 16px;
    button {
      width: 160px;
      height: 42px;
      color: #fff;
      font-size: 18px;
      font-weight: 600;
      background-color: #da1617;
      border: none;
      font-family: "Rubik", sans-serif;
    }
    .bold {
      font-weight: 600;
    }
    .title {
      font-size: 44px;
    }
    .info {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      height: 292px;
      max-width: 360px;
      z-index: -2;
      .title-sec {
        display: flex;
        justify-content: space-between;
      }
      .rating {
        display: flex;
        justify-content: space-between;
      }
    }
    .IMDB {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      height: 104px;
      z-index: -2;
    }
    .IMDB div:last-child {
      display: flex;
      flex-direction: column;
      margin-top: 24px;
    }
  }
  @media (max-width: 652px) {
    .info-sec {
      flex-direction: column;
      .IMDB {
        margin-top: 24px;
        flex-direction: row-reverse;
      }
      .IMDB div:last-child {
        margin-top: 0;
      }
    }
  }
}
</style>
