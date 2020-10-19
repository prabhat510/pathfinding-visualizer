<template>
  <div :class="`node ${obj.extraClassName}`" :id="`node-${x}-${y}`"></div>
</template>

<script>
export default {
  name: "Node",
  props: {
    x: {
      type: Number,
      required: true,
    },
    y: {
      type: Number,
      required: true,
    },
    obj: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {};
  },
  methods: {},
};
</script>

<style scoped>
.node {
  height: 25px;
  width: 25px;
  float: left;
  outline: 1px solid rgb(175, 216, 248);
}
.node-visited {
  animation-name: visitedAnimation;
  animation-duration: 1.5s;
  animation-timing-function: ease-out;
  animation-delay: 0;
  animation-direction: alternate;
  animation-iteration-count: 1;
  animation-fill-mode: forwards;
  animation-play-state: running;
}

@keyframes visitedAnimation {
  0% {
    transform: scale(0.3);
    background-color: rgba(0, 0, 66, 0.75);
    border-radius: 100%;
  }

  50% {
    background-color: rgb(46, 0, 0);
  }

  75% {
    transform: scale(1.2);
    background-color: brown;
  }

  100% {
    transform: scale(1);
    background-color: rgba(218, 0, 84, 0.863);
  }
}
.start-node {
  background: rgba(255, 255, 255, 0.89);
  background-image: url("../../public/triangletwo-right.jpg");
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: contain;
  background-repeat: no-repeat;
  position: static;
}
/* .start-node {
  background-image: url(https://image.shutterstock.com/image-vector/gpg-unique-monogram-style-vector-260nw-1678175218.jpg);
  background-color: rgb(255, 254, 106);
  background-position: center;
  background-repeat: no-repeat;
  background-size: contain;
  animation-name: specialNodes;
  animation-duration: 2s;
  animation-timing-function: ease-out;
  animation-delay: 0;
  animation-direction: alternate;
  animation-iteration-count: 1;
  animation-fill-mode: forwards;
  animation-play-state: running;
} */
/* @keyframes specialNodes {
  0% {
    transform: scale(0.3);
    /*background-color: darkslategrey;*/
/* } */

/* 50% {
    transform: scale(1.2); */
/*background-color: darkslategrey;*/
/* } */

/* 100% { */
/* transform: scale(1); */
/*background-color: darkslategrey;*/
/* } */
/* } */
.finish-node {
  background: rgba(255, 255, 255, 0.89);
  background-image: url("../../public/triangletwo-right.jpg");
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: contain;
  background-repeat: no-repeat;
}

.wall {
  background-color: black;
  transition: 0.2s ease-in;
  outline-color: black;
}
.white {
  background-color: white;
}
.node-shortest-path {
  animation-name: shortestPath;
  animation-duration: 1.5s;
  animation-timing-function: ease-out;
  animation-delay: 0;
  animation-direction: alternate;
  animation-iteration-count: 1;
  animation-fill-mode: forwards;
  animation-play-state: running;
}

@keyframes shortestPath {
  0% {
    transform: scale(0.6);
    background-color: rgb(255, 254, 106);
  }

  50% {
    transform: scale(1.2);
    background-color: rgb(255, 254, 106);
  }

  100% {
    transform: scale(1);
    background-color: rgb(255, 254, 106);
  }
}
</style>
