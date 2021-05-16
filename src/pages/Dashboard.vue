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

    <div v-if="profile">
      <div class="vx-row">
        <div class="vx-col w-full lg:w-1/3 xl:w-1/3 mb-base">
          <div class="m-6">
            <MineNewBlock
              :unconfirmed_txs="profile.unconfirmed_txs"
              @mine="mine($event)"
            />
          </div>
        </div>

        <div class="vx-col w-full lg:w-1/3 xl:w-1/3 mb-base">
          <WalletInfoCard class="m-6" :balance="profile.balance" />
        </div>
        <div class="vx-col w-full lg:w-1/3 xl:w-1/3 mb-base">
          <AddTransaction class="m-6" @pay="pay($event)" />
        </div>
      </div>
      <vs-divider />
      <div class="text-center text-5xl p-8">BLOCKCHAIN</div>

      <div class="flex flex-row items-center justify-center mt-6">
        <div class="w-1/2 flex-col">
          <BlockList :blocks="profile.chain" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CryptoService from "../services/CryptoService";
import { MessageTypeEnum } from "../constants/enums";
import BlockList from "../presentations/dashboard/BlockList";
import WalletInfoCard from "../presentations/dashboard/WalletInfoCard";
import MineNewBlock from "../presentations/dashboard/MineNewBlock";
import AddTransaction from "../presentations/dashboard/AddTransaction";

export default {
  components: { BlockList, WalletInfoCard, MineNewBlock, AddTransaction },
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
      profile: null,
      peers: [
        { name: "Mandy", isSelected: true },
        { name: "Christian", isSelected: false },
        { name: "Susan", isSelected: false }
      ]
    };
  },
  created() {
    const postData = {
      type: "REQUEST_PROFILE"
    };
    this.$socket.emit("data", JSON.stringify(postData));
    // <TODO>
    this.$socket.on("data", message => {
      this.handleIncomingMsg(message);
    });
    // </TODO>
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
    pay(submitData) {},
    mine(submitData) {
      // console.log(submitData);
      const postData = {
        type: MessageTypeEnum.MINE_NEW_BLOCK,
        payload: submitData
      };
      this.$socket.emit("data", JSON.stringify(postData));
      this.$socket.on("data", message => {
        this.handleIncomingMsg(message);
      });
    },
    handleIncomingMsg(message) {
      // console.log(message);
      const msg = JSON.parse(message);
      switch (msg.type) {
        case MessageTypeEnum.REQUEST_PROFILE:
          console.log(MessageTypeEnum.REQUEST_PROFILE);
          console.log(msg);
          this.profile = msg.payload;
          break;
        default:
          break;
      }
    }
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
