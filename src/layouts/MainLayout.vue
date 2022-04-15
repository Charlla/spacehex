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

    <q-drawer v-model="leftDrawerOpen" bordered> # About and Help </q-drawer>

    <q-page-container>
      <q-page>
        <div v-if="loading">Loading...</div>
        <div v-else-if="error">Error: {{ error.message }}</div>
        <div v-else-if="result && result.launchesPast">
          <q-timeline :layout="loose" color="secondary">
            <q-timeline-entry heading> SpaceX Launches </q-timeline-entry>
            <q-timeline-entry
              v-for="launch in result.launchesPast"
              :key="launch.id"
              :title="launch.mission_name"
              :subtitle="launch.launch_date_local"
              side="left"
            >
              <q-card class="my-card">
                <q-list>
                  <q-item clickable>
                    <q-item-section avatar>
                      <q-icon color="primary" name="rocket" />
                    </q-item-section>

                    <q-item-section>
                      <q-item-label>{{
                        launch.rocket.rocket_name
                      }}</q-item-label>
                      <q-item-label caption>Main Rocket</q-item-label>
                    </q-item-section>
                  </q-item>

                  <q-item clickable>
                    <q-item-section avatar>
                      <q-icon color="red" name="flight_takeoff" />
                    </q-item-section>

                    <q-item-section>
                      <q-item-label>{{
                        launch.launch_site.site_name_long
                      }}</q-item-label>
                      <q-item-label caption>Mission Site</q-item-label>
                    </q-item-section>
                  </q-item>

                  <q-item clickable>
                    <q-item-section avatar>
                      <q-icon color="amber" name="inventory" />
                    </q-item-section>

                    <q-item-section>
                      <q-item-label v-if="(launch_success = 'true')"
                        >Success</q-item-label
                      >
                      <q-item-label v-else>Failure</q-item-label>
                      <q-item-label caption>Status</q-item-label>
                    </q-item-section>
                  </q-item>
                </q-list>

                <q-video
                  :ratio="16 / 9"
                  :src="getYoutubeEmbedLink(launch.links.video_link)"
                />
              </q-card>
            </q-timeline-entry>
          </q-timeline>
        </div>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";
import { useQuery } from "@vue/apollo-composable";
import gql from "graphql-tag";

export default defineComponent({
  name: "MainLayout",

  components: {},

  setup() {
    const leftDrawerOpen = ref(false);

    const { result, loading, error } = useQuery(gql`
      query getLaunches {
        launchesPast(limit: 10) {
          mission_name
          launch_date_local
          launch_site {
            site_name
            site_name_long
          }
          links {
            video_link
          }
          rocket {
            rocket_name
          }
          launch_success
          id
        }
      }
    `);

    return {
      leftDrawerOpen,
      result,
      loading,
      error,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
      getYoutubeEmbedLink(link) {
        return "https://www.youtube.com/embed/" + /[^/]*$/.exec(link)[0];
      },
    };
  },
});
</script>
