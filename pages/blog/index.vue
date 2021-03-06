<template>
  <div>
    <div
      v-if="!darkThemeFlag"
      class="top-image--light"
      style="margin-top: -6px;margin-bottom: 40px;"
    >
      <div
        data-aos="fade-in"
        data-aos-dealy="100"
        data-aos-duration="1200"
        data-aos-once="true"
        class="x1"
      >
        <div class="x2">
          <div class="x3">
            Blog
          </div>
        </div>
      </div>
    </div>

    <div
      v-if="darkThemeFlag"
      class="top-image--dark"
      style="margin-top: -6px;margin-bottom: 40px;"
    >
      <div
        data-aos="fade-in"
        data-aos-dealy="100"
        data-aos-duration="1200"
        data-aos-once="true"
        class="x1"
      >
        <div class="x2">
          <div class="x3" style="color: #fff;">
            Blog
          </div>
        </div>
      </div>
    </div>

    <v-row :key="counter" class="content-container">
      <breadcrumbs-component :items="items" />

      <subpage-title-section-component
        :title="'{ ' + title + ' - ' + posts.length + ' }'"
      />

      <subpage-description-section-component :description="description" />

      <template v-if="posts.length">
        <v-col
          v-for="item in posts"
          :key="item.id"
          class="post-link-item"
          cols="12"
          lg="12"
          xs="12"
        >
          <nuxt-link
            :to="`/blog/` + item.id"
            :class="{ 'post-link-dark-theme': darkThemeFlag }"
            class="post-link"
          >
            <div class="post-main-title">
              <div class="post-title">
                {{ item.title }}
              </div>
            </div>
            <div class="description-section">
              {{ item.description }}
            </div>
            <div class="creation-date">
              {{ item.creationDate }}
            </div>
          </nuxt-link>
        </v-col>
      </template>
      <template v-else>
        <v-col lg="12">
          <v-alert dense type="info">
            Brak postów
          </v-alert>
        </v-col>
      </template>
    </v-row>
  </div>
</template>

<script>
import axios from 'axios'
import { mapGetters } from 'vuex'
import Breadcrumbs from '../../components/breadcrumbs'
import SubpageTitleSection from '../../components/subpage-title-section'
import SubpageDescriptionSection from '../../components/subpage-description-section'

export default {
  components: {
    'breadcrumbs-component': Breadcrumbs,
    'subpage-title-section-component': SubpageTitleSection,
    'subpage-description-section-component': SubpageDescriptionSection
  },
  async asyncData({ params }) {
    try {
      const { data } = await axios.get(
        `https://jakubgania.io/data/blog/list-of-posts.json`
      )

      return {
        posts: data.posts
      }
    } catch (error) {
      //
    }
  },
  data() {
    return {
      title: 'posty',
      description:
        'Zapraszam do czytania postów w ramach mojego bloga. Będę tutaj opisywał tematy techniczne jak i różne przemyślenia. Blog jest jeszcze we wczesnej fazie rozwoju także staram się eliminować wszelkie błędy a posty pisać systematycznie. Jest to dla mnie nowe doświadczenie dlatego mogę popełniać błędy, których nie widzę od razu lub nie jestem świadomy. W przypadku wątpliwości lub pytań proszę o kontakt ze mną.',
      items: [
        {
          text: 'menu',
          disabled: false,
          exact: true,
          nuxt: true,
          to: '/menu'
        },
        {
          text: 'blog',
          disabled: true,
          exact: true,
          nuxt: true,
          to: '/zasoby'
        }
      ],
      counter: 0
    }
  },
  computed: {
    ...mapGetters('DarkMode', ['darkTheme']),
    darkThemeFlag() {
      this.forceUpdate()
      return this.darkTheme
    }
  },
  methods: {
    forceUpdate() {
      this.counter += 1
    }
  },
  head() {
    return {
      title: 'Jakub Gania Software | Blog',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Blog'
        },
        {
          name: 'keywords',
          content: 'posty, wpisy, notatki, blog'
        },
        {
          hid: 'og:title',
          property: 'og:title',
          content: 'Jakub Gania Software | Blog'
        },
        {
          hid: 'og:url',
          property: 'og:url',
          content: 'https://jakubgania.io/blog'
        },
        {
          hid: 'og:description',
          property: 'og:description',
          content: 'Zapraszam do czytania postów w ramach mojego bloga.'
        },
        {
          hid: 'og:image',
          property: 'og:image',
          content: 'https://jakubgania.io/jakub-gania-software-logo-img.png'
        }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
.content-container {
  max-width: 800px;
  width: 100%;
  margin: auto;
  margin-bottom: 80px;
}
.top-image {
  width: 100%;
  height: 40vw;
  min-height: 200px;
  margin-top: -6px;
  margin-bottom: 80px;
  position: relative;

  &--dark {
    background-image: url('../../assets/images/dotted-wawes-dark.jpg');
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
  }

  &--light {
    background-image: url('../../assets/images/dotted-wawes-light.jpg');
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
  }
}
.x1 {
  width: 100%;
  height: 40vw;
}
.x2 {
  width: 100%;
  position: absolute;
  top: 50%;
  left: 50%;
  -webkit-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  z-index: 10;
}
.x3 {
  text-align: center;
  color: #000;
  text-transform: uppercase;
  font-size: 10vw;
  font-weight: 900;
  letter-spacing: 8px;
  -webkit-font-smoothing: antialiased;
}
.post-link-item {
  margin-top: 10px;
  margin-bottom: 20px;
  padding: 0;
  font-family: 'Roboto Mono', monospace;
}
.post-link {
  text-decoration: none;
  color: #000;

  &:hover {
    .post-main-title {
      color: #0066ff;
    }
  }
}
.post-link-dark-theme {
  color: #bfbfbf;
}
.post-main-title {
  font-size: 20px;
  font-weight: 500;
  letter-spacing: 0.4px;
  display: flex;
  padding-top: 2px;
  padding-bottom: 4px;
}
.post-title {
  font-size: 20px;
}
.description-section {
  font-size: 14px;
  padding-top: 4px;
  letter-spacing: 0.4px;
  padding-bottom: 10px;
}
.creation-date {
  text-align: left;
  font-size: 12px;
  letter-spacing: 1px;
  color: #8c8c8c;
  padding-top: 4px;
}

@media only screen and (max-width: 960px) {
  .content-container {
    padding-left: 14px;
    padding-right: 14px;
  }
}

@media only screen and (max-width: 600px) {
  .content-container {
    padding-left: 14px;
    padding-right: 14px;
  }
  .post-main-title {
    padding-top: 4px;
    padding-bottom: 4px;
  }
  .post-title {
    font-size: 16px;
    padding-left: 0px;
  }
  .creation-date {
    padding-left: 0px;
  }
}
</style>
