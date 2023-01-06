<template>
  <div ref="mdContainer" class="mdContainer markdown-body"></div>
</template>

<script lang="ts">
import {onMounted, ref, Ref} from "vue";
import {marked} from "marked";
import hljs from "highlight.js";
import axios from "axios";

// import "../style/common.css"

import "jetbrains-mono-webfont/jetbrains-mono.css"
import "github-markdown-css/github-markdown.css"
import "highlight.js/styles/github.css"

export default {
  setup() {
    // const mdURL: string = "http://127.0.0.1:8888/%E4%BA%91%E8%B5%9B%E6%96%87%E6%A1%A3.md";
    const mdURL: string = "http://127.0.0.1:8888/lam.md";
    const mdContainer: Ref = ref();

    onMounted(() => {
      axios.get(mdURL)
          .then(
              (response) => {
                if (response.status !== 200) return null;
                return response.data
              }
          )
          .then(
              (data) => {
                if (!data) return;

                const renderMarkdown: marked.Renderer = new marked.Renderer();

                marked.setOptions({
                  renderer: renderMarkdown,
                  highlight: (code: string) => {
                    return hljs.highlightAuto(code).value;
                  }
                })

                mdContainer.value.innerHTML = marked(data as string)
              }
          )
    })

    return {
      mdContainer
    }
  }
}
</script>

<style scoped>
img {
  width: 100%;
}

.mdContainer {
  width: 60%;
  overflow: hidden;
  margin: 0 auto;
  font-family: "JetBrains Mono NL", "Noto Sans SC", serif;
}
</style>