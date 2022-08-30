<script setup lang="ts">
import { reactive, ref, type Ref } from "vue";
import config from "../config.json";
import IconComponent from "./components/IconComponent.vue";

const api = `https://api.openweathermap.org/data/2.5/weather?lat=${config.latitude}&lon=${config.longitude}&appid=${config.openweathermapKey}`;
const weather: {
  temperature?: number | string;
  description?: string;
  iconId?: string;
} = reactive({});

weather.iconId = "CloudOff";
weather.temperature = "-";
fetch(api)
  .then(function (response) {
    const data = response.json();
    return data;
  })
  .then(function (data) {
    const celsius = Math.floor(data.main.temp - 273.15);
    weather.temperature =
      config.unit == "celsius" ? celsius : (celsius * 9) / 5 + 32;
    weather.description = data.weather[0].description;

    switch (data.weather[0].icon) {
      case "01d":
        weather.iconId = "Sun";
        break;
      case "01n":
        weather.iconId = "Moon";
        break;
      case "02d":
        weather.iconId = "CloudSun";
        break;
      case "02n":
        weather.iconId = "CloudMoon";
        break;
      case "03d":
        weather.iconId = "CloudSun";
        break;
      case "03n":
        weather.iconId = "CloudMoon";
        break;
      case "04d":
        weather.iconId = "Cloud";
        break;
      case "04n":
        weather.iconId = "Cloud";
        break;
      case "09d":
        weather.iconId = "CloudRain";
        break;
      case "09n":
        weather.iconId = "CloudRain";
        break;
      case "10d":
        weather.iconId = "CloudRain";
        break;
      case "10n":
        weather.iconId = "CloudRain";
        break;
      case "11d":
        weather.iconId = "CloudLightning";
        break;
      case "11n":
        weather.iconId = "CloudLightning";
        break;
      case "50d":
        weather.iconId = "CloudDrizzle";
        break;
      case "50n":
        weather.iconId = "CloudDrizzle";
        break;
    }
  });

let d;
const mm: Ref<string> = ref("");
const dd: Ref<number> = ref(0);
const min: Ref<number | string> = ref("");
const hh: Ref<number> = ref(0);
const ampm: Ref<string> = ref("");

function displayClock(): any {
  const monthNames = [
    "Jan",
    "Feb",
    "Mar",
    "Apr",
    "May",
    "Jun",
    "Jul",
    "Aug",
    "Sep",
    "Oct",
    "Nov",
    "Dec",
  ];

  d = new Date();
  mm.value = monthNames[d.getMonth()];
  dd.value = d.getDate();
  min.value = d.getMinutes();
  hh.value = d.getHours();
  ampm.value = hh.value >= 12 ? " PM" : " AM";

  if (min.value.toString()[1] === undefined || min.value.toString()[1] === null)
    min.value = `0${min.value.toString()[0]}`;

  hh.value = hh.value % 12;
  hh.value = hh.value ? hh.value : 12;

  setTimeout(displayClock, 1000);
}

window.onload = displayClock();

// ##@########OO#########oo#####@@@
// #######OO######@@@@@@o.*o#@#@@@@
// ####O####@@@#@#Oo**oo...°°O@#@@@
// #OOOO#@###@##O*°°°°°°°°.°°°O@#@@
// OOOO#O######O°°°°°°°°°°°.°°°O@#@
// O##oO###O###*°°°°°°°°°°°°.°°°##@
// ##@###@####o°°o°°°°°°°*°*.°°°o##
// @@@@@@@@@@#°°o#°°°°°°°°°°°°°°*##
// @##@###@##o°°#O°*°°°°°°°°°°°°°O@
// @########@o°o#O****°°°°°°°°°°°o@
// @@######@@o*O##oo****°°..**°°°O@
// ###@OO###@O*o°##OOo***°..oo.°°##
// ###o*o####OoO*##Oo****°..O#°.*##
// ###OO######*#####o.oo*o.*###*o##
// ##@###O##@#°######O#o**°####@###
// @#######O#O.########o*°o########
// #@###@##O#O°#######O**oo*o######
// @@@@@###O#O*Oo#####O**ooooo#####
// @@@########oO*o#OO#O*°oooooO####
// @@@@@@#######o*OOOooo°*O*ooooO##
// @@@###########*****oOOoOOooooo##
// #@@#####Oo#@##*°*oO#@#######OOo#
// ####@###ooo#O**o*#@####@########
// ###@###OOOoO**OO*o####@#########
// ###@###OOoooo###o*#####@########
// #@@####Oo°*o#@###*###@@#########
// ######OOO*O#OOO#@OO@#O#@########
// ######O#####OOo######OOO########
// #############OOOO###O@##########
// ##############ooOOOO*#@@@#######
// ##############OooOOOoO###OO#####
// ################OOOOOooooO######
function searchWeb(query: string) {
  if (query.startsWith("r/"))
    window.location.href = `https://reddit.com/${query}`;
  else if (query.startsWith("g/"))
    window.location.href = `https://github.com/${query.replace(/^.{2}/, "")}`;
  else if (query.startsWith("aw/"))
    window.location.href = `https://wiki.archlinux.org/index.php?search=${query.replace(
      /^.{3}/,
      ""
    )}&title=Special%3ASearch&fulltext=Search`;
  else if (query.startsWith("y/"))
    window.location.href = `https://www.youtube.com/results?search_query=${query.replace(
      /^.{2}/,
      ""
    )}`;
  else if (query.startsWith("w/"))
    window.location.href = `https://en.wikipedia.org/wiki/Special:Search?search=${query.replace(
      /^.{2}/,
      ""
    )}&go=Go&ns0=1`;
  else if (query.match(/^[a-z0-9]+:\/\//)) window.location.href = query; // URL
  else window.location.href = `https://duckduckgo.com/?q=${query}`;
}
</script>

<template>
  <div class="grid h-screen place-items-center">
    <div class="grid grid-cols-6 gap-5">
      <div
        class="col-span-3 text-center grid grid-cols-1 place-items-center mb-10"
      >
        <p class="font-bold text-8xl mb-2">{{ hh }}:{{ min }} {{ ampm }}</p>
        <p class="text-2xl">Hello {{ config.name }}!</p>
      </div>

      <div
        class="col-span-3 text-center grid grid-cols-1 place-items-center mb-10"
      >
        <p class="font-bold text-8xl mb-10">{{ mm }} {{ dd }}</p>
        <p class="text-2xl flex place-items-center gap-2">
          <IconComponent :name="weather.iconId" width="64" height="64" />
          {{ weather.temperature
          }}{{ config.unit === "celsius" ? " &#8451;" : " &#8457;" }}
        </p>
      </div>

      <a :href="config.icons[0].link" class="transition hover:scale-110">
        <div
          class="bg-ctp-mantle rounded text-center card grid place-items-center shadow-md shadow-[#0e0e1b]"
        >
          <IconComponent :name="config.icons[0].icon" width="32" height="32" />
        </div>
      </a>
      <a :href="config.icons[1].link" class="transition hover:scale-110">
        <div
          class="bg-ctp-mantle rounded text-center card grid place-items-center shadow-md shadow-[#0e0e1b]"
        >
          <IconComponent :name="config.icons[1].icon" width="32" height="32" />
        </div>
      </a>
      <a :href="config.icons[2].link" class="transition hover:scale-110">
        <div
          class="bg-ctp-mantle rounded text-center card grid place-items-center shadow-md shadow-[#0e0e1b]"
        >
          <IconComponent :name="config.icons[2].icon" width="32" height="32" />
        </div>
      </a>

      <div class="transition hover:-translate-y-1 row-span-3">
        <div
          class="bg-ctp-mantle rounded text-center list grid grid-cols-1 place-items-center shadow-md shadow-[#0e0e1b]"
        >
          <p class="scale-100 font-bold text-xl mt-3">
            <IconComponent
              :name="config.lists[0].titleicon"
              width="32"
              height="32"
            />
          </p>
          <a
            class="m-2 mt-0 mb-0 bg-ctp-crust listitem rounded-md transition shadow-md shadow-[#070711] hover:scale-110 flex flex-row justify-center items-center"
            :href="config.lists[0].items[0].link"
          >
            {{ config.lists[0].items[0].text }}
          </a>
          <a
            class="m-2 mt-0 mb-0 bg-ctp-crust listitem rounded-md transition shadow-md shadow-[#070711] hover:scale-110 flex flex-row justify-center items-center"
            :href="config.lists[0].items[1].link"
          >
            {{ config.lists[0].items[1].text }}
          </a>
          <a
            class="m-2 mt-0 mb-0 bg-ctp-crust listitem rounded-md transition shadow-md shadow-[#070711] hover:scale-110 flex flex-row justify-center items-center"
            :href="config.lists[0].items[2].link"
          >
            {{ config.lists[0].items[2].text }}
          </a>
          <a
            class="m-2 mt-0 mb-0 bg-ctp-crust listitem rounded-md transition shadow-md shadow-[#070711] hover:scale-110 flex flex-row justify-center items-center"
            :href="config.lists[0].items[3].link"
          >
            {{ config.lists[0].items[3].text }}
          </a>
        </div>
      </div>

      <div class="transition hover:-translate-y-1 row-span-3">
        <div
          class="bg-ctp-mantle rounded text-center list grid grid-cols-1 place-items-center shadow-md shadow-[#0e0e1b]"
        >
          <p class="scale-100 font-bold text-xl mt-3">
            <IconComponent
              :name="config.lists[1].titleicon"
              width="32"
              height="32"
            />
          </p>
          <a
            class="m-2 mt-0 mb-0 bg-ctp-crust listitem rounded-md transition shadow-md shadow-[#070711] hover:scale-110 flex flex-row justify-center items-center"
            :href="config.lists[1].items[0].link"
          >
            {{ config.lists[1].items[0].text }}
          </a>
          <a
            class="m-2 mt-0 mb-0 bg-ctp-crust listitem rounded-md transition shadow-md shadow-[#070711] hover:scale-110 flex flex-row justify-center items-center"
            :href="config.lists[1].items[1].link"
          >
            {{ config.lists[1].items[1].text }}
          </a>
          <a
            class="m-2 mt-0 mb-0 bg-ctp-crust listitem rounded-md transition shadow-md shadow-[#070711] hover:scale-110 flex flex-row justify-center items-center"
            :href="config.lists[1].items[2].link"
          >
            {{ config.lists[1].items[2].text }}
          </a>
          <a
            class="m-2 mt-0 mb-0 bg-ctp-crust listitem rounded-md transition shadow-md shadow-[#070711] hover:scale-110 flex flex-row justify-center items-center"
            :href="config.lists[1].items[3].link"
          >
            {{ config.lists[1].items[3].text }}
          </a>
        </div>
      </div>

      <div class="transition hover:-translate-y-1 row-span-3">
        <div
          class="bg-ctp-mantle rounded text-center list grid grid-cols-1 place-items-center shadow-md shadow-[#0e0e1b]"
        >
          <p class="scale-100 font-bold text-xl mt-3">
            <IconComponent
              :name="config.lists[2].titleicon"
              width="32"
              height="32"
            />
          </p>
          <a
            class="m-2 mt-0 mb-0 bg-ctp-crust listitem rounded-md transition shadow-md shadow-[#070711] hover:scale-110 flex flex-row justify-center items-center"
            :href="config.lists[2].items[0].link"
          >
            {{ config.lists[2].items[0].text }}
          </a>
          <a
            class="m-2 mt-0 mb-0 bg-ctp-crust listitem rounded-md transition shadow-md shadow-[#070711] hover:scale-110 flex flex-row justify-center items-center"
            :href="config.lists[2].items[1].link"
          >
            {{ config.lists[2].items[1].text }}
          </a>
          <a
            class="m-2 mt-0 mb-0 bg-ctp-crust listitem rounded-md transition shadow-md shadow-[#070711] hover:scale-110 flex flex-row justify-center items-center"
            :href="config.lists[2].items[2].link"
          >
            {{ config.lists[2].items[2].text }}
          </a>
          <a
            class="m-2 mt-0 mb-0 bg-ctp-crust listitem rounded-md transition shadow-md shadow-[#070711] hover:scale-110 flex flex-row justify-center items-center"
            :href="config.lists[2].items[3].link"
          >
            {{ config.lists[2].items[3].text }}
          </a>
        </div>
      </div>

      <a :href="config.icons[3].link" class="transition hover:scale-110">
        <div
          class="bg-ctp-mantle rounded text-center card grid place-items-center shadow-md shadow-[#0e0e1b]"
        >
          <IconComponent :name="config.icons[3].icon" width="32" height="32" />
        </div>
      </a>
      <a :href="config.icons[4].link" class="transition hover:scale-110">
        <div
          class="bg-ctp-mantle rounded text-center card grid place-items-center shadow-md shadow-[#0e0e1b]"
        >
          <IconComponent :name="config.icons[4].icon" width="32" height="32" />
        </div>
      </a>
      <a :href="config.icons[5].link" class="transition hover:scale-110">
        <div
          class="bg-ctp-mantle rounded text-center card grid place-items-center shadow-md shadow-[#0e0e1b]"
        >
          <IconComponent :name="config.icons[5].icon" width="32" height="32" />
        </div>
      </a>

      <input
        placeholder="Search..."
        aria-label="Search through site content"
        class="search bg-ctp-mantle rounded-full text-center shadow-md shadow-[#0e0e1b] col-span-3 transition hover:-translate-y-1 active:-translate-y-1"
        id="searchbar"
        v-on:keyup="(e: any) => e.key == 'Enter' && searchWeb(e.target.value)"
      />
    </div>
  </div>
</template>
