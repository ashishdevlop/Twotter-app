<template>
  <div class="user-profile_user-panel">
    <h1 class="user-profile_username">@{{ user.username }}</h1>
    <div class="user-profile_folloer-count">
      <strong>Followers:</strong> {{ followers }}
      <div class="user-profile"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: " UserProfile",
  data() {
    return {
      followers: 0,
      user: {
        id: 1,
        username: "_AshishSharma",
        firstName: "Ashish ",
        lastName: "Sharma",
        email: "ashishsharma.cse.204.bkbiet2023@gmail.com",
        isAdmin: true,
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
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
  },
  methods: {
    followUser() {
      this.followers++;
    },
  },
  mounted() {
    this.followUser();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;
  display: flex;
  flex-direction: column;
  text-align: center;
}
</style>
