<template>
  <div class="flex flex-col">
    <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
      <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
        <div class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg">
          <table class="min-w-full divide-y divide-gray-200">
            <thead class="bg-gray-50">
              <tr>
                <th v-for="(header, index) in headers" :key="index" scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  {{ header.text }}
                </th>
              </tr>
            </thead>
            <tbody class="bg-white divide-y divide-gray-200">
              <tr v-for="item in items" :key="item.order_id">
                <td class="px-6 py-4 whitespace-nowrap">
                   {{item.order_id}}
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  {{ calculateQty(item) }}
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  {{ item.del_subsidized_by_merchant }}
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  {{ item.del_subsidized_by_shopeefood }}
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  {{ item.subtotal }}
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  {{ Number(item.commission_rate) * Number(item.subtotal) }}
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  {{ calculateTotal(item) }}
                </td>
                <t class="px-5 py-5 border-b border-gray-200 text-sm">
                  <span v-if="getStatusProfit(item)" class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-100 text-green-800">
                    Untung
                  </span>
                  <span v-else class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-red-100 text-red-800">
                    Rugi
                  </span>
                </t>
                <td class="px-6 py-4 whitespace-nowrap">
                    {{ calculateUntungRugiModal(item)  }}
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DataList",
  props: {
    items: {
      type: Array,
      default: () => [],
    },
  },
  data () {
    return {
      headers: [
        {
          text: "Order Id",
          align: "left",
          sortable: false,
          value: "order_id",
        },
        {
          text: "Qty",
          align: "left",
          sortable: false,
          value: "qty",
        },
        {
          text: "Pengiriman Di tanggung Merchant",
          align: "left",
          sortable: false,
          value: "pengiriman_di_tanggung_merchant",
        },
        {
          text: "Pengiriman Di tanggung Shopee",
          align: "left",
          sortable: false,
          value: "pengiriman_di_tanggung_shopee",
        },
        {
          text: "Subtotal",
          align: "left",
          sortable: false,
          value: "subtotal",
        },
        {
          text: "Komisi yang di ambil Shopee",
          align: "left",
          sortable: false,
          value: "komisi_yang_di_ambil_shopee",
        },
        {
          text: "Penghasilan",
          align: "left",
          sortable: false,
          value: "penghasilan",
        },
        {
          text: "Keterangan",
          align: "left",
          sortable: false,
          value: "keterangan",
        },
        {
          text: "Total Untung / Rugi",
          align: "left",
          sortable: false,
          value: "total_untung_rugi",
        },
      ]
    }
  },
  methods: {
    
    formatPrice(bilangan) {
      let number_string = bilangan.toString(),
        sisa = number_string.length % 3,
        rupiah = number_string.substr(0, sisa),
        ribuan = number_string.substr(sisa).match(/\d{3}/g);

      if (ribuan) {
        let separator = sisa ? "." : "";
        rupiah += separator + ribuan.join(".");
      }

      return rupiah;
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

    getStatusProfit (item) {
        let qty = this.calculateQty(item);
        let modal = 13000;
        let hasilModal = qty * modal;
        let hasil = this.calculateTotal(item)

        if (hasil < hasilModal) {
            return false
        } else {
            return true
        }
    },

    calculateUntungRugiModal (item) {
        let qty = this.calculateQty(item);
        let modalKotor = 13000;
        let hasilModal = qty * modalKotor;
        let hasil = this.calculateTotal(item);


        if (hasil < hasilModal) {
            // return false
            return hasilModal - hasil
        } else {
            return hasil - hasilModal
        }

        
    }
  },
};
</script>

<style>
</style>