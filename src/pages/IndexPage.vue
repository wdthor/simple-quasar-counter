<template>
  <q-page
    v-touch-pan.vertical.prevent.mouse="handlePan"
    class="row items-end justify-evenly text-white"
  >
    <section class="row full-width items-center">
      <div class="col text-center">
        <q-btn
          @click="decrease"
          v-touch-repeat:300:300:300:300:50.mouse="decrease"
          icon="remove"
          size="xl"
          round
        />
      </div>
      <div class="col text-center text-h2">{{ count }}</div>
      <div class="col text-center">
        <q-btn
          @click="increase"
          v-touch-repeat:300:300:300:300:50.mouse="increase"
          icon="add"
          size="xl"
          round
        />
      </div>
    </section>
    <section class="row">
      <q-btn @click="reset" icon="restart_alt" size="xl" round />
    </section>
  </q-page>
</template>

<script setup lang="ts">
import { onMounted, ref, watch } from 'vue';
import type { TouchPanValue } from 'quasar';
import { useQuasar } from 'quasar';

const $q = useQuasar();

const count = ref(0);

const increase = () => {
  count.value++;
};

const decrease = () => {
  if (count.value > 0) {
    count.value--;
  }
};

const reset = () => {
  count.value = 0;
};

const handlePan: TouchPanValue = (details) => {
  if (details.delta?.y !== undefined) {
    if (details.delta.y < 0) {
      increase();
    } else {
      decrease();
    }
  }
};

onMounted(() => {
  const counter = Number($q.localStorage.getItem('counter'));
  if (!isNaN(counter)) {
    count.value = counter;
  }
});

watch(count, (newVal) => {
  $q.localStorage.set('counter', newVal);
});
</script>

<style scoped>
.q-page {
  max-width: 700px;
  margin: 0 auto;
}
</style>
