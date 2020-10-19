<template>
  <div class="nav-bar">
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <a class="navbar-brand" href="#">Pathfinding Visualizer</a>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarText"
        aria-controls="navbarText"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarText">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item">
            <a class="nav-link" href="#">
              Home
              <span class="sr-only">(current)</span>
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#" @click="clearBoard">Clear Board</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Pricing</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#"></a>
          </li>
          <li class="nav-item">
            <button :class="`btn ${className}`" @click="handleClick">
              Visualize!
            </button>
          </li>
        </ul>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  name: "Navbar",
  data() {
    return {
      className: "btn-primary",
      pressed: false,
    };
  },
  methods: {
    handleClick() {
      if (!this.pressed) {
        this.pressed = true;
        this.className = "btn-danger";
        // document.removeEventListener("click", DisableClickOnPage.handler, true);
        this.$emit("visualize-dijktras");
      }
    },
    clearBoard() {
      document.getElementsByClassName("btn btn-danger").className =
        "btn btn-primary";
      this.pressed = false;
      this.$emit("clear-board");
    },
  },
};
</script>

<style scoped>
.nav-bar {
  font-family: Cambria, Cochin, Georgia, Times, "Times New Roman", serif;
  width: 100%;
  position: absolute;
  top: 0;
}
.btn:focus {
  box-shadow: none !important;
}
</style>
