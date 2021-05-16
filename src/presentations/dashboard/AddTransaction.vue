<template>
  <div>
    <vx-card
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
          @click="$emit('pay', submitData)"
          >PAY</vs-button
        >
      </div>
    </vx-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      submitData: null,
      peer: null,
      peer_choices: ["Christian", "Susan"],
      amount: 0,
      fee: 0,
      funds: [],
      fund_choices: [
        { id: 0, label: "$100 from Mining" },
        { id: 1, label: "$100 from Mining" }
      ]
    };
  }
};
</script>
