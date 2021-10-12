<template>
  <div class="hello">
    <Button color="#3DB400" intensity="0.2" background-color="#F2F2F2">
      <span>버튼테스트</span>
    </Button>
    <Base class="asdf"/>
  </div>
</template>

<script>

import Button from "@/components/Button";

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  components: {Button}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.asdf {
  width: 120px;
  height: 120px;
}

h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

button {
  height: 48px;
  width: 120px;
  color: #000 !important;
  border-radius: 10px;
}
</style>
