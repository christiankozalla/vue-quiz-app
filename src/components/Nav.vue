<template>
  <nav id="nav-bar">
    <a href="#" id="nav-logo">headsUP</a>
    <ul id="nav-links" class="hidden">
      <li><a href="#">Home</a></li>
      <li><a href="#">Why headsUP</a></li>
      <li><a href="#">Start Quiz</a></li>
    </ul>
    <div class="open-overlay" tabindex="0" @click="burgerClickHandler">
      <span></span>
      <span></span>
      <span></span>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Nav",
};
</script>

<style scoped>
#nav-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100vw;
  background-color: #00b8d4;
  opacity: 0.7;
  box-shadow: 0px 2px 7px;
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
  #nav-links {
    flex-direction: column;
    align-items: center;
  }

  #nav-links li {
    margin: 1rem;
  }
}

a {
  padding: 0.7rem 1.2rem;
  background-color: #62ebff;
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

<script>
export default {
  name: "Nav",
  methods: {
    burgerClickHandler() {
      const navLinks = document.getElementById("nav-links");
      console.log(navLinks.classList);
      if (navLinks.classList.contains("hidden")) {
        navLinks.classList.remove("hidden");
        navLinks.classList.add("animate");
      } else if (navLinks.classList.contains("animate")) {
        navLinks.classList.add("hidden");
        navLinks.classList.remove("animate");
      }
    },
  },
};
</script>
