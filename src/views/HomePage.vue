<template>
  <main class = "homepage">
    <div class="banner">
      <banner-vue class="banner-vue"></banner-vue>
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
    </div>
    
    <GeneralSummary class="summary"></GeneralSummary>
  </main>
</template>

<script>
  import GeneralSummary from "./GeneralSummary.vue";
  import BannerVue from "./ui/BannerVue.vue";

  export default {
    name: "HomePage",
    components: { GeneralSummary, BannerVue },
    data() {
      return {
        links: ["Produce", "Sales", "Feeds", "Expenses"],
      };
    },
  };
</script>

<style lang="scss" scoped>

.homepage{
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  overflow: hidden;
  max-width: calc(100vw - 250px);


  .banner{
    flex-basis: 100%;
    display: flex;
    flex-wrap: wrap;
    background-color: var(--primary700);
    overflow: visible;
    margin-bottom: 10vh;
    height: fit-content;

    .banner-vue{
      flex-basis: 100%;
    }

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
            padding-top: 1.5rem;
            margin: 0;
            text-transform: uppercase;
            color: var(--shadow-grey);

            & span {
            margin-top: 2vh;
            font-weight: 600;
            line-height: 8vh;
            font-size: 1.3rem;
            color: var(--accent300);

            }
          }

          .last-p{
            border: none;
            text-align: center;
            background-color: var(--shadow-grey);
            color: var(--black);

          }
          .key-value{
            color: var(--accent400);
          }
      }
  }
  
  .summary {
    z-index: 1;
  }
  
}
















  .header {
    width: 100vw;
    height: 40vh;
    padding-top: 1vw;
    padding-bottom: 0;
    background-color: var(--primary700);
    background-size: cover;
    background-position: center;
    margin: 0;
    display: flex;
    /* background-color: var(--black); */

    /* background: 50%-transparent; */
  }

  .nav {
    width: 100vw;
    height: 6vh;
    display: flex;
    margin-top: 1vh;
    justify-content: flex-end;
  }
  .nav ul {
    margin-right: 10vw;
    margin-top: 1.5vh;
  }
  .nav ul li {
    display: inline;
    margin-right: 0.2vw;
  }
  .router-link {
    text-decoration: none;
    color: var(--light-grey);
    font-size: 1rem;
    padding: 1vw 1vw;
    border-radius: 0vw;
    font-weight: 400;
    background-color: none;
  }
  .nav ul li :hover {
    color: var(--accent100);
    font-weight: bold;
    /* color: var(--white); */
  }
  /* .nav ul li :active {
	color: var(--white);
	background-color: var(--primary300);
} */
  .router-link-active,
  .router-link-active {
    /* background: var(--accent400); */
    /* color: var(--accent400); */
    color: var(--primary400);
  }

  .nav .sticky {
    position: fixed;
    background-color: rgba(255, 255, 255, 0.95);
  }


</style>
