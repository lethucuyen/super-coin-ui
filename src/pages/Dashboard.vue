<template>
  <div>
    <h1>PEERS</h1>
    <div class="flex flex-row justify-between items-center mt-5">
      <div class="flex">
        <div
          class="flex-col text-center p-4"
          v-for="(el, index) in peers"
          :key="index"
        >
          <vs-avatar
            size="60px"
            :color="avtColor(el.isSelected)"
            :class="['m-0 ', { 'selected-peer': el.isSelected }]"
            @click="selectPeer()"
          />
          <p
            :class="[
              'font-medium mt-1',
              el.isSelected ? 'text-primary' : 'text-grey'
            ]"
          >
            {{ el.name }}
          </p>
        </div>
      </div>

      <vs-button @click="addPeer()">Add Peer</vs-button>
    </div>

    <vs-divider />

    <div class="flex">
      <vx-card
        class="card-overlay bg-cover m-6 p-4"
        title-color="#fff"
        content-color="#fff"
        :card-background="
          'linear-gradient(120deg ,rgba(247,97,161,0.5), rgba(140,27,171,.8)), url(' +
            card_bg_img_2 +
            ')'
        "
      >
        UNCONFIRMED TRANSACTIONS:
        <v-select
          placeholder="Please select a transaction to mine"
          class="bg-white text-grey"
          v-model="unconfirmed_transactions"
          :options="unconfirmed_transactions_choices"
        />
        <br />
        <br />
        <vs-button class="bg-danger" icon-pack="feather" icon="icon-codepen"
          >MINE NEW BLOCK</vs-button
        >
      </vx-card>
      <vx-card
        class="card-overlay bg-cover m-6 text-center"
        title-color="#fff"
        content-color="#fff"
        :card-background="
          'linear-gradient(120deg ,rgba(109,213,237,.8), rgba(33,147,176,0.5)), url(' +
            card_bg_img_1 +
            ')'
        "
      >
        <div class="border-solid border-white rounded p-5">
          <div class="text-5xl">300</div>
          <div class="text-2xl">BALANCE</div>
        </div>
        <feather-icon icon="FeatherIcon" class="mt-5" />
      </vx-card>
      <vx-card
        class="m-6"
        title-color="#fff"
        content-color="#fff"
        card-background="linear-gradient(120deg, #7f7fd5, #86a8e7, #91eae4)"
      >
        <div style="height: 330px">
          <div class="w-full">
            <label>RECIPIENT:</label>
            <v-select
              placeholder="Peer"
              class="bg-white text-grey"
              v-model="peer"
              :options="peer_choices"
            />
          </div>

          <div class="w-full mt-5">
            <label>AMOUNT / FEE:</label>
            <div class="w-full flex">
              <vs-input-number
                class="w-1/2"
                color="dark"
                :step="20"
                v-model="amount"
              />
              <vs-input-number
                class="w-1/2"
                color="dark"
                :step="20"
                v-model="fee"
              />
            </div>
            <!-- <small class="opacity-75">Insufficient balance!</small> -->
          </div>

          <div class="w-full mt-5">
            <label>FUNDS:</label>
            <v-select
              placeholder="Inputs"
              class="bg-white text-grey"
              multiple
              :closeOnSelect="false"
              v-model="funds"
              :options="fund_choices"
            />
            <!-- <small class="opacity-75">Insufficient funds!</small> -->
          </div>

          <vs-divider />

          <vs-button
            class="bg-dark-gradient mt-5"
            icon-pack="feather"
            icon="icon-feather"
            @click="pay()"
            >PAY</vs-button
          >
        </div>
      </vx-card>
    </div>
    <vs-divider />
    <div class="text-center text-5xl p-8">BLOCKCHAIN</div>

    <div class="flex flex-row items-center justify-center mt-6">
      <div class="w-1/2 flex-col">
        <BlockList />
      </div>
    </div>
  </div>
</template>

<script>
import CryptoService from "../services/CryptoService";
import BlockList from "../presentations/dashboard/BlockList";

export default {
  components: { BlockList },
  computed: {
    avtColor() {
      return isSelected => {
        if (isSelected) return "primary";
        return "warning";
      };
    }
  },
  data() {
    return {
      card_bg_img_1: require("@/assets/images/pages/card-bg-image-demo-1.jpg"),
      card_bg_img_2: require("@/assets/images/pages/card-bg-image-demo-2.jpg"),
      unconfirmed_transactions: null,
      unconfirmed_transactions_choices: ["150/0 Mandy to Susan"],
      peer: null,
      peer_choices: ["Christian", "Susan"],
      amount: 0,
      fee: 0,
      funds: [],
      fund_choices: [
        { id: 0, label: "$100 from Mining" },
        { id: 1, label: "$100 from Mining" }
      ],
      peers: [
        { name: "Mandy", isSelected: true },
        { name: "Christian", isSelected: false },
        { name: "Susan", isSelected: false }
      ]
    };
  },
  methods: {
    async addPeer() {
      try {
        await CryptoService.testService();
      } catch (e) {
        console.log(e);
      }
    },
    selectPeer() {},
    pay() {}
  }
};
</script>

<style lang="scss">
.bg-success-light {
  background-color: #f6ffed;
}
.bg-danger-light {
  background-color: #faeaea;
}
.bg-blue-light {
  background-color: #d2eafb;
}
.selected-peer {
  bottom: 5%;
  z-index: 51000;
  box-shadow: 0 1px 20px 1px rgb(146, 88, 255);
}
</style>
