<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="quiz"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title> SpaceHex </q-toolbar-title>

        <div>SpaceHex 2022</div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      # About and Help
    </q-drawer>

    <q-page-container>
      <div class="q-px-lg q-pb-md">
        <q-timeline :layout="loose" color="secondary">
          <q-timeline-entry heading> SpaceX Launches </q-timeline-entry>
          <q-timeline-entry
            title="Sentinel-6 Michael Freilich"
            subtitle="2020-11-21T17:17:00.000Z"
            side="left"
          >
            <q-card class="my-card">
              <q-list>
                <q-item clickable>
                  <q-item-section avatar>
                    <q-icon color="primary" name="rocket" />
                  </q-item-section>

                  <q-item-section>
                    <q-item-label>Falcon 9</q-item-label>
                    <q-item-label caption>Main Rocket</q-item-label>
                  </q-item-section>
                </q-item>

                <q-item clickable>
                  <q-item-section avatar>
                    <q-icon color="red" name="flight_takeoff" />
                  </q-item-section>

                  <q-item-section>
                    <q-item-label
                      >Vandenberg Air Force Base Space Launch Complex
                      4E</q-item-label
                    >
                    <q-item-label caption>Mission Site</q-item-label>
                  </q-item-section>
                </q-item>

                <q-item clickable>
                  <q-item-section avatar>
                    <q-icon color="amber" name="inventory" />
                  </q-item-section>

                  <q-item-section>
                    <q-item-label>Satellite</q-item-label>
                    <q-item-label caption>Payload</q-item-label>
                  </q-item-section>
                </q-item>
              </q-list>

              <q-video
                :ratio="16 / 9"
                src="https://www.youtube.com/embed/aVFPzTDCihQ"
              />

              <div>
                Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do
                eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut
                enim ad minim veniam, quis nostrud exercitation ullamco laboris
                nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor
                in reprehenderit in voluptate velit esse cillum dolore eu fugiat
                nulla pariatur. Excepteur sint occaecat cupidatat non proident,
                sunt in culpa qui officia deserunt mollit anim id est laborum.
              </div>
            </q-card>
          </q-timeline-entry>
        </q-timeline>
      </div>
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "MainLayout",

  components: {},

  setup() {
    const leftDrawerOpen = ref(true);

    return {
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
    };
  },
});
</script>
