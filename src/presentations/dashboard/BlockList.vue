<template>
  <div id="block-list">
    <div v-for="(el, index) in 3" :key="index" class="mb-4 px-6">
      <vx-card>
        <vs-table v-if="index != 0" :data="transactions">
          <template slot="thead">
            <vs-th class="text-sm font-medium">
              <feather-icon
                icon="ArrowUpCircleIcon"
                svgClasses="h-4 w-4 pr-1"
              />
              From
            </vs-th>
            <vs-th class="text-sm font-medium">
              <feather-icon
                icon="ArrowDownCircleIcon"
                svgClasses="h-4 w-4 pr-1"
              />
              To
            </vs-th>
            <vs-th class="text-sm font-medium">
              <feather-icon icon="AnchorIcon" svgClasses="h-4 w-4 pr-1" />
              Amount
            </vs-th>
            <vs-th class="text-sm font-medium">
              <feather-icon icon="InfoIcon" svgClasses="h-4 w-4 pr-1" />
              Status
            </vs-th>
            <vs-th class="text-sm font-medium">
              <feather-icon icon="AlertCircleIcon" svgClasses="h-4 w-4 pr-1" />
              Hack
            </vs-th>
          </template>

          <template slot-scope="{ data }">
            <vs-tr
              :key="indextr"
              v-for="(tr, indextr) in data"
              class="cursor-pointer hover:bg-warning hover:text-white"
            >
              <vs-td class="text-sm" :data="data[indextr].email">
                {{ data[indextr].from }}
              </vs-td>

              <vs-td class="text-sm" :data="data[indextr].username">
                {{ data[indextr].to }}
              </vs-td>

              <vs-td class="text-sm" :data="data[indextr].id">
                {{ data[indextr].amount }}
              </vs-td>

              <vs-td class="text-sm" :data="data[indextr].id">
                {{ data[indextr].status }}
              </vs-td>
              <vs-td class="text-sm text-success cursor-pointer">
                Mutate
              </vs-td>
            </vs-tr>
          </template>
        </vs-table>
        <div v-if="index != 0" class="w-full mt-5 text-sm">
          PREVIOUS HASH
          <span class="text-xs text-success ml-4"
            >000231f48b131b1c721b508434cdbf308b0ae7c1a7f6a5ed7db56a34c76c9530</span
          >
        </div>
        <div class="w-full mt-3 text-sm">
          HASH
          <span
            class="text-xs text-success border-2 border-solid border-success rounded bg-success-light p-1 ml-4"
            >000231f48b131b1c721b508434cdbf308b0ae7c1a7f6a5ed7db56a34c76c9530</span
          >
        </div>
        <div class="flex justify-between items-center mt-5">
          <div class="flex-col text-2xl">
            {{ index != 0 ? `BLOCK #${index}` : "GENESIS BLOCK" }}
            <span class="text-xs text-grey"
              >on Thu, 13 May 2021 16:52:56 GMT
            </span>
          </div>
          <div class="text-xs">
            NONCE:
            <span class="border-solid border-2 border-grey rounded text-sm p-1">
              3285
            </span>
          </div>
        </div>
      </vx-card>

      <div class="text-center mt-4">
        <feather-icon svgClasses="text-grey" icon="ChevronDownIcon" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      transactions: [
        {
          id: 1,
          from: "🏆 REWARD",
          to: "Susan",
          amount: "10",
          status: "unspent"
        }
      ]
    };
  },
  created() {
    const postData = {
      type: "REQUEST_BLOCKCHAIN"
    };
    this.$socket.emit("data", postData);
    // <TODO>
    this.$socket.on("data", message => {
      console.log("Blockchain:");
      console.log([JSON.parse(message)]);
    });
    // </TODO>
  }
};
</script>
