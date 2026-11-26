<template>
  <div class="row q-gutter-sm">
    <q-btn
      class="col-shrink c-btn"
      v-if="variable"
      icon="mdi-numeric-negative-1"
      @click="decrement"
      flat
      dense
      rounded
    />
    <div class="col-grow row no-wrap resource-bar">
      <q-btn v-for="n in opts" :key="n.value" class="col resource-cell" dense unelevated
        :label="n.label" :style="cellStyle(Number(n.value))"
        :aria-pressed="Number(data.value) === Number(n.value)"
        @click="selectValue(Number(n.value))" />
    </div>
    <q-btn
      class="col-shrink c-btn"
      v-if="variable"
      icon="mdi-numeric-positive-1"
      @click="increment"
      flat
      dense
      rounded
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType, ref, watch, computed } from 'vue';
import { IMomentum, ISelectOpt, ITrack } from 'src/components/models';

export default defineComponent({
  name: 'ResourceTrack',
  props: {
    modelValue: {
      type: Object as PropType<ITrack | IMomentum>,
      required: true,
    },
    tone: { type: String, default: 'primary' },
    reverse: {
      type: Boolean,
    },
    variable: {
      type: Boolean,
    },
  },
  emits: ['update:modelValue'],
  setup(props, ctx) {
    const data = ref(props.modelValue);
    watch(
      () => props.modelValue,
      () => (data.value = props.modelValue),
      { deep: true }
    );

    const updateValue = () => ctx.emit('update:modelValue', data.value);
    const selectValue = (n: number) => { data.value.value = n; updateValue(); };
    const cellStyle = (n: number) => {
      const v = Number(data.value.value);
      const negative = Number(data.value.min) < 0 && v < 0;
      const filled = negative ? n >= v && n <= 0 : n >= 0 && n <= v;
      const colors: Record<string, string> = { health: '#dba0a0', spirit: '#a5c4e4', supply: '#e6d18e' };
      return { background: filled ? (negative ? '#b9a0d5' : colors[props.tone] || 'var(--q-primary)') : 'rgba(128,128,128,.12)', color: filled ? (negative || colors[props.tone] ? '#292929' : '#fff') : 'inherit', fontWeight: n === v ? 'bold' : 'normal', boxShadow: n === v ? 'inset 0 0 0 2px currentColor' : 'none' };
    };

    const opts = computed((): ISelectOpt[] => {
      const out: ISelectOpt[] = [];
      for (let i = data.value.min; i <= data.value.max; i++) {
        const o: ISelectOpt = { label: i, value: i };
        out.push(o);
      }
      return out;
    });

    const decrement = () => {
      data.value.max--;
    };

    const increment = () => {
      data.value.max++;
    };

    return {
      data,
      opts,
      cellStyle,
      selectValue,
      increment,
      decrement,
      updateValue,
    };
  },
});
</script>

<style lang="sass">
.c-btn
  background-color: $primary
.resource-bar
  gap: 2px
.resource-cell
  min-width: 0
  padding: 2px 0
  font-size: 12px
</style>
