<template>
  <div class="flex flex-col">
    <!-- Search bar-->
    <div class="relative m-2">
      <div
        class="absolute flex items-center h-full pl-4 text-gray-600 cursor-pointer"
      >
        <div class="w-6 h-6">
          <MagnifyingGlassIcon @click="handleSearch" />
        </div>
      </div>
      <input
        v-model="search"
        class="flex items-center w-full pl-12 text-sm font-normal text-black dark:text-gray-100 bg-gray-200 border border-gray-200 rounded-full shadow dark:bg-di¨m-400 dark:border-dim-400 focus:bg-gray-100 dark:focus:bg-dim-900 focus:outline-none focus:border-blue-200 h-9"
        type="text"
        placeholder="Search tweet"
      />
    </div>

    <!-- Preview Card: What's happening -->
    <SidebarRightPreviewCard title="What's happening">
      <SidebarRightPreviewCardItem
        v-for="whatsHappening in whatsHappeningItems"
        :key="whatsHappening.title"
      >
        <div>
          <h2 class="font-bold text-gray-800 text-md dark:text-white">
            {{ whatsHappening.title }}
          </h2>
          <p class="text-xs text-gray-400">
            {{ whatsHappening.count }}
          </p>
        </div>
      </SidebarRightPreviewCardItem>
    </SidebarRightPreviewCard>
    <!-- Preview Card: Who to follow -->
    <SidebarRightPreviewCard title="Who to follow">
      <SidebarRightPreviewCardItem
        v-for="whoToFollow in whoToFollowItems"
        :key="whoToFollow.name"
      >
        <div class="flex flex-row items-center justify-between p-2">
          <div class="flex flex-row">
            <img
              class="w-10 h-10 rounded-full"
              :src="whoToFollow.image"
              :alt="whoToFollow.name"
            />
            <div class="flex flex-col ml-2">
              <h1 class="text-sm font-bold text-gray-900 :dark:text-white">
                {{ whoToFollow.name }}
              </h1>
              <p class="text-xs text-gray-400">{{ whoToFollow.handle }}</p>
            </div>
          </div>
          <div class="flex h-full">
            <button
              class="px-4 py-2 font-bold text-xs text-white dark:text-black bg-black dark:bg-white rounded-full"
            >
              Follow
            </button>
          </div>
        </div>
      </SidebarRightPreviewCardItem>
    </SidebarRightPreviewCard>
    <footer>
      <ul class="mx-2 my-4 text-xs text-gray-500">
        <li class="inline-block mx-2">
          <a href="#" class="hover:underline" @click.prevent="handleToggleDarkMode">Dark mode</a>
        </li>
        <li class="inline-block mx-2">
          <a href="#" class="hover:underline">Privacy Policy</a>
        </li>
        <li class="inline-block mx-2">
          <a href="#" class="hover:underline">Cookie Policy</a>
        </li>
        <li class="inline-block mx-2">
          <a href="#" class="hover:underline">Ads Info</a>
        </li>
        <li class="inline-block mx-2">
          <a href="#" class="hover:underline">More</a>
        </li>
        <li class="inline-block mx-2">
          Vitovo © Twitter 2022
        </li>
      </ul>
    </footer>
  </div>
</template>
<script setup>
import { MagnifyingGlassIcon } from "@heroicons/vue/24/outline";
const search = ref("");
const emitter = useEmitter();

function handleSearch() {
  useRouter().push({
    path: '/search',
    query: {
      q: search.value
    }
  })
}

const whatsHappeningItems = ref([
  {
    title: "SpaceShit",
    count: "19.19k Tweets",
  },
  {
    title: "Googles",
    count: "6.66k Tweets",
  },
  {
    title: "MarcoPolo",
    count: "45.6k Tweets",
  },
  {
    title: "Nesla",
    count: "66.2k Tweets",
  },
]);
const whoToFollowItems = ref([
  {
    name: "John Rambo",
    handle: "@JohnRambo",
    image: "https://picsum.photos/200/200",
  },
  {
    name: "John Rambo",
    handle: "@JohnRambo",
    image: "https://picsum.photos/200/200",
  },
  {
    name: "John Rambo",
    handle: "@JohnRambo",
    image: "https://picsum.photos/200/200",
  },
]);

function handleToggleDarkMode(){
  emitter.$emit('toggleDarkMode')
}
</script>
