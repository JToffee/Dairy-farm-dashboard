<template>
<sidebar-vue>

</sidebar-vue>
  <router-view class="container" :class="`${isExpanded ?'is-expanded-main' : ''}`"></router-view>
  <!-- <footer-vue></footer-vue> -->
</template>

<script>
import SidebarVue from './components/SidebarVue.vue';
// import FooterVue from './views/ui/FooterVue.vue';
  export default {
    name: "App",
    components: { SidebarVue},
    computed: {
      counter() {
        return this.$store.state.counter;
      },
      isExpanded() {
        return this.$store.state.isExpanded;
      }
    },
    methods: {
      increment() {
        this.$store.dispatch("increment");
      },
      decrement() {
        this.$store.dispatch("decrement");
      },
    },

  };
</script>
<style lang="scss">
  :root {
/* COLOURS*/

    //BACKGROUND
    --color-background: #e5f2f9;
    --background-grey: #e5f2f9;
    --background-grey: #f1f5f5;

    // TEAL-BLUE
    --primary200: #99b6ce;
    --primary300: #59d5e8;;
    --primary400: #2aa1b3;
    --primary500: #117382;
    --primary600: #06424b;
    --primary700: #043239;
    --primary800: #02191c;


    //ORANGE
    /* --accent200: #e5c7b1; */
    --accent200: #b6594c;
    --accent300: #db3f4c;
    --accent400: #e74a2ed3;
    --accent100: #f88570d3;


    //GREEN
    --accent1200: #9bd19b;
    --accent1300: #8bcd8b;


    //NEUTRAL
    --black: black;
    --white: #ffffff;
    --dark-grey: #464541;
    --mid-grey: #8a8585;
    --light-grey: #f5f0f0;
    --lightSteelBlue: #b0c4de;
    --borderGrey: #d4d0d0;
    --shadow-grey: #c1c8ce;

/* DIMENSIONS */
    --sidebar-width: 300px
  }

  ::-webkit-scrollbar {
  display: none;
}
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'Fira sans', sans-serif;
  scroll-behavior: smooth;
}
  #app {
    background-color: var(--white);
    scroll-behavior: smooth;
    font-family: "Fira Sans", sans-serif;
    overflow-x: hidden;
    display: flex;

    .container {
      flex: 1 1 0;
      padding: 0;
      font-family: "Fira Sans", sans-serif;
      overflow-x: hidden;
      display: flex;
      flex-wrap: wrap;
      font-size: 1rem;     
      margin-left: 80px;
      position: relative

    }
    .is-expanded-main{
      width: 800px;
      overflow: hidden;
    }
		@media (max-width: 1024px) {
			padding-left: 6rem;
		
	}
}
body {
    background-color: var(--color-background);
    scroll-behavior: smooth;
    margin: 0;
}
.thumbnails-container {
  display: flex;
  flex-wrap: wrap;
  background-color: var(--primary600);
  border: 0.1px solid rgb(233, 229, 229);
}
  
  /* FORMS */
form {
  border: 1px solid var(--light-grey);

  h2 {
    margin-bottom: 8vh;
    margin-left: 30%;
    font-size: 3vw;
    flex-basis: 100%;
    color: #000;
  }
  h2 span {
    padding-bottom: 2vh;
    border-bottom: 5px solid var(--primary500);
  }
  .form-group {
    padding: 1vw;
    min-height: 5vh;
    flex-basis: 80%;
    margin: auto;
    font-size: 1.7rem;
  }
  .form-group label {
    color: #d8dde0;
    color: var(--dark-grey);
    padding: 1vw;
    float: left;
    margin-top: -1vh;
    width: 17vw;
    font-size: 1.3rem;
  }
  .form-group input {
    height: 5vh;
    width: 12vw;
    font-size: 1rem;
    text-indent: 1vw;
    color: #000;
    background-color: var(--white);
    outline: none;
    border: 1px solid var(--light-grey);
  }
  .form-group select {
    height: 6vh;
    width: 20vw;
    font-size: 1rem;
    text-indent: 1vw;
    color: #000;
    background-color: var(--white);
  }

  .form-group button:hover {
  background-color: var(--primary400);
}
}

  
button {
  width: 12vw;
  padding: 0.7vw;
  font-size: 1.5rem;
  margin-left: 20%;
  background-color: var(--accent400);
  color: var(--white);
  border: none;
  /* border: 1px solid var(--accent400); */
  border-radius: 1vw;
}
  

.custom-period {
  min-height: 80vh;
}
.menu {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  margin-top: 4vh;
}
  /* .dropdown {
  /* margin-right: -3vw; */
.navbar {
  flex-basis: 30%;
}
.dropdown {
  margin-top: 0;
  margin-right: -2.2vw;
}

.icon {
  width: 2vw;
  height: 2vw;
}
.move-to-top {
  position: absolute;
  top: 15vh;
  width: 70%;
}
.custom-display {
  position: absolute;
  top: 35vh;
  left: 10%;
}
.move-to-bottom {
  position: absolute;
  top: 130vh;
  left: 5vw;
}
.remove-footer {
  display: none;
}
</style>
