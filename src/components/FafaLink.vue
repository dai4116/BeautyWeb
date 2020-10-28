<template>
  <div>
    <h5>{{ msg }}</h5>
    <ul>
      <li><router-link to="/index" target="_blank">Beauty</router-link><br></li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    msg: String
  }
}
</script>

<style scoped lang="scss">
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
</style>
