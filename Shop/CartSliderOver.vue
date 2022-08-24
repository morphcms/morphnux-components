<template>
  <slide-over ref="slide">
    <template #title>
      <h2 class="text-xl font-bold">
        {{ $t('Cart') }}
      </h2>
    </template>

    <cart-list height="max-h-max"/>

  </slide-over>
</template>

<script>
import SlideOver from "@/ui/SlideOver";
import Cart from "@/components/Shop/Cart";
import CartList from "~/components/Shop/CartList";

export default {
  components: {CartList, SlideOver, Cart},

  methods: {
    open() {
      this.$refs['slide'].open();
    },
  },

}
</script>
