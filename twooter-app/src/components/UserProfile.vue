<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">Admin</div>
      <!-- <div class="user-profile__admin-badge" v-else-if="">Not Admin</div> -->
      <div class="user-profile__follower-count">
        <strong>Followers:</strong> {{ followers }}
      </div>
      <form
        class="user-profile__create-tweet"
        @submit.prevent="createNewTweet"
        :class="{ '--exceeded': newTweetCharacterCount >= 180 }"
      >
        <label for="newTweet"
          ><strong>New Tweet</strong>( {{ newTweetCharacterCount }}/180)</label
        >
        <textarea id="newTweet" row="4" v-model="newTweetContent" />

        <div class="user-profile__create-tweet-type">
          <label for="newTweetType"><strong>Type:</strong></label>
          <select :id="newTweetType" v-model="selectedTweetType">
            <option
              :value="option.value"
              v-for="(option, index) in TweetTypes"
              :key="index"
            >
              {{ option.name }}
            </option>
          </select>
        </div>
        <button>Tweet!</button>
      </form>
    </div>
    <div class="user-profile__tweets-wrapper">
      <Tweetitem
        v-for="tweet in user.tweets"
        v-bind:key="tweet.id"
        username="user.username"
        :tweet="tweet"
        @favourite="toggleFavourite"
      />
    </div>
  </div>
</template>

<script>
import Tweetitem from "./Tweetitem";

export default {
  name: "UserProfile",
  components: { Tweetitem },
  data() {
    return {
      newTweetContent: "",
      selectedTweetType: "instant",
      TweetTypes: [
        { value: "draft", name: "Draft" },
        { value: "instant", name: "Instant Tweet" },
      ],
      followers: 0,
      user: {
        id: 1,
        username: "_AshishSharma",
        firstName: "Ashish ",
        lastName: "Sharma",
        email: "ashishsharma.cse.204.bkbiet2023@gmail.com",
        isAdmin: true,
        tweets: [
          {
            id: 1,
            content: "Tweeter is amazing!",
          },
          {
            id: 2,
            content: "Don't forgot to subscribe to  The Earth is Square!",
          },
        ],
      },
    };
  },
  watch: {
    followers(newfollowerCount, oldfollowerCount) {
      if (oldfollowerCount < newfollowerCount) {
        console.log(`${this.user.username} has gained a follower!`);
      }
    },
  },
  computed: {
    newTweetCharacterCount() {
      return this.newTweetContent.length;
    },
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFavourite(id) {
      console.log(`Favourited   tweet  #${id}?`);
    },
    createNewTweet() {
      if (this.newTweetContent && this.selectedTweetType !== "draft") {
        this.user.tweets.unshift({
          id: this.user.tweets.length + 1,
          content: this.newTweetContent,
          // other tweet properties
        });
        this.newTweetContent = ""; // Clear the new tweet content input
      }
    },
  },
  mounted() {
    this.followUser();
  },
};
</script>

<style lang="scss" scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: 100%;
  padding: 50px 5%;
  .user-profile__user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #dfe3e8;

    h1 {
      margin: 0;
    }

    .user-profile__user-panel {
      display: flex;
      flex-direction: column;
      margin-right: 50px;
      padding: 20px;
      background-color: white;
      border-radius: 5px;
      border: 1px solid #dfe3e8;
    }

    .user-profile__create-tweet {
      padding-top: 20px;
      display: flex;
      flex-direction: column;

      &:--exceeded {
        color: red;
        border-color: red;

        button {
          background-color: red;
          border: none;
          color: white;
        }
      }
    }
  }
  .user-profile__tweets-wrapper {
    display: grid;
    grid-gap: 10px;
  }
}
</style>
