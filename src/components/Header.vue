<template>
  <b-nav tabs>
    <b-nav-item disabled>
      <b>Funcy Quiz App</b>
    </b-nav-item>
    <b-nav-item disabled>counter : {{numCorrect}}/{{numTotal}}</b-nav-item>
  </b-nav>
</template>

<script>
export default {
  props:[
    'numCorrect',
    'numTotal'
  ]
};
</script>

<style>
</style>