<template>
  <main>
    <!-- <div class="menu">
      <navbar-vue
        section="sales"
        :links="['new', 'today', 'Past week', 'Past month', 'Choose period']"
      ></navbar-vue>
    </div> -->

    <div class="finance">
      <p>
        Cash Balance<br>
        <span> KES 500,000 </span>
      </p>
      <p>
        Accounts Receivables<br>
        <span> KES 50,000</span>
      </p>
      <p>
        Accounts Payable<br>
        <span> KES 50/L </span>
      </p>
      <p class="last-p">
        Net Profit margin<br>
        <span> 20%</span>
      </p>
      
    </div>

    <sales-summary :period="this.days"></sales-summary>
  </main>
</template>

<script>
  // import NavbarVue from "../ui/Navbar.vue";
  import SalesSummary from "./SalesSummary.vue";
  import { TOMILLISECS } from "../../util/config.js";

  export default {
    components: { SalesSummary },
    props: { duration: Number },
    data() {
      return {
        startDate: null,
        endDate: new Date(new Date().setHours(0, 0, 0, 0)),
      };
    },
    computed: {
      days() {
        return this.duration;
      },
    },
    created() {
      this.setStartDate();
      this.$store.dispatch("setStartDate", this.startDate);
      this.$store.dispatch("setEndDate", this.endDate);
    },
    updated() {
      this.setStartDate();
      this.$store.dispatch("setStartDate", this.startDate);
      this.$store.dispatch("setEndDate", this.endDate);
    },
    methods: {
      //set Start date
      setStartDate() {
        const milliseconds = this.days * TOMILLISECS;
        this.startDate =
          new Date(new Date().setHours(0, 0, 0, 0)).getTime() - milliseconds;
      },
    },
  };
</script>

<style lang="scss" scoped>
main{
  flex-basis: 100%;
  display: flex;
  flex-wrap: wrap;

  .finance{
    width: fit-content;
    display: flex;
    flex-wrap: wrap;
    margin-bottom: 10vh;

      & p{
          margin: 0.1rem;
          text-align: left;
          padding: 1rem;
          width: fit-content;
          font-size: 1rem;
          border: 1px solid var(--black);    
          padding-top: 1.5rem;
          margin: 0;
          text-transform: uppercase;


          & span {
          margin-top: 2vh;
          font-weight: 600;
          line-height: 8vh;
          font-size: 1.3rem;
          

          }
        }

        .last-p{
          border: none;
          text-align: center;
          background-color: var(--shadow-grey);

        }
        .key-value{
          color: var(--accent400);
         }
  }
}
.menu{
  justify-content: center;
}
  .split {
    display: flex;
    height: 100%;
  }
</style>
