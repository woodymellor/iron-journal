<template>
  <q-page padding>
    <div class="text-h4 text-center q-mb-md">Encounter</div>
    <q-card flat bordered class="q-pa-md q-mb-md">
      <div class="text-h5">{{ campaign.data.character.name }}</div>
      <div class="row items-center justify-between q-my-sm">
        <q-toggle :model-value="encounter.initiative" @update:model-value="setInitiative" label="I have initiative" />
        <q-btn flat dense label="Burn momentum" :disable="!canBurn" @click="burn" />
      </div>
      <div v-for="key in resources" :key="key" class="row items-center q-mb-xs">
        <div class="col text-capitalize">{{ key }}</div>
        <resource-track class="col-9" :tone="key" v-model="campaign.data.character.tracks[key]" />
      </div>
      <q-separator class="q-my-sm" />
      <div class="row items-center justify-between">
        <span>Action roll</span>
        <div class="row items-center">
          <span>Adds</span><q-btn flat dense icon="remove" @click="adds--" />{{ adds }}<q-btn flat dense icon="add" @click="adds++" />
        </div>
      </div>
      <div class="row q-gutter-xs justify-center">
        <q-btn v-for="stat in stats" :key="stat" outline class="col text-capitalize" no-caps @click="roll(stat)">
          <div>{{ stat }}<div class="text-h6">{{ campaign.data.character.stats[stat] }}</div></div>
        </q-btn>
      </div>
      <div v-if="result.result" class="text-center q-mt-md">
        <div class="text-h5" :class="result.action.color">{{ result.result }}{{ result.challenge.match ? ' — Match' : '' }}</div>
        <div class="text-h5">{{ result.action.score }} vs {{ result.challenge.die1.roll }} | {{ result.challenge.die2.roll }}</div>
        <div class="text-caption">Action die: {{ result.action.die }} · {{ lastStat }}</div>
      </div>
    </q-card>
    <div class="text-h4 text-center q-mb-sm">Foes<q-btn flat dense icon="add_circle" @click="addFoe" /></div>
    <div v-if="!encounter.foes.length" class="text-center text-grey q-pa-md">Add a foe or group to begin.</div>
    <q-card v-for="foe in encounter.foes" :key="foe.id" flat bordered class="q-mb-md q-pa-sm">
      <progress-track v-model="foe.track">
        <template v-slot:action><q-btn flat dense icon="delete" @click="removeFoe(foe.id)" /></template>
      </progress-track>
      <div class="text-caption q-pa-xs">Rank harm: {{ difficulty[foe.track.difficulty].harm }} · Mark progress once per harm inflicted</div>
      <q-input v-model="foe.notes" dense autogrow label="Features, tactics &amp; notes" />
    </q-card>
  </q-page>
</template>

<script lang="ts">
import { computed, defineComponent, ref, watch } from 'vue';
import { useCampaign } from 'src/store/campaign';
import { IStats, ITracks } from 'src/components/models';
import { NewProgressTrack, Difficulty } from 'src/lib/tracks';
import { moveRoll, NewRollData } from 'src/lib/roll';
import { v4 as uuid } from 'uuid';
import ResourceTrack from 'src/components/Tracks/ResourceTrack.vue';
import ProgressTrack from 'src/components/Tracks/ProgressTrack.vue';

export default defineComponent({
  name: 'Encounter',
  components: { ProgressTrack, ResourceTrack },
  setup() {
    const campaign = useCampaign();
    const encounter = computed(() => campaign.data.encounter || { initiative: false, foes: [] });
    const ensure = () => {
      if (!campaign.data.encounter) campaign.data.encounter = { initiative: false, foes: [] };
      return campaign.data.encounter;
    };
    const setInitiative = (value: boolean) => { ensure().initiative = value; };
    const resources: (keyof ITracks)[] = ['health', 'spirit', 'supply', 'momentum'];
    const stats: (keyof IStats)[] = ['edge', 'heart', 'iron', 'shadow', 'wits'];
    const adds = ref(0);
    const result = ref(NewRollData());
    const lastStat = ref('');
    const burned = ref(false);
    const adjust = (key: keyof ITracks, amount: number) => {
      const t = campaign.data.character.tracks[key];
      t.value = Math.max(Number(t.min), Math.min(Number(t.max), Number(t.value) + amount));
    };
    const log = (label: string) => {
      const r = result.value;
      if (campaign.data.journal.length) campaign.appendToJournal(0, `<div class="note"><b>[${label}: ${r.result} = ${r.action.score} vs ${r.challenge.die1.roll} | ${r.challenge.die2.roll}]</b></div>`);
    };
    const roll = (stat: keyof IStats) => {
      const c = campaign.data.character;
      result.value = moveRoll(c.stats[stat], adds.value, c.tracks.momentum.value, false);
      lastStat.value = stat;
      burned.value = false;
      log(stat);
    };
    const canBurn = computed(() => {
      const m = Number(campaign.data.character.tracks.momentum.value);
      const r = result.value;
      return !!r.result && !burned.value && m > 0 && [r.challenge.die1.roll, r.challenge.die2.roll].some(n => m > n && r.action.score <= n);
    });
    const burn = () => {
      if (!canBurn.value) return;
      const m = campaign.data.character.tracks.momentum;
      const r = result.value;
      const hits = [r.challenge.die1, r.challenge.die2].filter(d => r.action.score > d.roll || Number(m.value) > d.roll).length;
      r.result = ['Miss', 'Weak Hit', 'Strong Hit'][hits];
      r.action.color = ['text-negative', 'text-warning', 'text-positive'][hits];
      m.value = Number(m.reset);
      burned.value = true;
      log('Burn momentum');
    };
    const addFoe = () => {
      const track = NewProgressTrack();
      track.name = 'New foe';
      track.difficulty = 2;
      ensure().foes.push({ id: uuid(), track, notes: '' });
    };
    const removeFoe = (id: string) => {
      const e = ensure();
      e.foes = e.foes.filter(f => f.id !== id);
    };
    watch(() => campaign.data.id, () => {
      result.value = NewRollData();
      adds.value = 0;
      lastStat.value = '';
      burned.value = false;
    });
    return { campaign, encounter, setInitiative, resources, stats, adds, result, lastStat, adjust, roll, canBurn, burn, addFoe, removeFoe, difficulty: Difficulty };
  },
});
</script>
