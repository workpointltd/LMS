<template>
  <v-responsive>
    <div class="bg-white border-none">
      <v-container fluid>
        <v-row class="d-flex pb-4">
          <v-col>
            <h1 class="text-h6 font-weight-regular">All Statements</h1>
            <h2 class="text-caption text-grey-darken-2 font-weight-regular">
              Upload Bank Or Mobile Money Statements Below
            </h2>
          </v-col>
          <v-col
            cols="auto"
            class="d-flex align-center"
          >
            <v-row class="d-flex px-4 justify-end">
              <v-btn
                prepend-icon="mdi:mdi-file-outline"
                variant="flat"
                color="success"
                class="text-none text-caption"
                @click="router.push({ name: 'scoring' })"
              >
                Upload Statement
              </v-btn>
            </v-row>
          </v-col>
        </v-row>
      </v-container>
    </div>
  </v-responsive>
</template>

<style>
.fixed {
  position: fixed;
}
</style>
<script setup lang="ts">
import router from "@/router";
</script>
