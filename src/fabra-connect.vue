<template>
  <div class="open-fabra-connect-button">
    <slot name="button" :onClick="open">
      <button @click="() => open()">
        <slot>Open Fabra Connect</slot> <!-- default content -->
      </button>
    </slot>
  </div>
</template>

<script lang="ts">
import { FabraConnect } from '@/types';
import { defineComponent } from 'vue';

declare global {
  interface Window {
    fabra: FabraConnect;
  }
}

export default defineComponent({
  props: {
    containerID: String,
    linkToken: { type: String, required: true },
    customTheme: Object,
    supportEmail: String,
    docsLink: String,
  },
  async created() {
    try {
      await this.loadScript("https://connect.fabra.io/initialize.js")
      await this.initialize();
    } catch (error) {
      console.error("Failed to load script initialize.js");
    }
  },
  async updated() {
    await this.initialize();
    if (this.containerID) {
      this.reattach(this.containerID);
    }
  },
  unmounted() {
    this.destroy();
  },
  methods: {
    async initialize() {
      await window.fabra.initialize({
        containerID: this.containerID,
        customTheme: JSON.parse(JSON.stringify(this.customTheme)),
        supportEmail: this.supportEmail,
        docsLink: this.docsLink,
      });
    },
    destroy() {
      if (window.fabra) {
        window.fabra.destroy();
      }
    },
    open() {
      window.fabra.open(this.$props.linkToken);
    },
    close() {
      if (window.fabra) {
        window.fabra.close();
      }
    },
    reattach(containerID: string) {
      if (window.fabra) {
        window.fabra.reattach(containerID);
      }
    },
    loadScript(src: string): Promise<void> {
      return new Promise(function (resolve, reject) {
        if (document.querySelector(`script[src="${src}"]`)) {
          resolve();
          return;
        }
        const el = document.createElement("script");
        el.type = "text/javascript";
        el.async = true;
        el.src = src;
        el.addEventListener("load", () => resolve());
        el.addEventListener("error", () => reject());
        el.addEventListener("abort", () => reject());
        document.head.appendChild(el);
      });
    },
  },
  watch: {
    $props: {
      deep: true,
      handler() {
        this.destroy()
        this.initialize();
      },
    }
  },
});
</script>
