<template>
  <div
      v-show="this.$store.state.config.layout === 'landing'"
      class="landing-bg h-100 bg-gradient-primary position-fixed w-100"
  ></div>
  <sidenav
      :custom_class="this.$store.state.config.mcolor"
      :class="[
      this.$store.state.config.isTransparent,
      this.$store.state.config.isRTL ? 'fixed-end' : 'fixed-start'
    ]"
      v-if="this.$store.state.config.showSidenav"
  />
  <main
      class="main-content position-relative max-height-vh-100 h-100 border-radius-lg"
  >
    <navbar
        :class="[navClasses]"
        :textWhite="
        this.$store.state.config.isAbsolute ? 'text-white opacity-8' : 'text-white'
      "
        :minNav="navbarMinimize"
        v-if="this.$store.state.config.showNavbar"
    />
    <router-view/>
  </main>
</template>
<script>
import Sidenav from "./widgets/Sidenav";
import Navbar from "@/widgets/Navbars/Navbar.vue";
import {mapMutations, mapActions} from "vuex";

export default {
  name: "App",
  components: {
    Sidenav,
    Navbar,
  },
  methods: {
    ...mapMutations(["config/toggleConfigurator", "config/navbarMinimize"]),
    ...mapActions({
      fetchUsers: "users/fetchUsers",
      fetchSpecialists: "specialists/fetchSpecialists",
      fetchOrganizations: "organizations/fetchOrganizations",
      fetchTaskTypes: "taskTypes/fetchTaskTypes",
    }),
  },
  computed: {
    navClasses() {
      return {
        "position-sticky bg-white left-auto top-2 z-index-sticky":
            this.$store.state.config.isNavFixed && !this.$store.state.config.darkMode,
        "position-sticky bg-default left-auto top-2 z-index-sticky":
            this.$store.state.config.isNavFixed && this.$store.state.config.darkMode,
        "position-absolute px-4 mx-0 w-100 z-index-2": this.$store.state.config
            .isAbsolute,
        "px-0 mx-4": !this.$store.state.config.isAbsolute
      };
    }
  },
  async mounted() {
    await this.fetchUsers({page: 1});
    await this.fetchSpecialists();
    await this.fetchOrganizations();
    await this.fetchTaskTypes();
  },
  beforeMount() {
    this.$store.state.config.isTransparent = "bg-transparent";
  },

};
</script>
