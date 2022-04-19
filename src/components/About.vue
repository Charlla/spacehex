<template>
  <div class="q-pa-md q-gutter-sm text-body1">
      <h5>About</h5>
      <p>Welcome to SpaceHex - a little website exploring amongst other features:
        <ul>
          <li>SpaceX API</li>
          <li>Vue 3 and Quasar</li>
          <li>GraphQL in Vue (using Apollo base)</li>
        </ul>
      </p>
      <h5>Features and Nav</h5>
        <ul>
          <li>For coolness sake, content will scroll into the main screen when the page is loaded</li>
          <li>Click on any bar for a mission or next to the mission video to open an image gallery for the mission.</li>
          <li>Have fun and enjoy this un-Elongated take on a SpaceX timeline...</li>
        </ul>
      <b>SpaceHex 2022</b>
      </div>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "About",
});
</script>
