# Fabra Connect Vue SDK

## Installation
`npm i @fabra/vue-fabra-connect`

## Usage
```vue
<script>
import FabraConnect from "@fabra/vue-fabra-connect";
export default {
  components: { FabraConnect },
  data() {
    return {
      customTheme: {
        colors: {
          primary: {
            base: "#805AD5", // Default color for buttons, graphics, etc
            hover: "#553C9A", // Hover color for buttons and links
            text: "#FFFFFF", // The font color on top of the primary color
          }
        }
      }
    }
  }
};
</script>
<template>
  <fabra-connect :linkToken="ADD_GENERATED_LINK_TOKEN" :customTheme="customTheme">
    <template #button="props">
      <button @click="props.onClick">Open Fabra Connect</button>
    </template>
  </fabra-connect>
</template>
```
