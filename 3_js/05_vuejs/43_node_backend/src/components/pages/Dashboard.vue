<template lang="html">
  <main id="main" class="dashboard">
    <h1 class="title">DASHBOARD</h1>
    <div id="intro">
      <avatar></avatar>
      <about-me></about-me>
    </div>
    <h2 class="title">Users</h2>
    <tabler
       :editable="false"
       :deletable="true"
       :callbackDelete="tablerDelete"
       :callbackSort="tablerSort"
       :headerRows="[
           {name: 'id', sortable: true},
           {name: 'mail', sortable: true},
           {name: 'avatar', sortable: true},
           {name: 'about', sortable: true},
           {name: 'delete', sortable: false},
       ]"
       :bodyRows="users">
    </tabler>
  </main>
</template>

<script>
// import { mapGetters } from "vuex";
import { EventBus } from "./../../event-bus.js";
import Avatar from "./../dashboard-elements/Avatar.vue";
import AboutMe from "./../dashboard-elements/AboutMe.vue";
import Tabler from "./../tabler/Tabler.vue";

export default {
  created() {
    this.$store.dispatch("users/get")
    // this.$store.dispatch('getUser', 78);
    EventBus.$emit("message-from-app", null);
  },
  components: {
    AboutMe,
    Avatar,
    Tabler
  },
  computed: {
    users() {
      return this.$store.getters["users/all"]
    },
  },
  methods: {
    tablerDelete(evt, id) {
      console.log("tablerDelete@Dashboard");
      this.$store.dispatch("deleteUser", id)
      .then(res => {
        console.log(res);
        this.$store.dispatch("users/get")
      }, err => {
        console.error(err);
      });
    },
    tablerSort(v) {}
  }
}
</script>

<style lang="scss" scoped>
#intro {
  display: grid;
  grid-template-columns: 70px 1fr;
  grid-column-gap: 20px;
}
</style>
