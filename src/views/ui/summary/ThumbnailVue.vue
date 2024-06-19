<template>
  <div :class="`thumbnail ${className}`">
    <p class="title-thumbnail">{{ title }}</p>
    <p id="value" class="value-thumbnail">{{ value }} {{ unit }}</p>
    <p class="difference">{{ difference }}</p>
  </div>
</template>
<script>
  export default {
    name: "ThumbnailVue",
    props: ["title", "value", "unit", "difference", "valueClass", "colour"],
    data() {
      return {
        className: this.valueClass !== "" ? this.valueClass : "",
      };
    },
    mounted() {},
  };
</script>
<style lang="scss" scoped>
  .thumbnail {
    margin-top: 3vh;
    padding: 1vw;
    margin: 0;
    padding-right: 0;
    height: 30vh;
    color: var(--light-grey);
    width: fit-content;
    border-radius: 1vw;

    p {
    text-align: left;
    margin-left: 1vw;
}
  }

  .title-thumbnail {
    margin-bottom: 1vh;
    margin-top: 2vh;
    font-size: 0.88rem;
    text-align: center;
    border: none;
  
  }
  .value-thumbnail {
    width: fit-content;
    padding: 1vh 1vw;
    background-color: var(--shadow-grey);
    color: var(--black);
    font-size: 1rem;
  }

.thumbnail-active {

  .value-thumbnail {
    background-color: var(--accent1200);
    color: var(--black);
  }
}

.difference {
  text-indent: 1vw;
  margin-top: 0.3rem;

}

</style>
