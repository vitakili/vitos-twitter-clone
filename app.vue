<template>
  <div :class="{ dark: darkMode }">
    <div class="bg-white dark:bg-dim-900">
      <LoadingPage v-if="isAuthLoading" />

      <!-- App-->
      <div v-else-if="user" class="min-h-full">
        <div
          class="grid grid-cols-12 mx-auto sm:px-6 lg:max-w-7xl lg:px-8 lg:gap-5"
        >
          <!-- Left Sidebar -->
          <div class="hidden md:block xs-col-span-1 xl:col-span-2">
            <div class="sticky top-0">
              <SidebarLeft
                :user="user"
                @on-logout="handleUserLogout"
                @on-tweet="handleOpenTweetModal"
              />
            </div>
          </div>
          <!-- Main Content-->
          <div class="col-span-12 md:col-8 xl:col-span-6">
            <router-view />
          </div>
          <!-- Right Sidebar-->
          <div class="hidden md:block xl:col-span-4 md:col-span-3">
            <div class="sticky top-0">
              <SidebarRight />
            </div>
          </div>
        </div>
      </div>

      <AuthPage v-else />

      <UIModal :isOpen="postTweetModal" @on-close="handleModalClose">
        <TweetForm
          :replyTo="replyTweet"
          showReply
          :user="user"
          @onSuccess="handleFormSuccess"
        />
      </UIModal>
    </div>
  </div>
</template>
<script setup>
const darkMode = ref(false);
const { useAuthUser, initAuth, useAuthLoading, logout } = useAuth();
const isAuthLoading = useAuthLoading();
const user = useAuthUser();
const {
  closePostTweetModal,
  openPostTweetModal,
  usePostTweetModal,
  useReplyTweet,
} = useTweets();

const postTweetModal = usePostTweetModal();
const emitter = useEmitter();
const replyTweet = useReplyTweet();

emitter.$on("replyTweet", (tweet) => {
  openPostTweetModal(tweet);
});
emitter.$on("toggleDarkMode", () => {
  darkMode.value = !darkMode.value
});

onBeforeMount(() => {
  initAuth();
});

function handleFormSuccess(tweet) {
  closePostTweetModal();
  navigateTo({
    path: `/status/${tweet.id}`,
  });
};
function handleModalClose() {
  closePostTweetModal();
};
function handleOpenTweetModal() {
  openPostTweetModal(null);
};
function handleUserLogout() {
  logout();
};
</script>
