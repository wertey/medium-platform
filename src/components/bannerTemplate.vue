<template>
  <div class="app-banner" v-if="Object.keys(bannerInfo).length">
      <h1 class="app-banner__title">
        {{ bannerTitle }}
      </h1>
    <div class="app-banner-row">
      <div class="profile">
        <div class="profile__img">
          <img :src="author.image" alt="">
        </div>
        <div class="profile__text">
          <div class="profile__text-name">
            {{ author.username }}
          </div>
          <div class="profile__text-date">
            {{ createdAt }}
          </div>
        </div>
      </div>
      <div class="app-banner-row__buttons">
        <button class="app-button" @click="editArticle()">Edit Article</button>
        <button class="app-button" @click="deleteArticle(slug)">Delete Article</button>
      </div>
    </div>
  </div>
</template>

<script>
import { get } from 'lodash';
import articlesApi from '@/api/articles/articlesApi';

export default {
  name: 'BannerTemplate',
  props: {
    bannerInfo: {
      type: Object,
      default: () => {},
    },
  },
  computed: {
    bannerTitle() {
      return get(this.bannerInfo, 'article.title', '');
    },
    author() {
      return get(this.bannerInfo, 'article.author', {});
    },
    createdAt() {
      return get(this.bannerInfo, 'article.updatedAt', {});
    },
    slug() {
      return get(this.bannerInfo, 'article.slug', {});
    },
  },
  methods: {
    deleteArticle(slug) {
      articlesApi.deleteArticle(slug);
      this.$router.push('/');
    },
    editArticle() {
      this.$router.push('/articleEdit');
    },
  },
};
</script>

<style scoped lang="scss">
@import "./src/assets/scss/variables";

.app-banner {
  background: $colorBtn;
  min-height: 300px;
  padding: 20px;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;

  &__title {
    font-size: $mainTitle;
    margin-bottom: 15px;
  }

  &__subtitle {
    font-size: $mainSubtitle;
  }

  &-row {
    display: flex;
    align-items: flex-end;

    &__buttons {
      margin-left: 20px;
    }
  }

  .profile {
    display: flex;
    justify-content: flex-start;
    align-items: center;

    &__img {

      img {
        max-width: 33px;
        height: 33px;
        object-fit: cover;
        border-radius: 50%;
      }
    }

    &__text {
      text-align: left;
      padding-left: 20px;
    }
  }
}
</style>
