<script setup>
import BillMenu from "./OptionMenu/BillMenu.vue";
import Menu from "./OptionMenu/Menu.vue";
import { ref } from "vue";

const props = defineProps(["tableId", "tableState", "isOptionMenuOpen"]);

const emit = defineEmits(["update:tableState", "update:isOptionMenuOpen"]);

const isBillMenuOpen = ref(false);
const isOrderOpen = ref(false);

const handleReserveClick = () => {
  const newState = props.tableState === "Reserved" ? "Free" : "Reserved";
  emit("update:tableState", newState);
  emit("update:isOptionMenuOpen", false);
};
</script>

<template>
  <div class="background">
    <div class="optionMenu">
      <button @click="isOrderOpen = true">Order</button>
      <button @click="isBillMenuOpen = true">Bill</button>
      <BillMenu
        v-if="isBillMenuOpen"
        v-model:isBillMenuOpen="isBillMenuOpen"
        :tableId="tableId"
      />
      <button v-if="tableState != 'Busy'" @click="handleReserveClick">
        {{ tableState === "Reserved" ? "Unreserve" : "Reserve" }}
      </button>
      <button
        @click="emit('update:isOptionMenuOpen', false)"
        class="optionMenuClose"
      >
        cancel
      </button>
    </div>
    <Menu v-if="isOrderOpen" :tableId v-model:isOrderOpen="isOrderOpen" />
  </div>
</template>

<style scoped>
.background {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  min-height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
  width: 100%;
  backdrop-filter: blur(2px);
  z-index: 10;
}

.optionMenu {
  display: flex;
  flex-direction: column;
  gap: 1em;
  background-color: var(--background-color);
  padding: 2.5em;
}

.optionMenu button {
  font-size: 2rem;
  padding: 0.2em 0.5em;
}

.optionMenu .optionMenuClose {
  background: none;
  border: none;
  text-decoration: underline;
  font-size: 1.4rem;
  margin-top: 0.5em;
  color: var(--text-color);
}
</style>
