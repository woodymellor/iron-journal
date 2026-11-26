<template>
  <div>
    <div class="row justify-center q-mb-md">
      <q-btn label="Begin a Session" flat dense @click="rollAll" :disable="!clocks.length" />
    </div>
    <q-card v-for="clock in clocks" :key="clock.id" flat bordered class="q-pa-sm q-mb-md">
      <div class="row items-center q-col-gutter-sm">
        <div class="col">
          <q-input v-model="clock.name" label="Name" dense />
          <div class="row q-gutter-sm items-center">
            <q-select :model-value="clock.segments" :options="[4, 6, 8, 10]" label="Segments" dense @update:model-value="resize(clock, $event)" />
            <q-select v-model="clock.advance" :options="odds" label="Advance" dense class="col" />
          </div>
          <div class="row items-center q-gutter-xs q-mt-sm">
            <q-btn flat dense icon="remove" aria-label="Remove segment" @click="clock.filled = Math.max(0, clock.filled - 1)" />
            <q-btn flat dense icon="add" aria-label="Add segment" @click="advance(clock)" />
            <q-btn flat dense icon="mdi-dice-6" :label="clock.roll ? String(clock.roll) : ''" :disable="clock.advance === 'No Roll' || clock.filled >= clock.segments" @click="roll(clock)" />
            <q-btn flat dense icon="refresh" aria-label="Reset clock" @click="clock.filled = 0; clock.roll = 0" />
            <q-btn v-if="config.data.edit" flat dense icon="delete" aria-label="Delete clock" @click="remove(clock.id)" />
          </div>
        </div>
        <div class="col-auto">
          <svg viewBox="0 0 100 100" width="100" height="100" role="img" :aria-label="clock.filled + ' of ' + clock.segments + ' segments'">
            <path v-for="n in clock.segments" :key="n" :d="wedge(n - 1, clock.segments)" :fill="n <= clock.filled ? '#c44b4b' : '#444'" stroke="#ddd" stroke-width="1.5" />
            <circle cx="50" cy="50" r="15" fill="#222" />
            <text x="50" y="55" fill="white" text-anchor="middle" font-size="14">{{ clock.segments - clock.filled }}</text>
          </svg>
          <div class="text-center text-caption">{{ clock.filled === clock.segments ? 'Complete' : clock.filled + '/' + clock.segments }}</div>
        </div>
      </div>
    </q-card>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue';
import { v4 as uuid } from 'uuid';
import { IClock } from 'src/components/models';
import { useCampaign } from 'src/store/campaign';
import { useConfig } from 'src/store/config';

export default defineComponent({
  name: 'CampaignClocks',
  setup() {
    const campaign = useCampaign();
    const config = useConfig();
    const clocks = computed(() => campaign.data.clocks || []);
    const chances: Record<string, number> = {
      'Almost Certain': 90, Likely: 75, 'Fifty-fifty': 50, Unlikely: 25, 'Small Chance': 10, 'No Roll': 0,
    };
    const odds = Object.keys(chances);
    const add = () => {
      if (!campaign.data.clocks) campaign.data.clocks = [];
      campaign.data.clocks.unshift({ id: uuid(), name: '', segments: 4, filled: 0, advance: 'Likely', roll: 0 });
    };
    const remove = (id: string) => {
      if (campaign.data.clocks) campaign.data.clocks = clocks.value.filter(c => c.id !== id);
    };
    const resize = (clock: IClock, segments: number) => {
      clock.segments = segments;
      clock.filled = Math.min(clock.filled, segments);
    };
    const advance = (clock: IClock, amount = 1) => {
      clock.filled = Math.min(clock.segments, clock.filled + amount);
    };
    const roll = (clock: IClock) => {
      if (clock.advance === 'No Roll' || clock.filled >= clock.segments) return;
      const n = Math.floor(Math.random() * 100) + 1;
      clock.roll = n;
      if (n > 100 - (chances[clock.advance] || 0)) advance(clock, n === 100 || n % 11 === 0 ? 2 : 1);
    };
    const rollAll = () => clocks.value.forEach(roll);
    const wedge = (i: number, total: number) => {
      const a = 2 * Math.PI * i / total - Math.PI / 2;
      const b = 2 * Math.PI * (i + 1) / total - Math.PI / 2;
      return `M 50 50 L ${50 + 47 * Math.cos(a)} ${50 + 47 * Math.sin(a)} A 47 47 0 0 1 ${50 + 47 * Math.cos(b)} ${50 + 47 * Math.sin(b)} Z`;
    };
    return { clocks, config, odds, add, remove, resize, advance, roll, rollAll, wedge };
  },
});
</script>
