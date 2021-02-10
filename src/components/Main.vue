<template>
  <div class="main-content">
    <div class="container has-text-centered">
      <h1 class="title">Cloudflare datacenter reachability</h1>
      <p class="subtitle">Test your website:</p>
      <form @submit.prevent="loadTestHostname(testHostname)">
        <div class="field has-addons has-addons-centered">
          <div class="control">
            <input class="input" type="text" placeholder="example.com" v-model="testHostname" />
          </div>
          <div class="control">
            <a class="button is-info">GO</a>
          </div>
        </div>
      </form>

      <div class="domain-item" v-if="testHostname" :key="testHostname">
        <domain-item :hostname="testHostname" />
      </div>
      <hr />
    </div>
    <hr class="top-hr" />
    <div class="columns has-text-centered" v-if="preloaded">
      <div class="column">
        <p class="title">FREE</p>
        <div class="domain-item" v-for="(site, index) in free" :key="index">
          <domain-item :hostname="site" />
          <hr />
        </div>
      </div>
      <div class="column list-domains">
        <p class="title">PRO</p>
        <div class="domain-item" v-for="(site, index) in pro" :key="index">
          <domain-item :hostname="site" />
          <hr />
        </div>
      </div>
      <div class="column list-domains">
        <p class="title">BUSINESS</p>
        <div class="domain-item" v-for="(site, index) in business" :key="index">
          <domain-item :hostname="site" />
          <hr />
        </div>
      </div>
      <div class="column list-domains">
        <p class="title">ENTERPRISE & ACD</p>
        <div class="domain-item" v-for="(site, index) in enterprise" :key="index">
          <domain-item :hostname="site" />
          <hr />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Vue, Component, Prop, Model } from "vue-property-decorator";
import Axios from "axios";
import DomainItem from "./DomainItem";
@Component({
  components: {
    DomainItem
  }
})
export default class Main extends Vue {
  @Model({ type: String }) testHostname;
  preloaded = false;
  iata = [];
  free = [
    "howfastiswoo.com",
    "howfastiswoocommerce.com",
    "perffirst.org",
    "servebolt.co.uk",
    "servebolt.co.za",
    "servebolt.in",
    "servebolt.io",
    "servebolt.lt",
    "servebolt.ru",
    "serveboly.com",
    "serverbolt.com",
    "autobudget.no",
  ];
  pro = [
    "advhi.no",
  ];
  business = [
    "sbte.st",
    "servebo.lt",
    "servebolt.co",
    "allotment.pro",
  ];
  enterprise = [
    "cloudflare.com",
    "accelerateddomains.com",
    "r99.no",
    "servebolt.com",
    "servebolt.nl",
    "servebolt.no",
    "servebolt.se",
    "audunhus.com",
  ];
  mounted() {
    this.preloadAirports().then(() => {
      this.preloaded = true;
    });
    // load query string hostname
    if (location.hash) {
      this.testHostname = location.hash.replace("#", "");
      this.loadTestHostname(this.testHostname);
      this.$forceUpdate();
    }
  }
  loadTestHostname() {
    this.$forceUpdate();
  }
  getAirport(code) {
    return this.iata[code];
  }
  preloadAirports() {
    return Axios.get("/iata.json").then(response => {
      this.iata = response.data;
    });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.domain-item {
  margin-top: 2rem;
  margin-bottom: 2rem;
}
.top-hr {
  position: relative;
  bottom: -5rem;
}
.main-content {
  padding: 1.5rem;
  line-height: 1rem;
}
</style>
