<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";

export default defineComponent({
  name: "App",
  setup() {
    const clientId = process.env["VUE_APP_PAYPAL_CLIENT_ID"];
    const clientSecret = process.env["VUE_APP_PAYPAL_CLIENT_SECRET"];
    // const accountId = process.env["VUE_APP_PAYPAL_ACCOUNT_ID"];
    // const link = `https://www.paypal.com/bizsignup/partner/entry?&partnerClientId=${clientId}&returnToPartnerUrl=https://example.com&product=ppcp&integrationType=FO&features=PAYMENT,REFUND`;
    const link = "asd";

    const getAccessToken = async () => {
      const res = await axios.post(
        `https://api-m.sandbox.paypal.com/v1/oauth2/token`,
        {
          grant_type: "client_credentials",
        },
        {
          auth: {
            username: clientId,
            password: clientSecret,
          },
          headers: {
            Accept: "application/json",
            "Accept-Language": "en_US",
            "Content-Type": "application/x-www-form-urlencoded",
            // Authorization: "Basic " + btoa(`${clientId}:${clientSecret}`),
          },
        }
      );
      console.log("fmfm", res);
      return "hogea";
      // const accessToken =
      //   "A21AAKlSxjgmyZCf9Frn8Gca23A4zpuxdW5H-ZEKKnxOfzZ65yexfBfHhh-Qv-9SniB4JOkcIFdXjMnp7unMFgIaiNwWW81nA";
      // const res: any = await axios.post(
      //   "https://api-m.sandbox.paypal.com/v2/customer/partner-referrals",
      //   {
      //     operations: [
      //       {
      //         operation: "API_INTEGRATION",
      //         api_integration_preference: {
      //           rest_api_integration: {
      //             integration_method: "PAYPAL",
      //             integration_type: "THIRD_PARTY",
      //             third_party_details: {
      //               features: ["PAYMENT", "REFUND"],
      //             },
      //           },
      //         },
      //       },
      //     ],
      //     products: ["EXPRESS_CHECKOUT"],
      //     legal_consents: [
      //       {
      //         type: "SHARE_DATA_CONSENT",
      //         granted: true,
      //       },
      //     ],
      //     partner_config_override: {
      //       return_url: "http://localhost:8080/after",
      //     },
      //   },
      //   {
      //     headers: {
      //       "Content-Type": "application/json",
      //       Authorization: `Bearer ${accessToken}`,
      //     },
      //   }
      // );
      // const actionLink = res.data.links[1].href;
      // window.location.href = actionLink;
    };

    return { clientId, getAccessToken, link };
  },
});
</script>

<template>
  <div id="nav">
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
    <button @click="getAccessToken">Link Paypal</button>
    <p>{{ clientId }}</p>
  </div>
  <router-view />
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
