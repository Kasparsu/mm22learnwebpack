<template>
    <div v-if="showHearts" class="heart-animation-container">
      <span class="heart" v-for="index in 5" :key="index">&#10084;</span>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      showHearts: {
        type: Boolean,
        required: true,
      },
    },
  };
  </script>
  
  <style>
  .heart-animation-container {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 1000;
    font-size: 24px;
  }
  
  .heart {
    animation: heartBeat 1s infinite;
    margin: 0 5px;
    color: red;
  }
  
  @keyframes heartBeat {
    0%, 100% {
      transform: scale(1);
    }
    50% {
      transform: scale(1.3);
    }
  }
  </style>
  