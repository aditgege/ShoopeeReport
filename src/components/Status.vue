<template>
  <ul
    class="
      bg-gray-50
      p-4
      sm:px-8 sm:pt-6 sm:pb-8
      lg:p-4
      xl:px-8 xl:pt-6 xl:pb-8
      grid grid-cols-1
      sm:grid-cols-2
      lg:grid-cols-1
      xl:grid-cols-2
      gap-4
      text-sm
      leading-6
    "
  >
    <li>
      <a
        class="
          hover:bg-blue-500 hover:ring-blue-500 hover:shadow-md
          group
          rounded-md
          p-3
          bg-white
          ring-1 ring-gray-200
          shadow-sm
        "
      >
        <dl
          class="
            grid
            sm:block
            lg:grid
            xl:block
            grid-cols-2 grid-rows-2
            items-center
          "
        >
          <div>
            <dt class="sr-only">{{ result.rugi.status }}</dt>
            <dd class="group-hover:text-white font-semibold text-gray-900">
              {{ result.rugi.total }}
            </dd>
          </div>
        </dl>
      </a>
    </li>

    <li>
      <a
        class="
          hover:bg-blue-500 hover:ring-blue-500 hover:shadow-md
          group
          rounded-md
          p-3
          bg-white
          ring-1 ring-gray-200
          shadow-sm
        "
      >
        <dl
          class="
            grid
            sm:block
            lg:grid
            xl:block
            grid-cols-2 grid-rows-2
            items-center
          "
        >
          <div>
            <dt class="sr-only">{{ result.untung.status }}</dt>
            <dd class="group-hover:text-white font-semibold text-gray-900">
              {{ result.untung.total }}
            </dd>
          </div>
        </dl>
      </a>
    </li>
  </ul>
</template>
<script>
export default {
  name: "StatusList",
  props: {
    result: {
      type: Array,
    },
  },
  data() {
    return {
      formated: [],
      result: {
        rugi: {
          status: "Rugi",
          total: 0,
        },
        untung: {
          status: "Untung",
          total: 0,
        },
      },
    };
  },
  created () {
    this.createdStatus()
  },
  methods: {
    createdStatus() {
      let statuses = [];
      let res = JSON.parse(JSON.stringify(this.result))

      if (res.length) {
        res.forEach((item) => {
          
          let qty = this.calculateQty(item);
          let modal = 13000;
          let hasilModal = qty * modal;
          let hasil = this.calculateTotal(item);
  
          let status = false;
  
          if (hasil < hasilModal) {
            status = "RUGI";
          } else {
            status = "UNTUNG";
          }
  
          statuses.push({
            status: status,
            amount: this.calculateUntungRugiModal(item),
          });
        });
        let untung = statuses.filter((item) => item.status === "UNTUNG");
        let rugi = statuses.filter((item) => item.status === "RUGI");

        let untungAmount = untung.reduce((acc, item) => {
          return acc + item.amount;
        }, 0);

        let rugiAmount = rugi.reduce((acc, item) => {
          return acc + item.amount;
        }, 0);

        let result = {
          untung: {
            status: "UNTUNG",
            total: untungAmount,
          },
          rugi: {
            status: "RUGI",
            total: rugiAmount,
          },
        }


        this.result = result
        debugger
      }
    },
    calculateTotal(item) {
      let subtotal = Number(item.subtotal);
      let netAdjustment = Number(item.net_adjustment);
      let komisi = Number(item.commission_rate) * subtotal;
      let hasil = subtotal - komisi - netAdjustment;

      return hasil;
    },
    calculateQty(item) {
      let subtotal = Number(item.subtotal);
      let qty = subtotal / 20000;

      return qty;
    },

    calculateUntungRugiModal(item) {
      let qty = this.calculateQty(item);
      let modalKotor = 13000;
      let hasilModal = qty * modalKotor;
      let hasil = this.calculateTotal(item);

      if (hasil < hasilModal) {
        // return false
        return hasilModal - hasil;
      } else {
        return hasil - hasilModal;
      }
    },
  },
};
</script>