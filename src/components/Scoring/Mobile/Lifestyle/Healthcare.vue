<script setup lang="ts">
import { ref, computed, onMounted } from "vue";
import { useRoute } from "vue-router";

import axiosInstance from "@/services/api/axiosInstance";
import formatter from "@/helpers/currency";

interface HealthcareDataItem {
  total: number;
  highest: string;
  highest_who: string;
  lowest: string;
  lowest_who: string;
  classification: string;
}

interface HealthcareTopTransData {
  last_draw: string;
  last: string;
  highest: string;
  count: string;
  name: string;
  transactiontype: string;
  classification: string;
}

const route = useRoute();

const open = ref(true);
const loading = ref(false);
const itemsPerPage = ref(5);
const totalItems = computed(() => healthcareTopTransData.value.length);
const headers = ref<
  { title: string; key: string; align: string; sortable: boolean }[]
>([
  {
    title: "Count",
    align: "start",
    sortable: false,
    key: "count",
  },
  {
    title: "Transaction Type",
    key: "transactiontype",
    align: "start",
    sortable: false,
  },
  { title: "Healthcare Name", key: "name", align: "start", sortable: false },
  { title: "Highest", key: "highest", align: "end", sortable: false },
  { title: "Last Draw", key: "last_draw", align: "end", sortable: false },
  { title: "Last Amount", key: "last", align: "end", sortable: false },
]);

const healthcareTransSentData = ref<HealthcareDataItem[]>([]);
const healthcareTransBuyGoodsData = ref<HealthcareDataItem[]>([]);
const healthcareTopTransData = ref<HealthcareTopTransData[]>([]);

// API Call: Get Healthcare Transactions Data
const loadHealthcareTransSentData = async () => {
  await axiosInstance
    .get(
      `/e_statement/pay_bill_classifications_sent?statementRefId=${route.params.slug}&classification=Healthcare&pageSize=100&sortBy=id`
    )
    .then(response => (healthcareTransSentData.value = response.data.content))
    .catch(error => console.error(error));
};

const loadHealthcareTransBuyGoodsData = async () => {
  await axiosInstance
    .get(
      `/e_statement/buy_goods_classifications_summary?statementRefId=${route.params.slug}&classification=Healthcare&pageSize=100&sortBy=id`
    )
    .then(
      response => (healthcareTransBuyGoodsData.value = response.data.content)
    )
    .catch(error => console.error(error));
};

// API Call: Get Top Healthcare Trans Data
const loadHealthcareTopTransData = async () => {
  await axiosInstance
    .get(
      `/e_statement/top_paybill_classifications?statementRefId=${route.params.slug}&classification=Healthcare&pageSize=${itemsPerPage.value}&sortBy=id`
    )
    .then(response => (healthcareTopTransData.value = response.data.content))
    .catch(error => console.error(error));
};

onMounted(() => {
  loadHealthcareTransSentData();
  loadHealthcareTransBuyGoodsData();
});
</script>

<template>
  <v-container fluid>
    <div
      @click="open = !open"
      class="px-6 py-2 rounded bg-blue-darken-2 hover-cursor-pointer d-flex justify-space-between"
    >
      <p>Healthcare</p>
      <v-icon
        icon="mdi:mdi-arrow-down-thin-circle-outline"
        :class="open ? 'fa-rotate-180' : ''"
      ></v-icon>
    </div>
    <div v-if="open">
      <v-row>
        <v-col class="my-2">
          <v-card
            class="rounded text-caption"
            variant="flat"
            color="white"
          >
            <v-container fluid>
              <div class="mx-4">
                <h1 class="text-h6 font-weight-regular">
                  Healthcare Transactions (Paybill)
                </h1>
                <h2 class="text-caption text-grey-darken-2 font-weight-regular">
                  Summary of Healthcare Transactions
                </h2>
              </div>
              <div class="mx-4 my-8">
                <v-row class="justify-space-between d-flex font-weight-bold">
                  <v-col>Title</v-col>
                  <v-col>Received</v-col>
                  <v-col>Sent</v-col>
                </v-row>
                <v-divider
                  class="my-3"
                  :thickness="3"
                />
                <v-row class="justify-space-between d-flex">
                  <v-col class="font-weight-medium">Highest</v-col>
                  <v-col> - </v-col>
                  <v-col>{{
                    formatter(healthcareTransSentData[0]?.highest)
                  }}</v-col>
                </v-row>
                <v-divider class="my-2" />
                <v-row class="justify-space-between d-flex">
                  <v-col class="font-weight-medium">Highest To</v-col>
                  <v-col> - </v-col>
                  <v-col>{{ healthcareTransSentData[0]?.highest_who }}</v-col>
                </v-row>
                <v-divider class="my-2" />
                <v-row class="justify-space-between d-flex">
                  <v-col class="font-weight-medium">Lowest</v-col>
                  <v-col> - </v-col>
                  <v-col>{{
                    formatter(healthcareTransSentData[0]?.lowest)
                  }}</v-col>
                </v-row>
                <v-divider class="my-2" />
                <v-row class="justify-space-between d-flex">
                  <v-col class="font-weight-medium">Lowest To</v-col>
                  <v-col> - </v-col>
                  <v-col>{{ healthcareTransSentData[0]?.lowest_who }}</v-col>
                </v-row>
                <v-divider
                  class="my-3"
                  :thickness="3"
                />
                <v-row class="font-weight-bold justify-space-between d-flex">
                  <v-col>Total</v-col>
                  <v-col> - </v-col>
                  <v-col>{{
                    formatter(healthcareTransSentData[0]?.total)
                  }}</v-col>
                </v-row>
              </div>
            </v-container>
          </v-card>
        </v-col>
        <v-col>
          <v-card
            class="rounded text-caption"
            variant="flat"
            color="white"
          >
            <v-container fluid>
              <div class="mx-4">
                <h1 class="text-h6 font-weight-regular">
                  Healthcare Transactions (Buy Goods)
                </h1>
                <h2 class="text-caption text-grey-darken-2 font-weight-regular">
                  Summary of Healthcare Transactions
                </h2>
              </div>
              <div class="mx-4 my-8">
                <v-row class="justify-space-between d-flex font-weight-bold">
                  <v-col>Title</v-col>
                  <v-col>Received</v-col>
                  <v-col>Sent</v-col>
                </v-row>
                <v-divider
                  class="my-3"
                  :thickness="3"
                />
                <v-row class="justify-space-between d-flex">
                  <v-col class="font-weight-medium">Highest</v-col>
                  <v-col> - </v-col>
                  <v-col>{{
                    formatter(healthcareTransBuyGoodsData[0]?.highest)
                  }}</v-col>
                </v-row>
                <v-divider class="my-2" />
                <v-row class="justify-space-between d-flex">
                  <v-col class="font-weight-medium">Highest To</v-col>
                  <v-col> - </v-col>
                  <v-col>{{
                    healthcareTransBuyGoodsData[0]?.highest_who
                  }}</v-col>
                </v-row>
                <v-divider class="my-2" />
                <v-row class="justify-space-between d-flex">
                  <v-col class="font-weight-medium">Lowest</v-col>
                  <v-col> - </v-col>
                  <v-col>{{
                    formatter(healthcareTransBuyGoodsData[0]?.lowest)
                  }}</v-col>
                </v-row>
                <v-divider class="my-2" />
                <v-row class="justify-space-between d-flex">
                  <v-col class="font-weight-medium">Lowest To</v-col>
                  <v-col> - </v-col>
                  <v-col>{{
                    healthcareTransBuyGoodsData[0]?.lowest_who
                  }}</v-col>
                </v-row>
                <v-divider
                  class="my-3"
                  :thickness="3"
                />
                <v-row class="font-weight-bold justify-space-between d-flex">
                  <v-col>Total</v-col>
                  <v-col> - </v-col>
                  <v-col>{{
                    formatter(healthcareTransBuyGoodsData[0]?.total)
                  }}</v-col>
                </v-row>
              </div>
            </v-container>
          </v-card>
        </v-col>
      </v-row>
      <!--      Table-->
      <!--      <v-row>
        <v-container fluid>
          <v-card
            variant="flat"
            class="py-4 rounded"
            color="white"
          >
            <div class="px-8">
              <h1 class="text-h6 font-weight-regular">
                Top Healthcare Transactions
              </h1>
              <h2 class="text-caption text-grey-darken-2 font-weight-regular">
                Summary of Healthcare Transactions
              </h2>
            </div>
            <v-data-table-server
              class="px-4 text-caption"
              v-model:items-per-page="itemsPerPage"
              :headers="headers"
              :items-length="totalItems"
              :items="healthcareTopTransData"
              :loading="loading"
              loading-text="Loading...Please Wait"
              item-value="name"
              @update:options="loadHealthcareTopTransData()"
            >
              <template v-slot:[`item.highest`]="{ item }"
                ><span>{{ formatter(item.columns.highest) }}</span></template
              >
              <template v-slot:[`item.last`]="{ item }"
                ><span>{{ formatter(item.columns.last) }}</span></template
              >
            </v-data-table-server>
          </v-card>
        </v-container>
      </v-row>-->
    </div>
  </v-container>
</template>

<style>
.hover-cursor-pointer :hover {
  cursor: pointer;
}
</style>
