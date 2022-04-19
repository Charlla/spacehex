<template>
  <q-layout view="lHh lpR lFf">
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

    <q-drawer
      v-model="leftDrawerOpen"
      side="left"
      overlay
      behavior="mobile"
      bordered
    >
      <About />
    </q-drawer>

    <q-page-container>
      <q-page>
        <p class="intro-text">
          HELLO <br />
          Welcome to SpaceHex
        </p>
        <div v-if="loading">Loading...</div>
        <div v-else-if="error">Error: {{ error.message }}</div>
        <div v-else-if="result && result.launchesPast">
          <q-timeline layout="loose" color="secondary" class="timeline">
            <q-timeline-entry heading> SpaceX Launches </q-timeline-entry>
            <q-timeline-entry
              v-for="(launch, index) in result.launchesPast"
              :key="launch.id"
              :title="launch.mission_name"
              :subtitle="launch.launch_date_local"
              :side="launch.id % 2 ? 'left' : 'right'"
              @click="showImages(index)"
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
                      <q-icon color="primary" name="flight_takeoff" />
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
                      <q-icon
                        v-if="(launch_success = 'true')"
                        color="green"
                        name="check_circle"
                      />
                      <q-icon v-else color="green" name="check_circle" />
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

                <q-img
                  v-if="
                    launch.links.flickr_images != null &&
                    launch.links.flickr_images[0] != null
                  "
                  :src="launch.links.flickr_images[0]"
                />
                <!-- <q-video
                  :ratio="16 / 9"
                  :src="getYoutubeEmbedLink(launch.links.video_link)"
                /> -->
              </q-card>
            </q-timeline-entry>
          </q-timeline>
        </div>
        <q-dialog v-model="carousel">
          <q-card style="width: 700px; max-width: 80vw">
            <q-card-section>
              <div class="text-h6">
                Mission Images for
                {{ result.launchesPast[getClickedLaunchIndex()].mission_name }}
              </div>
            </q-card-section>
            <q-card-section>
              <q-banner
                v-if="
                  result.launchesPast[getClickedLaunchIndex()].links
                    .flickr_images[0] == null
                "
                >No images available for this mission</q-banner
              >
              <q-carousel
                v-if="
                  result.launchesPast[getClickedLaunchIndex()].links
                    .flickr_images[0] != null
                "
                navigation
                infinite
                :autoplay="5000"
                arrows
                v-model="slide"
              >
                <q-carousel-slide
                  :name="index"
                  v-for="(item, index) in result.launchesPast[
                    getClickedLaunchIndex()
                  ].links.flickr_images"
                  :key="index"
                  :img-src="item"
                />
              </q-carousel>
            </q-card-section>
            <q-card-section>
              <div class="text-body1">
                {{ result.launchesPast[getClickedLaunchIndex()].details }}
              </div>
            </q-card-section>
          </q-card>
        </q-dialog>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";
import { useQuery } from "@vue/apollo-composable";
import gql from "graphql-tag";
import About from "components/About.vue";

export default defineComponent({
  name: "MainLayout",

  components: { About },

  setup() {
    const leftDrawerOpen = ref(false);
    const carousel = ref(false);
    const layoutSide = ref("left");
    const missionIndex = ref(1);

    const { result, loading, error } = useQuery(gql`
      query getLaunches {
        launchesPast(limit: 10) {
          mission_name
          launch_date_utc
          launch_site {
            site_name
            site_name_long
          }
          links {
            video_link
            flickr_images
          }
          rocket {
            rocket_name
          }
          launch_success
          id
          details
        }
      }
    `);

    return {
      leftDrawerOpen,
      result,
      loading,
      error,
      carousel,
      slide: ref(1),
      missionIndex,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
      getYoutubeEmbedLink(link) {
        return "https://www.youtube.com/embed/" + /[^/]*$/.exec(link)[0];
      },
      getTimelineSide() {
        layoutSide.value = layoutSide.value == "left" ? "right" : "left";
        return layoutSide.value;
      },
      getImageIndex() {
        imageIndex.value = imageIndex.value + 1;
        return imageIndex.value;
      },
      getClickedLaunchIndex() {
        return missionIndex.value;
      },
      showImages(index) {
        carousel.value = true;
        missionIndex.value = index;
        return missionIndex.value;
      },
    };
  },
});
</script>

<style>
@-webkit-keyframes intro {
  0% {
    opacity: 1;
  }
  90% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@-moz-keyframes intro {
  0% {
    opacity: 1;
  }
  90% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@-ms-keyframes intro {
  0% {
    opacity: 1;
  }
  90% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@-o-keyframes intro {
  0% {
    opacity: 1;
  }
  90% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@keyframes intro {
  0% {
    opacity: 1;
  }
  90% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

.intro-text {
  position: relative;
  max-width: 16em;
  font-size: 200%;
  font-weight: 400;
  margin: 20% auto;
  color: rgb(27, 56, 56);
  opacity: 0;
  z-index: 1;
  text-align: center;
  -webkit-animation: intro 2s ease-out;
  -moz-animation: intro 2s ease-out;
  -ms-animation: intro 2s ease-out;
  -o-animation: intro 2s ease-out;
  animation: intro 8s ease-out;
}

@keyframes scroll {
  0% {
    top: 100%;
  }
  100% {
    top: 0%;
  }
}

.timeline {
  position: absolute;
  /* top: 100%; */
  animation: scroll 10s linear 1s forwards;
}

.q-timeline__subtitle {
  font-size: 200%;
  font-weight: 400;
}
</style>
