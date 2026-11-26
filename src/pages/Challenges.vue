<template>
  <q-page padding>
    <!-- content -->
    <!-- Vows -->
    <div class="text-h4 text-center q-mt-md q-mb-sm">Vows<q-btn icon="add_circle" flat dense @click="addVow" /></div>
    <progress-track
      class="q-mb-md"
      v-for="(vow, vIndex) in campaign.data.character.vows"
      :key="vIndex"
      v-model="campaign.data.character.vows[vIndex]"
    >
      <template v-slot:action v-if="config.data.edit">
        <q-btn class="col-shrink" icon="delete" flat dense @click="removeVow(vIndex)" />
      </template>
    </progress-track>

    <q-separator />

    <!-- Progress -->
    <div class="text-h4 text-center q-mt-md q-mb-sm">
      Progress<q-btn icon="add_circle" flat dense @click="addTrack" />
    </div>
    <progress-track
      class="q-mb-md"
      v-for="(track, pIndex) in campaign.data.progressTracks"
      :key="pIndex"
      v-model="campaign.data.progressTracks[pIndex]"
    >
      <template v-slot:action v-if="config.data.edit">
        <q-btn class="col-shrink" icon="delete" flat dense @click="removeTrack(pIndex)" />
      </template>
    </progress-track>
    <q-separator />
    <div class="text-h4 text-center q-mt-md q-mb-sm">
      Clocks<q-btn icon="add_circle" flat dense @click="clockPanel?.add()" />
    </div>
    <campaign-clocks ref="clockPanel" />
  </q-page>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

import { useCampaign } from 'src/store/campaign';

import { NewProgressTrack } from 'src/lib/tracks';

import CampaignClocks from 'src/components/Tracks/CampaignClocks.vue';
import ProgressTrack from 'src/components/Tracks/ProgressTrack.vue';
import { useConfig } from 'src/store/config';

export default defineComponent({
  name: 'Challenges',
  components: { ProgressTrack, CampaignClocks },
  setup() {
    const campaign = useCampaign();
    const config = useConfig();
    const clockPanel = ref<{ add: () => void } | null>(null);

    const addVow = () => campaign.data.character.vows.push(NewProgressTrack());
    const removeVow = (index: number) => campaign.data.character.vows.splice(index, 1);

    const addTrack = () => campaign.data.progressTracks.push(NewProgressTrack());
    const removeTrack = (index: number) => campaign.data.progressTracks.splice(index, 1);

    return {
      clockPanel,
      campaign,
      config,

      addVow,
      removeVow,

      addTrack,
      removeTrack,
    };
  },
});
</script>
