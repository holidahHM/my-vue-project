<template>
  <base-nav
    container-classes="container-fluid"
    class="navbar-top navbar-expand"
    :class="{ 'navbar-dark': type === 'default' }"
  >
    <!--Page heading removed to cater for more submenus in the layout-->
    <a
      href="#"
      aria-current="page"
      class="h4 mb-0 text-white text-uppercase d-none d-lg-inline-block active router-link-active"
    >
      {{ $route.name }}
    </a>
    <!-- Navbar links -->
    <b-navbar-nav class="align-items-center ml-md-auto">
      <!-- This item dont have <b-nav-item> because item have data-action/data-target on tag <a>, wich we cant add -->
    </b-navbar-nav>
    <b-navbar-nav class="align-items-center ml-auto ml-md-0">
      <b-nav-item to="/homepage">
        <i class="ni ni-planet"></i>
        <span class="nav-link-inner--text">Home</span>
      </b-nav-item>
      <b-nav-item to="/register" v-if="loggedOut">
        <i class="ni ni-circle-08"></i>
        <span class="nav-link-inner--text">Register</span>
      </b-nav-item>
      <b-nav-item to="/login" v-if="loggedOut">
        <i class="ni ni-key-25"></i>
        <span class="nav-link-inner--text">Login</span>
      </b-nav-item>
      <b-nav-item to="/profile">
        <i class="ni ni-single-02"></i>
        <span class="nav-link-inner--text">Profile</span>
      </b-nav-item>
      <base-dropdown class="nav-item" menu-on-right tag="li" title-tag="a">
        <a
          slot="title-container"
          class="nav-link nav-link-icon"
          href="#"
          role="button"
          aria-haspopup="true"
          aria-expanded="false"
        >
          <i class="ni ni-bell-55"></i>
        </a>

        <a class="dropdown-item" href="#">Action item#1</a>
        <a class="dropdown-item" href="#">Action item#2</a>
        <div class="dropdown-divider"></div>
        <a class="dropdown-item" href="#">Something else here</a>
      </base-dropdown>
      <base-dropdown
        menu-on-right
        class="nav-item"
        tag="li"
        title-tag="a"
        title-classes="nav-link pr-0"
      >
        <a href="#" class="nav-link pr-0" @click.prevent slot="title-container">
          <b-media no-body class="align-items-center">
            <span class="avatar avatar-sm rounded-circle">
              <img alt="Image placeholder" src="img/theme/team-4.jpg" />
            </span>
            <b-media-body class="ml-2 d-none d-lg-block">
              <span class="mb-0 text-sm font-weight-bold"> {{ userfullname }} </span>
            </b-media-body>
          </b-media>
        </a>

        <template>
          <b-dropdown-header class="noti-title">
            <h6 class="text-overflow m-0">Welcome!</h6>
          </b-dropdown-header>
          <b-dropdown-item href="#!">
            <i class="ni ni-single-02"></i>
            <span>My profile</span>
          </b-dropdown-item>
          <b-dropdown-item href="#!">
            <i class="ni ni-settings-gear-65"></i>
            <span>Settings</span>
          </b-dropdown-item>
          <b-dropdown-item href="#!">
            <i class="ni ni-calendar-grid-58"></i>
            <span>Activity</span>
          </b-dropdown-item>
          <b-dropdown-item href="#!">
            <i class="ni ni-support-16"></i>
            <span>Support</span>
          </b-dropdown-item>
          <div class="dropdown-divider"></div>
          <b-dropdown-item @click="onLogout()">
            <i class="ni ni-user-run"></i>
            <span>Logout</span>
          </b-dropdown-item>
        </template>
      </base-dropdown>
    </b-navbar-nav>
  </base-nav>
</template>

<script>
import { CollapseTransition } from "vue2-transitions";
import { BaseNav, Modal } from "@/components";

export default {
  components: {
    CollapseTransition,
    BaseNav,
    Modal,
  },
  props: {
    type: {
      type: String,
      default: "default", // default|light
      description: "Look of the dashboard navbar. Default (Green) or light (gray)",
    },
  },
  computed: {
    routeName() {
      const { name } = this.$route;
      return this.capitalizeFirstLetter(name);
    },
  },
  data() {
    return {
      activeNotifications: false,
      showMenu: false,
      searchModalVisible: false,
      searchQuery: "",
      userfullname: "",
      loggedIn: false,
      loggedOut: false,
    };
  },
  methods: {
    capitalizeFirstLetter(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
    toggleNotificationDropDown() {
      this.activeNotifications = !this.activeNotifications;
    },
    closeDropDown() {
      this.activeNotifications = false;
    },
    onLogout() {
      let userloggedin = JSON.parse(localStorage.getItem("userData"));
      if (userloggedin) {
        localStorage.removeItem("userData");
        this.$router.push("/login");
      }
    },
  },
  mounted() {
    if (localStorage.getItem("userData") !== null) {
      this.userfullname = "";
      let items = [JSON.parse(localStorage.getItem("userData"))];
      console.log("userdata", items);
      for (let i in items) {
        this.userfullname = items[i].fullname;
      }
      return (this.loggedOut = false);
    } else {
      return (this.loggedOut = true);
    }
  },
};
</script>
