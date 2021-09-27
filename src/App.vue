<template>
  <!-- ============ Marquee ============ -->
  <div class="marquee w-full h-12 overflow-hidden bg-black position-relative">
    <ul class="marquee-content h-full flex" ref="mq">
      <!-- main -->
      <li
        v-for="coin in tenItems"
        :key="coin.name"
        class="flex justify-center items-center flex-shrink-0 text-white transform scale-75 lg:scale-100"
      >
        <div class="flex justify-between w-3/4">
          <!-- ticker -->
          <div class="flex items-center">
            <img
              :src="coin.image"
              alt="coin logo"
              class="w-4 h-4 lg:w-6 lg:h-6 rounded-full mr-4 "
            />
            <div class="hidden lg:block">
              <p class="font-bold">{{ coin.name }}</p>
              <p class="text-xs uppercase tracking-widest ">
                {{ coin.symbol }}
              </p>
            </div>
          </div>
          <!-- price -->
          <div>
            <p class="font-bold text-xs lg:text-base flex justify-end ">
              {{ $filters.comma_sep(coin.current_price) }}
            </p>
            <!-- percent red -->
            <p
              class="font-bold text-xs text-red-400 flex justify-end items-center "
              v-if="$filters.price_changed(coin.price_change_percentage_24h)"
            >
              <fa icon="caret-down" class="mr-1" />

              {{ $filters.str_limit(coin.price_change_percentage_24h, 5) }}%
            </p>
            <!-- percent green -->
            <p
              v-else
              class="font-bold text-xs text-green-400 flex justify-end items-center "
            >
              <fa icon="caret-up" class="mr-1" />

              {{ $filters.str_limit(coin.price_change_percentage_24h, 5) }}%
            </p>
          </div>
        </div>
      </li>
      <!-- clones -->
      <li
        v-for="clone in clones"
        :key="clone.name"
        class="flex justify-center items-center flex-shrink-0 text-white transform scale-75 lg:scale-100"
      >
        <div class="flex justify-between w-3/4">
          <!-- ticker -->
          <div class="flex items-center">
            <img
              :src="clone.image"
              alt="coin logo"
              class="w-4 h-4 lg:w-6 lg:h-6 rounded-full mr-4 "
            />
            <div class="hidden lg:block">
              <p class="font-bold">{{ clone.name }}</p>
              <p class="text-xs uppercase tracking-widest">
                {{ clone.symbol }}
              </p>
            </div>
          </div>
          <!-- price -->
          <div>
            <p class="font-bold flex justify-end text-xs lg:text-base">
              {{ $filters.comma_sep(clone.current_price) }}
            </p>
            <!-- percent red -->
            <p
              class="font-bold text-xs text-red-400 flex justify-end items-center "
              v-if="$filters.price_changed(clone.price_change_percentage_24h)"
            >
              <fa icon="caret-down" class="mr-1" />

              {{ $filters.str_limit(clone.price_change_percentage_24h, 5) }}%
            </p>
            <!-- percent green -->
            <p
              v-else
              class="font-bold text-xs text-green-400 flex justify-end items-center "
            >
              <fa icon="caret-up" class="mr-1" />
              {{ $filters.str_limit(clone.price_change_percentage_24h, 5) }}%
            </p>
          </div>
        </div>
      </li>
    </ul>
  </div>
  <!-- ============ Events ============ -->
  <section class="event">
    <div class="container mx-auto py-4 lg:py-24 flex flex-wrap justify-center ">
      <a
        :href="event.website"
        class="rounded m-6"
        target="_blank"
        v-for="event in events"
        :key="event.title"
      >
        <img
          :src="event.screenshot"
          alt="event image"
          class="w-80 h-52 mx-auto object-cover rounded"
        />
        <div class="pt-2 w-80">
          <p class="font-medium pt-2">{{ event.title }}</p>
          <p class="text-xs pt-2 w-3/4">
            {{ $filters.str_limit(event.description, 80) }}
          </p>
        </div>
      </a>
    </div>
  </section>
  <!-- ============ Table ============ -->
  <div class="container mx-auto pt-18 pb-48 px-2">
    <!-- search -->
    <div class="flex justify-end pb-2">
      <div class="relative">
        <input
          type="text"
          placeholder="search..."
          class="border border-gray-500 rounded p-2 focus:outline-none focus:ring focus:ring-gray-300"
          v-model="search"
        />
        <fa
          icon="search"
          class="text-gray-500 absolute right-2 top-2/4 transform -translate-y-1/2"
        />
      </div>
    </div>
    <!-- table -->
    <table class="table-fixed cursor-pointer">
      <!-- head -->
      <thead class="bg-gray-200">
        <tr class="text-left text-gray-600 text-sm">
          <th class="w-1/4 p-4">Name</th>
          <th class="w-1/4">Price</th>
          <th class="w-1/4">1h %</th>
          <th class="w-1/4 hidden sm:table-cell">Market Cap</th>
          <th class="w-1/4 hidden sm:table-cell">Volume</th>
        </tr>
      </thead>
      <!-- body -->
      <tbody class="divide-y">
        <tr
          class="text-sm hover:bg-gray-100 transition duration-300"
          v-for="coin in matchingNames"
          :key="coin.name"
        >
          <td class="p-4 flex items-center">
            <p class="mr-2">{{ coin.market_cap_rank }}</p>
            <img
              :src="coin.image"
              alt="coin logo"
              class="w-6 h-6 rounded-full mr-1"
            />
            <p class="font-bold mr-1">{{ coin.name }}</p>
            <p class="uppercase text-gray-500 hidden sm:table-cell">
              {{ coin.symbol }}
            </p>
          </td>
          <td class="font-bold">
            ${{ $filters.comma_sep(coin.current_price) }}
          </td>
          <td class=" font-bold">
            <div
              v-if="$filters.price_changed(coin.price_change_percentage_24h)"
              class="text-red-500"
            >
              <fa icon="caret-down" class="mr-1" />{{
                coin.market_cap_change_percentage_24h
              }}%
            </div>
            <div v-else class="text-green-500">
              <fa icon="caret-up" class="mr-1" />{{
                coin.market_cap_change_percentage_24h
              }}%
            </div>
          </td>
          <td class="hidden sm:table-cell">
            ${{ $filters.comma_sep(coin.market_cap) }}
          </td>
          <td class="pr-4 hidden sm:table-cell">
            {{ $filters.comma_sep(coin.total_volume) }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import "./assets/tailwind.css";
import axios from "axios";
import { computed, ref, watchEffect } from "vue";
export default {
  name: "App",
  setup() {
    // selectors
    const mq = ref(null);
    const clones = ref(null);
    const coins = ref([]);
    const search = ref("");
    const events = ref([]);
    // api call
    const getCoins = async () => {
      try {
        const res = await axios.get(
          "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false&price_change_percentage=1h"
        );
        coins.value = res.data;
      } catch (err) {
        console.log(err);
      }
    };
    const getEvents = async () => {
      try {
        const res = await axios.get(
          "https://api.coingecko.com/api/v3/events?upcoming_events_only=true"
        );
        events.value = res.data.data;
      } catch (err) {
        console.log(err);
      }
    };
    getEvents();
    // fetch timer
    setInterval(() => {
      getCoins();
    }, 1200);
    // clone
    watchEffect(() => {
      const dup = coins.value.slice(0, 5);
      clones.value = dup;
    });
    // first 10
    const tenItems = computed(() => {
      const coin = coins.value.slice(0, 10);
      return coin;
    });
    // search
    const matchingNames = computed(() => {
      return coins.value.filter((name) => name.id.includes(search.value));
    });

    // return
    return {
      coins,
      mq,
      clones,
      tenItems,
      search,
      matchingNames,
      events,
    };
  },
};
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.marquee-content {
  animation: scrolling 20s linear infinite;
}
.marquee-content:hover {
  animation-play-state: paused;
}
.marquee-content li {
  width: 20%;
}
.marquee:before,
.marquee:after {
  position: absolute;
  top: 0;
  width: 10rem;
  height: 3em;
  content: "";
  z-index: 1;
}
.marquee:before {
  left: 0;
  background: linear-gradient(to right, #111 0%, transparent 100%);
}
.marquee:after {
  right: 0;
  background: linear-gradient(to left, #111 0%, transparent 100%);
}
.event {
  background-image: url("./assets/bg.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  width: 100%;
  position: relative;
}
.event:before {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 6em;
  content: "";
  z-index: 1;
  background: linear-gradient(to top, #fff 0%, transparent 100%);
}
@keyframes scrolling {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-200%);
  }
}
</style>
