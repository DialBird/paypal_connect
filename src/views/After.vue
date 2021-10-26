<script lang="ts">
import { defineComponent, onMounted } from "vue";

export default defineComponent({
  name: "App",
  setup() {
    onMounted(() => {
      initPayPalButton();
    });
    const initPayPalButton = () => {
      window.paypal
        .Buttons({
          style: {
            shape: "rect",
            color: "gold",
            layout: "vertical",
            label: "paypal",
          },

          createOrder: function(data, actions) {
            return actions.order.create({
              purchase_units: [
                {
                  description: "テスト",
                  amount: {
                    currency_code: "JPY",
                    value: 1100,
                    breakdown: {
                      item_total: { currency_code: "JPY", value: 1000 },
                      shipping: { currency_code: "JPY", value: 0 },
                      tax_total: { currency_code: "JPY", value: 100 },
                    },
                  },
                },
              ],
            });
          },

          onApprove: function(data, actions) {
            return actions.order.capture().then(function(orderData) {
              // Full available details
              console.log(
                "Capture result",
                orderData,
                JSON.stringify(orderData, null, 2)
              );

              // Show a success message within this page, e.g.
              const element = document.getElementById(
                "paypal-button-container"
              );
              if (!element) return;
              element.innerHTML = "";
              element.innerHTML = "<h3>Thank you for your payment!</h3>";

              // Or go to another URL:  actions.redirect('thank_you.html');
            });
          },

          onError: function(err) {
            console.log(err);
          },
        })
        .render("#paypal-button-container");
    };
  },
});
</script>

<template>
  <div class="about">
    <h1>After</h1>
    <div id="smart-button-container">
      <div style="text-align: center;">
        <div id="paypal-button-container"></div>
      </div>
    </div>
  </div>
</template>
