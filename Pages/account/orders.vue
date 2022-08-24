<template>
  <div class="mt-8 flex flex-col mx-auto w-3/4">
    <div class="-my-2 -mx-4 overflow-x-auto sm:-mx-6 lg:-mx-8">
      <div class="inline-block min-w-full py-2 align-middle md:px-6 lg:px-8">
        <div class="overflow-hidden shadow ring-1 ring-black ring-opacity-5 md:rounded-lg">
          <table class="min-w-full divide-y divide-gray-300">
            <thead class="bg-gray-50">
              <tr>
                <th scope="col" class="py-3.5 pl-4 pr-3 text-left text-sm font-semibold text-gray-900 sm:pl-6">UUID</th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Total</th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Placed At</th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900"></th>
             </tr>
            </thead>
            <tbody class="divide-y divide-gray-200 bg-white">
              <tr v-for="(order, orderIdx) in orders" :key="orderIdx">
                <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">{{ order.uuid }}</td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ order.total }}</td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ order.placed_at }}</td>
                <ui-button as-link :to="order.invoice_url" size="xs" color="primary" class="mt-3 ml-2">
                  Download
                </ui-button>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import UiButton from '../morphnux/ui/Button'

export default {
  name: "orders",
  layout: 'account',
  middleware: ['auth', 'verified'],
  components: {
    UiButton,
  },
  async asyncData({$axios}){
    const orders = (await $axios.get('api/shop/orders')).data;
    return {
      orders,
    }
  }
}
</script>
