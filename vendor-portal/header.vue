<template>
  <div>
    <!-- off canvas menu -->
    <div id="off-canvas-menu" class="off-canvas position-left hide-for-smedium" data-off-canvas data-transition="overlap">
      <ul class="menu vertical">
        <li>
          <a href="#">Corporate</a>
        </li>
        <li>
          <a href="#">Food Program</a>
        </li>
        <li>
          <a href="#">Become a Member</a>
        </li>
        <li>
          <a href="#">Blog</a>
        </li>
      </ul>
      <hr />
      <ul class="menu vertical">
        <li>
          <a href="#">My Account</a>
        </li>
      </ul>
    </div>

    <nav id="main-nav">
      <div>
        <div class="grid-container">
          <div class="grid-x grid-margin-x">
            <div class="cell small-2 hide-for-smedium">
              <button
                id="canvas-menu-btn"
                class="menu-icon"
                type="button"
                data-toggle="off-canvas-menu"
                data-responsive-toggle="top-menu"
                data-hide-for="smedium"></button>
            </div>

            <div id="top-links" class="cell smedium-4 medium-4 show-for-smedium">
              <ul id="top-menu" class="menu vertical smedium-horizontal">
                <li>
                  <nuxt-link :to="'/about'">About Us</nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="'/customers'">Our Customers</nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="'/partners'">Our Partners</nuxt-link>
                </li>
                <li>
                  <a href="mailto:lrapacci@everythingfood.com">Email</a>
                </li>
              </ul>
            </div>

            <div class="cell small-8 smedium-4 medium-4 text-center">
              <a id="logo-link" href="/vendorportal">
                <span class="show-for-sr" aria-hidden="true">EverythingFood</span>
                <span class="new-logo"></span>
              </a>
            </div>

            <div id="cart-links" class="cell small-2 smedium-4 medium-4 text-right">
              <a class="shopping-basket" href="#">
                Orders
                <span class="items-badge hide-for-medium">0</span>
              </a>
              <small class="show-for-medium">0 items - $0.00</small>
              <span class="vertical-line show-for-smedium"></span>
              <a href="#" class="show-for-smedium">
                <span class="hide-for-medium">
                  <i class="fas fa-user fa-lg"></i>
                </span>
                <small v-if="navkey=='' || typeof navkey == undefined" class="show-for-medium">
                  <nuxt-link :to="'/signin'">Sign In</nuxt-link>
                </small>
                <small v-else class="show-for-medium">
                  <nuxt-link :to="'/vendorportal'">My Account</nuxt-link>
                </small>
              </a>
            </div>
          </div>
        </div>
      </div>
    </nav>
  </div>
</template>
<script>
// Library components.
import http from '~/utils/http';
import localStorage from '~/utils/storage/local-storage';

export default {
  props: {
    search: String,
  },
  data() {
    return {
      srhtext: '',
      username: 'saritasa@everythingfood.com',
      password: 'Apple2018!',
      loading: false,
      token: '',
      departmentdefslist: [],
    };
  },
  computed: {
    navkey() {
      return localStorage.get('ZNavKey') ? localStorage.get('ZNavKey') : '';
    },
  },
  created() {
    //do something after creating vue instance
    this.getdepartmentdefslist();
  },
  mounted() {
    this.srhtext = this.$store.state.srhtext;
  },
  methods: {
    menucontrol(flag) {
      if (flag === 1) {
        document.getElementById('mega-menu').style.display = 'none';
        document.getElementById('mega-menu').style.visibility = 'hidden';
      } else {
        document.getElementById('mega-menu').style.display = 'block';
        document.getElementById('mega-menu').style.visibility = 'visible';
      }
    },
    search1() {
      // Save the search keyword to store
      this.$store.commit('setSearchText', this.srhtext);
      this.$parent.search();
    },
    getdepartmentdefslist() {
      //get listitems
      let config = {
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded',
        },
      };

      this.loading = true;

      config = {
        headers: {
          'Content-Type': 'application/json',
        },
      };

      // Get DepartmentDefs-List.
      http.post(`${process.env.API_URL}/DepartmentDefs-List`, {
        config,
      }).then(response => {
        this.departmentdefslist = response.data.Items;
      });
    },
  },
};
</script>
<style scoped>
#main-nav .grid-x {
    background-position: center 100px;
    background-repeat: no-repeat;
}
</style>
