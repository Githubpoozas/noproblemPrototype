<template>
  <div
    class="header"
    id="header"
    :class="{ headerTran: isNavTran, hover: isNavHover || panelOpen }"
    @mouseenter="navIsHover"
    @mouseleave="navNotHover"
  >
    <div class="header__menu" @mouseenter="menuEnter" @mouseleave="menuLeave">
      <svg width="32" height="32">
        <path
          class="header__menu__icon"
          fill="#000"
          fill-rule="evenodd"
          d="M4 9h25v.5H4V9zm0 7.2h25v.5H4v-.5zm.3 7.3H4v.5h25v-.5H4.3z"
          clip-rule="evenodd"
        ></path>
      </svg>
    </div>
    <div class="header__logo">
      <!-- <a href="" class="header__logo-link">
        <img class="header__logo-img" src="@/assets/img/logo_brand.svg" />
      </a> -->
      <router-link to="/">
        <img class="header__logo-img" src="@/assets/img/logo_brand.svg"
      /></router-link>
    </div>
    <div class="header__search">
      <a href="#" class="header__search__link">
        <span class="header__search__link-label">search</span>
        <span class="header__search__link-line"></span>
      </a>
    </div>
    <div class="header__user">
      <ul class="header__user__menu">
        <li class="header__user__menu-login">
          <a href="#">LOG IN</a>
        </li>
        <li class="header__user__menu-help">
          <a href="#">HELP</a>
        </li>
        <li class="header__user__menu-cart">
          <a href="#" class="header__user__menu-cart-link">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="32"
              height="32"
              viewBox="0 0 32 32"
            >
              <path
                fill="#000"
                fill-rule="nonzero"
                d="M12 7V6c0-1 1-2 2-2h4c1 0 2 1 2 2v1h7v19H5V7h7zm14 1H6v17h20V8zM13 6v1h6V6c0-.5-.5-1-1-1h-4c-.5 0-1 .5-1 1z"
              ></path>
            </svg>
            <span class="header__user__menu-cart-link-item">{{ cartNum }}</span>
          </a>
        </li>
      </ul>
    </div>
    <CollectionFilter
      :panelOpen="panelOpen"
      @panelOpening="panelOpen = !panelOpen"
      v-if="isCollection"
    ></CollectionFilter>
  </div>
</template>

<script>
import { eventBus } from "../main";
// import router from "../router";
import CollectionFilter from "@/components/item/CollectionFilter.vue";

export default {
  data() {
    return {
      menuHover: false,
      headerTran: false,
      panelOpen: false,
    };
  },
  components: {
    CollectionFilter,
  },
  methods: {
    menuEnter() {
      this.menuHover = true;
      eventBus.$emit("munuHoverUpdate", this.menuHover);
    },
    menuLeave() {
      this.menuHover = false;
      eventBus.$emit("munuHoverUpdate", this.menuHover);
    },
    handleScroll() {
      // let head = document.getElementById("header");
      // console.log(head.offsetHeight);

      let head = document.getElementById("header");
      let match = document.getElementById("match__section");

      let headBound = head.getBoundingClientRect();
      let matchBound = match.getBoundingClientRect();

      console.log(headBound.bottom + ":" + matchBound.top);

      // if (headBound.bottom > matchBound.top) {
      //   head.style.backgroundColor = "white";
      // } else {
      //   head.style.backgroundColor = "transparent";
      // }
    },
    navIsHover() {
      this.$store.commit("NAV_IS_HOVER");
    },
    navNotHover() {
      this.$store.commit("NAV_NOT_HOVER");
    },
  },
  computed: {
    isNavHover() {
      return this.$store.getters.getNavHover;
    },
    isNavTran() {
      let routeName = this.$route.name;
      // console.log(routeName);
      if (routeName == "Home" || routeName == "Collection") {
        return true;
      }
      return false;
    },
    isCollection() {
      if (this.$route.name == "Collection") {
        return true;
      }
      return false;
    },
    cartNum() {
      return this.$store.getters.getCartNum;
    },
  },

  // watch: {
  //   $route: function() {
  //     let routeName = this.$route.name;
  //     console.log(routeName);
  //     if (routeName == "Home" || routeName == "Collection") {
  //       this.headerTran = true;
  //     } else {
  //       this.headerTran = false;
  //     }
  //   },
  // },

  created() {
    // window.addEventListener("scroll", this.handleScroll);
  },
  destroyed() {
    // window.removeEventListener("scroll", this.handleScroll);
  },
};
</script>

<style lang="scss" scoped>
.header {
  // position: sticky;
  position: fixed;
  top: 0;
  z-index: 10;
  width: 100%;
  display: grid;
  grid-template-columns: minmax(65px, 5%) 20% 52% 23%;
  grid-template-rows: 1fr;
  justify-items: center;
  align-items: center;
  padding-top: 25px;
  padding-bottom: 5px;
  overflow: hidden;
  background-color: white;
  transition: background-color 0.3s;

  &.headerTran {
    background-color: transparent;

    transition: background-color 0.3s;

    &:hover {
      background-color: white;
    }
  }

  &.hover {
    background-color: white;
  }

  span {
    font-weight: 700;
  }

  &__menu {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  &__logo {
    height: 100%;
    width: 100%;
    display: flex;

    &-img {
      width: 340px;
    }
  }

  &__search {
    &__link {
      &-label {
        font-size: 2rem;
        margin-right: 1rem;
        text-transform: uppercase;
      }
      &-line {
        border-bottom: 1px solid black;
        width: 13rem;
        display: inline-block;
      }
    }
  }

  &__user {
    display: flex;
    justify-content: flex-end;

    width: 100%;
    padding-right: 40px;

    &__menu {
      display: flex;
      justify-content: flex-end;
      align-items: center;

      li {
        list-style: none;
        padding-left: 2rem;
        font-size: 1.7rem;

        a {
          font-size: 1.2rem;
        }
      }

      &-cart {
        padding-top: 2px;
        &-link {
          display: block;
          text-align: center;
          cursor: pointer;
          position: relative;
          padding-left: 1.6rem;

          svg {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -53%);
          }
          &-item {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 1rem;
          }
          & span {
            font-weight: 400;
          }
        }
      }
    }
  }
}
</style>
