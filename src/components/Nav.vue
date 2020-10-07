<template>
  <nav id="nav-bar">
    <a href="#" id="nav-logo">headsUP</a>
    <ul id="nav-links" :class="{ hidden: !navOpen, animate: navOpen }">
      <li><a href="#">Home</a></li>
      <li>
        <a href="#"><em>Ta</em>ke Quiz</a>
      </li>
      <li>
        <a href="#"><em>Ma</em>ke Quiz</a>
      </li>
    </ul>
    <!-- Burger animation needs to be fixed -> defocus with .blur() after click -->
    <div
      class="open-overlay"
      id="nav-burger"
      tabindex="0"
      @click="burgerClickHandler"
    >
      <span></span>
      <span></span>
      <span></span>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Nav",
  data() {
    return {
      navOpen: false,
    };
  },
  methods: {
    burgerClickHandler(event) {
      this.navOpen = !this.navOpen;
      setTimeout(() => {
        event.target.blur();
      }, 300);
    },
  },
};
</script>

<style scoped>
#nav-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100vw;
  background-color: rgb(102, 255, 166);
  opacity: 0.7;
  box-shadow: 0px 2px 7px;
  padding-left: 0.5rem;
}

#nav-bar > * {
  margin: 0 0.5rem;
  padding: 1rem;
}

.open-overlay {
  padding: 20px;
  cursor: pointer;
}

.open-overlay:hover {
  background-color: rgba(255, 255, 255, 0.2);
}

.open-overlay:focus {
  animation-name: rotateBurger;
  animation-duration: 300ms;
  background-color: rgba(255, 255, 255, 0);
  outline: none;
}

@keyframes rotateBurger {
  0% {
    transform: rotate(0deg);
  }
  50% {
    transform: rotate(-50deg);
  }
  100% {
    transfrom: rotate(0deg);
  }
}

.open-overlay span {
  display: block;
  width: 2.5rem;
  height: 0.25rem;
  margin: 0.25rem;
  background-color: black;
  border-radius: 2px;
  z-index: 1;
}

#nav-links {
  display: flex;
  list-style: none;
}

#nav-links li {
  margin: 0.25rem 1rem;
}

@media only screen and (max-width: 650px) {
  #nav-bar {
    flex-wrap: wrap;
    justify-content: space-around;
  }

  #nav-logo {
    order: 1;
  }

  #nav-burger {
    order: 2;
  }

  #nav-links {
    flex-direction: column;
    align-items: center;
    order: 3;
  }

  #nav-links li {
    margin: 1rem;
  }
}

a {
  padding: 0.7rem 1.2rem;
  background-color: rgba(0, 178, 72, 0.5);
  opacity: 0.7;
  box-shadow: 4px 3px;
  text-decoration: none;
}

/* Ease in #nav-links */
.hidden {
  display: none !important;
}

.animate {
  animation-name: animateNavLinks;
  animation-duration: 500ms;
}

@keyframes animateNavLinks {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
