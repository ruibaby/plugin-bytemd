<script setup lang="ts">
import "bytemd/dist/index.css";
import "github-markdown-css/github-markdown-light.css";
import { Editor } from "@bytemd/vue-next";
import gfm from "@bytemd/plugin-gfm";
import { getProcessor } from "bytemd";

const plugins = [gfm()];

const props = defineProps({
  raw: {
    type: String,
    required: false,
    default: "",
  },
  content: {
    type: String,
    required: false,
    default: "",
  },
});

const emit = defineEmits<{
  (event: "update:raw", value: string): void;
  (event: "update:content", value: string): void;
  (event: "update", value: string): void;
}>();

const handleChange = (v: string) => {
  emit("update:raw", v);
  emit("update", v);

  const processor = getProcessor({ plugins: plugins }).processSync(props.raw);

  emit("update:content", processor.toString());
};
</script>

<template>
  <Editor :value="raw" :plugins="plugins" @change="handleChange" />
</template>
<style>
.bytemd {
  height: 100%;
  border: none;
}
.bytemd.bytemd-fullscreen {
  z-index: 9999;
}
</style>
