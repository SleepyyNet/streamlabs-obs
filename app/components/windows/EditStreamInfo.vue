<template>
<modal-layout
  title="Update Stream Info"
  :show-controls="false"
  :customControls="true">
  <div slot="content">
    <div v-if="infoLoading">
      <i class="fa fa-spinner fa-pulse" />
    </div>
    <div v-if="infoError && !infoLoading" class="warning">
      There was an error fetching your channel information.  You can try
      <a @click="refreshStreamInfo">fetching the information again</a>,
      or you can
      <a @click="goLive">just go live.</a>
      If this error persists, you can try logging out and back in.
    </div>
    <div v-if="!infoLoading && !infoError">
      <TextInput v-model="streamTitleModel" />
      <ListInput
        v-if="isTwitch"
        :value="gameModel"
        :allowEmpty="true"
        placeholder="Search"
        :internal-search="false"
        :loading="searchingGames"
        @search-change="debouncedGameSearch"
        @input="onGameInput"/>
      <BoolInput v-model="doNotShowAgainModel" v-if="!midStreamMode"/>
      <div class="warning" v-if="updateError">
        <div v-if="midStreamMode">
          Something went wrong while updating your stream info.  Please try again.
        </div>
        <div v-else>
          Something went wrong while updating your stream info. You can try again, or you can
          <a @click="goLive">just go live</a>.
        </div>
      </div>
    </div>
  </div>
  <div slot="controls">
    <button
      class="button button--default"
      :disabled="updatingInfo"
      @click="cancel">
      Cancel
    </button>
    <button
      class="button button--action"
      :disabled="updatingInfo"
      @click="updateAndGoLive">
      <i class="fa fa-spinner fa-pulse" v-if="updatingInfo" />
      {{ submitText }}
    </button>
  </div>
</modal-layout>
</template>

<script lang="ts" src="./EditStreamInfo.vue.ts"></script>