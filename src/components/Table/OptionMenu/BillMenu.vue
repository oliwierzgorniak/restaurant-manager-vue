<script setup>
import { prices } from "@/data";
import { ref, inject, defineEmits } from "vue";
const link = ref(null);
const bills = inject("bills");

const props = defineProps({
  tableId: Number,
});

const emit = defineEmits(["update:isBillMenuOpen"]);

const downloadReceipt = (positions) => {
  let text = "Receipt\n";
  let totalAmount = 0;
  w;

  let groupedPositions = {};
  positions.forEach((position) => {
    if (!Object.hasOwn(groupedPositions, position)) {
      groupedPositions[position] = 0;
    }
    groupedPositions[position] = groupedPositions[position] + 1;
  });

  Object.keys(groupedPositions).forEach((name) => {
    const price = prices[name];
    const amount = groupedPositions[name];
    totalAmount += price * amount;
    text += `${name} - ${amount} × € ${price}\n`;
  });

  text += `\nTotal amount: € ${totalAmount}`;
  const blob = new Blob([text], { type: "text/plain" });

  const url = window.URL.createObjectURL(blob);

  console.log(link);
  link.value.href = url;
  link.value.download = "receipt.txt"; // Set the filename
  link.value.click();
};

const tableState = inject("tableState");
const isOptionMenuOpen = inject("isOptionMenuOpen");

const handleReceiptClick = (e) => {
  e.preventDefault();
  const positions = bills[props.tableId]
    .filter((position) => position.selected)
    .map((position) => position.name);
  if (bills[props.tableId].length === positions.length) {
    tableState.value = "Free";
    emit("update:isBillMenuOpen", false);
    isOptionMenuOpen.value = false;
  }
  bills[props.tableId] = bills[props.tableId].filter((position) => !position.selected);

  downloadReceipt(positions, link);
};
</script>

<template>
  <form class="form">
    <button
      v-if="bills[tableId].length > 0"
      @click="
        (e) => {
          e.preventDefault();
          bills[tableId].forEach((_, i) => {
            bills[tableId][i].selected = false;
          });
        }
      "
      class="selectButton"
    >
      Deselect All
    </button>
    <div>
      <label v-for="(position, i) in bills[tableId]" class="label">
        <input
          type="checkbox"
          :checked="bills[tableId][i].selected"
          @change="
            (e) => {
              bills[tableId][i].selected = e.target.checked;
            }
          "
        />
        {{ position.name }}
      </label>
      <span v-if="bills[tableId].length === 0">No things ordered</span>
    </div>
    <button @click="handleReceiptClick" class="receiptButton">Receipt</button>

    <a ref="link" style="display: 'none'"></a>
    <button
      @click="
        (e) => {
          e.preventDefault();
          emit('update:isBillMenuOpen', false);
        }
      "
      className="cancelButton"
    >
      Close
    </button>
  </form>
</template>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: var(--background-color);
  padding: 2.7em;
  font-size: 1.7rem;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  overflow-y: auto;
  width: 90vw;
  max-height: 90vh;
}

.label {
  padding: 0.3em;
  display: flex;
  gap: 0.4em;
  white-space: nowrap;
}

.label input {
  width: 1.7rem;
}

.form .selectButton {
  font-size: 1.5rem;
  margin-bottom: 0.7em;
}

.receiptButton {
  margin-top: 1em;
}

.form .cancelButton {
  background: none;
  border: none;
  text-decoration: underline;
  font-size: 1.4rem;
  margin-top: 0.7em;
  color: var(--text-color);
}
</style>
