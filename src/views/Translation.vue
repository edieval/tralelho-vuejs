<script lang="ts" setup>
import pageConfig from "./Translation.config.json";
import { useRouter } from "vue-router";
import countries from "../components/countries.json";
import iso3Languages from "../iso3-languages.json";
import { ref } from "vue";
import ReverseModal from "../components/reverse-modal.vue";

const countryISO3 = useRouter().currentRoute.value.query.country;
const countryConfig = countries.find((countryConfig) => {
  return countryConfig.iso3 === countryISO3;
});

const targetLanguages = useRouter().currentRoute.value.query.lang || ["fra"];
let selectedLanguage = ref(
  Array.isArray(targetLanguages) ? targetLanguages[0] : targetLanguages
);
let isOpenModal = ref(false);
let message = ref("");
const openReverseBox = (msg: string) => {
  message.value = msg;
  isOpenModal.value = true;
}
</script>

<template>
  <div class="section">
    <h1 class="title is-1">
      <span class="is-size-1 mr-2 flag-icon flag-icon-fr"></span>
      <font-awesome-icon icon="long-arrow-alt-right" />
      <span
        :class="
          'is-size-1 ml-2 flag-icon flag-icon-' +
          countryConfig.iso2.toLowerCase()
        "
      ></span>
    </h1>

    <div
      class="change-language mx-auto mb-6 field is-horizontal"
      v-if="countryConfig && countryConfig.languages.length > 1"
    >
      <div class="field-label is-normal field-label-size">
        <label class="label">Change language:</label>
      </div>
      <div class="select">
        <select v-model="selectedLanguage">
          <option
            v-for="lang of countryConfig.languages"
            :key="lang"
            :value="lang"
          >
            {{ iso3Languages[lang] }}
          </option>
        </select>
      </div>
    </div>

    <div className="columns">
      <div className="column is-2 has-text-left">
        <aside class="menu">
          <ul class="menu-list">
            <li style="text-align: center">
              <button onclick="txtUpDown" type="button" id="transbutton">
                <span><img class="navicon" src="/tablette.png" /></span>
              </button>
            </li>

            <li v-for="section of pageConfig" :key="section.title">
              <a :href="'#' + section.title" class="is-align-items-center">
                <img
                  :src="'/' + section.icon"
                  :alt="$t(section.title)"
                  width="35"
                  height="35"
                />
                <span class="menu-element-multiline">
                  {{ $t(section.title) }}
                </span>
              </a>
            </li>
          </ul>
        </aside>
      </div>

      <div className="column">
        <div
          class="container mb-6"
          v-for="section of pageConfig"
          :key="section.title"
        >
          <h2
            :id="section.title"
            class="title is-2 has-text-left"
            v-t="{ path: section.title }"
          ></h2>

          <div
            v-for="phrase of section.list"
            :key="phrase"
            class="box columns m-2 py-2 is-clickable change-background-on-hover"
            v-on:click="openReverseBox('test')"
          >
            <div class="column is-half py-1">
              <p v-t="{ path: phrase }" class="bd-notification is-primary"></p>
            </div>
            <div class="column is-half py-1">
              <p
                v-t="{ path: phrase, locale: selectedLanguage }"
                class="bd-notification is-primary"
              ></p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <ReverseModal v-if="isOpenModal" :message="{message}"></ReverseModal>
</template>

<style scoped lang="scss">
.is-align-items-center {
  display: flex;
  flex-wrap: wrap;
  align-content: center; /* used this for multiple child */
  align-items: center; /* if an only child */
  position: relative;
  top: 10px;
  background-color: #a2c0de;
  &:hover {
    background-color: whitesmoke;
  }
}
.main-color {
  background-color: #a2c0de;
}
.menu-element-multiline {
  position: relative;
  width: 100px;
  margin-left: 5px;
}

.change-language {
  max-width: 500px;
}

.txtUpDown {
  filter: progid:DXImageTransform.Microsoft.BasicImage(rotation=2);
  /* IE6,IE7 */
  -moz-transform: rotate(-180deg);
  /* FF3.5+ */
  -o-transform: rotate(-180deg);
  /* Opera 10.5 */
  -webkit-transform: rotate(-180deg);
  /* Safari 3.1+, Chrome */
  background-color: rgba(255, 192, 203, 0.8);
  width: 100%;
  height: 30vh;
  font-size: 30px;
  font-weight: bold;
  display: none;
  position: sticky;
  top: 8%;
  text-align: center;
  z-index: 0;
}
</style>
