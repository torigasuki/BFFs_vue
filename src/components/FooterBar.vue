<template>
  <div>
    <footer>
      <div class="wrap">
        <img src="@/assets/b.png" alt="BFFs">
      </div>
    </footer>
  </div>    
</template>

<script>
export default {

}
</script>

<style scoped>
#wrap {
    min-height: calc(100% - 4rem);
}

footer {
    background-color: #dddddd;
    height: 4rem;
    bottom: 0px;
}

footer > .wrap {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 70px;
}

footer > .wrap > img {
    width:42px;
    height: 42px;
}
</style>