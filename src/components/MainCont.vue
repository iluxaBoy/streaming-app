<script setup>
import moviesData from "../movies.json";
import MovieInfo from "./MovieInfo.vue";
import { onMounted, ref } from "vue";

const link = "https://live.mocat.amifactory.network/api/v1/movies/";

const movies = ref();
const title = ref();

const pass = ref("");
const pic = ref("");
const card = ref();
const change = ref();
const loadingDiv = ref();

const loading = ref(false);
const mobile = ref(false);

//fun for changing title and background pic
const changeMovieClicked = (movie) => {
  if (!mobile) {
    return;
  }
  change.value.style.opacity = 1;

  moviesData[0] = JSON.stringify(movies.value[movie.id - 1]);
  setTimeout(() => {
    title.value = movie.id - 1;
    pic.value = movies.value[movie.id - 1].bg_picture;
    change.value.style.opacity = 0;
  }, 300);
};

fetch(link)
  .then((response) => response.json())
  .then((data) => {
    movies.value = data.results;

    loadingDiv.value.style.opacity = 0;
    loadingDiv.value.style.zIndex = "-3";
    loading.value = true;

    title.value = 0;

    moviesData[0] = JSON.stringify(movies.value);
    pic.value = movies.value[0].bg_picture;
  })
  .catch((err) => console.log(err));

const chengeWidth = () => {
  if (window.innerWidth <= 1210) {
    pass.value = "movie";
    mobile.value = true;
  } else {
    pass.value = "";
    mobile.value = false;
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
    <!--  -->
    <div class="loading" ref="loadingDiv"></div>
    <div class="shadow"></div>
    <div class="change" ref="change"></div>
    <img class="big-pic" :src="pic" alt="" />
    <!--  -->
    <section class="info-sec" v-if="loading">
      <div class="info">
        <span class="bold">IMDB rating</span>
        <span>
          ⭐
          <span class="bold title">{{ movies[title].imdb_rating }}</span>
          /10
        </span>
        <span class="bold">
          <span class="title">{{ movies[title].title }}</span>
          ({{ movies[title].release_year }})
        </span>
        <p>{{ movies[title].description }}</p>
        <div class="rating">
          <span v-for="gener in movies[title].genres" :key="gener.id">
            {{ gener.title }}</span
          >|<span>{{ movies[title].mpa_rating }}</span
          >|<span>{{ movies[title].duration }} m</span>
        </div>
        <button>Watch trailer</button>
      </div>
      <div class="people">
        <span
          >Directors:
          <span
            class="bold"
            v-for="director in movies[title].directors"
            :key="director.id"
          >
            {{ director.first_name }} {{ director.last_name }},
          </span></span
        >
        <span
          >Writers:
          <span
            class="bold"
            v-for="writer in movies[title].writers"
            :key="writer.id"
            >{{ writer.first_name }} {{ writer.last_name }},
          </span></span
        >
        <span
          >Stars:
          <span class="bold" v-for="star in movies[title].stars" :key="star.id"
            >{{ star.first_name }} {{ star.last_name }},
          </span></span
        >
      </div>
    </section>
    <section class="cards-sec" v-if="loading">
      <div>
        <button><</button>
      </div>
      <div class="cards">
        <div
          @click="changeMovieClicked(movie)"
          ref="card"
          class="card"
          v-for="movie in movies"
          :key="movie.id"
        >
          <router-link v-show="mobile" :to="`${pass}/${movie.id}`">
            <div class="img-card">
              <img :src="movie.poster" alt="" srcset="" />
            </div>
          </router-link>
          <div v-show="!mobile" class="img-card">
            <img :src="movie.poster" alt="" srcset="" />
          </div>
          <span
            >⭐<span style="font-weight: 600">{{ movie.imdb_rating }}</span> /
            10</span
          >
          <h3>{{ movie.title }}</h3>
        </div>
      </div>
      <div class="r-btns">
        <div>
          <span style="cursor: pointer"
            ><h1>+</h1>
            <br />
            View more</span
          >
        </div>
        <button>></button>
      </div>
    </section>
  </div>
</template>

<style scoped lang="scss">
.container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  padding: 0 64px;
  justify-content: end;
  font-family: "Rubik", sans-serif;
  font-weight: 100;
  button {
    cursor: pointer;
  }
  // ---- effects
  .shadow,
  .change,
  .loading,
  .big-pic {
    position: absolute;
    top: 0;
    right: 0;
    width: 100vw;
    height: 100vh;
  }
  .shadow {
    background-image: linear-gradient(#16161600, #16161665, #161616);
    z-index: -3;
  }
  .change,
  .loading {
    background-color: #161616;
    z-index: -1;
    opacity: 0;
    transition: all 0.3s;
  }
  .loading {
    opacity: 1;
    z-index: 2;
  }
  .big-pic {
    object-fit: cover;
    transition: all 0.3s;
    z-index: -4;
  }
  // ---- info
  .info-sec {
    padding: 0 64px;
    display: flex;
    align-items: center;
    justify-content: space-between;
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
    .info {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      height: 292px;
      max-width: 560px;
      z-index: -2;
      .title {
        font-size: 44px;
      }
      .rating {
        display: flex;
        justify-content: space-between;
      }
    }
    @media (max-width: 1654px) {
      .info {
        max-width: 360px;
      }
    }
    .people {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      height: 104px;
      z-index: -2;
    }
  }
  @media (max-width: 1222px) {
    .info-sec {
      position: fixed;
      top: 398px;
      right: 0;
      padding: 0;
      width: 724px;
      flex-direction: column;
      justify-content: start;
      align-items: start;
      .people {
        margin-top: 24px;
      }
    }
  }
  @media (max-width: 1210px) {
    .info-sec {
      display: none;
    }
  }
  // ---- cards
  .cards-sec {
    display: flex;
    justify-content: center;
    position: relative;
    margin-top: 50px;
    display: flex;
    align-items: center;
    button {
      margin: 0 24px 64px;
      padding: 12px 22px;
      font-family: "Rubik", sans-serif;
      font-size: 28px;
      border: none;
      border-radius: 100%;
      color: #fff;
      background-color: #0000005d;
    }
    @media (max-width: 1654px) {
      button {
        padding: 8px 14px;
        font-size: 18px;
      }
    }
    @media (max-width: 1222px) {
      button {
        display: none;
      }
    }
    .cards {
      display: flex;
      .card {
        display: flex;
        flex-direction: column;
        width: 150px;
        margin: 15px;
        transition: all 0.2s;
        .img-card {
          height: 222px;
          transition: all 0.2s;
        }
        img {
          width: 100%;
          height: 100%;
          object-fit: cover;
        }
      }
      .card:hover {
        width: 164px;
        margin: 0 8px;
        .img-card {
          height: 242px;
        }
      }
      @media (max-width: 1654px) {
        .card {
          width: 100px;
          margin: 12px;
          font-size: 14px;
          .img-card {
            height: 150px;
          }
        }
        .card:hover {
          width: 112px;
          margin: 0 6px;
          .img-card {
            height: 168px;
          }
        }
      }
    }
    .r-btns {
      display: flex;
      text-align: center;
    }
    @media (max-width: 1222px) {
      align-items: baseline;
      margin-top: 10px;
      .cards {
        max-width: 480px;
        flex-wrap: wrap;
        .card {
          width: 205px;
          margin: 15px;
          .img-card {
            height: 322px;
          }
        }
        .card:hover {
          width: 205px;
          margin: 15px;
          .img-card {
            height: 322px;
          }
        }
      }
      .r-btns {
        display: none;
      }
    }
    @media (max-width: 1210px) {
      .cards {
        max-width: 1000px;
        justify-content: center;
      }
    }
    @media (max-width: 520px) {
      .cards {
        .card {
          width: 164px;
          margin: 12px;
          .img-card {
            height: 244px;
          }
        }
      }
    }
  }
  // ----
}
@media (max-width: 1222px) {
  .container {
    padding: 0 24px;
    flex-direction: row-reverse;
    justify-content: start;
    .big-pic,
    .shadow {
      position: fixed;
      top: 26px;
      right: 0;
      width: 724px;
      height: 382px;
    }
    .shadow {
      top: 32px;
      width: 728px;
      background-image: linear-gradient(#16161600, #16161600, #161616);
    }
  }
}
@media (max-width: 1210px) {
  .container {
    .big-pic,
    .shadow {
      display: none;
    }
  }
}
</style>
