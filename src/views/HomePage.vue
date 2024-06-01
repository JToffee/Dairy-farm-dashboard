<template>
  <main class = "homepage">
      <banner-vue class="banner"></banner-vue>
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
  justify-content: flex-start;
  flex-wrap: wrap;
  max-width: calc(100vw - 300px);
  overflow: hidden;

  .banner{
    flex-basis: 100%;
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
